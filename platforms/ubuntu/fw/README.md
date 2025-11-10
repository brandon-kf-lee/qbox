## Note about qemu_fw_cfg

If it's causing you problems if the Ubuntu image isn't able to run, you may be able to blacklist qemu_fw_cfg.

To edit the kernel command line in fw/Artifacts/ubuntu.dtb, you'll need to:

1. Extract the DTB:
`dtc -I dtb -O dts fw/Artifacts/ubuntu.dtb -o ubuntu.dts`

2. Edit the DTS file. Look for a /chosen node with bootargs:
	```
	chosen {
	    bootargs = "console=ttyS0 earlycon=sbi systemd.unit=multi-user.target audit=off loglevel=8 		root=LABEL=root-riscv64 rootfstype=ext4 rw modprobe.blacklist=qemu_fw_cfg;
	};
	```
	Add `modprobe.blacklist=qemu_fw_cfg` to the end

3. Recompile the DTB:
`dtc -I dts -O dtb ubuntu.dts -o fw/Artifacts/ubuntu.dtb`

