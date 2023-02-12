# Ryzen-5800x3d-linux-undervolting
A Python script to use Ryzen SMU for Linux PBO tuning of Ryzen CPUs. Mostly needed for Ryzen 5800x3d undervolting.
# What is it ?
This is a linux implementation of the PBO2 undevolting tool used to undervolt Ryzen CPUs in Windows. More info on how to do it in Windows is here: https://github.com/PrimeO7/How-to-undervolt-AMD-RYZEN-5800X3D-Guide-with-PBO2-Tuner
# How to use it ?
1. Clone this repository: https://github.com/leogx9r/ryzen_smu and install the Ryzen SMU driver that makes comunication with Ryzen SMU (System Management Unit) possible
```pwsh
git clone https://github.com/leogx9r/ryzen_smu
cd ryzen_smu
make dkms-install
```
Now make a reboot. The dkms-install should make a new module into you system called "ryzen_smu". It will autostart next time you reboot your system. Without it the provided Python script will not function.
2. When you have the driver installed and functioning clone this repository and start the undervolting tool.
```pwsh
git clone https://github.com/svenlange2/Ryzen-5800x3d-linux-undervolting.git
cd Ryzen-5800x3d-linux-undervolting
Python3 ruv.py
```
You should see this output:
```pwsh
usage: ruv.py [-h] [-l] [-o OFFSET] [-c CORECOUNT] [-r] [-f]

PBO undervolt for Ryzen processors

optional arguments:
  -h, --help            show this help message and exit
  -l, --list            List curve offsets
  -o OFFSET, --offset OFFSET
                        Set curve offset
  -c CORECOUNT, --corecount CORECOUNT
                        Set offset to cores [0..corecount]
  -r, --reset           Reset offsets to 0
  -f, --force           Force positive offset if you really want to risk burning your CPU.
```
The tool gives yu ability to see and write the PBO curve offsets on the fly. The corecount enables writng the offset to all the hardware cpus cores. On 5800x3d you have 8 cores (other 8 are virtual hyperthreading counterparts). So in my case Ill use it like this:
```pwsh
python3 ruv.py -c 8 -o -30
Core 0 set to: -30 readback:-30
Core 1 set to: -30 readback:-30
Core 2 set to: -30 readback:-30
Core 3 set to: -30 readback:-30
Core 4 set to: -30 readback:-30
Core 5 set to: -30 readback:-30
Core 6 set to: -30 readback:-30
Core 7 set to: -30 readback:-30
```
The "readback" in the response indicates what value was stored in the registers. I found out that if ill try to push it firther than -30 it will always read back -30 so there is a hardware limit to the number.
The force option lets apply positive offsets. I dont know how safe these are and I dont recommend experimenting with this.
