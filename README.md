# N-bit-sklansky-adder-generator
<h2>python notebook to generate code of n-bit sklansky adder</h2>

To open the notebook you can use <a href="https://www.anaconda.com/distribution/">anaconda</a> -> Jupyter Notebook
<br>
<br>
<br>
<h3>Let's see how this works</h3>

```python
def printverilog(n):
    initializeGP()
    initialiseverilog(n)
    print_wires(n)
    assignGP(n)
    #print("assign G_1_0 = 0;")
    for a in range(1,1+ceil(log(n)/log(2))):
        for b in range(n):
            s = findcell(a,b)
            i,j,k = findijk(a,b)
            printcell(s,i,j,k,a,b)
    computeS(n)
    print("assign C_out = (G"+str(n-1)+"_0"+"&P"+str(n)+"_"+str(n)+")|G"+str(n)+"_"+str(n)+";")
    print("endmodule")
```
 The above module prints the verilog code by making calls to other functions which are explained below

```python
initializeGP()
```
prints

```verilog 
//verilog_code
module initializeGP(A,B,G,P);
input A,B;
output G,P;
assign G = A&B;
assign P = A^B;
endmodule
```
a module which assigns input bits A & B to respective G & P wires which are fed into the black and grey cell network

```python
initialiseverilog(n)
```
prints module declarations for black and grey cells along with module declaration of sklansky adder eg: for 16 bit

```verilog
//verilog_code
module blkcell(G6_8,P6_8,G7_10,P7_10,G6_10,P6_10);
  input G6_8,P6_8,G7_10,P7_10;
  output G6_10,P6_10;
  wire s1 ;
  assign s1 = P6_8 & G7_10;
  assign G6_10=s1|G6_8;
  assign P6_10=P6_8&P7_10;
endmodule

module greycell(G4_3,P4_3,G2_2,G4_2);
  input G4_3,P4_3,G2_2;
  output G4_2;
  wire s1 ;
  assign s1 = P4_3 & G2_2;
  assign G4_2=s1|G4_3;
endmodule


module sklansky(C_in,A1,A2,A3,A4,A5,A6,A7,A8,A9,A10,A11,A12,A13,A14,A15,A16,B1,B2,B3,B4,B5,B6,B7,B8,B9,B10,B11,B12,B13,B14,B15,B16,S1,S2,S3,S4,S5,S6,S7,S8,S9,S10,S11,S12,S13,S14,S15,S16,C_out);
input C_in,A1,A2,A3,A4,A5,A6,A7,A8,A9,A10,A11,A12,A13,A14,A15,A16,B1,B2,B3,B4,B5,B6,B7,B8,B9,B10,B11,B12,B13,B14,B15,B16;
output C_out,S1,S2,S3,S4,S5,S6,S7,S8,S9,S10,S11,S12,S13,S14,S15,S16;

```
it is followed by 
```python
print_wires(n)
```
this module prints all the required wires eg:
```verilog
//verilog_code
wire G0_0;
 wire P0_0;
 wire G0_1;
 wire P0_1;
 wire G0_2;
 wire P0_2;
 wire G0_3;
 wire P0_3;
 wire G0_4;
 wire P0_4;
 wire G0_5;

```

next is
```python
assignGP(n)
```
assigns values to all G & P wires through the initialize GP module eg
```verilog
//verilog_code
initializeGP a1(A1,B1,G1_1,P1_1);
initializeGP a2(A2,B2,G2_2,P2_2);
initializeGP a3(A3,B3,G3_3,P3_3);
initializeGP a4(A4,B4,G4_4,P4_4);
initializeGP a5(A5,B5,G5_5,P5_5);
initializeGP a6(A6,B6,G6_6,P6_6);
initializeGP a7(A7,B7,G7_7,P7_7);
initializeGP a8(A8,B8,G8_8,P8_8);
```

finally calls to the black and grey cells are made using a for loop as follows
```python
for a in range(1,1+ceil(log(n)/log(2))):
        for b in range(n):
            s = findcell(a,b)
            i,j,k = findijk(a,b)
            printcell(s,i,j,k,a,b)

```

calls the black and grey cell modules with in and out terminals as per the sklansky adder network eg:
```verilog
//verilog_code
blkcell cell1_13(G13_13,P13_13,G12_12,P12_12,G13_12,P13_12);
  blkcell cell1_15(G15_15,P15_15,G14_14,P14_14,G15_14,P15_14);
  greycell cell2_2(G2_2,P2_2,G1_0,G2_0);
  greycell cell2_3(G3_2,P3_2,G1_0,G3_0);
  blkcell cell2_6(G6_6,P6_6,G5_4,P5_4,G6_4,P6_4);
  blkcell cell2_7(G7_6,P7_6,G5_4,P5_4,G7_4,P7_4);
```
at last 
```python
computeS(n)
```
finally computeS(n) assigns value of sum bits taking G & P wires as input eg for n = 16
```verilog
//verilog_code
assign S1 = G0_0^P1_1;
assign S2 = G1_0^P2_2;
assign S3 = G2_0^P3_3;
assign S4 = G3_0^P4_4;
assign S5 = G4_0^P5_5;
assign S6 = G5_0^P6_6;
assign S7 = G6_0^P7_7;
assign S8 = G7_0^P8_8;
assign S9 = G8_0^P9_9;
assign S10 = G9_0^P10_10;
assign S11 = G10_0^P11_11;
assign S12 = G11_0^P12_12;
assign S13 = G12_0^P13_13;
assign S14 = G13_0^P14_14;
assign S15 = G14_0^P15_15;
assign S16 = G15_0^P16_16;
assign C_out = (G15_0&P16_16)|G16_16;
```
and most importantly 
```python
print("endmodule")
```
prints out
```verilog
endmodule
```
<h2>The End</h2>
<br>
<br>
<br>
known issue:modules / python functions are in random order hence to print code of n-bit sklansky adder run all cells from top to bottom then again start from top and in the end, call to printverilog(n) function will print the verilog code for you
<br><br><br>
<br>
Update: notebook is complete and has been tested via simulation in xilinx vivado 2018.3 and on basys3 FPGA Board
