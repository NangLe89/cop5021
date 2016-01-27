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

We will analyze C language. C is a relatively low-level language and sacrifices safety for execution speed and has the full range of integer tricks up its sleeve. Because of the low-level capabilities of C, integer problems that show up when using it illustrates the issues that the processor encounters.      

#(d). Any related work on this problem that you can find, and why the related work does not solve the problem.

/*comment*/ I have not found any paper/material about integer flow yet. We can update it when we find something. 


#Reference
[1]. 19 Deadly Sins of Software Security: Programming Flaws and How to Fix Them by Michael Howard, David LeBlanc, John Viega, McGraw-Hill Osborne Media, July 2005.

