== Unit Tests ==

You can use ant on machine with installed spacewalk.

In order to run the unit tests, the following must first be done:

  1. Ensure that the spacewalk-java-tests package is installed.

To run a set of Unit tests within ant, simply run:
{{{
# ant -f /usr/share/rhn/unittest.xml -Dtestcase=ClassName
}}}
where !ClassName is the name of the class containing unit tests.  For example:
{{{
# ant -f /usr/share/rhn/unittest.xml -Dtestcase=CobblerCommandTest
}}}

Test logs are stored in ''/tmp/reports/''.