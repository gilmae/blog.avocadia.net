---
layout: post
title: "Markdown in Fargo"
date: 2013-07-12 21:18
comments: false
categories: writing bug
precis: Trying to xplain to Dave Winer why the Markdown feature in Fargo is poorly named
featured:
  image: http://farm3.staticflickr.com/2626/4036839601_02fe9234d6_b_d.jpg
  author: Carl D. Patterson
  author_url: http://www.flickr.com/photos/carldpatterson/4036839601
---
<blockquote class="twitter-tweet"><p><a href="https://twitter.com/davewiner">@davewiner</a> had a good moan when feedly implemented OPML importing weirdly, but behold Save as Markdown in Fargo…saves as a p element in HTML</p>&mdash; gilmae (@gilmae) <a href="https://twitter.com/gilmae/statuses/355643035775807488">July 12, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I should have thought that the bare fact that a function labeled "Save as Markdown" generates an HTML file was self-explanatory as a bug report and yet...

<blockquote class="twitter-tweet"><p><a href="https://twitter.com/gilmae">@gilmae</a> -- could you explain the problem in a blog post or email. Thanks.</p>&mdash; Dave Winer ☮ (@davewiner) <a href="https://twitter.com/davewiner/statuses/355643658441199616">July 12, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Fair enough. Whatever else Twitter is good for, bug reports are not it<sup><a name="fni1" href="#fn1">1</a></sup>. So here goes.

Dave. HTML is most definitely not Markdown.

Now I understand that the functionality is **supposed** to be doing exactly as I have described, applying a Markdown parser to Markdown in the document to produce an HTML document. I can indeed read the Fargo docs. Still, that's not the common understanding of the sentence fragment "Save as Markdown". It might even be the opposite of said common understanding.

Furthermore, my Fargo doc is, so far, a three deep tree. So even if I did want HTML, I assure you, I did not want HTML that lost the tree structure. It's particularly galling since the HTML that I might want - if indeed it was HTML I wanted which I didn't - is already in the page, right there in the DOM. But rather than put **that** in the HTML file, Fargo applies a lossey conversion and dumps it out as a list. Not a list in the sense of a series of li elements, but a list in the sense of a series of space seperated words.

But yes, I do understand that the tree depths should be thought of as headers, and thus marked up as headers.

What I want is a Markdown version of what I have produced in Fargo. Which is to say, the OPML fragment

    <outline text="Getting Started">
            <outline text="Creating Your Character" created="Mon, 08 Jul 2013 11:53:03 GMT">
                <outline text="Generating Attributes" created="Mon, 08 Jul 2013 11:53:08 GMT"/>

would be output as

    * Getting Started
        * Creating Your Character
            * Generating Attributes

 OPML is not useful to me, it is not readily editable in a text editor when I don't have access to Fargo. Hence, my desire for a text version. Fargo is a tool for quickly restructuring the document by moving sections around, and a pretty good one. OPML, on the other hand, brings insufficient benefits to the table to offset the lock in.

 **UPDATE**

 If any of you were waiting with bated breath for a response from Winer, there has been none. Not on his blog, not via email, or via Twitter.



<hr />
<a name="fn1">1. </a>: Not there appears to be a formal way to report bugs.
