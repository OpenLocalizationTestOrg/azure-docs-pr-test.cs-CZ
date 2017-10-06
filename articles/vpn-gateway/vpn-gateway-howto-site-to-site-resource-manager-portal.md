---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN: portál | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky můžete vytvořit připojení mezi různými místy Site-to-Site VPN Gateway pomocí portálu hello."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 6f0acbaf1bf016026cefade048a116e94686103d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-in-hello-azure-portal"></a>Umožňuje vytvořit připojení Site-to-Site v hello portálu Azure

Tento článek ukazuje, jak toouse hello Azure portálu toocreate připojení k bráně VPN Site-to-Site z vaší místní síti toohello virtuální sítě. Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Rozhraní příkazového řádku](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit. Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Než začnete

Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:

* Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho. Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).
* Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy. Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění. Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k. 

### <a name="values"></a>Příklady hodnot

Hello příklady v tomto článku používají hello následující hodnoty. Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.

* **Název virtuální sítě:** TestVNet1
* **Adresní prostor:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (volitelné pro toto cvičení)
* **Podsítě:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (volitelné pro toto cvičení)
* **Podsíť brány:** 10.11.255.0/27
* **Skupina prostředků:** TestRG1
* **Umístění:** Východní USA
* **Server DNS:** Volitelné. Hello IP adresa serveru DNS.
* **Název brány virtuální sítě:** VNet1GW
* **Veřejná IP adresa:** VNet1GWIP
* **Typ sítě VPN:** Trasová
* **Typ připojení:** Site-to-Site (protokol IPsec)
* **Typ brány:** Síť VPN
* **Název brány místní sítě:** Site2
* **Název připojení:** VNet1toSite2

## <a name="CreatVNet"></a>1. Vytvoření virtuální sítě

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. Určení serveru DNS

DNS není připojení vyžaduje toocreate Site-to-Site. Pokud chcete toohave překladu pro prostředky, které jsou nasazené tooyour virtuální sítě, ale měli určit DNS server. Toto nastavení umožňuje určit server DNS hello má toouse pro překlad názvů pro tuto virtuální síť. Neslouží k vytvoření serveru DNS. Další informace o překladu IP adres najdete v tématu [Překlad názvů pro instance rolí a VM](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Vytvořit podsíť brány hello

[!INCLUDE [vpn-gateway-aboutgwsubnet](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. Vytvoření brány VPN hello

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Vytvoření brány místní sítě hello

Hello brány místní sítě obvykle odkazuje tooyour místní umístění. Zadejte název, pomocí kterého můžete Azure odkazovat tooit a pak zadejte IP adresu hello hello lokality z hello místní VPN zařízení toowhich se vytvoří připojení. Můžete také určit hello předpony IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello. Hello předpony, které zadáte předpony hello umístěn ve vaší místní síti. Pokud změny vaší místní sítě, nebo můžete potřebovat pro zařízení VPN hello toochange hello veřejnou IP adresu, můžete snadno aktualizovat hodnoty hello později.

[!INCLUDE [Add local network gateway](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Konfigurace zařízení VPN

Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN. V tomto kroku nakonfigurujete zařízení VPN. Při konfiguraci zařízení VPN, je třeba hello následující:

- Sdílený klíč. To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN. V našich ukázkách používáme základní sdílený klíč. Doporučujeme, abyste vygenerování složitější klíče toouse.
- Hello veřejnou IP adresu brány virtuální sítě. Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku. toofind hello veřejnou IP adresu brány VPN pomocí hello portál Azure, přejděte příliš**brány virtuální sítě**, pak klikněte na název své brány hello.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Vytvoření připojení VPN hello

Vytvořte připojení VPN hello Site-to-Site mezi bránou virtuální sítě a vaše místní zařízení VPN.

[!INCLUDE [Add connections](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Ověření připojení VPN hello

[!INCLUDE [Verify - Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="connectVM"></a>tooconnect tooa virtuálního počítače

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="reset"></a>Jak tooreset brána sítě VPN

Resetování brány Azure VPN je užitečné v případě ztráty připojení VPN mezi lokalitami na jednom nebo více tunelech VPN typu Site-to-Site. V této situaci vaše místní zařízení VPN jsou všechny funguje správně, ale nebylo možné tooestablish tunelových propojení IPsec pomocí bran Azure VPN hello. Pokyny najdete v tématu [Resetování brány VPN](vpn-gateway-resetgw-classic.md).

## <a name="resize"></a>Jak toochange bránu SKU (změny velikosti brány)

Hello kroky toochange skladová položka brány, najdete v části [SKU brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="next-steps"></a>Další kroky

* Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Informace o vynuceném tunelování najdete v tématu [Informace o vynuceném tunelování](vpn-gateway-forced-tunneling-rm.md).
* Informace o vysoce dostupných připojeních typu aktivní-aktivní najdete v tématu [Připojení s vysokou dostupností mezi jednotlivými místy a VNet-to-VNet](vpn-gateway-highlyavailable.md).
