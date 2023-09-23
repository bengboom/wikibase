## Modules
可以Module里面套Module 按顺序映射端口
> tristate t0(d0, ~s, y);
#### Simple Sample
```verilog
module sillyfunction(input    logic a,b,c,
					 output   logic y);
    assign y = ~a & ~b & ~c | a & ~b & ~c | a & ~b & c;
endmodule
```
#### Inverters
```verilog
module inv(input logic [3:0] a,
		   output logic [3:0] y);
	assign y = ~a;
endmodule
```
#### Multiplexer 
**Verilog有三目运算符**
```verilog
module mux2(input logic [3:0] d0, d1,
			input logic s,
			output logic [3:0] y);
	 assign y = s ? d1 : d0;
endmodule
```
#### Internal Variable
defined as 'logic'. can use the name after defination *similar to C*
like ‘signal’ in VHDL

## *Always* Statement
#### Flip-Flop  
**always @ (sensitivity list)**, like process() in vhdl
always_latch always_ff always_comb
posedge clk ,like rising_edge(clk) in vhdl
```verilog
module flop(input logic clk, input logic [3:0] d, 
			output logic [3:0] q); 
	always_ff @(posedge clk) 
		q <= d; 
endmodule
```
**FSM Asynchronized Reset**
```verilog
// asynchronous reset 
always_ff @(posedge clk, posedge reset) 
	if (reset) 
		q <= 4'b0;
	else if (en) 
		q <= d; 
endmodule
```
**Inverter**
```verilog
module inv(input logic [3:0] a, 
		   output logic [3:0] y); 
	always_comb
		y = ~a;
endmodule
```
= *blocking assignment*
> In SystemVerilog, it is good practice to use blocking assignments for combinational logic and nonblocking assignments for sequential logic.

#### FSM type
```verilog
typedef enum logic [1:0] {S0, S1, S2} statetype; 
statetype [1:0] state, nextstate;
```

## Comment
Same as C++
## Delay
```verilog
// use assign #numver : operation to define
```
## TestBench
```verilog
module testbench1();
	logic a, b, c, y;

	//dut device under test
	sillyfunction dut(a,b,c,y);

	initial begin
		a = 0; b = 0; c = 0; #10; 
		assert (y === 1) else $error("000 failed.");
		c = 1;               #10; 
		b = 1; c = 0;        #10; 
		c = 1;               #10; 
		a = 1; b = 0; c = 0; #10; 
		c = 1;         		 #10; 
		b = 1; c = 0;        #10; 
		c = 1;                #10; //wait for 10 more time units
	end 
endmodule
```