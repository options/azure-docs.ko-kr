---
title: "Azure RemoteApp - 몇 가지 일반적 시나리오로 네트워크 대역폭 사용량 테스트 | Microsoft 문서"
description: "Azure RemoteApp에 필요한 네트워크 대역폭을 확인할 수 있도록 해주는 일반적인 사용량 시나리오에 대해 알아봅니다."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 06417c75-0c63-4ecf-b9d1-66a7af6b7b91
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1729d9ba3f873800127ba3ca77676b843611fded


---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - 몇 가지 일반적 시나리오로 네트워크 대역폭 사용량 테스트
> [!IMPORTANT]
> Azure RemoteApp은 중단되었습니다. 자세한 내용은 [알림](https://go.microsoft.com/fwlink/?linkid=821148) 을 읽어보세요.
> 
> 

[Azure RemoteApp 네트워크 대역폭 사용량 예측](remoteapp-bandwidth.md)에서 설명한 대로, Azure RemoteApp이 네트워크에 미치는 영향을 알아보기 위한 가장 좋은 방법은 일부 사용량 테스트를 실행하는 것입니다. 설정한 기간 동안 테스트를 실행하고 각 시나리오에 필요한 대역폭을 측정합니다. 기능이 있으면 네트워크 패킷 손실 및 네트워크 지터도 측정하여 특정 환경에서 만들어지는 네트워크 패턴을 이해할 수 있습니다.

대역폭 사용량을 평가할 때는 회사 내의 사용자마다 사용량이 다르다는 것을 기억해야 합니다. 예를 들어 일반적으로 텍스트를 읽거나 기록하는 사용자는 동영상으로 작업하는 사용자보다 적은 대역폭을 사용합니다. 최상의 결과를 얻으려면 사용자 고유의 요구 사항을 연구하고 다음 시나리오를 혼합하여 회사의 사용자를 가장 잘 나타내는 시나리오를 만듭니다. 이상적인 테스트를 식별할 수 있도록 [대역폭 사용량 및 사용자 환경에 영향을 주는 요인을 검토](remoteapp-bandwidthexperience.md) 해야 합니다.

먼저 테스트에 대해 읽고 혼합 테스트를 선택한 다음 실행합니다. 아래 표를 사용하여 성능을 추적할 수 있습니다.

> [!NOTE]
> 사용자 고유의 네트워크 테스트를 수행할 수 없거나 수행할 시간이 없는 경우 [기본 네트워크 대역폭 예측/권장 사항](remoteapp-bandwidthguidelines.md)을 확인합니다. 그러나 진행 정도가 다양할 수 있으므로 *가능하면* 자체 테스트를 실시합니다.
> 
> 

## <a name="the-usage-tests"></a>사용량 테스트
이들 각각의 테스트는 실행 시간이 다르고 네트워크 대역폭을 사용하는 다양한 함수/기능을 테스트합니다. 개별 회사 사용자와 가장 일치하는 혼합 테스트를 선택해야 합니다.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>고급/복잡한 PowerPoint - 900-1000초 동안 실행
사용자는 전체 화면 모드로 Microsoft Office PowerPoint를 사용하여 45-50개 사이의 고품질 슬라이드를 표시합니다. 슬라이드에는 회사에 대한 일반적인 이미지, 전환(애니메이션 사용) 및 색 그라데이션된 배경이 포함됩니다. 사용자는 각 슬라이드에 최소 20초 이상을 할애합니다.

이 시나리오는 프레젠테이션에서 슬라이드가 다음 슬라이드로 전환될 때 버스티 트래픽을 만듭니다.

### <a name="simple-powerpoint---run-for-620-seconds"></a>간단한 PowerPoint - 620초 이하 동안 실행
사용자는 전체 화면 모드로 Microsoft Office PowerPoint를 사용하여 약 30개의 슬라이드로 이루어진 간단한 PowerPoint 파일을 표시합니다. 슬라이드는 고급/복잡한 PowerPoint 시나리오에 비해 비교적 텍스트를 많이 사용하며 좀 더 단순한 배경과 이미지(검은색 다이어그램)를 사용합니다. 

### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - 250초 이하 동안 실행
사용자는 Internet Explorer를 사용하여 웹을 탐색합니다. 사용자는 혼합 텍스트, 자연스러운 이미지, 일부 계통도 간을 탐색 및 스크롤합니다. 웹 페이지는 RD 세션 호스트(원격 데스크톱 세션 호스트) 서버의 로컬 디스크 드라이브에 .MHT 파일로 저장됩니다. 사용자는 각 스크롤 키/종류에 대해 다양한 간격으로 Page Up, Page Down, Up 및 Down 키를 사용하여 스크롤합니다.

    - Down - 250 keystrokes very 500 ms
    - Page Up - 36 keystrokes every 1000 ms
    - Down - 75 keystrokes every 100 ms
    - Page Down - 20 keystrokes every 500 ms
    - Up - 120 keystrokes every 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF 문서 - 간단한 문서 - 610초 이하 동안 실행
사용자는 Adobe Acrobat Reader를 사용하여 다양한 방법으로 PDF 문서를 읽고 검색합니다. 문서는 표, 간단한 그래프 및 여러 개의 텍스트 글꼴로 구성됩니다. 문서는 35-40페이지입니다. 사용자는 두 가지 속도로 이전 및 이후로 스크롤하고, 네 가지의 확대/축소 크기(페이지에 맞춤, 너비에 맞춤, 100% 및 선택한 크기)로 스크롤합니다. 확대/축소는 다양한 크기의 텍스트(글꼴)가 렌더링되도록 합니다. 스크롤은 각 스크롤의 간격이 다양한 Page Up, Page Down, Up 및 Down 키를 사용합니다.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF 문서 - 혼합 문서 - 320초 이하 동안 실행
사용자는 Adobe Acrobat Reader를 사용하여 다양한 방법으로 PDF 문서를 읽고 검색합니다. 문서는 고품질 이미지(사진 포함), 표, 간단한 그래프 및 여러 텍스트 글꼴로 구성됩니다. 사용자는 두 가지 속도로 이전 및 이후로 스크롤하고, 네 가지의 확대/축소 크기(페이지에 맞춤, 너비에 맞춤, 100% 및 선택한 크기)로 스크롤합니다. 확대/축소는 다양한 크기의 텍스트(글꼴)가 렌더링되도록 합니다. 스크롤은 각 스크롤의 간격이 다양한 Page Up, Page Down, Up 및 Down 키를 사용합니다.

### <a name="flash-video-playback---run-for-180-seconds"></a>플래시 비디오 재생 - 180초 이하 동안 실행
사용자는 웹 페이지에 포함된 Adobe Flash 인코드된 동영상을 봅니다. 웹 페이지는 RD 세션 호스트 서버의 로컬 하드 드라이브에 저장됩니다. 동영상은 Internet Explorer 내에서 포함된 플레이어 플러그 인에 의해 재생됩니다.

이 시나리오는 멀티미디어를 포함하는 풍부한 콘텐츠 웹 페이지를 보는 사용자를 에뮬레이트합니다. 대부분의 데이터는 VOBR을 거칩니다.

### <a name="word-remote-typing---run-for-1800-seconds"></a>단어 원격 입력 - 1800초 이하 동안 실행
사용자가 RDP 세션을 통해 문서를 입력합니다. 키 입력은 클라이언트 쪽에서 RDP 세션을 통해 원격 세션에서 실행 중인 Microsoft Word의 문서로 전송됩니다. 입력 속도는 250ms당 한 자(총 7050자)입니다. 

이는 지식 근로자에 대한 가장 일반적인 시나리오 중 하나입니다. 이 시나리오는 현대적 작업 프로세서에 입력하는 사용자의 응답성을 테스트합니다. 이 시나리오는 대역폭 사용량의 작은 변화에도 민감합니다.

## <a name="tracking-the-test-results"></a>테스트 결과 추적
사용자 환경에서 시나리오를 평가하는 데 다음 표를 사용할 수 있습니다. 아래에 제공된 데이터는 설명을 위한 것이며 관찰한 것과는 크게 다를 수 있습니다. 

쉽게 말하면 모든 시나리오가 동일한 1920x1080 픽셀의 화면 해상도와 네트워크(200ms 미만의 대기 시간(지연) 및 120ms+ 표시된 것의 약 1% 네트워크 지터 사용)에서 TCP 전송을 사용하여 테스트된 것으로 가정합니다.

표 정보:

* **평균 환경** 에는 사용자 생산성은 크게 영향을 받지 않지만 간헐적인 비디오 또는 오디오 결함이 제외되지 않은 네트워크 대역폭이 포함됩니다. 시스템은 동적 논리를 활용하여 신속하게 복구할 수 있습니다. 네트워크 대역폭 예측은 사용자 환경의 품질을 보장하기 위한 것입니다.
  * **눈에 띄는 문제(중단점)** 에는 사용자 환경에서의 중요한 문제를 확인할 수 있고 생산성은 측정 가능한 기간에 영향을 받는 네트워크 대역폭이 포함됩니다. 이 시점에서 RDP 알고리즘에 어려움이 발생하고 부족한 네트워크 대역폭으로 인해 사용자 체감 품질을 보장할 수 없습니다.
  * **권장** 에는 적합한 또는 최적의 환경을 위해 권장되는 네트워크 대역폭이 포함됩니다. 이는 일반적으로 **평균 환경** 열에 해당하는 값보다 한 단계 높습니다.
  * **참고** 에는 관찰 내용 및 설명이 포함됩니다.

| 테스트 | 평균 환경 | 눈에 띄는 문제(중단점) | 권장 네트워크 대역폭 | 참고 |
| --- | --- | --- | --- | --- |
| 고급/복잡한 PPT |10MB/초 |1MB/초 |>10MB/s, 100MB/s 선호 |1MB/s에서 많은 애니메이션이 손실됨 |
| 간단한 PPT |5MB/초 |256KB/초 |10MB/초 |256KB/s에서 슬라이드 로드가 눈에 띄게 지연됨 |
| Internet Explorer |10MB/초 |1MB/초 |>10MB/s, 100MB/s 선호 |1MB/s에서 웹 동영상이 흐리고 고르지 못함, 빠른 스크롤 시 문제 발생 |
| 간단한 PDF |1MB/초 |256KB/초 |5MB/초 |256KB/s에서 페이지 로드에 시간이 걸림 |
| 혼합 PDF |1MB/초 |256KB/초 |5MB/초 |256KB/s에서 페이지 로드에 매우 많은 시간이 소요됨 |
| 플래시 동영상 재생 |10MB/초 |1MB/초 |>10MB/s, 100MB/s 선호 |1MB/s에서 동영상이 선명하지 못하며 일부 프레임이 손실됨 |
| 단어 원격 입력 |256KB/초 |128KB/초 |1MB/초 |256KB/s에서 사용자가 키 입력 간격을 알 수 있음 |

사용자당 네트워크 대역폭을 평가하려면 위 시나리오와 필요한 네트워크 대역폭에 해당하는 부분을 혼합하여 만듭니다. 시나리오에 필요한 가장 큰 숫자를 선택합니다. 사용자는 거의 시스템만을 사용하지는 않으므로 동일한 네트워크에서 동시에 작업하는 사용자를 위해 일부 예약하는 것이 좋습니다.

## <a name="learn-more"></a>자세한 정보
* [Azure RemoteApp 네트워크 대역폭 사용량 예측](remoteapp-bandwidth.md)
* [Azure RemoteApp - 네트워크 대역폭과 체감 품질을 함께 작동하는 방식은 무엇인가요?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp 네트워크 대역폭 - 일반적인 지침(자체 테스트를 수행할 수 없는 경우)](remoteapp-bandwidthguidelines.md)




<!--HONumber=Nov16_HO3-->


