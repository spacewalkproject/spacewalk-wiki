# Coding Conventions

In general we try to follow the defined coding conventions, but there are probably cases where we didn't.


[Java coding style](JavaCodingConventions)

[Python coding style](PythonCodingConventions)
## Schema conventions



If you change database schema ([schema/spacewalk/rhnsat](http://git.fedorahosted.org/git/spacewalk.git/?p=spacewalk.git;a=tree;f=schema/spacewalk/rhnsat)), make sure you put correct upgrade script into [schema/spacewalk/upgrade/spacewalk-<version1>-spacewalk-<version2>](http://git.fedorahosted.org/git/spacewalk.git/?p=spacewalk.git;a=tree;f=schema/spacewalk/upgrade). Use some digits at the start of the filename to force deterministic ordering of these upgrade scripts. Make the name descriptive a bit, put name of the database object you change there and maybe also some reason (column name, not-null, whatever). The suffix has to be *.sql*.

Look at [schema/spacewalk/upgrade/spacewalk-0.2-spacewalk-0.3](http://git.fedorahosted.org/git/spacewalk.git/?p=spacewalk.git;a=tree;f=schema/spacewalk/upgrade/spacewalk-0.2-spacewalk-0.3) or [schema/spacewalk/upgrade/satellite-5.1-spacewalk-0.2](http://git.fedorahosted.org/git/spacewalk.git/?p=spacewalk.git;a=tree;f=schema/spacewalk/upgrade/satellite-5.1-spacewalk-0.2) if you need inspiration, or ask.

The schema upgrade scripts do not need to include a commit statement since the spacewalk-schema-upgrade scripts adds commits there automatically.