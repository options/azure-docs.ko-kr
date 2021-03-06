---
title: "Active Directory를 사용하여 Data Lake Store 인증 | Microsoft 문서"
description: "Active Directory를 사용하여 Data Lake Store로 인증하는 방법에 대해 알아봅니다."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 820b7c5d-4863-4225-9bd1-df4d8f515537
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 35cde786bbc091c58f4dcb341cd47ce4c4f4b46c
ms.openlocfilehash: 02e52c3aba82ab8e3a8b1dc921731c29e505e23e


---
# <a name="service-to-serivce-authentication-with-data-lake-store-using-azure-active-directory"></a>Azure Active Directory를 사용하여 Data Lake Store로 서비스 간 인증
> [!div class="op_single_selector"]
> * [서비스 간 인증](data-lake-store-authenticate-using-active-directory.md)
> * [최종 사용자 인증](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store는 인증을 위해 Azure Active Directory를 사용합니다. Azure Data Lake Store 또는 Azure Data Lake Analytics와 함께 작동하는 응용 프로그램을 제작하기 전에 먼저 Azure Active Directory(Azure AD)를 사용하여 응용 프로그램을 인증하려는 방법을 결정해야 합니다. 사용할 수 있는 두 가지 주요 옵션은 다음과 같습니다.

* 최종 사용자 인증 및 
* 서비스 간 인증 

이 두 옵션 모두 Azure Data Lake Store 또는 Azure Data Lake Analytics에 대해 만들어진 각 요청에 연결하는 OAuth 2.0 토큰과 함께 제공되는 응용 프로그램에서 발생합니다.

이 문서는 서비스 간 인증을 위한 Azure AD 웹 응용 프로그램을 만드는 방법에 대해 설명합니다. 최종 사용자 인증을 위해 Azure AD 응용 프로그램 구성을 수행하는 방법은 [Azure Active Directory를 사용하여 Data Lake Store로 최종 사용자 인증](data-lake-store-end-user-authenticate-using-active-directory.md)을 참조하세요.

## <a name="prerequisites"></a>필수 조건
* Azure 구독. [Azure 무료 평가판](https://azure.microsoft.com/pricing/free-trial/)을 참조하세요.
* 구독 ID. Azure Portal에서 검색할 수 있습니다. 예를 들어 Data Lake Store 계정 블레이드에서 사용할 수 있습니다.
  
    ![구독 ID 가져오기](./media/data-lake-store-authenticate-using-active-directory/get-subscription-id.png)
* Azure AD 도메인 이름. Azure Portal의 오른쪽 위 모서리에 마우스를 가져가서 검색할 수 있습니다. 아래 스크린샷에서 도메인 이름은 **contoso.microsoft.com**이며 괄호 안의 GUID는 테넌트 ID입니다. 
  
    ![AAD 도메인 가져오기](./media/data-lake-store-authenticate-using-active-directory/get-aad-domain.png)

## <a name="service-to-service-authentication"></a>서비스 간 인증
최종 사용자가 자격 증명을 제공할 필요 없이 응용 프로그램이 자동으로 Azure AD로 인증하도록 하려는 경우 권장되는 방법입니다. 해당 자격 증명이 유효하기만 하면 응용 프로그램은 자체적으로 인증할 수 있습니다. 이는 연도의 순서로 사용자 지정될 수 있습니다.

### <a name="what-do-i-need-to-use-this-approach"></a>이 방법을 사용하려면 무엇이 필요한가요?
* Azure AD 도메인 이름. 이 이름은 이 문서의 필수 구성 요소에 이미 나열되어 있습니다.
* Azure AD **웹 응용 프로그램**.
* Azure AD 웹 응용 프로그램에 대한 클라이언트 ID.
* Azure AD 웹 응용 프로그램에 대한 클라이언트 암호.
* Azure AD 웹 응용 프로그램에 대한 토큰 끝점.
* 함께 작동하려는 Data Lake Store 파일/폴더 또는 Data Lake Analytics 계정에서 Azure AD 웹 응용 프로그램에 대한 액세스를 사용하도록 설정합니다.

Azure AD 웹 응용 프로그램을 만들고 아래 나열된 요구 사항을 위해 구성하는 방법은 아래의 [Active Directory 응용 프로그램 만들기](#create-an-active-directory-application) 섹션을 참조하세요.

> [!NOTE]
> 기본적으로 Azure AD 응용 프로그램은 Azure AD 응용 프로그램에서 검색할 수 있는 클라이언트 암호를 사용하도록 구성됩니다. 그러나 Azure AD 응용 프로그램에서 인증서를 대신 사용하려면 Azure PowerShell을 사용하여 [인증서를 사용하여 서비스 사용자 만들기](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate)에 설명된 대로 Azure AD 웹 응용 프로그램을 만들어야 합니다.
> 
> 

## <a name="create-an-active-directory-application"></a>Active Directory 응용 프로그램 만들기
이 섹션에서는 Azure Active Directory를 사용하여 Azure Data Lake Store로 서비스 간 인증을 위한 Azure AD 웹 응용 프로그램을 만들고 구성하는 방법에 대해 알아봅니다. 

### <a name="step-1-create-an-azure-active-directory-application"></a>1단계: Azure Active Directory 응용 프로그램 만들기
> [!NOTE]
> 아래 단계에서는 Azure Portal을 사용합니다. 또한 [Azure PowerShell](../resource-group-authenticate-service-principal.md) 또는 [Azure CLI](../resource-group-authenticate-service-principal-cli.md)를 사용하여 Azure AD 응용 프로그램을 만들 수 있습니다.
> 
> 

1. [클래식 포털](https://manage.windowsazure.com/)을 통해 Azure 계정에 로그인합니다.
2. 왼쪽 창에서 **Active Directory** 를 선택합니다.
   
     ![Active Directory 선택](./media/data-lake-store-authenticate-using-active-directory/active-directory.png)
3. 새 응용 프로그램을 만드는 데 사용할 Active Directory를 선택합니다. Active Directory가 두 개 이상인 경우에는 일반적으로 구독이 상주하는 디렉터리에 응용 프로그램을 만듭니다. 구독과 동일한 디렉터리에 있는 응용 프로그램의 구독 리소스에만 액세스 권한을 부여할 수 있습니다.  
   
     ![디렉터리 선택](./media/data-lake-store-authenticate-using-active-directory/active-directory-details.png)
4. 디렉터리에서 응용 프로그램을 보려면 **응용 프로그램**을 클릭합니다.
   
     ![응용 프로그램 보기](./media/data-lake-store-authenticate-using-active-directory/view-applications.png)
5. 해당 디렉터리에서 응용 프로그램을 만든 적이 없는 경우 다음과 비슷한 이미지가 표시됩니다. **응용 프로그램 추가**를 클릭합니다.
   
     ![응용 프로그램 추가](./media/data-lake-store-authenticate-using-active-directory/create-application.png)
   
     또는 아래쪽 창에서 **추가** 를 클릭합니다.
   
     ![추가](./media/data-lake-store-authenticate-using-active-directory/add-icon.png)
6. 응용 프로그램의 이름을 입력하고 만들 응용 프로그램의 유형을 선택합니다. 이 자습서에서는 **웹 응용 프로그램 및/또는 웹 API** 를 만들기로 선택하고 다음 단추를 클릭합니다.
   
     ![응용 프로그램 이름 지정](./media/data-lake-store-authenticate-using-active-directory/tell-us-about-your-application.png)

    > [!TIP]
    > 쉽게 검색할 수 있는 응용 프로그램 이름을 제공합니다. 이 자습서의 뒷부분에서 이 응용 프로그램을 검색하여 Data Lake Store 계정에 할당합니다.
    > 
    > 

7. 앱에 대한 속성을 입력합니다. **로그온 URL**의 경우 응용 프로그램을 설명하는 웹 사이트에 대한 URI를 제공합니다. 웹 사이트의 존재 여부는 확인되지 않습니다. 
   **앱 ID URI**의 경우 응용 프로그램을 식별하는 URI를 제공합니다.
   
     ![응용 프로그램 속성](./media/data-lake-store-authenticate-using-active-directory/app-properties.png)
   
    확인 표시를 클릭해 마법사를 완료하고 응용 프로그램을 만듭니다.

### <a name="step-2-get-client-id-client-secret-and-token-endpoint"></a>2단계: 클라이언트 ID, 클라이언트 암호 및 토큰 끝점 가져오기
프로그래밍 방식으로 로그인하는 경우 응용 프로그램에 대한 ID가 필요합니다. 응용 프로그램이 자체 자격 증명에서 실행되는 경우 인증 키도 필요합니다.

1. **구성** 탭을 클릭하여 응용 프로그램의 암호를 구성합니다.
   
     ![응용 프로그램 구성](./media/data-lake-store-authenticate-using-active-directory/application-configure.png)
2. **클라이언트 ID**를 복사합니다.
   
     ![클라이언트 ID](./media/data-lake-store-authenticate-using-active-directory/client-id.png)
3. 응용 프로그램이 자체 자격 증명에서 실행되는 경우 **키** 섹션까지 아래로 스크롤하여 암호가 유효한 기간을 선택합니다.
   
     ![키](./media/data-lake-store-authenticate-using-active-directory/create-key.png)
4. **저장** 을 선택하여 키를 만듭니다.
   
    ![저장](./media/data-lake-store-authenticate-using-active-directory/save-icon.png)
   
    저장된 키가 표시되고 키를 복사할 수 있습니다. 나중에 키를 검색할 수 없으므로 지금 복사해야 합니다.
   
    ![공유 키](./media/data-lake-store-authenticate-using-active-directory/save-key.png)
5. 아래와 같이 화면 아래쪽에서 **끝점 보기**를 선택하고 **OAuth 2.0 토큰 끝점** 필드의 값을 검색하여 토큰 끝점을 검색합니다.  
   
    ![테넌트 ID](./media/data-lake-store-authenticate-using-active-directory/save-tenant.png)

### <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>3단계: Azure Data Lake Store 계정 파일 또는 폴더에 Azure AD 응용 프로그램 할당(서비스 간 인증에만 해당)
1. 새 [Azure Portal](https://portal.azure.com)에 로그온하여 이전에 만든 Azure Active Directory 응용 프로그램에 연결할 Azure Data Lake Store 계정을 엽니다.
2. 데이터 레이크 저장소 계정 블레이드에서 **데이터 탐색기**를 클릭합니다.
   
    ![데이터 레이크 저장소 계정에서 디렉터리 만들기](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Create directories in Data Lake account")
3. **데이터 탐색기** 블레이드에서 Azure AD 응용 프로그램에 대한 액세스를 제공할 파일 또는 폴더를 클릭하고 **액세스**를 클릭합니다. 파일에 대한 액세스를 구성하려면 **파일 미리 보기** 블레이드에서 **액세스**를 클릭해야 합니다.
   
    ![데이터 레이크 파일 시스템에 ACL 설정](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Set ACLs on Data Lake file system")
4. **액세스** 블레이드는 루트에 이미 할당된 표준 액세스 및 사용자 지정 액세스를 나열합니다. **추가** 아이콘을 클릭하여 사용자 지정 수준 ACL을 추가합니다.
   
    ![표준 및 사용자 지정 액세스 나열](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "List standard and custom access")
5. **추가** 아이콘을 클릭하여 **사용자 지정 액세스 추가** 블레이드를 엽니다. 이 블레이드에서 **사용자 또는 그룹 선택**을 클릭한 다음 **사용자 또는 그룹 선택** 블레이드에서 이전에 만든 Azure Active Directory 응용 프로그램을 찾습니다. 검색할 그룹이 많을 경우 위쪽의 텍스트 상자를 사용하여 그룹 이름을 필터링합니다. 추가하려는 그룹을 클릭한 다음 **선택**을 클릭합니다.
   
    ![그룹 추가](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Add a group")
6. **사용 권한 선택**을 클릭하고 사용 권한 및 이러한 권한을 기본 ACL로 할당할지, 액세스 ALC로 할당할지 또는 둘 다로 할당할지 선택합니다. **확인**을 클릭합니다.
   
    ![그룹에 권한 할당](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Assign permissions to group")
   
    Data Lake Store의 사용 권한 및 기본/액세스 ACL에 대한 자세한 내용은 [Data Lake Store에서 액세스 제어](data-lake-store-access-control.md)를 참조하세요.
7. **사용자 지정 액세스 추가** 블레이드에서 **확인**을 클릭합니다. 이제 연결된 권한으로 새롭게 추가된 그룹이 **액세스** 블레이드에 나열됩니다.
   
    ![그룹에 권한 할당](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Assign permissions to group")    

## <a name="next-steps"></a>다음 단계
이 문서에서는 Azure AD 웹 응용 프로그램을 만들고 .NET SDK, Java SDK 등을 사용하여 만든 클라이언트 응용 프로그램에 필요한 정보를 수집했습니다. 이제 다음 문서를 읽고 Azure AD 웹 응용 프로그램을 사용하여 Data Lake Store로 인증한 다음 저장소에서 다른 작업을 수행하는 방법에 대해 알아볼 수 있습니다.

* [.NET SDK를 사용하여 Azure 데이터 레이크 저장소 시작](data-lake-store-get-started-net-sdk.md)
* [Java SDK를 사용하여 Azure Data Lake Store 시작](data-lake-store-get-started-java-sdk.md)




<!--HONumber=Nov16_HO5-->


