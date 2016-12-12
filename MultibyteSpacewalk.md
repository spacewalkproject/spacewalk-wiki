= Spacewalk and Multibyte (UTF8) input =

Spacewalk has some limitations on accepting and processing multibyte character input.  

The following caveats apply:

 * Logins must be in ASCII
 * Multibyte passwords will be accepted for webui logins
 * Multibyte passwords may cause errors with rhn_register and rhnreg_ks.  Workaround: use activation keys when registering a system.
 * The Java webui pages (pages ending in .do) will accept multibyte input properly and will store it in the database correctly.
 * The Java webui pages (pages ending in .do) may have display issues showing multibyte user inputted data.