
  jquery.xmlns.js:  xml-namespace selector support for jQuery

This plugin modifies the jQuery tag and attribute selectors to
support optional namespace specifiers as defined in CSS 3:

  $("elem")      - matches 'elem' nodes in the default namespace
  $("|elem")     - matches 'elem' nodes that don't have a namespace
  $("NS|elem")   - matches 'elem' nodes in declared namespace 'NS'
  $("*|elem")    - matches 'elem' nodes in any namespace
  $("NS|*")      - matches any nodes in declared namespace 'NS'

A similar synax is also supported for attribute selectors, but note
that the default namespace does *not* apply to attributes - a missing
or empty namespace selector selects only attributes with no namespace.

In a slight break from the W3C standards, and with a nod to ease of
implementation, the empty namespace URI is treated as equivalent to
an unspecified namespace.  Plenty of browsers seem to make the same
 assumption...

Namespace declarations live in the $.xmlns object, which is a simple
mapping from namespace ids to namespace URIs.  The default namespace
is determined by the value associated with the empty string.

  $.xmlns.D = "DAV:"
  $.xmlns.FOO = "http://www.foo.com/xmlns/foobar"
  $.xmlns[""] = "http://www.example.com/new/default/namespace/"

Unfortunately this is a global setting - I can't find a way to do
query-object-specific namespaces since the jQuery selector machinery
is stateless.  However, you can use the 'xmlns' function to push and
pop namespace delcarations with ease:

  $().xmlns({D:"DAV:"})     // pushes the DAV: namespace
  $().xmlns("DAV:")         // makes DAV: the default namespace
  $().xmlns(false)          // pops the namespace we just pushed
  $().xmlns(false)          // pops again, returning to defaults

To execute this as a kind of "transaction", pass a function as the
second argument.  It will be executed in the context of the current
jQuery object:

  $().xmlns("DAV:",function() {
    //  The default namespace is DAV: within this function,
    //  but it is reset to the previous value on exit.
    return this.find("response").each(...);
  }).find("div")

If you pass a string as the function, it will be executed against the
current jQuery object using find(); i.e. the following will find all
"href" elements in the "DAV:" namespace:

  $().xmlns("DAV:","href")


And finally, the legal stuff:

  Copyright (c) 2009, Ryan Kelly.
  TAG and ATTR functions derived from jQuery's selector.js.
  Dual licensed under the MIT and GPL licenses.
  http://docs.jquery.com/License

