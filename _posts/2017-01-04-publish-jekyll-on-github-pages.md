---
layout: post
title: Publishing a Jekyll Blog on GitHub Pages with custom (HTTPS) domain
excerpt: Follow these step-by-step instructions on publishing a Jekyll blog on GitHub Pages (featuring free SSL support using Cloudflare) to get a blog up and running in no time!
---
<div class="message">
  Follow these step-by-step instructions on publishing a Jekyll blog on GitHub Pages (featuring free SSL
  support using Cloudflare) to get a blog up and running in no time! These are the documented steps
  that I took to create and host this very site.
</div>

## 1) Setup and create your Jekyll blog

Jekyll is a "simple, blog-aware, static site generator perfect for personal, project, or organization
sites". It's built using Ruby, so before we begin you'll need to to make sure you have it available
on your system (You can check with the command `ruby --version`). If you don't have ruby available
on your system, I recommend using a Ruby version manager such as [RVM](https://rvm.io/)
or [rbenv](https://github.com/rbenv/rbenv). Once you have that setup,

### Setup Bundler and Jekyll libraries

Run the following command to install Bundler:
{%highlight bash%}$ gem install bundler{% endhighlight %}

And this command to install Jekyll:
{%highlight bash%}$ gem install jekyll{% endhighlight %}

### Scaffold the Jekyll project
{%highlight bash%}
  $ jekyll new blogger && cd blogger
  $ jekyll build
  $ jekyll serve
{% endhighlight %}

Voila, the Jekyll blog should be up and running at <http://localhost:4000>.
Before we git going, let's save and commit our changes:

{%highlight bash%}
  $ git init
  $ git add -A
  $ git commit -m "Finish blog in 12 parsecs"
{% endhighlight %}

## 2) Push to GitHub
Now that we have a basic Jekyll blog ready, let's set up our GitHub repository:

### Set up GitHub repository
![](/public/images/2017-01-04/github_newrepo.jpg)
* Replace `YOUR_USERNAME` with your actual GitHub profile name.
* The choice of using a public vs. private repo is up to you! (Both will work).

Once you create the repo, take note of the remote url and add it to your local Jekyll project...
  and then just push!
{%highlight bash%}
  $ git remote add origin [GITHUB_REMOTE_URL]
  $ git push origin master
{% endhighlight %}

Check out your site at <https://YOUR_USERNAME.github.com>. Notice that, amazingly enough, GitHub
Pages supports HTTPS out of the box!

## 3) Use a custom domain name
Great, now your site is up and running on GitHub. But what if we want to use a custom domain name?
Assuming you have a domain name ready (I purchased `andrewyi.xyz` from name.com), you'll need to
login to your domain registar's site and add some DNS Records:

![](/public/images/2017-01-04/namedotcom_dns_github.jpg)

* Those two IP addresses that I'm using for the A records (`192.30.252.153` and `192.30.252.154`) were
    taken from the following GitHub article, under the "DNS configuration errors" section:
    <https://help.github.com/articles/troubleshooting-custom-domains>.
* You'll also need to add the CNAME record for your domain if you want the `www` subdomain to work.

Now all you need to do is add your custom domain name in the settings page of your repo.

![](/public/images/2017-01-04/github_settings.jpg)

* **NB**: I was experiencing an issue where pushes to remote would remove the custom domain. Using
    an alternative method for adding a custom domain name fixed the issue:
    [GitHub instructions](https://help.github.com/articles/troubleshooting-custom-domains/)
    (i.e. involves adding a `CNAME` file in the project directory, with the content being your
    custom domain).

And now, check out your custom domain using your favorite browser! Unfortunately, although GitHub
supports HTTPS for `github.io` domain names, it doesn't work with custom domains. *However*,
Cloudflare offers a FREE solution that will let us work around this problem.

## 4) Obtaining the shiny green padlock (SSL)
Setting up SSL for our custom domain is super easy using Cloudflare.
After you sign-up for a free Cloudflare account at <https://www.cloudflare.com/a/sign-up>
(I used the "Free Website" tier, which costs $0/month, without the need to enter a credit card
number), all you need to do is to submit a form with your custom domain:

![](/public/images/2017-01-04/cloudflare_signup.jpg)
* Just add the domain you used for GitHub here, and it will auto detect your DNS records.

![](/public/images/2017-01-04/cloudflare_scanning.jpg)
* Scanning...Scanning...Scanning...

![](/public/images/2017-01-04/cloudflare_auto_records.jpg)

Now all you need to do is to (1) remove the DNS records we added in the previous step
**AND** (2) edit your nameservers as instructed by Cloudflare:

![](/public/images/2017-01-04/namedotcom_dns_github.jpg)
* Remember these DNS Records we added? *Remove them!*

![](/public/images/2017-01-04/namedotcom_ns_cloudflare.jpg)
It may take up to 24 hours, but *eventually* you should be able to visit `https://YOUR_DOMAIN`.
Presto, green lock in the url bar!

### 5) Other improvements/tweaks to consider:
* [Cloudflare, Caching/CDN Setup](https://support.cloudflare.com/hc/en-us/articles/200168256-What-are-Cloudflare-s-caching-levels-)
* [Cloudflare, Force HTTPS redirection](https://support.cloudflare.com/hc/en-us/articles/200170536-How-do-I-redirect-all-visitors-to-HTTPS-SSL-)
* [Cloudflare, SSL options](https://support.cloudflare.com/hc/en-us/articles/203295200-What-are-my-options-for-enabling-SSL-support-at-Cloudflare-)

***
*[edit: 2017-07-08] Update instructions and screenshots*
