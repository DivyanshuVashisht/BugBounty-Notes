# What is BAC?
![[BAC.png]]

- Privilege escalation
	- Vertical 
	- Horizontal
- Examples:
	- We have an admin and a normal user. We can test the admin settings with the low priv user
	- We have a normal user and a prospect user. The prospect user can not execute all the functions because he only has a trial account.
	- We have two users of the same authorization level: See IDOR


# Attack Strategy
## General Tips
- Make sure we have the right target
	- Need users with different access levels for vertical priv esc
	- Need multiple accounts for IDOR 
		- No static websites
- Create a mindmap of the target
	- Note down functionalities
	- Note down privilege levels
	- Indicate if privilege level can execute functionality
- Test BAC for all different privilege levels
- The CTO might have different BAC issues than an employee

## Manual
- Sometimes if user can not execute function, front end button is just hidden
	- Javascript function might still work
	- We can execute javascript function via the developer console
- We can just log as admin and copy & paste the URL of functions we should bno execute as low priv user
- We can execute request as admin and capture in burp, then send to repeater and paste in low priv user authorization header.

## Semi-automated
- We can use authorise - free burp extension
	- Log in as low priv user
	- Copy their cookie
	- Paste it in authorise
	- Log in as admin user
	- Activate Authorise
	- ![[BAC Authorise.png]]
- Auto repeater
- Match and replace
	- ![[BAC Match and replace.png]]





