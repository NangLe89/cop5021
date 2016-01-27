##Proposal
Numbers can get distorted from truncation, sign extension addition, etc. A long may be truncated to a short. Sign extension of 0xf3 (243) could become 0xfffffff3 (-2147483635). Finally, it is possible to add 1 to 255 and get 0.

This is a security problem. For example, truncation allowed a hacker to gain root access. After typing in a user ID, the number became 0, the user ID of root. 

Our goal is to write a tool that detects overflow.

For example, the following code should throw an error.
> unsigned char x = 16;  
x = x*x;  

That's because an unsigned char has a range 0-255. Also because _char * char_ returns an int.

for instance, in the following line
> short len = strlen(input);

because the attacker can give a very large input and overflow len.

But it does not give a warning when compiling
> char x = 16;  
x = x*x;  


In general, we need to check for all statements of the form 
> LHS = RHS

and check that 

> sizeof(LHS) <= sizeof(RHS).

To do this we need to keep track of the types of each variable.
        


To catch this error, we need to keep track of two pieces of information. The types, which helps us to know its limits and the greatest value the variable can become.

How can we keep track of the types of each variable? That sounds easy. For each variable you come across, put it in a table as well as its data type. 

