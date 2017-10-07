---
title: "aaaCreate pravidla služby application gateway pomocí směrování adres URL | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfigurace služby Azure application gateway pomocí pravidel směrování adres URL"
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
ms.openlocfilehash: 54fcccc39e48a933576968ce3d8160518c0d0b7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a>Vytvoření služby application gateway pomocí směrování na základě cesty

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování adres URL umožňuje vám trasy tooassociate na základě hello cesty adresy URL požadavku Http. Zkontroluje, jestli je fond back-end tooa trasy nakonfigurované pro adresu URL hello uvedené v hello Application Gateway. Pak odešle hello síťový provoz toohello definovanými fond back-end. Běžně používá pro směrování podle adresy URL je tooload vyrovnávat požadavky pro různé typy obsahu toodifferent back-end serverů fondy.

Na základě adresy URL směrování zavádí novou bránu tooapplication typ pravidla. Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting. Typ základní pravidlo poskytuje kruhového dotazování služby pro hello back-end fondy při PathBasedRouting kromě tooround každý s každým distribuční, také vzorek cesty adresy URL žádosti hello bere v úvahu při výběru hello back-endový fond.

## <a name="scenario"></a>Scénář

V následujícím příkladu hello, Application Gateway obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: fondu video serverů a fond serverů bitové kopie.

Požadavky pro http://contoso.com/image * směrují fondu serverů tooimage (pool1) a http://contoso.com/video * směrují fondu serverů toovideo (pool2). Pokud cesta vzory hello neodpovídají, bude vybrán výchozí fond serverů (pool1).

![Adresa URL trasy](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvoříte virtuální síť a podsíť pro aplikační bránu. Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení. Hello Aplikační brána musí být sám o sobě v podsíti virtuální sítě.
3. přidány servery Hello toohello fond back-end toouse hello Aplikační brána musí existovat nebo musí mít své koncové body vytvořené ve virtuální síti hello nebo s veřejné nebo virtuálními IP Adresami přiřazen.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Co je požadovaná toocreate služby application gateway?

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** hello pravidlo váže naslouchací proces hello, hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Hello rozdíl mezi použitím Azure Classic a Azure Resource Manager je hello pořadí, ve kterém vytvoříte hello aplikační brány a hello položkách, které toobe nakonfigurované.

S Resource Managerem se všechny položky, které služby application gateway se konfigurovat individuálně a potom se spojí dohromady prostředku aplikační brány toocreate hello.

Zde jsou hello kroky, které jsou potřebné toocreate aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro aplikační bránu hello.
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvoření prostředku služby Application Gateway

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se tooAzure

```powershell
Login-AzureRmAccount
```

Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.<BR>

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná hello pro účet hello.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Zvolte, které vaše toouse předplatných Azure. <BR>

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

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Tato skupina prostředků se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.

V předchozím příkladu hello jsme vytvořili skupinu prostředků s názvem "appgw-RG" a umístěním "Západní USA".

> [!NOTE]
> Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu hello application gateway

Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru. Tento příklad vytvoří virtuální síť pro hello Application Gateway. Aplikační brána vyžaduje vlastní podsíti, z tohoto důvodu je menší než hello adresní prostor sítě VNET podsíť hello vytvořená pro hello Application Gateway. To umožňuje, aby jiné prostředky, včetně, ale toobe mimo jiné tooweb servery konfigurované v hello stejné virtuální síti.

### <a name="step-1"></a>Krok 1

Přiřaďte hello adresa rozsahu 10.0.0.0/24 toohello podsíť proměnné toobe používá toocreate virtuální sítě.  Tím se vytvoří objekt konfigurace hello podsíť pro aplikační brány, který se používá v dalším příkladu hello hello.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Krok 2

Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24. Tím dokončíte konfiguraci hello hello virtuální síť s jednu podsíť pro aplikační bránu tooreside hello.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Krok 3

Přiřaďte proměnnou podsítě hello pro hello další kroky, to je předán toohello `New-AzureRMApplicationGateway` rutiny v příštím kroku.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci hello

Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello. Aplikační bránu můžete použít veřejnou IP adresu, interní IP adresu nebo oba tooreceive požadavky pro vyrovnávání zatížení.  V tomto příkladu se používá jen veřejná IP adresa. V následujícím příkladu hello nakonfigurovaný žádný název DNS pro vytvoření hello veřejnou IP adresu.  Služba Application Gateway nepodporuje u veřejných IP adres vlastní názvy DNS.  Pokud je požadován pro veřejný koncový bod hello vlastní název, by se vytvořit název DNS toopoint toohello automaticky generovány pro veřejnou IP adresu hello záznam CNAME.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.

## <a name="create-application-gateway-configuration"></a>Vytvoření konfigurace brány aplikace

Všechny položky konfigurace, musí ho nastavit před vytvořením hello aplikační brány. Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Krok 2

Konfigurace služby fond hello back-end IP adresy s názvem **pool01** a **pool2** s IP adresami pro **pool1** a **pool2**. Tyto IP adresy jsou IP adresy hello hello prostředků, které jsou hostiteli hello webové aplikace toobe chráněn hello aplikační brány. Tito členové fondu back-end jsou všechny ověřené toobe podle sondy v pořádku, ať už sondy základní nebo vlastní testy paměti.  Provoz se směruje pak toothem při žádosti o začalo hello aplikační brány. Back-endové fondy mohou být využívána více pravidel v rámci hello aplikační bránu, což znamená, jeden fond back-end by mohly být použity několika webových aplikací, které jsou umístěné na hello stejné hostitele.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

V tomto příkladu jsou dva back endové fondy tooroute síťový provoz na základě cesty adresy URL hello. Jeden fond přijímá provoz z cesty URL „/video“ a druhý fond přijímá provoz z cesty „/images“. Nahraďte hello předcházející IP adresy tooadd koncovými body IP adresy vlastní aplikace. 

### <a name="step-3"></a>Krok 3

Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** hello Vyrovnávání zatížení síťových přenosů ve fondu back-end hello. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro hello back endové fondy. Každý fond back-end může mít vlastní nastavení fondu back-end.  Nastavení HTTP back-end jsou používány pravidla tooroute provoz toohello správné back-end členy fondu. Určuje hello protokol a port, který se používá při odesílání provozu toohello členy fondu back-end. Na základě souborů cookie relací jsou určeny také nastavení HTTP back-end hello.  Pokud je povoleno, spřažení relace na základě souborů cookie odešle provoz toohello stejnou back-end jako předchozí požadavky jednotlivých paketů.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Nakonfigurujte veřejný koncový bod IP hello front-end IP adresu. objekt konfigurace IP front-endu Hello je používán hello toorelate naslouchacího procesu vzdálené čelí IP adresu naslouchacího procesu hello.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte port front-end hello aplikační brány. objekt konfigurace Hello front-end port je používán toodefine naslouchací proces port hello Application Gateway naslouchá provoz na hello naslouchací proces.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Krok 6

Nakonfigurujte hello naslouchací proces. Tento krok nakonfiguruje hello naslouchací proces pro hello veřejnou IP adresu a port používá tooreceive příchozí síťový provoz. Následující ukázka Hello trvá hello dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje hello naslouchací proces. V tomto příkladu naslouchá naslouchací proces hello tooHTTP přenosy na portu 80 na hello veřejnou IP adresu, která jste vytvořili.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Krok 7

Konfigurovat pravidla cesty adresy URL pro hello back endové fondy. Tento krok nakonfiguruje hello relativní cesta používaná systémem aplikační brány a definuje hello mapování mezi hello cestu adresy URL a hello fond back-end, který je přiřazen toohandle hello příchozí provoz.

> [!IMPORTANT]
> Každá cesta musí začínat znakem / a jediným místem hello "\*" je povolena, je na konci hello. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *. Hello řetězec dodáni toohello cesta objekt přiřazení vzorce nezahrnuje jakýkoli text po hello nejprve "?" nebo "#" a tyto znaky nejsou povoleny. 

Hello následující příklad vytvoří dvě pravidla: jeden pro "/ image /" cesty směrování provozu tooback-end "pool1" a další pro "/ video či" cestu směrování provozu tooback-end "pool2." Tato pravidla zajistěte, aby byl provoz pro každou sadu adres URL směrované toohello back-end. Například http://contoso.com/image/figure1.jpg přejde toopool1 a http://contoso.com/video/example.mp4 přejde toopool2.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Pokud cesta hello neodpovídá žádné z pravidel hello předem definovaná cesta, hello pravidla cesty mapy konfigurace nakonfiguruje taky výchozího fondu adres back-end. Například http://contoso.com/shoppingcart/test.html přejde toopool1, jako je definována jako hello výchozí fond pro neodpovídající provoz.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Krok 8

Vytvořte nastavení pravidla. Tento krok nakonfiguruje hello aplikace brány toouse směrování adres URL na základě cesty. Hello `$urlPathMap` proměnná definovaná v hello z předchozích kroků je nyní použité toocreate hello na základě cesty pravidlo. V tomto kroku jsme přidružit hello pravidlo naslouchací proces a mapování cesty adresy url hello vytvořili dříve.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Krok 9

Nakonfigurujte hello počet instancí a velikost hello aplikační brány.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků hello.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure hello front-endu záznam IP CNAME načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány. název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME. Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.

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

Pokud chcete toolearn přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-arm.md).

