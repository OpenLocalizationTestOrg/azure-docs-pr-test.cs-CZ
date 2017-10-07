---
title: "aaaConfigure SSL snižování zátěže prostředí PowerShell – Azure Application Gateway - | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate přesměrování zpracování úloh služby application gateway pomocí protokolu SSL pomocí Azure Resource Manager"
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
ms.openlocfilehash: c2855d8d3caaa97ec05475c67ff0f8dce72ef2a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí Azure Resource Manageru

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway může být relace Secure Sockets Layer (SSL) nakonfigurované tooterminate hello v hello brány tooavoid nákladná SSL dešifrování úlohy toohappen v hello webové farmy. Přesměrování zpracování SSL zjednodušuje i nastavení serveru front-end hello a Správa webové aplikace hello.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvoříte virtuální síť a podsíť pro aplikační bránu hello. Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení. Aplikační brána musí být sama o sobě v podsíti virtuální sítě.
3. Hello servery nakonfigurujete toouse hello Aplikační brána musí existovat nebo mít své koncové body vytvořené ve virtuální síti hello nebo s veřejné nebo virtuálními IP Adresami přiřazen.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Co je požadovaná toocreate služby application gateway?

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tato nastavení jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu. V současné době pouze hello *základní* pravidel je podporována. Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL, hello protokol v **HttpListener** by se měl změnit příliš*Https* (malá a velká písmena). Hello **SslCertificate** prvek přidán příliš**HttpListener** s hello hodnotou proměnné nakonfigurovanou pro certifikát SSL hello. Hello front-end port musí být aktualizované too443.

**spřažení na základě souborů cookie tooenable**: služby application gateway může být nakonfigurované tooensure, zda je žádost o od klientské relace vždy směrovanou toohello stejný virtuální počítač v hello webové farmy. Tento scénář se provádí injektáží souboru cookie relace, které umožňuje přenos toodirect hello brány správně. Nastavení spřažení na základě souborů cookie tooenable **CookieBasedAffinity** příliš*povoleno* v hello **BackendHttpSettings** element.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Hello rozdíl mezi použitím modelu nasazení Azure Classic hello a Azure Resource Manager je hello pořadí, ve kterém vytvoříte brány a hello položky aplikace vyžadující toobe nakonfigurované.

S Resource Managerem se všechny součásti služby application gateway se konfigurovat individuálně a potom se spojí dohromady toocreate prostředek aplikační brány.

Zde jsou kroky potřebné toocreate hello aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro bránu aplikace hello
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

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto nastavení se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy toocreate používá služby application gateway hello stejné skupiny prostředků.

V předchozím příkladu hello, jsme vytvořili skupinu prostředků s názvem **appgw-RG** a umístění **západní USA**.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu hello application gateway

Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru:

### <a name="step-1"></a>Krok 1

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Tato ukázka přiřadí hello adresa rozsahu 10.0.0.0/24 tooa podsíť proměnné toobe používá toocreate virtuální sítě.

### <a name="step-2"></a>Krok 2

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

Tato ukázka vytvoří virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24.

### <a name="step-3"></a>Krok 3

```powershell
$subnet = $vnet.Subnets[0]
```

Tato ukázka přiřadí hello podsítě objektu toovariable $subnet pro další kroky hello.

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci hello

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Tato ukázka vytvoří prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvořte objekt konfigurace aplikační brány 

### <a name="step-1"></a>Krok 1

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Tato ukázka vytvoří konfigurace IP aplikační brány s názvem **gatewayIP01**. Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

### <a name="step-2"></a>Krok 2

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

Tato ukázka konfiguruje hello back-end IP adres fond s názvem **pool01** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50** . Tyto hodnoty jsou hello IP adresy, které přijímají hello síťový provoz, který přichází z hello koncového bodu front-end IP adresy. Nahraďte hello IP adresy z hello předcházející příklad s hello IP adresy koncových bodů vaší webové aplikace.

### <a name="step-3"></a>Krok 3

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

Tato ukázka nakonfiguruje nastavení brány aplikace **poolsetting01** tooload vyrovnáváním síťového provozu ve fondu back-end hello.

### <a name="step-4"></a>Krok 4

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

Tato ukázka nakonfiguruje hello port front-end IP s názvem **frontendport01** pro hello koncový bod veřejné IP adresy.

### <a name="step-5"></a>Krok 5

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

Tato ukázka nakonfiguruje hello certifikátu používaného pro připojení SSL. certifikát Hello musí toobe ve formátu .pfx a hello heslo musí být mezi 4 znaky too12.

### <a name="step-6"></a>Krok 6

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

Tato ukázka vytvoří hello konfiguraci front-end IP adresy s názvem **fipconfig01** a partnerů hello veřejnou IP adresu s konfigurací front-end IP adresy hello.

### <a name="step-7"></a>Krok 7

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

Tato ukázka vytvoří název naslouchacího procesu hello **listener01** a partnerů hello front-end port toohello front-endové konfigurace protokolu IP a certifikát.

### <a name="step-8"></a>Krok 8

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Tato ukázka vytvoří hello směrování se pravidlo Vyrovnávání zatížení s názvem **rule01** které konfiguruje chování nástroje pro vyrovnávání zatížení hello.

### <a name="step-9"></a>Krok 9

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Tato ukázka se nakonfiguruje velikost instance hello hello aplikační brány.

> [!NOTE]
> Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota pro Hello *GatewaySize* je střední. Můžete si vybrat mezi hodnotami Standard_Small (Standardní_malá), Standard_Medium (Standardní_střední) a Standard_Large (Standardní_velká).

### <a name="step-10"></a>Krok 10

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

Tento krok definuje hello SSL zásad toouse ve hello application gateway. Navštivte [verze zásad konfigurace protokolu SSL a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn Další.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Vytvořte aplikační bránu pomocí New-AzureApplicationGateway

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Tato ukázka se vytvoří aplikační brána se všemi položkami konfigurace z předchozích kroků hello. V příkladu hello hello Aplikační brána nazývá **appgwtest**.

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). toodo se načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány. název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS. Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.


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

Pokud chcete tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání interní zatížení (ILB), najdete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

