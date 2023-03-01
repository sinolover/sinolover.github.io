<h1>把音频插进EXE文件并且播放</h1>
提交日期:2004-4-24  
 
关键词:资源文件  
## 步骤1）建立一个SOUNDS.RC文件

使用NotePad记事本-象下面：
~~~delphi
#define WAVE WAVEFILE 

SOUND1 WAVE "anysound.wav" 
SOUND2 WAVE "anthersound.wav" 
SOUND3 WAVE "hello.wav" 
~~~

## 步骤2）把它编译到一个RES文件

使用和Delphi一起的BRCC32.EXE程序。使用下面的命令行：

BRCC32.EXE -foSOUND32.RES SOUNDS.RC

你应该以'sound32.res'结束一个文件。


## 步骤3）把它加入你的程序

在DPR文件把它加入{$R*.RES}下面，如下：

{$R SOUND32.RES} 


## 步骤4）把下面的代码加入程序去播放内含的音频
~~~delphi
USES MMSYSTEM 
Procedure PlayResSound(RESName:String;uFlags:Integer); 
var 
hResInfo,hRes:Thandle; 
lpGlob:Pchar; 
Begin
     hResInfo:=FindResource(HInstance,PChar(RESName),MAKEINTRESOURCE('WAVEFILE'));
     if hResInfo = 0 then
     begin
        messagebox(0,'未找到资源。',PChar(RESName),16);
        exit;
     end;
     hRes:=LoadResource(HInstance,hResinfo);
     if hRes = 0 then
     begin
        messagebox(0,'不能装载资源。',PChar(RESName),16);
        exit;
     end;
     lpGlob:=LockResource(hRes);
     if lpGlob=Nil then
     begin
        messagebox(0,'资源损坏。',PChar(RESName),16);
        exit;
     end;
     uFlags:=snd_Memory or uFlags;
     SndPlaySound(lpGlob,uFlags);
     UnlockResource(hRes);
     FreeResource(hRes); 
End; 
~~~

## 步骤5）调用程序，用你在步骤（1）编译的声音文件名。
~~~delphi
PlayResSound('SOUND1',SND_ASYNC) 
Flags are: 
SND_ASYNC = Start playing, and don't wait to return 
SND_SYNC = Start playing, and wait for the sound to finish 
SND_LOOP = Keep looping the sound until another sound is played 
~~~