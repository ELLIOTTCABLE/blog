Markright
=========

So, [Markdown][] is pretty sexy. I write a lot of Markdown. I'm writing this
in Markdown. It's a really cool concept.

A small number of people I know prefer [Textile][]. They say it's better than
Markdown because it's a lot more powerful or flexible, allowing you to create
much more complex syntactical structures. Here's the thing: Markdown and
Textile really have entirely different goals. The goal of Textile is futile,
IMVHO. Specifically, Textile attempts to be a sort of replacement for HTML, a
language and syntax designed for editing large documents with complex
structures. There's only one problem with this premise: we already *have* a
language for marking up large, complex documents: it's called HTML.

Markdown's goal (or at least, the places where it ends up being strong) is
different. It's meant for "small things". Places where the entirety of HTML is
unnecessary. It's meant to be light, clean, and sensible.

Another huge benefit of Markdown is the fact that the language itself is also
quite easily parsable by the human eye at a glance. That is, most of Markdown's
elements of markup are simply the things that people have been using for ages
to mark-up plain text documents. Most of these derive from age-old standards
established on Usenet and in email discussions, or even on the original
BBSes that formed the original "internet".

Unfortunately, everything is not well in paradise. While Markdown is a huge
step in the right direction, there are still things that nag me, and I surmise
might [nag][t1] [others][t2] as well.

  [Markdown]: <http://daringfireball.net/projects/markdown/syntax> "Markdown, the premier markup syntax"
  [Textile]: <http://hobix.com/textile/> "Textile, the lame alternative to Markdown"
  *[IMVHO]: in my very humble opinion
  *[HTML]: HyperText Markup Language
  *[BBS]: Bulletin Board System
  [t1]: <http://twitter.com/leethal/status/1257331519> "August Lilleaas: @elliottcable Let's go get '_em_!"
  [t2]: <http://twitter.com/judofyr/status/1257306965> "Magnus Holm: @elliottcable I'm with you!"

General problems
----------------
First of all, there needs to be more universal support for the
[awesome extensions][PHP-Markdown-Extras] provided by [PHP Markdown][]. I'm
not too worried about the table support, that seems to be getting into a
Textile-ish realm of "HTML replacement", but the `<abbr>` support is
absolutely awesome (I'm even using it in this post!).

Second, and this is a small but important one, implementations need to be more
anal about being whitespace-agnostic. I get really annoyed when I have to
break my 78-character-line rule in a plaintext file for a Markdown link or
something.

  [PHP-Markdown-Extras]: <http://michelf.com/projects/php-markdown/extra/> "Markdown extensions implemented by PHP Markdown"
  [PHP Markdown]: <http://michelf.com/projects/php-markdown/> "PHP Markdown processing library"

Problems with inline elements
-----------------------------
Now, this is the main point of this post. The single most annoying thing about
Markdown is that it doesn't follow traditional wisdom regarding marking up
inline style elements.

Since the inception of the internet, the following have been rather standard:

- `*bold*`
- `_underline_`
- `/italic/`

Markdown breaks from this long-established standard, with the following rules:

- `*italic*`
- `_italic_`
- `/normal text/`

Wait, what? This makes no sense. At least, not to a general user. It took me a
while, but I finally puzzled out Gruber's thought process here: he's mentally
linked the HTML-specific rules regarding emphasis to Markdown's. To understand
this, one must first know a little bit of HTMLs history.

Originally, HTML had three elements in this vein: `<b>`, for bold text; `<i>`
for italic text; and `<u>` for underlined text. Simple enough. However, with
the advent of CSS, people decided that HTML itself, sans CSS, should be
exclusively utilized for structural and semantic markup, no associated style
data. Thus, with the most recent versions of HTML, these elements have been
deprecated in favour of their new replacements: `<em>` for emphasized text,
and `<strong>` for *more* emphasized text. (On a bit of a sidenote, `<u>`
received no replacement, for a multitude of reasons. Primarily among them,
underlining on the web has come to be seen as a hallmark of a hypertext link,
and it is somewhat bad form to superfluously underline an element that *isn't*
such a link.)

Gruber assumably associated these concepts with Markdown; so that `*this*` was
meant to correspond to `<em>` emphasis, and `**this**` corresponding to `<strong>`
emphasis. However, this doesn't work out too well in practice. People editing
text aren't thinking in terms of emphasis and more emphasis, they're thinking
about two different forms of emphasis. Usually, these two types correspond to
italics and bold text. That, however, is irrelevant - the point is that
although `<em>` and `<strong>` were originally designed to refer to two
different *levels* (or "strengths") of emphasis, they no longer do so. People
have psychologically combined the original concept of dual emphasis types
(italics and bold) with the new concepts of emphasis and strong, such that
`<em>` and `<strong>` are often interpreted in entirely different ways.

When I sit down to write some text, I make the assumption that I will be able
to alternatively attach more than one type of emphasis to elements of that
document. Italics and bolded text, or some variations thereupon. However, when
the accepted standards of `*bold*` and `_italic_` don't result in what I
expect, I get confused. Hence, I would like to see these re-assigned to more
sensible 'defaults'.

However, I wouldn't like to lose the functionality afforded by the concept of
"levels" of emphasis. Hence, I suggest we provide more emphasis by simply
"stacking" the elements. `**extremely important text**`, for instance, could
parse into `<strong><strong>extremely important text</strong></strong>`. This
makes it easy to style the elements in such a way as that more important text
of each "emphasis type" could be, well, *more emphasized*.

Since some people still utilize `/this sort/` of emphasis, I see no reason why
we shouldn't support it as well. But, since we're already there, why not
support `\this\` as well? Come to think of it, people using these two sorts of
emphasis are probably more insistent upon seeing actual italics, not just
whatever sort of emphasis the designer decided upon (I've seen plenty of
websites that choose to style `<em>` elements with, say, a light yellow
background, such as a hilighting pen might give). So why not convey that
information with the content? I don't want to use deprecated `<i>` elements,
so we can instead suggest that the `<em>` elements have classes corresponding
to the type of slant the user wanted: `<em class="italic">` and
`<em class="oblique">`!

As such, I'm proposing that we use an alternative form of markdown, that I'll
henceforth call "Markright" (if only to differentiate it from the original
Markdown). Markright utilizes the same syntax rules as Markdown, with the
following exceptions:

    _some text_         => <em>some text</em>
    __some text__       => <em><em>some text</em></em>
    ___some text___     => <em><em><em>some text</em></em></em>
    ...
    *some text*         => <strong>some text</strong>
    **some text**       => <strong><strong>some text</strong></strong>
    ...
    /some text/         => <em class="italic">some text</em>
    //some text//       => <em class="italic"><em class="italic">some text</em></em>
    ...
    \some text\         => <em class="oblique">some text</em>
    \\some text\\       => <em class="oblique"><em class="oblique">some text</em></em>

Typographic concerns
--------------------
Several Markdown parsers, as well as Gruber's "[SmartyPants][]" utility, will
automagically convert some ASCII nonsense into proper typographic elements.

Except ... they get them wrong.

Let's start with the easiest topic, ellipses. Best-practice rules that one
must provide proper spacing around an ellipsis, but not all people writing
documents in Markdown will know how to handle this. Thus, I suggest we eat all
spaces within and around a sequence of periods, and replace them with the
context-proper combination of Unicode ellipsis character and Unicode thin
spaces (see [Wikipedia's summary][wikipedia-ellipsis] of [Bringhurst][] on the
subject). Simply replacing a sequence of three ASCII periods with a Unicode
ellipsis is, frankly, not enough.

Secondly, dashes. The implementation of dashes is also abysmal. As with
ellipses, I don't suggest any syntactical changes here, only implementational.
Any space surrounding the syntax for em- or en-dashes should be "eaten up",
and replaced with singular Unicode hair space elements surrounding the Unicode
emdash/endash. While we're at it, let's add proper support for the swung dash,
eh?

Next, hyphens. It's not really visually a big deal, but something nobody seems
to care about is the proliferation of the hyphen-minus character. Don't know
what that is? It's the character in the middle of the word "hyphen-minus".
There's actually a proper Unicode character for a [hyphen][wikipedia-hyphen]
(that is, the dash that separates two parts of a word), I'd love to see any
single ASCII hyphen-minus characters with no space (only normal characters) on
either side be replaced by a true hyphen.

Finally, I don't like how most Markdown parsers handle "smart quotes". They
often get them flat out *wrong*. I've seen stupid things like `'it's'` being
turned into `‘it’s‘`, or nesting failing hard (such as
`"A 'quote' inside a quote!"` becoming `“A ‘quote’ inside a quote!“`). While
in many cases, the ordering of "smart quotes" can be intelligently deduced,
there are plenty of cases where it can't. Thus, my final addition would be a
syntax for specifically defining the order of smart quotes. Since many people
are used to writing arrays in code with square brackets, and since said square
brackets are readily available, why not make use of them? Have a quote mark
followed by an opening bracket "compile" into an opening quote mark, and have
a closing bracket followed by a closing bracket "compile" into a closing quote
mark.

To summarize:

    foo ... bar                     => foo … bar
    foo, ... bar                    => foo, … bar
    foo ..., bar                    => foo…, bar
    foo ... .                       => foo….
    foo ... ?                       => foo…?
    
    foo -- bar                      => foo – bar
    foo --- bar                     => foo — bar
    foo ~~ bar                      => foo ⁓ bar
    
    foo-bar                         => foo‐bar
    
    "[Elliott]'s not here.]"        => “Elliott’s not here.”
    "[Bob]'s the Macrons]' uncle!]" => “Bob’s the Macrons’ uncle!”

  [SmartyPants]: <http://daringfireball.net/projects/smartypants/> "John Gruber's SmartyPants"
  [wikipedia-ellipsis]: <http://en.wikipedia.org/wiki/Ellipsis#Typographical_rules> "The Typographical rules for using Ellipses on Wikipedia"
  [Bringhurst]: <http://en.wikipedia.org/wiki/The_Elements_of_Typographic_Style> "Robert Bringhurst's canonical 'The Elements of Typographic Style'"
  [wikipedia-hyphen]: <http://en.wikipedia.org/wiki/Hyphen> "Wikipedia's page on the Hyphen"