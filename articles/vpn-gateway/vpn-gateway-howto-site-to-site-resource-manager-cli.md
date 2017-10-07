---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN: rozhraní příkazového řádku | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky vám pomůžou vytvořit připojení VPN Gateway typu Site-to-Site mezi různými místy pomocí rozhraní příkazového řádku."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: c652cf2caf3928cdeb19d7dc329f6db101e5ed90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí rozhraní příkazového řádku

Tento článek ukazuje, jak toouse hello rozhraní příkazového řádku Azure toocreate připojení k bráně VPN Site-to-Site z vaší místní síti toohello virtuální sítě. Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:<br>

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Rozhraní příkazového řádku](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit. Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Než začnete

Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:

* Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho. Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).
* Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy. Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění. Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k.
* Ověřte, že jste nainstalovali nejnovější verzi rozhraní příkazového řádku hello (2.0 nebo novější). Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).

### <a name="example"></a>Příklady hodnot

Můžete použít následující hodnoty toocreate testovacím prostředí hello nebo najdete hodnoty toothese toobetter pochopit hello příklady v tomto článku:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Připojení tooyour odběru

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="rg"></a>2. Vytvoření skupiny prostředků

Hello následující příklad vytvoří skupinu prostředků s názvem 'TestRG1"hello 'eastus' umístění. Pokud už máte skupinu prostředků v hello oblasti, které chcete toocreate virtuální síť, můžete použít než místo.

```azurecli
az group create --name TestRG1 --location eastus
```

## <a name="VNet"></a>3. Vytvoření virtuální sítě

Pokud ještě nemáte virtuální síť, vytvořte jednu pomocí hello [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz. Při vytváření virtuální sítě, ujistěte se, že hello adresní prostory, které zadáte nepřekrývají s hello adresní prostory, které máte ve vaší místní síti.

Hello následující příklad vytvoří virtuální síť s názvem "TestVNet1" a podsítě, 'Subnet1'.

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## 4. <a name="gwsub"></a>Vytvořit podsíť brány hello

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Pro tuto konfiguraci potřebujete také podsíť brány. Brána virtuální sítě Hello používá podsíť brány, který obsahuje hello IP adresy, které jsou používány hello služby brány VPN. Při vytváření podsítě brány je nutné ji pojmenovat GatewaySubnet. Pokud zadáte jiný název, vytvoříte sice podsíť, ale Azure ji nebude považovat za podsíť brány.

velikost Hello hello podsíť brány, který zadáte závisí na konfiguraci brány VPN hello, které chcete toocreate. I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem/27 nebo velikosti/28. Pomocí větší podsíť brány umožňuje dost IP adres tooaccommodate možné budoucí konfigurace.

Použití hello [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) podsíť brány hello toocreate příkaz.

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Vytvoření brány místní sítě hello

Hello brány místní sítě obvykle odkazuje tooyour místní umístění. Zadejte název, pomocí kterého můžete Azure odkazovat tooit a pak zadejte IP adresu hello hello lokality z hello místní VPN zařízení toowhich se vytvoří připojení. Můžete také určit hello předpony IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello. Hello předpony, které zadáte předpony hello umístěn ve vaší místní síti. Pokud se změní na místní síti, můžete snadno aktualizovat hello předpony.

Použijte hello následující hodnoty:

* Hello *– adresa ip brány* je hello IP adresa vašeho místního zařízení VPN. Zařízení VPN nesmí být umístěné za překladem adres (NAT).
* Hello *– předpony adres místní* jsou místní adresní prostory.

Použití hello [az brány místní-vytvořit](/cli/azure/network/local-gateway#create) příkaz tooadd bránu místní sítě s více předponami adresy:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Vyžádání veřejné IP adresy

Brána VPN musí mít veřejnou IP adresu. Nejprve požádali o prostředek hello IP adresy a pak odkazovat tooit při vytváření brány virtuální sítě. Hello je přiřazená IP adresa dynamicky toohello prostředků při vytváření brány VPN hello. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. Nemůžete si vyžádat statické přiřazení IP adresy. Však neznamená to, že hello IP adresa změní po byl přiřazen tooyour brány VPN. Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.

Použití hello [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) toorequest příkaz dynamické veřejnou IP adresu.

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Vytvoření brány VPN hello

Vytvořte bránu VPN virtuální sítě hello. Vytvoření brány VPN může trvat až minut too45 nebo další toocomplete.

Použijte hello následující hodnoty:

* Hello *– typ brány* pro Site-to-Site je konfigurace *Vpn*. Typ brány Hello je vždy konkrétní toohello konfigurace, kterou implementujete. Další informace najdete v části [Typy bran](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* Hello *typ sítě vpn –* může být *RouteBased* (označované tooas dynamickou bránu v některé dokumentaci), nebo *PolicyBased* (označované tooas statická brána v některých dokumentace). nastavení Hello je konkrétní toorequirements hello zařízení, které se připojujete. Další informace o typech bran VPN najdete v tématu [Informace o nastavení konfigurace služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* Vyberte hello skladová položka brány, které chcete toouse. Pro určité skladové jednotky (SKU) platí omezení konfigurace. Další informace najdete v části [Skladové jednotky (SKU) brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Vytvoření brány VPN hello pomocí hello [az brány virtuální sítě-vytvořit](/cli/azure/network/vnet-gateway#create) příkaz. Pokud spustíte tento příkaz pomocí hello parametr '– žádné - wait', se nezobrazí žádné zpětnou vazbu nebo výstup. Tento parametr umožňuje toocreate hello brány v pozadí hello. Trvá přibližně 45 minut toocreate bránu.

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <a name="VPNDevice"></a>8. Konfigurace zařízení VPN

Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN. V tomto kroku nakonfigurujete zařízení VPN. Při konfiguraci zařízení VPN, je třeba hello následující:

- Sdílený klíč. To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN. V našich ukázkách používáme základní sdílený klíč. Doporučujeme, abyste vygenerování složitější klíče toouse.
- Hello veřejnou IP adresu brány virtuální sítě. Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku. toofind hello veřejnou IP adresu brány virtuální sítě, použijte hello [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz. Pro snadné čtení výstup hello je formátovaný toodisplay hello seznam veřejné IP adresy ve formátu tabulky.

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>9. Vytvoření připojení VPN hello

Vytvořte připojení VPN hello Site-to-Site mezi bránou virtuální sítě a vaše místní zařízení VPN. Platím zvláštní pozornost toohello sdílené hodnotu klíče, který musí odpovídat hello nakonfigurované sdílené hodnota klíče pro vaše zařízení VPN.

Vytvoření připojení hello pomocí hello [az sítě – připojení vpn vytvářet](/cli/azure/network/vpn-connection#create) příkaz.

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Za malou chvíli hello bude vytvořeno připojení.

## <a name="toverify"></a>10. Ověření připojení VPN hello

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

Pokud chcete toouse jiná metoda tooverify připojení, najdete v části [ověření připojení VPN Gateway](vpn-gateway-verify-connection-resource-manager.md).

## <a name="connectVM"></a>tooconnect tooa virtuálního počítače

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="tasks"></a>Běžné úlohy

Tato část obsahuje běžné příkazy, které jsou užitečné při práci s konfiguracemi typu Site-to-Site. Hello úplný seznam síťových rozhraní příkazového řádku najdete v tématu [rozhraní příkazového řádku Azure - sítě](/cli/azure/network).

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a>Další kroky

* Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Informace o vynuceném tunelování najdete v tématu [Informace o vynuceném tunelování](vpn-gateway-forced-tunneling-rm.md).
* Informace o vysoce dostupných připojeních typu aktivní-aktivní najdete v tématu [Připojení s vysokou dostupností mezi jednotlivými místy a VNet-to-VNet](vpn-gateway-highlyavailable.md).
* Seznam příkazů Azure CLI pro práci se sítěmi najdete v tématu věnovaném [Azure CLI](https://docs.microsoft.com/cli/azure/network).