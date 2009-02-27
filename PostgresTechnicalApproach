[wiki:PostgreSqlProject Main Project Page]

=== Application Infrastructure ===

[wiki:PostgresAppInfrastructure Application Infrastructure]

=== Approach ===

''' DDL '''

Data Definition Language (DDL) will require separate versions for Oracle and Postgres, e.g.
CREATE TABLE, CREATE FUNCTION, CREATE INDEX.  These statements are centralized and change rarely so it is easy to maintain two versions.  

Tables can be converted using ora2pg, then tuned to use the proper native Postgres data types, e.g. INTEGER, BOOLEAN.

Functions will need to be categorized as easy and hard and then worked on by people with the appropriate skill level.

''' Non-DDL '''

3k queries and ~63 views must be studied and perhaps modified.

Queries and views (Non-DDL) must be managed differently.  Queries and views will fall into five categories (lower numbers are better):
        1. Work unchanged in both Oracle and Postgres
        2. Simple changes to the query eg: adding as keyword to specify alias name.
        3. Requires db changes, e.g. add compatibility functions from Orafce eg: decode(), NVL()
        4. Query rewrite which will work for both db's eg: ANSI joins
        5. Need to create a Postgres-specific version of the query eg: CURRENT_TIMESTAMP

[wiki:QueryTaggingExamples Tagging examples for queries]

This analysis is going to require major effort so it is best to handle it in stages:

        1.  Mark all queries and views via SQL comments?  '-- PGPORT_1:condition ?'.  This gives each query a unique identifier so it can be referenced in later stages
        2.  Make wholesale changes to minimize incompatibility, like sysdate
        3.  Change all comment question marks to one of the four values listed above
        4.  Create a GIT branch
        5.  Rewrite # 2 queries to # 1
        6.  Add db changes and change # 3 queries to # 1
        7.  Develop and implement a system to allow for Postgres-specific versions of # 5 queries


''' Installation '''

Install Postgres as part of the Spacewalk install, and convert Oracle data to Postgres using ora2pg; moving from Postgres to Oracle is problematic.


''' Testing '''

Use 'log_min_error_statement' and 'log_min_duration_statement' to find Postgres queries that are causing errors or are slow.  Single queries and performance unit tests should be of similar speed on Oracle and Postgres.  The standard regression tests should pass.

Existing Java JUnit tests, API tests, and QA built web UI tests should give us substantial coverage to determine if things are working. Additional manual testing for client communication will probably be required.
