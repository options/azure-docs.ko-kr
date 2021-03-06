---
title: "Azure Portal을 사용하여 DNS 영역에 대한 레코드 집합 및 레코드 만들기 | Microsoft 문서"
description: "Azure DNS에 대한 호스트 레코드를 만들고 Azure 포털을 사용하여 레코드 집합 및 레코드를 만드는 방법"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f93905f4-e82e-45db-b490-878d318e6aba
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: b3951106fe2e8607e65bd0ae47fa2ea3346b8ca5

---

# <a name="create-dns-record-sets-and-records-by-using-the-azure-portal"></a>Azure 포털을 사용하여 DNS 레코드 집합 및 레코드 만들기

> [!div class="op_single_selector"]
> * [Azure 포털](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)

이 문서는 Azure 포털을 사용하여 레코드 및 레코드 집합을 만드는 과정을 안내합니다. 이 작업을 수행하려면 먼저 DNS 레코드 및 레코드 집합을 이해해야 합니다.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

이 페이지에 나오는 모든 예제에서는 'A' DNS 레코드 유형을 사용합니다. 다른 레코드 유형에 대한 프로세스도 비슷합니다.

새 레코드가 기존 레코드와 이름 및 형식이 똑같은 경우 기존 레코드 집합에 추가해야 하며&mdash; [Azure 포털을 사용하여 DNS 레코드와 레코드 집합을 참조하세요](dns-operations-recordsets-portal.md). 새 레코드가 기존 레코드와 이름 및 형식이 다른 경우 아래 설명처럼 새 레코드 집합을 만들어야 합니다.

## <a name="create-records-in-a-new-record-set"></a>새 레코드 집합에서 레코드 만들기

다음 예제에서는 Azure 포털을 사용하여 레코드 집합 및 레코드를 만드는 과정을 안내합니다.

1. 포털에 로그인합니다.
2. 레코드 집합을 만들려는 **DNS 영역** 블레이드로 이동합니다.
3. **DNS 영역** 블레이드의 위쪽에서 **+레코드 집합**을 클릭하여 **레코드 집합 추가** 블레이드를 엽니다.

    ![새 레코드 집합](./media/dns-getstarted-create-recordset-portal/newrecordset500.png)

4. **레코드 집합 추가** 블레이드에서 레코드 집합의 이름을 지정합니다. 예를 들어 레코드 집합의 이름을 "**www**"로 지정할 수 있습니다.

    ![레코드 집합 추가](./media/dns-getstarted-create-recordset-portal/addrecordset500.png)

5. 만들려는 레코드 유형을 선택합니다. 예를 들어 **A**를 선택합니다.
6. **TTL**을 설정합니다. 포털에서 라이브되는 기본 시간은&1;시간입니다.
7. 레코드 집합에서 각 레코드의 세부 정보를 추가합니다. 이 경우 레코드 형식은 'A'므로 A 레코드 IP 주소, 한 줄당 하나의 IP 주소를 추가해야 합니다.
8. IP 주소 추가가 끝나면 블레이드 맨 아래에서 **확인** 을 선택합니다. DNS 레코드 집합이 생성됩니다.

### <a name="verify-name-resolution"></a>이름 확인을 확인하기

nslookup, dig, [Resolve-DnsName PowerShell cmdlet](https://technet.microsoft.com/library/jj590781.aspx)과 같은 DNS 도구를 사용하여 DNS 영역이 Azure DNS 이름 서버에 존재하는지 테스트할 수 있습니다.

Azure DNS에서 새 영역을 사용하도록 도메인을 아직 위임하지 않은 경우 [DNS 쿼리를 영역에 대한 이름 서버 중 하나로 직접 보내](dns-getstarted-create-dnszone.md#test-name-servers)야 합니다. 아래 명령을 사용자 레코드 영역의 올바른 값으로 대체해야 합니다.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>다음 단계

[Azure DNS 이름 서버에 도메인 이름을 위임](dns-domain-delegation.md)하는 방법을 알아봅니다.

레코드 집합과 레코드를 관리하려면 [Azure 포털을 사용하여 DNS 레코드 및 레코드 집합 관리](dns-operations-recordsets-portal.md)를 참조하세요.



<!--HONumber=Dec16_HO2-->


