= ListTag 3.0 Decorators =

Adding a new decorator to the ListTag 3.0 framework is simply done through the creation of a class. Each decorator is an implementation of the `ListDecorator` interface, though often they will extend the `BaseListDecorator` utility class. This interface provides a number of hooks into different locations on the resulting table into which code can be injected, giving the developer flexibility over where the new functionality will reside.

Note that all decorators (as well as the aforementioned two classes) '''must be located in the `com.redhat.rhn.frontend.taglibs.list.decorators` package.'''

The body of these methods will typically use the `HtmlTag` class to generate one or more snippets of HTML to embed in the page. As covering every possibility is, well, impossible, it's easiest to just refer developers to the existing decorators as a model to follow. Some other utilities that may come in handy include:

 * The `LocalizationService` singleton (retrieved through getInstance()) can be used to lookup i18n values for the page.
 * `ListTagUtil` contains a number of utilities that are helpful for decorator developers.
