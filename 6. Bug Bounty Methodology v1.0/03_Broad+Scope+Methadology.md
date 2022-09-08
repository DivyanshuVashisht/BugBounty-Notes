#  Bird's Eye view of Broad Scope Methodology

![[Bird's Eye view of Broad+Scope+Methodology.png]]

# Summary
1. Do subdomain enumeration on as much sources as possible
2. Check which subdomains are live
3. Run a vulnerability scanner
4. Write a new template for our scanner
5. Scan all the existing subdomains for new vulnerabilities
6. Scan new subdomains found for existing templates
7. Automate the whole thing
	1. VPS recommended for stability

# Strategy
## Subdomain Enumeration
- Whatis it?
	- Grab as much subdomains as we can
	- From as much sources as we can
		- Such as google dorking
		- Such as shodan
		- Such as crt.sh
		- ...
	- The more sources, the bigger of a chance to find a unique subdomain
- How to do it?
	- Run all the tools you can find:
		- https://github.com/projectdicovery/subfinder
		- https://dnsdumpster.com/
		- https://www.shodan.io/
		- https://github.com/fwaeytens/dnsenum/
		- https://githbum.com/tomnomnom/assetfinder
		- https://crt.sh/
		- amass
		- findomain
- What is our result?
	- A list of subdomains
	- That may or may not be up
	- That we can use for our next steps

## Manual Recon
- Google dorking:
	- `site:google.com -www`
- Waybackmachine
	- https://web.archive.org/
	- Summary tab will provide us with mimetype and URLs of the asset where we can filter by URL or by MIME type
	- The sitemap functionality shows all the URLs in a handy pie chart which allows us to explore the site in a birds-eye view.
- Show me your secrets:
	- https://securityonline.info/github-dorks/

## Identifying a target from list of subdomains
- Suppose you did subdomain enumeration (using either sublist3r, amass etc.) and found a massive list of subdomains.
- Our next step should be to find all subdomains that are having a web server. We can do that via the following steps:
	1. Copy the whole list and paste in VScode because there you can put https in front of all of them.
	2. Create a directory named EyeWitness and then create a file with your target's name
	3. Paste all the urls in this file.
	4. you can run eyewitness using docker: `docker container run --run -it -v $(pwd)/Eyewitness:/tmp/EyeWitness/ 1y4e/eyewitness-docker -f <target_file_name> --all-protocols`
	5. What this will do is give you a screenshot of all the different hosts that it visits
	6. This makes it easy to identify good targets.


## httprobe
- What is it?
	- Checking which of the subdomains from our list is live
- How do we do it?
	- https://github.com/tomnomnom/httprobe
- What is our result?
	- A list of subdomains that we know are live

## Vulnerabilit Scanning
- What is it?
	- We'll automatically fire requests
	- We'll then check the results
	- Very basic idea with huge potential
	- See video: How custom nuclei template can help us in recon
- How to do it?
	- Run nuclei vulnerabilit scanner on the list of existing subdomains
	- Verify our potential report
- What is our result?
	- A list of potential vulnerabilities we need to verify

## Custom Templates
- What is it?
	- Nuclei vulnerability scanner uses templates
	- These are simple yaml files
	- In it's most basic form
		- Define metadata
		- Define requests that need to be made
		- Define checks that need to happen
- How to do it?
	- Templating Guide - Nuclei - Community Powered Vulnerability Scanner (projectdiscovery.io) https://nuclei.projectdiscovery.io/templating-guide/


# Basic Attack Plan
![[Basic Attack Plan.png]]

# Where to go from here
- Create a cronjob that will pick up any list in a certain folder and run nuclei on it
- Write all your subdomain lists to that folder
- Add subdomain brute forcing to your process
- Write your own templates for nuclei
- Auto scan all your existing domains with new nuclei templates