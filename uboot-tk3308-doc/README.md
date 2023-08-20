# U-boot build instructions

U-boot tag: uboot v2022.04

- Prepare idbloader.img

```bash
make rock-pi-s-rk3308_defconfig
make -j12 CROSS_COMPILE=aarch64-linux-gnu- all
./tools/mkimage -n rk3308 -T rksd -d /home/ubuntu/bins/rk3308_ddr_589MHz_uart0_m0_v2.06.bin idbloader.img 
cat /home/ubuntu/bins/rk3308_miniloader_v1.39.bin >> idbloader.img 
cp idbloader.img /home/ubuntu/rockpis/package/boot/uboot-rockchip/src/blob/
```


- Prepare uboot.img and trust.img (rockchip/rkbin repo required)

```bash
./tools/loaderimage --pack --uboot /home/ubuntu/u-boot/u-boot.bin uboot.img 0x00600000
cp uboot.img /home/ubuntu/rockpis/package/boot/uboot-rockchip/src/blob/
```

- trust.img is not required to generate every time
```bash
./tools/trust_merger RKTRUST/RK3308TRUST.ini 
cp trust.img /home/ubuntu/rockpis/package/boot/uboot-rockchip/src/blob/
```