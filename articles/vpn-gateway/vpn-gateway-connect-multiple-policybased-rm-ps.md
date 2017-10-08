---
title: "Připojení na základě zásad zařízení VPN serveru Azure VPN Gateway toomultiple místní: Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede konfigurací Azure založené na trasách zařízení brány VPN toomultiple na základě zásad sítě VPN pomocí Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/27/2017
ms.author: yushwang
ms.openlocfilehash: 866c78d96305207106a66cc3300c355e4b6bfbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-vpn-gateways-toomultiple-on-premises-policy-based-vpn-devices-using-powershell"></a>Připojení Azure VPN Gateway toomultiple místně na základě zásad zařízení VPN pomocí prostředí PowerShell

Tento článek vám pomůže nakonfigurovat Azure založené na trasách zařízení brány VPN tooconnect toomultiple místně na základě zásad VPN využívat vlastní zásady protokolu IPsec/IKE na připojení k síti S2S VPN.

## <a name="about-policy-based-and-route-based-vpn-gateways"></a>O zásadové a trasové brány VPN

Zásady - *oproti* založené na trasách zařízení VPN se liší v jak selektory provoz protokolu IPsec hello jsou nastaveny na připojení:

* **Na základě zásad** zařízení VPN použijte hello kombinace předpon z obou sítě toodefine jak provoz je zašifrovaná nebo dešifrovat do tunelových propojení IPsec. Obvykle je založen na zařízení brány firewall, které provádějí filtrování paketů. Protokol IPsec tunel šifrování a dešifrování se přidají filtrování paketů toohello a modul zpracování.
* **Založené na trasách** zařízení VPN použijte selektory provoz any-to-any (zástupný znak) a umožňují směrování/předávání tabulky tunelových propojení IPsec toodifferent přímé přenosy. Obvykle je založen na platformách směrovače, kde je každý tunelu IPsec modelován jako síťové rozhraní nebo VTI (virtuální tunel rozhraní).

Hello následující diagramy zvýrazněte dva modely hello:

### <a name="policy-based-vpn-example"></a>Příklad sítě VPN založené na zásadách
![na základě zásad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Příklad sítě VPN založené na směrování
![založené na směrování](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Podpora Azure pro sítě VPN založené na zásadách
V současné době Azure podporuje oba režimy brány sítě VPN: brány sítě VPN založené na směrování a bran VPN na základě zásad. Tyto šablony jsou sestaveny na různých platformách interní, které mít za následek jiné specifikace:

|                          | **Brána sítě VPN PolicyBased** | **Brána sítě VPN RouteBased**               |
| ---                      | ---                         | ---                                      |
| **Služba Azure Gateway SKU**    | Basic                       | Basic, Standard, HighPerformance         |
| **Verze IKE**          | IKEv1                       | IKEv2                                    |
| **Max. Připojení S2S** | **1**                       | Basic nebo Standard: 10<br> HighPerformance: 30 |
|                          |                             |                                          |

Pomocí vlastních zásad protokolu IPsec/IKE hello teď můžete konfigurovat Azure založené na směrování VPN Gateway toouse provozu na základě předpony selektory s možností "**PolicyBasedTrafficSelectors**", zařízení sítě VPN založené na zásadách místní tooon tooconnect. Díky této funkci můžete tooconnect z virtuální sítě Azure a toomultiple brány VPN místně na základě zásad zařízení VPN nebo brány firewall hello jednoho připojení limit odebráním hello aktuální Azure na základě zásad brány sítě VPN.

> [!IMPORTANT]
> 1. tooenable toto připojení, musí podporovat zařízení sítě VPN založené na zásadách místní **IKEv2** tooconnect toohello Azure brány sítě VPN založené na trasách. Zkontrolujte dokumentaci sítě VPN.
> 2. připojení prostřednictvím zařízení VPN založené na zásadách s Tento mechanismus Hello místní sítě lze připojit pouze toohello virtuální síť Azure; **jejich nelze přenosu tooother místní sítě nebo virtuální sítě prostřednictvím hello tutéž bránu Azure VPN**.
> 3. možnost konfigurace Hello je součástí hello vlastní zásady protokolu IPsec/IKE připojení. Pokud povolíte možnost selektoru hello provozu na základě zásad, musíte zadat dokončení zásad hello (algoritmy šifrování a integrity protokolu IPsec/IKE, klíče síly a životnost přidružení zabezpečení).

Hello následující diagram znázorňuje, proč směrování přenosu prostřednictvím brány Azure VPN nefunguje s možností na základě zásad hello:

![na základě zásad přenosu](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Jak je vidět v diagramu hello, má hello Azure VPN gateway selektory provoz z virtuální sítě tooeach hello předpony hello místní sítě, ale není hello křížové připojení předpony. Například na místní lokalitu 2, 3 lokality a lokality 4 může každý komunikovat tooVNet1 v uvedeném pořadí, ale nemůže připojit prostřednictvím hello Azure VPN gateway tooeach jiné. Hello diagram znázorňuje hello provoz selektory, které nejsou k dispozici v bráně Azure VPN hello v této konfiguraci připojení mezi.

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a>Konfigurace založená na zásadách provoz selektory na připojení

Hello pokyny v této postupujte podle článku hello stejné příklad, jak je popsáno v [zásady Konfigurace protokolu IPsec/IKE pro připojení S2S nebo VNet-to-VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish připojení S2S VPN. To je ukázáno v následujícím diagramu hello:

![zásady s2s](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

Hello pracovního postupu tooenable tyto možnosti připojení:
1. Vytvoření virtuální sítě hello, brána sítě VPN a bránu místní sítě pro připojení mezi různými místy
2. Vytvoření zásady protokolu IPsec/IKE
3. Použít zásady hello při vytváření připojení S2S nebo VNet-to-VNet a **povolit hello provozu na základě zásad selektory** hello připojení
4. Pokud hello připojení je již vytvořený, můžete použít nebo aktualizovat existující připojení tooan hello zásad

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a>Povolit provoz na základě zásad selektory na připojení

Ujistěte se, když jste dokončili [část 3 článku zásady Konfigurace protokolu IPsec/IKE hello](vpn-gateway-ipsecikepolicy-rm-powershell.md) pro tento oddíl. Následující příklad používá Hello hello stejné parametry a kroky:

### <a name="step-1---create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>Krok 1 – Vytvoření hello virtuální sítě, brána sítě VPN a bránu místní sítě

#### <a name="1-declare-your-variables--connect-tooyour-subscription"></a>1. Deklarace proměnných & připojení tooyour odběru
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
toouse hello rutiny Resource Manager, ujistěte se, že jste přešli tooPowerShell režimu. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

Otevřete konzolu prostředí PowerShell a připojte tooyour účtu. Použijte následující ukázka toohelp, ke kterým se připojujete hello:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Vytvoření virtuální sítě hello, brána sítě VPN a bránu místní sítě
Hello následující ukázka vytvoří hello virtuální sítě, virtuální sítě TestVNet1 s tři podsítě a brány VPN hello. Při nahrazování hodnot je důležité vždy pojmenovat podsítě brány konkrétně GatewaySubnet. Pokud použijete jiný název, vytvoření brány se nezdaří.

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

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>Krok 2 – Vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE

#### <a name="1-create-an-ipsecike-policy"></a>1. Vytvoření zásady protokolu IPsec/IKE

> [!IMPORTANT]
> Je nutné toocreate zásady protokolu IPsec/IKE v pořadí tooenable možnost "UsePolicyBasedTrafficSelectors" hello připojení.

Hello následující příklad vytvoří zásady protokolu IPsec/IKE s tyto algoritmy a parametry:
* IKEv2: DHGroup24 AES256, SHA384
* Protokol IPsec: AES256, SHA256, PFS24 SA životnost 3600 sekund & 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. Vytvoření připojení S2S VPN hello s selektory provozu na základě zásad a zásad protokolu IPsec/IKE
Vytvoření připojení k síti VPN S2S a použití zásady protokolu IPsec/IKE hello vytvořili v předchozím kroku hello. Mějte na paměti další parametru hello "-UsePolicyBasedTrafficSelectors $True" což umožňuje na základě zásad provoz selektory hello připojení.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Po dokončení kroků hello hello připojení S2S VPN používat hello definované zásady protokolu IPsec/IKE a povolit provoz na základě zásad selektory hello připojení. Můžete opakovat hello stejné kroky tooadd další připojení tooadditional místně na základě zásad zařízení VPN z hello tutéž bránu Azure VPN.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>Aktualizace na základě zásad provoz selektory pro připojení
poslední část Hello ukazuje, jak tooupdate hello provozu na základě zásad selektory možnost pro existující připojení S2S VPN.

### <a name="1-get-hello-connection"></a>1. Získání připojení hello
Získáte prostředek připojení hello.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-hello-policy-based-traffic-selectors-option"></a>2. Zkontrolujte možnost selektory hello provozu na základě zásad
Hello následující řádek ukazuje, zda hello provozu na základě zásad selektory se používají pro připojení hello:

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

Pokud vrátí řádek hello "**True**", pak selektory založené na zásadách provozu se konfigurují na hello připojení; jinak vrátí hodnotu "**False**."

### <a name="3-update-hello-policy-based-traffic-selectors-on-a-connection"></a>3. Aktualizovat hello selektory provozu na základě zásad v připojení
Po získání hello připojení prostředků můžete povolit nebo zakázat možnost hello.

#### <a name="disable-usepolicybasedtrafficselectors"></a>Zakázat UsePolicyBasedTrafficSelectors
Hello následující příklad zakazuje možnost selektory hello provozu na základě zásad, ale nechá hello zásad protokolu IPsec/IKE beze změny:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a>Povolit UsePolicyBasedTrafficSelectors
Hello následující příklad povolí možnost selektory hello provozu na základě zásad, ale nechá hello zásad protokolu IPsec/IKE beze změny:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Také zkontrolujte [zásady Konfigurace protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) Další informace o vlastních zásad protokolu IPsec/IKE.
