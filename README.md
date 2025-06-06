# Table of Contents
- [0.1 Introduction](#01-introduction) 
- [0.2 Description of the Design](#02-description-of-the-design) 
- [0.3 Implementation](#03-implementation)
  - [0.3.1 Unit Cell](#031-unit-cell)
  - [0.3.2 8x8 Unit Cell Array](#032-8x8-unit-cell-array)
  - [0.3.3 Reference Generator](#033-reference-generator)
  - [0.3.4 Decoder](#034-decoder)
  - [0.3.5 Decoder (Additional Circuit)](#035-decoder-additional-circuit)
  - [0.3.6 Final Implementation Of DAC and Pre-Layout Simulation](#036-final-implementation-of-dac-and-Pre-Layout-Simulation)
- [0.4 Parasitic Extraction and Post-Layout Simulation](#04-parasitic-extraction-and-post-layout-simulation)
- [0.5 What is the idea of having the transistors M7 and M8 in the design?](#05-what-is-the-idea-of-having-the-transistors-m7-and-m8-in-the-design)
- [0.6 Reference](#06-reference)

---

## 0.1 Introduction

This project was developed as part of my Master's program in  Integrated Systems and Circuits Design (ISCD) at Carinthia University of Applied Science. It focuses on the design, implementation, and analysis of a 6bit DAC.

The following sections present the project architecture, detailed circuit design, simulation results, and final implementation insights.

 In this lab a 6-bit digital to analog converter is generated. While the input is a digital 6-bit word, the
 analogue output is represented by a variable current. Main parts of the design are:
 - A unit cell which generates either a current of 1 µA or 0 µA
 - A 6bit thermometer decoder

<p align="justify">
The main objectives are to understand the design principles of a DAC, generatea hierarchical schematic, verify the functionality of the design through simulations, draw a compact hierarchical layout that is LVS clean and free of DRC errors, and performparasitic extraction to verify functionality with parasitics. The report provides a detailed explanation of the design process and simulation results. The final layout is verified to be free of any errors and the DAC is found to operate as expected. This project providesvaluable insights into the design of DACs and the challenges involved in designing an efficient and reliable circuit.
</p>

## 0.2 Description of the Design
<p align="justify">
A digital to analogue converter (DAC) converts a binary digital word into an analogue value, either a voltage or a current. In our example, the binary word has a length of 6-bit and the minimum step size of the analogue word, which is also called a LSB, will be 1 µA as we are using a current as the analogue signal. Figure 1 shows the top-level schematic of such a DAC During operation the number of unit cells
  
![Toplevel Schematic of 6bit Digital to Analog Converter](./img/1.png)

**Fig 1:** Toplevel Schematic of 6bit Digital to Analog Converter

which are turned on are equivalent to the number of 1’s at the output of the decoder. Subsequently the output is the sum of all the unit cells which are turned on As this implementation requires a lot of wiring, it can be simplified by arranging the unit cells in a matrix of 8x8 cells and select them by a row- and a column-decoder, each 3bit binary to 7bit thermometer. The output of a 3 to 7 binary to thermometer decoder is according to Table 1.

![3bit Binary to 7bit Thermometer Decoder](./img/) 
**Table 1:** 3bit Binary to 7bit Thermometer Decoder

![Description of the Design](./img/)

which are turned on are equivalent to the number of 1’s at the output of the decoder. Subsequently the output is the sum of all the unit cells which are turned on As this implementation requires a lot of wiring, it can be simplified by arranging the unit cells in a matrix of 8x8 cells and select them by a row- and a column-decoder, each 3bit binary to 7bit thermometer. The output of a 3 to 7 binary to thermometer decoder is according to table 1.

![Description of the Design](./img/)


Figure 3 shows the new top-level design with the 8x8 matrix of unit cells and the two decoders.The operation is as following: with the column decoder the first 7bits of a the first row are individually selected. For bit 8 the complete row will be selected by the row decoder, all bits of the column decoder are set to 0 again. This procedure is repeated until all 7 rows are selected. The remaining 7bits are again individually selected by the column decoder.As this scheme uses only 63 of the 64 cells, the remaining cell can be used as a reference diode for the current mirror. Implementation will be shown later in the
description of the circuits.

![ Modified Schematic of 6bit Digital to Analog Converter](./img/)
**Fig 2:** Modified Schematic of 6bit Digital to Analog Converter

![Description of the Design](./img/)

</p>




## 0.3 Implementation
The schematic,symbol and layout for the following modules has been created separately and made LVS and DRC clean.
 - Unit Cell
 - 8x8 Unit cell array
 - Reference Generator
 - Column Decoder
 - Row Decoder
 - Decoder (Additional Circuit)




### 0.3.1 Unit Cell

<p allign="justify">
The basic structure of the unit cell can be seen in figure 6. A unit cell in this design is to generate 1 µA current.The Unit cell symbol is seen in figure 4. The dimensions of the individual transistors can be found in table 2. While the bulk of all nmos transistors should be connected to ground, bulk of pmos transistors has to be connected to vdd!

### Tset Bench and Simulation
Initially, the functionality of the unit cell schematic is verified using the current reference generator with the setup shown in Figure 7 below. The functionality verification of the unit cell is performed using transient analysis by checking the current (Iout) mirrored in the unit cell when the on, selRow, and selCell Signals, kept as (011) respectively and observed that it has generated a current of 1 µA as shown in Figure 8.
- VDC =1.2 V.
- IDC = 2 µA.
  
![Unit Cell](./img/)

**Fig 3:** Unit Cell Symbol

![Parameters of Transistors in Unit Cell](./img/)
**Table 2:** Parameters of Transistors in Unit Cell

![Transistor Level Schematic of Unit Cell](./img/)
**Fig 4:** Transistor Level Schematic of Unit Cell

![Unit Cell Test Bench](./img/)
**Fig 5:** Unit Cell Test Bench

![Transient Response of unit cell](./img/)
**Fig 6:** Transient Response of unit cell

![Unit Cell Test Layout](./img/)
**Fig 7:** Unit Cell Layout



</p>



### 0.3.2 8x8 Unit Cell Array

<p align="justify">
A total of 63 current-mirrored unit cells that are arranged in an 8 by 8 matrix, with each generating a current of 1uA. The symbol, schematic, and layout of the unit cell are shown in Figures numbered 5, 6, and 7 below. The number of unit cells that turn on depends on the decimal equivalent of the 6-bit digital code provided at the input of the row and column decoders. The unit cells have to be matched among themselves and the current reference generator in layout for accurate mirroring of current into the unit cells. The matching is the reason for including the current reference generator in the matrix of unit cells, as shown in Figures numbered 8, 9, and 10.
</p>

![8x8 Unit Cell Array](./img/)

**Fig 8:** Schematic of the 8 by 8 unit cell matrix

![8x8 Unit Cell Array symbol](./img/)
**Fig 9:** Symbol of the 8 by 8 unit cell matrix


### 0.3.3 Reference Generator


<p align ="justfy">
A current reference generator that serves as an input to the 8 by 8 matrix of unit cells. This reference generator is included in the 8 by 8 matrix to mirror the current of the reference generator into the unit cells. The reference current is supplied from an external source to the reference generator. The reference voltages vb and vc (biasing voltage) are generated by a simple circuit like depicted in figure 10.

![Transistor Level Schematic of Reference Generator](./img/)
**Fig 10:** Transistor Level Schematic of Reference Generator

The dimensions of the individual transistors for this design can be found in table 3. The bulk of all transistors should be connected to ground.
Note: By injection of the reference current Iin two voltages vc and vb are generated, which have to be connected to the voltages vc and vb of the Unit Cell. As the connection for Iin and vc are physically the same net, you have to decide for one of those nets as pin and connect the circuit accordingly, which means either you choose vc and inject the current into that node or you choose Iin and connect this pin to vc of the Unit Cell.

![Parameters of Devices in Reference Generator](./img/)
**Table 3:** Parameters of Devices in Reference Generator

Reference generator symbol and layout is shown in figures 11 and 12 respectively.

![Reference Generato symbol](./img/)
**Fig 11:** Reference Generator symbol

![Reference Generator Layout](./img/)
**Fig 12:** Reference Generator Layout
</p>

### 0.3.4 Decoder

<p align="justify">
A 3 to 8 Row Decoder and a 3 to 8 column decoder to select the unit cells in the desired row and column of the matrix, respectively. The digital code is provided as input to the decoders, and the outputs are passed to the unit cell matrix through the decoder logic.The two decoders for row- and column-selection are the same and perform 3-bit binary to-thermometer decoding according to table 1. A possible implementation can be seen in figure 14. Since both decoder are similar only row decoder schematic, symbol and layouts are going to be mentioned.

![Schematic of a 3-bit Binary to Thermometer Decoder(Row/Column decoder)](./img/)
**Fig 13:** Schematic of a 3-bit Binary to Thermometer Decoder(Row/Column decoder)

The logical gates used in this design are according to Table 4.

![Schematic of a 3-bit Binary to Thermometer Decoder(Row/Column decoder)](./img/)
**Table 4:** Schematic of a 3-bit Binary to Thermometer Decoder(Row/Column decoder)

![Layout of a 3-bit Binary to Thermometer Decoder(Row/Column decoder)](./img/)
**Fig 14:** Layout of 3-bit Binary to Thermometer Decoder(Row/Column decoder)
</p>



### 0.3.5 Decoder (Additional Circuit)
 The row- and column-decoder needs an additional decoder circuit which generates the following signals:
 - 8 signals for the active rows
 - 7 signals for selecting a single cell in an active row
 - 7 signals for turning on a complete row
 The decoder circuit is implemented in using iterated instance and connection by name as follow

![Decoder Circuit for Conversion of Row-/Column-Decoder Outputs](./img/)
**Fig 15:**  Decoder Circuit for Conversion of Row-/Column-Decoder Outputs

![Decoder Circuit  Symbol for Conversion of Row-/Column-Decoder Outputs](./img/)
**Fig 16:**  Decoder Circuit for Conversion of Row-/Column-Decoder Outputs

 The dimensions of the individual transistors can be found in Table 5.

![Logical Gates used in the Circuit in Figure 20](./img/)
**Table :5** Logical Gates used in the Circuit in Figure 15

![Layout of Decoder Circuit for Conversion of Row-/Column-Decoder Outputs](./img/)
**Fig 17:**  Layout of Decoder Circuit for Conversion of Row-/Column-Decoder Outputs


### 0.3.6 Final Implementation Of DAC and Pre-Layout Simulation

<p align="justify">
 
The block diagram for the signal flow in the proposed 6-bit DAC is presented in Figure 20 below for further reference.
 
 ![ Block Diagram of the Signal Flow in the DAC](./img/)
 **Fig 17:**   Block Diagram of the Signal Flow in the DAC
 

The layout design was initiated following successful verification of the schematics, with a focus on ad
hering to the existing design hierarchy. At each level of the design hierarchy, LVS and DRC violations
 were rectified. power mesh was routed to provide proper ground and power to all modules. To ensure a
 proper fit within the matrix, a unit cell matrix was created.The layout design is based on the presented
 floorplan, as illustrated in Figure 18.

 ![ Layout floorplan of the DAC](./img/)
 **Fig 18:**  Layout floorplan of the DAC

The functionality verification for the 6-bit DAC is performed using the test bench setup shown in Figure 25.

 ![Test Bench for DAC](./img/)
 **Fig 19:**  Test Bench for DAC

The ADE environment setup for the simulation of 6-bit DAC is shown in Figure 25 for reference.

 ![Transient Analysis setup of the DAC test bench](./img/)
 **Fig 20:** Transient Analysis setup of the DAC test bench
 
To verify the functionality of the 3 to 8 decoder, decoder logic, and the unit cell matrix in the 6-bit DAC test bench setup, with the above test bench setup of 6-bit ADC, the functionality of the 3 to 8 decoder, decoder logic, and the unit cell matrix can also be verified by cross-referencing the digital input data with the obtained current (Iout) at the unit cell matrix output as seen in Figure 24 above. In addition, the signal flow diagram from Figure 20 comes in handy to debug and apply corrections during the functional verification of the DAC and its sub-blocks.
 
 
![DAC-All modules integrated](./img/)
 **Fig 21:** DAC-All modules integrated

To verify the functionality of the 3 to 8 decoder, decoder logic, and the unit cell matrix in the 6-bit DAC test bench setup, it is possible to cross-reference the digital input data with the obtained current (Iout) at the output of the unit cell matrix, as illustrated in Figure 22 and 23 respectively. As we expected 63 µA current is observed at the Iout for the complete 64 6-bit digital input.

![Digital Input of DAC](./img/)
 **Fig 22:** Digital Input of DAC

 Transient response for each inputs of 8x8 matrix unit cell is given in the figure 24.

 ![Pre-Layout Output characteristics of the DAC](./img/)
 **Fig 23:** Pre-Layout Output characteristics of the DAC

 ![Transient Response of decoder logic output signal](./img/)
 **Fig 24:**  Transient Response of decoder logic output signal

  The Final Layout, LVS and DRC results of the 6-bit DAC are as shown in Figures 25,26 and 27 respectively.
  
  ![6-bit DAC layout](./img/)
 **Fig 25:**   6-bit DAC layout

 
  ![6-bit DAC LVS summary](./img/)
 **Fig 26:** 6-bit DAC LVS summary

 
  ![6-bit DAC DRC summary](./img/)
 **Fig 27:** 6-bit DAC DRC summary
 
</p>





## 0.4 Parasitic Extraction and Post-Layout Simulation

<p align="justify">
 Under typical operating conditions, RC and C-only extraction were performed on the DAC layout using Quantus, which extracted the parasitic capacitance and resistance values. This step allows for a more precise analysis of circuit performance and signal integrity as it can identify parasitics that may affect circuit behavior. Accurately extracting and accounting for parasitics in the design can help optimize it for better performance and lower noise. The process of RC and C-only extraction is an essential step in ensuring the quality and reliability of the final design. With the same ADE setup from pre-layout simulations and the inclusion of the newly created calibre views of parasitic extraction in the config view for the 6-bit DAC test bench schematic, the post-layout simulation was performed as shown in Figure 28.

![Config view of the 6-bit DAC](./img/)
 **Fig 28:**  Config view of the 6-bit DAC
  
Later the netlist was generated for the testbench to  check whether the schematic view of the 6-bit DAC was replaced by the extracted file generated from the layout. This is followed by the simulation with the C-Only and RC extracted files. The output response is shown in Figures 29 and 30. 

![Output characteristics of the 6-bit DAC using Conly](./img/)
 **Fig 29:**  Output characteristics of the 6-bit DAC using C only

 ![Output characteristics of the 6-bit DAC using RConly](./img/)
 **Fig 30:**  Output characteristics of the 6-bit DAC using RConly

 The output response for digital input code (111111) marked in the pre-layout and post-layout simu lations results in Figures 28,34, and 35 are compared as shown in Table below.

 ![Comparison of pre-layout and post-layout responses of the DAC](./img/)
 **Fig 31:** Comparison of pre-layout and post-layout responses of the DAC
</p>


## 0.5 What is the idea of having the transistors M7 and M8 in the design?
![Transistors M7 and M8](./img/)

_Explain why M7 and M8 are needed in your design._

---

## 0.6 Reference
![Reference](./img/)

_List your references here._

---
