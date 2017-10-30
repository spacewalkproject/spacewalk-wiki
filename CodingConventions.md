# Coding Conventions

In general we try to follow the defined coding conventions, but there are probably cases where we didn't.


[Java coding style](JavaCodingConventions)

[Python coding style](PythonCodingConventions)

## Schema conventions


If you change database schema (schema/spacewalk/), make sure you put correct upgrade script into **schema/spacewalk/upgrade/spacewalk-schema-\<version1\>-to-spacewalk-schema-\<version2\>/**. Use some digits at the start of the filename to force deterministic ordering of these upgrade scripts. Make the name descriptive a bit, put name of the database object you change there and maybe also some reason (column name, not-null, whatever). The suffix has to be *.sql*.

NOTE: Look at [schema/spacewalk/upgrade/spacewalk-schema-2.6-to-spacewalk-schema-2.7/](https://github.com/spacewalkproject/spacewalk/tree/master/schema/spacewalk/upgrade/spacewalk-schema-2.6-to-spacewalk-schema-2.7) if you need inspiration, or ask.

The schema upgrade scripts do not need to include a commit statement since the spacewalk-schema-upgrade script adds commits there automatically.