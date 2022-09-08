#  Attack Strategy
- We'll need to fuzz every parameter we find.

#  Separators
- The following command separators work on both windows and unix-based systems:
	- &
	- &&
	- |
	- ||
- The following command separators work only on Unix-based systems:
	- ;
	- Newline (0x0a or \\n)
	- On unix-based systems, you can also use backticks or the dollar character to perform inline execution of an injected command within the original command:
		- \`injected command\`
		- $(injected command)

#  Commands
- Below are some commands that are useful on windows as well as unix:

| Purpose of command    | Linux       | Windows       |
| --------------------- | ----------- | ------------- |
| Name of current user  | whoami      | whoami        |
| Operating system      | uname -a    | ver           |
| Network configuration | ifconfig    | ipcofnig /all |
| Network connections   | netstat -an | netstat -an   |
| Running processes     | ps -ef      | tasklist      |

#  Fuzzing List
- Based on this, we can create a fuzzing list that contains all the separators together with all the possible commands. 
- Fuzz every single parameter you can find with this wordlist by using burp intruder.


#  Blind Command Injection
- We can test for blind command injection by launching a request that will execute a ping command to the loopback address.
- `& ping -c 10  127.0.0.1`
- Again, add this to your fuzzing list and mind the response time for this attack vector. If it's longer than 10 seconds, we probably have a blind command injection but be mightful as lag might occur and give a false positive.

