---
title: Service Bus FAQ | Microsoft Docs
description: "Azure 서비스 버스에 대한 일부 자주 묻는 질문을 답변합니다."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2016
ms.author: sethm;juconway
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: b350bac36f1d46c97da37a2807ead8c9c732d69a


---
# <a name="service-bus-faq"></a>서비스 버스 FAQ
이 문서는 Microsoft Azure 서비스 버스에 대한 일부 자주 묻는 질문을 답변합니다. 또한 일반적인 Azure 가격 책정 및 지원 정보는 [Azure 지원 FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) 를 방문할 수 있습니다. 다음 항목이 포함됩니다.

* [Azure Service Bus 메시징에 대한 일반적인 질문](#general-questions-about-azure-service-bus-messaging)
* [Service Bus 모범 사례](#service-bus-best-practices)
* [Service Bus 가격 책정](#service-bus-pricing)
* [Service Bus 할당량](#service-bus-quotas)
* [구독 및 네임스페이스 관리](#subscription-and-namespace-management)
* [문제 해결](#service-bus-troubleshooting)

## <a name="general-questions-about-azure-service-bus-messaging"></a>Azure Service Bus 메시징에 대한 일반적인 질문
### <a name="what-is-azure-service-bus-messaging"></a>Azure Service Bus 메시징이란?
[Azure Service Bus 메시징](service-bus-messaging-overview.md)은 분리된 시스템 간에 데이터를 보낼 수 있도록 하는 비동기 메시징 클라우드 플랫폼입니다. Microsoft에서 이 기능을 서비스로 제공하므로 이를 사용하기 위해 고유한 하드웨어 중 하나를 호스트할 필요는 없습니다.

### <a name="what-is-a-service-bus-namespace"></a>서비스 버스 네임스페이스란?
[네임스페이스](service-bus-create-namespace-portal.md)는 응용 프로그램 내에서 Service Bus 리소스의 주소를 지정하기 위한 범위 컨테이너를 제공합니다. 네임스페이스를 만드는 작업은 서비스 버스를 사용하는 데 필요하고 서비스 버스를 시작하는 첫 번째 단계 중 하나입니다.

### <a name="what-is-an-azure-service-bus-queue"></a>Azure 서비스 버스 큐란?
[Service Bus 큐](service-bus-queues-topics-subscriptions.md)는 메시지가 저장되는 엔터티입니다. 큐는 서로 통신해야 하는 여러 응용 프로그램 또는 분산된 응용 프로그램의 여러 부분이 있는 경우에 특히 유용합니다. 큐는 여러 제품(메시지)를 해당 위치에서 받고 보내는 배포 센터와 비슷합니다.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Service Bus 토픽 및 구독이란?
토픽은 큐로 시각화할 수 있고 여러 구독을 사용하는 경우 더 풍부한 메시징 모델이 되며 기본적으로는 일대다 통신 도구입니다. 이 게시/구독 모델(또는 *pub/sub*)을 사용하면 여러 응용 프로그램에서 받은 메시지를 여러 구독을 사용하여 토픽에 메시지를 전송하는 응용 프로그램을 활성화합니다.

### <a name="what-is-a-partitioned-entity"></a>분할된 엔터티란?
일반적인 큐 또는 항목은 단일 메시지 broker에서 처리되며 하나의 메시징 저장소에 저장됩니다. [분할된 큐 또는 토픽](service-bus-partitioning.md)은 여러 메시지 broker에 의해 처리되고 여러 메시징 저장소에 저장됩니다. 즉, 분할된 큐 또는 항목의 전체 처리량은 단일 메시지 broker 또는 메시징 저장소의 성능으로 제한되지 않습니다. 또한 메시징 스토어가 일시적으로 중단된 경우에도 분할된 큐 또는 항목을 계속 렌더링할 수 없습니다.

분할 엔터티를 사용하는 경우 순서는 보장할 수 없습니다. 파티션을 사용할 수 없는 이벤트에서도 여전히 메세지를 보내고 다른 파티션에서 메시지를 수신할 수 있습니다.

## <a name="service-bus-best-practices"></a>서비스 버스 모범 사례
### <a name="what-are-some-azure-service-bus-best-practices"></a>일부 Azure 서비스 버스 모범 사례는 무엇인가요?
* [Service Bus 조정된 메시징을 사용한 성능 향상의 모범 사례][Service Bus 조정된 메시징을 사용한 성능 향상의 모범 사례] - 이 문서에서는 조정된 메시지를 교환할 때 성능을 최적화하는 방법을 설명합니다.

### <a name="what-should-i-know-before-creating-messaging-entities"></a>메시징 엔터티를 만들기 전에 무엇을 알아야 하나요?
큐 및 토픽에서 다음 속성을 변경할 수 없습니다. 새 대체 엔터티를 만들지 않고 엔터티를 프로비전하는 경우 다음 항목을 수정할 수 없다는 점을 고려해야 합니다.

* 크기
* 분할
* 세션
* 중복 검색
* Express 엔터티

## <a name="service-bus-pricing"></a>서비스 버스 가격 책정
이 섹션은 서비스 버스 가격 책정 구조에 대한 일부 자주 묻는 질문을 답변합니다. 또한 일반적인 Microsoft Azure 가격 책정 정보는 [Azure 지원 FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) 를 방문할 수 있습니다. 서비스 버스 가격 책정에 대한 전체 내용은 [서비스 버스 가격 책정 세부 정보](https://azure.microsoft.com/pricing/details/service-bus/)를 참조하세요.

### <a name="how-do-you-charge-for-service-bus"></a>서비스 버스 요금을 어떻게 청구하나요?
Service Bus 가격 책정에 대한 전체 내용은 [Service Bus 가격 책정 세부 정보][가격 책정 개요]를 참조하세요. 언급된 가격 외에도 응용 프로그램이 프로비전되는 데이터 센터의 외부에서 송신을 위해 연결된 데이터 전송에 요금이 부과됩니다.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>서비스 버스를 어떻게 사용하면 데이터가 전송의 대상이 되나요? 어떤 경우에 대상이 아닌가요?
지정된 Azure 지역 내에서 데이터 전송은 비용 없이 제공됩니다. 지역 외부의 데이터 전송은 북아메리카 및 유럽 지역에서 GB 당 0.15달러($)이며 아시아-태평양 지역에서 GB 당 0.20달러($)의 요금제로 송신 요금이 적용됩니다. 인바운드 데이터 전송은 무료로 제공됩니다.

### <a name="does-service-bus-charge-for-storage"></a>서비스 버스는 저장소에 대한 요금을 청구하나요?
아니요, 서비스 버스는 저장소에 대한 요금을 청구하지 않습니다. 하지만 큐/항목 당 지속될 수 있는 데이터의 최대 크기를 제한하는 할당량이 있습니다. 다음 FAQ를 참조하세요.

## <a name="service-bus-quotas"></a>서비스 버스 할당량
Service Bus 제한 및 할당량 목록은 [할당량 개요][할당량 개요]를 참조하세요.

### <a name="does-service-bus-have-any-usage-quotas"></a>서비스 버스는 사용 할당량이 있나요?
기본적으로 모든 클라우드 서비스의 경우 Microsoft는 모든 고객의 구독에 대해 계산되는 월별 사용 할당량을 집계합니다. 이러한 제한 보다 사용자의 필요가 많을 수도 있다는 것을 이해하기 때문에 사용자의 요구를 이해하고 이러한 제한을 적절하게 조정할 수 있도록 언제든지 서비스에 문의하세요. 서비스 버스의 경우 집계 사용 할당량은 다음과 같습니다.

* 50억 개의 메시지

지정된 달에 사용 할당량을 초과하는 고객의 계정을 사용하지 않도록 설정하는 권한을 보유하지만 메일 알림을 제공하며 조치를 취하기 전에 고객에게 연락을 여러 번 시도합니다. 이러한 할당량을 초과하는 고객은 할당량을 초과하는 요금을 지불해야 합니다.

Azure에서 다른 서비스와 마찬가지로 서비스 버스는 리소스의 공정한 사용을 보장하기 위해 특정한 할당량 집합을 적용합니다. 다음은 서비스에서 적용하는 사용 할당량입니다.

#### <a name="queuetopic-size"></a>큐/토픽 크기
큐 또는 토픽이 생성될 때 최대 큐 또는 토픽의 크기를 지정합니다. 이 할당량 값은 1, 2, 3, 4 또는 5GB일 수 있습니다. 최대 크기에 도달하면, 추가로 수신되는 메시지가 거부되며 호출 코드에서 예외를 수신합니다.

#### <a name="naming-restrictions"></a>명명 제한
Service Bus 네임스페이스 이름의 길이는 6-50자여야 합니다. 각 큐, 토픽 또는 구독에 대한 문자 수는 1-50자 사이로 제한됩니다.

#### <a name="number-of-concurrent-connections"></a>동시 연결 수
큐/토픽/구독 - 큐/토픽/구독에 동시 TCP 연결 수가 100개로 제한됩니다. 이 할당량에 도달하면 후속 추가 연결 요청이 거부되며 호출 코드에서 예외를 수신합니다. 모든 메시징 팩터리의 경우 해당 메시징 팩터리에서 만든 클라이언트가 활성 작업을 보류하거나 60초 이내에 작업을 완료한 경우 서비스 버스는 하나의 TCP 연결을 유지합니다. REST 작업은 동시 TCP 연결 계산에 포함되지 않습니다.

#### <a name="number-of-topicsqueues-per-service-namespace"></a>서비스 네임스페이스당 토픽/큐 수
토픽/큐(영구 저장소 지원 엔터티)는 서비스 네임스페이스에 최대 10,000개로 제한합니다. 이 할당량에 도달하면 서비스 네임스페이스에 새 토픽/큐를 만들기 위한 후속 요청이 거부됩니다. 이 경우에 만들기 시도가 포털을 통해 또는 클라이언트 코드에서 수행되었는지 여부에 따라 Azure 클래식 포털이 오류 메시지를 표시하거나 호출 클라이언트 코드에서 예외가 발생합니다.

### <a name="message-size-quotas"></a>메시지 크기 할당량
#### <a name="queuetopicsubscription"></a>큐/토픽/구독
**메시지 크기** -각 메시지는 메시지 헤더를 포함하여 총 크기를 256KB로 제한합니다.

**메시지 헤더 크기** – 각 메시지 헤더는 64KB로 제한합니다.

해당 크기 할당량을 초과하는 메시지가 거부되며 호출 코드에서 예외를 수신합니다.

**항목당 구독 수** – 항목당 최대 구독 수를 2,000개로 제한합니다. 이 할당량에 도달하면 항목에 추가 구독을 만들기 위한 후속 요청이 거부됩니다. 이 경우에 만들기 시도가 포털을 통해 또는 클라이언트 코드에서 수행되었는지 여부에 따라 Azure 클래식 포털이 오류 메시지를 표시하거나 호출 클라이언트 코드에서 예외가 발생합니다.

**항목당 SQL 필터 수** – 항목당 SQL 필터의 최대 수를 2,000개로 제한합니다. 이 할당량에 도달하면 후속 항목에서 추가 필터의 생성이 거부되며 호출 코드에서 예외를 수신합니다.

**항목당 상관관계 필터 수** – 항목당 상관관계 필터의 최대 수는 100,000개로 제한됩니다. 이 할당량에 도달하면 후속 항목에서 추가 필터의 생성이 거부되며 호출 코드에서 예외를 수신합니다.

## <a name="subscription-and-namespace-management"></a>구독 및 네임스페이스 관리
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>다른 Azure 구독으로 네임스페이스를 마이그레이션하려면 어떻게 해야 하나요?
PowerShell 명령([여기][여기] 문서에 위치)을 사용하여 Azure 구독 간에 네임스페이스를 이동할 수 있습니다. 작업을 실행하기 위해 네임스페이스가 활성화되어야 합니다. 또한 명령을 실행하는 사용자는 원본 및 대상 구독에 대한 관리자여야 합니다.

## <a name="service-bus-troubleshooting"></a>서비스 버스 문제 해결
[예외 개요][예외 개요]

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-messaging-apis-and-their-suggested-actions"></a>Azure 서비스 버스 메시징 API 및 해당 제안된 작업에 의해 생성된 일부 예외는 무엇인가요?
메시징 API를 생성할 수 있는 예외는 다음과 같은 범주로 분류됩니다.

* 사용자 코딩 오류
* 설치/구성 오류
* 일시적 예외
* 기타 예외

[Service Bus 메시징 예외][예외 개요] 문서에서는 권장된 조치를 사용하여 몇 가지 예외를 설명합니다.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>공유 액세스 서명이란 무엇이고 어떤 언어가 서명 생성을 지원하나요?
공유 액세스 서명은 SHA – 256 보안 해시 또는 URI에 따른 인증 메커니즘입니다. 노드, PHP, Java 및 C\#에서 직접 서명을 생성하는 방법에 대한 내용은 [공유 액세스 서명][공유 액세스 서명] 문서를 참조하세요.

## <a name="next-steps"></a>다음 단계
서비스 버스 메시징에 대해 자세히 알아보려면 다음 항목을 참조하세요.

* [Azure 서비스 버스 프리미엄 메시징 소개(블로그 게시물)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure 서비스 버스 프리미엄 메시징 소개(Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [서비스 버스 메시징 개요](service-bus-messaging-overview.md)
* [Azure 서비스 버스 아키텍처 개요](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus 큐 시작](service-bus-dotnet-get-started-with-queues.md)

[Service Bus 조정된 메시징을 사용한 성능 향상의 모범 사례]: service-bus-performance-improvements.md
[Service Bus 가동 중단 및 재해로부터 응용 프로그램을 보호하기 위한 모범 사례]: service-bus-outages-disasters.md
[가격 책정 개요]: https://azure.microsoft.com/pricing/details/service-bus/
[할당량 개요]: service-bus-quotas.md
[여기]: service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription
[예외 개요]: service-bus-messaging-exceptions.md
[공유 액세스 서명]: service-bus-sas-overview.md



<!--HONumber=Nov16_HO3-->


