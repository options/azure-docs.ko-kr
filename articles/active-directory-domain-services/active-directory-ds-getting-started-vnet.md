---
title: "Azure AD Domain Services: 가상 네트워크 만들기 또는 선택 | Microsoft Docs"
description: "Azure Active Directory 도메인 서비스 시작"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/03/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9e933774e3b618b1584b4f24a0491eda49e42077


---
# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Azure AD 도메인 서비스의 가상 네트워크 만들기 또는 선택
## <a name="guidelines-to-select-an-azure-virtual-network"></a>Azure 가상 네트워크 선택 지침
> [!NOTE]
> **시작하기 전에**: [Azure AD 도메인 서비스의 네트워킹 고려 사항](active-directory-ds-networking.md)을 참조하세요.
> 
> 

## <a name="task-2-create-an-azure-virtual-network"></a>작업 2: Azure 가상 네트워크 만들기
다음 구성 작업은 Azure Virtual Network를 만들고 그 안에 서브넷을 만드는 것입니다. 가상 네트워크 내에서 이 서브넷의 Azure AD 도메인 서비스를 사용하도록 설정합니다. 사용하고 싶은 기존 가상 네트워크가 이미 있는 경우 이 단계를 건너뛸 수 있습니다.

> [!NOTE]
> 만들거나 Azure AD 도메인 서비스와 함께 사용하도록 선택하는 Azure 가상 네트워크가 Azure AD 도메인 서비스에서 지원하는 Azure 지역에 속하는지 확인합니다. Azure AD 도메인 서비스를 사용할 수 있는 Azure 지역을 알아보려면 [지역별 Azure 서비스](https://azure.microsoft.com/regions/#services/) 페이지를 참조하세요.
> 
> 

후속 구성 단계에서 Azure AD 도메인 서비스를 사용하도록 설정할 때 올바른 가상 네트워크를 선택할 수 있도록 가상 네트워크의 이름을 기록해 둡니다.

Azure AD 도메인 서비스를 사용하도록 설정할 Azure 가상 네트워크를 만들려면 다음 구성 단계를 수행합니다.

1. **Azure 클래식 포털** ([https://manage.windowsazure.com](https://manage.windowsazure.com))로 이동합니다.
2. 왼쪽 창에서 **네트워크** 노드를 선택합니다.
   
    ![네트워크 노드](./media/active-directory-domain-services-getting-started/networks-node.png)
3. 페이지 아래쪽의 작업창에서 **새로 만들기** 를 클릭합니다.
   
    ![가상 네트워크 노드](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. **Network Services** 노드에서 **Virtual Network**를 선택합니다.
5. 가상 네트워크를 만들기 위해 **빠른 생성** 을 클릭합니다.
   
    ![가상 네트워크 - 빠른 생성](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
6. 가상 네트워크의 **이름** 을 지정합니다. 이 네트워크의 **주소 공간** 또는 **최대 VM 수**를 구성하도록 선택할 수도 있습니다. 지금은 **DNS 서버** 설정을 ‘없음’으로 설정된 상태로 둘 수 있습니다. DNS 서버 설정은 Azure AD 도메인 서비스를 사용하도록 설정한 후에 업데이트할 수 있습니다.
7. **위치** 드롭다운에서 지원되는 Azure 지역을 선택해야 합니다. Azure AD 도메인 서비스를 사용할 수 있는 Azure 지역을 알아보려면 [지역별 Azure 서비스](https://azure.microsoft.com/regions/#services/) 페이지를 참조하세요.
8. **Virtual Network 만들기** 단추를 클릭하여 가상 네트워크를 만듭니다.
   
    ![Azure AD 도메인 서비스에 대한 가상 네트워크를 만듭니다.](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. 가상 네트워크를 만든 후에 가상 네트워크를 선택하고 **구성** 탭을 클릭합니다.
   
    ![서브넷 만들기](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. **가상 네트워크 주소 공간** 섹션으로 이동합니다. **서브넷 추가**를 클릭하여 **AaddsSubnet**이라는 이름으로 서브넷을 지정합니다. **저장**을 클릭하여 서브넷을 만듭니다.
    
    ![Azure AD 도메인 서비스에 대한 서브넷을 만듭니다.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>작업 3 - Azure AD 도메인 서비스 활성화
다음 구성 태스크는 [Azure AD 도메인 서비스를 활성화](active-directory-ds-getting-started-enableaadds.md)하는 것입니다.




<!--HONumber=Dec16_HO2-->


