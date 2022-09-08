# Conditions for IDOR
- Following must be true
	- Identifier exists in request (ex. UserID=64)
	- Broken access control issues exists
- Example:
	- GET /invoice.php?id=12
	- POST /personalInfo.php?{personId:23, name:"tester"}
	- GET /invoices/1234.txt

# Attack Strategy
## Methods and Tools
- Automated Testing
	- Burp match and replace
	- Auto repeater plugin
	- Authorize plugin
- Manual Testing
	- Copy and paste URL
	- Execute Javascript in the developer console
	- Replacing authentication headers in burp repeater

## Deep Dive
- Webshop
	- Test ALL the procesess, including buyng an item, returning an item, subscribing to a service, ...
- Game
	- IDOR between users
- Newspaper
	- Subscription confirmations
- Bank
	- Test ALL the processes

## General Tips
- B2B application with same tenant policy
	- If you **can't** create sub-accounts
		- IDORs between users of same tenant
		- IDORs between tenant
	- If you **can** create sub-accounts
		- Also test for IDORs between sub accounts
- B2B application with non-same tenant policy
	- If you **can't** create sub-accounts
		- IDORs between users of same tenant
	- If you **can** create sub-accounts
		- Also test for IDORs between sub accounts
- ![[IDOR.png]]

# First vs Second order IDOR
- Ex-1 GET to /receipt?id=4566 redirects to /success if okay but to /erro if unauthorised for call
	- Make call to /receipt?id=4566 while not authorised
	- Navigate to /success instead of /error via burp intercept
	- Get shown receipt that is not yours
- Ex-2 GET /invoice?id=4566 might not be possible
	- Make POST call to /print/invoices with the id of the invoice
	- Get shown the PDF you should not see
- Ex-3 The user is not able to edit products that are not theirs
	- Import product with the same name as the one you want to overwrite
	- Overwrite the product info