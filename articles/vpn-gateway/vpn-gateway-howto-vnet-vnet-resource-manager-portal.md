---
title: "Připojit virtuální síti Azure tooanother virtuální sítě: portál | Microsoft Docs"
description: "Vytvoření připojení k bráně VPN mezi virtuálními sítěmi pomocí Správce prostředků a hello portálu Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: a529f90d976bee0f50403947d06e9da8a6c05349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-hello-azure-portal"></a>Konfigurace připojení k bráně VPN VNet-to-VNet pomocí hello portálu Azure

Tento článek ukazuje, jak toocreate připojení k bráně VPN mezi virtuálními sítěmi. Hello virtuální sítě může být v hello stejné nebo různých oblastí, a z hello stejné nebo různých předplatných. Při připojování virtuální sítě z různých předplatných, odběry hello nemusí toobe přidružené hello stejné klienta služby Active Directory. 

Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager a použijte hello portálu Azure. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Propojení různých modelů nasazení – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Propojení různých modelů nasazení – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) je podobné tooconnecting umístění lokality tooan místní virtuální síť. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooconsider připojení pomocí virtuální sítě partnerský vztah. Partnerské vztahy virtuálních sítí nepoužívají bránu VPN. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).

Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery. To umožňuje vytvářet topologie sítí, které spojují připojení mezi různými místy s připojením propojování virtuálních sítí, jak je znázorněno v následujícím diagramu hello:

![Informace o připojeních](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informace o připojeních")

### <a name="why-connect-virtual-networks"></a>Proč propojovat virtuální sítě?

Tooconnect virtuální sítě může být vhodné pro hello následujících důvodů:

* **Geografická redundance napříč oblastmi a geografická přítomnost**
  
  * Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.
  * Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure. Jedním z důležitých příkladů je tooset až SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.
* **Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**
  
  * Hello uvnitř stejné oblasti, můžete nastavit vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu tooisolation nebo požadavků na správu.

Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku. Všimněte si, že pokud vaše virtuální sítě v různých předplatných, nelze vytvořit připojení hello hello portálu. Můžete použít [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) nebo [rozhraní příkazového řádku](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Příklady nastavení

Pokud používáte tyto kroky jako cvičení, můžete použít hello příklad nastavení hodnoty. Pro účely tohoto příkladu použijeme pro jednotlivé virtuální sítě více adresních prostorů. Konfigurace VNet-to-VNet nicméně použití více adresních prostorů nevyžadují.

**Hodnoty pro virtuální síť TestVNet1:**

* Název virtuální sítě: TestVNet1
* Adresní prostor: 10.11.0.0/16
  * Název podsítě: FrontEnd
  * Rozsah adres podsítě: 10.11.0.0/24
* Skupina prostředků: TestRG1
* Umístění: Východní USA
* Adresní prostor: 10.12.0.0/16
  * Název podsítě: BackEnd
  * Rozsah adres podsítě: 10.12.0.0/24
* Název podsítě brány: GatewaySubnet (bude automatické vyplňování hello portálu)
  * Rozsah adres podsítě brány: 10.11.255.0/27
* DNS Server: Použijte hello IP adresu serveru DNS
* Název brány virtuální sítě: TestVNet1GW
* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách
* Skladová položka: Vyberte hello skladová položka brány, které chcete toouse
* Název veřejné IP adresy: TestVNet1GWIP
* Hodnoty připojení:
  * Název: TestVNet1toTestVNet4
  * Sdílený klíč: hello sdílený klíč můžete vytvořit sami. V tomto příkladu použijeme abc123. Hello důležité je, že při vytváření hello připojení mezi virtuálními sítěmi hello hello hodnota musí odpovídat.

**Hodnoty pro virtuální síť TestVNet4:**

* Název virtuální sítě: TestVNet4
* Adresní prostor: 10.41.0.0/16
  * Název podsítě: FrontEnd
  * Rozsah adres podsítě: 10.41.0.0/24
* Skupina prostředků: TestRG1
* Umístění: Západní USA
* Adresní prostor: 10.42.0.0/16
  * Název podsítě: BackEnd
  * Rozsah adres podsítě: 10.42.0.0/24
* Název GatewaySubnet: GatewaySubnet (bude automatické vyplňování hello portálu)
  * Rozsah adres podsítě brány: 10.41.255.0/27
* DNS Server: Použijte hello IP adresu serveru DNS
* Název brány virtuální sítě: TestVNet4GW
* Typ brány: Síť VPN
* Typ sítě VPN: Založená na trasách
* Skladová položka: Vyberte hello skladová položka brány, které chcete toouse
* Název veřejné IP adresy: TestVNet4GWIP
* Hodnoty připojení:
  * Název: TestVNet4toTestVNet1
  * Sdílený klíč: hello sdílený klíč můžete vytvořit sami. V tomto příkladu použijeme abc123. Hello důležité je, že při vytváření hello připojení mezi virtuálními sítěmi hello hello hodnota musí odpovídat.

## <a name="CreatVNet"></a>1. Vytvoření a konfigurace virtuální sítě TestVNet1
Pokud již máte virtuální síť, ověřte, zda text hello nastavení jsou kompatibilní s návrhem vaší brány VPN. Věnujte zvláštní pozornost tooany podsítě, které se mohou překrývat s jinými sítěmi. Pokud se podsítě překrývají, připojení nebude fungovat správně. Pokud jsou vaše virtuální síť nakonfigurované hello správné nastavení, můžete začít hello kroky v hello [určení serveru DNS](#dns) části.

### <a name="toocreate-a-virtual-network"></a>toocreate virtuální sítě
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Přidání dalšího adresního prostoru a vytvoření podsítí
Po vytvoření virtuální sítě můžete přidat další adresní prostor a vytvořit podsítě.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Vytvoření podsítě brány
Před připojením tooa brány virtuální sítě, musíte nejdřív podsíť brány hello toocreate toowhich hello virtuální sítě se má tooconnect. Pokud je to možné je nejlepší toocreate podsíť brány pomocí blok CIDR/28 nebo /27 v pořadí tooprovide dost IP adres tooaccommodate další budoucí konfigurační požadavky.

Pokud vytváříte tuto konfiguraci jako cvičení, přečtěte si téma toothese [příklad nastavení](#values) při vytváření podsítě brány.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="toocreate-a-gateway-subnet"></a>toocreate podsíť brány
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Určení serveru DNS (volitelné)
DNS není pro připojení VNet-to-VNet vyžadováno. Pokud chcete toohave překladu pro prostředky, které jsou nasazené tooyour virtuální sítě, ale měli určit DNS server. Toto nastavení umožňuje určit server DNS hello má toouse pro překlad názvů pro tuto virtuální síť. Neslouží k vytvoření serveru DNS.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Vytvoření brány virtuální sítě
V tomto kroku vytvoříte bránu virtuální sítě hello sítě vnet. Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na vybrané skladová položka brány hello. Pokud vytváříte tuto konfiguraci jako cvičení, můžete se podívat toohello [příklad nastavení](#values).

### <a name="toocreate-a-virtual-network-gateway"></a>toocreate bránu virtuální sítě
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Vytvoření a konfigurace virtuální sítě TestVNet4
Po konfiguraci virtuální sítě TestVNet1 vytvořte virtuální síť TestVNet4 opakováním předchozích kroků hello, nahraďte hello hodnoty s těmi, která z virtuální sítě TestVNet4. Nepotřebujete toowait dokud hello brány virtuální sítě pro virtuální síť TestVNet1 dokončí vytváření před konfigurací virtuální sítě TestVNet4. Pokud použijete vlastní hodnoty, ujistěte se, že hello adresní prostory nepřekrývají s žádným z hello virtuální sítě, který chcete tooconnect k.

## <a name="TestVNet1Connection"></a>7. Nakonfigurujte připojení hello virtuální sítě TestVNet1
Když jste dokončili hello brány virtuální sítě pro virtuální síť TestVNet1 a virtuální sítě TestVNet4, můžete vytvořit virtuální síť připojení brány. V této části vytvoříte připojení z VNet1 tooVNet4. Tento postup funguje pouze pro virtuální sítě v hello stejné předplatné. Pokud vaše virtuální sítě jsou v různých předplatných, musíte použít PowerShell toomake hello připojení. V tématu hello [prostředí PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) článku.

1. V **všechny prostředky**, přejděte toohello brány virtuální sítě pro virtuální síť. Například **TestVNet1GW**. Klikněte na tlačítko **TestVNet1GW** okno brány virtuální sítě hello tooopen.
   
    ![Okno Připojení](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Okno Připojení")
2. Klikněte na tlačítko **+ přidat** tooopen hello **přidat připojení** okno.
3. Na hello **přidat připojení** okno, v poli Název hello, zadejte název připojení. Například **TestVNet1toTestVNet4**.
   
    ![Název připojení](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Název připojení")
4. Jako **Typ připojení** Vyberte **VNet-to-VNet** z rozevíracího seznamu hello.
5. Hello **první Brána virtuální sítě** hodnota pole je automaticky vyplněno vzhledem k tomu, že vytváříte toto připojení z brány hello zadané virtuální síti.
6. Hello **druhá Brána virtuální sítě** pole je Brána virtuální sítě hello hello virtuální sítě, které mají toocreate připojení k. Klikněte na tlačítko **zvolte další bránu virtuální sítě** tooopen hello **zvolte bránu virtuální sítě** okno.
   
    ![Přidání připojení](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Přidání připojení")
7. Brány virtuální sítě hello zobrazení uvedené v tomto okně. Všimněte si, že jsou uvedené pouze brány virtuálních sítí v rámci vašeho předplatného. Pokud chcete bránu virtuální sítě tooa tooconnect, který není v rámci vašeho předplatného, použijte prosím hello [prostředí PowerShell článku](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Klikněte na tlačítko hello brány virtuální sítě, který chcete tooconnect k.
9. V hello **sdílený klíč** pole, zadejte sdílený klíč pro připojení. Tento klíč si můžete vygenerovat nebo vytvořit sami. V připojení site-to-site hello klávesu, kterou by být přesně hello stejné pro místní zařízení a připojení brány virtuální sítě. Koncept Hello je podobný tady, vyjma toho, že místo připojování tooa zařízení VPN, se připojujete tooanother brány virtuální sítě.
   
    ![Sdílený klíč](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Sdílený klíč")
10. Klikněte na tlačítko **OK** v hello dolní části okna toosave hello změny.

## <a name="TestVNet4Connection"></a>8. Nakonfigurujte připojení hello virtuální sítě TestVNet4
Dále vytvořte připojení z virtuální sítě TestVNet4 tooTestVNet1. Použití hello stejné metoda, kterou jste použili toocreate hello připojení z virtuální sítě TestVNet1 tooTestVNet4. Ujistěte se, že používáte hello stejný sdílený klíč.

## <a name="VerifyConnection"></a>9. Ověření stavu připojení
Ověření připojení hello. Pro každou bránu virtuální sítě hello následující:

1. Vyhledejte hello okno pro bránu virtuální sítě hello. Například **TestVNet4GW**. 
2. V okně brány virtuální sítě hello, klikněte na tlačítko **připojení** tooview hello připojení okno pro bránu virtuální sítě hello.

Zobrazit hello připojení a ověřte stav hello. Po vytvoření hello připojení, uvidíte **úspěšné** a **připojeno** jako hello hodnoty stavu.

![Úspěšné](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Úspěšné")

Poklepáním na každé připojení samostatně tooview Další informace o připojení hello.

![Základy](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Základy")

## <a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet
Další informace o připojení VNet-to-VNet v podrobnostech hello – nejčastější dotazy.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. V tématu hello [virtuální počítače dokumentaci](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) Další informace.
