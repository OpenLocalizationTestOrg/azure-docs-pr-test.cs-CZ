---
title: "Vytvoření služby application gateway pomocí pravidel směrování adres URL | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření, konfiguraci služby Azure application gateway pomocí pravidel směrování adres URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: ba756d3262b9780c5701e69faad860ba32bba08b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a>Vytvoření služby application gateway pomocí směrování na základě cesty

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování adres URL umožňuje přidružit tras na základě cesty adresy URL požadavku Http. Zkontroluje, pokud je trasu k back-end fondu pro adresu URL uvedené v bráně aplikace nakonfigurovaná. Pak odešle síťový provoz na definované fond back-end. Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.

Na základě adresy URL směrování zavádí nový typ pravidla aplikační brány. Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting. Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy při PathBasedRouting kromě distribučních kruhové dotazování, také vzorek cesty adresy URL žádosti bere v úvahu při výběru fondu back-end.

## <a name="scenario"></a>Scénář

V následujícím příkladu se Aplikační brána obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: fondu video serverů a fond serverů bitové kopie.

Požadavky pro http://contoso.com/image * jsou směrovány do fondu serverů bitové kopie (pool1) a http://contoso.com/video * jsou směrovány do fondu serverů videa (pool2). Pokud cesta vzory neodpovídají, je vybrán výchozí fond serverů (pool1).

![Adresa URL trasy](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvoříte virtuální síť a podsíť pro aplikační bránu. Ujistěte se, že žádné virtuální počítače nebo cloudová nasazení nepoužívají podsíť. Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.
3. Servery přidané do fondu back-end pro použití služby application gateway, musí existovat nebo mít své koncové body vytvořené ve virtuální síti nebo s veřejné nebo virtuálními IP Adresami přiřazen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je potřeba k vytvoření služby Application Gateway?

* **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Rozdíl mezi použitím nástrojů Azure Classic a Azure Resource Manager je v tom, v jakém pořadí tvoříte službu Application Gateway, a v položkách, které konfigurujete.

S Resource Managerem se všechny položky, které tvoří službu Application Gateway, konfigurují individuálně, potom se spojí dohromady a vytvoří prostředek služby Application Gateway.

Toto jsou kroky, které se musí provést k vytvoření služby Application Gateway:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro službu Application Gateway
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvoření prostředku služby Application Gateway

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se k Azure.

```powershell
Login-AzureRmAccount
```

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.<BR>

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná pro příslušný účet.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Zvolte předplatné Azure, které chcete použít. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Tato skupina prostředků se používá jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem „appgw-RG“ a umístěním „Západní USA“.

> [!NOTE]
> Pokud pro svoji službu Application Gateway potřebujete nakonfigurovat vlastní test paměti, přečtěte si článek [Vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v článku [Vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu Application Gateway

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Resource Manageru. Tento příklad vytvoří virtuální síť pro službu Application Gateway. Aplikační brána vyžaduje vlastní podsíti, z tohoto důvodu je menší než adresní prostor sítě VNET podsíť vytvořená pro službu Application Gateway. To umožňuje jiné prostředky, včetně mimo jiné webové servery do nakonfigurovat ve stejné virtuální síti.

### <a name="step-1"></a>Krok 1

Proměnné podsítě, která se má použít k vytvoření virtuální podsítě, přiřaďte rozsah adres 10.0.0.0/24.  Tím se vytvoří objekt konfigurace podsítě pro službu Application Gateway, který se používá v dalším příkladu.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2

Vytvořte virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast Západní USA s použitím předpony 10.0.0.0/16 s podsítí 10.0.0.0/24. Tím dokončíte konfiguraci virtuální sítě s jednu podsíť pro aplikační brány, aby se nacházejí.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3

Přiřaďte proměnnou podsítě pro další kroky, to je předán `New-AzureRMApplicationGateway` rutiny v příštím kroku.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci

Vytvořte prostředek veřejné IP adresy **publicIP01** ve skupině prostředků **appgw-rg** pro oblast Západní USA. Služba Application Gateway může pro příjem požadavků na vyrovnávání zatížení používat veřejnou IP adresu, interní IP adresu nebo obojí.  V tomto příkladu se používá jen veřejná IP adresa. V následujícím příkladu není pro vytvoření veřejné IP adresy nakonfigurován žádný název DNS.  Služba Application Gateway nepodporuje u veřejných IP adres vlastní názvy DNS.  Pokud je pro veřejný koncový bod vyžadován vlastní název, měl by být vytvořen záznam CNAME odkazující na automaticky vygenerovaný název DNS pro příslušnou veřejnou IP adresu.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP adresa je ke službě Application Gateway přiřazena při spuštění služby.

## <a name="create-application-gateway-configuration"></a>Vytvoření konfigurace brány aplikace

Před vytvořením služby Application Gateway musí být nastaveny všechny položky konfigurace. Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2

Nakonfigurujte fond back-end IP adres s názvem **pool01** a **pool2** s IP adresami pro **pool1** a **pool2**. Tyto IP adresy jsou IP adresy prostředků, které jsou hostiteli webové aplikace, která má být chráněna službou Application Gateway. Funkčnost všech těchto členů fondu back-end se ověřuje základními nebo vlastními testy.  Provoz je pak směrován do nich, když služba Application Gateway obdrží požadavky. Fondy back-end můžou být využívány více pravidly v rámci služby Application Gateway. To znamená, že jeden fond back-end může být využíván pro více webových aplikacích umístěných ve stejném hostiteli.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

V tomto příkladu jsou pro směrování síťového provozu na základě cesty URL použity dva fondy back-end. Jeden fond přijímá provoz z cesty URL „/video“ a druhý fond přijímá provoz z cesty „/images“. Nahrazením předchozích IP adres přidejte vlastní koncové body IP adres aplikace. 

### <a name="step-3"></a>Krok 3

Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro back endové fondy. Každý fond back-end může mít vlastní nastavení fondu back-end.  Nastavení back-endu HTTP jsou využívána pravidly pro směrování provozu do správných členů fondu back-end. Určuje protokol a port, který se používá při odesílání provozu do členy fondu back-end. Podle nastavení HTTP back-endu se určují i relace založené na souborech cookie.  Pokud je tato funkce povolena, spřažení relace založené na souborech cookie odesílá provoz do stejného back-endu jako předchozí požadavky pro jednotlivé pakety.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Nakonfigurujte IP adresu front-endu s koncovým bodem s veřejnou IP adresou. Objekt konfigurace IP adresy front-endu je používán naslouchacím procesem k předání veřejně viditelné IP adresy pro naslouchací proces.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte front-end port pro službu Application Gateway. Objekt konfigurace portu front-end je používán naslouchacím procesem k definování portu, na kterém služba Application Gateway v naslouchacím procesu naslouchá provozu.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Krok 6

Nakonfigurujte naslouchací proces. V tomto kroku je nakonfigurován naslouchací proces pro veřejnou IP adresu a port používaný pro příjem příchozího síťového provozu. Následující příklad trvá dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje naslouchací proces. V tomto příkladu naslouchací proces naslouchá provozu HTTP na portu 80 pro veřejnou IP adresu, kterou jste vytvořili dříve.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Krok 7

Konfigurovat pravidla cesty adresy URL pro back endové fondy. Tento krok nakonfiguruje relativní cesta používaná systémem aplikační brány a definuje mapování mezi cestu adresy URL a fond back-end, která je přiřazena k zpracovávat příchozí provoz.

> [!IMPORTANT]
> Každá cesta musí začínat znakem / a bude jediným místem "\*" je povolena, je na konci. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *. Řetězec dodáni do objekt přiřazení vzorce cesta nezahrnuje jakýkoli text po první "?" nebo "#" a tyto znaky nejsou povoleny. 

Následující příklad vytvoří dvě pravidla: jeden pro "/ image /" cesty směrování provozu na back-end "pool1" a další pro "/ video /" cesty směrování provozu na back-end "pool2." Tato pravidla se ujistěte, že přenosy dat pro každou sadu adresy URL se směruje na back-end. Například http://contoso.com/image/figure1.jpg přejde na pool1 a http://contoso.com/video/example.mp4 přejde na pool2.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Pokud cesta neodpovídá žádné z pravidel předem definovaná cesta, nakonfiguruje konfiguraci pravidla cesty mapy taky výchozího fondu adres back-end. Například http://contoso.com/shoppingcart/test.html přejde na pool1, jako je definován jako výchozí fond pro neodpovídající provoz.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Krok 8

Vytvořte nastavení pravidla. Tento krok konfiguruje službu application gateway používat na základě cestu směrování adres URL. `$urlPathMap` Proměnná definovaná v předchozím kroku se teď používá k vytvoření pravidla na základě cesty. V tomto kroku jsme pravidlo přidružit naslouchací proces a mapování cesty adresy url vytvořili dříve.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Krok 9

Nakonfigurujte počet instancí a velikost pro službu Application Gateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Pokud chcete nakonfigurovat záznam IP CNAME front-endu, načíst podrobnosti o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress připojit k službě application gateway. Název DNS služby application gateway by měl použít k vytvoření záznamu CNAME. Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.

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

Pokud chcete získat další přesměrování zpracování Secure Sockets Layer (SSL), přečtěte si téma [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-arm.md).

