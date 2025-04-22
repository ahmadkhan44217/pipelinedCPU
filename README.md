This is a simple design of Theoretical pipelined CPU, each instruction taking 4 Clock cycles

1.	Graphical Explanation of the part of circuits included in each step of the pipeline.
 Note: check out **Annotated CPU Image** 
A.	This Unit is responsible for calculating the address next instruction.
B.	Memory from which instruction is fetched and J cmd along with the multiplexer at the bottom is the unit responsible to halt PC in case a cycle is to be wasted.
C.	It is the halter.
D.	It is the register file and multiplexer which decide which 3 bits to use as Register 2.
E.	It is the main Control unit which set flags based on the opcode.
F.	It is the forwarding unit responsible for knowing any data dependency in the next instruction.
G.	It is ALU, which calculates all the arithmetic and decide which values to use in the arithmetics.
H.	It is the sign extender.
I.	It is comparator to evaluate BLE instruction.
J.	It is the Data memory and multiplexer which decide write back ALU output or data loaded from memory.
 

Our Instruction are divided into 5 stage:
1.	Fetch: PC calculate the next address of instruction and the instruction is fetch, halt signal is generated here as well for which we have halter unit
2.	Instruction decode: Control Unit decodes the flags for the appropriate opcodes and value from register are read, forwarding is also implemented in this cycle.
3.	Execution: ALU executes the instructions, if no ALU is required this step is ignored. Also it is decided whether to use register value or sign extended immediate here.
4.	Memory access: where data is stored or loaded from memory.
5.	Write back: data is stored in the registers in this cycle.
	Note: Divider unit is only register files which allow propagation of flags and data to next stage

2.	Explanation of how does the processor deal with different types of hazards.
	For structural hazard, we have separate instruction and data memory.
	For control, one cycle is wasted for any unconditional jump such as jump, call, rtn, reboot, and for ble 2 cycles are wasted. We have a j cmd unit which decides for how many cycles to stop pc. 
	For data hazards we have 2 separate forwarding which determines are data to be used in next 2 instructions and provide the value as required. In case of load, PC make waste 1 cycle and have temporary halt. And only value provided to a instruction 2 addresses after the load instruction. 

3.	Example Assembly code execution result
Test program 5:
instruction	machine code (binary)	machine code (hex)
li $r1, 10	              1101100000001010	D80A
li $r2, 8	              1101100100001000	D908
li $r3, 0	              1101101000000000	DA00
store $r1, $r3	0110100001000000	6840
load $r4, $r3	0111001101000000	7340
add $r5, $r4, $r2	0001110001100100	1C64
ble $r2, $r4, 2	0010100101100010	2962
and $r6, $r4, $r2	0000110101100100	0D64
halt	                             1111111111111111	FFFF
