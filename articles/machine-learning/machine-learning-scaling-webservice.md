---
title: "웹 서비스 확장 | Microsoft Docs"
description: "동시성을 늘리고 새 끝점을 추가하여 웹 서비스를 확장하는 방법을 알아봅니다."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "azure 기계 학습, 웹 서비스, 조작화, 확장, 끝점, 동시성"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/05/2016
ms.author: neerajkh
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 559db01d2c9f44af17b86f99031a8827dfeb5ab3


---
# <a name="scaling-a-web-service"></a>웹 서비스 확장
> [!NOTE]
> 이 토픽에서는 기존 Machine Learning 웹 서비스에 적용되는 기술을 설명합니다. 
> 
> 

기본적으로 게시된 각각의 웹 서비스는 20개의 동시 요청을 지원하고 최대 200개의 동시 요청을 지원할 수 있도록 구성됩니다. Azure 클래식 포털은 이 값을 설정하는 방법을 제공하지만 Azure Machine Learning은 웹 서비스에 대한 최상의 성능을 제공하기 위해 설정을 자동으로 최적화하고 포털 값은 무시됩니다. 

최대 동시 호출 값인 200에서 지원하는 것보다 많은 부하를 가진 API를 호출하려는 경우 동일한 웹 서비스에서 여러 끝점을 만들어야 합니다. 그런 다음 모든 끝점에 부하를 무작위로 분산해야 합니다.

## <a name="add-new-endpoints-for-same-web-service"></a>동일한 웹 서비스에 대한 새 끝점 추가
웹 서비스의 크기를 조정하는 것은 일반적인 작업입니다. 크기를 조정하는 몇 가지 이유는 200개 이상의 동시 요청을 지원하거나, 여러 끝점을 통해 가용성을 높이거나, 웹 서비스에 대한 별도의 끝점을 제공하기 위해서 입니다. [Azure 클래식 포털](https://manage.windowsazure.com/) 또는 [Azure Machine Learning 웹 서비스](https://services.azureml.net/) 포털을 통해 동일한 웹 서비스에 대한 추가 끝점을 추가하여 규모를 증가시킬 수 있습니다.

새로운 끝점 추가에 대한 자세한 내용은 [끝점 만들기](machine-learning-create-endpoint.md)를 참조하세요.

해당하는 높은 속도로 API를 호출하지 않을 경우 매우 높은 동시성 개수를 사용하는 것은 바람직하지 않습니다. 높은 부하로 구성된 API에 상대적으로 낮은 부하를 배치할 경우 가끔 시간 초과 및/또는 대기 시간 급증이 나타날 수 있습니다.

동기 API는 일반적으로 낮은 대기 시간을 원하는 경우에 사용됩니다. 여기서 대기 시간은 API가 하나의 요청을 완료하는 데 걸리는 시간을 의미하며 네트워크 지연을 고려하지 않습니다. 대기 시간이 50ms인 API가 있다고 가정합시다. 높음 제한 수준 및 최대 동시 호출 = 20으로 사용 가능한 용량을 완전히 사용하려면 이 API를 초당 20 * 1000 / 50 = 400회 호출해야 합니다. 더욱 확장하여 최대 동시 호출 수를 200으로 설정하면 대기 시간이 50ms일 경우 API를 초당 4000회 호출할 수 있습니다.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png



<!--HONumber=Nov16_HO3-->


