<!-- 
.. link: 
.. description: Seting up Let's Encrypt TLS certificate, a true story.
.. tags: ssl, letsencrypt
.. date: 2015/11/13 20:53:32
.. title: Using letsencrypt.org
.. slug: using-letsencryptorg
-->

As I
[announced before](https://blog.kotur.org/posts/lets-encrypt.html) I
will be moving to [Let's Encrypt](https://letsencrypt.org/) as the
only provider of TLS certificates for my websites. After spending some
time setting it up, I am asking myself why did it took us so long to
address this issue.

I'll describe here what it took to set up one certificate using Let's
Encrypt.

[Documentation](https://letsencrypt.readthedocs.org/en/latest/)
exists, but it needs a lot of love. Instructions are not immediately
clear, and you can see that they are trying to exploit current
catch-all technologies: the second paragraph in the `Installation`
section is *Using Docker*, while at this stage is not even clear how
`auth` option even works.

In short, all you need to do is to clone the repository:

```
git clone https://github.com/letsencrypt/letsencrypt
```

In newly created directory there is `letsencrypt-auto` script which
does everything. That is all there is to it: there is no installation.

Let's create our first TLS certificate. I chose my
[Pastebin](https://bin.kotur.org) as a guinea pig because it's the
site only I use so it can take some downtime. Let's Encrypt is
boasting how it can configure the website itself by fiddling your
configuration files, but Nginx support is still experimental, and I
actually prefer to run configuration I control. So, I will be using
manual mode.

First, lets initiate certificate creation:

```
cd letsencrypt
./letsencrypt-auto -a manual -d bin.kotur.org
```

The script will tell you to put a hash to be reached from a certain
URL in your website, and after you created that file, it will fetch it
and verify that you actualy do own the domain. Within seconds
everything will be over and our new certificate will be located in
`/etc/letsencrypt/live/bin.kotur.org`.

Certificates issued with Let's Ecrypt are valid only for 3
months. While that decision is debatable, it is super easy to automate
certificate renewal: you just run that command again, and restart your
webserver gracefuly.

Since I use Puppet to version configuration of my servers, supporting these certificates is super easy. For example, this is a part of generated configuration:

```
/etc/nginx/ssl.d# cat bin.kotur.org.conf 
# File managed with PUPPET.

include /etc/nginx/ssl.d/global.conf;
ssl_certificate     /etc/letsencrypt/live/bin.kotur.org/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/bin.kotur.org/privkey.pem;
```

At the end, here are the good and the bad.

**The good**

* It is easy.
* No more paying for certificates. No more racket.
* You can automate it (I once worked on Namecheap's API for SSL
  certificates - it is an episode of my life I want to forget).

**The bad**

* Let's Encrypt client is written in Python, but they are warning you
  not to install it with `pip` or with `python setup.py install`!?
  What the?!
* Making the validation link start with a dot
  (`/.well-known/acme-challenge`) is a stupid decision that can
  jeopardize security since people might misconfigure their webservers
  and leak dot-files and directories. Luckily Nginx is not hard to
  [configure](https://gist.github.com/kotnik/f0691e1e43c4d7d94284) to
  support this.
* Documentation needs a lot of work.
* Let's Encrypt client is very slow. Why would you make it re-init the
  virtual environment everytime, including fetching packages with pip?
  We know how to install standard Python package, and for the rest of
  folks `pip` is enough.
* Just look at the
  [issue queue](https://github.com/letsencrypt/letsencrypt/issues).

I really enjoyed playing with it, and I can't wait for them to finally go live or to grant me developer preview access I applied for.
