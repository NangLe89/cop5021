##Proposal

#(a)The problem you wish to solve in overview, and why it is important or interesting.

We want to solve integer overflow detection with our statci analysis tool. Numbers can get distorted from truncation, sign extension, addition, etc. A _long_ may be truncated to a short. Sign extension of 0xf3 (243) could become 0xfffffff3 (-2147483635). Finally, it is possible to add 1 to 255 and get 0.

All common programming languages are affected, but the effects differ, depending on how the language handles integers internally. C and C++ are arguably the most dangerous and are likely to turn an integer overflow into a buffer overrun and arbitrary code execution
This is a serious security problem. For example, truncation allowed a hacker to gain root access. After typing in a user ID, the number became 0, the user ID of root. 

#(b)The static analysis question that your tool will answer. Give one or two brief examples to illustrate the question. Explain why this question can be answered statically (without executing the code)

Our goal is to write a tool that detects integer overflow such as casting operations, operator conversions, arithmetic operations and so on. For example, about operator conversions issue, Most programmers aren't aware that just invoking an operator changes the type of the result. The following code should throw an error.

> unsigned char x = 16;  
x = x*x;  

The variable x becomes 0, not 256. One way to check for this error is to notice that _char * char_ returns an int.

_/*comment*/To Joseph: your following statements are trying to answer "Explain why this question can be answered statically", right?_  
Our first task could be to look for assignment statements
> LHS = RHS

and check that 

> sizeof(LHS) <= sizeof(RHS).

To do this we need to keep track of the types of each variable with our analysis tool.

#(c). The language your tool will analyze. 
/*check*/
>> "We are about to use LLVM compiler infrastructure to complete our project and our main language is C. It is a “low-level virtual machine” which contains a set of modular and reusable compiler, including full support for tool-chain technologies. Its front-ends use C and C++ as a tools for development". We will analyze C language. C is a relatively low-level language and sacrifices safety for execution speed and has the full range of integer tricks up its sleeve. Because of the low-level capabilities of C, integer problems that show up when using it illustrates the issues that the processor encounters.      

#(d). Any related work on this problem that you can find, and why the related work does not solve the problem.

/*comment*/ I have not found any paper/material about integer flow yet. We can update it when we find something. 
/*please check my work*/ These are some research papers on integer overflow problem. Some researchers built a program to check and detect the integer overflow vulnerabilities on C, which is based on semantics and symbolic function models checking [2] [3]. The program checker, called SMT-Constrained Symbolic Execution Engine, run the analysis on a program and find the unrestricted context depth (path sensitive) and find the program path (based on thread identify number), then apply SMT Solving engine and Eclipse extension to check whether or not the program have integer overflow problem or bugs [2].  The other research paper was described another checking program, called Integer Overflow Checker (IOC). The checking program was used to detect the undefined integer behaviors and well-define wraparound behaviors in C/C++ [3]. Moreover, Raphael Ernani Rodrigues, et. al, proposed a new algorithm applied static range analysis to detect the integer overflow (detect the false value out of range) and uses novel techniques to handle comparison between variables [6]. 

#Reference /*updated*/
[1]. 19 Deadly Sins of Software Security: Programming Flaws and How to Fix Them by Michael Howard, David LeBlanc, John Viega, McGraw-Hill Osborne Media, July 2005.
[2]. Muntean, P.; Rahman, M.; Ibing, A.; Eckert, C., "SMT-constrained symbolic execution engine for integer overflow detection in C code," in Information Security for South Africa (ISSA), 2015 , vol., no., pp.1-8, 12-13 Aug. 2015.
[3]. Dietz, W.; Peng Li; Regehr, J.; Adve, V., "Understanding integer overflow in C/C++," in Software Engineering (ICSE), 2012 34th International Conference on , vol., no., pp.760-770, 2-9 June 2012.
[4]. Qixue Xiao; Yu Chen; Hui Huang; Lanhan Qi, "SMT Solvers for Integer Overflows," in Instrumentation, Measurement, Computer, Communication and Control (IMCCC), 2013 Third International Conference on , vol., no., pp.106-113, 21-23 Sept. 2013.
[5]. Cotroneo, D.; Natella, R., "Monitoring of Aging Software Systems Affected by Integer Overflows," in Software Reliability Engineering Workshops (ISSREW), 2012 IEEE 23rd International Symposium on , vol., no., pp.265-270, 27-30 Nov. 2012.
[6]. Rodrigues, R.E.; Sperle Campos, V.H.; Magno Quintao Pereira, F., "A fast and low-overhead technique to secure programs against integer overflows," in Code Generation and Optimization (CGO), 2013 IEEE/ACM International Symposium on , vol., no., pp.1-11, 23-27 Feb. 2013.
[7]. Gok, M.; Schulte, M.J.; Balzola, P.I., "Efficient integer multiplication overflow detection circuits," in Signals, Systems and Computers, 2001. Conference Record of the Thirty-Fifth Asilomar Conference on , vol.2, no., pp.1661-1665 vol.2, 4-7 Nov. 2001.
[8].

