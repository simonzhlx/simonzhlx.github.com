---
layout: post
title: ERROR: missing Change-Id in commit message footer
permalink: /Missing-ChangeId-In-Commit-Message-Footer/
---

Today, my first day of contributing code to another new project.  
  
I failed to push commits to Gerrit with the error 'ERROR: missing Change-Id in commit message footer'.   
After googled, I found [my answer](https://gerrit-review.googlesource.com/Documentation/error-missing-changeid.html) already offered by Gerrit that is the Gerrit has been configured always  requiring a Change-Id in the commit message.
  
And the simple way to fix this is to install a hook to automatically insert Change-Id on each commit.  
either way below could obtain the hook from Gerrit for you:

```
$ scp -p -P 29418 <your username>@<your Gerrit review server>:hooks/commit-msg <local path to your git>/.git/hooks/
$ curl -Lo <local path to your git>/.git/hooks/commit-msg <your Gerrit http URL>/tools/hooks/commit-msg
```

More info please trace: https://gerrit-review.googlesource.com/Documentation/cmd-hook-commit-msg.html
