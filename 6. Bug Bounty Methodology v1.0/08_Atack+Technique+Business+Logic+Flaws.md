# What is Business logic vulnerability
- Business process usually consists of :
	- Analysis > Development > Testing > Production

# Impact
- Client side calculations of prices in a clothing webshop - High/Crit
- This is core business for the target so any issues related to the core businesss will automatically be more impactful
	- When brute forcing usernames, you get a 200 OK status when the username you're trying to brute force exists and a 403 if it does not exist on the login page - Low
	- Negative amounts of items on a webshop lead to negative prices - High/Crit
	- If price = integer and amount = integer and total price = integer we can overflow total price when we price * amount - Critical
	- Registering with the same username as an existing user takes over the account - Critical
	- The user manual might tell you that you can't deactivate super admin user, but after trying it you can - Medium
	- Field in the response that's not in the original reequest but does get processed by the server when you add it - Nothing/Critical
	- Importing products with the same name as existing ones overwrites them. Even if the products do not belong to you and you should not be able to overwrite them - Medium.
	- 