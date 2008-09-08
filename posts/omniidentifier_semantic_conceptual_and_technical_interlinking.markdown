OmniIdentifier - Semantic Conceptual and Technical Interlinking
=================================================================

*Worth noting:* I'm using the word 'Interlinking' in the title of this post, because I want to differentiate between the concept of creating a conceptual link between two known objects/locations/concepts, and creating an actual hyperlink as defined by today's technical implementation (`<a href...>`, etc).

Some History
------------

I've had this idea bouncing around in my head for a while. It didn't have a name, really, or even any coherent implementation in my head. All I knew is that several things bothered me about links.

After talking about it with a friend of mine named [Mikael Høilund](io:person/Mikael+Høilund), I made concrete some of the implementation details, and came up with a codename for this 'psuedo-protocol' - "io". I'd wanted something short, so it was easy to type quickly by hand, and that fit the bill.

I proceeded to explain the concept to a few other acquaintances, and they and I started to trade such links back and forth in general conversation, and found they served their purpose(s) well.

A few days ago, feeling rather bored (and not feeling like doing my homework d-;), I decided to do something (other than said homework) that I'd been putting off for a while - write a RubyCocoa app. Cocoa's been "out there" for me for a long while - the Objective-C syntax simply revolts me (though the advent of a more Ruby-like "dot syntax" with Objective-C 2.0 has placated my hatred a tad), and RubyCocoa still utilizes the horrible method-naming practices evident in Cocoa, which doesn't improve matters all that much.

I couldn't really think of something I could do in Cocoa that I couldn't do in plain Ruby at first... until I realized that OS X only allows "actual" applications (that is, Cocoa applications) to register as handlers for URI schemes and file extensions. This seemed like the perfect opportunity to kill two birds with one stone, and write a simple proof of concept app to handle these "io" URLs.

However, to actually commit this to code, I needed to solidify the concepts behind this idea, I needed some concrete goals for the project. Last and probably least (heh), I needed an official name. Thus, this post.

What and Why
------------

So, that was a bit long-winded... sorry. Let's get that last bit out of the way first - I simply experienced a typo during the initial stages of development, and after looking at "oi" for a moment, I realized it could have a relevant bacronym - "OmniIdentifier".

This name demonstrates, in a nutshell, the differences between oi and URLs - though the technical implementation of oi is a subset of the technical implementation of URIs, they hold a totally different purpose. URLs are *Uniform* Resource *Locators*, indicating that a URI is supposed to refer to the *location* of a resource, and a URI is supposed to be *uniform* - that is, it holds the same meaning (points to the same location) for all those using it. oi (the spelling "oi" is both the singular and the plural), however, are not intended to be locators, nor are they intended to be uniform - they are *Omni* (meaning everywhere, everything) *Identifiers* (meaning they associate a contextually semantic answer to "what is this").

To summarize, when you know the location of something, and want to share this location with somebody, a URL is more appropriate - when you know what something is, and want to show somebody what it is (or show them how to find out), an OI is more appropriate.