
# **DEPRECATED, NO LONGER USED**

# Local Ivy Repository

There are times when you need to use a jar right now but don't want to put it in the 

ivy repo or tmp jars.  So what do you do?  Create a local ivy repo for it.


     mkdir -p $HOME/.ivy/{PROJECTNAME}/local/
     cp name-version.jar $HOME/.ivy/{PROJECTNAME}/local/
     vi ivy.xml

put your jar in the above directory (name-version.jar) then update ivy.xml.
It will look at your local ivy repo first.

Enjoy.

[Ivy docs](http://www.jaya.free.fr/ivy/doc/tutorial/defaultconf.html)
