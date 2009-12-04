== Multiple Organizations Guide ==
I've created this guide, initially to hold a single bit of advice about creating multiple organizations because I spent about half a day crying over my keyboard trying to work out why I couldn't create new users under the Org Admin account for a new organization I'd created and hopefully stop anyone else going through the pain...

=== Creating a new Organisation ===
This is fairly straightforward...

1) Login as your primary spacewalk administrator account (or any other account that you've granted spacewalk admin right to in fact) and navigate to Admin (tab) -> Organization (left menu) -> create new organization (button, right).

2) Enter the appropriate details I've adopted the following naming convention ${company} - ${department} (in the case "Gala Coral Group - Engineering"). I also create the main org account as a "departmental" level account which doesn't rely on PAM authentication even though we have this enable. I see this as an administrative account only and the delegate the same org admin privileges to PAM authenticated user accounts anyway.

3) Here's the non-obvious part, after creating the new organization you're taken to the subscriptions sub-tab. Now if this is your first organization all of your 20,000 odd entitlements will still be assigned to the default organization and hence it will tell you "Possible Values: 0 to 0". I took this to mean it was, by default unlimited. Oh how wrong I was...

4) Navigate to Admin (tab) -> Subscriptions (left menu) -> System Entitlements (let menu) -> Management (main pane) -> Organisations (main pane) and you'll see that you've probably got all of the entitlements assigned to the default organization, redistribute them so that your new organization has some as well. Repeat this process for each entitlement type as appropriate (Monitoring, Provisioning, Virtualization and Virtualization Platform).

=== Adding Users into Organisations ===
Potentially straightforward...

1) Login as the organization admin (probably your new one if you've been following from above) and navigate to Users (tab) -> create new user (button, right). If you don't have a "User" tab then it's because you haven't assigned any management entitlements to your new org, see above. This also goes for the Configuration (Provisioning entitlement) and Monitoring (Monitoring entitlement) tabs as well. See above for instructions on redistributing entitlements.

