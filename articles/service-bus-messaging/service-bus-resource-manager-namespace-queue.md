---
title: "Azure Resource Manager 템플릿을 사용하여 큐가 있는 Service Bus 네임스페이스 만들기 | Microsoft Docs"
description: "Azure Resource Manager 템플릿을 사용하여 서비스 버스 네임스페이스 및 큐 만들기"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/14/2016
ms.author: sethm;shvija
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 71e11a2279350236e0f65ac0b310034fb464d71a


---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Azure Resource Manager 템플릿을 사용하여 서비스 버스 네임스페이스 및 큐 만들기
이 문서에서는 서비스 버스 네임스페이스 및 큐를 만드는 Azure Resource Manager 템플릿을 사용하는 방법을 보여 줍니다. 어떤 리소스를 배포할지 정의하는 방법 및 배포를 실행할 때 매개 변수를 지정하는 방법을 알게 됩니다. 배포를 위해 이 템플릿을 사용하거나 요구 사항에 맞게 사용자 지정을 할 수 있습니다.

템플릿을 만드는 더 자세한 내용은 [Azure Resource Manager 템플릿 작성][Azure Resource Manager 템플릿 작성]를 참조하세요.

전체 템플릿은 GitHub에서 [서비스 버스 네임스페이스 및 큐 템플릿][서비스 버스 네임스페이스 및 큐 템플릿]을 참조하세요.

> [!NOTE]
> 다음 Azure Resource Manager 템플릿은 다운로드하여 배포할 수 있습니다.
> 
> * [큐 및 권한 부여 규칙이 있는 서비스 버스 네임스페이스 만들기](service-bus-resource-manager-namespace-auth-rule.md)
> * [토픽 및 구독이 있는 서비스 버스 네임스페이스 만들기](service-bus-resource-manager-namespace-topic.md)
> * [서비스 버스 네임스페이스 만들기](service-bus-resource-manager-namespace.md)
> * [토픽, 구독 및 규칙이 있는 Service Bus 네임스페이스 만들기](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> 최신 템플릿을 확인하려면 "Service Bus"에 대한 [Azure 빠른 시작 템플릿][Azure 빠른 시작 템플릿] 갤러리 및 검색을 방문하세요.
> 
> 

## <a name="what-will-you-deploy"></a>배포할 항목
이 템플릿으로 큐가 있는 서비스 버스 네임스페이스를 배포합니다.

[Service Bus 큐](service-bus-queues-topics-subscriptions.md#queues)는 하나 이상의 경쟁 소비자에게 FIFO(선입선출) 메시지 배달을 제공합니다.

배포를 자동으로 실행하려면 다음 단추를 클릭합니다.

[![Azure에 배포](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>매개 변수
Azure 리소스 관리자와 함께 템플릿을 배포할 때 지정하고자 하는 값으로 매개 변수를 정의합니다. 템플릿은 모든 매개 변수 값이 포함된 `Parameters` 라는 섹션을 포함합니다. 배포하는 프로젝트에 따라 또는 환경에 따라 달라지는 이러한 값에 대한 매개 변수를 정의해야 합니다. 항상 동일하게 유지되는 값으로 매개 변수를 정의하지 마십시오. 각 매개 변수 값은 배포되는 리소스를 정의하는 템플릿에 사용됩니다.

템플릿은 다음 매개 변수를 정의합니다.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
만들 서비스 버스 네임스페이스 이름입니다.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
서비스 버스 네임스페이스에서 만든 큐의 이름입니다.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion
템플릿의 서비스 버스 API 버전입니다.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>배포할 리소스
큐가 있는 **메시징**형식의 표준 서비스 버스 네임스페이스를 만듭니다.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>배포 실행 명령
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>다음 단계
이제 Azure Resource Manager를 사용하여 리소스를 만들고 배포했으므로 다음 문서를 참조하여 이러한 리소스를 관리하는 방법에 대해 알아봅니다.

* [PowerShell을 사용하여 서비스 버스 관리](service-bus-powershell-how-to-provision.md)
* [서비스 버스 탐색기로 서비스 버스 리소스 관리](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

[Azure Resource Manager 템플릿 작성]: ../resource-group-authoring-templates.md
[서비스 버스 네임스페이스 및 큐 템플릿]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure 빠른 시작 템플릿]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[서비스 버스 큐에 대해 자세한 알아봅니다.]: service-bus-queues-topics-subscriptions.md
[Azure Resource Manager로 Azure PowerShell 사용]: ../powershell-azure-resource-manager.md
[Azure 리소스 관리에서 Mac, Linux 및 Windows용 Azure CLI 사용]: ../xplat-cli-azure-resource-manager.md



<!--HONumber=Nov16_HO3-->


