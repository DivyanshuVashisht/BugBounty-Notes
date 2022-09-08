#  Serialization
- processing complex structures such as objects into a much flatter format so that it can be sent and received in a sequential stream of bytes.
- This allows us to write complex data structures to memory, files or databases and also to send that data over the network to different APIs.
- For example:
```
$user->name = "carlos";
$user->isLoggedIn = true;
```

When serialized, the object may look something like this:

```
O:4:"User":2:{s:4:"name":s:6:"carlos";s:10:"isLoggedIn":b:1;}
```

This can be interpreted as follows:
- O:4:"User" - An object with the 4-character class name "User"
- 2 - the object has 2 attributes
- s:4:"name" - The key of the first attribute is the 4-character string "name"
- s:6:"carlos" - The value of the first attribute is the 6-character string "carlos"
- s:10:"isLoggedIn" - The key of the second attribute is the 10-character string "isLoggedIn"
- b:1 - The value of the second attribute is the boolean value true

- **Python refers to serialization as pickling**
- **Ruby refers to serialization as marshalling**

#  Deserialization
- When we deserialize, we use the byte stream that was created and turn it back into an object.


#  How does insecure deserialization occur?
- occurs due to object injection
- even before the deserialization is complete


#  How to identify it?
- Different languages all use a specific format and if you know what it looks like it can be easy to identify:
- PHP:
	- O:4:"User":2:{s:4:"name":s:6:"carlos";s:10:"isLoggedIn":b:1;}
	- PHP will serialize data in a mostly human readable format
- Ruby:
	- marshalled = Marshal.dump([1,2, 'string', Ojbect.new])
	- This dump method will create a serialized object. Our input contains numbers, a string and even an object. We can easily serialize those into one and then later deserialize it when we need it.
	- Resutl => "\\x04\\b\\ti\\x06i\\aI\\(//x04//b%5B//ti//x06i//aI//)"\\vstring\\x06:\\x06ETo:\\vObject\\x00"
	- As you can see, ruby indicates it's "marshalled" data with hexadeciamal notations (\x04)
	- What matters most if we encounter this is that we can create a very simple ruby script to deserialize this object for us.
	- Marshal.load("\\x04\\b\[\\ti\\x06i\\aI\\"\\vstring\\x06:\\x06ETo:\\vObject\\x00")
	- \# => [1,2, "string", \#<Object:0x00000002643000>]
	- We can then make any changes we need to make and re-encode it with the marshal dump method we saw earlier
	- marshalled = Marshal.dump([2, 2, 'string', Ojbect.new])


#  How to exploit it?
##  Functional Testing
- We try to find serialized objects somewhere that we can manipulate the value.
- Could be cookies or other locations such as hidden form fields. Another location could be in javascript code as it might need to pass on one of those values to the back-end.
- Programming languagse like PHP have something that's known as loose comparison operators. In PHP this is an example:
	- if("a"=\="b")
		- This =\= is calle dan operator and in this case two = signs means that PHP will check the values of the provided variables but not the types.
		- This means that the following condition would be true if the real password does not start with a number and we are able to manipulate the password to be 0.
		- `$login = unserialize($_COOKIE)`
		- `if ($login['password'] == $password)`

