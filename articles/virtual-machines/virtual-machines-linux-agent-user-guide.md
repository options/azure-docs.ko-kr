---
title: "Linux 에이전트 사용자 가이드 | Microsoft Docs"
description: "Linux 에이전트(waagent)를 설치 및 구성하여 가상 컴퓨터와 Azure 패브릭 컨트롤러의 상호 작용을 관리하는 방법에 대해 알아봅니다."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 43d3ad9e2b50a84cb9fd70582eb03b9d7d45d788


---
# <a name="azure-linux-agent-user-guide"></a>Azure Linux 에이전트 사용자 가이드
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>소개
Microsoft Azure Linux 에이전트(waagent)는 Linux 및 FreeBSD 프로비저닝, Azure 패브릭 컨트롤러와 VM의 상호 작용을 관리합니다. 이 에이전트는 다음과 같은 Linux 및 FreeBSD laaS 배포 기능을 제공합니다.

> [!NOTE]
> 자세한 내용은 Azure Linux 에이전트 [추가 정보](https://github.com/Azure/WALinuxAgent/blob/master/README.md)를 참조하세요.
> 
> 

* **이미지 프로비전**
  
  * 사용자 계정 만들기
  * SSH 인증 유형 구성
  * SSH 공개 키 및 키 쌍 배포
  * 호스트 이름 설정
  * 플랫폼 DNS에 호스트 이름 게시
  * 플랫폼에 SSH 호스트 키 지문 보고
  * 리소스 디스크 관리
  * 리소스 디스크 포맷 및 탑재
  * 스왑 공간 구성
* **네트워킹**
  
  * 플랫폼 DHCP 서버와의 호환성을 개선하기 위한 경로 관리
  * 네트워크 인터페이스 이름의 안정성 확인
* **커널**
  
  * 가상 NUMA 구성(커널에 대해 사용 안 함 <2.6.37)
  * /dev/random의 Hyper-V 엔트로피 이용
  * 루트 장치(원격일 수 있음)에 대한 SCSI 시간 제한 구성
* **진단**
  
  * 직렬 포트로 콘솔 리디렉션
* **SCVMM 배포**
  
  * System Center Virtual Machine Manager 2012 R2 환경에서 실행되는 경우 Linux용 VMM 에이전트 검색 및 부트스트랩
* **VM 확장**
  
  * 소프트웨어 및 구성 자동화를 사용하도록 Microsoft 및 Partner에서 작성된 구성 요소를 Linux VM(IaaS)에 삽입
  *  [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>통신
플랫폼에서 에이전트로의 정보 흐름은 다음 두 채널을 통해 진행됩니다.

* IaaS 배포를 위해 부팅 시 연결된 DVD. 이 DVD에는 실제 SSH 키 쌍이 아닌 모든 프로비전 정보가 포함된 OVF 규격 구성 파일이 포함되어 있습니다.
* 배포 및 토폴로지 구성을 가져오는 데 사용된 REST API를 공개하는 TCP 끝점

## <a name="requirements"></a>요구 사항
다음 시스템은 테스트를 거쳐 Azure Linux 에이전트와 동작하는 것으로 알려져 있습니다.

> [!NOTE]
> [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)에서 설명한 대로 Microsoft Azure 플랫폼에서 지원되는 시스템의 공식 목록에서 이 목록은 달라질 수 있습니다.
> 
> 

* CoreOS
* CentOS 6.3 이상
* Red Hat Enterprise Linux 6.7 이상
* Debian 7.0 이상
* Ubuntu 12.04 이상
* openSUSE 12.3 이상
* SLES 11 SP3 이상
* Oracle Linux 6.4 이상

기타 지원되는 시스템:

* FreeBSD 10 이상(Azure Linux 에이전트 v2.0.10 이상)

Linux 에이전트는 다음과 같은 일부 시스템 패키지가 있어야 제대로 기능을 수행합니다.

* Python 2.6 이상
* OpenSSL 1.0 이상
* OpenSSH 5.3 이상
* 파일 시스템 유틸리티: sfdisk, fdisk, mkfs, parted
* 암호 도구: chpasswd, sudo
* 텍스트 처리 도구: sed, grep
* 네트워크 도구: ip-route
* UDF 파일 시스템 탑재에 대한 커널 지원

## <a name="installation"></a>설치
배포 패키지에서 리포지토리의 RPM 또는 DEB 패키지를 사용한 설치는 선호하는 Azure Linux Azure 설치 및 업그레이드 방법입니다. 모든 [인증 배포 공급자](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)는 이미지 및 리포지토리에 Azure Linux 에이전트 패키지를 통합합니다.

원본에서 설치 또는 사용자 지정 위치 또는 접두사로의 설치와 같은 고급 설치 옵션에 대한 자세한 내용은 [Github의 Azure Linux 에이전트 리포지토리](https://github.com/Azure/WALinuxAgent).

## <a name="command-line-options"></a>명령줄 옵션
### <a name="flags"></a>플래그
* verbose: 지정한 명령의 세부 정보 표시를 늘립니다.
* force: 일부 명령의 대화형 확인을 건너뜁니다.

### <a name="commands"></a>명령
* help: 지원되는 명령 및 플래그를 나열합니다.
* deprovision: 시스템을 정리하여 다시 프로비전하기에 적합하도록 만듭니다. 이 작업은 다음을 삭제합니다.
  
  * 모든 SSH 호스트 키(구성 파일에서 Provisioning.RegenerateSshHostKeyPair가 'y'인 경우)
  * /etc/resolv.conf의 Nameserver 구성
  * /etc/shadow의 루트 암호(구성 파일에서 Provisioning.DeleteRootPassword가 'y'인 경우)
  * 캐시된 DHCP 클라이언트 임대
  * 호스트 이름을 localhost.localdomain으로 다시 설정

> [!WARNING]
> Deprovision 명령을 사용한다고 해서 이미지에서 중요한 정보를 모두 지우고 다시 배포하기에 적합하게 만든다고 보증할 수는 없습니다.
> 
> 

* deprovision+user: -deprovision(위 참조) 명령의 모든 작업을 수행하고 마지막으로 프로비전한 사용자 계정(/var/lib/waagent에서 가져온 계정) 및 연결된 데이터를 삭제합니다. Azure에서 이전에 프로비전한 이미지의 프로비전을 해제하여 이미지를 캡처하고 다시 사용할 수 있도록 하는 경우에 이 매개 변수를 사용합니다.
* version: waagent의 버전을 표시합니다.
* serialconsole: ttyS0(첫 번째 직렬 포트)을 부팅 콘솔로 표시하도록 GRUB를 구성합니다. 이 매개 변수는 커널 부팅 로그를 직렬 포트로 보내고 디버깅에 사용할 수 있도록 설정합니다.
* daemon: waagent를 디먼으로 실행하여 플랫폼 조작을 관리합니다. 이 인수는 waagent init 스크립트에서 waagent에 지정됩니다.
* start: waagent를 백그라운드 프로세스로 실행

## <a name="configuration"></a>구성
구성 파일(/etc/waagent.conf)은 waagent의 동작을 제어합니다. 다음은 샘플 구성 파일입니다.

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

다양한 구성 옵션이 아래에 자세히 설명되어 있습니다. 구성 옵션으로 부울, 문자열 또는 정수의 세 가지 형식이 있습니다. 부울 구성 옵션은 "y" 또는 "n"으로 지정할 수 있습니다. 특수 키워드 "None"은 아래에 자세히 설명된 대로 일부 문자열 형식 구성 항목에 사용할 수 있습니다.

**Provisioning.Enabled:**  
형식: Boolean  
기본값: y

이 옵션을 통해 사용자가 에이전트의 프로비전 기능을 사용하거나 사용하지 않도록 설정할 수 있습니다. 유효한 값은 "y" 또는 "n"입니다. 프로비전을 사용하지 않도록 설정한 경우 이미지의 SSH 호스트 및 사용자 키는 유지되며 Azure 프로비전 API에서 지정한 모든 구성은 무시됩니다.

> [!NOTE]
> 프로비전에 cloud-init를 사용하는 Ubuntu 클라우드 이미지에서 `Provisioning.Enabled` 매개 변수의 기본값은 "n"입니다.
> 
> 

**Provisioning.DeleteRootPassword:**  
형식: Boolean  
기본값: n

설정한 경우 /etc/shadow 파일의 루트 암호가 프로비전 프로세스 중 삭제됩니다.

**Provisioning.RegenerateSshHostKeyPair:**  
형식: Boolean  
기본값: y

설정한 경우 모든 SSH 호스트 키 쌍(ecdsa, dsa 및 rsa)이 프로비전 프로세스 중 /etc/ssh/에서 삭제됩니다. 그리고 새로운 단일 키 쌍이 생성됩니다.

새로운 키 쌍의 암호화 종류는 Provisioning.SshHostKeyPairType 항목으로 구성할 수 있습니다. 일부 배포에서는 SSH 디먼이 다시 시작될 때(예: 다시 부팅될 때) 누락된 모든 암호화 종류에 대해 SSH 키 쌍을 다시 만듭니다.

**Provisioning.SshHostKeyPairType:**  
형식: String  
기본값: rsa

이 옵션은 가상 컴퓨터의 SSH 디먼에서 지원하는 암호화 알고리즘 형식으로 설정할 수 있습니다. 일반적으로 지원되는 값은 "rsa", "dsa" 및 "ecdsa"입니다. Windows의 "putty.exe"는 "ecdsa"를 지원하지 않습니다. 따라서 Windows에서 putty.exe를 사용하여 Linux 배포에 연결하려는 경우 "rsa" 또는 "dsa"를 사용하세요.

**Provisioning.MonitorHostName:**  
형식: Boolean  
기본값: y

설정한 경우 waagent가 Linux 가상 컴퓨터에서 호스트 이름("hostname" 명령에서 반환하는 이름)의 변경 여부를 모니터링하고, 이미지의 네트워킹 구성을 자동으로 업데이트하여 변경 내용을 반영합니다. DNS 서버로 이름 변경을 푸시하기 위해 가상 컴퓨터에서 네트워킹이 다시 시작됩니다. 이 때문에 인터넷 연결이 잠시 끊어집니다.

**Provisioning.DecodeCustomData**  
형식: Boolean  
기본값: n

설정되면 waagent는 Base64에서 CustomData를 디코딩합니다.

**Provisioning.ExecuteCustomData**  
형식: Boolean  
기본값: n

설정되면 waagent는 프로비전 후에 CustomData를 실행합니다.

**Provisioning.PasswordCryptId**  
형식:String  
기본값: 6

암호 해시를 생성할 때 암호화에 사용되는 알고리즘입니다.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
형식:String  
기본값: 10

암호 해시를 생성할 때 사용되는 임의 salt의 길이입니다.

**ResourceDisk.Format:**  
형식: Boolean  
기본값: y

설정한 경우 "ResourceDisk.Filesystem"에서 사용자가 요청한 파일 시스템 유형이 "ntfs" 이외의 유형이면 플랫폼에서 제공한 리소스 디스크가 waagent로 포맷되어 탑재됩니다. 단일 Linux 파티션 유형(83)을 디스크에서 사용할 수 있습니다. 이 파티션이 탑재될 수 있는 경우에는 포맷되지 않습니다.

**ResourceDisk.Filesystem:**  
형식: String  
기본값: ext4

이 옵션은 리소스 디스크의 파일 시스템 유형을 지정합니다. 지원되는 값은 Linux 배포에 따라 달라집니다. 문자열이 X이면 mkfs.X가 Linux 이미지에 표시됩니다. 일반적으로 SLES 11 이미지는 'ext3'을 사용해야 합니다. 여기에서 FreeBSD 이미지는 'ufs2'를 사용해야 합니다.

**ResourceDisk.MountPoint:**  
형식: String  
기본값: /mnt/resource 

이 옵션은 리소스 디스크가 탑재되는 경로를 지정합니다. 리소스 디스크는 *임시* 디스크이며 VM의 프로비전을 해제할 때 비워질 수 있습니다.

**ResourceDisk.MountOptions**  
형식: String  
기본값: 없음

mount -o 명령에 전달될 디스크 탑재 옵션을 지정합니다. 쉼표로 구분된 값 목록입니다(예: 'nodev,nosuid'). 자세한 내용은 mount(8)를 참조하세요.

**ResourceDisk.EnableSwap:**  
형식: Boolean  
기본값: n

설정한 경우 스왑 파일(/swapfile)이 리소스 디스크에 만들어져서 시스템 스왑 공간에 추가됩니다.

**ResourceDisk.SwapSizeMB:**  
형식: Integer  
기본값: 0

스왑 파일의 크기(MB)입니다.

**Logs.Verbose:**  
형식: Boolean  
기본값: n

설정한 경우 로그에 대한 세부 정보 표시가 향상됩니다. Waagent가 /var/log/waagent.log에 로깅하고 시스템 logrotate 기능을 활용하여 로그를 순환시킵니다.

**OS.EnableRDMA**  
형식: Boolean  
기본값: n

설정되면 에이전트는 기본 하드웨어의 펌웨어 버전과 일치하는 RDMA 커널 드라이버를 로드한 후 설치하려고 합니다.

**OS.RootDeviceScsiTimeout:**  
형식: Integer  
기본값: 300

이 옵션은 OS 디스크 및 데이터 드라이브에서 SCSI 시간 제한을 초 단위로 구성합니다. 설정하지 않은 경우 시스템 기본값이 사용됩니다.

**OS.OpensslPath:**  
형식: String  
기본값: 없음

이 옵션은 openssl 이진의 대체 경로를 지정하는 데 사용하여 암호화 작업에 사용할 수 있습니다.

**HttpProxy.Host, HttpProxy.Port**  
형식: String  
기본값: 없음

설정되면 에이전트는 이 프록시 서버를 사용하여 인터넷에 액세스합니다. 

## <a name="ubuntu-cloud-images"></a>Ubuntu 클라우드 이미지
Ubuntu 클라우드 이미지는 [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) 을 사용하여 Azure Linux 에이전트에 서 관리되는 여러 구성 작업을 수행할 수 있습니다.  다음과 같은 차이점에 유의하세요.

* **Provisioning.Enabled** 프로비전 작업을 수행하기 위해 cloud-init을 사용하는 Ubuntu 클라우드 이미지에서 기본값은 "n"입니다.
* 다음 구성 매개 변수는 cloud-init을 사용하여 리소스 디스크와 swap 공간을 관리하는 Ubuntu 클라우드 이미지에 적용되지 않습니다.
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**
* 프로비전 중 Ubuntu 클라우드 이미지에서 리소스 디스크 탑재 지점 및 swap 공간을 구성하려면 다음 리소스를 참조하세요.
  
  * [Ubuntu Wiki: Swap 파티션 구성](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Azure 가상 컴퓨터에 사용자 지정 데이터 삽입](virtual-machines-windows-classic-inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)




<!--HONumber=Nov16_HO3-->


