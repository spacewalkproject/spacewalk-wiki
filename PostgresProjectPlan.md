# ****DEPRECATED, NO LONGER USED****

[Main Project Page](PostgreSqlProject)
# Project Plan



This is s _straw man_ for the project plan and needs more work.
## Planning - phase 1



_<add detail here>_

[Development Plan"](_PostgreSQL)

[Tasks/Track"](_PostgreSQL)
### Knowledge Transfer



_<add detail here>_
#### Overview



_<add detail here>_
#### Datamodel Review



_<add detail here>_
### Analysis



_<add detail here>_
#### Sample (populated) Database (oracle & postgres)

       

_<add detail here>_
#### Schema



_<add detail here>_
#### Views



_<add detail here>_
#### Queries



_<add detail here>_
#### Stored Procedures



_<add detail here>_
### Identify Risks



_<add detail here>_
### Infrastructure



_<add detail here>_
#### Communication



_<add detail here>_
#### Source Control



_<add detail here>_
### Packaging Plan



_<add detail here>_
### Technical (migration) Approach



_<add detail here>_

[Technical Approach](PostgresTechnicalApproach)
### Assemble Team



  * Bruce Momjian mailto:bruce@momjian.us
  * George Williams mailto:george.williams@enterprisedb.com
  * Gurjeet Singh mailto:gurjeet.singh@enterprisedb.com
  * Vikram Rai mailto:vikram.singh@enterprisedb.com
  * Tushar Ahuja mailto:tushar.ahuja@enterprisedb.com
### Test Plan



_<add detail here>_

[Test Plan"](_PostgreSQL)
#### Functional Test Plan



_<add detail here>_
#### Query / Stored Procedure _( Level )_



_<add detail here>_
#### System _( Level )_



_<add detail here>_
#### Performance Test Plan



_<add detail here>_
### Test Environment



_<add detail here>_
### Identify Gaps In Automated Testing



_<add detail here>_
### Data Migration Plan



_<add detail here>_
#### Oracle -> Postgres



_<add detail here>_
#### Postgres -> Oracle



_<add detail here>_
### Schema Upgrade Plan



_How do we do this for multiple databases?_
## Implementation - phase 2

### Git Setup


 

Cleanup or drop and recreate the postgres branch.

_<add detail here>_
### Create PG Schema & Sample Database



_<add detail here>_
### Compatibility Libraries



_<add detail here>_
 * orafce
 * other?
#### Back porting



_<add detail here>_
#### Packaging



_<add detail here>_
### Development Packages



_<add detail here>_
 * spacewalk-schema.rpm
 * spacewalk-setup.rpm
 * ...
### Spacewalk Database Infrastructure



Modify Spacewalk infrastructure to support multiple databases.
 * Drivers
 * Installer
 * DB Wrappers 
   * Java Stack
   * Python Stack
   * Perl Stack
 * db-control scripts

_<add detail here>_
### Refine/Tune Postgres Schema



_<add detail here>_
### Tag Queries



_<add detail here>_
### Isolate Development Branch from Master



Stop pulling from master.

_<add detail here>_
### Migrate Queries



_<add detail here>_
### Migrate Stored Procedures



Create postgres function of Oracle stored procedures.

_<add detail here>_
## Testing - phase 3

### Functional Regression Test_(Automated Testing)_




_<add detail here>_
### Query/Stored Procedure Performance Testing



_<add detail here>_
### Client _driven_ Performance Testing



   _<add detail here>_
