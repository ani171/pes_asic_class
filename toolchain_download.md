![WhatsApp Image 2023-08-19 at 4 52 04 PM](https://github.com/ani171/pes_asic_class/assets/97838595/74d00fa2-3ffa-444d-a243-6566a2b010a1)
Error found as the gcc wasn't of version 12. To update GCC to version 12

```sudo apt update
sudo apt upgrade
sudo apt install build-essential
sudo apt -y install gcc-12 g++-12
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 12
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
gcc --version; g++ --version
```
After updating the GCC to version 12 and restarting the terminal

![WhatsApp Image 2023-08-19 at 4 58 01 PM](https://github.com/ani171/pes_asic_class/assets/97838595/d6225229-73e7-4d02-8af4-3bd998cf15c8)

