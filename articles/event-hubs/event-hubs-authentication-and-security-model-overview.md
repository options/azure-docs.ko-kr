---
title: "Azure Event Hubs 인증 및 보안 모델의 개요 | Microsoft Docs"
description: "이벤트 허브 인증 및 보안 모델 개요"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/30/2016
ms.author: sethm;clemensv
translationtype: Human Translation
ms.sourcegitcommit: 05ca343cfdfc602759eb3ea30a7186a0bb47bd74
ms.openlocfilehash: 3f20a19c212c082aa766cc2bd67e938aaabf715e


---
# <a name="event-hubs-authentication-and-security-model-overview"></a>이벤트 허브 인증 및 보안 모델 개요
Azure 이벤트 허브 보안 모델은 다음 요구 사항을 만족합니다.

* 유효한 자격 증명을 제공하는 장치만이 이벤트 허브로 데이터를 보낼 수 있습니다.
* 장치는 다른 장치를 가장할 수 없습니다.
* Rogue 장치는 이벤트 허브로 데이터를 보내지 못하도록 차단할 수 있습니다.

## <a name="device-authentication"></a>장치 인증
Event Hubs 보안 모델은 [공유 액세스 서명(SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) 토큰 및 *이벤트 게시자*의 조합을 기반으로 합니다. 이벤트 게시자는 이벤트 허브에 대한 가상 끝점을 정의합니다. 게시자는 이벤트 허브에 메시지를 보내는 데만 사용할 수 있습니다. 게시자에서 메시지를 받을 수 없습니다.

일반적으로 이벤트 허브에서는 장치 당 하나의 게시자를 사용합니다. 이벤트 허브의 게시자에게 전달되는 모든 메시지는 해당 이벤트 허브 내에서 큐에 삽입합니다. 게시자는 세분화된 액세스 제어 및 제한을 사용하도록 설정합니다.

각 장치는 장치에 업로드되는 고유 토큰을 할당합니다. 각 고유 토큰이 고유한 다른 게시자에 대한 액세스가 허용되도록 토큰이 생성됩니다. 토큰을 소유하는 장치는 하나의 게시자에게만 보낼 수 있으며 다른 게시자에는 보낼 수 없습니다. 여러 장치에서 동일한 토큰을 공유하는 경우 이러한 각 장치들은 게시자를 공유합니다.

권장되지는 않지만 이벤트 허브에 대한 직접 액세스 권한을 부여하는 토큰을 사용하는 장치를 장착할 수 있습니다. 이 토큰을 보유하는 모든 장치는 해당 이벤트 허브에 직접 메시지를 보낼 수 있습니다. 이러한 장치는 제한의 대상이 되지 않습니다. 또한 장치에서 해당 이벤트 허브로 보내는 것을 차단할 수 없습니다.

모든 토큰은 SAS 키로 서명됩니다. 일반적으로 모든 토큰은 동일한 키로 서명됩니다. 장치는 키를 인식하지 않습니다. 장치는 토큰을 제조하지 못합니다.

### <a name="create-the-sas-key"></a>SAS 키 만들기
Azure 이벤트 허브 네임스페이스를 만들면, 서비스는 **RootManageSharedAccessKey**라는 256비트 SAS 키를 생성합니다. 이 키는 네임스페이스에 대한 송신, 수신 및 관리 권한을 부여합니다. 추가 키를 만들 수 있습니다. 특정 이벤트 허브에 대한 송신 권한을 부여하는 키를 생성하는 것이 좋습니다. 이 항목의 나머지 부분에서는 이 키의 이름을 **EventHubSendKey**로 지정했다고 가정합니다.

다음 예제에서는 이벤트 허브를 만들 때 송신 전용 키를 만듭니다.

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>토큰 생성
SAS 키를 사용하여 토큰을 생성할 수 있습니다. 장치당 하나만 토큰만 생성해야 합니다. 다음 메서드를 사용하여 토큰을 생성할 수 있습니다. 모든 토큰은 **EventHubSendKey** 키를 사용하여 생성됩니다. 각 토큰에는 고유한 URI가 할당됩니다.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

이 메서드를 호출할 때 URI는 `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`로 지정해야 합니다. 모든 토큰에 대해 `PUBLISHER_NAME`는 제외하고 URI는 동일하며, 각 토큰에 대해서는 달라야 합니다. `PUBLISHER_NAME` 은 해당 토큰을 받는 장치의 ID를 나타냅니다.

이 메서드는 다음 구조를 사용하여 토큰을 생성합니다.

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

토큰 만료 시간(초)은 1970년 1월 1일에서부터 지정됩니다. 다음은 토큰 예제입니다.

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

일반적으로 토큰은 장치의 수명과 유사하거나 초과하는 수명을 보유합니다. 장치에 새 토큰을 가져올 수 있는 기능이 있으면, 더 짧은 수명의 토큰을 사용할 수 있습니다.

### <a name="devices-sending-data"></a>데이터를 전송하는 장치
토큰을 만든 후에 각 장치는 자체의 고유한 토큰으로 프로비전됩니다.

장치가 이벤트 허브로 데이터를 전송하면 장치는 보내기 요청으로 토큰의 태그를 지정합니다. 공격자가 도청 및 토큰 가로채기를 방지 하려면, 장치 및 이벤트 허브 간의 통신은 암호화된 채널을 통해 수행되어야 합니다.

### <a name="blacklisting-devices"></a>장치 차단 목록
공격자가 토큰을 도난한 경우, 공격자가 토큰이 도난당한 장치로 가장할 수 있습니다. 장치가 다른 게시자를 사용하는 새 토큰을 제공할 때까지 장치 차단 목록은 장치를 사용할 수 없습니다.

## <a name="authentication-of-back-end-applications"></a>백 엔드 응용 프로그램의 인증
장치에서 생성된 데이터를 사용하는 백 엔드 응용 프로그램을 인증하려면 이벤트 허브에서는 서비스 버스 항목에 사용된 모델과 유사한 보안 모델을 사용합니다. 이벤트 허브 소비자 그룹은 서비스 버스 토픽에 대한 구독과 같습니다. 소비자 그룹을 만드는 요청이 이벤트 허브 또는 이벤트 허브가 속해 있는 네임스페이스에 대한 관리 권한을 부여하는 토큰과 함께 지정된 경우, 클라이언트는 소비자 그룹을 만들 수 있습니다. 수신 요청이 해당 소비자 그룹, 이벤트 허브 또는 이벤트 허브가 속해 있는 네임스페이스에 대한 수신 권한을 부여하는 토큰과 함께 지정된 경우, 클라이언트는 소비자 그룹에서 데이터를 사용할 수 있습니다.

서비스 버스의 현재 버전은 개별 구독에 대한 SAS 규칙을 지원하지 않습니다. 이벤트 허브 소비자 그룹에 대해서도 마찬가지입니다. SAS 지원은 나중에 두 기능에 대해 추가됩니다.

개별 소비자 그룹에 대한 SAS 인증이 없는 경우, 공용 키가 있는 모든 소비자 그룹을 보호하기 위해 SAS 키를 사용할 수 있습니다. 이 방식을 통해 응용 프로그램을 사용하면 이벤트 허브의 소비자 그룹 중 하나에서 데이터를 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계
이벤트 허브에 대한 자세한 내용은 다음 항목을 방문하세요.

* [이벤트 허브 개요]
* [SAS 개요]
* [Event Hubs를 사용하는 응용 프로그램 예제] 전체

[이벤트 허브 개요]: event-hubs-overview.md
[Event Hubs를 사용하는 응용 프로그램 예제]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[SAS 개요]: ../service-bus-messaging/service-bus-sas-overview.md




<!--HONumber=Feb17_HO2-->


