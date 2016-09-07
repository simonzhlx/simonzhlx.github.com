---
layout: post
title: BSOD Diagnostics
permalink: /BSOD-Diagnostics/
---

The [BSOD](https://en.wikipedia.org/wiki/Blue_Screen_of_Death) issue has been bugging me for a while. :tired_face:  

At the very begining, I thought it's regardless of our App because I don't think our service(a windows service running under local service account) gets the power to crash OS.  
However, with the constant duplication of this issue, I have to believe it's related to our App. Even our app is not the root cause, it at least triggers something leading to the crash.  
  
So after learnt about how to analyze the dump file with [the utility Windbg](https://msdn.microsoft.com/en-us/library/windows/hardware/ff551063(v=vs.85).aspx) and how to debug the issue with the dump file and the symbol files(.pdb) of the executables, I started digging into this issue.  
Based on the findings, with the symbol files, the culprit crashing the service was locked. A method that is invoked on each bits job completes threw an unhandled stackoverflow exception.  Furthermore, I found this method was called more than 3 thouand times and I believe that's why the stack of the thread was used up.  
  
Finally, I got this BSOD issue fixed :laughing:  

In general, to diagnose a crash issue the dump files and the symbol files(.pdb) are required if some symbol files of the managed libraries could not be found then [DotPeek](https://www.jetbrains.com/decompiler/) may help by generating corresponding symbol files.  

OK. following steps should be taken when trying to figure out what caused an BSOD error(Blue Screen Of Died, bugcheck or etc.):  

0. Never, ever, fully trust your code :broken_heart:
1. Analyze the memory dump with the utility Windbg, through which we could have a basic view of the error type and which application or module caused the error.  
2. Go over the system event logs to see if there's any clue.  
3. If a specific application could be determined as the culprit of the error then try to dump the specific app by WER(Windows Error Report) 
more detail please refer to http://blogs.msdn.com/b/chaun/archive/2013/11/12/steps-to-catch-a-simple-crash-dump-of-a-crashing-process.aspx  
4. Once got the dump and the symbol files you're able to debug the error with Visual Studio. Open dump file first and make sure all symbol files have been put into the same position where the crash dump occurs.  
5. During debugging the error with Visual Studio, take a notice on the call stack where you could trace track of calls that lead to the crash.  
6. Good luck!
