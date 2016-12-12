## Problem



* Spacewalk asks user to pick dates using a collection of list boxes that is not a common pattern.
* The picker is implemented as an included fragment that shares state

![Alt](images/pickdates-current.png?raw=True)
## Date and time picker



We propose an implementation that first, fixes the usability.

* Allows to select with mouse but also quickly enter values using a keyboard
* Is connected to the i18n system

After evaluation various datepickers combined with timepickers, the one used at facebook.com feels very usable, allowing quick usage both with mouse as with keyboard.

----

![Alt](images/89af0308-626f-11e3-960a-fb6d6131affb.png?raw=True)

----

The result is a nice looking picker that looks like:

----

![Alt](images/datepicker1.png?raw=True)

----

Clicking on the date field would open a calendar. But you can still just enter the date.

----

![Alt](images/time-css.png?raw=True)

----

![Alt](images/datepicker3.png?raw=True)
## Implementation



* The picker is implemented as a JSP custom tag that glues some javascript and css together.
* The translation (javascript struct) is generated from the Spacewalk application settings and available translations
* The datepicker.jspf is still available (for backward compatibilit), it uses rhn:datepicker itself, so it is now just two lines.
* The tag generates the input fields of the previous picker as hidden inputs and populates them, so actions still just work out of the box

