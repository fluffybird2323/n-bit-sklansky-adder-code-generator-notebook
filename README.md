# N-bit-sklansky-adder-generator
<h2>Group 4 :Pramod(18114055),Ishaan Yadav(18114032),karnati Vivek Veman(18114037),G.vamsi krishna(18114023) </h2>
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
sample verilog code of 16 bit adder
```verilog

module initializeGP(A,B,G,P);
input A,B;
output G,P;
assign G = A&B;
assign P = A^B;
endmodule

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
 wire P0_5;
 wire G0_6;
 wire P0_6;
 wire G0_7;
 wire P0_7;
 wire G0_8;
 wire P0_8;
 wire G0_9;
 wire P0_9;
 wire G0_10;
 wire P0_10;
 wire G0_11;
 wire P0_11;
 wire G0_12;
 wire P0_12;
 wire G0_13;
 wire P0_13;
 wire G0_14;
 wire P0_14;
 wire G0_15;
 wire P0_15;
 wire G0_16;
 wire P0_16;
 wire G1_0;
 wire P1_0;
 wire G1_1;
 wire P1_1;
 wire G1_2;
 wire P1_2;
 wire G1_3;
 wire P1_3;
 wire G1_4;
 wire P1_4;
 wire G1_5;
 wire P1_5;
 wire G1_6;
 wire P1_6;
 wire G1_7;
 wire P1_7;
 wire G1_8;
 wire P1_8;
 wire G1_9;
 wire P1_9;
 wire G1_10;
 wire P1_10;
 wire G1_11;
 wire P1_11;
 wire G1_12;
 wire P1_12;
 wire G1_13;
 wire P1_13;
 wire G1_14;
 wire P1_14;
 wire G1_15;
 wire P1_15;
 wire G1_16;
 wire P1_16;
 wire G2_0;
 wire P2_0;
 wire G2_1;
 wire P2_1;
 wire G2_2;
 wire P2_2;
 wire G2_3;
 wire P2_3;
 wire G2_4;
 wire P2_4;
 wire G2_5;
 wire P2_5;
 wire G2_6;
 wire P2_6;
 wire G2_7;
 wire P2_7;
 wire G2_8;
 wire P2_8;
 wire G2_9;
 wire P2_9;
 wire G2_10;
 wire P2_10;
 wire G2_11;
 wire P2_11;
 wire G2_12;
 wire P2_12;
 wire G2_13;
 wire P2_13;
 wire G2_14;
 wire P2_14;
 wire G2_15;
 wire P2_15;
 wire G2_16;
 wire P2_16;
 wire G3_0;
 wire P3_0;
 wire G3_1;
 wire P3_1;
 wire G3_2;
 wire P3_2;
 wire G3_3;
 wire P3_3;
 wire G3_4;
 wire P3_4;
 wire G3_5;
 wire P3_5;
 wire G3_6;
 wire P3_6;
 wire G3_7;
 wire P3_7;
 wire G3_8;
 wire P3_8;
 wire G3_9;
 wire P3_9;
 wire G3_10;
 wire P3_10;
 wire G3_11;
 wire P3_11;
 wire G3_12;
 wire P3_12;
 wire G3_13;
 wire P3_13;
 wire G3_14;
 wire P3_14;
 wire G3_15;
 wire P3_15;
 wire G3_16;
 wire P3_16;
 wire G4_0;
 wire P4_0;
 wire G4_1;
 wire P4_1;
 wire G4_2;
 wire P4_2;
 wire G4_3;
 wire P4_3;
 wire G4_4;
 wire P4_4;
 wire G4_5;
 wire P4_5;
 wire G4_6;
 wire P4_6;
 wire G4_7;
 wire P4_7;
 wire G4_8;
 wire P4_8;
 wire G4_9;
 wire P4_9;
 wire G4_10;
 wire P4_10;
 wire G4_11;
 wire P4_11;
 wire G4_12;
 wire P4_12;
 wire G4_13;
 wire P4_13;
 wire G4_14;
 wire P4_14;
 wire G4_15;
 wire P4_15;
 wire G4_16;
 wire P4_16;
 wire G5_0;
 wire P5_0;
 wire G5_1;
 wire P5_1;
 wire G5_2;
 wire P5_2;
 wire G5_3;
 wire P5_3;
 wire G5_4;
 wire P5_4;
 wire G5_5;
 wire P5_5;
 wire G5_6;
 wire P5_6;
 wire G5_7;
 wire P5_7;
 wire G5_8;
 wire P5_8;
 wire G5_9;
 wire P5_9;
 wire G5_10;
 wire P5_10;
 wire G5_11;
 wire P5_11;
 wire G5_12;
 wire P5_12;
 wire G5_13;
 wire P5_13;
 wire G5_14;
 wire P5_14;
 wire G5_15;
 wire P5_15;
 wire G5_16;
 wire P5_16;
 wire G6_0;
 wire P6_0;
 wire G6_1;
 wire P6_1;
 wire G6_2;
 wire P6_2;
 wire G6_3;
 wire P6_3;
 wire G6_4;
 wire P6_4;
 wire G6_5;
 wire P6_5;
 wire G6_6;
 wire P6_6;
 wire G6_7;
 wire P6_7;
 wire G6_8;
 wire P6_8;
 wire G6_9;
 wire P6_9;
 wire G6_10;
 wire P6_10;
 wire G6_11;
 wire P6_11;
 wire G6_12;
 wire P6_12;
 wire G6_13;
 wire P6_13;
 wire G6_14;
 wire P6_14;
 wire G6_15;
 wire P6_15;
 wire G6_16;
 wire P6_16;
 wire G7_0;
 wire P7_0;
 wire G7_1;
 wire P7_1;
 wire G7_2;
 wire P7_2;
 wire G7_3;
 wire P7_3;
 wire G7_4;
 wire P7_4;
 wire G7_5;
 wire P7_5;
 wire G7_6;
 wire P7_6;
 wire G7_7;
 wire P7_7;
 wire G7_8;
 wire P7_8;
 wire G7_9;
 wire P7_9;
 wire G7_10;
 wire P7_10;
 wire G7_11;
 wire P7_11;
 wire G7_12;
 wire P7_12;
 wire G7_13;
 wire P7_13;
 wire G7_14;
 wire P7_14;
 wire G7_15;
 wire P7_15;
 wire G7_16;
 wire P7_16;
 wire G8_0;
 wire P8_0;
 wire G8_1;
 wire P8_1;
 wire G8_2;
 wire P8_2;
 wire G8_3;
 wire P8_3;
 wire G8_4;
 wire P8_4;
 wire G8_5;
 wire P8_5;
 wire G8_6;
 wire P8_6;
 wire G8_7;
 wire P8_7;
 wire G8_8;
 wire P8_8;
 wire G8_9;
 wire P8_9;
 wire G8_10;
 wire P8_10;
 wire G8_11;
 wire P8_11;
 wire G8_12;
 wire P8_12;
 wire G8_13;
 wire P8_13;
 wire G8_14;
 wire P8_14;
 wire G8_15;
 wire P8_15;
 wire G8_16;
 wire P8_16;
 wire G9_0;
 wire P9_0;
 wire G9_1;
 wire P9_1;
 wire G9_2;
 wire P9_2;
 wire G9_3;
 wire P9_3;
 wire G9_4;
 wire P9_4;
 wire G9_5;
 wire P9_5;
 wire G9_6;
 wire P9_6;
 wire G9_7;
 wire P9_7;
 wire G9_8;
 wire P9_8;
 wire G9_9;
 wire P9_9;
 wire G9_10;
 wire P9_10;
 wire G9_11;
 wire P9_11;
 wire G9_12;
 wire P9_12;
 wire G9_13;
 wire P9_13;
 wire G9_14;
 wire P9_14;
 wire G9_15;
 wire P9_15;
 wire G9_16;
 wire P9_16;
 wire G10_0;
 wire P10_0;
 wire G10_1;
 wire P10_1;
 wire G10_2;
 wire P10_2;
 wire G10_3;
 wire P10_3;
 wire G10_4;
 wire P10_4;
 wire G10_5;
 wire P10_5;
 wire G10_6;
 wire P10_6;
 wire G10_7;
 wire P10_7;
 wire G10_8;
 wire P10_8;
 wire G10_9;
 wire P10_9;
 wire G10_10;
 wire P10_10;
 wire G10_11;
 wire P10_11;
 wire G10_12;
 wire P10_12;
 wire G10_13;
 wire P10_13;
 wire G10_14;
 wire P10_14;
 wire G10_15;
 wire P10_15;
 wire G10_16;
 wire P10_16;
 wire G11_0;
 wire P11_0;
 wire G11_1;
 wire P11_1;
 wire G11_2;
 wire P11_2;
 wire G11_3;
 wire P11_3;
 wire G11_4;
 wire P11_4;
 wire G11_5;
 wire P11_5;
 wire G11_6;
 wire P11_6;
 wire G11_7;
 wire P11_7;
 wire G11_8;
 wire P11_8;
 wire G11_9;
 wire P11_9;
 wire G11_10;
 wire P11_10;
 wire G11_11;
 wire P11_11;
 wire G11_12;
 wire P11_12;
 wire G11_13;
 wire P11_13;
 wire G11_14;
 wire P11_14;
 wire G11_15;
 wire P11_15;
 wire G11_16;
 wire P11_16;
 wire G12_0;
 wire P12_0;
 wire G12_1;
 wire P12_1;
 wire G12_2;
 wire P12_2;
 wire G12_3;
 wire P12_3;
 wire G12_4;
 wire P12_4;
 wire G12_5;
 wire P12_5;
 wire G12_6;
 wire P12_6;
 wire G12_7;
 wire P12_7;
 wire G12_8;
 wire P12_8;
 wire G12_9;
 wire P12_9;
 wire G12_10;
 wire P12_10;
 wire G12_11;
 wire P12_11;
 wire G12_12;
 wire P12_12;
 wire G12_13;
 wire P12_13;
 wire G12_14;
 wire P12_14;
 wire G12_15;
 wire P12_15;
 wire G12_16;
 wire P12_16;
 wire G13_0;
 wire P13_0;
 wire G13_1;
 wire P13_1;
 wire G13_2;
 wire P13_2;
 wire G13_3;
 wire P13_3;
 wire G13_4;
 wire P13_4;
 wire G13_5;
 wire P13_5;
 wire G13_6;
 wire P13_6;
 wire G13_7;
 wire P13_7;
 wire G13_8;
 wire P13_8;
 wire G13_9;
 wire P13_9;
 wire G13_10;
 wire P13_10;
 wire G13_11;
 wire P13_11;
 wire G13_12;
 wire P13_12;
 wire G13_13;
 wire P13_13;
 wire G13_14;
 wire P13_14;
 wire G13_15;
 wire P13_15;
 wire G13_16;
 wire P13_16;
 wire G14_0;
 wire P14_0;
 wire G14_1;
 wire P14_1;
 wire G14_2;
 wire P14_2;
 wire G14_3;
 wire P14_3;
 wire G14_4;
 wire P14_4;
 wire G14_5;
 wire P14_5;
 wire G14_6;
 wire P14_6;
 wire G14_7;
 wire P14_7;
 wire G14_8;
 wire P14_8;
 wire G14_9;
 wire P14_9;
 wire G14_10;
 wire P14_10;
 wire G14_11;
 wire P14_11;
 wire G14_12;
 wire P14_12;
 wire G14_13;
 wire P14_13;
 wire G14_14;
 wire P14_14;
 wire G14_15;
 wire P14_15;
 wire G14_16;
 wire P14_16;
 wire G15_0;
 wire P15_0;
 wire G15_1;
 wire P15_1;
 wire G15_2;
 wire P15_2;
 wire G15_3;
 wire P15_3;
 wire G15_4;
 wire P15_4;
 wire G15_5;
 wire P15_5;
 wire G15_6;
 wire P15_6;
 wire G15_7;
 wire P15_7;
 wire G15_8;
 wire P15_8;
 wire G15_9;
 wire P15_9;
 wire G15_10;
 wire P15_10;
 wire G15_11;
 wire P15_11;
 wire G15_12;
 wire P15_12;
 wire G15_13;
 wire P15_13;
 wire G15_14;
 wire P15_14;
 wire G15_15;
 wire P15_15;
 wire G15_16;
 wire P15_16;
 wire G16_0;
 wire P16_0;
 wire G16_1;
 wire P16_1;
 wire G16_2;
 wire P16_2;
 wire G16_3;
 wire P16_3;
 wire G16_4;
 wire P16_4;
 wire G16_5;
 wire P16_5;
 wire G16_6;
 wire P16_6;
 wire G16_7;
 wire P16_7;
 wire G16_8;
 wire P16_8;
 wire G16_9;
 wire P16_9;
 wire G16_10;
 wire P16_10;
 wire G16_11;
 wire P16_11;
 wire G16_12;
 wire P16_12;
 wire G16_13;
 wire P16_13;
 wire G16_14;
 wire P16_14;
 wire G16_15;
 wire P16_15;
 wire G16_16;
 wire P16_16;
assign G0_0 = C_in;
assign P0_0 = 0;
initializeGP a1(A1,B1,G1_1,P1_1);
initializeGP a2(A2,B2,G2_2,P2_2);
initializeGP a3(A3,B3,G3_3,P3_3);
initializeGP a4(A4,B4,G4_4,P4_4);
initializeGP a5(A5,B5,G5_5,P5_5);
initializeGP a6(A6,B6,G6_6,P6_6);
initializeGP a7(A7,B7,G7_7,P7_7);
initializeGP a8(A8,B8,G8_8,P8_8);
initializeGP a9(A9,B9,G9_9,P9_9);
initializeGP a10(A10,B10,G10_10,P10_10);
initializeGP a11(A11,B11,G11_11,P11_11);
initializeGP a12(A12,B12,G12_12,P12_12);
initializeGP a13(A13,B13,G13_13,P13_13);
initializeGP a14(A14,B14,G14_14,P14_14);
initializeGP a15(A15,B15,G15_15,P15_15);
initializeGP a16(A16,B16,G16_16,P16_16);


  greycell cell1_1(G1_1,P1_1,G0_0,G1_0);

  blkcell cell1_3(G3_3,P3_3,G2_2,P2_2,G3_2,P3_2);

  blkcell cell1_5(G5_5,P5_5,G4_4,P4_4,G5_4,P5_4);

  blkcell cell1_7(G7_7,P7_7,G6_6,P6_6,G7_6,P7_6);

  blkcell cell1_9(G9_9,P9_9,G8_8,P8_8,G9_8,P9_8);

  blkcell cell1_11(G11_11,P11_11,G10_10,P10_10,G11_10,P11_10);

  blkcell cell1_13(G13_13,P13_13,G12_12,P12_12,G13_12,P13_12);

  blkcell cell1_15(G15_15,P15_15,G14_14,P14_14,G15_14,P15_14);


  greycell cell2_2(G2_2,P2_2,G1_0,G2_0);
  greycell cell2_3(G3_2,P3_2,G1_0,G3_0);


  blkcell cell2_6(G6_6,P6_6,G5_4,P5_4,G6_4,P6_4);
  blkcell cell2_7(G7_6,P7_6,G5_4,P5_4,G7_4,P7_4);


  blkcell cell2_10(G10_10,P10_10,G9_8,P9_8,G10_8,P10_8);
  blkcell cell2_11(G11_10,P11_10,G9_8,P9_8,G11_8,P11_8);


  blkcell cell2_14(G14_14,P14_14,G13_12,P13_12,G14_12,P14_12);
  blkcell cell2_15(G15_14,P15_14,G13_12,P13_12,G15_12,P15_12);




  greycell cell3_4(G4_4,P4_4,G3_0,G4_0);
  greycell cell3_5(G5_4,P5_4,G3_0,G5_0);
  greycell cell3_6(G6_4,P6_4,G3_0,G6_0);
  greycell cell3_7(G7_4,P7_4,G3_0,G7_0);




  blkcell cell3_12(G12_12,P12_12,G11_6,P11_6,G12_6,P12_6);
  blkcell cell3_13(G13_12,P13_12,G11_6,P11_6,G13_6,P13_6);
  blkcell cell3_14(G14_12,P14_12,G11_6,P11_6,G14_6,P14_6);
  blkcell cell3_15(G15_12,P15_12,G11_6,P11_6,G15_6,P15_6);








  greycell cell4_8(G8_6,P8_6,G5_0,G8_0);
  greycell cell4_9(G9_6,P9_6,G5_0,G9_0);
  greycell cell4_10(G10_6,P10_6,G5_0,G10_0);
  greycell cell4_11(G11_6,P11_6,G5_0,G11_0);
  greycell cell4_12(G12_6,P12_6,G5_0,G12_0);
  greycell cell4_13(G13_6,P13_6,G5_0,G13_0);
  greycell cell4_14(G14_6,P14_6,G5_0,G14_0);
  greycell cell4_15(G15_6,P15_6,G5_0,G15_0);
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
endmodule

```
