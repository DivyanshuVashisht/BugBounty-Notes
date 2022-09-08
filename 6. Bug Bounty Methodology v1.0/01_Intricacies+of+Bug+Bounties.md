# Basic Tips
-> Bug Bounties is NOT pentesting
	-  Bug bounty targets have been tested before
	- Internally by the company
	- By pentesters before us
	- Probably by automatic tools

-> Spend time picking a good target
	- As much as you need
	- It can be the difference between finding a bug and finding frustration

-> Finding valid bug requires
	- Speed
	- Creativity
	- or both

-> We can either be
	- The first to find and scan a subdomain
	- The one to outsmart everyone
	- or both

-> Don't follow a methodology, it doesn't help
	- Build an intuition for bugs instead

-> Don't mourn for dupes
	- The are also valid bugs, be faster next time or think differently

-> Don't forget to take notes
	- We want to test our target over multiple days
	- We want to retest our target often
		- Agile release cycles are 2 weeks!! (Not all companies use agile)
	- We want to test complicated scenarios

# Picking a platform
-> 4 types of platform
	- Major platforms, well known
	- Regional platforms, less known
	- Privtate platforms, can only join by application/invitation
	- Self hosted bug bounty programs such as google or security.txt

-> Major platforms (Intigriti, bugcrowd, hackerone)
	- More competition
	- More programs to pick from
	- Intigriti also doesn't have negative karma

-> Regional platforms (Yeswehack, yogosha...)
	- Less competition
	- smaller selection of programs
	- Mostly regional languages, use translator plugins
	- Less hardened targets due to less competition

-> Private platforms (Synack)
	- You have to apply to join
	- Way less competition
	- Quality targets

-> Self hosted platforms (Google, firebounty.com, google dorking)
	- Quality is highly dependent on the program
	- Usually less hackers
	- Usually less hardened since most are VDP


# Picking a Target
-> We can notice several properties when we look at targets:
	- B2B vs B2C target
	- wide scope vs main app
	- web app vs mobile vs desktop vs ip range vs iot vs...
	- VDP vs PAID
	- Public vs private program
-> You should usually go for
	- B2B target such as invoicing application
	- main app
	- web app
		- Mobile is harder but also has less competition
	- PAID but I recommend starting with VDP
		- Get those invites for private programs before you get cash
	- Private program, but first you need to get invites
	- Programs where I can create or receive different users of different priviledge levels
-> Avoid high payouts, usually more hardened
-> Avoid news papers, usually very little functionality
-> Avoid banks, usually more hardened
-> Avoid mobile, learn API testing first via web and then expand to mobile
-> Avoid webshops unless you're willing to spend money
	- You need to test ALL the functionality including buying, returning...
-> Avoid programs that don't give you credentials and don't let you self register
-> DO TAKE THESE BACK UP LATER WHEN YOU KNOW MORE


# Getting Invites to Private Programs
-> Be active
-> Report valid bugs
-> Bug Bounty platforms will
	- keep stats on your reports
	- keep stats on your skills
	- Invite you if the see fit

-> participate in the CTFs hackerone and bugcrowd sometimes host
	- they give private invites of lower quality
