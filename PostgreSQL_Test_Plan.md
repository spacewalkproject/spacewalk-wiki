= PostgreSQL Test Plan =

=== Objective ===

The objective is to describe the Test plan of Space Walk - during the migration of the Database from the Oracle to Postgres as well as testing the application to perform against both databases for predefined performance benchmarks and all functionality.

=== The areas identified to Test ===

   1.  Functional Testing
   2.  Query/Stored Procedure Unit Tests
   3.  System Testing
   4.  Performance Test - queries
   5.  Data Migration - Oracle -> Postgres, Postgres -> Oracle
   6.  API Testing.
   7.  Upgrade Testing
   8.  Scale/Concurrency Testing (eg: large numbers of web requests at once)

=== Important Milestones during the Testing Process ===

   1.  Application Knowledge Transfer.
   2.  Environment Identification and Setup
   3.  Access to Scripts and Knowledge Transfer - the scripts referred here are the available selenium scripts.
   4.  Identify all the gaps in the existing scripts
   5.  Priorterize the order of the modules.
   6.  Establish a set of benchmarks for performance on Oracle.
   7.  Communication & Reporting plan
   8.  Plan to execute and Test modules - along with sign-offs
   9.  UAT.
   10.  Establish as set of scale benchmarks for performance on Oracle.

=== Communication Plan ===

  1.  The project wiki - will be the primary communication area - regarding the tasks, dates and tracking progress.
  2.  Conference calls - twice a week - to measure progress.
  3.  Daily and coverage during the overlap of Team - will be available on the irc channel as well as other modes identified.

=== Reporting & Tracking Issues ===

  1.  Identify the tool to report and Track issues to closure.
  2.   Identify the priority and severity.
  3.  The process for the life cycle of a issue - report - plan - resolve - test – close
  4.  Query/Stored Procedures

=== User Acceptance Criteria ===

  1.  Identify and document the user acceptance criteria for the application.
  2.  Includes the sign-off criteria.

=== Application Knowledge Transfer ===

1. Spend 1 day - Teaching the Test resources the spacewalk application functionality.

Initial estimate is one day for a walk through and then as we identify the priority of the modules - we will go deep into the functionality and evolve the knowledge -- as we build the test cases and core team members review them.

=== Environment Setup ===

  1.  Identify Environment Setup:
	 1.  Identified as a 4 machine setup.
	 2.  Get further details on hardware and software requirements.
  2.  Acquisition of the Environment.
  3.  Setup the environment - a 2 day task.
  4.  Setup access to RedHat to the Environment.

=== Identify Modules to Test and Create Functional Use/Test Cases ===

  1.  Identify all the different modules to Test.
  2.  Create the priority Order
  3. Create functional Use Cases and Test Cases around the Test Cases.
	1.  Who will do this? - Ideally the Testing Team to develop
	2.  Without a complete understanding of the application, how will EDB do this? -- This is a way to force knowledge of the application -- will track the quality of the cases by constant review from the core Team
	3.  Will these be documented using RH test case templates? - definitely we can use the existing Test Template
	4.  Are the test cases to be created for ALL functionality or just those functions not already documented by RH test cases? - we will build upon the existing base.
  4. Review -> update -> review -> sign off cycle for each module.
  5.  The Modules identified to Test are:
	 1.  Web-UI
	 2.  API
	 3.  Release Engineering
	 4.  E-mail regression
	 5.  Sanity
	 6.  Proxy 
	 7.  Monitoring
	 8.  Quick Search
	 9.  Advanced Search
	 10.  Channels
	 11.  Errata
	 12.  Auto Errata Updates
	 13.  Errata Search
	 14.  Configuration Management
	 15.  Pagination
	 16.  RHN Registration
	 17.  SDC Software
	 18.  Activation Key 
	 19.  Reactivation Key
	 20.  Multi Org - RHN
	 21.  Multi Org - II
	 22.  Authentication
	 23.  Virtualization
	 24.  Kickstart
	 25.  Solaris
	 26.  Users
	 27.  SSM
	 28.  Satellite Sync & Export
	
=== Automation ===

   1.  Get access to existing automated test scripts.
   2.  Setup and run the automated test scripts.
   3.  Knowledge Transfer on the existing Test scripts.
   4.  Start building on the existing Test Scripts.

We need to account for manual testing using client side parts of the application.  For example: Have 5000+ systems register or check-in and receive some number of updates (rpms).


=== Performance ===

  1.  Get a sample data set.
  2.  Identify use cases for performance testing.
  3.  Identify the parameters of performance testing.
  4.  Create a baseline benchmark with the existing application.
  5.  Run the same and identify and fix - on the migrated environment.
  6.  Identify test cases for scale and DB concurrency testing.
	
=== Migration ===

   1.  Assumption - Only migration from the latest release version to the with Oracle to Postgres and Postgres to Oracle.
   2.  Create a data set for migration.
   3.  Migrate to Postgres.
   4.  Run regression of the whole application. 
   5.  Migrate to Oracle from Postgres.
   6.  Run regression of the whole application.
   7.  Schema upgrade testing.

=== User Acceptance Testing ===

  1.  Run a complete regression of the application.
  2.  Perform performance and generate a score sheet.
  3.  Performance scale/concurrency testing and generate a score sheet.
  4. Review and get sign-offs. This is a 2 phase:
	1.  Module by module sign-offs
	2.  Entire application sign-off.
	

 