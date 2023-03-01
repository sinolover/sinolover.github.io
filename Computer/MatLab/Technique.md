1.安装

~~~bash
sudo mount -t cifs //10.0.0.66/bd ~/shared -o username='shared',password='12345678'

sudo mount -t auto -o loop /home/steven/shared/Linux/R2018a_glnxa64_dvd1.iso ~/dvd/
sudo /home/haes/Downloads/matlab/dvd/install

sudo umount ~/dvd/

sudo mount -t auto -o loop /home/steven/shared/Linux/R2018a_glnxa64_dvd2.iso ~/dvd/


sudo cp ~/shared/Linux/crack/Crack/libmwlmgrimpl.so /usr/local/MATLAB/R2018a/bin/glnxa64/matlab_startup_plugins/lmgrimpl/

sudo cp ~/shared/Linux/crack/Crack/license_standalone.lic /usr/local/MATLAB/R2018a/licenses/

sudo gedit /usr/share/applications/Matlab2018a.desktop

[Desktop Entry] 
Encoding=UTF-8
Name=Matlab 2018a
Comment=MATLAB
Exec=/usr/local/MATLAB/R2018a/bin/matlab
Icon=/usr/local/MATLAB/R2018a/toolbox/shared/dastudio/resources/MatlabIcon.png
Terminal=true
Type=Application
Categories=Application
~~~
配置

~~~bash
MATLAB="/usr/local/MATLAB/R2018a" (比如说我的安在了$HOME/MATLAB/R2011b下面，所以应该写全到2011b)
export PATH=~/Program:$MATLAB/bin:$PATH
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$MATLAB/bin/glnx86 #这里注意cpu是x86还是a64
export C_INCLUDE_PATH=$C_INCLUDE_PAT:$MATLAB/extern/include
export LIBRARY_PATH=$LIBRARY_PATH:$MATLAB/bin/glnx86

~~~


/usr/local/MATLAB/MATLAB_Runtime/v95/extern/include
/usr/local/MATLAB/R2018a/extern/include
/usr/local/MATLAB/R2018a/extern/bin/glnxa64


"mclmcr","mclmcrrt","libmx.lib","libmat.lib","libcut_proj.lib
