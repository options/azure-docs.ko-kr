---
title: "Azure 외부 서비스 요금의 이해 | Microsoft Docs"
description: "이전에는 마켓플레이스로 알려진 외부 서비스의 요금 청구, Azure의 요금에 대해 알아봅니다."
services: 
documentationcenter: 
author: adpick
manager: ruchic
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2016
ms.author: adpick
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 45e0d553179b63de2d0664314271472f1151d73f


---
# <a name="understand-your-azure-external-service-charges"></a>Azure 외부 서비스 요금의 이해
이 문서에서는 Azure에서 외부 서비스의 요금 청구를 설명합니다. 외부 서비스는 마켓플레이스 주문으로 불렸습니다. 외부 서비스는 독립 서비스 공급 업체에 의해 제공되지만 Azure 에코시스템 내에서 완전히 통합됩니다. 방법 배우기:

* 외부 서비스 식별
* 요금 청구가 다른 Azure 리소스와 어떻게 다른지 이해
* 외부 서비스의 사용에서 축적한 모든 비용 확인 및 추적
* 외부 서비스 주문 및 지불 방법 관리

## <a name="what-are-azure-external-services"></a>Azure 외부 서비스는 무엇입니까?
외부 서비스는 Azure Marketplace로 불렸습니다. 일반적으로 Azure에 대해 사용 가능한 제 3자에 의해 게시된 서비스입니다. 예를 들어 ClearDB 및 SendGrid는 Azure에서 구입할 수 있지만 Microsoft에서 게시되지 않은 외부 서비스입니다.

### <a name="identify-external-services"></a>외부 서비스 식별
새 외부 서비스 또는 리소스를 프로비전하면 다음 경고가 표시됩니다.

![마켓플레이스 구매 경고](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> 외부 서비스는 Microsoft가 아닌 회사에 의해 게시되지만 때때로 Microsoft 제품도 외부 서비스로 분류됩니다.
> 
> 

### <a name="external-services-are-billed-separately"></a>외부 서비스는 별도로 청구됩니다.
외부 서비스는 Azure 구독 내에서 개별 주문으로 처리됩니다. 각 서비스에 대한 청구 기간은 서비스를 구입할 때 설정됩니다. 구매한 구독의 청구 기간과 혼동되지 않습니다. 또한 별도의 청구를 받으며 신용 카드는 별도로 청구됩니다.

### <a name="each-external-service-has-a-different-billing-model"></a>각 외부 서비스에는 다른 청구 모델이 있습니다.
일부 서비스는 종량제 방식으로 청구되는 반면 다른 서비스는 월별 기반 지불 모델을 사용합니다. Azure 외부 서비스에 대한 신용 카드가 필요하며 청구서 지불로 외부 서비스를 구입할 수 없습니다.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>외부 서비스에 대한 월별 무료 크레딧을 사용할 수 없습니다.
[무료 크레딧](https://azure.microsoft.com/pricing/spending-limits/)을 포함하는 Azure 구독을 사용하는 경우 외부 서비스 청구에 적용될 수 없습니다. 신용 카드를 사용하여 외부 서비스를 구입합니다.

## <a name="view-external-service-spending-and-history"></a>외부 서비스 지출 및 기록 보기
[Azure 포털](https://portal.azure.com/) 내에서 각 구독에 있는 외부 서비스의 목록을 볼 수 있습니다. 

1. 계정 관리자 권한으로 [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. 허브 메뉴에서 **구독**을 선택합니다.
   
    ![허브 메뉴에서 구독 선택](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. **구독** 블레이드에서 보려는 구독을 선택하고 **외부 서비스**를 선택합니다.
   
    ![청구 블레이드에서 구독 선택](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. 각 외부 서비스 주문, 게시자 이름, 구입한 서비스 계층, 리소스를 지정한 이름 및 현재 주문 상태가 표시됩니다. 외부 서비스를 선택하여 과거 청구서를 봅니다.
   
    ![외부 서비스 선택](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. 여기에서 세금 분석 결과를 포함한 과거 청구 금액을 볼 수 있습니다.
   
    ![외부 서비스 청구 내역 보기](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>외부 서비스 주문에 대한 지불 방법 관리
[계정 센터](https://account.windowsazure.com/)에서 외부 서비스 주문에 대한 결제 방법을 업데이트합니다.

> [!NOTE]
> 회사 또는 학교 계정으로 구독을 구매한 경우 [지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하여 지불 방법을 변경해야 합니다.
> 
> 

1. [계정 센터](https://account.windowsazure.com/)에 로그인하고 [**마켓플레이스** 탭으로 이동](https://account.windowsazure.com/Store)
   
    ![계정 센터에서 마켓플레이스 선택](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. 관리하려는 외부 서비스 선택
   
    ![관리하려는 외부 서비스 선택](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. 페이지의 오른쪽에서 **지불 방법 변경**을 클릭합니다. 이 링크를 통해 지불 방법을 관리하는 다른 포털로 이동합니다.
   
    ![주문 요약](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. **정보 편집**을 클릭하고 지침을 따라 결제 정보를 업데이트합니다.
   
    ![정보 편집 선택](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>외부 서비스 주문 취소
외부 서비스 주문을 취소하려는 경우 [Azure 포털](https://portal.azure.com)에서 리소스를 삭제해야 합니다.

![리소스 삭제](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>도움이 필요하세요? 지원에 문의하세요.
계속해서 다른 질문이 있는 경우 [지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하여 문제를 신속하게 해결하세요.




<!--HONumber=Nov16_HO3-->


