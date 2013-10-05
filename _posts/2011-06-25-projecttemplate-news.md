---
title: ProjectTemplate News
author: John Myles White
layout: post
permalink: /notebook/2011/06/25/projecttemplate-news/
categories:
  - Programming
  - Statistics
---

The news below was recently reported on the ProjectTemplate mailing list. For completeness, I'm also reporting it here.

* The first piece of ProjectTemplate news is that I won't be the exclusive maintainer for ProjectTemplate anymore. Allen Goodman, who works at BankSimple, is now my co-maintainer and he has full commit privileges. In the next few months, the emerging group with commit privileges is likely to grow beyond the two of us, but hopefully just having one more person in charge of ProjectTemplate's development will help to keep things moving forward.
* There's a [new draft of ProjectTemplate available on GitHub](https://github.com/johnmyleswhite/ProjectTemplate). v0.3-1 fixes problems with the YAML configuration system not working on Windows 64 machines by switching over to the DCF format that R naturally supports. Editing your configuration scripts should be trivial, but be prepared for ProjectTemplate to break on your existing v0.2-1 projects until you've updated them to use DCF instead of YAML.
* In addition to switching the configuration system over to DCF, ProjectTemplate v0.3-1 now uses namespaces and separate functions to implement all of the automatic data loading functions that were previously nested inside of `load.project()`. Hopefully this will make it easier for end users to override ProjectTemplate's defaults, while allowing ProjectTemplate releases to automatically rolls out bug fixes to less advanced users. On that note, the list of supported file formats for automatic data loading is growing and new patches on that front are always welcome.
* A minimal project format: Some people have asked for the option to create projects without some of the clutter that the standard project format creates, such as the diagnostics and profiling directories. There's now a minimal project format that you can use by invoking `create.project()` with the option `create.project(minimal = TRUE)`.
* Starting in two weeks, the version of ProjectTemplate available on CRAN will stay in pace with the version on GitHub. If you're still using v0.1-3, please consider upgrading or forking.
* There is now an official ProjectTemplate website at <http://projecttemplate.net/> that will hopefully be the start of a new era of better documentation for ProjectTemplate. While the material on the site is still in noticeably draft form, I expect the documentation to improve considerably in the near future. If anyone out there is a graphic designer and would like to make the new site look better, please let me know by e-mailing me at <jmw@johnmyleswhite.com>.

For now that's all, but there's more ProjectTemplate news coming soon. Stay tuned!
