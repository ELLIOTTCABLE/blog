Syntactically Aware Pagination
==============================

Another short and simple tutorial post (hopefully). I recently decided I
wanted colorful `cat`-ing, and ended up doing myself one better - colorful
(syntax-highlighted) paging through `less` / `more`. Here's a taste:

<object class="video" type="application/x-shockwave-flash" width="395" height="400" data="http://www.flickr.com/apps/video/stewart.swf?v=55430" classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000"> <param name="flashvars" value="intl_lang=en-us&amp;photo_secret=751191c159&amp;photo_id=2721656506"></param> <param name="movie" value="http://www.flickr.com/apps/video/stewart.swf?v=55430"></param> <param name="bgcolor" value="#000000"></param> <param name="allowFullScreen" value="true"></param><embed type="application/x-shockwave-flash" src="http://www.flickr.com/apps/video/stewart.swf?v=55430" bgcolor="#000000" allowfullscreen="true" flashvars="intl_lang=en-us&amp;photo_secret=751191c159&amp;photo_id=2721656506" height="400" width="395"></embed></object>

On a side note... god, do I wish Flickr wouldn't make screencasts look like
absolute shit by re-encoding them to the horrible .FLV format. C'mmon Flickr,
where's the h.264 love?

Anyway, as for how to achieve this - it's not difficult. You need the
[`highlght`](http://www.andre-simon.de/ "Highlight: code and syntax highlighting")
library, which you can get from your favourite package-management system - for
me, on a Apple Macintosh using [MacPorts](http://macports.org "MacPorts"), I
just have to run the following:
    
    sudo port install highlight
    
Once you've installed `highlight`, just add a file with the following content
to a file named `page` in your `$PATH` (you will, of course, need to adjust
the paths to whatever suits your package installation method - `which
highlight` may help you there):
    
    #!/usr/bin/env bash
    /opt/local/bin/highlight --ansi --force "$*" | /opt/local/bin/less -r
    
Finally, add something like the following to your `.profile`:
    
    export PAGER=page
    
All of this information can be found compacted into [the commit to my personal dotfiles repository with the changes mentioned here](http://github.com/elliottcable/dotfiles/commit/d239765a1867e0aa8d2062cfee8dfa788e83ed4e) - if you are interested in
what changes (if any) I've made to this concept since I posted this, check the
files mentioned in this changeset. Specifically, [this](http://github.com/elliottcable/dotfiles/tree/master/bin/page "bin/page at master from elliottcable's dotfiles") is my `page` script,
any changes I decide to make to it will appear there.