# How to use this document
1. Use this document for the main app methodology
2. First go through the intricacies of bug bounty notes.
3. Make note of what parameters are tested for what vulnerabilities
4. Explore the vulnerabilities in their own sections
5. Practice your methodology to develop an intuition
6. Re-read this document
7. Imagine all the ways in which we can create impact with these vulnerabilities

# Preparation
- Manually explore your target
	- At least a couple of hours
	- 8 Hours + is recommended
- Keep Burp suite open in the background
	- Scope set properly
	- Will later on be used to explore the requests
- Make a mindmap of the funcitonality
- Take note of the privilege levels
- Read **any manual** you can find - Public info is never out of scope, so google
- Register your accounts
	- While you do use XSS attack vector in every possible field
		- Triggers integration issues if existent
	- Also SSTI
	- Ex. \<img src=x onerror=alert()\>'">$\{\{7*7\}\}
		- This will test for HTML injection
		- This will test for JS context XSS
		- This will test for HTML tag attribute injection
		- This will test for SSTI
		- This will test for CSTI

# Exploring the Requests
- Burp: Filter on all requests with parameters
	- Study each request and parameter
	- Think of what it does
	- Think of how to break it functionally
	- Does our bug have security impact if we find any?
- Burp Settings:
	- Go to Target > Site map > click on the big square box at the top to apply a filter > click the square next to the "show parameterized requests" option.

# Parameter Analysis
- CSRF tokens should be checked 
- JWT tokens should be checked 
- Any URL that redirects people, even partial or in files (import) should be checked
- We have to test for BAC and IDOR if we see functions we should not be able to execute as certain user.
- Captcha's can be bypassed
- Files that are received from local system LFI or remote system RFI
- SQLi but in weaker form for every CRUD action
- XXE in SVG, XML, DOCX, ....
- Template injections via automated checks if you suspect templating engines
- URLs that resolve get checked for SSRF
- Every parameter gets checked for command injection
- Admin panels do not mean the end, try to bypass them
- Looking for hidden fields, both in the requests and in the response

# Parameters to test for
- If it will seem to interact with the logic of the web application (This is why it is so important to know the application properly)
- Any parameters that seem to contain a URL. If the target resolves the URL that we enter, it might be vulnerable to SSRF
- Any parameter that appears to grab files from the local file system such as images being stored on the HDD of the web server and being refered to as image=file.png. This might be vulnerable to LFI/RFI
- Any CSRF parameter
- Anywhere you can upload an image I will try XXE via svg
- Anywhere you can upload a file and it accepts DOCX or XLSX, try XXE
- Try to save your account settings, personal settings and any other settings you can find and look for hidden parameters.

# Issue Types
- CSRF
	- Check if the thoken is present on any form it should be
	- Server checks if the token length is correct
	- Server checks if parameter is there
	- Server accepts empty parameter
	- Server accepts responds without CSRF token
	- Toekn is not session bound
- JWT
	- None-signing algorithm is allowed
	- Secret is leaked somewhere
	- Server never checks secret
	- Secret is easily guessable or brute-forceable
- Open redirect bypass:
	- evil.com/expected.com
	- Javascript openRedirects
	- Hidden link open redirects
	- Using // to bypass
	- https:evil.com (browser might correct this, filter might not catch it)
	- /\ to bypass 
	- %00 to bypass (null byte)
	- @ to bypass
	- Parameter pollution (adding the same parameter twice)
- BAC (Broken Access Control):
	- Test higher priv functions should not be able to be executed by lower priv user
	- Test all user levels
	- Test with Authorise Tool
	- Test functions via developer console
	- Copy and paste of URL
- IDOR:
	- Test between ALL tenants (companies hosted on one server/database. Can also be divisions of companies)
		- Test with Authorise Tool
		- JS Functions via developer console
		- Copy and paste of URL
- Captcha Bypass:
	- Try change request method
	- Remore the captcha param from the request
	- leave param empty
	- Fill in a random value
- LFI:
	- Using // to bypass
	- /\\ to bypass
	- %00 to bypass (null byte)
	- @ to bypass
	- URL encoding
	- double encoding
- RFI:
	- Using // to bypass
	- /\\ to bypass
	- %00 to bypass (null byte)
	- @ to bypass
	- URL encoding
	- double encoding
- SQLi:
	- ' and " to trigger
	- If you get an error, run SQLmap
	- Test every Databaes read or write
	- Investigate DB systems and build wordlists
- XXE:
	- SVG files (images), DOCX/XLSX, SOAP, anything XML that renders
	- Blind SSRF, file exfiltration, command exec
- Template injections (CSTI/SSTI)
	- ${7*7}
	- If resolves, what templating engine
	- Try exploit by looking at manuals
		- URL encode special characters({}\*)
		- HTML entities
		- Double encodings
- SSRF:
	- SSRF against server itself
	- SSRF against other servers on the network
- Command Injection:
	- Test every single parameter
	- make a list of commands + command separators for target OS
- Admin panel bypass:
	- Try referer header
	- Easy username/password
	- Directory brute forcing for unprotected pages

# Finding more endpoints
- Javascript analysis
- wayback URLs
- Google dorking
- API documentation
- Use the presence of an API to determine the presence of another API. For ex: if a request is being made to **"/api/v2/getinvoices"**, we might be guess that there's a possiblity of a **v1** of that API, so we can manually try to navigate to that URL

# General Tips
- Get to know your application, spend some time on using it normally but keep burp open 
- Start with XSS and SSTI as early as possible if in scope, insert them into every field you see
- Create your own fuzzing lists
- Expand this list with your own findings
- VDP over PAID if you want less competition
- PoC or GTFO
- Prove your impact

