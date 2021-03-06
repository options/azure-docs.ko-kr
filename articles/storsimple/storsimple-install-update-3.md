---
title: "StorSimple 장치에 업데이트 3 설치 | Microsoft Docs"
description: "StorSimple 8000 시리즈 장치에서 StorSimple 8000 시리즈 업데이트 3를 설치하는 방법에 대해 설명합니다."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/05/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: f1e67f591ea32cab814257658d023395f1350773
ms.openlocfilehash: 28f26edc36f0850ba21db3ba501793780e61612c


---
# <a name="install-update-3-on-your-storsimple-device"></a>StorSimple 장치에 업데이트 3 설치

## <a name="overview"></a>개요

이 자습서에서는 Azure 클래식 포털을 통해서와 핫픽스 방법을 사용하여 이전 소프트웨어 버전을 실행하는 StorSimple 장치에 업데이트 3을 설치하는 방법에 대해 설명합니다. 핫픽스 방법은 StorSimple 장치의 DATA 0 이외의 네트워크 인터페이스에 게이트웨이가 구성되어 있고 업데이트 1 이전 소프트웨어 버전에서 업데이트하려는 경우에 사용됩니다.

업데이트 3에는 장치 소프트웨어, LSI 드라이버 및 펌웨어, Storport 및 Spaceport 업데이트가 포함됩니다. 업데이트 2 또는 이전 버전에서 업데이트하는 경우 iSCSI, WMI를 적용해야 하며 경우에 따라 디스크 펌웨어 업데이트도 적용해야 합니다. 장치 소프트웨어, WMI, iSCSI, LSI 드라이버, Spaceport 및 Storport 픽스는 무중단 업데이트이며 Azure 클래식 포털을 통해 적용할 수 있습니다. 디스크 펌웨어 업데이트는 작업 중단 업데이트이며 장치의 Windows PowerShell 인터페이스를 통해서만 적용할 수 있습니다. 

> [!IMPORTANT]
> * 설치하기 전에 일련의 수동 및 자동 전 검사를 수행하며 하드웨어 상태와 네트워크 연결 측면에서 장치 상태를 확인합니다. Azure 클래식 포털에서 업데이트를 적용하는 경우 이러한 사전 검사를 수행합니다.
> * Azure 클래식 포털을 통해 소프트웨어 및 드라이버 업데이트를 설치하는 것이 좋습니다. 포털에서 사전 업데이트 게이트웨이 검사가 실패한 경우 (업데이트를 설치하려면) 장치의 Windows PowerShell 인터페이스로 이동해야 합니다. 업데이트하는 버전에 따라 업데이트 설치에 1.5~2.5시간이 걸릴 수 있습니다. 장치의 Windows PowerShell 인터페이스를 통해 유지 관리 모드 업데이트를 설치해야 합니다. 유지 관리 모드 업데이트는 중단 업데이트입니다. 이는 장치에 중단 시간을 발생시킵니다.
> * 선택적 StorSimple Snapshot Manager를 실행하는 경우 장치를 업데이트하기 전에 Snapshot Manager 버전을 업데이트 2로 업그레이드했는지 확인합니다.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Azure 클래식 포털을 통해 업데이트 3 설치
다음 단계를 수행하여 장치를 [업데이트 3](storsimple-update3-release-notes.md)로 업데이트합니다.

> [!NOTE]
> 업데이트 2 이상(업데이트 2.1 포함)을 적용하는 경우 Microsoft가 장치에서 추가 진단 정보를 가져올 수 있습니다. 따라서 운영 팀에서 문제가 있는 장치를 확인하는 경우, 장치로부터 정보를 수집하고 문제를 진단할 준비가 더욱 잘 갖추어져 있습니다. 업데이트 2 이상을 수락하면, 이러한 주도적 지원을 제공해도 된다고 허락하는 것입니다.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

장치가 **StorSimple 8000 시리즈 업데이트 3(6.3.9600.17759)**를 실행하고 있는지 확인합니다. **마지막 업데이트 날짜** 도 수정해야 합니다. 
   - 업데이트 2 이전 버전에서 업데이트하는 경우 유지 관리 모드 업데이트가 사용 가능하다고도 표시됩니다(이 메시지는 업데이트를 설치한 후 최대 24시간 동안 계속 표시될 수 있음).
     유지 관리 모드 업데이트는 작업 중단 업데이트이므로 장치 가동 중지 시간이 발생할 수 있으며, 장치의 Windows PowerShell 인터페이스를 통해서만 적용할 수 있습니다. 일부 경우에 업데이트 1.2를 실행할 때 디스크 펌웨어가 이미 최신 버전일 수 있습니다. 이 경우 모든 유지 관리 모드 업데이트를 설치할 필요는 없습니다.
   - 업데이트 2 이상에서 업데이트하는 경우 이제 장치가 최신 상태입니다. 다음 단계를 건너뛸 수 있습니다.

[핫픽스를 다운로드하려면](#to-download-hotfixes)에 나열된 단계를 사용하여 유지 관리 모드 업데이트를 다운로드한 후 KB3121899를 검색한 후 다운로드합니다. 이 KB는 디스크 펌웨어 업데이트를 설치합니다(다른 업데이트가 이미 설치되어 있어야 함). [유지 관리 모드 핫픽스 설치 및 확인](#to-install-and-verify-maintenance-mode-hotfixes) 에 나열된 단계를 따라 유지 관리 모드 업데이트를 설치합니다. 

## <a name="install-update-3-as-a-hotfix"></a>핫픽스로 업데이트 3 설치
Azure 클래식 포털을 통해 업데이트를 설치하려고 할 때 게이트웨이 검사에 실패하는 경우 이 절차를 따릅니다. 비데이터 0 네트워크 인터페이스에 할당된 게이트웨이가 있고 장치가 업데이트 1 이전의 소프트웨어 버전을 실행하는 경우 확인이 실패합니다.

핫픽스 방법을 사용하여 업그레이드할 수 있는 소프트웨어 버전은 다음과 같습니다.

* 업데이트 0.1, 0.2, 0.3
* 업데이트 1, 1.1, 1.2
* 업데이트 2, 2.1, 2.2 

> [!IMPORTANT]
> * 장치가 릴리스(GA) 버전을 실행하는 경우 이 업데이트 설치를 위한 지원을 받으려면 [Microsoft 지원에 문의](storsimple-contact-microsoft-support.md)하세요.
> 
> 

핫픽스 메서드에는 다음 세 단계가 포함됩니다.

1. Microsoft Update 카탈로그에서 핫픽스를 다운로드합니다.
2. 일반 모드 핫픽스를 설치 및 확인합니다.
3. 유지 관리 모드 핫픽스를 설치 및 확인합니다(업데이트 2 이전 소프트웨어에서 업데이트하는 경우에만).

#### <a name="download-updates-for-your-device"></a>장치에 대한 업데이트 다운로드
**장치에서 업데이트 2.1 또는 2.2를 실행 중**인 경우 다음 핫픽스를 사전 설정된 순서대로 다운로드하고 설치해야 합니다.

| 순서 | KB | 설명 | 업데이트 유형 | 설치 시간 |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |소프트웨어 업데이트 &#42; |일반  <br></br>중단 없음 |~ 45분 |
| 2. |KB3186859 |LSI 드라이버 및 펌웨어 |일반 <br></br>중단 없음 |~ 20분 |
| 3. |KB3121261 |Storport 및 Spaceport 픽스  </br> Windows Server 2012 R2 |일반 <br></br>중단 없음 |~ 20분 |

&#42; *소프트웨어 업데이트는 `all-hcsmdssoftwareupdate`가 붙은 장치 소프트웨어 업데이트 및 `all-cismdsagentupdatebundle`이 붙은 Cis 및 Mds 에이전트와 같은 두 개의 이진 파일로 구성되어 있음에 유의하세요. 장치 소프트웨어 업데이트는 Cis 및 Mds 에이전트 전에 설치해야 합니다. 또한 Cis 및 Mds 에이전트 업데이트를 적용한 후, 나머지 업데이트를 적용하기 전에 `Restart-HcsController` cmdlet을 통해 활성 컨트롤러를 다시 시작해야 합니다.* 

**장치에서 업데이트 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 또는 2.0을 실행 중**인 경우 해당 소프트웨어, LSI 드라이버 및 펌웨어 업데이트(앞의 표에 나와 있음) 외에도 다음 핫픽스를 사전 설정된 순서대로 다운로드하여 설치해야 합니다.

| 순서 | KB | 설명 | 업데이트 유형 | 설치 시간 |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |iSCSI 패키지 |일반 <br></br>중단 없음 |~ 20분 |
| 5. |KB3103616 |WMI 패키지 |일반 <br></br>중단 없음 |~ 12분 |

<br></br>

**장치에서 버전 0.2, 0.3, 1.0, 1.1 및 1.2를 실행 중**인 경우 앞의 표에 나와 있는 모든 업데이트 맨 위에 디스크 펌웨어 업데이트를 설치해야 할 수도 있습니다. `Get-HcsFirmwareVersion` cmdlet을 실행하여 디스크 펌웨어 업데이트가 필요한지를 확인할 수 있습니다. 펌웨어 버전 `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`을 실행하는 경우 이러한 업데이트를 설치할 필요가 없습니다.

| 순서 | KB | 설명 | 업데이트 유형 | 설치 시간 |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |디스크 펌웨어 |유지 관리 <br></br>중단 |~ 30분 |

<br></br>

> [!IMPORTANT]
> * 이 절차는 업데이트 3에만 적용하도록 한 번만 수행해야 됩니다. Azure 클래식 포털을 사용하여 후속 업데이트를 적용할 수 있습니다.
> * 업데이트 2.2에서 업데이트하는 경우 전체 설치 시간은 1.1시간에 가깝습니다.
> * 이 절차에 따라 업데이트를 적용하기 전에 두 장치 컨트롤러가 온라인 상태이고 모든 하드웨어 구성 요소가 정상 상태인지 확인합니다.
> 
> 

다음 단계에 따라 핫픽스를 다운로드하고 설치합니다.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>다음 단계
[업데이트 3 릴리스](storsimple-update3-release-notes.md)에 대해 자세히 알아봅니다.




<!--HONumber=Dec16_HO1-->


