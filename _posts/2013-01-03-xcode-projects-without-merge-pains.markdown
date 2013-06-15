---
layout: post
title: "XCode projects without merge pains"
date: 2013-01-03 22:09
comments: true
categories: 
---

[XCode's](http://stackoverflow.com/questions/2004135/how-to-merge-conflicts-file-project-pbxproj-in-xcode-use-svn) 
[formats](http://stackoverflow.com/questions/4022362/merging-xcode-project-files)
[are](https://discussions.apple.com/thread/3081125?start=0&tstart=0) 
[shitty](http://stackoverflow.com/questions/10552082/finding-the-error-in-xcodes-project-pbxproj-after-merge).
They are pain to merge and are impossible to read. YAML is pretty.

Imagine a brave new world with XCode's `nib`s, model files, storyboards,
project files - all in YAML. Thats what [that project](https://github.com/darvin/xcode-yamlizer) do!

You can see [how](https://github.com/darvin/iHubot/blob/xcode-yamlizer/iHubot/iHubot.xcdatamodeld/iHubot.xcdatamodel/contents.yaml)
[pretty](https://github.com/darvin/iHubot/blob/xcode-yamlizer/iHubot/iHubot.xcdatamodeld/.xccurrentversion.yaml)
[it](https://github.com/darvin/iHubot/blob/xcode-yamlizer/iHubot/ViewController.xib.yaml)
[looks](https://github.com/darvin/iHubot/blob/xcode-yamlizer/iHubot.xcodeproj/project.pbxproj.yaml)
on Github in this sample [repo](https://github.com/darvin/iHubot/blob/xcode-yamlizer/).

<!-- more -->

## Installation

Install XcodeYamlizer with:

    $ gem install xcode-yamlizer

## Usage

### Git hooks

The best and recommended way is to install `pre-commit` and `post-merge` hook.
You can do that from your project's working directory:

    $ xcode-yamlize install

Then, before commit, `pre-commit` hook will:

  - find all obscure `.xib`s, `.xcdatamodel`s, project files, etc.
  - create appropriate YAML files with the same name + `.yaml` extension
  - add them to commit (if necessary)
  - add all obscure files to `.gitignore` (if necessary)
  - remove all obscure files from git (if necessary) (but will leave them be in file system)

After merge, `post-merge` hook will:

  - copy all obscure files to the same name + `~` postfix
  - overrite all obscure files from the version controlled `yaml`es.

### Standalone

```
$ xcode-yamlizer
options:
 -input (-i)  convert file (autodetects direction)
 -dir (-d)    convert directory (default direction - from XCode to YAML)
 -to_xcode    direction: from YAML to XCode format
 -verbose     verbose mode
 -help (-h)   show help
```

