# Assembly-langguge-X86-Arm-processor-ECE371--Project2

## Full report open "Project1.docx"
First, I have to recognize input and output of the project. Here is are simplify the project into black box 
![alt text](https://github.com/Phasor2/Assembly-langguge-X86-Arm-processor-ECE371--Project1/blob/master/project1%20black%20box.png)
Clearly, I can see there are 2 outputs. It’s my goal to design the system calculates the output that I want. In order to obtain these outputs, I must need 2 sub routines. They are Conversion and Average. 
Once, I realize the subroutine I need to design each sub routine in pseudocode so it can be designed in any language, not just assembly langue. 

# PSEUDOCODE
```
Conversion
			if rough value adding into true value 0
.			If rough value > 20 adding 1 to into rough value save in true value
			If rough value > 39 adding 3 to into rough value save in true value
			If rough value > 59 adding 7 to into rough value save in true value
If rough value > 79 adding 12 to into rough value save in true value
If rough value > 99 adding 20 to into rough value save in true value
	Average:
			Counter is 16
			While the counter is > 0
Previous Sum add true value save into sum
			Until counter is 0 next step
			Sum dive it into 16 
			Check if rounding 
			Round by adding 1 
			Else return to main program
```
# FlowChart
![alt text](https://github.com/Phasor2/Assembly-langguge-X86-Arm-processor-ECE371--Project1/blob/master/ece%20371%20project%201-1.jpg)


# TRUTH TABLE
In order to design a perfect system working smoothly without glitch or mistake, I give a trials value and calculate them beforehand. I make a table of these trials value by the time I finish coding I can run them and compare the values from my table to the values output of the program. It’s a really nice technique I figure out when coming across the project problems. This is similar to truth table.
```
Fahreinth Rough 	 	Expected Fahreinth True 	 
Decimal	Hex	Decimal	Hex
54	0036	57	0039
55	0037	58	003A
66	0042	73	0049
77	004D	84	0054
88	0058	100	0064
99	0063	111	006F
78	004E	85	0055
63	003F	70	0046
82	0052	94	005E
24	0018	25	0019
63	003F	70	0046
74	004A	81	0051
84	0054	96	0060
54	0036	57	0039
94	005E	106	006A
80	0050	92	005C

Using offset to know when to rounded	 	 	 	
TRUTH TABLE FOR ROUNDING	 	 	 	
1	1	r<0.1	+0	
2	10	r<0.5	+0	
3	11	r<0.2	+0	
4	100	r<0.25	+0	
5	101	r<0.35	+0	
6	110	r<0.4	+0	
7	111	r<0.45	+0	
8	1000	r<0.50	+0	
9	1001	r<0.55	+1	<- rounding start from here
10	1010	r<0.60	+1	
11	1011	r<0.65	+1	
12	1100	r<0.70	+1	
13	1101	r<0.75	+1	
14	1110	r<0.80	+1	
15	1111	r<0.85	+1	


SUM	1259	04EB
Average	78.6875	
```

# ALGORTIHM
```
Phong Nguyen
Algorithm Project 1 ECE 371
Mainline 
Main:
Counter R3= 16
Load input pointer Fahrenheit Rough array into R0 half word
Load input pointer Fahrenheit True array into R1 half word
Load input pointer correction factor array into R2 half word
Load input pointer Average in to R7 word
Calling STEP1 (BL)
NOP
Calling STEP2 (BL) 
NOP
STEP1:	Save registers into the stack (R0-R9,R14)
Conversion:
		 
Initialize pointer R2 = 0 correction factor
		+Condition (Check what kind of A/D reading)
			@ Initially R2 pointer is at 0 
			If (R4 > 20): increment pointer R2 for #2 	@ R2= 1
			If (R4 > 39): increment pointer R2 for #2	@ R2=2
			If (R4 > 59): increment pointer R2 for #2 	@ R2= 3
			If (R4 > 79): increment pointer R2 for #2	@ R2=4
			If (R4 > 99): increment pointer R2 for #2	@ R2=5	
		Now I have R2 pointer condition 
		
+Correction (Add the correction factor into Rough value)
			Load half word R0 into R4 (F rough)
			Load half word R1 into R5 (F true)
			Load half word R2 into R6 (correction factor)
			R5 = R4 + R6		        @ Adding correction
Store R5 back R1 array
			R0 increment pointer into #2 @ R0++ for the next rough value
			R1 increment pointer into #2 @ R2++ for the next true value
			R3 = R3 – 1 		        @ decrement counter
			Keep calling conversion BNE Conversion
			Restore the register by popping from the stack (R0-R9, R14)
			Move PC, R14

STEP2: Save registers into the stack (R0-R9,R14)
Average:
	Load half word R1 into R5 (F true)
	Load word R7 into R8 a word (average)
	R6= R5+R6 @ adding for sum 
	R1 increment pointer into #2 @ R1 ++ for next true value
	R3=R3-1 @decrement R3
	If R3 = 0 then next otherwise keep calling Average
	R6 is now the Sum of 16 F true Values
	R9 will mask 4 loIr bit of R6 (R9 will decide how I should rounded for average number)
	R8 = Shift the values to the right 4 bits (This is same as take the value dive by 16)
	Compare R9 and 0x8 if update N flags and Z flags
	If PL plus positive or zero (N clear) Called rounded

END:  			Restore the register by popping from the stack (R0-R9, R14)
			Return to mainline program MOV PC, R14

Rounded:
R8 = R8+1 @ rounded R8 by adding one (round up)
Jump to end
```
# Stack
![alt text](https://github.com/Phasor2/Assembly-langguge-X86-Arm-processor-ECE371--Project1/blob/master/My%20stack%20R13.png)

