---
title: "Připojení klasické virtuální sítě tooAzure virtuální sítě Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate připojení VPN mezi klasické virtuální sítě a virtuální sítě Resource Manager pomocí brány sítě VPN a prostředí PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 8b1cf6ae4becf1829fa99961c5dd09a422fcc1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Připojení virtuálních sítí z různých modelů nasazení pomocí PowerShellu



Tento článek ukazuje, jak tooconnect klasické virtuální sítě tooResource Správce virtuálních sítí tooallow hello prostředků umístěných v hello samostatné nasazení modely toocommunicate mezi sebou. Hello kroky v tomto článku pomocí prostředí PowerShell, ale můžete také vytvořit této konfigurace pomocí portálu Azure hello výběrem hello článek z tohoto seznamu.

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Připojení klasické virtuální sítě tooa virtuální sítě Resource Manageru je podobné tooconnecting umístění lokality tooan místní virtuální síť. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Můžete vytvořit připojení mezi virtuální sítě, které jsou v různých předplatných a v různých oblastech. Virtuální sítě, které už máte připojení tooon místní sítě, můžete také připojit, pokud je hello brány, které byly nakonfigurovány k dynamické nebo založené na trasách. Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku. 

Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooinstead zvažte připojení pomocí virtuální sítě partnerský vztah. Partnerské vztahy virtuálních sítí nepoužívají bránu VPN. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md). 

## <a name="before-beginning"></a>Před zahájením

Hello následující postup vás provede procesem hello nastavení potřebné tooconfigure bránu dynamické nebo založené na směrování pro každou virtuální síť a vytvoření připojení VPN mezi bránami hello. Tato konfigurace nepodporuje statickou nebo na základě zásad brány.

### <a name="prerequisites"></a>Požadavky

* Již byly vytvořeny obě virtuální sítě.
* Hello rozsahy adres pro hello virtuální sítě není navzájem překrývají, nebo překrývat s žádným z rozsahů hello pro další připojení, které hello brány může být připojen k.
* Nainstalovali jste nejnovější rutiny prostředí PowerShell hello. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace. Ujistěte se, že instalujete hello služby správy (SM) a hello rutiny Resource Manager (RM). 

### <a name="exampleref"></a>Příklady nastavení

Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.

**Klasické nastavení sítě VNet**

Název virtuální sítě = ClassicVNet <br>
Umístění = západní USA <br>
Adresní prostory virtuální sítě = 10.0.0.0/24 <br>
Podsíť 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Název místní sítě = RMVNetLocal <br>
GatewayType = DynamicRouting

**Nastavení virtuální sítě Resource Manageru**

Název virtuální sítě = RMVNet <br>
Skupina prostředků = RG1 <br>
Virtuální síť adresní prostory IP adres = 192.168.0.0/16 <br>
Podsíť 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Umístění = východní USA <br>
Název veřejné IP adresy brány = gwpip <br>
Brána místní sítě = ClassicVNetLocal <br>
Název virtuální síťová brána = RMGateway <br>
Konfigurace adresování IP brány = gwipconfig

## <a name="createsmgw"></a>Oddíl 1 – konfigurace hello klasické virtuální sítě
### <a name="part-1---download-your-network-configuration-file"></a>Část 1 – stažení souboru konfigurace sítě
1. Přihlaste se tooyour účet Azure v konzole PowerShell hello se zvýšenými oprávněními. Hello následující rutiny vás vyzve k zadání hello přihlašovací údaje pro účet Azure. Po přihlášení stahování nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell. Hello toocomplete rutiny prostředí PowerShell SM použijete tuto část konfigurace hello.

  ```powershell
  Add-AzureAccount
  ```
2. Exportujte konfiguračního souboru sítě Azure tak, že spustíte následující příkaz hello. Hello umístění hello tooexport tooa jiné umístění souboru v případě potřeby můžete změnit.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. Soubor otevřete hello .xml, který jste si stáhli tooedit ho. Příklad konfiguračního souboru hello sítě, naleznete v části hello [schéma konfigurace sítě](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="part-2--verify-hello-gateway-subnet"></a>Část 2 - Ověřte podsíť brány hello
V hello **VirtualNetworkSites** elementu, přidejte tooyour podsíť brány virtuální sítě, pokud dosud nebyla vytvořena. Při práci s hello sítě konfigurační soubor, hello podsíť brány musí mít název "GatewaySubnet" nebo Azure nelze rozpoznat a používejte ho jako podsíť brány.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Příklad:**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-hello-local-network-site"></a>Část 3 – Přidání hello místní síťový Web
Hello místní síťový web, které přidáte představuje hello chcete tooconnect toowhich RM virtuální sítě. Přidat **LocalNetworkSites** element toohello soubor, pokud dosud neexistuje. V tomto okamžiku v konfiguraci hello hello VPNGatewayAddress může být jakékoli platná veřejná IP adresa protože jsme ještě nevytvořili hello brány pro hello virtuální sítě Resource Manageru. Jakmile vytvoříme hello brány, jsme nahraďte tuto IP adresu zástupný symbol hello správné veřejnou IP adresu, která byla přiřazena toohello RM brány.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-hello-vnet-with-hello-local-network-site"></a>Součástí 4 – hello virtuální síť přidružit hello místní síťový Web
V této části určíme hello místní síťový web, který má tooconnect hello VNet-to. V takovém případě je hello virtuální sítě Resource Manageru, který jste dříve odkazuje. Ujistěte se, zda text hello názvy shodovat. Tento krok není vytvořit bránu. Určuje, zda text hello místní síť, kterou hello brány se budou připojovat k.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-hello-file-and-upload"></a>Část 5 – hello soubor uložte a nahrajte
Uložte soubor hello a potom ho importovat tooAzure tak, že spustíte následující příkaz hello. Ujistěte se, že změníte cestu k souboru hello podle potřeby pro vaše prostředí.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Zobrazí se podobné výsledek zobrazující, že hello import proběhlo úspěšně.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-hello-gateway"></a>Část 6 – Vytvoření brány hello

Před spuštěním tohoto příkladu, najdete v toohello sítě konfigurační soubor, který jste stáhli pro hello přesný názvů této Azure očekává toosee. Hello sítě konfigurační soubor obsahuje hello hodnoty pro klasické virtuální sítě. Někdy hello hello názvy pro klasické virtuální sítě jsou změnit v konfiguračním souboru na hello sítě, při vytváření klasických nastavení virtuální sítě v portálu Azure kvůli toohello rozdíly v modelech nasazení hello. Například pokud jste použili Azure portálu toocreate klasické virtuální síti s názvem klasické virtuální sítě a vytvořit ve skupině prostředků s názvem "ClassicRG" hello, hello název, který je obsažen v souboru konfigurace sítě hello je převeden too'Group ClassicRG klasické virtuální sítě ". Při zadávání hello název virtuální sítě, který obsahuje mezery, uzavřete hello hodnotu do uvozovek.


Použijte následující příklad toocreate brány dynamického směrování hello:

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

Stav hello hello brány můžete zkontrolovat pomocí hello **Get-AzureVNetGateway** rutiny.

## <a name="creatermgw"></a>Část 2: Konfigurace hello bránu RM VNet
toocreate brána sítě VPN pro hello RM sítě VNet, postupujte podle pokynů hello. Nespouštějte hello kroky až po načtení hello veřejné IP adresy pro bránu hello klasické virtuální sítě. 

1. Přihlaste se tooyour účet Azure v konzole PowerShell hello. Hello následující rutiny vás vyzve k zadání hello přihlašovací údaje pro účet Azure. Po přihlášení, se stáhnou nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell.

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  Získání seznamu předplatné Azure, pokud máte více než jedno předplatné.

  ```powershell
  Get-AzureRmSubscription
  ```
   
  Zadejte hello předplatné, které chcete toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Vytvoření brány místní sítě. Ve virtuální síti hello brány místní sítě obvykle odkazuje tooyour místní umístění. V takovém případě brána místní sítě hello odkazuje tooyour klasické virtuální sítě. Zadejte jeho název, pomocí kterého můžete Azure naleznete tooit a také zadat předponu adresního prostoru hello. Azure používá předpona IP adresy hello zadáte tooidentify které tooyour toosend provoz místní umístění. Pokud potřebujete informace tooadjust hello později, před vytvořením brány, můžete upravit hodnoty hello a spuštění hello ukázková znovu.
   
   **-Název** je název hello chcete tooassign toorefer toohello místní síťová brána.<br>
   **-AddressPrefix** je hello adresní prostor vaší klasické virtuální sítě.<br>
   **-GatewayIpAddress** je hello veřejnou IP adresu brány hello klasické virtuální sítě. Ujistěte se, že toochange hello následující ukázka tooreflect hello správnou IP adresu.<br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. Požádat o veřejné IP adresy toobe přidělené toohello bránu virtuální sítě pro hello virtuální sítě Resource Manageru. Nelze zadat, které chcete toouse hello IP adresu. Hello IP adresa se přidělí dynamicky toohello brány virtuální sítě. Však neznamená to hello změny IP adresy. Změna IP adresy hello virtuální sítě brány je při hello brány jenom jednou Hello je odstraněno a znovu vytvořeno. Nezmění, v rámci změny velikosti, resetování nebo jiné operace údržby/upgradu hello brány.

  V tomto kroku jsme také nastavit proměnné, která se používá v pozdější fázi.

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. Ověřte, že virtuální sítě má podsíť brány. Pokud neexistuje žádná podsíť brány, přidáte. Zajistěte, aby podsíť brány hello jmenuje *GatewaySubnet*.
5. Načtěte hello podsítě používané pro bránu hello tak, že spustíte následující příkaz hello. V tomto kroku jsme také nastavit proměnnou toobe použije v dalším kroku hello.
   
   **-Název** je hello název virtuální sítě Resource Manager.<br>
   **-ResourceGroupName** je skupina prostředků hello této hello je přidružený virtuální sítě. podsíť brány Hello již musí existovat pro tuto virtuální síť a musí mít název *GatewaySubnet* toowork správně.<br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. Vytvoření konfigurace adresování IP brány hello. Hello konfigurace brány definuje podsíť hello a toouse hello veřejnou IP adresu. Použijte následující ukázka toocreate hello vlastní konfiguraci brány.

  V tomto kroku hello **- SubnetId** a **- PublicIpAddressId** parametry musí být předán vlastnost id hello z hello podsíť a IP adresu objekty, v uvedeném pořadí. Nelze použít jednoduchý řetězec. Tyto proměnné se nastavují v kroku toorequest hello veřejnou IP adresu a hello krok tooretrieve hello podsítě.

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. Vytvoření brány virtuální sítě Resource Manager hello tak, že spustíte následující příkaz hello. Hello `-VpnType` musí být *RouteBased*. Může trvat 45 minut nebo déle pro toocreate hello brány.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. Po vytvoření brány VPN hello, zkopírujte hello veřejnou IP adresu. Použijete jej při konfiguraci nastavení místní sítě hello pro klasické virtuální síti. Můžete použít následující rutiny tooretrieve hello veřejnou IP adresu hello. Hello veřejná IP adresa je uvedena v hello návratový jako *IpAddress*.

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-hello-classic-vnet-local-site-settings"></a>Část 3: Úprava hello klasické virtuální sítě místní lokality nastavení

V této části můžete pracovat s hello klasické virtuální sítě. Nahradíte hello zástupný symbol IP adresu, která jste použili při zadávání nastavení hello místní lokality, které budou použité tooconnect toohello brány virtuální sítě Resource Manageru. 

1. Exportujte hello sítě konfigurační soubor.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Pomocí textového editoru, upravte hodnotu hello pro prvek VPNGatewayAddress. Nahraďte hello veřejnou IP adresu brány Resource Manager hello hello zástupný symbol IP adresu a potom uložte změny hello.

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. Import hello upravit tooAzure soubor konfigurace sítě.

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <a name="connect"></a>Oddíl 4: Vytvoření připojení mezi bránami hello
Vytvoření připojení mezi bránami hello vyžaduje rozhraní PowerShell. Může být nutné tooadd váš účet Azure toouse hello klasickou verzi rutin prostředí PowerShell hello. Ano, použít toodo **Add-AzureAccount**.

1. V konzole PowerShell text hello nastavte sdílený klíč. Před spuštěním rutin hello, najdete v toohello sítě konfigurační soubor, který jste stáhli pro hello přesný názvů této Azure očekává toosee. Při zadávání hello název virtuální sítě, který obsahuje mezery, použijte jednoduché uvozovky hello hodnotu.<br><br>V následujícím příkladu **- VNetName** je název hello hello klasické virtuální sítě a **- LocalNetworkSiteName** je název hello jste zadali pro hello místní síťový Web. Hello **- SharedKey** je hodnota, která můžete vygenerovat a zadat. V příkladu hello jsme použili 'abc123', ale můžete vygenerovat a použít něco složitější. Důležité: co je tuto hodnotu hello, který zde určíte Hello musí být hello stejnou hodnotu, při vytváření připojení zadejte v dalším kroku hello. Hello návratový by měl zobrazit **stav: úspěšné**.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. Vytvořte připojení VPN hello spuštěním hello následující příkazy:
   
  Nastavení proměnných hello.

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  Vytvořte připojení hello. Všimněte si, že hello **- ConnectionType** je protokol IPsec, není Vnet2Vnet.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a>Část 5: Ověření připojení

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a>tooverify hello připojení z klasické virtuální sítě tooyour virtuální sítě Resource Manageru

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>portál Azure

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a>tooverify hello připojení z vaší virtuální sítě Resource Manageru tooyour klasické virtuální sítě

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>portál Azure

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

