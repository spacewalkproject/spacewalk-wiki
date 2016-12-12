= !SearchServer - Developer Notes =
== Overview ==
The spacewalk webui performs searches through a XMLRPC interface to a separate java process that uses [http://lucene.apache.org Lucene] for the search engine.  We call this separate process the "!SearchServer".[[BR]]

A typical search involves
 * WebUI forming a query in [http://lucene.apache.org/core/2_9_4/queryparsersyntax.html LuceneQueryParser] syntax.
 * WebUI sends the query to !SearchServer through XMLRPC
 * !SearchServer looks to see what index should be searched: {Errata, Packages, System, Documentation}
 * Lucene searches index files, and a list of results are obtained.
  * Result consists of the object ID and the score from lucene (float 1.0 max value, 0.0 lowest) 
 * !SearchServer trims the returned list so only values which are above a threshold score (set in configuration file) are returned.
  * Additionally the max number of results returned is limited by the configuration setting of "search.max_hits_returned"
 * WebUI uses the returned ids to flesh out the info it wants to display
  * This step is also responsible for filtering the data to retain proper user/org permission rules.
 * Fleshed out objects displayed 

=== Basic Operations ===
==== Index ====
 * Upon start up !SearchServer reads the database table, "rhnIndexerWork" to determine what has been indexed previously. 
  * Each row corresponds to a different object_type, ex: {errata, packages, systems}.   
 * Once !SearchServer knows the last time the index operation ran, and the last id it indexed, it asks the database for objects which are new and/or modified since then.
 * These new objects are passed into Lucene so they may be indexed for later searching.
 * After startup, !SearchServer will poll the database for updated changes.  Typical polling period is 5 minutes, but this configurable.
==== Retrieval ====
When a request comes in through XMLRPC and matches the "index" namespace, lucene is used to search the indexes for a match.  
 * Format of message:  
  * Session ID  
  * Index name  - controls which index to search, example: {docs, errata, hwdevice, package, server}
   * This corresponds to the directory name for the index, typically stored at: /usr/share/rhn/search/indexes 
  * Query - this is what we are searching for, could be a package name, system name, phrase for doc search etc
 * In our code, the entry point for search functionality is defined here: "com.redhat.satellite.search.index.IndexManager::search"


=== XMLRPC ===
 * By default we listen on 127.0.0.1, port 2828
  * The address to listen on is configured by : "search.rpc_address"
  * The port number is configured by : "search.rpc_port"
 * Namespace: index  
 * Handler: com.redhat.satellite.search.rpc.handlers.!IndexHandler
  * Description:  This handles the basic searches through lucene indexes.  Most searches use this namespace.
 * Namespace: db  
 * Handler: com.redhat.satellite.search.rpc.handlers.!DatabaseHandler
  * Description:  This handles searches which ONLY look at the database, it will not search any lucene indexes.  It's mainly used for errata search by a date range.   
 * Namespace: admin  
 * Handler: com.redhat.satellite.search.rpc.handlers.!AdminHandler
  * Description:  This is the administration interface, currently is supports triggering a "reindex" for any index type we know about.  This is useful if you want the index to be updated immediately, 
   * Example: this is invoked by the python backend after a server is registered and from webui when a new custom errata is created. 

=== Lucene ===
 * Index files are stored at: /usr/share/rhn/search/indexes
  * Documentation "doc" indexes are delivered through a RPM "spacewalk-doc-indexes".  
  * All other indexes are generated based on the database !SearchServer is connected to.  
 * How can I see what is indexed
  * You can use [http://code.google.com/p/luke/ Luke] to open a lucene index in a GUI and see what is available
   * We include a luke jar in our git repo, this is not bundled for delivery, it is only for development
    * spacewalk/search-server/scripts/lukeall-0.8.1.jar
   * Example run:
    * java -jar spacewalk/search-server/scripts/lukeall-0.8.1.jar
    * Select index to open: "/usr/share/rhn/search/indexes/package"  
    * Click OK
    * Select the "Documents" view, then browse through the items.
     * Note:  Lucene does not store all values in a way we can view, only fields which were marked as "Stored" will be displayed.  
    * Searches will likely not work as desired from Luke.  Since we are using a custom NGramAnalyzer, we need to import this into Luke in order for our searches to function.

 * How to Clean
  * If you want to clean the existing indexes you can run a script: "/etc/init.d/rhn-search cleanindex"
   * REMEMBER  there are 2 parts to cleaning the indexes
    * Delete the directory containing the indexes
    * Adjust the database 'rhnIndexerWork' entry for the specified 'object_type'.  This is what will trigger the data to be re-read.
    * NEVER delete the doc indexes, they come pre-packaged in a RPM.  (spacewalk-doc-indexes)

=== Database ===
 * [http://ibatis.apache.org iBatis] is the ORM !SearchServer uses
 * Location of queries
  * spacewalk/search-server/src/config/com/redhat/satellite/search/db



== Howto Build/Run ==
=== Build ===
 * cd {SPACEWALK_GIT}/spacewalk/search-server
 * ant

=== Run === 
 * We include a run script "run.sh" which will run !SearchServer
 * You can interact with the !SearchServer through spacewalk/search-server/scripts/search.py
  * ./search.py --help
  * ./search.py --username admin --password spacewalk --package firefox
   * When is "--serverAddr" helpful
    * During development you may want to run a spacewalk instance and db on a remote machine while running search-server on your local box.
    * --serverAddr allows you to specify the spacewalk instance you want to log into for retrieving the session-id
     * NOTE:  The search-server must be running locally (to search.py), unless you open up the rpc binding to occur on the network interface and not 127.0.0.1

== Configuration ==
 * Configuration values from: /etc/rhn/rhn.conf are inherited by !SearchServer
 * !SearchServer specific configuration resides in git at: spacewalk/search-server/src/config/rhn-search.conf
  * Is installed on a target machine at: /etc/rhn/search/rhn-search.conf
 * Example
{{{
search.index_work_dir=/usr/share/rhn/search/indexes
search.rpc_handlers=index:com.redhat.satellite.search.rpc.handlers.IndexHandler,db:com.redhat.satellite.search.rpc.handlers.DatabaseHandler,admin:com.redhat.satellite.search.rpc.handlers.AdminHandler
search.max_hits_returned=500
search.connection.driver_class=oracle.jdbc.driver.OracleDriver
search.score_threshold=.10
search.system_score_threshold=.01
search.errata_score_threshold=.20
search.errata.advisory_score_threshold=.30
search.min_ngram = 1
search.max_ngram = 5
search.doc.limit_results = false
search.schedule.interval = 300000
search.log.explain.results = false
}}}
 * search.index_work_dir  : Specifies where Lucene indexes are kept
 * search.rpc_handlers  : semi-colon separated list of classes to act as handlers for XMLRPC calls.   
 * search.max_hits_returned : maximum number of results which will be returned to the caller
 * search.connection.driver_class : JDBC class
 * search.score_threshold : minimum score a result needs to be returned back to caller
 * search.system_score_threshold : minimum score a system search result needs to be returned back to caller
 * search.errata_score_threshold : minimum score an errata search result needs to be returned back to caller
 * search.errata.advisory_score_threshold : minimum score an errata advisory result needs to be returned back to caller
 * search.min_ngram : minimum length of n-gram characters (any change to this value requires 'clean-index' to be run, plus doc-indexes need to be modified and rebuilt)
 * search.max_ngram : maximum length of n-gram characters (any change to this value requires 'clean-index' to be run, plus doc-indexes need to be modified and rebuilt)
 * search.doc.limit_results : true means we limit the number of results both on search.score_threshold and restrict max hits to be below search.max_hits_returned, false means to return all doc search matches
 * search.schedule.interval : value is in milliseconds, controls the interval !SearchServer polls the database for changes, default is 5 minutes (300000)
 * search.log.explain.results : used during development/debugging.  If set to true, will log additional info hinting at what influenced the score of each result.

=== Documentation Search ===
 * Documentation search requires pre-packaged indexes to be installed, these indexes will not be re-generated if they are deleted.
  * rpm -q spacewalk-doc-indexes
  * The 'clean-index' script is intelligent enough to leave document indexes alone
 * To build updated doc indexes 
  * cd {SPACEWALK_GIT}/spacewalk/search-server/spacewalk-doc-indexes
  * run "./crawl_www.sh"
  * rpmbuild -ba ./spacewalk-doc-indexes.spec 
 * The build process involves running "nutch" to crawl the online documents and generated index files which are then packaged into the spacewalk-doc-indexes rpm

== Misc Notes ==
 * [http://en.wikipedia.org/wiki/N-gram NGram]
  * We are using a ngram analyzer.  This gives us a "fuzzy" like search behavior, it allows spelling errors to be forgiven and likely matches to be found.  
   * Drawback - people are often confused initially when they search on a specific term and see unrelated matches.  
   * Since results are presented with highest score first, users will be presented with best choices first, followed by potentially undesired results.
 * Security Limitations
  * !SearchServer does little limiting of results to user permissions, it is expected that the webui/api will handle user/org permission filtering.   Typically this is handled automatically in the webui/api code when we "flesh" out a DTO from the list of ids.  
== Further Questions ==
Send an email to jmatthews AT redhat DOT com


