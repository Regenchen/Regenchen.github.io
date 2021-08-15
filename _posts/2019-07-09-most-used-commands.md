---
layout: post
title: What's my Most Used Commands?
tags: Dev
---

Yesterday I was curious about how often I use certain Linux commands - i.e. `ls`, `grep` and `git` - and I knew all the previous commands have been stored in the `history` file. However, I don't feel like going through the file and do the statistics all by myself.

Fortunately someone saved me with this one-line magic:

	history | awk '{CMD[$2]++;count++;}END { for (a in CMD)print CMD[a] " " CMD[a]/count*100 "% " a;}' | grep -v "./" | column -c3 -s " " -t | sort -nr | nl |  head -n10

And the result is:

	1	378  75.6%  git
	2	44   8.8%   ls
	3	28   5.6%   cd
	4	10   2%     python
	5	6    1.2%   spyder
	6	6    1.2%   history
	7	4    0.8%   vi
	8	3    0.6%   rm
	9	3    0.6%   cat
	10	2    0.4%   pip

Wow! I never realized `git` was so dominant in my daily command line engagement. All my works have been done in web-IDEs or local integrated IDEs such as pyCharm and IntelliJ, rescueing me from the burden of local environment configuration.

`Git` actions can also be done in the IDEs now but I feel like more _code-y_
with command line, maybe.


## References

- [https://superuser.com/questions/250227/how-do-i-see-what-my-most-used-linux-command-are]()
- [https://linux.byexamples.com/archives/332/what-is-your-10-common-linux-commands/]()