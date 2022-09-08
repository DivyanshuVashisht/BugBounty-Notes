#  Basic SSRF Strategy
- Usually we'll abuse this vulnerablity by making the server execute a request to itself on a port which only accepts requests from the internal network.
- 
- Bassic types of SSRF attacks
	- SSRF against the server itself
	- SSRF against other backend systems
- If all else fails
	- Blind SSRF
- Where to also look 
	- Partial URLs in the body instead of a full URL
	- URLs with data files such as XML files or CSV files (import functionality)
	- The refere header can somtimes contain SSRF defects


#  SSRF against the server itself
- Where to start?
- Attack on loopback ip
- Example:

![[SSRF Example 1.png]]

# SSRF against other backend systems
- When can it occur?

![[SSRF Example 2.png]]


#  Blind SSRF
- How it works?
	- Simply replace the attack vectors you would use for normal SSRFs with the burp collaborator URL. When you see a HTTP request come in, you might have an SSRF vulnerability but it'll be hard to exploit since you might need to find an RCE for this.
- If you only receive a DNS request and no HTTP request, that means the HTTP requests are filtered when outgoing which makes it nearly impossible to exploit SSRF.


#  FAQ
- How can we bypass blacklist based SSRF filtering?
	- Using an alternative IP representation of 127.0.0.1, such as 2130706433, 017700000001.
- How can we bypass whitelist based SSRF filtering?
	- You can embed credentials in a URL before the hostname, using the @ character. For example: https://expected-host@evil-host.