WebKit (Safari 3.1) and the CSS @font-face declaration
======================================================

I'm a big fan of typography in general. If you check out [my homepage](http://elliottcable.name) or my [contact elliottcable](http://elliottcable.name/contact.xhtml) page, and you're using Safari/WebKit or Opera/Kestrel, you'll notice the typefaces (fonts, as colloquialized) are *very* non-standard. (As of this writing, I'm using [Museo][] and [Diavlo][][^jos] heavily on both.)

The internet has not be a friendly place for typohiles like myself, up to this point, at least. One might even say it was a frightful, mentally scarring environment for those akin to yours truly. We've been restricted to reading page after page after page on day after day after day for year after year after year abominations of markup and design enslaved by the horrible overlords we know as Lucida, Verdana, Arial, Helvetica, Geneva, Georgia, Courier, and... dare I invoke ye, thou my terrible overlord? Times New Roman.

Wherefore art thou, my glorious Archer? And thee as well, my beautiful Garamond? The technical restrictions of that horrible monster we know as the Web Browser hath forced us all too long to use those most banal, those most common, and those most abused, out of all of the typefaces of the world.

All hyperbole aside, I'm extremely happy to see the advent of a standard `@font-face` declaration in CSS. Internet Explorer first implemented a crutched, basic version of this way back in version 4, but nothing ever really came of it - their decision to create the proprietary[^msft] .EOT format to appease overly restrictive type foundries' worries about intellectual property (aka. the cold, hard dominatrix that we know only as Ms. Profit) truly and completely killed that initial attempt at bringing astute typography and it's advocates to the web. This new run at `@font-face` by an established, trusted, and open group (the [W3C][] itself, responsible for helping to make much of what we use as designers on the web standard and cross-system compatible) has a much better chance, in my humble opinion - and I am quite looking forward to the consequences if it succeeds.

Now, onwards to the topic of my post as declared in the header (yes, I know, a slow start - but it's an interesting topic with an interesting history!). WebKit, the open source rendering engine behind the wonderfulness that is Safari, and how it handles the 'new' `@font-face` declaration. No, it's not really 'new', but yes, it feels like it is.

To put it simply, and to be very blunt, it's broken.

The [CSS spec section][spec] for `@font-face` is very specific - typefaces are to be selected based on a wide array of criteria placed in the `@font-face` declaration block itself. Various textual CSS attributes may be defined within the `@font-face` declaration, and then they will be checked when the typeface is referred to later in the CSS. For instance, if I have two `@font-face` declarations for the Diavlo family - one for regular text, and one for a heavier weighted version of the typeface - then I later utilize Diavlo in a `font-family:` attribute, it should refer to the basic Diavlo font defined in the first `@font-face`. However, if I were to do the same, but also specify a heavy `font-weight:`, then it should use the heavier version of Diavlo. To place this example in code:

    @font-face {
      font-family: 'Diavlo';
      src: url(./Diavlo/Diavlo_Book.otf) format("opentype");
    }
    
    @font-face {
      font-family: 'Diavlo';
      font-weight: 900;
      src: url(./Diavlo/Diavlo_Black.otf) format("opentype");
    }
    
    h1, h2, h3, h4, h5, h6 {
      font-family: 'Diavlo';
      font-weight: 900;
    }
    
    div#content {
      font-family: 'Diavlo';
    }

As you can see, my headings should use the typeface defined in `Diavlo_Black.otf`, while my body content should use `Diavlo_Book.otf`. However, in WebKit, this doesn't work - it completely ignores any attribute except `font-family:` and `src:` in a `@font-face` declaration! Completely ignores them! Not only that - not *only* that - it disregards all but the last `@font-face` for a given `font-family:` attribute string!

The implication here is that, to make `@font-face` work as it is currently implemented in WebKit (and thus, Safari 3.1), I have to declare *completely imaginary, non-existent type families* to satisfy WebKit alone. Here's the method I have used in the places I current implement `@font-face`:

    @font-face {
      font-family: 'Diavlo Book';
      src: url(./Diavlo/Diavlo_Book.otf) format("opentype");
    }
    
    @font-face {
      font-family: 'Diavlo Black';
      src: url(./Diavlo/Diavlo_Black.otf) format("opentype");
    }
    
    h1, h2, h3, h4, h5, h6 {
      font-family: 'Diavlo Black';
    }
    
    div#content {
      font-family: 'Diavlo Book';
    }

Isn't it horrible? Seriously, my eyes, they bleed. There's lots of problems with this far beyond the lack of semanticity when it comes to the typeface names... let me see how many ways this breaks the purpose of `@font-face`:

 * You remove a large element our control over the display of the page.

   * As soon as we begin to use `@font-face` in our page, we can no longer make any use of any other textual control attribute - `font-weight:`, `font-style:`, and `font-variant:` are no longer available to us, because they no longer correctly map to technical typeface variant/features.

   * Also, many default elements are destroyed, unusable, without 'fixing' - for instance, `<b>` would have no effect in a page styled for WebKit as above; We would have to specify something like `b {font-family: 'Diavlo Black';}` - how broken is that? Unless we caught all such default elements and re-styled them to use the bastardized names instead of the correct attributes, lots of basic HTML formatting would be broken. I myself may never use in-document formatting (separation of design and content!), but what about comments forms? Forum posts? Direct HTML-literal quotes?

   * If we want to use Javascript to modify the display of the content, we can't simply adjust the mentioned textual control attributes - we have to know and change the entire `font-family:` array of strings.

 * You make us very wet. And by wet, I mean 'not DRY'. What if we decide to change one of the bastardized font names? Or use a different font entirely? We have to go through all of our CSS, all of our Javascript, and make sure we update every occurrence of the typeface's bastardized name.

 * You remove our user's user choice, and waste bandwidth. Since the names refer to families that don't, in fact, exist, the browser can't override the declaration with a user's installed version of the typeface. This means that, regardless of whether the user already has the typeface installed on their own computer, the browser won't use that - it doesn't know to use 'Diavlo', which the user has installed, because it was told to use 'Diavlo Black', which no user in the entire world has installed on their computer.

This whole thing is rather worrying - I've heard Opera has `@font-face` support, though I haven't had time to test this myself, so I don't know if it actually does - or, for that matter, if it does it 'correctly', or has the same problems as WebKit. But either way, WebKit is one of the first two implementations to ever attempt to support `@font-face` (Microsoft's unrelated `@font-face` declaration notwithstanding) - I really don't want to see it's early mistakes carried on to FireFox in a few years, and then Internet Explorer a few decades after that. That will leave us stuck with this broken system forever, as it has been demonstrated time and time again that if nobody else supports an old standard correctly, a newcomer to the standard will not do it correctly either. I for one would really, really, hate that.

In summary... come on, WebKit team, this isn't like you - you're always the ones with the closest-to-standard implementation, and the cleanest code, and... hell, overall? Webkit is the most secure/fastest browser available. But this is making me lose my faith in you, guys, please get it right. You're pioneering a leap into the future when it comes to the Web - this is as important, or _more_ important, than Mosiac's allowing of images was.

To put it succinctly - don't fuck this up, y'all.

[^jos]: These are fonts by [Jos Buivenga][jos], quite the amazing person. His (free) fonts are, uniquely, released for use on the web in `@font-face` declarations - unlike the vast majority of other (even free to download) typefaces, which have ridiculously restricting licenses and terms of use statements. Props, Jos - you're a pioneer, and deserve recognition as such.
[^msft]: To give Microsoft a little credit, something I rarely do... Yes, I'm aware Microsoft submitted EOT to the W3C as a proposal - the problem isn't with their attempts to make it non-proprietary, but with the basic concept of making typefaces on the web DRMed. Look what such attempts have done to the music and video industry - simply decimated it. Do we really want to see the same thing happen to our beloved medium as typography moves into the 21st century?

[Museo]: http://www.josbuivenga.demon.nl/museo.html (Jos Buivenga's Museo free typeface)
[Diavlo]: http://www.josbuivenga.demon.nl/diavlo.html (Jos Buivenga's free Diavlo typeface)
[jos]: http://www.josbuivenga.demon.nl/ (Jos Buivenga's free typefaces)
*[CSS]: Cascading Style Sheets
*[EOT]: Embedded OpenType
*[W3C]: World Wide Web Consortium
[W3C]: http://w3c.org (World Wide Web Consortium)
[spec]: http://www.w3.org/TR/REC-CSS2/fonts.html#font-selection (CSS2 specifications, Chapter 15 - Fonts, Section 3 - Font selection)
*[DRY]: Don't Repeat Yourself
*[DRM]: Digital Rights Management