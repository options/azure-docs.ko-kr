---
title: "OMS(Operations Management Suite)의 관리 솔루션 만들기 | Microsoft Docs"
description: "관리 솔루션은 고객이 OMS 작업 영역에 추가할 수 있는 패키지 관리 시나리오를 제공하여 OMS(Operations Management Suite)의 기능을 확장합니다.  이 문서에서는 자체 환경에 사용할 관리 솔루션 또는 고객에게 제공할 관리 솔루션을 만드는 방법에 대해 자세히 설명합니다."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: ab33a7610b8e7bbf64e9f1bfde3753f95956a82f


---
# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>OMS(Operations Management Suite)(Preview)의 관리 솔루션 만들기
> [!NOTE]
> 현재 Preview로 제공되는 OMS의 사용자 지정 솔루션 만들기에 대한 예비 설명서입니다. 아래 설명된 스키마는 변경될 수 있습니다.  
>
>

관리 솔루션은 고객이 OMS 작업 영역에 추가할 수 있는 패키지 관리 시나리오를 제공하여 OMS(Operations Management Suite)의 기능을 확장합니다.  이 문서에서는 자체 환경에 사용할 수 있는 자체 관리 솔루션 또는 커뮤니티를 통해 고객에게 제공할 수 있는 관리 솔루션을 만드는 방법에 대해 자세히 설명합니다.

## <a name="planning-your-management-solution"></a>관리 솔루션 계획
OMS의 관리 솔루션에는 특정 관리 시나리오를 지원하는 여러 리소스가 포함됩니다.  솔루션을 계획할 때 달성하려는 시나리오 및 관리 시나리오를 지원하는 데 필요한 모든 리소스에 중점을 두어야 합니다.  하나 이상의 리소스를 다른 솔루션에서도 정의하더라도 각 솔루션은 필요한 각 리소스를 자체적으로 포함하고 정의해야 합니다.  관리 솔루션이 설치되면 이미 있는 리소스를 제외한 각 리소스가 생성되고, 사용자는 솔루션이 제거될 때 리소스에 발생하는 일을 정의할 수 있습니다.  

예를 들어 [일정](../automation/automation-schedules.md) 및 [보기](../log-analytics/log-analytics-view-designer.md)를 사용하여 Log Analytics 리포지토리에 데이터를 수집하고 수집된 데이터의 다양한 시각화를 제공하는 [Azure Automation runbook](../automation/automation-intro.md)이 관리 솔루션에 포함될 수 있습니다.  똑같은 일정이 다른 솔루션에서 사용될 수 있습니다.  개발자는 관리 솔루션 작성자로서 세 리소스를 모두 정의하지만 솔루션이 제거되면 runbook과 보기가 자동으로 제거되도록 지정해야 합니다.    또한 일정을 정의하지만 다른 솔루션에서 여전히 똑같은 일정을 사용하는 경우를 대비하여 솔루션이 제거되어도 일정이 계속 작동하도록 지정해야 합니다.

## <a name="management-solution-files"></a>관리 솔루션 파일
관리 솔루션은 [리소스 관리 템플릿](../azure-resource-manager/resource-manager-template-walkthrough.md)으로 구현됩니다.  관리 솔루션을 작성하는 방법에서 주요 작업은 [템플릿 작성](../azure-resource-manager/resource-group-authoring-templates.md)입니다.  이 문서에서는 솔루션에 사용되는 템플릿의 고유한 세부 정보와 일반적인 솔루션 리소스를 정의하는 방법을 설명합니다.

관리 솔루션 파일의 기본 구조는 다음과 같은 [리소스 관리자 템플릿](../azure-resource-manager/resource-group-authoring-templates.md#template-format)과 같습니다.  이어지는 각 섹션에서는 솔루션의 최상위 수준 요소와 그 콘텐츠에 대해 설명합니다.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>parameters
[매개 변수](../azure-resource-manager/resource-group-authoring-templates.md#parameters)는 사용자가 관리 솔루션을 설치할 때 사용자에게 요구할 값입니다.  모든 솔루션에 포함될 표준 매개 변수가 있고, 특정 솔루션에 필요한 다른 매개 변수를 추가할 수 있습니다.  사용자가 솔루션을 설치할 때 매개 변수 값을 제공하는 방법은 솔루션 설치 방법과 특정 매개 변수에 따라 다릅니다.

사용자가 [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) 또는 [Azure 빠른 시작 템플릿](operations-management-suite-solutions.md#finding-and-installing-management-solutions)을 통해 관리 솔루션을 설치할 경우에는 [OMS 작업 영역 및 Automation 계정](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account)을 선택하라는 메시지가 표시됩니다.  이러한 템플릿은 각 표준 매개 변수의 값을 채우는 데 사용됩니다.  사용자에게는 표준 매개 변수의 값을 직접 제공하라는 메시지가 표시되지 않고 추가 매개 변수의 값을 제공하라는 메시지가 표시됩니다.

사용자가 솔루션을 [다른 방법](operations-management-suite-solutions.md#finding-and-installing-management-solutions)으로 설치할 경우에는 모든 표준 매개 변수와 모든 추가 매개 변수의 값을 제공해야 합니다.

샘플 매개 변수는 다음과 같습니다.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

다음 표에서는 매개 변수의 특성을 설명합니다.

| 특성 | 설명 |
|:--- |:--- |
| type |매개 변수의 데이터 형식입니다. 사용자에게 표시되는 입력 컨트롤은 데이터 형식에 따라 다릅니다.<br><br>bool - 드롭다운 상자<br>string - 텍스트 상자<br>int - 텍스트 상자<br>securestring - 암호 필드<br> |
| 카테고리 |매개 변수의 선택적 범주입니다.  같은 범주의 매개 변수는 함께 그룹화됩니다. |
| control |string 매개 변수의 추가 기능입니다.<br><br>datetime - Datetime 컨트롤이 표시됩니다.<br>guid - Guid 값이 자동으로 생성되고 매개 변수가 표시되지 않습니다. |
| 설명 |매개 변수에 대한 선택적 설명입니다.  매개 변수 옆에 정보 풍선으로 표시됩니다. |

### <a name="standard-parameters"></a>표준 매개 변수
다음 표에는 모든 관리 솔루션에 대한 표준 매개 변수가 나열됩니다.  솔루션이 Azure Marketplace 또는 빠른 시작 템플릿에서 설치될 경우 매개 변수에 대한 메시지가 표시되지 않고 이러한 값이 자동으로 채워집니다.  솔루션이 다른 방법으로 설치될 경우 사용자가 값을 입력해야 합니다.

> [!NOTE]
> Azure Marketplace 및 빠른 시작 템플릿의 사용자 인터페이스에는 테이블의 매개 변수 이름을 사용해야 합니다.  다른 매개 변수 이름을 사용하면 사용자에게 매개 변수에 대한 메시지가 표시되고 매개 변수가 자동으로 채워지지 않습니다.
>
>

| 매개 변수 | 형식 | 설명 |
|:--- |:--- |:--- |
| accountName |string |Azure Automation 계정 이름입니다. |
| pricingTier |string |Log Analytics 작업 영역 및 Azure Automation 계정의 가격 책정 계층입니다. |
| regionId |string |Azure Automation 계정의 지역입니다. |
| solutionName |string |솔루션의 이름입니다. |
| workspaceName |string |Log Analytics 작업 영역 이름입니다. |
| workspaceRegionId |string |Log Analytics 작업 영역의 지역입니다. |

### <a name="sample"></a>샘플
솔루션에 대한 샘플 매개 변수 엔터티는 다음과 같습니다.  여기에는 모든 표준 매개 변수와 같은 범주에 있는 두 개의 추가 매개 변수가 포함됩니다.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


**parameters('매개 변수 이름')** 구문을 사용하여 솔루션의 다른 요소에 있는 매개 변수 값을 참조할 수 있습니다.  예를 들어 작업 영역 이름에 액세스하려면 **parameters('workspaceName')**을 사용합니다.

## <a name="variables"></a>변수
**Variables** 요소는 관리 솔루션의 나머지 부분에 사용할 값을 포함합니다.  이러한 값은 솔루션을 설치하는 사용자에게 노출되지 않습니다.  작성자가 솔루션을 만드는 동안 여러 번 사용할지도 모르는 값을 관리할 수 있는 단일 위치를 제공하는 것이 이러한 값의 목적입니다. 솔루션 관련 값을 **resources** 요소로 하드 코드하지 않고 해당 값을 변수에 포함해야 합니다.  이렇게 하면 코드를 더 쉽게 읽을 수 있고 이후 버전에서 이들 값을 쉽게 변경할 수 있습니다.

다음은 솔루션에 일반적인 매개 변수를 사용한 **variables** 요소의 예입니다.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

**variables('변수 이름')** 구문을 사용하여 솔루션 전체의 변수 값을 참조할 수 있습니다.  예를 들어 SolutionName 변수에 액세스하려면 **variables('solutionName')**을 사용합니다.

## <a name="resources"></a>리소스
**resources** 요소는 관리 솔루션에 포함된 여러 리소스를 정의합니다.  이번 파트가 아마도 템플릿에서 가장 크고 복잡한 내용일 것입니다.  리소스는 다음과 같은 구조를 사용하여 정의됩니다.  

    "resources": [
        {
            "name": "<name-of-the-resource>",            
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",        
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>종속성
**dependsOn** 요소는 다른 리소스에 대한 [종속성](../azure-resource-manager/resource-group-define-dependencies.md)을 지정합니다.  솔루션이 설치될 때 모든 리소스의 종속성이 만들어진 후에야 리소스가 만들어지지 않습니다.  예를 들어 솔루션이 [작업 리소스](operations-management-suite-solutions-resources-automation.md#automation-jobs)를 사용하여 설치될 경우 솔루션에서 [Runbook을 시작](operations-management-suite-solutions-resources-automation.md#runbooks)할 수 있습니다.  작업이 만들어지기 전에 runbook이 만들어지도록 작업 리소스는 runbook 리소스에 종속됩니다.

### <a name="oms-workspace-and-automation-account"></a>OMS 작업 영역 및 Automation 계정
관리 솔루션은 뷰를 포함하는 [OMS 작업 영역](../log-analytics/log-analytics-manage-access.md)과 Runbook 및 관련 리소스를 포함하는 [Automation 계정](../automation/automation-security-overview.md#automation-account-overview)이 필요합니다.  이러한 항목은 솔루션의 리소스가 만들어지기 전에 제공되어야 하며 솔루션 자체에 정의될 수 없습니다.  사용자는 솔루션을 배포할 때 [작업 영역과 계정을 지정](operations-management-suite-solutions.md#oms-workspace-and-automation-account)하지만, 작성자는 다음 사항을 고려해야 합니다.

## <a name="solution-resource"></a>솔루션 리소스
각 솔루션은 솔루션 자체를 정의하는 요소인 **resources** 요소에 리소스 항목이 필요합니다.  이 항목의 형식은 **Microsoft.OperationsManagement/solutions**가 됩니다.  솔루션 리소스의 예는 다음과 같습니다.  아래 섹션에는 여러 가지 요소가 설명되어 있습니다.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>솔루션 이름
솔루션 이름은 Azure 구독에서 고유해야 합니다. 권장 사용 값은 다음과 같습니다.  이 요소는 기본 이름에 대한 **SolutionName** 변수와 이름이 고유한지 확인하기 위한 **workspaceName** 매개 변수를 사용합니다.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

이 요소는 다음과 같이 이름을 확인합니다.

    My Solution Name [MyWorkspace]


### <a name="dependencies"></a>종속성
솔루션이 만들어지기 전에 솔루션 리소스가 있어야 하므로 솔루션 리소스는 솔루션의 모든 다른 리소스에 대해 [종속성](../azure-resource-manager/resource-group-define-dependencies.md)을 가져야 합니다.  **dependsOn** 요소에서 각 리소스의 항목을 추가하면 됩니다.

### <a name="properties"></a>속성
솔루션 리소스는 테이블의 속성을 가집니다.  여기에는 솔루션 설치 후 리소스 관리 방식을 정의하는 솔루션에서 참조 및 포함하는 리소스가 포함됩니다.  솔루션의 각 리소스는 **referencedResources** 또는 **containedResources** 속성에 나열되어야 합니다.

| 속성 | 설명 |
|:--- |:--- |
| workspaceResourceId |OMS 작업 영역의 ID이며 *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<작업 영역 이름\>*형식입니다. |
| referencedResources |솔루션을 제거해도 함께 제거되면 안 되는 솔루션의 리소스 목록입니다. |
| containedResources |솔루션을 제거하면 함께 제거되어야 하는 솔루션의 리소스 목록입니다. |

위의 예제는 runbook, 일정, 보기가 포함된 솔루션과 관련됩니다.  일정 및 runbook은 **properties** 요소에서 *참조*되므로 솔루션이 제거될 때 제거되지 않습니다.  보기는 *포함*되어 있으므로 솔루션을 제거하면 함께 제거됩니다.

### <a name="plan"></a>계획
솔루션 리소스의 **plan** 엔터티는 테이블의 속성을 가집니다.

| 속성 | 설명 |
|:--- |:--- |
| name |솔루션의 이름입니다. |
| 버전 |솔루션 버전은 작성자가 결정합니다. |
| product |솔루션을 식별하는 고유 문자열입니다. |
| publisher |솔루션의 게시자입니다. |

## <a name="other-resources"></a>기타 리소스
관리 솔루션에 공통적으로 적용되는 리소스의 세부 정보 및 샘플은 다음 문서에서 얻을 수 있습니다.

* [보기 및 대시보드](operations-management-suite-solutions-resources-views.md)
* [Automation 리소스](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>관리 솔루션 테스트
관리 솔루션을 배포하기 전에 [Test-AzureRmResourceGroupDeployment](../azure-resource-manager/resource-group-template-deploy.md#deploy)를 사용하여 테스트하는 것이 좋습니다.  솔루션을 배포하기 전에 솔루션 파일의 유효성을 검사하고 문제를 식별할 수 있습니다.

## <a name="next-steps"></a>다음 단계
* [Azure Resource Manager 템플릿 작성](../azure-resource-manager/resource-group-authoring-templates.md)에 대해 자세히 알아봅니다.
* [Azure 빠른 시작 템플릿](https://azure.microsoft.com/documentation/templates)에서 다양한 Resource Manager 템플릿 샘플을 검색합니다.
* [관리 솔루션에 보기를 추가](operations-management-suite-solutions-resources-views.md)하는 방법을 자세히 살펴봅니다.
* [관리 솔루션에 Automation 리소스를 추가](operations-management-suite-solutions-resources-automation.md)하는 방법을 자세히 살펴봅니다.



<!--HONumber=Jan17_HO1-->


