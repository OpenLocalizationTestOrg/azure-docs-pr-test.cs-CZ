---
title: "aaaUsing Azure Application Gateway s interní nástroj pro vyrovnávání zatížení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfiguraci, spuštění a odstranění služby Azure application gateway s vyrovnávání interní zatížení (ILB) pro Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: dd0d7e954b1fa219ae6ebe42cb4b479dbcf08653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Vytvořte aplikační bránu s interním nástrojem pro vyrovnávání zatížení (ILB) pomocí nástroje Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Classic PowerShell](application-gateway-ilb.md)
> * [Azure Resource Manager PowerShell](application-gateway-ilb-arm.md)

Služba Azure Application Gateway lze nastavit straně Internetu VIP nebo s vnitřní koncový bod, který není zveřejněné toohello Internetu, také známé jako koncový bod vyrovnávání (ILB) interní zatížení. Konfigurace hello brány s ILB je užitečná pro interní-obchodní aplikace, které nejsou zveřejněné toohello Internetu. Je také užitečné pro služby a vrstvy v rámci vícevrstvé aplikace, které se nacházejí v rámci hranice zabezpečení, který není zveřejněné toohello Internet, ale stále vyžadují kruhového dotazování Načíst distribuční, dlouhodobost relace nebo ukončení Secure Sockets Layer (SSL).

Tento článek vás provede kroky tooconfigure hello aplikační brány s ILB.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvořte virtuální síť a podsíť služby Application Gateway. Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení. Aplikační brána musí být sama o sobě v podsíti virtuální sítě.
3. Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Co je požadovaná toocreate služby application gateway?

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. Hello uvedené IP adresy by měly buď patřit toohello virtuální sítě, ale v jiné podsíti pro aplikační bránu hello nebo by měly být veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, ty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu. V současné době pouze hello *základní* pravidel je podporována. Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Hello rozdíl mezi použitím Azure Classic a Azure Resource Manager je hello pořadí, ve kterém vytvoříte hello aplikační brány a hello položkách, které toobe nakonfigurované.
S Resource Managerem se všechny položky, které služby application gateway je konfigurovat individuálně a potom se spojí dohromady prostředku aplikační brány toocreate hello.

Zde jsou hello kroky, které jsou potřebné toocreate aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvoření virtuální sítě a podsítě pro službu hello application gateway
3. Vytvořte objekt konfigurace aplikační brány 
4. Vytvořte prostředek aplikační brány

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že jste přepnuli rutiny Azure Resource Manager prostředí PowerShell režimu toouse hello. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná hello pro účet hello.

```powershell
Get-AzureRmSubscription
```

Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.

### <a name="step-3"></a>Krok 3

Zvolte, které vaše toouse předplatných Azure.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Vytvořte novou skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy toocreate používá služby application gateway hello stejné skupiny prostředků.

V předchozím příkladu hello jsme vytvořili skupinu prostředků s názvem "Appgw-rg" a umístěním "západní USA".

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu hello application gateway

Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru:

### <a name="step-1"></a>Krok 1

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Tento krok přiřadí hello adresa rozsahu 10.0.0.0/24 tooa podsíť proměnné toobe používá toocreate virtuální sítě.

### <a name="step-2"></a>Krok 2

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

Tento krok vytvoří virtuální síť s názvem "appgwvnet" v prostředku skupiny "appgw-rg" pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.

### <a name="step-3"></a>Krok 3

```powershell
$subnet = $vnet.subnets[0]
```

Tento krok přiřadí hello podsítě objektu toovariable $subnet pro další kroky hello.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvořte objekt konfigurace aplikační brány 

### <a name="step-1"></a>Krok 1

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Tento krok vytvoří konfigurace IP aplikační brány s názvem "gatewayIP01". Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

### <a name="step-2"></a>Krok 2

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

Tento krok konfiguruje hello back-end IP adres fond s názvem "pool01" s IP adresami "10.1.1.8, 10.1.1.9, 10.1.1.10". Ty jsou hello IP adresy, které přijímají hello síťový provoz, který přichází z hello koncového bodu front-end IP. Nahraďte hello předcházející IP adresy tooadd koncovými body IP adresy vlastní aplikace.

### <a name="step-3"></a>Krok 3

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Tento krok nakonfiguruje aplikaci brány nastavení "poolsetting01" pro zatížení hello vyrovnáváním síťového provozu ve fondu back-end hello.

### <a name="step-4"></a>Krok 4

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

Tento krok nakonfiguruje hello port front-end IP adresy s názvem "frontendport01" pro hello ILB.

### <a name="step-5"></a>Krok 5

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

Tento krok vytvoří hello konfiguraci front-end IP adresy, nazvanou "fipconfig01" a přidruží ji s privátní IP Adresou z aktuální podsítě virtuální sítě hello.

### <a name="step-6"></a>Krok 6

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

Tento krok vytvoří hello naslouchací proces nazvaný "listener01" a přiřadí hello front-end port toohello konfiguraci front-end IP adresy.

### <a name="step-7"></a>Krok 7

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Tento krok vytvoří hello pravidlo Vyrovnávání zatížení směrování názvem "rule01", které konfiguruje chování nástroje pro vyrovnávání zatížení hello.

### <a name="step-8"></a>Krok 8

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Tento krok nakonfiguruje velikost instance hello hello aplikační brány.

> [!NOTE]
> Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota pro Hello *GatewaySize* je střední. Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Vytvořte aplikační bránu pomocí New-AzureApplicationGateway

Vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků hello. V tomto příkladu hello Aplikační brána nazývá "appgwtest".

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Tento krok vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků hello. V příkladu hello hello Aplikační brána nazývá "appgwtest".

## <a name="delete-an-application-gateway"></a>Odstranění služby Application Gateway

toodelete služby application gateway, je třeba toodo hello následující kroky v pořadí:

1. Použití hello `Stop-AzureRmApplicationGateway` rutiny toostop hello brány.
2. Použití hello `Remove-AzureRmApplicationGateway` rutiny tooremove hello brány.
3. Ověřte, že hello brány byla odebrána pomocí hello `Get-AzureApplicationGateway` rutiny.

### <a name="step-1"></a>Krok 1

Získejte hello objektu application gateway a přidružte ji tooa proměnné "$getgw".

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Krok 2

Použití `Stop-AzureRmApplicationGateway` toostop hello aplikační brány. Tento příklad ukazuje hello `Stop-AzureRmApplicationGateway` na hello prvním řádku, následovanou výstup hello.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Jakmile hello Aplikační brána v zastaveném stavu, použijte hello `Remove-AzureRmApplicationGateway` rutiny tooremove hello služby.

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> Hello **-force** přepínač může být použité toosuppress hello odebrat potvrzovací zpráva.

Odebrali jsme tooverify, který hello služby, můžete použít hello `Get-AzureRmApplicationGateway` rutiny. Tenhle krok není povinný.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
```

## <a name="next-steps"></a>Další kroky

Pokud chcete tooconfigure přesměrování zpracování SSL, najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete, aby tooconfigure toouse aplikace brány s ILB, přečtěte si téma [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

