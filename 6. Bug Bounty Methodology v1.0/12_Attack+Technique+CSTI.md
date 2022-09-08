#  What is it?
- Client side template injection
- occurs when developers use a client side templating engine such as vue or angular. 
- These templating engines allow us to push code to the client that contains placeholders. These placeholders will then be replaced in the clients browser with their respective values.

#  Impact
- CSTI vulnerabilities lead to XSS attacks which we can exploit. 
- However, if the templating engine is only used to display non-sensitive public data, there's nothing much we can steal with our attack of value. 
- If, however, there exists another application on the same domain that can access the session cookies, we might be able to steal those and raise our severity.

