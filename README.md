# Sky130-openLANE-workshop

## DAY 1 - Inception of open-source EDA, openLANE and Sky130 PDK

### Design Preparation Step
./flow.tcl -interactive

![pic1](https://user-images.githubusercontent.com/66617592/97317402-c5ee0600-1890-11eb-8dfd-950a8b62abd2.PNG)

package require openlane 0.9

prep -design picorv32a

![pic2](https://user-images.githubusercontent.com/66617592/97317418-cb4b5080-1890-11eb-90e4-366701631175.PNG)

### To run Synthesis and charaterization of synthesis Results
run_synthesis

![pic3](https://user-images.githubusercontent.com/66617592/97317424-cd151400-1890-11eb-9f85-4f1632197723.PNG)

**DAY 1 Output**

![d1](https://user-images.githubusercontent.com/66617592/97317373-bec6f800-1890-11eb-97c8-8f0f97f58d2f.PNG)

## DAY 2 - Good Floorplan vs Bad Floorplan and introdiction to library cells

### Floorplan using open LANE and review layout in Magic

Specifying the VMETAL and HMETAL configurations explicitly in picorv32a/config.tcl file. 

Then : prep -design picorv32a -tag <any folder name> -overwrite
  
Then : run synthesis 

Then : run_floorplan
  
![pic4](https://user-images.githubusercontent.com/66617592/97321040-7578a780-1894-11eb-9026-141c1a61aeb5.PNG)

To view the layout in Magic

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

![d2l22](https://user-images.githubusercontent.com/66617592/97321157-950fd000-1894-11eb-9ec9-061905e82153.PNG)

![d2222](https://user-images.githubusercontent.com/66617592/97321173-97722a00-1894-11eb-99f2-d23c207d0852.PNG)

**Changing the FP IO MODE**

set ::env(FP_IO_MODE)2

run_floorplan

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

![22](https://user-images.githubusercontent.com/66617592/97321813-457dd400-1895-11eb-8b2b-39ef5b778642.PNG)

### Placement

run_placement

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

![pl](https://user-images.githubusercontent.com/66617592/97321781-3e56c600-1895-11eb-93f3-551cfc6129ce.PNG)

![sdf](https://user-images.githubusercontent.com/66617592/97321794-4151b680-1895-11eb-879d-0f3b32f7f2c3.PNG)


## DAY 3 - Design library cell using Magic Layout and ngspice Characterization

VSDSTDCELLDESIGN cloning

**In Tab 1:**

cd ~/work/tools/openlane_working_dir/openlane

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

cd vsdstdcelldesign

**In Tab 2 :**

cd ~/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic

cp sky130A.tech /work/tools/openlane_working_dir/openlane/vsdstdcelldesign

**In Tab 1:** - Lab introduuction to Sky130 basic layers layout and LEF using inverter

magic -T sky130A.tech sky130_inv.mag &

![55](https://user-images.githubusercontent.com/66617592/97337147-e96f7b80-18a5-11eb-960d-1391f5b7148e.PNG)

**Lab Exercise : To create SPICE deck using Sky130 tech and to characterize inverter using sky130 model files**

In the tckon window:

pwd
/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

extract all

ext2spice cthresh 0 rthresh 0

In Tab 1 @ ~/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

Sky130_inv.spice and Sky130_inv.ext files have been created

Edit Sky130_inv.spice to include pmoas and nmos lib files

![cat](https://user-images.githubusercontent.com/66617592/97339247-5b48c480-18a8-11eb-9004-002a5d6e4a70.PNG)

@ ~/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

ngspice sky130_inv.spice

![33](https://user-images.githubusercontent.com/66617592/97339814-0c4f5f00-18a9-11eb-9b57-4b8c58391ef2.PNG)

plot y vs time a
 
![333](https://user-images.githubusercontent.com/66617592/97339904-25581000-18a9-11eb-95ef-4770b241d68b.PNG)
 
![3r](https://user-images.githubusercontent.com/66617592/97339970-399c0d00-18a9-11eb-86b1-f8e8c62a0f8c.PNG)

Conversion of grid info to track info

![grid](https://user-images.githubusercontent.com/66617592/97340355-af07dd80-18a9-11eb-9303-3dca62d85f47.PNG)

**Lab Exercise : Introduction to MAGIC, load sky130 tech rules, fix poly.9 error in sky130 tech file**

Introduction to MAGIC

![1](https://user-images.githubusercontent.com/66617592/97341424-0d818b80-18ab-11eb-806d-82c24836fbc9.PNG)

![5](https://user-images.githubusercontent.com/66617592/97341436-12ded600-18ab-11eb-9e38-04578a23e23b.PNG)

Load Sky130 Tech rules

![7](https://user-images.githubusercontent.com/66617592/97341450-170af380-18ab-11eb-9c4c-d32992699be7.PNG)

Fix poly.9 Error in Sky130 tech file

![poly 9](https://user-images.githubusercontent.com/66617592/97341472-1d00d480-18ab-11eb-8144-b121bd4c50d1.PNG)

Implement polyresistor spacing to diff and tap 

![poly out](https://user-images.githubusercontent.com/66617592/97341489-20945b80-18ab-11eb-8a4b-8639644611d5.PNG)

## DAY 4 - Pre Layout timing Analysis and importance of food clock tree

Lef write file

![less](https://user-images.githubusercontent.com/66617592/97342907-da3ffc00-18ac-11eb-9aab-96b04e730aea.PNG)

Include the below command to include the additional lef into the flow:

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

Then Perform Synthesis

run_synthesis

run_placement

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

Placement completed

![placement](https://user-images.githubusercontent.com/66617592/97342919-ddd38300-18ac-11eb-990a-6df3c35abf3b.PNG)

In the terminal:

echo $::env(SYNTH_STRATEGY)

set ::env(SYNTH_STRATEGY) 1

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING) 

set $::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis

run_placement

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

**Output**

![ddd](https://user-images.githubusercontent.com/66617592/97344329-9b12aa80-18ae-11eb-84ce-6a70e3b6d182.PNG)

![CE](https://user-images.githubusercontent.com/66617592/97344262-87ffda80-18ae-11eb-9b9d-86973d3719d1.PNG)

![ced](https://user-images.githubusercontent.com/66617592/97344276-8b936180-18ae-11eb-97e4-04b7df881c8d.PNG)

![vsddd](https://user-images.githubusercontent.com/66617592/97344303-92ba6f80-18ae-11eb-8016-716a1808c003.PNG)

**Timing Analysis with ideal clocks using openSTA and Clock Tree Synthesis using Trioton CTS and signal integrity**

basesdc values

![mybasedsdc](https://user-images.githubusercontent.com/66617592/97345190-c649c980-18af-11eb-8440-11861a97730c.PNG)

Static Timing Analysis

![sta 2](https://user-images.githubusercontent.com/66617592/97345189-c5b13300-18af-11eb-97a5-1f313eba03a3.PNG)

Optimizing synthesis to reduce setup violations and basic timing ECO

![sta3](https://user-images.githubusercontent.com/66617592/97345186-c5189c80-18af-11eb-8ad5-cd427d428251.PNG)

Clock Tree Synthesis using Trioton CTS

![cts](https://user-images.githubusercontent.com/66617592/97345176-c34ed900-18af-11eb-883c-3716fd229610.PNG)

Verifying CTS run

![ctss](https://user-images.githubusercontent.com/66617592/97345170-c2b64280-18af-11eb-9740-e6908928d235.PNG)

**Timing Analysis with real clocks using openSTA**

Timing Analysis with real clocks using openSTA

![sdcc](https://user-images.githubusercontent.com/66617592/97345198-c8ac2380-18af-11eb-8ea2-a06b423019a5.PNG)

Execute openSTA with right timing libraries and CTS assignment

![ctssss](https://user-images.githubusercontent.com/66617592/97345194-c77af680-18af-11eb-8793-832e7013ea0d.PNG)

![dc](https://user-images.githubusercontent.com/66617592/97345192-c6e26000-18af-11eb-9bc2-93c33b530558.PNG)

To observe bigger impact CTS buffer on setup and hold timing 

![d4e](https://user-images.githubusercontent.com/66617592/97345235-d5c91280-18af-11eb-91f5-96f0dcb245b8.PNG)

## DAY 4 - Final steps for RTL2GDS using tritonRoute and openSTA

Building power Distribution Network

![d51](https://user-images.githubusercontent.com/66617592/97345232-d497e580-18af-11eb-96e8-63c4bdbae493.PNG)

From Power Straps to std cell power

![gen_pdn](https://user-images.githubusercontent.com/66617592/97345254-d9f53000-18af-11eb-98bc-871d27440282.PNG)

![pdn](https://user-images.githubusercontent.com/66617592/97345252-d95c9980-18af-11eb-866f-131fa75255eb.PNG)

Routing - Detailed and Global using TriotonRoute

![d55](https://user-images.githubusercontent.com/66617592/97345250-d95c9980-18af-11eb-8f51-3627345075da.PNG)

Routing Completed

![routing](https://user-images.githubusercontent.com/66617592/97345247-d8c40300-18af-11eb-958b-3172e2e8955d.PNG)

Routing information

![rou](https://user-images.githubusercontent.com/66617592/97345246-d82b6c80-18af-11eb-85a3-1df76b71bf4b.PNG)

![rrrrrrrr](https://user-images.githubusercontent.com/66617592/97345245-d82b6c80-18af-11eb-8448-d2f3dda47a2c.PNG)

Final files list post route

![last](https://user-images.githubusercontent.com/66617592/97345243-d792d600-18af-11eb-93d3-29c5af0409bc.PNG)



# ACKNOWLEDGEMENTS

1. Nickson Jose : https://github.com/nickson-jose/vsdstdcelldesign.git
2. Kunal ghosh 
3. Ahmed Ghazy : https://github.com/efabless/openlane.git








































