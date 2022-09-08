#  What is it?
- Server side template injection
- can occur because of a technique that developers use to solve the issue of dynamic page generations. Some websites take user input or calculated values and place them in a template that is ready to be served to the site visitor.
- SSIT can occur in plaintext and in code context

#  Attack Strategy
- For a full attack strategy, we want to detect SSTI, identify the templating engine being used and finaly exploit it.

##  Detect
- Attack vector:
	- ${7\*7}
- When the server doesn't return our original attack vector, but instead returns 49, we have likely detected either an SSTI or CSTI

##  Identify
- Step1: enter the attack string:- {{7\*7}}
	- if you get 49 back then move on to the next step
- Step2: enter the attack string:- {{7'\*7'}}
	- if you get 49 back, the templating engine is Twig, move on to the exploit step. If you get 7777777 back, the templating engine is Jinja2, move on the exploit. 
- Step3: enter the attack string:- A{\*comment\*}b
	- if it resolves, the templating engine is Smarty. 
- Step 4: enter the attack string:- ${"z".join{"ab"}}
	- if it resolves, the templating engine is Mako
- If none of the above attack strings work then SSTI is probably not present, move on to CSTI

##  Exploit
- This is Highly dependent on the templating engine that is being used.
- You can refer to the following article. [Server-Side-Template-Injection](https://portswigger.net/research/server-side-template-injection)

