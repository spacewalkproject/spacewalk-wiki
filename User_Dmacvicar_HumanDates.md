## Problem



Spacewalk shows timestamps in lot of places of the user interface. Those timestamps require the user to think about them.

![Alt](images/dates1.png?raw=True)
## Human dates



The patch allows to see the dates in a human format that is natural to the user. 

![Alt](images/dates2.png?raw=True)

You can still hover and see details. Even more, the generated tag is a semantic tag that has the timestamp in the attributes, so javascript can still access the original data.

![Alt](images/dates3.png?raw=True)

And of course, it is localized...

![Alt](images/dates-locale.png?raw=True)
## Implementation



It adds a rhn:formatDateHuman that is used
very similarly to fmt:formatDate.

The tag implementation uses http://ocpsoft.org/prettytime for the formatting.
## Alternative implementations



An alternative implementation is to do the change in the browser itself using http://momentjs.com.

In this case there is some extra work required in order to wire the locales right with the user preferences so that the right version of the .js is laoded and displayed. This
is much easier in the Java side.