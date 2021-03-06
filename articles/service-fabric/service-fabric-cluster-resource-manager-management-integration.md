---
title: "Service Fabric 클러스터 리소스 관리자 - 관리 통합 | Microsoft Docs"
description: "클러스터 리소스 관리자과 서비스 패브릭 관리 간에 통합 지점의 개요입니다."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f13e38b4c01bc718f6f25f92461e332e1aa30136


---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>클러스터 리소스 관리자와 서비스 패브릭 클러스터 관리 통합
서비스 패브릭 클러스터 리소스 관리자는 관리 작업(예: 응용 프로그램 업그레이드)을 처리하는 서비스 패브릭의 주요 구성 요소가 아니지만 관련이 있습니다. 클러스터 Resource Manager가 관리에 도움이 되는 첫 번째 방법은 클러스터를 원하는 구성에 넣을 수 없을 때 리소스 관리 및 분산 측면과 상태 보고서 전송에서 클러스터와 내부 서비스의 필요한 상태를 추적하는 것입니다(예를 들어 충분한 용량이 없는 경우 또는 서비스를 배치해야 할 위치에 대한 규칙 충돌이 있는 경우). 통합의 다른 부분은 업그레이드의 작동 방법과 관련이 있습니다. 업그레이드하는 동안 클러스터 Resource Manager는 해당 동작을 변경합니다. 아래에서 두 가지 모두에 대해 알아보겠습니다.

## <a name="health-integration"></a>상태 통합
클러스터 Resource Manager는 지속적으로 서비스와 노드 및 클러스터의 사용 가능한 용량에 대해 지정한 규칙을 추적하고 해당 규칙을 만족할 수 없거나 용량이 부족한 경우 상태 경고 및 오류를 내보냅니다. 예를 들어, 노드가 용량이 초과한 상태이고 클러스터 Resource Manager가 상황을 수정할 수 없는 경우 노드 용량 초과를 나타내고 메트릭에 대한 상태 경고를 내보냅니다.

리소스 관리자가 상태 경고를 발생시키는 또 다른 예제는 배치 제약 조건을 정의(예: "NodeColor == 블루")하고 리소스 관리자가 해당 제약 조건의 위반을 감지한 경우입니다. 자동으로 적용되는 기본 제약 조건(예: 장애 및 업그레이드 도메인 제약 조건)뿐만 아니라 사용자 지정 제약 조건에 이 작업을 수행합니다.

이러한 상태 보고서의 예를 들면 다음과 같습니다. 이 경우에 오류의 문자열이 있는 경우 발생할 수 있는 것처럼 해당 파티션의 복제본이 일시적으로 너무 적은 업그레이드 도메인에 압축되기 때문에 상태 보고서는 시스템 서비스의 파티션 중 하나에 대한 것입니다.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: Node -- 3d1a4a68b2592f55125328cd0f8ed477  Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

상태 메시지는 다음과 같은 사항을 알려줍니다.

1. 모든 복제본은 자체로 정상입니다(서비스 패브릭의 첫 번째 우선 순위임).
2. 업그레이드 도메인 배포 제약 조건은 현재 위반되었습니다(특정 업그레이드 도메인에 이 파티션에 대한 복제본이 있어야 하는 것 보다 많음을 의미함).
3. 위반의 원인이 되는 복제본을 포함하는 노드(해당 노드 ID: 3d1a4a68b2592f55125328cd0f8ed477)
4. 이 모두가 발생한 때(2015년 8월 10일 오후 7:13:02)

오류가 발생하여 살펴봐야 한다는 것을 알 수 있도록 프로덕션에서 발생하는 경고로서 유용한 데이터입니다. 예를 들어 이 경우에 리소스 관리자가 업그레이드 도메인에 복제본을 압축하는 것 이외의 선택이 없다고 여긴 이유를 알아낼 수 있는지 확인하려고 합니다. 다른 업그레이드 도메인 노드의 모든 노드가 중지되고 충분한 다른 예비 도메인이 없었기 때문이었을 수 있습니다. 또는 충분한 도메인이 다른 업그레이드 도메인에 있는 노드가 무효화되게 만든 다른 작업을 수행하고 있는 경우이었을 수 있습니다(예: 서비스의 일부 배치 정책 또는 불충분한 용량).

그러나 서비스를 만들거나 리소스 관리자가 일부 서비스를 배치할 수 있는 위치를 찾으려고 시도함에도 불구하고 해결할 수 있는 방법이 없어 보입니다. 여러 가지 이유가 있을 수 있지만 일반적으로 다음 두 가지 조건 중 하나로 인한 것입니다.

1. 일부 일시적 조건 때문에 서비스 인스턴스 또는 복제본을 올바르게 배치할 수 없습니다.
2. 서비스의 요구 사항은 해당 요구 사항을 충족시킬 수 없는 방법으로 잘못 구성되어 있습니다.

이러한 각 조건에서 진행 상황 및 서비스를 배치할 수 없는 이유를 확인할 수 있는 정보를 제공하는 클러스터 Resource Manager의 상태 보고서가 표시됩니다. 이 프로세스는 "제약 조건 제거 시퀀스"라고 합니다. 이 프로세스를 실행하는 동안 서비스에 영향을 주는 구성된 제약 조건이 설명되고 제거한 항목이 기록됩니다. 이와 같이 서비스를 배치할 수 없는 경우 제거된 노드 및 원인을 확인할 수 있습니다.

## <a name="constraint-types"></a>제약 조건 형식
이러한 상태 보고서에서 확인할 수 있는 다양한 제약 조건 및 상태 보고서가 검사하는 항목을 살펴보겠습니다. 대부분의 경우 기본적으로 제약 조건이 소프트나 최적화 수준이기 때문에 이러한 일부 제약 조건이 제거한 노드가 표시되지 않습니다(이 문서에 제약 조건 우선 순위에 대한 추가 정보가 나와 있음). 그러나 이러한 제약 조건이 하드 제약 조건으로 구성되거나, 드문 경우지만 노드가 제거되도록 하는 경우 이러한 제약 조건과 관련된 상태 메시지가 표시될 수 있으므로, 완전한 설명을 위해 여기에 제공됩니다.

* ReplicaExclusionStatic 및 ReplicaExclusionDynamic – 검색하는 동안 동일한 파티션에 있는 두 개의 상태 저장 복제본 또는 상태 비저장 인스턴스를 동일한 노드에 배치해야 하는(허용되지 않음) 상황임을 나타내는 내부 제약 조건입니다. ReplicaExclusionStatic 및 ReplicaExclusionDynamic은 거의 동일한 규칙입니다. ReplicaExclusionDynamic 제약 조건은 "유일하게 제안된 솔루션이 이미 여기에 복제본을 배치했기 때문에 이 복제본을 여기에 배치할 수 없습니다"라고 합니다. 제안된 충돌이 아닌 실제 충돌임을 나타내는 ReplicaExclusionStatic 제외 목록과 달리 노드에 복제본이 이미 있습니다. 혼란스러운가요? 예. 문제가 됩니까? 아니요. ReplicaExclusionStatic 또는 ReplicaExclusionDynamic 제약 조건이 포함된 제약 조건 제거 시퀀스가 표시되는 경우 클러스터 Resource Manager는 모든 복제본을 배치할 충분한 노드가 없다고 여깁니다. 추가 제약 조건은 일반적으로 처음부터 노드가 너무 적었다는 점을 알려줍니다.
* PlacementConstraint: 이 메시지가 표시되는 경우 서비스의 배치 제약 조건에 일치하지 않는 일부 노드를 제거했음을 의미합니다. 현재 구성된 배치 제약 조건을 이 메시지의 일부분으로 추적합니다. 이러한 메시지는 배치 제약 조건을 제공하는 경우에 일반적으로 나타나지만, 배치 제약 조건에 노드가 너무 많아 제거되지 못하게 하는 버그가 있는 경우 이 메시지를 통해 결과를 볼 수 있습니다.
* NodeCapacity: 이 제약 조건이 표시되면 이 방법이 노드의 용량을 초과할 수 있기 때문에 복제본을 표시된 노드에 배치할 수 없었다는 의미입니다.
* 선호도: 이 제약 조건은 선호도 제약 조건을 위반하기 때문에 영향을 받는 노드에 복제본을 배치할 수 없었다는 것을 나타냅니다.
* FaultDomain & UpgradeDomain: 이 제약 조건은 표시된 노드에 복제본을 배치하여 특정 장애 또는 업그레이드 도메인에서 압축이 일어나는 경우 노드를 제거합니다. 이 제약 조건을 설명하는 몇 가지 예는 [장애 및 업그레이드 도메인 제약 조건 및 결과 동작](service-fabric-cluster-resource-manager-cluster-description.md)
* PreferredLocation: 일반적으로 이 제약 조건을 확인하면 기본적으로 최적화되었기 때문에 솔루션에서 노드를 제거하게 됩니다. 또한 기본 설정된 위치 제약 조건은 일반적으로 업그레이드 중에만 제공되지만(업그레이드를 시작할 때 원래 위치에 그대로 다시 복제본을 이동하는 데 사용되는 경우) 가능합니다.

### <a name="constraint-priorities"></a>제약 조건 우선 순위
이러한 모든 제약 조건에서 다음과 같이 생각했을 수 있습니다. "내 시스템에서 배치 제약 조건이 가장 중요하다고 생각합니다. 배치 제약 조건이 위반되지 않는 다는 것을 보장한다면 심지어 선호도 및 용량과 같은 다른 제약 조건을 위반하겠습니다."

가능한 것으로 판명됩니다! 제약 조건은 몇 가지 다른 수준의 적용으로 구성될 수 있지만 “hard”(0), “soft”(1), “optimization”(2) 및 “off”(-1)가 됩니다. 기본적으로 hard로 정의한 대부분의 제약 조건(예를 들어 대부분의 사람들은 일반적으로 용량을 완화할 것으로 생각하지 않기 때문) 및 거의 모두는 hard 또는 soft입니다. 그러나 고급 시나리오에서는 변경될 수 있습니다. 예를 들어 노드 용량 문제를 해결하기 위해 선호도를 항상 위반하도록 하려는 경우 선호도 제약 조건의 우선 순위를 "Soft"(1)로 지정하고 용량 제약 조건을 "Hard"(0)로 설정된 상태로 둘 수 있습니다. 다양한 제약 조건 우선 순위는 일부 제약 조건 위반 경고가 다른 것보다 더 자주 표시되는 이유입니다. 일시적으로 완화(위반)하려는 특정 제약 조건이 있기 때문입니다. 이러한 수준은 지정된 제약 조건이 항상 위반 또는 위반되지 않는 것을 의미하지 않습니다. 모두를 만족시킬 수 없는 경우 올바르게 균형을 유지할 수 있도록 우선적으로 적용되는 순서가 있는 것입니다.

다른 제약 조건에 대한 구성 및 기본 우선 순위 값은 아래에 나열되어 있습니다.

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

업그레이드 및 장애 도메인에 대해 정의된 제약 조건이 있으며 업그레이드 도메인 제약 조건이 Soft임을 여기에서 알 수 있습니다. 또한 우선 순위에 이 이상한 "PreferredLocation" 제약 조건이 있습니다. 이러한 것들은 모두 무엇입니까?

먼저 리소스 관리자의 엔진 내에서 제약 조건으로 장애 및 업그레이드 도메인 간에 분산되는 서비스를 유지하고자 원하는 것을 모델링합니다. 지금까지 장애 및 업그레이드 도메인과 관련한 배치에 대해 엄격해야 했던 경우와 완전히 무시해야 했던 일부 문제가 있었던 경우가 몇 번 있었습니다(실제로 간단하게!). 따라서 해당 디자인 선택 및 제약 조건 인프라의 유연성에 감사합니다. 대부분의 경우 업그레이드 도메인 제약 조건은 soft 제약 조건입니다. 즉, 리소스 관리자가 일시적으로 진행할 업그레이드 또는 다양한 동시 실패 또는 다른 제약 조건 위반을 처리하기 위해 몇 가지 복제본을 업그레이드 도메인에 압축해야 하는 경우(hard 제약 조건에서) 문제없습니다. 올바른 배치를 방지하는 시스템에 오류 또는 기타 이탈이 많지 않는 한 이는 일반적으로 발생하지 않습니다. 환경이 올바르게 구성된 경우 안정적인 상태는 항상 업그레이드 도메인이 완전히 적용되는 상태입니다.

PreferredLocation 제약 조건은 약간 다르기 때문에 "Optimization"으로 설정된 유일한 제약 조건입니다. 업그레이드 전에 찾은 위치로 서비스를 다시 배치하기 위해 업그레이드가 진행 중인 동안 이 제약 조건을 사용합니다. 실제로 작동하지 않는 모든 종류의 이유가 있지만 훌륭한 최적화이므로 여기에 있습니다. 클러스터 Resource Manager가 업그레이드를 지원하는 방법에 대해 논의할 때 자세히 설명하겠습니다.

## <a name="upgrades"></a>업그레이드
또한 클러스터 Resource Manager는 응용 프로그램 및 클러스터 업그레이드 도중에 업그레이드를 원활하게 할 뿐만 아니라 클러스터의 규칙 및 성능이 손상되지 않도록 도움을 줍니다.

### <a name="keep-enforcing-the-rules"></a>규칙 적용 유지
알고 있어야 하는 중요한 점은 배치 제약 조건과 같은 사항의 엄격한 제약 조건이 업그레이드하는 동안 계속 적용된다는 것입니다. 언급하지 않아도 당연하지만 명시적으로 밝혀둡니다. 긍정적인 면은 해당 특정 워크로드가 특정 노드에서 실행되지 않도록 하려는 경우 이를 확인할 수 있다는 점입니다. 해당 규칙은 업그레이드 중에도 자동으로 적용됩니다. 환경이 매우 제한된 경우 업데이트를 위해 서비스(또는 서비스가 있는 노드)를 중지해야 한다면 서비스가 이동될 수 있는 위치 옵션이 극소수로 한정되기 때문에 업그레이드하는 데 시간이 오래 걸릴 수 있습니다.

### <a name="smart-replacements"></a>스마트 대체
업그레이드를 시작할 때 리소스 관리자는 클러스터의 현재 정렬에 대한 스냅숏을 만들고 업그레이드가 완료되면 해당 상태로 작업을 반환하려고 합니다. 이 원인은 간단합니다. 우선 업그레이드를 목적으로 각 서비스 복제본 또는 인스턴스에 대한 몇 가지 변환이 필요합니다(영향을 받은 노드에서 벗어나서 다시 들어옴). 다음으로 업그레이드 자체가 클러스터의 레이아웃에 크게 영향을 주지 않도록 합니다. 업그레이드하기 전에 클러스터가 잘 정렬된 경우 이후에도 잘 정렬됩니다. 또는 적어도 나빠지지 않습니다.

### <a name="reduced-churn"></a>이탈 감소
업그레이드하는 동안 발생하는 다른 작업은 클러스터 리소스 관리자가 업그레이드된 엔터티에 대한 부하 분산을 해제하는 것입니다. 따라서 두 개의 응용 프로그램 인스턴스를 가지고 그 중 하나에서 업그레이드를 시작하면 해당 응용 프로그램 인스턴스에 대한 부하 분산이 일시 중지되지만 다른 응용 프로그램 인스턴스에는 영향을 주지 않습니다. 반응 부하 분산 방지는 업그레이드 자체에 대한 불필요한 반응을 방지합니다.(“이런! 빈 노드입니다. 모든 종류의 항목으로 채우는 것이 좋습니다.”) 그리고 업그레이드가 완료된 후에 서비스가 노드로 다시 이동해야 하는 경우 실행을 취소한 클러스터의 서비스에 대한 추가 이동을 결과적으로 많이 방지합니다. 문제가 되는 업그레이드가 클러스터 업그레이드인 경우 전체 클러스터는 업그레이드 기간에 부하 분산을 위해 일시 중지됩니다(제약 조건 검사 – 규칙이 적용되도록 함 – 활성 상태로 유지, 사전 균형 재조정만 사용되지 않음).

### <a name="relaxed-rules"></a>관대한 규칙
업그레이드 하는 동안 발생하는 한 가지 문제는 일반적으로 클러스터가 전체적으로 다소 제한되거나 가득 찬 경우라도 업그레이드를 완료하려 할 경우입니다. 클러스터까지 업그레이드가 진행되고 일반적으로 워크로드가 다른 위치로 이동되어야 하므로, 한 번에 용량의 5~20%가 감소합니다. 따라서 업그레이드하는 동안 클러스터의 용량을 관리할 수 있는 기능이 평소보다 더 중요합니다. 이 경우에 [버퍼링된 용량](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity)의 개념이 실제로 발휘됩니다. 버퍼링된 용량이 정상적인 작업(일부 오버헤드 허가) 적용되는 동안 클러스터 Resource Manager는 업그레이드 중에 전체 용량(버퍼 사용)을 가득 채우게 됩니다.

## <a name="next-steps"></a>다음 단계
* 처음부터 시작 및 [서비스 패브릭 클러스터 Resource Manager 소개](service-fabric-cluster-resource-manager-introduction.md)




<!--HONumber=Nov16_HO3-->


