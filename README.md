Download Link: https://assignmentchef.com/product/solved-ee371-lab-2-memory-blocks
<br>
In computer systems it is necessary to provide a substantial amount of memory. If a system is implemented using FPGA technology, it is possible to provide some amount of memory by using the memory resources that exist in the FPGA device. In this lab we will examine the general issues involved in implementing such memory.

<strong>Introduction </strong>

A diagram of the Random Access Memory (RAM) module that we will implement is shown in Figure 1a. It contains 32 four-bit words (rows), which are accessed using a five-bit address port, a four-bit data port, and a write control input.

Figure 1. 32×4 RAM module

The FPGA included on the DE1-SoC board provides dedicated memory resources and has M10K blocks, where each M10K block contains 10240 memory bits. The M10k blocks can be configured to implement memories of various sizes. A common term used to specify the size of a memory is its aspect ratio, which gives the depth in words and the width in bits (depth x width). In this lab we will use an aspect ratio that is four bits wide, and we will use only the first 32 words in the memory.

There are two important features of the M10K blocks. First, they include registers that can be used to synchronize all the input and output signals to a clock input. Second, the blocks have separate ports for data being written to the memory and data being read from the memory. Given these requirements, we will implement the modified 32 x 4 RAM module shown in Figure 1b. It includes registers for the address, data input, and write ports, and uses a separate unregistered data output port.

<strong>Task 1: </strong>

Commonly used logic structures, such as adders, registers, counters and memories, can be implemented in an FPGA chip by using prebuilt modules that are provided in libraries. In this exercise we will use such a module to implement the memory shown in Figure 1b.

<ol>

 <li>Create a new Quartus project to implement the memory module.</li>

 <li>To open the IP Catalog in the Quartus software click on Tools &gt; IP Catalog.

  <ol>

   <li>In the IP Catalog window choose the RAM: 1-PORT module, which is found under the Basic Functions &gt; On Chip Memory category.</li>

   <li>Select SystemVerilog HDL as the type of output file to create, give the file the name ram32x4.sv, and click OK. If SystemVerilog is not available as an option, create the file in Verilog, and name it ram32x4.v.</li>

   <li>In the configuration window, specify a memory size of 32 four-bit words. Select M10K. Also on this screen accept the default setting to use a single clock for the memory’s registers, and then advance to the next page.</li>

   <li>On this page deselect the setting called ’q’ output port under the category <em>Which ports should be registered?</em> This setting creates a RAM module that matches the structure in Figure 1b, with registered input ports and unregistered output ports.</li>

   <li>Accept defaults for the rest of the settings in the Wizard and click the Finish button to exit from this tool.</li>

  </ol></li>

</ol>

Examine the ram32x4.sv (or ram32x4.v) file which defines the following module:

module ram32x4 (address, clock, data, wren, q);        input             [4:0]  address;              input      clock;             input             [3:0]  data;       input      wren;             output [3:0]  q;

<ol start="3">

 <li>Create a new top-level SystemVerilog file and instantiate the ram32x4 module, using appropriate input and output signals for the memory ports given in Figure 1b. Compile the circuit. Observe in the Compilation Report, under “Total block memory bits,” that the Quartus Compiler uses 128 bits in one of the FPGA memory blocks to implement the RAM circuit.</li>

 <li>Simulate the behavior of your circuit in ModelSim and ensure that you can read and write data in the memory.

  <ol>

   <li>You may encounter simulation errors. Try the following:

    <ol>

     <li><em>Module ‘ram_testbench’ does not have a timeunit/timeprecision specification in effect, but other modules do. </em></li>

    </ol></li>

  </ol></li>

</ol>

Solution: Add the following line above your testbench module declaration

`timescale 1 ps / 1 ps

<ol>

 <li><em>Instantiation of ‘altsyncram’ failed. The design unit was not found. </em></li>

</ol>

Solution: Go to “Start Simulation…” under “Simulate” and under the libraries tab, add “altera_mf_ver” under “Search Libraries First ( -Lf ).” Then, select your testbench module under the “Design” tab and select “OK”




<strong>Task 2: </strong>

Now, we want to realize the memory circuit in the FPGA on the DE1-SoC board and use slide switches to load some data into the created memory. We also want to display the contents of the RAM on the 7- segment displays.

<ol>

 <li>Make a new Quartus project.</li>

 <li>Create another SystemVerilog file that instantiates the ram32x4 module and that includes the required input and output pins on your DE1-SoC board.

  <ol>

   <li>Use slide switches SW 3−0 to provide input data for the RAM and switches SW 8−4 to specify the address</li>

   <li>Use SW 9 as the Write signal and use KEY 0 as the Clock input</li>

   <li>Using hexadecimal, show the address value on the 7-segment displays HEX5 − 4, show the data being input to the memory on HEX2, and show the data read out of the memory on HEX0.</li>

  </ol></li>

 <li>Test your circuit and make sure that data can be stored into the memory at various locations. Demonstrate the circuit to your TA.</li>

</ol>

<strong>Task 3: </strong>

Instead of creating a memory module by using the IP Catalog, we can implement the required memory by specifying its structure in SystemVerilog code. In a SystemVerilog-specified design it is possible to define the memory as a multidimensional array. A 32 x 4 array, which has 32 words with 4 bits per word, can be declared by the statement

logic [3:0] memory_array [31:0];

In the FPGAs, such an array can be implemented either by using the flip-flops that each logic element contains or, more efficiently, by using the built-in memory blocks.

Perform the following steps:

<ol>

 <li>Create a new Quartus project.</li>

 <li>Write a SystemVerilog file that provides the necessary functionality, including the ability to load the RAM and read its contents as was done in task 2.</li>

 <li>Assign the pins on the FPGA to connect to the switches and the 7-segment displays.</li>

 <li>Compile the circuit and download it into the DE1_SoC.</li>

 <li>Test the functionality of your design by applying some inputs and observing the output. Demonstrate to your TA.</li>

</ol>

<strong>Task 4: </strong>

The RAM block in Figure1 has a single port that provides the address for both read and write operations. For this task you will create a different type of memory module, in which there is one port for supplying the address for a read operation, and a separate port that gives the address for a write operation. Perform the following steps.

<ol>

 <li>Create a new Quartus project for your circuit. To generate the desired memory module open the IP Catalog and select the <em>RAM: 2-PORT</em> module in the <em>Basic Functions &gt; On Chip Memory</em> Choose “<em>With one read port and one write port</em>” in the category called “<em>How will you be using the dual port ram?</em>”

  <ol start="2">

   <li>Configure the memory size, clocking method, and registered ports the same way as in task 2.</li>

   <li>Select <em>I do not care (The outputs will be undefined)</em> for <em>Mixed Port Read-During-Write for Single Input Clock RAM</em>. This setting specifies that it does not matter whether the memory outputs the new data being written, or the old data previously stored, in the case that the write and read addresses are the same during a write operation.</li>

   <li>On the following configuration window, choose the setting <em>Yes, use this file for the memory content data</em>, and specify the filename <em>mif</em>. This configuration window shows how the memory words can be initialized to specific values. It makes use of a feature that allows the memory module to be loaded with data when the circuit is programmed into the FPGA chip.</li>

   <li>An example of a MIF file is provided in Figure 2. You can also learn about the format of a memory initialization file (MIF) by using the Quartus Help. You will need to create a MIF file like the one in Figure 2 to test your circuit.</li>

   <li>Finish the Wizard and then examine the generated memory module in the file ram32x4.sv.</li>

  </ol></li>

</ol>

DEPTH = 32;

WIDTH = 4;

ADDRESS_RADIX = HEX;

DATA_RADIX = BIN;

CONTENT

BEGIN

<ul>

 <li>: 0000;</li>

 <li>: 0001;</li>

 <li>: 0010;</li>

 <li>: 0011;</li>

</ul>

… (some lines not shown)

1E : 1110;

1F : 1111;

END;




Figure 2: An example memory initialization file (MIF).

<ol start="2">

 <li>Write a SystemVerilog file that instantiates your dual-port memory.

  <ol>

   <li>To see the RAM contents, add to your design a capability to display the content of each four-bit word (in hexadecimal format) on the 7-segment display HEX0</li>

   <li>Use a counter as a read address and scroll through the memory locations by displaying each word for about one second. As each word is being displayed, show its address (in hex format) on the 7-segment displays HEX3−2</li>

   <li>Use the 50 MHz clock, CLOCK_50, and use KEY 0 as a reset input</li>

   <li>For the write address and corresponding data use switches SW 8−4 and SW 3−0</li>

   <li>Show the write address on HEX5−4 and show the write data on HEX1.</li>

   <li>Make sure that you properly synchronize the slide switch inputs to the 50 MHz clock signal.</li>

  </ol></li>

 <li>Test your circuit and verify that the initial contents of the memory match your ram32x4.mif file. Make sure that you can independently write data to any address by using the slide switches.</li>

 <li>Use the SignalTap II functionality of Quartus to verify the contents of your RAM module. Find the Intel SignalTap II tutorial on canvas (SignalTap.pdf) to get started. For your RAM module, you’ll probably want to probe the read address and data output port.</li>

 <li>Demonstrate to your TA.</li>

</ol>


