= Thrift RPC API Proposal =

[http://developers.facebook.com/thrift/ Thrift] is a library released by the Facebook team under their own open source license. (appears to be suitable for inclusion in Fedora but not yet verified) Thrift has been accepted into the Apache incubator and the proposal can be found [http://wiki.apache.org/incubator/ThriftProposal here]. 

It appears to be a relatively lightweight solution to the problem (as opposed to some of the other alternatives), and extremely fast. Doing some tinkering to see just how fast I wrote a simple do nothing service in Java, and then used a Python client that opened and closed 10,000 consecutive connections. (and ran this twice simultaneously to see how the server dealt) The result from both processes was approximately the same: '''2.80s user 2.39s system 54% cpu 9.499 total'''

It is however poorly documented so initial exploration is a bit hit or miss. In the "postgresql" branch on the git repository you can see some prototype code for this. Some points of interest:

|| spacewalk/thrift/spacewalk.thrift || Service definition used to auto-generate the service code in the various languages. ||
|| java/code/src/com/redhat/rhn/frontend/thrift/SpacewalkService.java || Auto-generated Java service code. ||
|| java/code/src/com/redhat/rhn/frontend/thrift/SpacewalkServiceHandler.java || Our actual implementation of the service. || 
|| spacewalk/java/code/src/com/redhat/rhn/webapp/RhnServletListener.java || Code which starts the RPC service when the webapp comes up. ||

Thrift seems like a relatively good fit for what we wish to accomplish. It appears to have a strong open source future and considering it's use at Facebook, should provide ample performance for what we wish to do.

Concerns or alternative suggestions?
 * ''Will this api layer only contain the stored procs/code equivalent?'' - JesusRodriguez
    * ''It would be great if we could move the entire Java Manager layer into this box, leaving the actions to call api.'' - JesusRodriguez
    * ''I don't see why it couldn't, not something I'd thought of but the RPC layer could easily come into use if we're adding any new code to Perl/Python, or even some kind of more service oriented code.'' - DevanGoodwin
 * ''I'd like to see a diagram of how the flow would work within the current application context. i.e. user hits web page that needs stored proc access or client sends XML-RPC request to python backend.'' - JesusRodriguez
