---
title: Resource Manager 지원 서비스 | Microsoft Docs
description: 리소스 관리자, 스키마, 제공되는 API 버전 및 리소스를 호스팅할 수 있는 지역을 지원하는 리소스 공급자에 대해 설명합니다.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: magoedte;tomfitz

---
# 리소스 관리자 공급자, 지역, API 버전 및 스키마
Azure 리소스 관리자는 응용 프로그램을 구성하는 서비스를 배포하고 관리하는 새로운 방법을 제공합니다. 모두는 아니지만, 대부분의 서비스는 리소스 관리자를 지원하고 일부 서비스는 리소스 관리자를 부분적으로만 지원합니다. Microsoft는 향후 솔루션을 위해 중요한 모든 서비스에 리소스 관리자를 사용하도록 할 예정이지만 일관적으로 지원될 때까지 각 서비스의 현재 상태를 알아야 합니다. 이 항목에서는 Azure 리소스 관리자에 대해 지원되는 리소스 공급자 목록을 제공합니다.

리소스를 배포할 때는 이러한 리소스를 지원하는 지역 및 리소스에 사용할 수 있는 API 버전도 알아야 합니다. [지원되는 지역](#supported-regions) 섹션에서는 구독 및 리소스에 대해 작동할 지역을 확인하는 방법을 보여줍니다. 섹션 [지원되는 API 버전](#supported-api-versions)에서는 사용할 수 있는 API 버전을 결정하는 방법을 보여줍니다.

Azure 포털 및 클래식 포털에서 어떤 서비스가 지원되는지 알아보려면 [Azure 포털 가용성 차트](https://azure.microsoft.com/features/azure-portal/availability/)를 참조하세요. 리소스 이동을 지원하는 서비스를 보려면 [새 리소스 그룹 또는 구독으로 리소스 이동](resource-group-move-resources.md)을 참조하세요.

다음 표에서는 리소스 관리자를 통해 배포 및 관리를 지원 또는 지원하지 않는 서비스 목록을 보여줍니다. **빠른 시작 템플릿** 열의 링크는 지정된 리소스 공급자에 대한 Azure 빠른 시작 템플릿 리포지토리에 쿼리를 보냅니다. 빠른 시작 템플릿은 수시로 추가 및 업데이트됩니다. 특정 서비스에 대한 링크가 있어도 반드시 쿼리가 리포지토리의 템플릿을 반환하는 것은 아닙니다.

## 계산
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| 배치 |예 |[Batch REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) |[2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json) | |
| 컨테이너 |예 |[컨테이너 서비스 REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) |[2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) |[Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics Lifecycle Services |예 | | | |
| 크기 집합 |예 |[크기 집합 REST](https://msdn.microsoft.com/library/azure/mt705635.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |[virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) |
| 서비스 패브릭 |예 |[서비스 패브릭 Rest](https://msdn.microsoft.com/library/azure/dn707692.aspx) | |[Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| 가상 컴퓨터 |예 |[VM REST](https://msdn.microsoft.com/library/azure/mt163647.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |[virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| 가상 컴퓨터(클래식) |제한됨 |- |- |- |
| 원격 앱 |아니요 |- |- |- |
| 클라우드 서비스(클래식) |제한됨(아래 참조) |- |- |- |

가상 컴퓨터(클래식)는 리소스 관리자 배포 모델 대신, 클래식 배포 모델을 통해 배포된 리소스를 참조합니다. 일반적으로 이러한 리소스는 리소스 관리자 작업을 지원하지 않지만 일부 작업은 가능합니다. 이러한 배포 모델에 대한 자세한 내용은 [리소스 관리자 배포 및 클래식 배포 이해](resource-manager-deployment-model.md)를 참조하세요.

클라우드 서비스(클래식)는 다른 클래식 리소스에 사용될 수 있지만 클래식 리소스는 모든 리소스 관리 기능을 사용하지 않으며 향후 솔루션에 적합하지 않습니다. 대신, 응용 프로그램 인프라가 Microsoft.Compute, Microsoft.Storage 및 Microsoft.Network 네임스페이스의 리소스를 사용하도록 변경하는 것이 좋습니다.

## 네트워킹
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| 응용 프로그램 게이트웨이 |예 |[응용 프로그램 게이트웨이 REST](https://msdn.microsoft.com/library/azure/mt684939.aspx) | |[applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS |예 |[DNS REST](https://msdn.microsoft.com/library/azure/mt163862.aspx) |[2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) |[dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| Express 경로 |예 |[Express 경로 REST](https://msdn.microsoft.com/library/azure/mt586720.aspx) | |[expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| 부하 분산 장치 |예 |[부하 분산 장치 REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |[loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| 트래픽 관리자 |예 |[트래픽 관리자 REST](https://msdn.microsoft.com/library/azure/mt163667.aspx) |[2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json) |[trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| 가상 네트워크 |예 |[가상 네트워크 REST](https://msdn.microsoft.com/ko-KR/library/azure/mt163650.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |[virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN 게이트웨이 |예 |[네트워크 게이트웨이 REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) | |[virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[connections](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |

## 데이터 및 저장소
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- | --- |
| DocumentDB |예 |[DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) |[2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json) |[Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis 캐시 |예 | |[2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) |[Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| 검색 |예 |[검색 REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) | |[Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| 저장소 |예 |[저장소 REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) |[저장소 계정](resource-manager-template-storage.md) |[Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| SQL 데이터베이스 |예 |[SQL 데이터베이스 REST](https://msdn.microsoft.com/library/azure/mt163571.aspx) |[2014-04-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) |[Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL 데이터 웨어하우스 |예 | | | |
| StorSimple |아니오 |- |- |- |

## 웹 및 모바일
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| API 앱 |예 | |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[API 앱](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API 관리 |예 |[API 관리 REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | |[Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) |
| 논리 앱 |예 |[워크플로 서비스 REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) |[2015-02-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-02-01-preview/Microsoft.Logic.json) |[Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| 모바일 앱 |예 | |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code) |
| 모바일 고객 관리 |예 | | |[Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| 웹앱 |예 | |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## 분석
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| 데이터 카탈로그 |예 |[데이터 카탈로그 REST](https://msdn.microsoft.com/library/azure/mt267595.aspx) |[2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) | |
| 데이터 팩터리 |예 |[데이터 팩터리 REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) | |[Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| 데이터 레이크 분석 |예 | |[2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) |[Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| 데이터 레이크 저장소 |예 |[Data Lake Store 저장소 REST](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) |[Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights |예 |[HDInsights REST](https://msdn.microsoft.com/library/azure/mt622197.aspx) | |[Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| 기계 학습 |예 |[기계 학습 REST](https://msdn.microsoft.com/library/azure/mt767538.aspx) |[2016-05-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) | |
| 스트림 분석 |예 |[스트림 분석 REST](https://msdn.microsoft.com/library/azure/dn835031.aspx) | | |
| Power BI |예 |[Power BI Embedded REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) |[2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) | |

## 인텔리전스
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| Cognitive Services |예 | |[2016-02-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) | |

## 사물 인터넷
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| 이벤트 허브 |예 |[이벤트 허브 REST](https://msdn.microsoft.com/library/azure/dn790674.aspx) | | |
| IoTHubs |예 |[IoT Hub REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) |[2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) |[Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| 알림 허브 |예 |[알림 허브 REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) |[2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) |[Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## 미디어 및 CDN
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| CDN |예 |[CDN REST](https://msdn.microsoft.com/library/azure/mt634456.aspx) |[2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) |[Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| 미디어 서비스 |예 |[미디어 서비스 REST](https://msdn.microsoft.com/library/azure/hh973617.aspx) |[2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) | |

## 하이브리드 통합
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| BizTalk 서비스 |예 | |[2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) | |
| 복구 서비스 |예 |[Site Recovery REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) |[2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) |[Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| 서비스 버스 |예 | | |[Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## ID 및 액세스 관리
Azure Active Directory는 구독에 대해 리소스 관리자로 작동하므로 역할 기반 액세스 제어가 가능합니다. 역할 기반 액세스 제어 및 Active Directory를 사용하는 방법에 대한 자세한 내용은 [Azure 역할 기반 액세스 제어](active-directory/role-based-access-control-configure.md)를 참조하세요.

## 개발자 서비스
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| Application Insights |예 |[Insights REST](https://msdn.microsoft.com/library/azure/dn931943.aspx) |[2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) |[Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing 지도 |예 | | | |
| DevTest Labs |예 | |[2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) |[Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code) |
| Visual Studio 계정 |예 | |[2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) | |

## 관리 및 보안
| 부여 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- |
| 자동화 |예 |[자동화 REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) |[2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json) |[Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| 키 자격 증명 모음 |예 |[키 자격 증명 모음 REST](https://msdn.microsoft.com/library/azure/dn903609.aspx) |[키 자격 증명 모음](resource-manager-template-keyvault.md)<br />[키 자격 증명 모음 암호](resource-manager-template-keyvault-secret.md) |[Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Operational Insights |예 | | | |
| 스케줄러 |예 |[스케줄러 REST](https://msdn.microsoft.com/library/azure/mt629143.aspx) |[2014-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-08-01/Microsoft.Scheduler.json) |[Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| 보안(미리 보기) |예 |[보안 REST](https://msdn.microsoft.com/library/azure/mt704034.aspx) | |[Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code) |

## 리소스 관리자
| 기능 | 리소스 관리자 사용 | REST API | 스키마 | 빠른 시작 템플릿 |
| --- | --- | --- | --- | --- | --- |
| 권한 부여 |예 |[관리 잠금](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[역할 기반 액세스 제어](https://msdn.microsoft.com/library/azure/dn906885.aspx) |[리소스 잠금](resource-manager-template-lock.md)<br />[역할 할당](resource-manager-template-role.md) |[Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| 리소스 |예 |[연결된 리소스](https://msdn.microsoft.com/library/azure/mt238499.aspx) |[리소스 링크](resource-manager-template-links.md) |[Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |

## 리소스 공급자 및 형식
리소스를 배포할 때는 리소스 공급자 및 형식에 대한 정보를 자주 검색하게 됩니다. REST API, Azure PowerShell 또는 Azure CLI를 통해 이 정보를 검색할 수 있습니다.

리소스 공급자로 작업하려면 해당 리소스 공급자를 계정에 등록해야 합니다. 기본적으로 리소스 공급자는 대부분 자동으로 등록되지만 일부 리소스 공급자는 수동으로 등록해야 합니다. 아래 예제에서는 리소스 공급자의 등록 상태를 가져오고 필요한 경우 리소스 공급자를 등록하는 방법을 보여 줍니다.

### REST API
형식, 위치, API 버전 및 등록 상태를 비롯한 모든 사용 가능한 리소스 공급자를 가져오려면 [모든 리소스 공급자 나열](https://msdn.microsoft.com/library/azure/dn790524.aspx) 작업을 사용합니다. 리소스 공급자를 등록해야 하는 경우 [리소스 공급자로 구독 등록](https://msdn.microsoft.com/library/azure/dn790548.aspx)을 참조하세요.

### PowerShell
다음 예제는 사용 가능한 모든 리소스 공급자를 가져오는 방법을 보여 줍니다.

    Get-AzureRmResourceProvider -ListAvailable


다음 예제는 특정 리소스 공급자에 대한 리소스 형식을 가져오는 방법을 보여 줍니다.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes

다음과 유사하게 출력됩니다.

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...

네임스페이스를 제공하는 리소스 공급자를 등록하려면

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### Azure CLI
다음 예제는 사용 가능한 모든 리소스 공급자를 가져오는 방법을 보여 줍니다.

    azure provider list

다음과 유사하게 출력됩니다.

    info:    Executing command provider list
    + Getting ARM registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

다음 명령을 사용하여 특정 리소스 공급자에 대한 정보를 파일에 저장할 수 있습니다.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

네임스페이스를 제공하는 리소스 공급자를 등록하려면

    azure provider register -n Microsoft.ServiceBus

## 지원되는 지역
리소스를 배포할 때는 일반적으로 리소스에 대한 지역을 지정해야 합니다. 리소스 관리자는 모든 지역에서 지원되지만 배포한 리소스는 모든 지역에서 지원되지 않을 수 있습니다. 또한 해당 리소스를 지원하는 일부 지역을 사용하지 못하게 하는 구독에 대한 제한 사항이 있을 수 있습니다. 이러한 제한은 본국에 대한 세금 문제와 관련되거나 구독 관리자가 특정 지역만 사용하도록 배치한 정책에 따른 결과일 수 있습니다.

모든 Azure 서비스에 대해 지원되는 모든 지역의 전체 목록은 [지역별 서비스](https://azure.microsoft.com/regions/#services)를 참조하세요. 그러나 이 목록에는 구독에서 지원하지 않는 지역이 포함될 수 있습니다. 다음 명령 중 하나를 실행하여 구독에서 지원하는 특정 리소스 종류에 대한 지역을 확인할 수 있습니다.

### REST API
구독 중인 특정 리소스 종류에 사용할 수 있는 지역을 검색하려면 [모든 리소스 공급자 나열](https://msdn.microsoft.com/library/azure/dn790524.aspx) 작업을 사용하세요.

### PowerShell
다음 예제에서는 웹 사이트에 대해 지원되는 지역을 가져오는 방법을 보여 줍니다.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

다음과 유사하게 출력됩니다.

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### Azure CLI
다음 예에서는 각 리소스 유형에 대해 지원되는 모든 지역을 반환합니다.

    azure location list

또한 [jq](https://stedolan.github.io/jq/)와 같은 JSON 유틸리티를 사용하여 위치 결과를 필터링할 수도 있습니다.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

반환하는 내용은 다음과 같습니다.

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## 지원되는 API 버전
서식 파일을 배포할 때 각 리소스를 만들기 위해 사용하는 API 버전을 지정해야 합니다. API 버전은 리소스 공급자가 릴리스하는 REST API 작업의 버전에 해당합니다. 리소스 공급자는 새 기능을 사용하도록 설정할 때 REST API의 새 버전을 릴리스합니다. 따라서 서식 파일에 지정하는 API의 버전은 템플릿에 지정할 수 있는 속성에 영향을 줍니다. 일반적으로 새 템플릿을 만들 때 가장 최근의 API 버전을 선택하려고 합니다. 기존 템플릿의 경우 이전 API 버전을 계속 사용할지 아니면 새 기능을 이용하도록 최신 버전에 맞게 템플릿을 업데이트할지 여하를 결정할 수 있습니다.

### REST API
리소스 종류에 사용할 수 있는 API 버전을 검색하려면 [모든 리소스 공급자 나열](https://msdn.microsoft.com/library/azure/dn790524.aspx) 작업을 사용하세요.

### PowerShell
다음 예제에서는 특정 리소스 종류에 사용할 수 있는 API 버전을 가져오는 방법을 보여 줍니다.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

다음과 유사하게 출력됩니다.

    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### Azure CLI
다음 명령을 사용하여 리소스 공급자에 대한 정보(사용할 수 있는 API 버전 포함)를 파일로 저장할 수 있습니다.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

파일을 열고 **apiVersions** 요소를 찾을 수 있습니다.

## 다음 단계
* 리소스 관리자 템플릿을 만드는 방법에 대한 자세한 내용은 [Azure 리소스 관리자 템플릿 작성](resource-group-authoring-templates.md)을 참조하세요.
* 리소스 배포에 대한 자세한 내용은 [Azure 리소스 관리자 템플릿을 사용하여 응용 프로그램 배포](resource-group-template-deploy.md)를 참조하세요.

<!---HONumber=AcomDC_0720_2016-->