# Open Redirects
Ideally the user should never be able to influence the outcome of a redirection because it can add legitimacy to a phishing attempt to fool a victim. 

If the redirection happens based on URL parameter, we call it reflected open redirect. While they might seem harmless because they don't directly damage the website that hosts them, they can be very damaging to the reputation of a company.

You've to know that in bug bounties, there are targets that do not accept open redirects in the whole application or certain features. Read the Out of Scope section well! In pentesting, this is often reported as a low/medium issue.

