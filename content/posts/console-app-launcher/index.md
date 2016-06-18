+++
date = "2016-06-17T15:40:57+09:00"
title = "콘솔 프로그램 실행기"
subtitle = "C# Winform을 이용해 파라미터 입력이 가능한 실행기를 만들자"
tags = ["C# winform", "C++a", "?"]
+++

영상처리 과목의 과제를 할 때였다. 과목 특성상, 처리 알고리즘의 파라미터를 다양하게 변화시켜 그에 따른 여러 결과를 보여야 했다.  
Yuv 동영상의 빠른 처리를 위해 C++ 콘솔 프로그램으로 작성했는데, 윈도우즈에서 파라미터를 변화시켜가며 실행하려니 여간 귀찮은게 아니었다. 특히 프로그램의 실행이 끝나면 무의식적으로 cmd 창을 종료시키는 버릇 때문에, cmd 창을 켜고 실행 파일 위치까지 이동해서 다시 파라미터를 써넣는 과정이 너무 귀찮았다.  
Batch 파일을 작성하면 되지 않을까? 하지만 작성하는 방법을 제대로 알지 못했고, 입력 파라미터가 정해진게 아니어서 미리 작업를 예약하는 방식은 쓸 수 없었다.  
영상처리 프로그램 자체를 GUI로 작성하면? 닭 잡는데 소 잡는 칼 쓸 필요가 있을까. 무엇보다 GUI 하면 C# Winform 밖에 사용할 줄 모르는데, C++ 콘솔 프로그램과 병합하는 방법을 모른다.  
그래서, 간단히 콘솔 프로그램의 위치를 기억하고 파라미터를 입력할 수 있는 실행기를 작성하기로 했다.

# Result
![결과 이미지](#)

# Objectives
* 콘솔 프로그램에 파라미터를 입력할 수 있다.
* 콘솔 프로그램의 위치와 입력했던 파라미터를 기억한다.

## 파라미터 입력 및 실행
내가 원했던 것은, 콘솔 프로그램에 파라미터를 입력하고 실행하면 그 실행 내용과 결과가 cmd 창에 출력되는 모습이었는데, `System.Diagnotics.Process`로 콘솔 프로그램을 바로 실행시켰더니 프로그램이 종료되면 바로 창이 닫혀서 결과를 확인할 수 없었다.   
그래서, cmd부터 실행시키고 cmd에서 콘솔 프로그램을 실행시키는 방향으로 구현해야 했는데, 이는 cmd의 파라미터로 콘솔 프로그램과 그 파라미터를 넘기는 방식을 이용해 해결할 수 있었다.

[cmd의 옵션](https://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/cmd.mspx?mfr=true)을 확인해 보니, cmd를 이용해 다른 프로그램을 실행시키려면 `/c` 또는 `/k` 옵션을 이용하면 된다.

> /c : Carries out the command specified by string and then stops.  
/k : Carries out the command specified by string and continues.

`/c`는 프로그램 실행 후 cmd 종료, `/k`는 프로그램 실행 후 cmd 계속.

둘 중 무슨 옵션을 이용해야 할까? 바로 창이 닫히는 문제를 해결하기 위해 cmd를 사용하므로 `/k` 옵션을 쓰는게 좋을까? 그러나 `/k` 옵션을 이용하면 결과 확인 후 cmd 창을 수작업으로 종료하는 단계가 하나 더 생기게 된다. 이걸 원했던거 아니냐고? 결과를 확인하는게 목적이므로 프로그램 종료는 손 쉽게 할 수 있으면 좋겠다는 거다.

결국 `/c` 옵션을 이용해야 하는데, 결과를 확인하고 싶으니 프로그램이 끝나면 '계속하려면 아무키나 누르세요'를 띄우자. `pause`를 이용하면 되는데, 한 번의 명령어로 `pause`까지 실행시켜야 하니 `&`를 이용하면 된다.

명령어는 다음과 같을 것이다.
``` nohighlight
# cmd.exe /c myapp.exe -myparam & pause
```

이를 C# Winform에서 실행해 보자.
``` C#
// using System.Diagnotics;
// appPath: 프로그램 경로. ex) "c:\myapp.exe"
// tb_parameters: 파라미터가 입력된 Textbox.

Process app = new Process();
app.StartInfo.FileName = "cmd.exe";
app.StartInfo.Arguments = "/C " + appPath + " " + tb_parameters.Text + "& pause";
app.Start();
```

이때, `Process`의 작업 디렉토리를 설정해주지 않으면 콘솔 프로그램 **실행기의 위치**가 작업 디렉토리가 되므로, `System.IO.Path.GetDirectoryName()`으로 콘솔 프로그램의 위치를 가져와 작업 디렉토리로 설정해주자.  

한 줄만 추가해 주면 된다.
``` C#
// using System.IO;

...
app.StartInfo.Arguments = "/C " + appPath + " " + tb_parameters.Text + "& pause";
app.StartInfo.WorkingDirectory = Path.GetDirectoryName(appPath);
app.Start();
```

## 콘솔 프로그램 위치와 파라미터의 기억
실행했던 콘솔 프로그램의 위치와 파라미터의 기억은 레지스트리를 이용한다.

레지스트리의 저장 위치는 `LOCAL_MACHINE`과 `CURRENT_USER`의 두 가지 선택지가 존재하는데, `LOCAL_MACHINE`은 컴퓨터 전역 / 모든 사용자에게 적용되는 필드로 저장할 때 관리자 권한을 요구한다. `CURRENT_USER`는 사용자 계정에 종속되는 필드로 프로그램의 환경 설정을 저장할 때 일반적으로 사용된다.  
(이 둘의 자세한 차이는 [여기](http://www.differencebetween.net/technology/hardware-technology/difference-between-hkey_current_user-and-hkey_local_machine/)를 참조)   
콘솔 프로그램 실행기는 특별한 정보를 저장하는게 아니니 `CURRENT_USER` 필드를 이용한다.

레지스트리를 가져올 때는 `Load` 이벤트를, 기록할 때는 `FormClosed` 이벤트를 이용했다.

``` C#
// using Microsoft.Win32;
// Form_Load()

RegistryKey subkey = Registry.CurrentUser.OpenSubKey("Software\\AppExecutor");
if (subkey != null)
{
    appPath = subkey.GetValue("AppPath").ToString();
    tb_appname.Text = Path.GetFileName(appPath);
    string parameters = subkey.GetValue("Parameters").ToString();
    if (parameters != "")
    {
        tb_parameters.Text = parameters;
    }
}
```

``` C#
// using Microsoft.Win32;
// Form_FormClosed()

if (appPath == "")
{
    Registry.CurrentUser.DeleteSubKey("Software\\AppExecutor", false);
}
else
{
    RegistryKey subkey = Registry.CurrentUser.OpenSubKey("Software\\AppExecutor", true);
    if (subkey == null)
    {
        subkey = Registry.CurrentUser.CreateSubKey("Software\\AppExecutor");
    }
    subkey.SetValue("AppPath", appPath, RegistryValueKind.String);
    if (isParametersEmpty)
    {
        subkey.SetValue("Parameters", "", RegistryValueKind.String);
    }
    else
    {
        subkey.SetValue("Parameters", tb_parameters.Text, RegistryValueKind.String);
    }
}
```