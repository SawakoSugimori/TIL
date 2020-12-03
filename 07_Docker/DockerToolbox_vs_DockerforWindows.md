#Differences between Docker Toolbox and Docker for Windows

##参考・引用
  - https://fumidzuki.com/knowledge/1033/
  
##DockerをWindowsで使う方法
1. Docker Toolbox
2. Docker Desktop for Windows

##Docker Toolbox
  - 古い
  - Oracle VM VirtualBoxという仮想化システムを使う

##Docker Desktop for Windows
  - Hyper-Vという仮想化システムを使う
  - 新しいが、使うためには以下のシステム要件を満たす必要がある。
    - Windows 10（64bit）でかつ、エディションがPro、Enterprise、またはEducation（ビルド15063以降）である
    - BIOSで仮想化が有効になっている
    - CPUがSLAT（Second Level Address Translation）に対応している
    - 4GB以上のRAMが搭載されている

##自分のPCについて
- Windows 10 Homeだったので、Docker Toolboxをインストールした
