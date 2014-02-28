---
layout: post
title:  "Deploying Jekyll on Azure with Github"
date:   2014-01-20 14:32:16
categories: website
header: /images/post-images/azure-header.png

---

**Up until now,** I've been piggy backing on the server BU provides its students which was more than enough for my use cases. But as I move onto something more advanced for this site, I've outgrown it (and it's space limitations). 

This is where Azure Websites comes in.

Jekyll's homepage provides a overview of supported deployment methods -- this post will expand upon it by deploying Jekyll on Azure. 

Azure's website building wizard is incredibly powerful and will get you up and running in a matter of minutes (it also includes some very cool options such as [Ghost](http://ghost.org)).  

I will assume you already have your jekyll blog running locally. If you don't, follow [Jekyll's getting started guide](http://jekyllrb.com/docs/quickstart/). 

###1) Move your blog to Github

By using Github, you'll be able to quickly push changes to your site. Chances are, if you're using Jekyll, you are probably already familiar with [Github](http://github.com). If not, start by creating your account and making a new repository for your site. If you've never done this before, check out [this help file](https://help.github.com/articles/create-a-repo) from GitHub. 

Next, you'll want to copy over all the contents blog's folder into your new repo. The most graphical and way to to do is with Github's desktop app: [Mac](http://mac.github.com), [Windows](http://mac.github.com). 

Once in the app, clone your repository to your computer and navigate to folder you picked. Drop in all the files from your blogs folder. (You can also have Github sync with your original blog's folder, but I like to keep them separate for ease.)

#### Important Deployment Settings

Your blog's folder may be different from mine but the way Jekyll works is that it will generate the website in a special folder. In my case it is `_site`. Your's may be the same or `site`,`website`, etc, depending on your configuration. 

![my blog's folder](/images/post-images/azure-2.png)

Azure's git deployment engine, [Kudu](https://github.com/projectkudu/kudu), will need to know where to look for your site. We'll need to create a `.deployment` file for this (otherwise it will attempt to generate the site from our root folder).

Create a new file called `.deployment` with the following: 

	[config]
	project = _site
	
Make sure edit the project property to your site's folder. 

Now sync all your changes with the Github app and you should see your files in the repository on Github. 


###2) Setup Azure

Azure's portal is very well designed and makes this step incredibly simple. 

#### Create a New Azure Site 

1) In the Azure portal, click the "+" to add an addition to your Azure account and choose a `custom create` website. 

![custom create a new site](/images/post-images/azure-3.png)

2) We're going to link our site to Github so check the `Publish from Source Control` option. 

![publish from source control](/images/post-images/azure-4.png)

3) Azure will ask you to give it permission to access your Github account. The default settings for the permissions are sufficient. 

4) Pick your blog's repository. If you've created a separate branch, specify it here.  

![pick your repo](/images/post-images/azure-5.png)

5. Your site should be up and running now! You can find your site's URL on the main settings page in the portal.

![visit your site](/images/post-images/azure-6.png)


#### Setting up your Domain

You can specify a custom domain name in the `Configure` tab in the Azure portal. Do note that you will need to upgrade to the Shared or Standard website mode. In most situations with a blog, the Shared mode is more than enough.

At this point you'll need to go to your domain's control panel to link it to your Azure site. The directions for this vary with providers to provider but here are some guides for some of the more popular domain services:

[GoDaddy](http://blogs.msdn.com/b/kaushal/archive/2013/07/06/windows-azure-web-sites-how-to-configure-a-custom-domain.aspx)

[Namecheap](http://devtxt.com/blog/azurewebsites-dns-namecheap)

[Network Solutions](http://stackoverflow.com/questions/20392619/azure-custom-domain-name-with-networksolutions-dns)

![setup domain](/images/post-images/azure-7.png) 

---

Your blog should now be fully functioning. Since Azure is monitoring your Github repository, any changes made will be automatically reflected on your Azure site. You can see a history of these updates in the `Deployments` tab in the Azure portal. 

#### Adding Posts

The Github app will be monitoring your blog's local folder. Any changes you make in the folder can be synced with the repository on Github with the app. 

What this means for you is that all you need to do is add your posts in the `posts` directory, open up the Github app, and sync. 

---

If you have any problems, make sure to check your `.deployment` file. 