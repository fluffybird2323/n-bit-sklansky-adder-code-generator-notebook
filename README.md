# nbit-sklansky-adder-generator
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

module initializeGP(A,B,G,P);
input A,B;
output G,P;
assign G = A&B;
assign P = A^B;
endmodule
```
a module which assigns input bits A & B to respective G & P wires

<br>
<br>
<br>
known issue:modules / python functions are in random order hence to print code of n-bit sklansky adder run all cells from top to bottom then again start from top and in the end, call to printverilog(n) function will print the verilog code for you
<br><br><br>
<br>
Update: notebook is complete and has been tested via simulation in xilinx vivado 2018.3 and on basys3 FPGA Board
