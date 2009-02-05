= Overview =
Provide access to the search server's package search capability through API.

== Desired Search Criteria ==
 1. Search by name
 1. Search by version
 1. Search by release        '''NEEDS TO BE ADDED TO SEARCH SERVER'''
 1. Search by arch

=== Filter Results By ===
 1. Channel
 1. User
 1. Key ??   !ActivationKey?


== !PackageSearch WebUI Capability ==
 1. Name Only  
 1. Name and Description
 1. Name and Summary 
 1. Free form  - allows lucene query for search on anything packages index. 

=== Searchable Fields From !SearchServer ===
 1. name
 1. version
 1. filename
 1. description
 1. summary
 1. arch

Note: The class which does the work for indexing packages is: com.redhat.satellite.search.scheduler.tasks.!IndexPackagesTask
  To support searching by "release" we need to make a one line change to "indexPackage".
{{{
Map<String, String> attrs = new HashMap<String, String>();
attrs.put("name", pkg.getName());
attrs.put("version", pkg.getPrettyVersion());
attrs.put("filename", pkg.getFileName());
attrs.put("description", pkg.getDescription());
attrs.put("summary", pkg.getSummary());
attrs.put("arch", pkg.getArch());
}}} 
 Need to add attrs.put("release", pkg.getRelease())
{{{
Map<String, String> attrs = new HashMap<String, String>();
attrs.put("name", pkg.getName());
attrs.put("version", pkg.getPrettyVersion());
attrs.put("filename", pkg.getFileName());
attrs.put("description", pkg.getDescription());
attrs.put("summary", pkg.getSummary());
attrs.put("arch", pkg.getArch());
attrs.put("release", pkg.getRelease());
}}}
For background, the package model is populated from the DB query "listPackagesFromId" from spacewalk/search-server/src/config/com/redhat/satellite/search/db/package.xml
 

=== How to call a package search on !SearchServer ===
{{{
XmlRpcClient client = new XmlRpcClient(Config.get().getSearchServerUrl(), true);
List args = new ArrayList();
args.add(sessionId);
args.add("package");
String exampleQuery = "(name:(vim) description:(editor) arch:(i386) summary:(editor))"
args.add(exampleQuery);
List results = (List)client.invoke("index.search", args);
}}}

=== Example Lucene Query ===
 Query can be a string such as: (attributeName:(searchTerm) attributeName2:(searchTerm))[[BR]]
 You can get a little fancier and add modifiers such as "AND", "NOT"[[BR]]
 Lucene [http://lucene.apache.org/java/2_3_2/queryparsersyntax.html|Lucene Query Syntax]


==== Results Format ====
 1. String id  - package ID corresponding to the ID in database, rhnPackage.id
 1. String name - package Name corresponding to database, rhnPackageName.name
 1. float score - lucene score from query, higher number is a better result
 1. int rank - ranking, lower number is most relevant

 '''The data returned from the search server is not ready to be returned to the api caller.  You must first use the "id" and flesh out the package details from the DB.'''  



