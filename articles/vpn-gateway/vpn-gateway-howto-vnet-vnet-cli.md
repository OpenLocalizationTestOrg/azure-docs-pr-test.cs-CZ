---
title: "Připojit virtuální síť tooanother virtuální sítě: rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek vás provede propojováním virtuálních sítí s použitím Azure Resource Manageru a Azure CLI."
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
ms.openlocfilehash: 70113914bcae03c80f9ad133ff081d1cf37fc309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a>Konfigurace připojení brány VPN typu VNet-to-VNet pomocí Azure CLI

Tento článek ukazuje, jak toocreate připojení k bráně VPN mezi virtuálními sítěmi. Hello virtuální sítě může být v hello stejné nebo různých oblastí, a z hello stejné nebo různých předplatných. Při připojování virtuální sítě z různých předplatných, odběry hello nemusí toobe přidružené hello stejné klienta služby Active Directory. 

Hello kroky v tomto článku použít toohello modelu nasazení Resource Manager a používat rozhraní příkazového řádku Azure. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

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

![Informace o připojeních](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <a name="why"></a>Proč propojovat virtuální sítě?

Tooconnect virtuální sítě může být vhodné pro hello následujících důvodů:

* **Geografická redundance napříč oblastmi a geografická přítomnost**

  * Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.
  * Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure. Jedním z důležitých příkladů je tooset až SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.
* **Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**

  * Hello uvnitř stejné oblasti, můžete nastavit vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu tooisolation nebo požadavků na správu.

Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku.

### <a name="which-set-of-steps-should-i-use"></a>Kterou posloupnost kroků provést?

V tomto článku uvidíte dvě různé sady kroků. Jednu sadu kroky pro [hello virtuální sítě, které jsou umístěny ve stejné předplatné](#samesub)a druhý pro [patřící do různých předplatných](#difsub).

## <a name="samesub"></a>Propojení virtuálních sítí, které jsou v hello stejného předplatného.

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a>Než začnete

Než začnete, nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější). Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).

### <a name="Plan"></a>Plánování rozsahů IP adres

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


### <a name="Connect"></a>Krok 1 – připojení tooyour odběru

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <a name="TestVNet1"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1

1. Vytvořte skupinu prostředků.

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. Vytvořte virtuální síť TestVNet1 a hello podsítě pro virtuální síť TestVNet1. Tento příklad vytvoří virtuální síť TestVNet1 a podsíť FrontEnd.

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. Vytvořte další adresní prostor pro podsíť hello back-end. Všimněte si, že v tomto kroku jsme zadejte oba hello adresní prostor, který jsme vytvořili předtím a hello další adresní prostor, že má být tooadd. Důvodem je, že hello [aktualizace az sítě vnet](https://docs.microsoft.com/cli/azure/network/vnet#update) příkaz přepíše hello předchozí nastavení. Zajistěte, aby toospecify všechny předpon adres hello při použití tohoto příkazu.

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. Vytvořte podsíť hello back-end.
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. Vytvořte podsíť brány hello. Všimněte si, že tuto podsíť brány hello je s názvem "GatewaySubnet". Název je povinný. V tomto příkladu používá podsíť brány hello 27. I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem minimálně/28 nebo /27. To vám umožní dostatek adresy tooaccommodate možné další konfigurace, které můžete ve hello budoucí.

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. Požádat o veřejné IP adresy toobe přidělené toohello bránu, které vytvoříte pro virtuální síť. Všimněte si, že hello AllocationMethod je dynamický. Nelze zadat, které chcete toouse hello IP adresu. Je dynamicky přidělené tooyour brány.

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. Vytvoření brány virtuální sítě hello pro virtuální síť TestVNet1. Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased. Pokud spustíte tento příkaz pomocí hello parametr '– žádné - wait', se nezobrazí žádné zpětnou vazbu nebo výstup. Hello '– žádné - wait' parametr umožňuje toocreate hello brány v pozadí hello. Neznamená to brány sítě VPN hello dokončí vytváření okamžitě. Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na hello SKU brány, můžete použít.

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="TestVNet4"></a>Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4

1. Vytvořte skupinu prostředků.

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. Vytvořte virtuální síť TestVNet4.

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. Vytvořte další podsítě pro virtuální síť TestVNet4.

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. Vytvořte podsíť brány hello.

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. Vyžádejte si veřejnou IP adresu.

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. Vytvoření brány virtuální sítě TestVNet4 hello.

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="createconnect"></a>Krok 4 – vytvoření připojení hello

Nyní máte dvě virtuální sítě s bránami VPN. dalším krokem Hello je toocreate připojení brány VPN mezi hello brány virtuální sítě. Pokud jste použili výše uvedených příkladech hello, vaší brány virtuální sítě jsou v různých skupinách prostředků. Když jsou brány v různých skupinách prostředků, třeba tooidentify a při navazování připojení zadat ID hello prostředků pro každou bránu. Pokud vaše virtuální sítě jsou v hello stejnou skupinu prostředků, můžete použít hello [druhé sadě pokyny](#samerg) protože nepotřebujete ID toospecify hello prostředků.

### <a name="diffrg"></a>tooconnect virtuální sítě, které jsou umístěny v různých skupinách prostředků

1. Získání hello prostředků ID VNet1GW z hello výstup hello následující příkaz:

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  Ve výstupu hello najde hello "id:" řádku. Hello hodnoty v uvozovkách hello jsou potřebné toocreate hello připojení v další části hello. Zkopírujte tyto hodnoty tooa textový editor, například Poznámkový blok, takže můžete snadno vložit je při vytváření připojení.

  Příklad výstupu:

  ```
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65515, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
   }, 
  "enableBgp": false, 
  "etag": "W/\"ecb42bc5-c176-44e1-802f-b0ce2962ac04\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW", 
  "ipConfigurations":
  ```

  Zkopírujte hodnoty hello po **"id":** v rámci hello uvozovky.

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. Získáte hello prostředků ID VNet4GW a zkopírujte hello hodnoty tooa textový editor.

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1. V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet4. Není sdílený klíč uváděný v příkladech hello. Můžete použít vlastní hodnoty pro sdílený klíč hello. pro obě připojení musí shodovat Hello důležité věc, je tento sdílený klíč hello. Vytvoření připojení trvá malou chvíli toocomplete.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello. Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet4 tooTestVNet1. Zkontrolujte, zda text hello sdílené klíče shodují. Připojení hello tooestablish trvá několik minut.

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. Ověřte stav připojení. Viz [Ověření stavu připojení](#verify).

### <a name="samerg"></a>tooconnect patřící do hello stejné skupiny prostředků

1. Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1. V tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet4. Skupiny prostředků hello oznámení jsou hello stejné v příkladech hello. Zobrazí také sdílený klíč uváděný v příkladech hello. Pro hello sdílený klíč můžete použít vlastní hodnoty, ale musí odpovídat hello sdílený klíč pro obě připojení. Vytvoření připojení trvá malou chvíli toocomplete.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello. Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet4 tooTestVNet1. Zkontrolujte, zda text hello sdílené klíče shodují. Připojení hello tooestablish trvá několik minut.

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. Ověřte stav připojení. Viz [Ověření stavu připojení](#verify).

## <a name="difsub"></a>Propojení virtuálních sítí patřících k různým předplatným

![Diagram v2v](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

V tomto scénáři propojíme sítě TestVNet1 a TestVNet5. Hello virtuální sítě jsou umístěny na různých předplatných. odběry Hello nemusí toobe přidružené hello stejné klienta služby Active Directory. Hello kroky pro tuto konfiguraci přidat další připojení VNet-to-VNet v pořadí tooconnect tooTestVNet5 virtuální sítě TestVNet1.

### <a name="TestVNet1diff"></a>Krok 5: Vytvoření a konfigurace virtuální sítě TestVNet1

Tyto pokyny pokračovat od hello kroky v předchozích částech hello. Je třeba provést [kroku 1](#Connect) a [kroku 2](#TestVNet1) toocreate a konfigurace virtuální sítě TestVNet1 a hello brána sítě VPN pro virtuální síť TestVNet1. Pro tuto konfiguraci nemůžete se vyžaduje toocreate virtuální sítě TestVNet4 z předchozí části hello, ale pokud ho vytvoříte, se nebude v konfliktu s tyto kroky. Po dokončení kroků 1 a 2 pokračujte krokem 6 (níže).

### <a name="verifyranges"></a>Krok 6 – ověření hello rozsahy IP adres

Při vytváření další připojení, je důležité tooverify, který hello adresní prostor IP adres hello nové virtuální sítě se nepřekrývá s žádným z vaší rozsahy virtuálních sítí ani rozsahů bran místních sítí. Pro tento postup můžete použít následující hodnoty pro hello virtuální sítě TestVNet5 hello:

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

### <a name="TestVNet5"></a>Krok 7: Vytvoření a konfigurace virtuální sítě TestVNet5

Tento krok je třeba provést v kontextu hello hello nové předplatné, předplatné 5. Tuto část může provést pomocí Správce hello v jiné organizaci, který vlastní hello předplatné. tooswitch mezi odběrů použijte ' seznamu účtů az--všechny ' toolist hello účet k dispozici tooyour odběry, potom použijte ' nastaven účet az--předplatné <subscriptionID>' tooswitch toohello předplatné, které chcete toouse.

1. Ujistěte se, že jsou připojené tooSubscription 5 a potom vytvořte skupinu prostředků.

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. Vytvořte virtuální síť TestVNet5.

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. Přidejte podsítě.

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. Přidání podsítě brány hello.

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. Vyžádejte si veřejnou IP adresu.

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. Vytvoření brány virtuální sítě TestVNet5 hello

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="connections5"></a>Krok 8 – vytvoření připojení hello

Jsme rozdělit tento krok do dvou relací rozhraní příkazového řádku, označen jako **[předplatné 1]**, a **[předplatné 5]** vzhledem k tomu, že jsou v různých předplatných hello hello brány. tooswitch mezi odběrů použijte ' seznamu účtů az--všechny ' toolist hello účet k dispozici tooyour odběry, potom použijte ' nastaven účet az--předplatné <subscriptionID>' tooswitch toohello předplatné, které chcete toouse.

1. **[Předplatné 1]**  Přihlásit a připojte tooSubscription 1. Hello spusťte následující příkaz tooget hello název a ID hello brány z výstupu hello:

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  Kopírovat výstup hello "id:". Odešlete hello ID a název hello hello virtuální síť brány (VNet1GW) toohello správci předplatného 5 prostřednictvím e-mailu nebo jiným způsobem.

  Příklad výstupu:

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. **[Předplatné 5]**  Přihlásit a připojte tooSubscription 5. Hello spusťte následující příkaz tooget hello název a ID hello brány z výstupu hello:

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  Kopírovat výstup hello "id:". Odešlete hello ID a název hello hello virtuální síť brány (VNet5GW) toohello správci předplatného 1 prostřednictvím e-mailu nebo jiným způsobem.

3. **[Předplatné 1]**  v tomto kroku vytvoříte hello připojení z virtuální sítě TestVNet1 tooTestVNet5. Pro hello sdílený klíč můžete použít vlastní hodnoty, ale musí odpovídat hello sdílený klíč pro obě připojení. Vytvoření připojení může trvat malou chvíli toocomplete. Ujistěte se, že jste připojeni tooSubscription 1.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. **[Předplatné 5]**  Tento krok je podobný toohello jeden vyšší, s výjimkou toho, kterou vytváříte hello připojení z virtuální sítě TestVNet5 tooTestVNet1. Ujistěte se, že tento hello sdíleného klíče shodu a že se můžete připojit tooSubscription 5.

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <a name="verify"></a>Ověřte připojení hello
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Další kroky

* Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu hello [virtuální počítače dokumentaci](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Informace o protokolu BGP najdete v tématu hello [přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [jak tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
