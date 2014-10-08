---
layout: post
title: Starting to blog
tags:
- jekyll
- code
- lanyon
- blog
---

So, I am being encouraged to start blogging about the work I do. I
thought the second post (the first was very brief) would be about the
bits and pieces of technology I decided to use to make the blog.


I wanted, something light, that would integrate easily with the way I'm
currently doing things. I use Linux with the Pycharm IDE.  We use quite
a bit of python, but the IDE is used for other things such as XML/XSL
as well. Plenty of plugins are available, for example we can connect to
the JIRA server to look at tasks and issues.

I'm finding Pycharm a comfortable place to work with good version control
integration with git.

I started by googleing for 'blogging for
developers' which threw up a list of suggestions.
[This](http://www.quora.com/Whats-the-best-blogging-platform-for-programmers)
caught my eye. After reading the comments I went to
[pages.github.com](https://pages.github.com) and followed the instructions
and did some configuration.  Now, I have [Jekyll](http://jekyllrb.com/),
[Lanyon](https://github.com/poole/lanyon), their various dependencies
and nice looking blog.

So, the point is vast amounts of time and collaboration
have gone into all of this, mostly, open source software. Some of that
software is very complex. The end result is that I have something
wonderfully simple to work with.

But useful:

###Code blocks
{% highlight python %}
def print_log(s):
    caller = stack()[1][3]  # get the name of the calling function
    q = """
      insert into ldiv_log values (current_timestamp, '{}', '{}');
    """.format(caller, s)
    run_query(q)
{% endhighlight %}

###Lists

1. first
2. second
 * a
 * b

###Tables

| A         | B     |
| --------- | -----:|
| fsfdfsfd  |  £100 |
| hjhds     |   £19 |
| xcvd      |    £1 |

To make a post I just type, into a file, using my preferred IDE, some
simple [markdown](http://en.wikipedia.org/wiki/Markdown) text. Then
I do 'git add' of the new post, test locally then 'git push' up to
[jhkrug.github.io](http://jhkrug.github.io). Simple.

Just like this:

{% highlight text %}
###Lists

1. first
2. second
 * a
 * b

###Tables

| A         | B     |
| --------- | -----:|
| fsfdfsfd  |  £100 |
| hjhds     |   £19 |
| xcvd      |    £1 |

To make a post I just type, into a file, using my preferred IDE, some
simple [markdown](http://en.wikipedia.org/wiki/Markdown) text. Then
I do 'git add' of the new post, test locally then 'git push' up to
[jhkrug.github.io](http://jhkrug.github.io). Simple.
{% endhighlight %}

[This](http://joshualande.com/jekyll-github-pages-poole/), from Joshua Lande, was very
useful for integrating [Disqus](https://disqus.com/), Twitter and 
Google Analytics.

This feels like a nice way for a system administrator & would-be developer
to maintain a blog. Thanks to everyone who has made this possible.

{% include twitter_plug.html %}

