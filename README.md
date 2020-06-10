# Quine-McCluskey-Minimization-Technique
The Quine–McCluskey algorithm (QMC), also known as the method of prime implicants or tabulation method, is a method used for minimization of Boolean functions for large numbers of variables. There is no limit to number of input variables to the algorithm but higher number of input variables will require more time and processing to solve. The highest number of variable I have tested this code is 13 variables. That means the bianry was 8192 bit or 1K byte. It took several minutes for code to process on my intel i5, 7th generation processor. You can either give input in binary string like [0 0 0 1]  or just [3] as minterm, both will give AND gate as result (A0.A1). To do that comment the either one of the first two section with %{ ... %}.  Make sure one and only one of them should be commented at a time. Commenting first section and using second section will mean mean the program will ask for minterms and commenting second section and using first section will mean program will ask for bianry string input.
