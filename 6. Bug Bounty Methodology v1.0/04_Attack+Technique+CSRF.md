# Attack Strategy
Though the idea of CSRF token is very solid, It's easy to mess up the implementation. We have several options to test for:

- Remove the CSRF token from requests
- Replace the CSRF token with a random value (for example 1)
- Replace the CSRF token with random token of the same restraints
- Leave CSRF Parameter empty
- Use a CSRF token that has been used before
- See if you can request a CSRF by executing the call manually and use that token for the request


# Automation
- We'll be using the match and replace functionality of burp.
- Depending on if the CSRF token is in the HEADER or the BODY section of the request, we'll need to pick one.
- Go to Proxy > Options, fill in the regex to indicate to burp how it can find the CSRF token in your request and replace it with a value of your own. 
- Specify the details of match/replace rule
- Click through the application and try to make changes. If you're able to make changes where a CSRF token is normally expected, investigate this further. It may be a vulnerability.

![[CSRF Match and replace.png]]


# Example CSRF Walkthrough
## CSRF vulnerability with no defenses
- With your browser proxying traffic through Burp Suite, log in to your account submit the "Change email" from, and find the resulting request in your proxy history. If you're using Burp Suite Professional, right-click on the request, and from the context menu select Engagement tools / Generate CSRF PoC.
- For example, if we come across a request being made to POST /email/change-email with no CSRF token in place, we can navigate to https://security.love/CSRF-PoC-Genorator
- Copy the email parameter from the request in burp and paste it in the PoC generator, make sure you make some changes so you can verify this CSRF attack later on. To grab the URI, we can righ click on our request in burp and click on "Copy URL" from the dropdown menu.
- Now, Download the PoC, it should download an HTML file. When you open this file, you should see a button. Make sure you are logged in another tab and click the button. This should change the useres email address. Verify this if you're doing bug bounties

## CSRF where token validation depends on request method
- We repeat the same steps as before until we change our email one time. We should now see another request to POST /email/change-email. Let's right click this request and send it to the repeater.
- If we change our provided CSRF token, we can see that our request is not accepted.
- However, we can also send our POST parameters as GET parameters in a GET request. Some servers also accept GET requests instead of POST request. Burp can easily transform our method by righ clicking the request and picking "change request method" from the context menu.
- This will give us a GET request.
- Which we can again try to enter in our CSRF PoC generator to prove impact.


