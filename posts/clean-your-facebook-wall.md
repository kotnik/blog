<!--
.. description: Deleting your old posts, you know they are really outdated.
.. tags: facebook, selenium, python
.. date: 2014/05/08 21:26:50
.. title: Clean your Facebook wall
.. slug: clean-your-facebook-wall
-->

While being a great platform to keep up with family, friends and acquaintances, not to mention being spied upon, Facebook is very actual - the current moment is everything. Looking through previous post reveils past concerns and music you once liked. There's not much value in that.

I wanted to delete my old posts, but the job is very tedious. It is obvious that Facebook doesn't want to you do it. So, shortly, I wrote a script that does that for me using Selenium, you can get it from [this gist on Github](https://gist.github.com/kotnik/7738fccd6f8dbfa966fc).

I'll describe the easiest way to use it. First, create virtual environment:

<code>
mkvirtualenv -p /usr/bin/python2.7 fbcleaner<br>
workon fbcleaner<br>
pip install selenium
</code>

Download the gist, and simply run it with `python`. Then sit back and watch the history disappear. Or do something more interesting in the meantime.

_Note:_ if you have a lot of posts in old events, you will have issues since those can't be deleted. You can update script to catch that popup easily.
