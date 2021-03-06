---
title: "DocumentDB 데이터베이스 질문 - 질문과 대답 | Microsoft Docs"
description: "JSON용 NoSQL 문서 데이터베이스 서비스인 Azure DocumentDB에 대한 질문과 대답을 가져옵니다. 용량, 성능 수준 및 크기 조정에 대한 데이터베이스 질문에 대답합니다."
keywords: "데이터베이스 질문, 질문과 대답, Documentdb, Azure, Microsoft Azure"
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 8f3e6b271f3ca5973a00f2a5a9e11dafeb9637aa
ms.openlocfilehash: ed520532e7a035baa0283f829ec13540194d136e


---
# <a name="frequently-asked-questions-about-documentdb"></a>DocumentDB에 대한 질문과 대답
## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Microsoft Azure DocumentDB 기본 사항에 대한 데이터베이스 질문
### <a name="what-is-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB란 무엇인가요?
Microsoft Azure DocumentDB는 대규모의 아주 빠른 NoSQL 문서 DaaS(Database-as-a-Service)로, Microsoft Azure의 성능과 범위로 지원되는 관리 플랫폼을 통해 스키마 제약이 없는 데이터에 대해 다양한 쿼리를 제공하고, 구성 가능하며 안정적인 성능을 제공하고, 신속한 개발을 지원합니다. 예측 가능한 처리량, 높은 가용성, 짧은 대기 시간 및 스키마 제약이 없는 데이터 모델이 핵심 요구 사항인 경우 DocumentDB는 웹, 모바일, 게임 및 IoT 응용 프로그램에 적합한 솔루션입니다. DocumentDB는 네이티브 JSON 데이터 모델을 통해 스키마 유연성과 다양한 인덱싱을 제공하며, 통합된 JavaScript로 다중 문서 트랜잭션도 지원합니다.  

이 서비스 배포 및 사용에 대한 데이터베이스 질문, 대답 및 지침은 [DocumentDB 설명서 페이지](https://azure.microsoft.com/documentation/services/documentdb/)를 참조하세요.

### <a name="what-kind-of-database-is-documentdb"></a>DocumentDB는 어떤 종류의 데이터베이스인가요?
DocumentDB는 JSON 형식으로 데이터를 저장하는 NoSQL 문서 지향 데이터베이스입니다.  DocumentDB는 다양한 DocumentDB [SQL 쿼리 문법](documentdb-sql-query.md)을 통해 쿼리할 수 있는 자체 포함된 중첩 데이터 구조를 지원합니다. DocumentDB는 [저장 프로시저, 트리거 및 사용자 정의 함수](documentdb-programming.md)를 통해 서버 쪽 JavaScript의 고성능 트랜잭션 처리를 제공합니다. 또한 데이터베이스는 연결된 [성능 수준](documentdb-performance-levels.md)으로 개발자가 조정할 수 있는 일관성 수준을 지원합니다.

### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>DocumentDB 데이터베이스에 관계형 데이터베이스(RDBMS)와 같은 테이블이 있나요?
아니요, DocumentDB는 JSON 문서의 컬렉션에 데이터를 저장합니다.  DocumentDB 리소스에 대한 자세한 내용은 [DocumentDB 리소스 모델 및 개념](documentdb-resources.md)을 참조하세요. DocumentDB와 같은 NoSQL 솔루션과 관계형 솔루션이 어떻게 다른지에 대한 자세한 내용은 [NoSQL vs SQL](documentdb-nosql-vs-sql.md)을 참조하세요.

### <a name="do-documentdb-databases-support-schema-free-data"></a>DocumentDB 데이터베이스에서 스키마 제약이 없는 데이터를 지원하나요?
예, DocumentDB를 통해 응용 프로그램은 스키마 정의 또는 힌트 없이 임의의 JSON 문서를 저장할 수 있습니다. DocumentDB SQL 쿼리 인터페이스를 통해 쿼리에 즉시 데이터를 사용할 수 있습니다.   

### <a name="does-documentdb-support-acid-transactions"></a>DocumentDB에서 ACID 트랜잭션을 지원하나요?
예, DocumentDB는 JavaScript 저장 프로시저 및 트리거로 표현되는 문서 간 트랜잭션을 지원합니다. 동시에 실행되는 다른 코드 및 사용자 요청과 모두 분리되거나 전혀 분리되지 않으므로 트랜잭션은 범위가 각 컬렉션 내의 단일 파티션으로 지정되고 ACID 시맨틱으로 실행됩니다.  JavaScript 응용 프로그램 코드의 서버 쪽 실행을 통해 예외가 발생하는 경우 전체 트랜잭션이 롤백됩니다. 트랜잭션에 대한 자세한 내용은 [데이터베이스 프로그램 트랜잭션](documentdb-programming.md#database-program-transactions)을 참조하세요.

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>DocumentDB의 대표적인 사용 사례는 무엇인가요?
DocumentDB는 자동 크기 조정, 예측 가능한 성능, 밀리초 응답 시간의 빠른 주문, 스키마 제약 없는 데이터에 대한 쿼리 기능 등이 중요한 경우 새로운 웹, 모바일, 게임 및 IoT 응용 프로그램에 적합한 솔루션입니다. DocumentDB는 응용 프로그램 데이터 모델의 신속한 개발 및 연속적인 반복 지원에 적합합니다. 사용자 생성 콘텐츠 및 데이터를 관리하는 응용 프로그램은 [DocumentDB의 일반적인 사용 사례](documentdb-use-cases.md)입니다.  

### <a name="how-does-documentdb-offer-predictable-performance"></a>DocumentDB는 어떻게 예측 가능한 성능을 제공하나요?
DocumentDB의 처리량 측정값은 [요청 단위](documentdb-request-units.md) 입니다. 1RU는 1KB 문서의 GET 처리량에 해당합니다. 읽기, 쓰기, SQL 쿼리, 저장 프로시저 실행 등 DocumentDB에서의 모든 작업에는 작업을 완료하는 데 필요한 처리량을 기반으로 하는 결정론적 RU 값이 있습니다. CPU, IO 및 메모리와 이 각각이 응용 프로그램 처리량에 미치는 영향을 고려하는 대신 단일 RU 측정값 측면에서 고려할 수 있습니다.

초당 RU 처리량 면에서 프로비전된 처리량으로 각 DocumentDB 컬렉션을 예약할 수 있습니다. 모든 규모의 응용 프로그램에 대해 개별 요청을 벤치마킹하여 해당 RU 값을 측정하고 모든 요청에서 총 요청 단위 합계를 처리하도록 컬렉션을 프로비전할 수 있습니다. 또한 응용 프로그램 요구 사항이 진화함에 따라 컬렉션의 처리량을 확장하거나 축소할 수 있습니다. 요청 단위에 대한 자세한 내용 및 컬렉션 요구 사항 확인에 대한 도움말은 [성능 및 용량 관리](documentdb-manage.md)를 참고하고 [처리량 계산기](https://www.documentdb.com/capacityplanner)를 사용해 봅니다.

### <a name="is-documentdb-hipaa-compliant"></a>DocumentDB HIPAA 규정을 준수합니까?
예, DocumentDB는 HIPAA 규정을 준수합니다. HIPAA는 개별적으로 식별할 수 있는 상태 정보의 사용, 공개 및 보호에 대한 요구 사항을 설정합니다. 자세한 내용은 [Microsoft 보안 센터](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA)를 참조하세요.

### <a name="what-are-the-storage-limits-of-documentdb"></a>DocumentDB의 저장소 한도는 어느 정도인가요?
하나의 컬렉션이 DocumentDB에 저장할 수 있는 총 데이터 양에 대한 한도는 없습니다.

### <a name="what-are-the-throughput-limits-of-documentdb"></a>DocumentDB의 처리량 한도는 어느 정도인가요?
충분히 많은 파티션 키 간에 워크로드가 균등하게 분산될 수 있는 경우 하나의 컬렉션이 DocumentDB에 저장할 수 있는 총 데이터 양에 대한 한도는 없습니다.

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Microsoft Azure DocumentDB 비용은 얼마인가요?
자세한 내용은 [DocumentDB 가격 책정 세부 정보](https://azure.microsoft.com/pricing/details/documentdb/) 페이지를 참조하세요. DocumentDB 사용 요금은 사용 중인 컬렉션 수, 컬렉션이 온라인이었던 시간, 소비된 저장소 및 각 컬렉션의 프로비전된 처리량에 따라 결정됩니다.

### <a name="is-there-a-free-account-available"></a>무료 계정을 사용할 수 있나요?
Azure를 처음 사용하는 경우 [Azure 무료 계정](https://azure.microsoft.com/free/)에 등록하면 30일간 제공된 $200로 모든 Azure 서비스를 사용해 볼 수 있습니다. 또는 Visual Studio 구독이 있다면 모든 Azure 서비스에 사용할 수 있는 [월별 $150의 무료 Azure 크레딧](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)에 대한 자격이 주어집니다.  

[Azure DocumentDB 에뮬레이터](documentdb-nosql-local-emulator.md)를 사용하여 Azure 구독을 구입하지 않고도 무료로 로컬에서 응용 프로그램을 개발하고 테스트할 수 있습니다. DocumentDB 에뮬레이터에서 응용 프로그램이 작동하는 방식에 만족하는 경우 Azure DocumentDB 계정을 클라우드에서 사용하도록 전환할 수 있습니다.

### <a name="how-can-i-get-additional-help-with-documentdb"></a>DocumentDB 추가 도움말은 어떻게 구할 수 있나요?
도움이 필요한 경우 [스택 오버플로](http://stackoverflow.com/questions/tagged/azure-documentdb), [Azure DocumentDB MSDN 개발자 포럼](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)을 통해 문의하거나 [DocumentDB 엔지니어링 팀과의 1:1 채팅](http://www.askdocdb.com/)을 예약하세요. 최신 DocumentDB 소식과 기능에 대한 최신 동향을 파악하려면 [Twitter](https://twitter.com/DocumentDB)를 팔로우하세요.

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB 설정
### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB에는 어떻게 등록하나요?
Microsoft Azure DocumentDB는 [Azure Portal][azure-portal]에 있습니다.  먼저, Microsoft Azure 구독에 등록해야 합니다.  Microsoft Azure 구독에 등록하면 사용자의 Azure 구독에 DocumentDB 계정을 추가할 수 있습니다. DocumentDB 계정 추가에 대한 지침은 [DocumentDB 데이터베이스 계정 만들기](documentdb-create-account.md)를 참조하세요.   

### <a name="what-is-a-master-key"></a>마스터 키란 무엇인가요?
마스터 키는 계정의 모든 리소스에 액세스할 수 있는 보안 토큰입니다. 이 키가 있는 개인은 데이터베이스 계정의 모든 리소스에 대해 읽기 및 쓰기 액세스 권한을 가집니다. 마스터 키를 배포할 때 주의하세요. 주 마스터 키 및 보조 마스터 키는 [Azure Portal][azure-portal]의 **키** 블레이드에서 사용할 수 있습니다. 키에 대한 자세한 내용은 [액세스 키 보기, 복사 및 다시 생성](documentdb-manage-account.md#keys)을 참조하세요.

### <a name="how-do-i-create-a-database"></a>데이터베이스를 어떻게 만드나요?
[DocumentDB 데이터베이스 만들기](documentdb-create-database.md)의 설명대로 [Azure Portal]()을 사용하거나 [DocumentDB SDK](documentdb-sdk-dotnet.md) 중 하나를 사용하거나 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)를 통해 데이터베이스를 만들 수 있습니다.  

### <a name="what-is-a-collection"></a>컬렉션이란 무엇인가요?
컬렉션은 JSON 문서 및 관련 JavaScript 응용 프로그램 논리의 컨테이너입니다. 컬렉션은 사용한 처리량 및 저장소에 따라 [비용](documentdb-performance-levels.md) 이 결정되는 청구 가능 엔터티입니다. 컬렉션은 하나 이상의 파티션/서버에 걸쳐 있을 수 있으며 크기가 거의 무제한인 저장소 또는 처리량을 처리하도록 확장할 수 있습니다.

컬렉션은 DocumentDB에 대한 청구 엔터티입니다. 각 컬렉션은 프로비전된 처리량 및 사용된 저장소 공간에 따라 시간 단위로 요금이 청구됩니다. 자세한 내용은 [DocumentDB 가격](https://azure.microsoft.com/pricing/details/documentdb/)을 참조하세요.  

### <a name="how-do-i-set-up-users-and-permissions"></a>사용자 및 사용 권한을 어떻게 설정하나요?
[DocumentDB SDK](documentdb-sdk-dotnet.md) 중 하나를 사용하거나 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)를 통해 사용자 및 사용 권한을 만들 수 있습니다.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB에 대한 개발과 관련된 데이터베이스 질문
### <a name="how-to-do-i-start-developing-against-documentdb"></a>DocumentDB에 대해 어떻게 개발을 시작하나요?
[SDK](documentdb-sdk-dotnet.md) 는 .NET, Python, Node.js, JavaScript 및 Java용으로 제공됩니다.  또한 개발자는 [RESTful HTTP API](https://msdn.microsoft.com/library/azure/dn781481.aspx) 를 사용하여 다양한 플랫폼 및 언어의 DocumentDB 리소스를 조작할 수 있습니다.

GitHub에서 DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md) 및 [Python](documentdb-python-samples.md) SDK의 샘플을 사용할 수 있습니다.

### <a name="does-documentdb-support-sql"></a>DocumentDB에서 SQL을 지원하나요?
DocumentDB SQL 쿼리 언어는 SQL에서 지원하는 쿼리 기능의 향상된 하위 집합입니다. DocumentDB SQL 쿼리 언어는 JavaScript 기반 UDF(사용자 정의 함수)를 통해 다양한 계층 구조 및 관계형 연산자를 제공합니다. JSON 문법은 트리 노드로 레이블이 지정된 트리로 JSON 문서 모델링을 허용합니다. 이 내용은 DocumentDB 자동 인덱싱 기술 및 DocumentDB의 SQL 쿼리 언어 모두에 사용됩니다.  SQL 문법을 사용하는 방법에 대한 자세한 내용은 [DocumentDB 쿼리][query] 문서를 참조하세요.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>DocumentDB에서 지원하는 데이터 형식은 무엇인가요?
DocumentDB에서 지원되는 기본 데이터 형식은 JSON과 동일합니다. JSON에는 문자열, 숫자(IEEE754 배정밀도), 부울(true 및 false) 및 Null로 구성된 단순한 형식 시스템이 있습니다.  DateTime, Guid, Int64 및 Geometry와 같은 더욱 복잡한 데이터 형식은 { } 연산자를 사용하는 중첩 개체 및 [ ] 연산자를 사용하는 배열의 생성을 통해 JSON 및 DocumentDB로 표시될 수 있습니다.

### <a name="how-does-documentdb-provide-concurrency"></a>DocumentDB는 어떻게 동시성을 제공하나요?
DocumentDB는 HTTP 개체 태그 또는 ETag를 통해 OCC(낙관적 동시성 제어)를 지원합니다. 모든 DocumentDB 리소스에는 ETag가 있고 ETag는 문서가 업데이트될 때마다 서버에서 설정됩니다. ETag 헤더 및 현재 값은 모든 응답 메시지에 포함됩니다. ETag는 If-Match 헤더와 함께 사용하여 서버가 리소스를 업데이트해야 하는지를 결정하도록 할 수 있습니다. If-Match 값은 검사해야 할 ETag 값입니다. ETag 값이 서버 ETag 값과 일치하는 경우 리소스가 업데이트됩니다. ETag가 최신 상태가 아닌 경우 서버에서는 "HTTP 412 사전 조건 실패" 응답 코드와 함께 작업을 거부합니다. 클라이언트는 리소스를 다시 반입하여 리소스에 대한 현재 ETag 값을 가져와야 합니다. 또한 If-None-Match 헤더와 함께 ETag를 사용하여 리소스를 다시 인출해야 하는지를 결정합니다.

.NET에서 낙관적 동시성을 사용하려면 [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) 클래스를 사용합니다. .NET 샘플은 github에 있는 DocumentManagement 샘플에서 [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) 를 참조하세요.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>DocumentDB는 어떻게 트랜잭션을 수행하나요?
DocumentDB는 JavaScript 저장 프로시저 및 트리거를 통해 언어 통합 트랜잭션을 지원합니다. 스크립트 내 모든 데이터베이스 작업은 컬렉션(단일 파티션 컬렉션인 경우) 또는 파티션 키 값이 동일한 컬렉션 내의 문서(컬렉션이 분할된 경우)로 범위가 지정된 경우 스냅숏 격리에 따라 실행됩니다. 문서 버전의 스냅숏(ETags)은 트랜잭션의 시작 부분에서 찍고 스크립트가 성공한 경우에만 커밋됩니다. JavaScript에서 오류가 발생한 경우 트랜잭션이 롤백됩니다. 자세한 내용은 [DocumentDB 서버 쪽 프로그래밍](documentdb-programming.md) 을 참조하세요.

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>DocumentDB에 문서를 어떻게 일괄 삽입할 수 있나요?
DocumentDB에 문서를 일괄 삽입하는 세 가지 방법이 있습니다.

* [DocumentDB로 데이터 가져오기](documentdb-import-data.md)에 설명된 데이터 마이그레이션 도구.
* [문서 탐색기를 사용하여 문서 일괄 추가](documentdb-view-json-document-explorer.md#bulk-add-documents)에 설명된 Azure Portal의 문서 탐색기.
* [DocumentDB 서버 쪽 프로그래밍](documentdb-programming.md)에 설명된 저장 프로시저.

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB에서 리소스 링크 캐싱을 지원하나요?
예, DocumentDB는 RESTful 서비스이므로 리소스 링크가 제한되며 캐시될 수 있습니다. DocumentDB 클라이언트는 문서 또는 컬렉션과 같은 모든 리소스 읽기에 대해 "If-None-Match" 헤더를 지정하고, 서버 버전이 변경될 때만 로컬 복사본을 업데이트할 수 있습니다.

### <a name="is-a-local-instance-of-documentdb-available"></a>DocumentDB의 로컬 인스턴스를 사용할 수 있나요?
예. [Azure DocumentDB 에뮬레이터](documentdb-nosql-local-emulator.md)는 신뢰도 있는 DocumentDB 서비스의 에뮬레이션을 제공합니다. JSON 문서 만들기 및 쿼리, 컬렉션 프로비전 및 확장, 저장 프로시저 및 트리거 실행을 비롯하여 Azure DocumentDB으로 동일한 기능을 지원합니다. DocumentDB 에뮬레이터를 사용하여 응용 프로그램을 개발 및 테스트하고 DocumentDB에 대한 연결 끝점에 대한 단일 구성을 변경하여 글로벌 규모로 Azure에 배포할 수 있습니다.

[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md



<!--HONumber=Dec16_HO3-->


