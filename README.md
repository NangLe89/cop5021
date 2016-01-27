##Proposal
###What is the problem?
Numbers can get distorted from truncation, sign extension addition, etc. A long may be truncated to a short. Sign extension of 0xf3 (243) could become 0xfffffff3 (-2147483635). Finally, it is possible to add 1 to 255 and get 0.

This may cause security problems. For example, truncation once allowed a hacker to gain root access. After typing in a user ID, the number became 0, the user ID of root. 

###Where do errors occur?


Our goal is to detect integer overflow.

When compiling the line 
> char x = 256; 

gcc will give the warning
> signed.c : In function 'main':  
signed.c6:12: warning: overflow in implicit constant conversion [-Woverflow]  
char x = 256;  
        ^  
        
But it does not give a warning when compiling
> char x = 16;  
x = x*x;  

To catch this error, we need to keep track of two pieces of information. The types, which helps us to know its limits and the greatest value the variable can become.

How can we keep track of the types of each variable? That sounds easy. For each variable you come across, put it in a table as well as its data type. 

