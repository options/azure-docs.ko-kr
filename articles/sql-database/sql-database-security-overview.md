---
title: "Azure SQL Database 보안 개요 | Microsoft Docs"
description: "인증, 권한 부여, 연결 보안, 암호화 및 규정 준수와 관련하여 클라우드 및 SQL Server 온-프레미스 간의 차이점을 포함하여 Azure SQL 데이터베이스 및 SQL Server 보안에 대해 알아봅니다."
services: sql-database
documentationcenter: 
author: tmullaney
manager: jhubbard
editor: 
ms.assetid: a012bb85-7fb4-4fde-a2fc-cf426c0a56bb
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 06/09/2016
ms.author: thmullan;jackr
translationtype: Human Translation
ms.sourcegitcommit: 69faa86ddbc43793146653fc8d8dc2bf35c40aa1
ms.openlocfilehash: f3a7bcbc80580232f2704087eb529ee9ec8ead46


---
# <a name="securing-your-sql-database"></a>SQL 데이터베이스 보안 설정

이 문서는 Azure SQL 데이터베이스를 사용하여 응용 프로그램의 데이터 계층에 보안을 설정하기 위한 기본 사항을 안내합니다. 특히 이 문서에서는 데이터 보호, 액세스 제어 및 사전 모니터링을 위한 리소스로 시작합니다. 다음 다이어그램에서는 SQL Database에서 제공하는 보안 계층을 보여 줍니다.

![SQL 보안 및 규정 준수](./media/sql-database-security-overview/diagram.png)

SQL의 모든 버전에서 사용할 수 있는 보안 기능의 전체 개요에 대해서는 [SQL Server 데이터베이스 엔진 및 Azure SQL 데이터베이스를 위한 보안 센터](https://msdn.microsoft.com/library/bb510589)를 참조하세요. 추가 정보는 에서도 사용할 수는 [보안 및 Azure SQL 데이터베이스 기술 백서](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>데이터 보호
SQL Database는 이동 중인 데이터의 경우 [전송 계층 보안](https://support.microsoft.com/en-us/kb/3135244), 미사용 데이터의 경우 [투명한 데이터 암호화](http://go.microsoft.com/fwlink/?LinkId=526242) 및 사용 중인 데이터의 경우 [상시 암호화](https://msdn.microsoft.com/library/mt163865.aspx)를 제공하여 데이터를 보호합니다. SQL Database에서 이러한 데이터 보호 기능을 사용하는 방법에 대한 설명은 [데이터 보호 및 보안](sql-database-protect-data.md)을 참조하세요.

> [!IMPORTANT]
>Azure SQL 데이터베이스에 대한 모든 연결은 데이터베이스로/로부터 데이터 “전송 중"에 항상 암호화(SSL/TLS)가 필요합니다. 응용 프로그램의 연결 문자열에서 연결을 암호화하고 서버 인증서를 신뢰하지 *않도록* 매개 변수를 지정해야 합니다(Azure 클래식 포털에서 연결 문자열을 복사하는 경우 이 작업이 자동으로 수행됨). 그렇지 않으면 연결에서 서버의 ID를 확인할 수 없으며 "메시지 가로채기(man-in-the-middle)" 공격에 취약할 수 있습니다. 예를 들어, ADO.NET 드라이버의 경우 이러한 연결 문자열 매개 변수는 **Encrypt=True** 및 **TrustServerCertificate=False**입니다. 

데이터를 암호화하는 다른 방법으로 다음을 고려해 보세요.

* [셀 수준 암호화](https://msdn.microsoft.com/library/ms179331.aspx) 는 특정 열 또는 서로 다른 암호화 키를 가진 데이터의 셀도 암호화합니다.
* 하드웨어 보안 모듈 또는 암호화 키 계층의 중앙 관리가 필요한 경우 [Azure VM에서 SQL Server와 함께 Azure 키 자격 증명 모음](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)을 사용하는 것을 고려해 보세요.

## <a name="control-access"></a>액세스 제어
SQL Database는 방화벽 규칙, 사용자에게 ID 확인을 요구하는 인증 메커니즘 및 역할 기반 멤버 자격과 권한을 통한 데이터 인증을 사용하여 데이터베이스에 대한 액세스를 제한할 뿐만 아니라 행 수준 보안과 동적 데이터 마스킹을 사용하여 데이터베이스에 대한 액세스도 제한함으로써 데이터를 보호합니다. SQL Database에서 액세스 제어 기능을 사용하는 방법에 대한 설명은 [액세스 제어](sql-database-control-access.md)를 참조하세요.

> [!IMPORTANT]
> Azure에서 데이터베이스와 논리 서버를 관리할 경우 포털 사용자 계정의 역할 할당으로 제어합니다. 이 항목에 대한 자세한 내용은 [Azure 포털의 역할 기반 액세스 제어](../active-directory/role-based-access-control-configure.md)를 참조하세요.
>

### <a name="firewall-and-firewall-rules"></a>방화벽 및 방화벽 규칙
데이터를 보호하기 위해 방화벽은 [방화벽 규칙](sql-database-firewall-configure.md)을 사용하여 권한이 있는 컴퓨터를 지정할 때까지 데이터베이스 서버에 대한 모든 액세스를 차단합니다. 방화벽은 각 요청이 시작된 IP 주소의 데이터베이스에 대한 액세스를 허용합니다.

### <a name="authentication"></a>인증
SQL Database 인증은 데이터베이스에 연결할 때 사용자의 ID를 증명하는 방법을 나타냅니다. SQL 데이터베이스는 두 가지 인증 유형을 지원합니다.

* **SQL 인증**은 사용자 이름과 암호를 사용합니다. 데이터베이스의 논리 서버를 만들 때 사용자 이름 및 암호를 사용하여 "서버 관리자" 로그인을 지정했습니다. 이러한 자격 증명을 사용하면 해당 서버의 모든 데이터베이스에 데이터베이스 소유자 또는 "dbo"로 인증할 수 있습니다. 
* **Azure Active Directory 인증**은 Azure Active Directory에서 관리하는 ID를 사용하고 관리되고 통합된 도메인을 지원합니다. [가능한 경우](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory 인증(통합 보안)을 사용합니다. Azure Active Directory 인증을 사용하려는 경우 Azure AD 사용자 및 그룹을 허용하는 "Azure AD 관리자"라는 다른 서버 관리자를 만들어야 합니다. 이 관리자는 일반 서버 관리자가 할 수 있는 모든 작업을 수행할 수도 있습니다. Azure AD 관리자를 만들어 Azure Active Directory 인증을 활성화하는 방법에 대한 연습은 [Azure Active Directory 인증을 사용하여 SQL 데이터베이스에 연결](sql-database-aad-authentication.md) 을 참조하세요.

### <a name="authorization"></a>권한 부여
권한 부여는 사용자가 Azure SQL Database에서 수행할 수 있는 작업을 나타내며, 사용자 계정의 데이터베이스 역할 멤버 자격과 개체 수준 권한으로 제어합니다. 사용자에게 필요한 최소한의 권한을 부여하는 것이 가장 좋습니다. 연결 중인 서버 관리자 계정은 데이터베이스 내에서 작업을 수행할 권한이 있는 db_owner의 구성원입니다. 스키마 업그레이드 및 기타 관리 작업을 배포하기 위해서는 이 계정을 저장합니다. 응용 프로그램에서 해당 응용 프로그램에 필요한 최소한의 권한이 있는 데이터베이스에 연결하려면 보다 제한된 사용 권한을 가진 "ApplicationUser" 계정을 사용합니다.

### <a name="row-level-security"></a>행 수준 보안
행 수준 보안을 통해 고객은 쿼리를 실행하는 사용자의 특성(예: 그룹 멤버 자격 또는 실행 컨텍스트)을 기반으로 하여 데이터베이스 테이블의 행에 대한 액세스를 제어할 수 있습니다. 자세한 내용은 [행 수준 보안](https://msdn.microsoft.com/library/dn765131)을 참조하세요.

### <a name="data-masking"></a>데이터 마스킹 
SQL 데이터베이스 동적 데이터 마스킹에서는 권한이 없는 사용자에 대해 중요한 데이터를 마스킹해 표시함으로써 데이터 노출을 제한합니다. 동적 데이터 마스킹은 Azure SQL Database에서 잠재적으로 중요한 데이터를 자동으로 검색하고 응용 프로그램 계층에 미치는 영향을 최소화하면서 이러한 필드를 마스킹할 수 있는 실행 가능한 권장 사항을 제공합니다. 이 기능은 지정된 데이터베이스 필드를 통해 쿼리의 결과 집합에 있는 중요한 데이터를 혼란스럽게 만들면서 작동하지만 데이터베이스의 데이터를 변경하지는 않습니다. 자세한 내용은 중요한 데이터의 노출을 제한하는 데 사용할 수 있는 [SQL Database 동적 데이터베이스 마스킹](sql-database-dynamic-data-masking-get-started.md)을 참조하세요.

## <a name="proactive-monitoring"></a>사전 모니터링
SQL Database는 감사 및 위협 검색 기능을 제공하여 데이터를 보호합니다. 

### <a name="auditing"></a>감사
SQL Database 감사는 데이터베이스 활동을 추적하고 데이터베이스 이벤트를 Azure Storage 계정의 감사 로그에 기록하여 규정 준수를 유지하는 데 도움을 줍니다. 감사를 통해 진행 중인 데이터베이스 활동을 파악하고 이전 활동을 분석 및 조사하여 잠재적인 위협이나 의심스러운 악용 및 보안 위반을 식별할 수 있습니다. 자세한 내용은 [SQL Database 감사 시작](sql-database-auditing-get-started.md)을 참조하세요.  

### <a name="auditing--threat-detection"></a>감사 및 위협 감지 
SQL Database 감사는 데이터베이스 활동을 추적하고 데이터베이스 이벤트를 Azure Storage 계정의 감사 로그에 기록하여 규정 준수를 유지하는 데 도움을 줍니다. 감사를 통해 진행 중인 데이터베이스 활동을 파악하고 이전 활동을 분석 및 조사하여 잠재적인 위협이나 의심스러운 악용 및 보안 위반을 식별할 수 있습니다. 자세한 내용은 [SQL Database 감사 시작](sql-database-auditing-get-started.md)을 참조하세요.  
 
위협 검색은 Azure SQL Database 서비스에 구축되는 추가적 보안 인텔리전스 계층을 제공하여 감사 기능을 보완합니다. 이 기능은 비정상적인 데이터베이스 활동을 파악하고, 수집하고, 검색하기 위해&24;시간 내내 작동합니다. 의심스러운 활동, 잠재적 취약성, SQL 삽입 공격 및 비정상적인 데이터베이스 액세스 패턴에 대해 경고합니다. 제공되는 유익하고 실행 가능한 지침에 따라 경고에 대응할 수 있습니다. 자세한 내용은 [SQL 데이터베이스 위협 감지 시작](sql-database-threat-detection-get-started.md)을 참조하세요.  
 
### <a name="data-masking"></a>데이터 마스킹 
SQL 데이터베이스 동적 데이터 마스킹에서는 권한이 없는 사용자에 대해 중요한 데이터를 마스킹해 표시함으로써 데이터 노출을 제한합니다. 동적 데이터 마스킹은 Azure SQL Database에서 잠재적으로 중요한 데이터를 자동으로 검색하고 응용 프로그램 계층에 미치는 영향을 최소화하면서 이러한 필드를 마스킹할 수 있는 실행 가능한 권장 사항을 제공합니다. 이 기능은 지정된 데이터베이스 필드를 통해 쿼리의 결과 집합에 있는 중요한 데이터를 혼란스럽게 만들면서 작동하지만 데이터베이스의 데이터를 변경하지는 않습니다. 자세한 내용은 [Azure SQL Database 동적 데이터 마스킹](sql-database-dynamic-data-masking-get-started.md) 시작을 참조하세요.
 
## <a name="compliance"></a>규정 준수
위의 기능 및 응용 프로그램이 다양한 보안 규정 준수 요구 사항을 충족하도록 도울 수 있는 기능 외에도 Azure SQL 데이터베이스는 정기적인 감사에 참여하고 여러 규정 준수 표준에 대해 인증받았습니다. 자세한 내용은 [Microsoft Azure 보안 센터](https://azure.microsoft.com/support/trust-center/)를 참조하세요. 여기서 최신 [SQL Database 규정 준수 인증서](https://azure.microsoft.com/support/trust-center/services/) 목록을 찾을 수 있습니다.

## <a name="next-steps"></a>다음 단계

- SQL Database에서 데이터 보호 기능을 사용하는 방법에 대한 설명은 [데이터 보호 및 보안](sql-database-protect-data.md)을 참조하세요.
- SQL Database에서 액세스 제어 기능을 사용하는 방법에 대한 설명은 [액세스 제어](sql-database-control-access.md)를 참조하세요.
- 사전 모니터링에 대한 설명은 [SQL Database 감사 시작](sql-database-auditing-get-started.md) 및 [SQL Database 위협 검색 시작](sql-database-threat-detection-get-started.md)을 참조하세요.



<!--HONumber=Jan17_HO2-->


