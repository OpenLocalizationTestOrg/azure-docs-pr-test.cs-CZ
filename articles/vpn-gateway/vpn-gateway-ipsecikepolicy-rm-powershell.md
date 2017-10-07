---
title: "Konfigurace zásad protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet: Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede procesem konfigurace zásad protokolu IPsec/IKE pro připojení S2S nebo VNet-to-VNet s Azure VPN Gateway pomocí Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: f8d2e29276efdec7071f2aa0d463b1abd64a5253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>Konfigurace zásad protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet

Tento článek vás provede kroky tooconfigure hello zásady protokolu IPsec/IKE pro připojení Site-to-Site VPN nebo VNet-to-VNet pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.

## <a name="about"></a>O parametry zásad protokolu IPsec a IKE pro Azure VPN Gateway
Protokol IPsec a IKE standardní podporuje širokou škálu kryptografických algoritmů v různých kombinacích. Odkazovat příliš[o kryptografických požadavky a brány Azure VPN](vpn-gateway-about-compliance-crypto.md) toosee, jak to může pomoct zajistit mezi různými místy a připojení VNet-to-VNet splňují vaše požadavky na dodržování předpisů nebo zabezpečení.

Tento článek obsahuje pokyny toocreate a konfigurovat zásady protokolu IPsec/IKE a použít tooa nové nebo existující připojení:

* [Část 1 - toocreate pracovního postupu a nastavit zásady protokolu IPsec/IKE](#workflow)
* [Část 2 - podporované kryptografické algoritmy a klíče síly](#params)
* [Část 3 – vytvoření nového připojení S2S VPN pomocí zásad protokolu IPsec/IKE](#crossprem)
* [Součástí 4 – vytvoření nového připojení VNet-to-VNet s zásad protokolu IPsec/IKE](#vnet2vnet)
* [Část 5 – spravovat (vytvořit, přidat, odebrat) zásady protokolu IPsec/IKE pro připojení](#managepolicy)

> [!IMPORTANT]
> 1. Všimněte si, že zásady protokolu IPsec/IKE funguje pouze na hello následující SKU brány:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (trasové)
>    * ***Standardní*** a ***HighPerformance*** (trasové)
> 2. Pro jedno připojení můžete zadat pouze ***jednu*** kombinaci zásad.
> 3. Je nutné zadat všechny algoritmy a parametry pro IKE (hlavní režim) a protokolu IPsec (rychlý režim). Zadání částečných zásad není povoleno.
> 4. Poraďte se s VPN dodavatele dokumentaci zásad tooensure hello je podporované na vaše místní zařízení VPN. S2S nebo připojení VNet-to-VNet nejde vytvořit, pokud jsou zásady hello nekompatibilní.

## <a name ="workflow"></a>Část 1 - toocreate pracovního postupu a nastavit zásady protokolu IPsec/IKE
Tato část obsahuje přehled hello pracovního postupu toocreate a aktualizace protokolu IPsec/IKE zásady na připojení S2S VPN nebo VNet-to-VNet:
1. Vytvoření virtuální sítě a brány VPN
2. Vytvoření brány místní sítě pro mezi místní připojení nebo jinou virtuální sítí a Brána pro propojení VNet-to-vnet
3. Vytvoření zásady protokolu IPsec/IKE s vybranou algoritmů hash a parametry
4. Vytvoření připojení (protokol IPsec nebo VNet2VNet) pomocí zásad protokolu IPsec/IKE hello
5. Přidání nebo aktualizace nebo odebrání zásad protokolu IPsec/IKE pro existující připojení

Hello pokyny v tomto článku vám pomůže nastavit a konfigurovat zásady protokolu IPsec/IKE, jak je vidět v diagramu hello:

![zásady protokolu IPSec ike](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>Část 2 - podporované kryptografické algoritmy & klíče síly

Hello následující tabulka uvádí hello podporované kryptografické algoritmy a klíče síly konfigurovat zákazníci hello:

| **IPsec/IKEv2**  | **Možnosti**    |
| ---  | --- 
| Šifrování protokolem IKEv2 | AES256, AES192, AES128, DES3, DES  
| Integrita protokolu IKEv2  | SHA384, SHA256, SHA1, MD5  |
| Skupina DH         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None |
| Šifrování protokolem IPsec | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Žádné    |
| Integrita protokolu IPsec  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| Skupina PFS        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Žádná 
| Doba života přidružení zabezpečení v rychlém režimu   | (**Volitelné**: výchozí hodnoty jsou použít, pokud není zadána)<br>Sekundy (integer; **min. 300** / výchozí hodnota 27 000 sekund)<br>Kilobajty (integer; **min. 1024** / výchozí hodnota 102 400 000 kilobajtů)   |
| Selektor provozu | UsePolicyBasedTrafficSelectors ** ($True nebo $False; **Volitelné**, výchozí $False není-li zadána)    |
|  |  |

> [!IMPORTANT]
> 1. **Pokud GCMAES slouží jako algoritmus šifrování pomocí protokolu IPsec, je nutné vybrat hello stejné GCMAES algoritmus a délku klíče pro protokol IPsec Integrity; například pro obě pomocí GCMAES128**
> 2. Životnost přidružení zabezpečení hlavního režimu IKEv2 vyřešen v 28 800 sekund na branách Azure VPN hello
> 3. Nastavení "UsePolicyBasedTrafficSelectors" příliš$ True na připojení nakonfigurujete hello Azure VPN gateway tooconnect toopolicy VPN brány firewall založená na místní. Pokud povolíte PolicyBasedTrafficSelectors, je třeba tooensure hello odpovídající provoz selektory definovaný s všechny kombinace předponami vaší místní sítě (brány místní sítě) z předpony hello virtuální síť Azure, má vaše zařízení VPN místo any-to-any. Například pokud předponami vaší místní sítě se 10.1.0.0/16 a 10.2.0.0/16 a předponami vaší virtuální sítě jsou 192.168.0.0/16 a 172.16.0.0/16, je třeba toospecify hello následující selektory provozu:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

Další informace týkající se provozu na základě zásad selektory najdete v tématu [připojení více místně na základě zásad zařízení VPN](vpn-gateway-connect-multiple-policybased-rm-ps.md).

Následující tabulka seznamy Hello hello odpovídající skupin Diffie-Hellman podporovaná hello vlastní zásady:

| **Skupina Diffie-Hellman**  | **DHGroup**              | **PFSGroup** | **Délka klíče** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768bitová skupina MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024bitová skupina MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048bitová skupina MODP  |
| 19                        | ECP256                   | ECP256       | 256bitová skupina ECP    |
| 20                        | ECP384                   | ECP284       | 384bitová skupina ECP    |
| 24                        | DHGroup24                | PFS24        | 2048bitová skupina MODP  |

Odkazovat příliš[RFC3526](https://tools.ietf.org/html/rfc3526) a [RFC5114](https://tools.ietf.org/html/rfc5114) další podrobnosti.

## <a name ="crossprem"></a>Část 3 – vytvoření nového připojení S2S VPN pomocí zásad protokolu IPsec/IKE

Tato část vás provede kroky hello k vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE. Hello následujících kroků vytvořte připojení hello jak je znázorněno v diagramu hello:

![zásady s2s](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

V tématu [vytvoření připojení S2S VPN](vpn-gateway-create-site-to-site-rm-powershell.md) podrobnější podrobné pokyny pro vytvoření připojení S2S VPN.

### <a name="before"></a>Než začnete

* Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Nainstalujte rutiny Powershellu pro Azure Resource Manager hello. V tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.

### <a name="createvnet1"></a>Krok 1 – Vytvoření hello virtuální sítě, brána sítě VPN a bránu místní sítě

#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných

Pro toto cvičení začneme deklarací proměnných. Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí.

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Připojení tooyour odběru a vytvořit novou skupinu prostředků

Ujistěte se, že jste přešli tooPowerShell režimu toouse hello rutiny správce prostředků. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

Otevřete konzolu prostředí PowerShell a připojte tooyour účtu. Použijte následující ukázka toohelp, ke kterým se připojujete hello:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Vytvoření virtuální sítě hello, brána sítě VPN a bránu místní sítě

Hello následující ukázka vytvoří hello virtuální sítě, virtuální sítě TestVNet1, se tři podsítě a brány VPN hello. Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet. Pokud použijete jiný název, vytvoření brány se nezdaří.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="s2sconnection"></a>Krok 2 – Vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE

#### <a name="1-create-an-ipsecike-policy"></a>1. Vytvoření zásady protokolu IPsec/IKE

Následující ukázkový skript Hello vytvoří zásadu protokolu IPsec/IKE se hello následující parametry a algoritmy:

* IKEv2: DHGroup24 AES256, SHA384
* Protokol IPsec: AES256, SHA256, PFS24 7200 životnost přidružení zabezpečení sekund & 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

Pokud používáte GCMAES pro IPsec, musíte použít hello stejné GCMAES algoritmus a délku klíče pro šifrování pomocí protokolu IPsec a integritu, například:

* IKEv2: DHGroup24 AES256, SHA384
* Protokol IPsec: **GCMAES256, GCMAES256**, PFS24 7200 životnost přidružení zabezpečení sekund & 2048 KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-hello-ipsecike-policy"></a>2. Vytvoření připojení S2S VPN hello s hello zásad protokolu IPsec/IKE

Vytvoření připojení k síti VPN S2S a použití zásad protokolu IPsec/IKE hello vytvořili dříve.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Volitelně můžete přidat "-UsePolicyBasedTrafficSelectors $True" toohello vytvořit připojení rutiny tooenable Azure VPN gateway tooconnect na základě toopolicy zařízení VPN místně, jak je popsáno výše.

> [!IMPORTANT]
> Po připojení je zadán pro zásady protokolu IPsec/IKE, hello Azure VPN gateway pouze odeslat nebo přijmout hello protokolu IPsec/IKE návrh s zadané kryptografické algoritmy a klíče síly na konkrétní připojení. Zajistěte, aby vaše místní zařízení VPN pro připojení hello používá nebo přijímá hello kombinaci přesný zásad, jinak nebude vytvořit tunel S2S VPN hello.


## <a name ="vnet2vnet"></a>Součástí 4 – vytvoření nového připojení VNet-to-VNet s zásad protokolu IPsec/IKE

Hello kroky k vytvoření připojení VNet-to-VNet pomocí zásad protokolu IPsec/IKE jsou podobné toothat připojení S2S VPN. Hello následující ukázkové skripty vytvořit hello připojení jak je vidět v diagramu hello:

![zásady v2v](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

V tématu [vytvořit připojení VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md) podrobné kroky pro vytvoření připojení VNet-to-VNet. Je třeba provést [část 3](#crossprem) toocreate a konfigurace virtuální sítě TestVNet1 a hello brány VPN.

### <a name="createvnet2"></a>Krok 1 – Vytvoření hello druhý virtuální sítě a brány VPN

#### <a name="1-declare-your-variables"></a>1. Deklarace proměnných

Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-hello-second-virtual-network-and-vpn-gateway-in-hello-new-resource-group"></a>2. Vytvoření hello druhý virtuální sítě a brány VPN v hello novou skupinu prostředků

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-hello-ipsecike-policy"></a>Krok 2 – Vytvoření připojení virtuální sítě toVNet pomocí hello zásad protokolu IPsec/IKE

Podobně jako toohello připojení S2S VPN, vytvoření zásady protokolu IPsec/IKE potom použít toopolicy toohello nové připojení.

#### <a name="1-create-an-ipsecike-policy"></a>1. Vytvoření zásady protokolu IPsec/IKE

Následující ukázkový skript Hello vytvoří jiné zásady protokolu IPsec/IKE s hello následující parametry a algoritmy:
* IKEv2: DHGroup14 AES128, SHA1,
* Protokol IPsec: GCMAES128, GCMAES128, PFS14, životnost přidružení zabezpečení 7200 sekund a 4096KB

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-hello-ipsecike-policy"></a>2. Vytvoření připojení VNet-to-VNet s hello zásad protokolu IPsec/IKE

Umožňuje vytvořit připojení VNet-to-VNet a použít zásady protokolu IPsec/IKE hello, kterou jste vytvořili. V tomto příkladu jsou obě brány v hello stejného předplatného. Proto je možné toocreate a nakonfigurovat obě připojení s hello stejné zásady protokolu IPsec/IKE v hello stejné relace prostředí PowerShell.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Po připojení je zadán pro zásady protokolu IPsec/IKE, hello Azure VPN gateway pouze odeslat nebo přijmout hello protokolu IPsec/IKE návrh s zadané kryptografické algoritmy a klíče síly na konkrétní připojení. Ujistěte se, zda text hello zásady protokolu IPsec pro obě připojení jsou hello stejné, jinak nebude navázání připojení VNet-to-VNet.

Po dokončení těchto kroků, hello připojení za pár minut a budete mít hello následující topologie sítě, jak je znázorněno v začátku hello:

![zásady protokolu IPSec ike](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>Část 5 – zásady aktualizace protokolu IPsec/IKE pro připojení

poslední část Hello se dozvíte, jak toomanage zásad protokolu IPsec/IKE pro existující připojení S2S nebo VNet-to-VNet. cvičení Hello níže vás provede následující operace na připojení hello:

1. Zobrazit zásady protokolu IPsec/IKE hello připojení
2. Přidat nebo aktualizovat připojení tooa zásad protokolu IPsec/IKE hello
3. Odebrání zásad protokolu IPsec/IKE hello připojení

Hello stejný postup použijte tooboth S2S a připojení VNet-to-VNet.

> [!IMPORTANT]
> Zásady protokolu IPsec/IKE je podporována v *standardní* a *HighPerformance* založené na trasách pouze VPN Gateway. Nefunguje se na základní skladová položka brány hello nebo brány sítě VPN založené na zásadách hello.

#### <a name="1-show-hello-ipsecike-policy-of-a-connection"></a>1. Zobrazit zásady protokolu IPsec/IKE hello připojení

Hello následující příklad ukazuje, jak tooget hello zásady protokolu IPsec/IKE nakonfigurované na připojení. skripty Hello také pokračovat od hello cvičení výše.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

poslední příkaz Hello uvádí aktuální zásady protokolu IPsec/IKE hello nakonfigurované na hello připojení, pokud existuje. Následující ukázkový výstup Hello je pro připojení hello:

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

Pokud nejsou nakonfigurovány žádné zásady protokolu IPsec/IKE, hello příkazu (PS > $connection6.policy) získá návratový prázdná. Neznamená to protokolu IPsec/IKE není konfigurován na připojení hello, ale že se žádné vlastní zásady protokolu IPsec/IKE. skutečné připojení Hello používá výchozí zásady hello mezi místní zařízení VPN a hello brány Azure VPN.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Přidat nebo aktualizovat zásady protokolu IPsec/IKE pro připojení

Hello kroky tooadd novou zásadu nebo aktualizovat existující zásady na připojení jsou hello stejné: Vytvořte novou zásadu a použít hello nové zásady toohello připojení.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

tooenable "UsePolicyBasedTrafficSelectors" při připojování tooan místní zařízení VPN založené na zásadách přidat hello "-UsePolicyBaseTrafficSelectors" parametr toohello rutiny, nebo ji nastavte příliš$ False toodisable hello možnost:

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

Můžete získat hello připojení znovu toocheck Pokud hello zásada se aktualizuje.

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Výstup hello hello poslední řádek, byste měli vidět, jak je znázorněno v hello následující ukázka:

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : GCMAES128
IpsecIntegrity      : GCMAES128
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14--
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Odebrání připojení zásady protokolu IPsec/IKE

Vlastní zásady hello odebraný z připojení brány Azure VPN hello vrátí zpět toohello [výchozí seznam protokolu IPsec/IKE návrhy](vpn-gateway-about-vpn-devices.md) a obnoví vyjednávání znovu s vaše místní zařízení VPN.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

Můžete použít stejný skriptu toocheck text hello, pokud hello zásada byla odebrána z hello připojení.

## <a name="next-steps"></a>Další kroky

V tématu [připojení více místně na základě zásad zařízení VPN](vpn-gateway-connect-multiple-policybased-rm-ps.md) další podrobnosti týkající se provozu na základě zásad selektorů.

Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
