---
title: "실행 중인 IIS 웹 사이트에서 성능 문제 진단 | Microsoft Docs"
description: "다시 배포하지 않고 웹 사이트의 성능을 모니터링합니다. 독립 실행형 또는 Application Insights SDK를 사용하여 종속성 원격 분석을 가져옵니다."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: ee9ebc23ce805bb4665669077a4d3fddf4c43e32
ms.openlocfilehash: a190b1990a4ae4e7ad52cc1a7e802c8002522917


---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Application Insights를 사용한 런타임 시 웹앱 계측


코드를 수정하거나 다시 배포할 필요 없이 Azure Application Insights를 사용하여 라이브 웹앱을 계측할 수 있습니다. 앱이 온-프레미스 IIS 서버로 호스팅되는 경우 상태 모니터를 설치합니다. 또는 앱이 Azure 웹앱이거나 Azure VM에서 실행되는 경우 Application Insights 확장을 설치할 수 있습니다. ([라이브 J2EE 웹앱](app-insights-java-live.md) 및 [Azure Cloud Services](app-insights-cloudservices.md)를 계측하는 방법을 설명하는 별도의 문서도 있습니다.)

![예제 차트](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Application Insights를 .NET 웹 응용 프로그램에 적용하는 세 가지 경로가 있습니다.

* **빌드 시간:** 웹앱 코드에 [Application Insights SDK를 추가][greenbrown]합니다.
* **실행 시간:** 코드를 다시 빌드하거나 다시 작성하지 않고 아래 설명된 대로 서버에서 웹앱을 계측합니다.
* **모두:** 웹앱 코드에 SDK를 빌드하고 런타임 확장도 적용합니다. 두 옵션의 좋은 점을 선택합니다.

다음은 각 루트의 장점을 요약한 것입니다.

|  | 빌드 시간 | 실행 시간 |
| --- | --- | --- |
| 요청 및 예외 |예 |예 |
| [자세한 예외 정보](app-insights-asp-net-exceptions.md) | |예 |
| [종속성 진단](app-insights-asp-net-dependencies.md) |.NET 4.6+, 간단히 |예, 전체 세부 정보: 결과 코드, SQL 명령 텍스트, HTTP 동사|
| [시스템 성능 카운터](app-insights-performance-counters.md) |예 |예 |
| [사용자 지정 원격 분석에 대 한 API][api] |예 | |
| [추적 로그 통합](app-insights-asp-net-trace-logs.md) |예 | |
| [페이지 보기 및 사용자 데이터](app-insights-javascript.md) |예 | |
| 코드를 다시 빌드할 필요 없음 |아니요 | |

## <a name="instrument-your-web-app-at-run-time"></a>런타임 시 웹앱 계측
[Microsoft Azure](http://azure.com) 구독이 필요합니다.

### <a name="if-your-app-is-an-azure-web-app-or-cloud-service"></a>앱이 Azure 웹앱 또는 클라우드 서비스인 경우
* Azure에서 앱 제어판에서 Application Insights를 선택합니다.

    [자세히 알아보세요](app-insights-azure.md)을 확인하세요.

### <a name="if-your-app-is-hosted-on-your-iis-server"></a>앱이 IIS 서버에서 호스팅되는 경우
1. IIS 웹 서버에서 관리자 자격 증명으로 로그인합니다.
2. [상태 모니터 설치 관리자](http://go.microsoft.com/fwlink/?LinkId=506648)를 다운로드하고 실행합니다.  
3. 모니터링할 설치되어 있는 웹 응용 프로그램 또는 웹 사이트를 선택한 다음, Application Insights 포털의 결과를 볼 리소스를 구성합니다. Microsoft Azure에 로그인해야 합니다.

    ![앱과 리소스를 선택합니다.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    일반적으로 새 리소스 및 [리소스 그룹][roles]을 구성하도록 선택합니다.

    그렇지 않으면 사이트에 대해 [웹 테스트][availability] 또는 [웹 클라이언트 모니터링][client]을 이미 설정한 경우 기존 리소스를 사용합니다.
4. IIS를 다시 시작합니다.

    ![대화 상자의 위쪽에 있는 다시 시작을 선택합니다.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    웹 서비스가 잠시 중단됩니다.
5. ApplicationInsights.config가 모니터링할 웹앱에 삽입되었습니다.

    ![웹앱의 코드 파일과 함께 .config 파일을 찾습니다.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)
   

#### <a name="want-to-reconfigure-later"></a>나중에 다시 구성하시겠습니까?
마법사를 완료한 다음 원할 때마다 에이전트를 다시 구성할 수 있습니다. 에이전트를 설치했지만 초기 설정과 몇 가지 문제가 있는 경우에도 이를 사용할 수 있습니다.

![작업 표시줄의 Application Insights 아이콘 클릭](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)

## <a name="view-performance-telemetry"></a>성능 원격 분석 보기
[Azure 포털](https://portal.azure.com)에 로그인하고 Application Insights를 찾아본 다음 만든 리소스를 엽니다.

![찾아보기, Application Insights를 선택한 후 앱을 선택합니다.](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

성능 블레이드를 열어 요청, 응답 시간, 종속성 및 기타 데이터를 확인합니다.

![성능](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

아무 차트를 클릭하여 자세히 보기를 엽니다.

차트 또는 전체 블레이드를 [편집, 다시 정렬, 저장](app-insights-metrics-explorer.md)하고 [대시보드](app-insights-dashboards.md)에 고정할 수 있습니다.

## <a name="dependencies"></a>종속성
종속성 기간 차트에서는 앱에서 데이터베이스, REST API 또는 Azure BLOB 저장소와 같은 외부 구성 요소로의 호출에 걸린 시간을 보여 줍니다.

호출에 의한 차트를 다른 종속성으로 분할하려면 차트를 편집하고 그룹화를 설정한 다음, 종속성, 종속성 유형 또는 종속성 성능별로 그룹화합니다.

![종속성](./media/app-insights-monitor-performance-live-website-now/23-dep.png)

## <a name="performance-counters"></a>성능 카운터
개요 블레이드에서 서버를 클릭하여 CPU 선점 및 메모리 사용과 같은 서버 성능 카운터의 차트를 확인합니다.

다수의 서버 인스턴스가 있는 경우 차트를 편집하여 역할 인스턴스별로 그룹화하는 것이 좋습니다.

![서버](./media/app-insights-monitor-performance-live-website-now/22-servers.png)

또한 SDK에서 보고된 성능 카운터 집합을 변경할 수 있습니다. 

## <a name="exceptions"></a>예외
![서버 예외 차트를 클릭합니다.](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

특정 예외로 드릴다운(지난 7일부터)하고 스택 추적 및 컨텍스트 데이터를 가져올 수 있습니다.

## <a name="sampling"></a>샘플링
응용 프로그램이 대량의 데이터를 전송하고 ASP.NET 버전 2.0.0-beta3 또는 그 이상에 대해 Application Insights SDK를 사용하는 경우 적응 샘플링 기능이 작동하여 원격 분석의 백분율만 보낼 수 있습니다. [샘플링에 대해 자세히 알아봅니다.](app-insights-sampling.md)

## <a name="troubleshooting"></a>문제 해결
### <a name="connection-errors"></a>연결 오류
상태 모니터가 작동할 수 있도록 서버 방화벽에서 [일부 나가는 포트](app-insights-ip-addresses.md#outgoing-ports) 를 열어야 합니다.

### <a name="no-telemetry"></a>원격 분석이 없나요?
* 사이트를 사용하여 일부 데이터를 생성합니다.
* 데이터가 들어올 때까지 몇 분 정도 기다린 다음 **새로 고침**을 클릭합니다.
* 진단 검색(검색 타일)을 열어 개별 이벤트를 봅니다. 이벤트는 집계 데이터가 차트에 표시되기 전에 진단 검색에 종종 표시됩니다.
* 상태 모니터를 열고 왼쪽 창에서 응용 프로그램을 선택합니다. "구성 알림" 섹션에 이 응용 프로그램에 대한 진단 메시지가 있는지 확인합니다.

  ![성능 블레이드를 열어 요청, 응답 시간, 종속성 및 기타 데이터를 확인합니다.](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* 서버 방화벽이 위에 나열된 포트에서 나가는 트래픽을 허용하는지 확인합니다.
* 서버에서 "권한 부족"에 대한 메시지가 표시되는 경우 다음을 시도합니다.
  * IIS 관리자에서 응용 프로그램 풀을 선택하고 **고급 설정**을 연 다음 **프로세스 모델**에서 ID를 확인합니다.
  * 컴퓨터 관리 제어판에서 성능 모니터 사용자 그룹에 이 ID를 추가합니다.
* 서버에 MMA/SCOM이 설치된 경우 일부 버전이 충돌할 수 있습니다. SCOm과 상태 모니터를 제거한 다음 최신 버전을 다시 설치하세요.
* [문제 해결][qna]을 참조하세요.

## <a name="system-requirements"></a>시스템 요구 사항
Server에서 Application Insights 상태 모니터에 대한 OS 지원:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

최신 SP 및 .NET Framework 4.5 포함

클라이언트 쪽 Windows 7, 8, 8.1 및 10에서, 역시 .NET Framework 4.5 포함

IIS 지원: IIS 7, 7.5, 8, 8.5(IIS 필요)

## <a name="automation-with-powershell"></a>PowerShell을 사용한 자동화
IIS 서버에서 PowerShell을 사용하여 모니터링을 시작하고 중지할 수 있습니다.

먼저 Application Insights 모듈을 가져옵니다.

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

어떤 앱을 모니터링 중인지 확인합니다.

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (선택 사항) 웹앱의 이름입니다.
* 이 IIS 서버에서 각 웹앱(또는 명명된 앱)에 대한 상태를 모니터링하여 Application Insights를 표시합니다.
* 각 앱에 대해 `ApplicationInsightsApplication`을(를) 반환합니다.

  * `SdkState==EnabledAfterDeployment`: 상태 모니터 도구 또는 `Start-ApplicationInsightsMonitoring`에서 앱을 모니터링하고 런타임 시 계측했습니다.
  * `SdkState==Disabled`: 앱이 Application insights에 대해 계측되지 않습니다. 앱을 계측하지 않았거나 상태 모니터 도구 또는 `Stop-ApplicationInsightsMonitoring`을(를) 사용하여 런타임 모니터링이 비활성화되었습니다.
  * `SdkState==EnabledByCodeInstrumentation`: 소스 코드에 SDK를 추가하여 앱을 계측했습니다. 해당 SDK은 업데이트되거나 중지될 수 없습니다.
  * `SdkVersion` 은(는) 이 앱을 모니터링하는 데 사용하는 버전을 나타냅니다.
  * `LatestAvailableSdkVersion`은(는) NuGet 갤러리에서 현재 사용할 수 있는 버전을 나타냅니다. 앱을 이 버전으로 업그레이드하려면 `Update-ApplicationInsightsMonitoring`을(를) 사용합니다.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` IIS에서 앱의 이름
* `-InstrumentationKey` 결과를 표시하려는 Application Insights 리소스의 ikey입니다.
* 이 cmdlet은 아직 계측되지 않은 앱에만 영향을 줍니다. 즉, SdkState==NotInstrumented입니다.

    cmdlet은 빌드 시 코드에 SDK를 추가하거나 런타임 시 사전에 이 cmdlet을 사용하여 이미 계측된 앱에 영향을 주지 않습니다.

    앱을 계측하는 데 사용한 SDK 버전은 가장 최근에 이 서버에 다운로드된 버전입니다.

    최신 버전을 다운로드하려면 Update-ApplicationInsightsVersion을 사용합니다.
* 성공 시 `ApplicationInsightsApplication` 을(를) 반환합니다. 실패한 경우 stderr에 대한 추적을 기록합니다.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name` IIS에서 앱의 이름
* `-All` `SdkState==EnabledAfterDeployment`인 이 IIS 서버에서 모든 앱에 대한 모니터링을 중지합니다.
* 지정된 앱의 모니터링을 중지하고 계측을 제거합니다. 실행 시 상태 모니터링 도구 또는 Start-ApplicationInsightsApplication을 사용하여 계측된 앱에서 작동합니다. (`SdkState==EnabledAfterDeployment`)
* ApplicationInsightsApplication을 반환합니다.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: IIS에서 웹앱의 이름
* `-InstrumentationKey`(옵션) 이를 사용하여 앱의 원격 분석이 전송되는 리소스를 변경합니다.
* 이 cmdlet은:
  * 최근에 이 컴퓨터에 다운로드된 SDK 버전으로 명명된 앱을 업그레이드합니다. ( `SdkState==EnabledAfterDeployment`인 경우에만 작동)
  * 계측 키를 제공하는 경우 명명된 앱은 해당 키가 있는 리소스에 원격 분석을 전송하도록 다시 구성됩니다. ( `SdkState != Disabled`인 경우 작동)

`Update-ApplicationInsightsVersion`

* 서버에 최신 Application Insights SDK를 다운로드합니다.

## <a name="a-namenextanext-steps"></a><a name="next"></a>다음 단계
* [웹 테스트를 만들어][availability] 사이트가 라이브 상태로 유지되고 있는지 확인합니다.
* [이벤트 및 로그를 검색][diagnostic]하여 문제를 진단할 수 있습니다.
* [웹 클라이언트 원격 분석을 추가][usage]하여 웹 페이지 코드에서 예외를 확인하고 추적 호출을 삽입합니다.
* [Application Insights SDK를 웹 서비스 코드에 추가][greenbrown]하여 서버 코드에 추적 및 로그 호출을 삽입할 수 있도록 합니다.

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-web-track-usage.md



<!--HONumber=Jan17_HO1-->


