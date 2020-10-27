# Sky130-openLANE-workshop

## DAY 1

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

## DAY 2

### Floorplan using open LANE and review layout in Magic

Specifying the VMETAL and HMETAL configurations explicitly in picorv32a/config.tcl file. 

Then : prep -design picorv32a -tag <any folder name> -overwrite
  
Then : run synthesis 

Then : run_floorplan
  
![pic4](https://user-images.githubusercontent.com/66617592/97321040-7578a780-1894-11eb-9026-141c1a61aeb5.PNG)

To view the layout in Magic

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def   from  ~/results/floorplan directory

![d2l22](https://user-images.githubusercontent.com/66617592/97321157-950fd000-1894-11eb-9ec9-061905e82153.PNG)

![d2222](https://user-images.githubusercontent.com/66617592/97321173-97722a00-1894-11eb-99f2-d23c207d0852.PNG)


