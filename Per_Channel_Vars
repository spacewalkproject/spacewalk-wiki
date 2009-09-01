Currently we have one satellite server managing servers across multiple sites, multiple networks, timezones, ldap ou's etc.. In our case this means we have a *lot* of similar files being managed on different places.

If we could configure "custom_info" type values per config channel which then could be overridden in order of ranking this would go a long way into solving that issue. This allows for fine grained control of the contents of the config files without maintaining a large number of similar files.

Example:

|| Variable || Value ||
|| SEARCHBASE || "uk.example.com nl.example.com be.example.com" ||
|| NAMSERVER1 || "10.0.0.20" ||
|| NAMSERVER2 || "10.0.0.21" ||
''Central config variables''

{{{
search {|rhn.config.var(SEARCHBASE)|}
nameserver {|rhn.config.var(NAMESERVER1)|}
nameserver {|rhn.config.var(NAMESERVER2)|}
}}}
''/etc/resolv.conf example''

-- jvzantvoort(jydawg) 