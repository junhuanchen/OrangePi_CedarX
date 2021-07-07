OrangePi CedarX
------------------------------------------------------

* Host Platform

  Ubuntu14.04+/x86_64

* Target Platform
  
  OrangePi PC2 - Ubuntu16.04/aarch64


### Porting Guide

* Download source code
```
git clone https://github.com/OrangePiLibra/OrangePi_Cedarx.git
```

* Prepare toolchain

  Please install toolchain on Host PC
  ```
sudo apt-get install -y gcc-arm-linux-gnueabi
  ```

// arm-cortexa9-linux-gnueabihf-4.9.3-20160512.tar.xz

* Compile cedarx on Host PC

  When we get source code of Cedarx, we should comile first, as follow:
  Note! The "CURRENT_PATH" is path of "OrangePi_CedarX".
  ```
  
cd OrangePi_Cedarx/cedarx
./bootstrap

./configure --host=arm-none-linux-gnueabihf CC=/home/dls/CedarX/4.9.3/bin/arm-none-linux-gnueabihf-gcc CXX=/home/dls/CedarX/4.9.3/bin/arm-none-linux-gnueabihf-g++ CFLAGS="-D__ENABLE_ZLIB__" CPPFLAGS="-D__ENABLE_ZLIB__" LDFLAGS="-lcrypto -lz -L${CURRENT_PATH}/OrangePi_CedarX/cedarx/external/lib32/arm-linux-gnueabihf -L${CURRENT_PATH}/OrangePi_CedarX/libcedarc/library/lib32/gnueabihf_3.10/" --prefix=${CURRENT_PATH}/OrangePi_CedarX/output

./configure --host=arm-linux-gnueabihf CFLAGS="-D__ENABLE_ZLIB__" CPPFLAGS="-D__ENABLE_ZLIB__" LDFLAGS="-lcrypto -lz -L${CURRENT_PATH}/OrangePi_CedarX/cedarx/external/lib32/arm-linux-gnueabihf -L${CURRENT_PATH}/OrangePi_CedarX/libcedarc/library/lib32/gnueabihf_3.10/" --prefix=${CURRENT_PATH}/OrangePi_CedarX/output

make

make install
  ```
  Note! The "CURRENT_PATH" is path of "OrangePi_CedarX"

  Now, we can get executable file on "${CURRENT_PATH}/OrangePi_CedarX/output".

* Update executable file onto OrangePi PC2

  Copy executable file  to OrangePi PC2.
  ```
cp -rfa ${CURRENT_PATH}/OrangePi_CedarX/output ${OrangePiPC2}/rootfs/home/orangepi/CedarX
  ```
  Copy 32bit-library of "arm-linux-gnueabi" to OrangePi PC2.
  ```
cp -rfa ${CURRENT_PATH}/OrangePi_CedarX/lib32/arm-linux-gnueabi ${OrangePiPC2}/rootfs/home/orangepi/CedarX
  ```
  Copy 32bit-library of "linuxgnuebi" to OrangePi PC2.
  ```
cp -rfa ${CURRENT_PATH}/OrangePi_CedarX/libcedarc/library/lib32/linuxgnueabi_3.10/ ${OrangePiPC2}/rootfs/home/orangepi/CedarX
  ```
  Copy specify file to OrangePi PC2.
  ```
cp -rfa ${CURRENT_PATH}/OrangePi_CedarX/cedarx/config/t3_linux_cedarx.conf ${OrangePiPC2}/rootfs/etc/cedarx.conf
cp -rfa ${CURRENT_PATH}/OrangePi_CedarX/cedarx/OrangePi_CedarX.sh ${OrangePiPC2}/rootfs/home/orangepi/CedarX
  ```

* Prepare running envirnoment on OrangePi PC2

  Login OrangePi PC with terminal, and change dirent into "/home/orangepi/CedarX", as follow:

  If it's first time to do, please execute these command first:
  ```
sudo apt-get install -y gcc-arm-linux-gnueabi
sudo ./usr/local/sbin/resize_rootfs.sh
  ```
  After then, running command:
  ```
cd /home/orangepi/CedarX
sudo chmod 755 OrangePi_CedarX.sh
sudo ./OrangePi_CedarX.sh
  ```
  At last, please export dynatic shared library:
  ```
export LD_LIBRARY_PATH=/home/pi/OrangePi_CedarX/cedarx/external/lib32/arm-linux-gnueabihf:/home/pi/OrangePi_CedarX/output/lib:/home/pi/OrangePi_CedarX/cedarx/gnueabihf_3.10:/home/pi/OrangePi_CedarX/libcedarc/library/lib32/gnueabihf_3.10
  ```
export LD_LIBRARY_PATH=~/OrangePi_CedarX/output/lib/:




* Running demo code

  The finally, we can running demo code to test CedarX. 
  Please follow this step:
  ```
  cd /home/orangepi/CedarX/output/bin
  ./xplayerdemo
  ``` 
