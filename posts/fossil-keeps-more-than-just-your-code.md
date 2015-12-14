<!-- 
.. link: 
.. description: Fossil, source code management tool, is one of the newer ones you should pay attention to.
.. tags: fossil, scm, git
.. date: 2015/12/14 23:06:04
.. title: Fossil keeps more than just your code
.. slug: fossil-keeps-more-than-just-your-code
-->

I know, you think that SCM war is over. The battle was fierce, features clashed - Git won. Everybody is using Git now, even the people who do not deal with the code: they keep their designs, texts, blogs in it. Yes, there are islands out there indeed. Hg is still among us, its users just love it. Hell, there are even some poor souls sticking to Subversion.

But...

I am asking you to try one more: [Fossil](http://fossil-scm.org) SCM.

Why? I will assume you use and know Git, so let me begin.

Git repository is made of a lot of files, blobs as you know them. Their names are SHA1s of their contents. The only SHA1 I know by heart is `da39a3`: a SHA1 of an empty string. On Fossil side, **repository is a single file**, and not just any file, but a relational SQLite database. One you can query, inspect, write scripts that are using it,... Comparing this with Git's plumbing is just not quite fair.

Git does not care about history, you can rewrite it as you feel like (`git rebase`). On the other hand, Fossil is **immutable**. Audit is possible, every action leaves a trail, and you can not rewrite it.

Git only takes care of your files. I like that, that sole decision enabled rich ecosystem of tools that take care of other needs developing requires: colaboration, synchronization and decision making among most important ones. But, what if you want to keep all that versioned in the same repository, for cross reference or immutability I mentioned previously? Fossil does not take care only of your files, but it tracks **tickets, wiki and technotes** (something like blog or announcement channel) too. Its all the part of the same timeline (or `git log`).

There are other aspects I like about it, but I leave you with these ones I described since I feel they are its strongest ones. Its concepts are [not hard to grasp](http://fossil-scm.org/index.html/doc/trunk/www/concepts.wiki) if you can spare an hour, but I'll give you my quick'n'dirty quickstart.

Here we go....

*Install it*. Use your package manager. On Arch:

```
pacman -S fossil
```

You can *clone* repos out there, but lets say you want to *start your own*:

```
fossil init glasshouse
```

You can create files, commit them, change, do all the stuff you expect from a standard SCM, but here's something other ones do not provide: you can *run built-in webserver* to control every aspect of your repository. I like to create `systemd` units for all my repositories, so lets do that.

Create file `$HOME/.config/systemd/user/fossil-glasshouse.service` with following contents:

```
[Unit]
Description=Fossil Glasshouse

[Service]
ExecStart=/usr/bin/fossil server %h/glasshouse/glasshouse.fossil --port 8055
Restart=always

[Install]
WantedBy=default.target
```

Start the unit and visit [http://localhost:8055](http://localhost:8055):

```
systemctl --user start fossil-glasshouse
open http://localhost:8055
```

After you click around the user interface you will get enough ideas, there's no need for me to give you mine. If you have, or track, a lot of Fossil repos you can use its `fossil htttp` command to have UI for all of them. Actually, [Arch's Fossil package](https://www.archlinux.org/packages/community/x86_64/fossil/) comes with Systemd units already ready for this setup. But I gave you the simple one too.

This is not all there can be written about Fossil, be sure to check [the website](http://fossil-scm.org), but more importantly take some time to try it out, you might like it too. Git is not the answer to all questions.
