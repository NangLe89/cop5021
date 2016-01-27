##Proposal
Numbers can get distorted from truncation, sign extension, addition, etc. A _long_ may be truncated to a short. Sign extension of 0xf3 (243) could become 0xfffffff3 (-2147483635). Finally, it is possible to add 1 to 255 and get 0.

This is a security problem. For example, truncation allowed a hacker to gain root access. After typing in a user ID, the number became 0, the user ID of root. 

Our goal is to write a tool that detects overflow.

For example, the following code should throw an error.
> unsigned char x = 16;  
x = x*x;  

The variable x becomes 0, not 256. One way to check for this error is to notice that _char * char_ returns an int.

Our first task could be to look for assignment statements
> LHS = RHS

and check that 

> sizeof(LHS) <= sizeof(RHS).

To do this we need to keep track of the types of each variable.


        





