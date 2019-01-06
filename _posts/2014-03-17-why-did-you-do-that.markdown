---
layout: post
title: "Why did you do that?"
date: 2014-03-17
published: true
categories: Work wtf
precis: In which I am confused - why are you doing that? And why *just* that?
featured:
  image: http://alltheragefaces.com/img/faces/large/troll-troll-face-l.png
  author: Troll Build Process
---
Weird behaviour encountered in a build today, and I want to write it done. Chances are I will see it again one day. No doubt far enough in the future that I will forget specifics, particularly any solution I might come up with.

I have a website that has been put together in an odd way that made sense at the time. We can't build a single package that can be used to deploy the site, it has to be built on top of a known working copy. That alone has made us at home to Mr Cockup a couple of times. So we are fixing it. Months after we should have but better late than never.

We're transitioning to a tool for deployments called [Octopus Deploy](http://octopusdeploy.com). It means I am busy making the new build scripts for all our work. It has mostly been straight forward, except for this one project, this previously kinda, sorta-unbuildable project. It's not as simple as the others, it makes a few steps which means we have to put a bit more detail into the build script - a nuget spec - to ensure it gets everything. One of those steps is getting a bunch of files that aren't in source control in there, ensuring they get picked up so we have a single package for the whole site. And here is where the weirdness begins. 

There are eight libraries that are exhibiting an odd behaviour; when they are included in a _lib_ folder, the build script copies them to the _bin_ folder. At first I though there must be a reference to the files somewhere else and some of the older build script fragments are picking them up, but after grepping I have flensed away everything bar the references including them in the Visual Studio project, treating them as just files, no copying to output dir. And there's not a command somewhere to copy the whole directory; it copies only eight files of many. Also, I can rename the _lib_ directory to something else entirely, and it still picks up those files. 

They are dependencies of direct dependencies but they aren't being pulled from a remote server because of dependency definitions. They are definitely being copied from the _lib_ folder and if they aren't in _lib_, they don't show up in _bin_. I wish I could believe msbuild is AI powered and is picking up the files because It Knows!

Now, this is only a problem because the nuget spec building the package is being told to package all files in the _bin_ and _lib_ directories. It's not a very sophisticated packaging format so when it encounters these duplicate files, it blows up. 

I've burned a few hours with my mind blown, to no avail. The only reference I can find on Google is, oddly enough, [the opposite problem](http://our.umbraco.org/forum/ourumb-dev-forum/bugs/16562-Assemblies-being-deleted-from-bin-folder), the files being deleted from the _bin_ folder, not mysteriously showing up. I've spent more time than I can really afford, and I expect when I get in tomorrow I am going to just go with it, make these files dependencies so they get jammed into _bin_ because I asked for it, and remove the duplicates. But it will nag at me, this issue, until I work it out, because really...what the fuck?

### Update 20/03/2014
Friend of the show and former partner in crime, So Su, reminds me that:

> You and I have seen something very similar to this issue before. It turned out to be Resharper adding references to dependencies of dependants.

That sounds familiar. I can't quite remember the circumstances he's referring to, but I can verify that at least one of the dependencies of dependants shows up buried deep in Resharper config. Hmmm.
