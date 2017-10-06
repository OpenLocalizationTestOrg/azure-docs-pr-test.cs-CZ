---
title: "Připojit virtuální síti Azure tooanother virtuální sítě: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede propojováním virtuálních sítí s použitím Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 2da30c76867cc3f71d040e63e0dd15d153e15c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a>Konfigurace připojení brány VPN typu VNet-to-VNet pomocí PowerShellu

Tento článek ukazuje, jak toocreate připojení k bráně VPN mezi virtuálními sítěmi. Hello virtuální sítě může být v hello stejné nebo různých oblastí, a z hello stejné nebo různých předplatných. Při připojování virtuální sítě z různých předplatných, odběry hello nemusí toobe přidružené hello stejné klienta služby Active Directory. 

Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager a pomocí prostředí PowerShell. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Propojení různých modelů nasazení – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Propojení různých modelů nasazení – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

Propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) je podobné tooconnecting umístění lokality tooan místní virtuální síť. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooconsider připojení pomocí virtuální sítě partnerský vztah. Partnerské vztahy virtuálních sítí nepoužívají bránu VPN. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).

Komunikaci typu VNet-to-VNet můžete kombinovat s konfiguracemi s více servery. To umožňuje vytvářet topologie sítí, které spojují připojení mezi různými místy s připojením propojování virtuálních sítí, jak je znázorněno v následujícím diagramu hello:

![Informace o připojeních](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a>Proč propojovat virtuální sítě?

Tooconnect virtuální sítě může být vhodné pro hello následujících důvodů:

* **Geografická redundance napříč oblastmi a geografická přítomnost**

  * Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.
  * Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure. Jedním z důležitých příkladů je tooset až SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.
* **Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**

  * Hello uvnitř stejné oblasti, můžete nastavit vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu tooisolation nebo požadavků na správu.

Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku.

## <a name="which-set-of-steps-should-i-use"></a>Kterou posloupnost kroků provést?

V tomto článku uvidíte dvě různé sady kroků. Jednu sadu kroky pro [hello virtuální sítě, které jsou umístěny ve stejné předplatné](#samesub)a druhý pro [patřící do různých předplatných](#difsub). Hello klíčovým rozdílem mezi sadami hello je jestli můžete vytvořit a nakonfigurovat všechny virtuální sítě a brány prostředky v rámci hello stejné relace prostředí PowerShell.

Hello kroky v tomto článku používají proměnné, které jsou deklarované v hello začátku každého oddílu. Pokud již pracujete s existující virtuální sítě, upravte hello proměnné tooreflect hello nastavení ve svém vlastním prostředí. Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="samesub"></a>Jak tooconnect virtuální sítě, jsou v hello stejného předplatného.

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Než začnete

Než začnete, musíte tooinstall hello nejnovější verzi rutin Powershellu pro Azure Resource Manager hello alespoň 4.0 nebo novější. Další informace o instalaci rutin prostředí PowerShell hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

### <a name="Step1"></a>Krok 1: Plánování rozsahů IP adres

V hello následující kroky vytvoříme dvě virtuální sítě spolu s jejich příslušnými podsítěmi brány a konfigurace. Poté vytvoříme připojení VPN mezi hello dvě virtuální sítě. Je důležité tooplan rozsahy IP adres hello pro konfiguraci sítě. Mějte na paměti, že je třeba zajistit, aby se žádné rozsahy virtuálních sítí ani místní síťové rozsahy žádným způsobem nepřekrývaly. V těchto příkladech nezahrnujeme server DNS. Pokud chcete překlad IP adres pro virtuální sítě, přečtěte si téma [Překlad IP adres](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Můžeme použít následující hodnoty v příkladech hello hello:

**Hodnoty pro virtuální síť TestVNet1:**

* Název virtuální sítě: TestVNet1
* Skupina prostředků: TestRG1
* Umístění: Východní USA
* TestVNet1: 10.11.0.0/16 a 10.12.0.0/16
* FrontEnd: 10.11.0.0/24
* BackEnd: 10.12.0.0/24
* GatewaySubnet: 10.12.255.0/27
* Název brány: VNet1GW
* Veřejná IP adresa: VNet1GWIP
* Typ sítě VPN: RouteBased
* Připojení (1 ke 4): VNet1toVNet4
* Připojení (1 k 5): VNet1toVNet5
* Typ připojení: VNet2VNet

**Hodnoty pro virtuální síť TestVNet4:**

* Název virtuální sítě: TestVNet4
* TestVNet2: 10.41.0.0/16 a 10.42.0.0/16
* FrontEnd: 10.41.0.0/24
* BackEnd: 10.42.0.0/24
* GatewaySubnet: 10.42.255.0/27
* Skupina prostředků: TestRG4
* Umístění: Západní USA
* Název brány: VNet4GW
* Veřejná IP adresa: VNet4GWIP
* Typ sítě VPN: RouteBased
* Připojení: VNet4toVNet1
* Typ připojení: VNet2VNet


### <a name="Step2"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1

1. Deklarujte proměnné. Tento příklad deklaruje hello proměnné pomocí hello hodnot pro toto cvičení. Ve většině případů má nahradit hello hodnoty vlastními. Tyto proměnné však můžete použít, pokud používáte prostřednictvím toobecome kroky hello seznámili s tímto typem konfigurace. Umožňuje změnit proměnné hello v případě potřeby pak zkopírujte a vložte je do konzoly prostředí PowerShell.

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
  $Location1 = "East US"
  $VNetName1 = "TestVNet1"
  $FESubName1 = "FrontEnd"
  $BESubName1 = "Backend"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.11.0.0/16"
  $VNetPrefix12 = "10.12.0.0/16"
  $FESubPrefix1 = "10.11.0.0/24"
  $BESubPrefix1 = "10.12.0.0/24"
  $GWSubPrefix1 = "10.12.255.0/27"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. Připojte tooyour účet. Použijte následující příklad toohelp, ke kterým se připojujete hello:

  ```powershell
  Login-AzureRmAccount
  ```

  Zkontrolujte předplatná hello pro účet hello.

  ```powershell
  Get-AzureRmSubscription
  ```

  Zadejte hello předplatné, které chcete toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. Vytvořte novou skupinu prostředků.

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. Vytvoření konfigurací podsítě pro virtuální síť TestVNet1 hello. Tato ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě: jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem BackEnd. Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet. Pokud použijete jiný název, vytvoření brány se nezdaří.

  Hello následující příklad používá hello proměnné, které jste nastavili dříve. V tomto příkladu používá podsíť brány hello 27. I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem minimálně/28 nebo /27. To vám umožní dostatek adresy tooaccommodate možné další konfigurace, které můžete ve hello budoucí.

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. Vytvořte virtuální síť TestVNet1.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. Požádat o veřejné IP adresy toobe přidělené toohello bránu, které vytvoříte pro virtuální síť. Všimněte si, že hello AllocationMethod je dynamický. Nelze zadat, které chcete toouse hello IP adresu. Je dynamicky přidělené tooyour brány. 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. Vytvoření konfigurace brány hello. Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu. Příklad toocreate hello používejte vlastní konfiguraci brány.

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. Vytvoření hello brány pro virtuální síť TestVNet1. V tomto kroku vytvoříte bránu virtuální sítě hello pro virtuální síť TestVNet1. Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased. Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na vybrané skladová položka brány hello.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a>Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4

Po konfiguraci virtuální sítě TestVNet1 vytvořte virtuální síť TestVNet4. Postupujte podle kroků hello níže nahraďte hello hodnoty vlastními v případě potřeby. Tento krok lze provést v rámci hello stejné relace prostředí PowerShell protože je v hello stejné předplatné.

1. Deklarujte proměnné. Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. Vytvořte novou skupinu prostředků.

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. Vytvoření konfigurací podsítě pro virtuální síť TestVNet4 hello.

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. Vytvořte virtuální síť TestVNet4.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. Vyžádejte si veřejnou IP adresu.

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. Vytvoření konfigurace brány hello.

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. Vytvoření brány virtuální sítě TestVNet4 hello. Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na vybrané skladová položka brány hello.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-hello-connections"></a>Krok 4 – vytvoření připojení hello

1. Získejte obě brány virtuální sítě. Pokud jsou obě brány hello v hello stejného předplatného, jako v příkladu hello, můžete použít tento krok v hello stejné relace prostředí PowerShell.

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1. V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet4. Zobrazí se sdílený klíč uváděný v příkladech hello. Můžete použít vlastní hodnoty pro sdílený klíč hello. pro obě připojení musí shodovat Hello důležité věc, je tento sdílený klíč hello. Vytvoření připojení může trvat malou chvíli toocomplete.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello. Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet4 tooTestVNet1. Zkontrolujte, zda text hello sdílené klíče shodují. Po několika minutách bude vytvořeno připojení Hello.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. Ověřte své propojení. Části hello [jak tooverify připojení](#verify).

## <a name="difsub"></a>Jak tooconnect virtuální sítě, jsou v různých předplatných

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

V tomto scénáři propojíme sítě TestVNet1 a TestVNet5. Virtuální sítě TestVNet1 a TestVNet5 patří do různých předplatných. odběry Hello nemusí toobe přidružené hello stejné klienta služby Active Directory. Hello rozdíl mezi tyto kroky a předchozí sadu hello je, že některé z kroků konfigurace hello nutné toobe provést v samostatné relaci prostředí PowerShell v kontextu hello hello druhého předplatného. Zejména při hello dva předplatná patří toodifferent organizace.

### <a name="step-5---create-and-configure-testvnet1"></a>Krok 5: Vytvoření a konfigurace virtuální sítě TestVNet1

Je třeba provést [kroku 1](#Step1) a [kroku 2](#Step2) z hello předchozí část toocreate a konfigurace virtuální sítě TestVNet1 a hello brána sítě VPN pro virtuální síť TestVNet1. Pro tuto konfiguraci nemůžete se vyžaduje toocreate virtuální sítě TestVNet4 z předchozí části hello, ale pokud ho vytvoříte, se nebude v konfliktu s tyto kroky. Po dokončení kroku 1 a 2 kroku v kroku 6 toocreate virtuální sítě TestVNet5 pokračujte. 

### <a name="step-6---verify-hello-ip-address-ranges"></a>Krok 6 – ověření hello rozsahy IP adres

Je důležité toomake jistotu, že prostor IP adres hello nové virtuální sítě TestVNet5 hello nepřekrývá s žádným z rozsahů virtuálních sítí ani rozsahů bran místních sítí. V tomto příkladu hello virtuální sítě může patřit toodifferent organizace. Pro tento postup můžete použít následující hodnoty pro hello virtuální sítě TestVNet5 hello:

**Hodnoty pro virtuální síť TestVNet5:**

* Název virtuální sítě: TestVNet5
* Skupina prostředků: TestRG5
* Umístění: Japonsko – východ
* TestVNet5: 10.51.0.0/16 a 10.52.0.0/16
* FrontEnd: 10.51.0.0/24
* BackEnd: 10.52.0.0/24
* GatewaySubnet: 10.52.255.0.0/27
* Název brány: VNet5GW
* Veřejná IP adresa: VNet5GWIP
* Typ sítě VPN: RouteBased
* Připojení: VNet5toVNet1
* Typ připojení: VNet2VNet

### <a name="step-7---create-and-configure-testvnet5"></a>Krok 7: Vytvoření a konfigurace virtuální sítě TestVNet5

Tento krok je třeba provést v kontextu hello hello nové předplatné. Tuto část může provést pomocí Správce hello v jiné organizaci, který vlastní hello předplatné.

1. Deklarujte proměnné. Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. Připojte toosubscription 5. Otevřete konzolu prostředí PowerShell a připojte tooyour účtu. Použijte následující ukázka toohelp, ke kterým se připojujete hello:

  ```powershell
  Login-AzureRmAccount
  ```

  Zkontrolujte předplatná hello pro účet hello.

  ```powershell
  Get-AzureRmSubscription
  ```

  Zadejte hello předplatné, které chcete toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. Vytvořte novou skupinu prostředků.

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. Vytvoření konfigurací podsítě pro virtuální sítě TestVNet5 hello.

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. Vytvořte virtuální síť TestVNet5.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. Vyžádejte si veřejnou IP adresu.

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. Vytvoření konfigurace brány hello.

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. Vytvoření brány virtuální sítě TestVNet5 hello.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-hello-connections"></a>Krok 8 – vytvoření připojení hello

V tomto příkladu protože hello brány jsou v hello různých předplatných, rozdělíme tento krok do dvou relací prostředí PowerShell označených [předplatné 1] a [předplatné 5].

1. **[Předplatné 1]**  Get hello brány virtuální sítě pro předplatné 1. Přihlaste se a připojit tooSubscription 1 před spuštěním hello následující ukázka:

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  Zkopírujte výstup hello hello následujících prvků a pošlete tyto toohello správci předplatného 5 prostřednictvím e-mailu nebo jiným způsobem.

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  Tyto dva prvky budou mít hodnoty podobné toohello následující příklad výstupu:

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. **[Předplatné 5]**  Get hello brány virtuální sítě pro předplatné 5. Přihlaste se a připojit tooSubscription 5 před spuštěním hello následující ukázka:

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  Zkopírujte výstup hello hello následující prvky a odesílat tyto toohello správci předplatného 1 prostřednictvím e-mailu nebo jiným způsobem.

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  Tyto dva prvky budou mít hodnoty podobné toohello následující příklad výstupu:

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. **[Předplatné 1]**  Vytvořit připojení hello tooTestVNet5 virtuální sítě TestVNet1. V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet5. Hello rozdíl zde spočívá v tom že $vnet5gw nelze získat přímo, protože je v jiném předplatném. Budete potřebovat toocreate nový objekt prostředí PowerShell s hello hodnotami zjištěnými z předplatného 1 v předchozích krocích hello. Použijte následující příklad hello. Nahraďte hello název, Id a sdílený klíč vlastními hodnotami. pro obě připojení musí shodovat Hello důležité věc, je tento sdílený klíč hello. Vytvoření připojení může trvat malou chvíli toocomplete.

  Připojte tooSubscription 1 před spuštěním hello následující ukázka:

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. **[Předplatné 5]**  Vytvoření připojení virtuální sítě TestVNet5 tooTestVNet1 hello. Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet5 tooTestVNet1. Hello stejný postup vytváření objektu prostředí PowerShell na základě hello hodnot zjištěných z předplatného 1 používá i zde. V tomto kroku se ujistěte, že hello sdílené klíče shodují.

  Připojte tooSubscription 5 před spuštěním hello následující ukázka:

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <a name="verify"></a>Jak tooverify připojení

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Další kroky

* Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. V tématu hello [virtuální počítače dokumentaci](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) Další informace.
* Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
