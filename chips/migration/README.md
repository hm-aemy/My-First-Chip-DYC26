# Migrating from sg13g2 to sg13cmos5l

## Overview

The tapeout target PDK has changed from ihp-sg13g2 to ihp-sg13cmos5l. 

sg13cmos5l is essentially a subset of sg13g2 with:

- 5 metal layers instead of 7
- a reduced set of devices

For digital designs, this change should not affect the functionality of your design, and the migration should be relatively straightforward.

This guide explains how to migrate your design environment to the new technology.

## 1. Update Librelane

Update librelane to the last version of the branch ```dev```. 

```
cd librelane
git checkout dev
```

This branch of the repo has the librelane updated. 

## 2. Update the PDK

Clone the new PDK folder inside the ```IHP-Open-PDK``` folder. Like it is explain on this [REAME](https://github.com/IHP-GmbH/ihp-sg13cmos5l/blob/main/README.md). In the case of this repo you have to do this inside the ```pdk``` folder. like this:

```
cd pdk
git clone https://github.com/IHP-GmbH/ihp-sg13cmos5l.git
```

Then the pdk directory is going to look like this:

```
📁 pdk
├── 📁 ihp-sg13g2
├── 📁 ihp-sg13cmos5l
.
.
.
```

## 3. Update the PDK reference in your design

Update the PDK variable on you ```Makefile``` (the ```PDK_ROOT``` stays the same):

```diff
.
.
.
-PDK = ihp-sg13g2
+PDK = ihp-sg13cmos5l <- point to new PDK

.PHONY: librelane
librelane:
	PDK_ROOT=$(PDK_ROOT) PDK=$(PDK) librelane --manual-pdk --run-tag $(TAG) --overwrite config.yaml
	rm -rf final
	cp -r runs/$(TAG)/final final
```

## 4. Modify the Bondpads

Copy the ```bondpad_5l``` on this folder into you desing, you can put it besides the old one, like:

```
📁 you_design_dir
├── 📁 bondpad
├── 📁 bondpad_5l <-- New bondpads
├── 📄 Makefile
├── 📄 config.yaml
.
.
.
```

The modify the paths on your ```config.yaml```:

```diff
-EXTRA_GDS: dir::bondpad/bondpad_70x70.gds
-EXTRA_LEFS: dir::bondpad/bondpad_70x70.lef
# Override bondpad name
+PAD_BONDPAD_NAME: bondpad_70x70_5L
# Change Bondpad Paths
+EXTRA_GDS: dir::bondpad_5l/gds/bondpad_70x70_5L.gds
+EXTRA_LEFS: dir::bondpad_5l/lef/bondpad_70x70_5L.lef

IGNORE_DISCONNECTED_MODULES:
-  - bondpad_70x70_5
+  - bondpad_70x70_5L

```

## 5. Modify the PADs names

The PADs also changed since this are specific macros for specific PDKs. Fortunetly, it only changes the prefix from ```sg13g2_IOPAD* ->sg13cmos5l_IOPAD*```. 

On you ```top.v``` file change the names of the pads like so:

```diff
generate
for (genvar i=0; i<1; i++) begin : iovdd_pads
    (* keep *)
-    sg13g2_IOPadIOVdd iovdd_pad  (
+    sg13cmos5l_IOPadIOVdd iovdd_pad  (
        `ifdef USE_POWER_PINS
        .iovdd  (IOVDD),
        .iovss  (IOVSS),
        .vdd    (VDD),
        .vss    (VSS)
        `endif
    );
end
```

## 6. Image on top

The new PDK has only 5 layers:

1. Metal1
2. Metal2
3. Metal3
4. Metal4
5. TopMetal1

The last layer is not ```TopMetal2``` anymore, so we need to modify the top image integration:

