---
layout: post
category: lessons
tags: [makefile, read, debug]
---
{% include JB/setup %}

Following are few tips on reading or understanding a makefile and its flow. This post could be useful for those who are new  to make. To explain in very short how makefile works, mMakefile has some targets, and the targets have some dependencies. If the dependencies are satisfied, then the command below that target is executed. Dependency could also be an target.

###Understand the flow

Sometimes one target in the makefile calls many targets creating a chain of commands. Suppose you want to understand the flow of makefile, in that case print a number using  `echo` command after every target. The numbers, should be unique and sequential, so that you can trace the flow.


###Executing the commands

Also you can execute the command one by one on the console. Most of the times special macros are used in the makefile like `$@` or `$<`. It would be helpful if you get meaning of those but if you don't still you can understand what those macros and the command using the macros are doing. Run the makefile, or the target and make sure you do not use the `-s` option while running, or else nothing would be printed on console. This would then print the commands on console along with the substituted values of special variables. Once you have those commands, you again could try executing command one by one. If you dont know the command use `man` command to understand what the command does.


###Printing the variables

`echo` command could also be used to print the special macros or variable values. For instance you have a variable or a special macro, just print the values with `echo` command. This also would help in understanding the commands used and there effect in makefile. 

