== How to modify Spacewalk's DB schema ==

If you are working with the Spacewalk code and need to make a change to the DB schema please remember todo the following:


1.  Craft yourself an upgrade script that will take the state of the current spacewalk release's schema to the next.  Examples of this can be found in:

{{{
  spacewalk/schema/spacewalk/upgrade
}}}

This script will alter the '''existing''' schema with your changes. In correct versioned subdirectory (you will probably want the latest one), pick the next free number and create upgrade scripts. If they are database-specific, you will need to provide both Oracle and PostgreSQL version of the schema upgrades script.

2.  Modify the original schema so fresh installs also contain your changes.  The DDL files that are ran during an initial install are found in:

{{{
  spacewalk/schema/spacewalk/common
  spacewalk/schema/spacewalk/oracle
  spacewalk/schema/spacewalk/postgres
}}}

In there you will see subdirs:  '''class data packages procs  synonyms tables triggers types views'''.  The '''tables''' subdir contains all the definitions for the tables, their indexes and constraits.  '''packages''' and '''procs''' dirs contain our stored procedures and functions.  If you are creating a new table that depends on other tables being created before it please view and edit spacewalk/schema/spacewalk/*/tables/tables.deps.

Use the common directory for things that are, well, common syntax, or things that are easily preprocessed -- for example, most of our tables are in the common directory even if they contain varchar2 because we just run substitution for PostgreSQL on the SQL sources.

3. Run

{{{
  perl schema-source-sanity-check.pl
}}}

in the spacewalk/schema/spacewalk directory to check that your changes are sane.

4. Run

{{{
  tito build --test --rpm
}}}

in the spacewalk/schema/spacewalk directory to check that your changes will be buildable.
