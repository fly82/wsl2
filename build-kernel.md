First you need to get the kernel source code used by Microsoft. You can get it from their github page with this command:
```
git clone --branch linux-msft-wsl-5.15.y --depth 1 https://github.com/microsoft/WSL2-Linux-Kernel.git
```
Then inside the WSL2-Linux-Kernel directory you just downloaded, you need to copy the current kernel configuration with:
```
zcat /proc/config.gz > .config
```
Use your favourite text editor and open the .config file you just copied.
Now you can compile the kernel with the right flag enabled. Use:
```
make -j $(nproc)
```
$(nproc) is the number of threads your CPU can handle. You can edit this value if you want to use fewer threads for some reason. In my case this value is 12, the max amount provided by my CPU, but you can leave it as is and let the command identify it on its own.
It will take a while to finish, so be patient.

Next you have to copy the newly compiled kernel image to the Windows side. You can copy it using this:
```
cp arch/x86_64/boot/bzImage /mnt/c/Users/<user>/bzImage
```
Next you have to close WSL and shut it down from a cmd prompt with:
```
wsl --shutdown
```
Tell WSL to use the new kernel by adding a configuration file:

In the folder /mnt/c/Users/<user>/ or C:\Users\<user>\ there should be a .wslconfig file with the content:
```
[wsl2]
kernel=C:\\Users\\<user>\\bzImage
```
