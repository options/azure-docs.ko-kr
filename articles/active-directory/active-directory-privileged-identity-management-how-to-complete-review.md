---
title: "액세스 검토를 완료하는 방법 | Microsoft Docs"
description: "Azure AD Privileged Identity Management에서 액세스 검토를 시작한 후, 그것을 완료하고 결과를 보는 방법을 알아봅니다."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/30/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a5a9d2aeb5d9d13d0561e21ea90160511659d6a3


---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management의 액세스 검토를 완료하는 방법
[보안 검토가 시작](active-directory-privileged-identity-management-how-to-start-security-review.md)된 후에 권한 있는 역할 관리자가 권한이 있는 액세스를 검토할 수 있습니다. Azure AD PIM(Privileged Identity Management)이 사용자에게 해당 액세스를 검토하도록 요청하는 메일을 자동으로 전송합니다. 사용자가 메일을 받지 못한 경우 [보안 검토를 수행하는 방법](active-directory-privileged-identity-management-how-to-perform-security-review.md)의 지침을 보낼 수 있습니다.

보안 검토 기간이 끝나거나 모든 사용자가 자체 검토를 완료하면 이 문서의 단계에 따라 검토를 관리하고 결과를 표시합니다.

## <a name="manage-security-reviews"></a>보안 검토 관리
1. [Azure 포털](https://portal.azure.com/) 로 이동하고 대시보드에서 **Azure AD Privileged Identity Management** 응용 프로그램을 선택합니다.
2. 대시보드의 **액세스 검토** 섹션을 선택합니다.
3. 관리하려는 액세스 검토를 선택합니다.

액세스 검토의 세부 정보 블레이드에는 검토를 관리하는 몇 가지 옵션이 있습니다.

![PIM 액세스 검토 단추-스크린 샷][1]

### <a name="remind"></a>알림
액세스 검토가 설정되어 사용자가 자체적으로 검토하는 경우 **알림** 단추가 알림을 보냅니다. 

### <a name="stop"></a>중지
모든 액세스 검토는 종료 날짜가 있지만, **중지** 단추를 사용하여 일찍 완료할 수 있습니다. 만일 이 때까지 검토하지 않은 사용자가 있다면, 검토를 중지한 후에는 그들을 검토할 수 없습니다. 검토를 중단한 후에는 다시 시작할 수 없습니다.

### <a name="apply"></a>적용
종료 날짜가 되었거나 수동으로 중지하여 액세스 검토가 완료되면, **적용** 단추가 검토 결과를 구현합니다. 사용자의 액세스가 검토 중에 거부되었다면, 이는 그들의 역할 할당을 제거할 단계입니다.  

### <a name="export"></a>내보내기
보안 검토 결과를 수동으로 적용하려는 경우 검토를 내보낼 수 있습니다. **내보내기** 단추는 CSV 파일 다운로드를 시작합니다. Excel 또는 CSV 파일을 여는 다른 프로그램에서 결과를 관리할 수 있습니다.

### <a name="delete"></a>삭제
더 이상 검토가 필요 없는 경우 검토를 삭제합니다. **삭제** 단추는 PIM 응용 프로그램에서 검토를 제거합니다.

> [!IMPORTANT]
> 삭제가 일어나기 전에 경고가 표시되지 않으므로 삭제를 원하는 검토인지 확인해야 합니다.
> 
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>다음 단계
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png



<!--HONumber=Nov16_HO3-->


