---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Tahle stránka obsahuje informace k vytvoření aplikační brány s přesměrováním zpracování SSL pomocí Azure Resource Manageru"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: ededabc7c665d6bb05b91e4d21d01fb1379add32
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvoříte virtuální síť a podsíť pro službu Application Gateway. Ujistěte se, že tuto podsíť nepoužívají žádné virtuální počítače ani cloudová nasazení. Aplikační brána musí být sama o sobě v podsíti virtuální sítě.
3. Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je potřeba k vytvoření služby Application Gateway?

* **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, tato nastavení rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu. V tuhle chvíli se podporuje jenom *základní* pravidlo. *Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na *Https* (rozlišování velkých a malých písmen). Element **SslCertificate** se přidá do **HttpListener** s hodnotou proměnné nakonfigurovanou pro certifikát SSL. Front-end port se musí aktualizovat na hodnotu 443.

**Když chcete povolit spřažení na základě souborů cookie**: aplikační brána se může nakonfigurovat tak, aby se žádost od klientské relace vždy směrovala na stejný virtuální počítač v prostředí webové serverové farmy. Takový scénář se provádí injektáží souboru cookie relace, který bráně umožňuje řídit provoz odpovídajícím způsobem. Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na *Povoleno* v elementu **BackendHttpSettings**.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Rozdíl mezi použitím modelu nasazení Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte aplikační bránu, a v položkách, které konfigurujete.

S Resource Managerem se všechny komponenty služby Application Gateway konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.

Tady jsou kroky, které se musí udělat k vytvoření aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro aplikační bránu
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvořte prostředek aplikační brány

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že jste přepnuli režim prostředí PowerShell tak, aby se mohly použít rutiny Azure Resource Manageru. Další informace jsou k dispozici v části [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná pro příslušný účet.

```powershell
Get-AzureRmSubscription
```

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.

### <a name="step-3"></a>Krok 3

Zvolte předplatné Azure, které chcete použít.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto nastavení slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem **appgw-RG** a umístěním **Západní USA**.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu Application Gateway

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Managera:

### <a name="step-1"></a>Krok 1

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Tento vzorový kód přiřadí proměnné podsítě rozsah adres 10.0.0.0/24, který se použije k vytvoření virtuální sítě.

### <a name="step-2"></a>Krok 2

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

Tato ukázka vytvoří virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.

### <a name="step-3"></a>Krok 3

```powershell
$subnet = $vnet.Subnets[0]
```

Tímto vzorovým kódem se přiřadí objekt podsítě k proměnné $subnet pro další kroky.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Tato ukázka vytvoří prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvořte objekt konfigurace aplikační brány 

### <a name="step-1"></a>Krok 1

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Tato ukázka vytvoří konfigurace IP aplikační brány s názvem **gatewayIP01**. Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

### <a name="step-2"></a>Krok 2

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

Tato ukázka konfiguruje fond back-end IP adres s názvem **pool01** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50**. Jsou to IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy. Nahraďte IP adresy z předchozího příkladu IP adresami koncových bodů vaší webové aplikace.

### <a name="step-3"></a>Krok 3

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

Tato ukázka nakonfiguruje nastavení brány aplikace **poolsetting01** síťový provoz s vyrovnáváním zatížení ve fondu back-end.

### <a name="step-4"></a>Krok 4

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

Tato ukázka se nakonfiguruje port front-end IP s názvem **frontendport01** pro koncový bod veřejné IP adresy.

### <a name="step-5"></a>Krok 5

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

Tímto vzorovým kódem se nakonfiguruje certifikát používaný pro připojení SSL. Certifikát musí být ve formátu .pfx, heslo musí mít 4 až 12 znaků.

### <a name="step-6"></a>Krok 6

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

Tato ukázka vytvoří konfiguraci front-end IP adresy s názvem **fipconfig01** a přidruží veřejnou IP adresu s konfigurací front-end IP adresy.

### <a name="step-7"></a>Krok 7

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

Tato ukázka vytvoří název naslouchacího procesu **listener01** a přiřadí front-end port ke konfiguraci front-end IP adresy a certifikát.

### <a name="step-8"></a>Krok 8

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Tento příklad vytvoří pravidlo směrování pro vyrovnávání zatížení s názvem **rule01** které konfiguruje chování nástroje pro vyrovnávání zatížení.

### <a name="step-9"></a>Krok 9

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Tímto vzorovým kódem se nakonfiguruje velikost instance aplikační brány.

> [!NOTE]
> Výchozí hodnota pro *InstanceCount* je 2 s maximální hodnotou 10. Výchozí hodnota *GatewaySize* je Medium (Střední). Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).

### <a name="step-10"></a>Krok 10

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

Tento krok definuje zásady protokolu SSL pro použití ve službě application gateway. Navštivte [verze zásad konfigurace protokolu SSL a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) Další informace.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Vytvořte aplikační bránu pomocí New-AzureApplicationGateway

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Tenhle vzorový kód vytvoří službu Application Gateway se všemi položkami konfigurace z předchozích kroků. V příkladu se Aplikační brána nazývá **appgwtest**.

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Budete muset načíst podrobnosti o službě Application Gateway a název její přidružené IP adresy nebo DNS, a to pomocí elementu PublicIPAddress připojeného ke službě Application Gateway. Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS. Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.


```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Další kroky

Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení (ILB), přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

