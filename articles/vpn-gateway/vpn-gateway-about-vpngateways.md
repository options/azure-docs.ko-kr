---
title: "VPN Gateway 개요: Azure 가상 네트워크에 대한 크로스-프레미스 VPN 연결 만들기 | Microsoft Docs"
description: "이 VPN Gateway 개요에서는 VPN 연결을 사용하여 인터넷을 통해 Azure 가상 네트워크에 연결하는 방법에 대해 설명합니다. 기본 연결 구성의 다이어그램이 포함됩니다."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: c47ff077a1a3193559e98278bc930101bed26ba0
ms.openlocfilehash: e9f34fd4607fa52dec44f27a723ca506cb6cd584


---
# <a name="about-vpn-gateway"></a>VPN 게이트웨이 정보
Azure 가상 네트워크와 온-프레미스 위치 간에 네트워크 트래픽을 보내려는 경우 가상 네트워크 게이트웨이를 만들어야 합니다. VPN Gateway는 공용 연결을 통해 암호화된 트래픽을 전송하는 가상 네트워크 게이트웨이의 유형입니다. 또한 가상 네트워크 게이트웨이 사용하여 Microsoft 네트워크를 통해 Azure 가상 네트워크 간에 트래픽을 보낼 수 있습니다.

가상 네트워크 게이트웨이를 만들 때 몇 가지 설정을 지정합니다. 필요한 설정 중 하나는 '-GatewayType'입니다. 각각의 가상 네트워크에는 게이트웨이 유형당 하나의 가상 네트워크 게이트웨이가 있을 수 있습니다. 예를 들어 '-GatewayType ExpressRoute'를 사용하는 하나의 가상 네트워크 게이트웨이와 '-GatewayType Vpn'을 사용하는 하나의 가상 네트워크 게이트웨이가 있을 수 있습니다. 이 문서에서는 주로 VPN Gateway에 대해 다룹니다. 

네트워크 트래픽을 공용 연결을 통해 암호화하여 전송하면 'VPN' 게이트웨이 유형을 사용합니다. 이를 VPN Gateway라고도 합니다. 사이트 간, 지점 및 사이트 간, VNet 간 연결은 모두 VPN Gateway를 사용합니다. 네트워크 트래픽을 전용 개인 연결에 전송하면 'Express 경로' 게이트웨이 유형을 사용합니다. 이를 Express 경로 게이트웨이라고도 합니다. Express 경로에 대한 자세한 내용은 [Express 경로 기술 개요](../expressroute/expressroute-introduction.md)를 참조하세요.

## <a name="pricing"></a>가격
[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

## <a name="gateway-skus"></a>게이트웨이 SKU
[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

VPN Gateway용 게이트웨이 SKU에 대한 자세한 내용은 [게이트웨이 SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku)를 참조하세요.

### <a name="estimated-aggregate-throughput-by-sku"></a>SKU 기준으로 예상된 총 처리량
[!INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="configuring-a-vpn-gateway"></a>VPN Gateway 구성
VPN Gateway를 구성할 때 사용할 지침은 가상 네트워크를 만드는 데 사용되는 배포 모델에 따라 달라집니다. 예를 들어 클래식 배포 모델을 사용하여 VNet을 만든 경우 클래식 배포 모델에 대한 가이드 및 지침을 사용하여 VPN 게이트웨이 설정을 만들고 구성합니다. 배포 모델에 대한 자세한 내용은 [Resource Manager 배포 및 클래식 배포 모델 이해](../azure-resource-manager/resource-manager-deployment-model.md)를 참조하세요.

VPN Gateway 연결은 특정 설정으로 구성된 여러 리소스에 따라 다릅니다. 대부분의 리소스를 특정 순서로 구성해야 하지만 어떤 경우에는 개별적으로 구성할 수 있습니다. Azure Portal과 같은 하나의 구성 도구를 사용하여 리소스를 시작하고 구성할 수 있습니다. 그런 다음 나중에 PowerShell과 같은 다른 도구로 전환하도록 결정하여 추가 리소스를 구성하거나 해당하는 경우 기존 리소스를 수정할 수 있습니다. 현재, Azure Portal에서 모든 리소스 및 리소스 설정을 구성할 수 없습니다. 각 연결 토폴로지에 대한 문서의 지침은 특정 구성 도구가 필요한지 여부를 지정합니다. VPN Gateway의 개별 리소스 및 설정에 대한 정보는 [VPN Gateway 설정 정보](vpn-gateway-about-vpn-gateway-settings.md)를 참조하세요.

다음 섹션에서는 다음과 같은 내용을 나열하는 테이블을 포함합니다.

* 사용 가능한 배포 모델
* 사용 가능한 구성 도구
* 사용할 수 있는 경우 문서로 직접 이동하는 링크

다이어그램 및 설명을 사용하여 요구 사항에 맞게 연결 토폴로지를 선택하도록 도울 수 있습니다. 다이어그램은 기본 초기 토폴로지를 보여 주지만 다이어그램을 지침으로 사용하여 더 복잡한 구성을 작성할 수 있습니다.

## <a name="site-to-site-and-multi-site"></a>사이트 간 및 다중 사이트
### <a name="site-to-site"></a>사이트 간
S2S(사이트 간) VPN Gateway 연결은 IPsec/IKE(IKEv1 또는 IKEv2) VPN 터널을 통한 연결입니다. 이 연결 유형은 할당된 공용 IP 주소를 가지고 NAT 다음에 위치하지 않는 온-프레미스에 있는 VPN 장치를 필요로 합니다. S2S 연결은 프레미스 간 및 하이브리드 구성에 사용될 수 있습니다.   

![Azure VPN Gateway 사이트 간 연결 예제](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>다중 사이트
VNet과 여러 온-프레미스 네트워크 간의 VPN Gateway 연결을 만들고 구성할 수 있습니다. 여러 연결을 사용하는 경우 경로 기반 VPN 유형(클래식 VNet에 대한 동적 게이트웨이)을 사용해야 합니다. VNet이 하나의 VPN Gateway만 사용할 수 있으므로 게이트웨이를 통한 모든 연결은 사용 가능한 대역폭을 공유합니다. 이는 "다중 사이트" 연결이라고 합니다.

![Azure VPN Gateway 다중 사이트 연결 예제](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>사이트 간 및 다중 사이트의 배포 모델 및 메서드
[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="vnet-to-vnet"></a>VNet 간
가상 네트워크를 다른 가상 네트워크에 연결(VNet-VNet)하는 것은 VNet을 온-프레미스 사이트 위치에 연결하는 것과 유사합니다. 두 연결 유형 모두 VPN 게이트웨이를 사용하여 IPsec/IKE를 통한 보안 터널을 제공합니다. VNet 간 통신을 다중 사이트 연결 구성과 통합할 수도 있습니다. 이렇게 하면 프레미스 간 연결을 가상 네트워크 간 연결과 결합하는 네트워크 토폴로지를 설정할 수 있습니다.

연결할 VNet의 상태는 다음과 같습니다.

* 동일하거나 다른 지역에 위치
* 동일하거나 다른 구독에 위치 
* 동일하거나 다른 배포 모델

![Azure VPN Gateway VNet 간 연결 예제](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

#### <a name="connections-between-deployment-models"></a>배포 모델 간의 연결
Azure에는 현재 클래식 및 Resource Manager 등 두 개의 배포 모델이 있습니다. 일정 기간 동안 Azure를 사용한 경우 Azure VM 및 인스턴스 역할이 클래식 VNet에서 실행되고 있을 것입니다. 이후의 VM 및 역할 인스턴스는 Resource Manager에서 만들 VNet에서 실행되고 있을 것입니다. VNet 간의 연결을 만들어 어떤 VNet의 리소스가 다른 리소스와 서로 직접 통신하도록 할 수 있습니다.

#### <a name="vnet-peering"></a>VNet 피어링
가상 네트워크가 특정 요구 사항을 충족하면 VNet 피어링을 사용하여 연결을 만들 수 있습니다. VNet 피어링은 가상 네트워크 게이트웨이를 사용하지 않습니다. 자세한 내용은 [VNet 피어링](../virtual-network/virtual-network-peering-overview.md)을 참조하세요.

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>VNet 간 배포 모델 및 메서드
[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="point-to-site"></a>지점 및 사이트 간
지점 및 사이트 간(P2S) VPN Gateway 연결을 사용하면 개별 클라이언트 컴퓨터에서 가상 네트워크에 안전한 연결을 만들 수 있습니다. P2S는 SSTP를 통한 VPN 연결입니다(보안 소켓 터널링 프로토콜). P2S 연결을 작동하는 데는 VPN 장치 또는 공용 IP 주소가 필요하지 않습니다. 클라이언트 컴퓨터부터 시작하여 VPN 연결을 설정합니다. 이 솔루션은 집 또는 회의와 같은 원격 위치에서 VNet에 연결하려는 경우 또는 몇 명의 클라이언트만 VNet에 연결해야 하는 경우에 유용합니다. P2S 연결은 동일한 VPN Gateway를 통해 S2S 연결과 함께 사용할 수 있으며 두 연결 모두에 대한 구성 요구 사항을 모두 호환하도록 지원합니다.

![Azure VPN Gateway 지점 및 사이트 연결 예제](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>지점 및 사이트에 대한 배포 모델 및 메서드
[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="expressroute"></a>Express 경로
[!INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Express 경로 연결에서 가상 네트워크 게이트웨이는 'VPN'이 아닌 'Express 경로' 게이트웨이 유형으로 구성됩니다. Express 경로에 대한 자세한 내용은 [Express 경로 기술 개요](../expressroute/expressroute-introduction.md)를 참조하세요.

## <a name="site-to-site-and-expressroute-coexisting-connections"></a>사이트 간 및 Express 경로 공존 연결
Express 경로는 공용 인터넷을 사용하지 않고 WAN에서 Azure를 비롯한 Microsoft 서비스에 대한 직접 전용 연결입니다. 사이트 간 VPN 트래픽은 공용 인터넷을 통해 암호화되어 이동합니다. 동일한 가상 네트워크에 대한 사이트 간 VPN 및 Express 경로 연결을 구성할 수 있으면 여러 장점이 있습니다.

사이트 간 VPN을 Express 경로에 대한 보안 장애 조치(failover) 경로로 구성하거나 사이트 간 VPN을 사용하여 사용자 네트워크의 일부가 아니지만 Express 경로를 통해 연결된 사이트에 연결할 수 있습니다. 여기에는 동일한 가상 네트워크에 대한 두 개의 가상 네트워크 게이트웨이가 필요합니다. 하나는 -GatewayType Vpn을 사용하고 다른 하나는 -GatewayType Express 경로를 사용합니다.

![ExpressRoute 및 VPN Gateway 공존 연결 예제](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>S2S 및 Express 경로에 대한 배포 모델 및 메서드
[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="next-steps"></a>다음 단계
VPN Gateway 구성을 계획합니다. [VPN Gateway 계획 및 디자인](vpn-gateway-plan-design.md) 및 [Azure에 온-프레미스 네트워크 연결](../guidance/guidance-connecting-your-on-premises-network-to-azure.md)을 참조하세요.




<!--HONumber=Jan17_HO4-->


