# Migrating from sg13g2 to sg13cmos5l

## Overview

The tapeout target PDK has changed from ihp-sg13g2 to ihp-sg13cmos5l.

sg13cmos5l is essentially a subset of sg13g2 with:

- 5 metal layers instead of 7
- a reduced set of devices

For digital designs, this change should not affect the functionality of your design, and the migration should be relatively straightforward.

## 1. Update Librelane

Update librelane to the latest version of the `dev` branch:
```
cd librelane
git checkout dev
git pull
```

## 2. Update the PDK

Clone the new PDK inside the `pdk` folder, as explained in the [README](https://github.com/IHP-GmbH/ihp-sg13cmos5l/blob/main/README.md):
```
cd pdk
git clone https://github.com/IHP-GmbH/ihp-sg13cmos5l.git
```

Afterwards, the `pdk` directory should look like this:
```
📁 pdk
├── 📁 ihp-sg13g2
├── 📁 ihp-sg13cmos5l
.
.
.
```

## 3. Update the PDK reference in your design

Update the `PDK` variable in your `Makefile` (`PDK_ROOT` stays the same):
```diff
-PDK = ihp-sg13g2
+PDK = ihp-sg13cmos5l

.PHONY: librelane
librelane:
	PDK_ROOT=$(PDK_ROOT) PDK=$(PDK) librelane --manual-pdk --run-tag $(TAG) --overwrite config.yaml
	rm -rf final
	cp -r runs/$(TAG)/final final
```

## 4. Update the Bondpads

Copy the `bondpad_5l` folder into your design directory, alongside the old one:
```
📁 your_design_dir
├── 📁 bondpad
├── 📁 bondpad_5l    <-- New bondpads
├── 📄 Makefile
├── 📄 config.yaml
.
.
.
```

Then update the paths in your `config.yaml`:
```diff
-EXTRA_GDS: dir::bondpad/bondpad_70x70.gds
-EXTRA_LEFS: dir::bondpad/bondpad_70x70.lef
+PAD_BONDPAD_NAME: bondpad_70x70_5L
+EXTRA_GDS: dir::bondpad_5l/gds/bondpad_70x70_5L.gds
+EXTRA_LEFS: dir::bondpad_5l/lef/bondpad_70x70_5L.lef

 IGNORE_DISCONNECTED_MODULES:
-  - bondpad_70x70_5
+  - bondpad_70x70_5L
```

## 5. Update the PAD names

The PADs are PDK-specific macros. Fortunately, only the prefix changes: `sg13g2_IOPAD*` → `sg13cmos5l_IOPAD*`.

In your `top.v`, update the pad instantiations accordingly:
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