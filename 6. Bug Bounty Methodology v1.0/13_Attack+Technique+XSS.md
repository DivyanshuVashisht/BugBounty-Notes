#  XSS Contexts
- For stored and reflected XSS
	- XSS into JS
	- XSS in HTML tag attribute
	- XSS between HTML tags
	- XSS in JavaScript template literals
	- XSS in the context of the AngularJS sandbox
	- XSS in CSS
	- XSS in headers
- Into JS
	- const poem = "The Wide Ocean";
	- const author = prompt("Please enter your name", "Harry Potter");
	- const favepoem = 'My favorite poem is ${poem} by ${author}'
	- since we can control author
		- we can insert
		- we can breakout of function
		- we can insert our javascript
		- 'alert()/\*
			- ' to break out of JS function
			- alert() to pop an alert
			- /* to put the rest of the JS in comment so it doesn't break
- XSS in HTML tag attributes
	- \<script\> var name= prompt("please enter your name", "Harry Potter");\</script\>
	- \<input onload="this.value=name;" style="margin-top:3%"\>
	- We can
		- break out of the attribute if our input is not sanitized
		- insert our own arbitrary JS code
		- Execute XSS
		- \">\<script>confirm()\</script>
- XSS in HTML tag
	```
	<head>
		<title>Test</title>
	</head>

	<body>
		<h1>Hi there <span id="username"></span>!</h1>
		<script>
			let userName = prompt("What is your name?");
			document.getElementById('username').innerHTML = userName;
		</script>
	</body>
	```
	- We can 
		- insert our own HTML tags if developer did not sanitize properly
		- Make that a javascript executing tag
		- Execute arbitrary javascript code
		- Execute an XSS attack
	- Input = \<script>confirm(document.cookie)
	- Input = \<img src=x onerror=confirm()>
	- ...


#  Types of XSS
##  Reflected
- User input gets reflected 
	- into an attribute of an html tag
	- into the HTML page
	- into tht javascript context
- user input does not gets stored into database
- user input is not properly sanitized
- user input can contain javascript code

##  Stored
- user input gets stored into database
- database value gets reflected
	- into html page
	- into html tag attribute
	- into the javascript context
- user input is not sanitized properly
	- at input
	- and at write to the database
	- and at read from the database
- user input can contain malicious javascript code


##  DOM
- var search = document.getElementbyId('search').value;
- var results = document.getElementById('results');
- results.innerHTML = 'You searched for:' + search;
	- results.innerHTML <<< DOM sink
- DOM sinks
	- where user input enters document object model
		- eval()
		- innerHTML
- Most common sources of DOM XSS
	- arises from windows.location
	- usually in query string(?)
	- **OR** fragment protion of URL(#)


#  How to test for XSS - General
- Attack vectors
	- Depend on the context
		- JS: "'\`...
			- Single quote, double quote, backtick... to break out of JS function
		- HTML: \<img src=x>
			- First test for HTML injection, then expand to XSS
		- HTML attribute: '>">\`>
			- Single quote, double quote, backtick along with '>' ... to break out of html tag attribute
	- All are simple, if they break the page or insert image, look deeper
	- replace the \<img src> tag with your own tag like \<script>
- Passive method
	- As soon as you register, enter attack vector into every input field you see
	- Hope XSS pops somewhere down the line


#  Getting around filters
- many different filters out there
- Backlist based
	- Try fuzzing all possible HTML tags and javascript handlers
	- Try URL encoding xss attack vector
	- try double or triple encoding it
	- try putting blacklisted word into other blacklisted word as filtered might only happen one time leaving our original blacklisted word intact

#  How to bypass WAF
- Whitelist based
	- Very hard to get around
	- only allow certain words
	- block the rest
- Pattern based
	- tries to look for things that look like a XSS attack
	- Depends on the configuration



#  Raising our impact
- Steal cookies
	- Very hard recently
	- HTTPonly flags is gaining traction
- Execute a keylogger
	- Getting harder to smuggle out data
- Steal data from the page
	- Same as keylogger
- Execute JS function on page


#  JS FILES
- provide
	- new endpoints
	- hidden parameters
	- API keys
	- business logic
	- HTML/Javascript sinks
	- Secrets/passwords
	- Potentially dangerous aread in code(eval, dangerously SetInnerHTML etc)
- use burp suite pro to gather js files
	- Explore the application with burp open
	- right click the target in site map
	- Engagement tools > Find scripts
- use waybackmachine
	- Install waybackurls
		- github.com/tomnomnom/waybackurls
	- Grep for JS files
		- `waybackurls google.com | grep "\.js" | uniq | sort`


#  XSS Cheat Sheet
![[XSS CHEAT SHEET 1.png]]

![[XSS CHEAT SHEET 2.png]]

![[XSS CHEAT SHEET 3.png]]

![[XSS CHEAT SHEET 4.png]]