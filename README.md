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

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

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

## DAY 3 - Pre Layout timing Analysis and importance of food clock tree

Lef write file

![less](https://user-images.githubusercontent.com/66617592/97342907-da3ffc00-18ac-11eb-9aab-96b04e730aea.PNG)

Placement completed

![placement](https://user-images.githubusercontent.com/66617592/97342919-ddd38300-18ac-11eb-990a-6df3c35abf3b.PNG)
