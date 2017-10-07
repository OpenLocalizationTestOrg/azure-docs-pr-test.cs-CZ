---
title: "aaaHow toouse Azure API Management ve virtuální síti s aplikační brány | Microsoft Docs"
description: "Zjistěte, jak toosetup a nakonfigurovat Azure API Management v interní virtuální síť s aplikací brány (firewall webových aplikací) jako front-endu"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
ms.openlocfilehash: 74303a2ee8a10db633ab1740ec7267728eacb473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Integrovat správu rozhraní API v interní virtuální síť s aplikační brány 

##<a name="overview"></a> – Přehled
 
Hello služba API Management můžete nakonfigurovat ve virtuální síti v interní režimu, takže je dostupné pouze v aplikaci hello virtuální sítě. Služba Azure Application Gateway je služba PAAS, které poskytuje nástroj pro vyrovnávání zatížení vrstvy 7. To funguje jako služba reverznímu proxy serveru a poskytuje mezi jeho nabídky webové aplikace brány Firewall (firewall webových aplikací).

Kombinování API Management zřízené v interní virtuální síti VNET s front-endu Application Gateway hello umožňuje hello následující scénáře:

* Použití hello stejné rozhraní API správy prostředků pro spotřeba příjemci interní i externí uživatelé.
* Použít jeden zdroj API Management a mít podmnožinu rozhraní API definované ve službě API Management, které jsou k dispozici pro externí příjemci.
* Zadejte způsob klíč tooswitch přístup tooAPI správy z hello veřejného Internetu zapnout a vypnout. 

##<a name="scenario"></a> Scénář
Tento článek se týkají jak toouse jeden rozhraní API správy služby pro interní i externí příjemce a nastavte jej fungovat jako jeden front-end pro obě místní a cloudové rozhraní API. Zobrazí se také jak tooexpose pouze podmnožina vašich rozhraní API (v příkladu hello jsou vyznačené na zelená) pro externí spotřebu pomocí funkce PathBasedRouting hello je k dispozici v aplikační brány.

V prvním příkladu instalace hello všechna rozhraní API spravují pouze v rámci virtuální sítě. Interní příjemci (zvýraznit v oranžová) mají přístup všechny vaše interní a externí rozhraní API. Provoz se nikdy nedostane mimo tooInternet, vysoký výkon je doručovány prostřednictvím okruhy Expressroute.

![Adresa URL trasy](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"></a> Před zahájením

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. Vytvoření virtuální sítě a vytvořte oddělené podsítě pro API Management a aplikační brány. 
3. Pokud máte v úmyslu toocreate vlastního serveru DNS pro hello virtuální sítě, můžete tak učiňte před zahájením nasazení hello. Překontrolujte, který funguje zajištěním virtuální počítač vytvořený v novou podsíť ve hello virtuální sítě můžete vyřešit a přístup všechny koncové body služby Azure.

## <a name="what-is-required-toocreate-an-integration-between-api-management-and-application-gateway"></a>Co je požadovaná toocreate integraci mezi API Management a Application Gateway?

* **Fond back-end serverů:** hello interní virtuální IP adresa hello služba API Management.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** jde hello veřejný port, který se otevírá ve hello aplikační brány. Provoz stiskne ho získá přesměrovaného tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** hello pravidlo váže naslouchací proces fondu tooa back-end serverů.
* **Vlastní test stavu:** aplikační bránu, ve výchozím nastavení, použije IP adresy na základě sondy toofigure, na které servery v hello BackendAddressPool jsou aktivní. Hello služba API Management pouze odpoví toorequests mít hlavička hello správné hostitele, proto hello výchozí sondy nezdaří. Test vlastní stavu musí toobe definované toohelp Aplikační brána určit, že služba hello je aktivní a předávat požadavky.
* **Certifikát vlastní domény:** tooaccess API Management z hello internet, je nutné toocreate mapování CNAME hostname toohello Application Gateway front-end DNS názvu. To zajišťuje, že hlavičku hello název hostitele a certifikát odeslat tooApplication bránu, která se předají tooAPI správu jednoho APIM rozpoznat jako platný.

## <a name="overview-steps"></a> Kroky potřebné k integraci API Management a aplikační brány 

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro hello Application Gateway. Vytvořte další podsítě pro API Management.
3. Vytvoření služby API Management uvnitř podsíť virtuální sítě hello vytvořili výše a ujistěte se, že používáte interní režim hello.
4. Instalační program hello vlastní název domény v hello služba API Management.
5. Vytvoření objektu konfigurace služby Application Gateway.
6. Vytvořte prostředek aplikační brány.
7. Vytvořte záznam CNAME z hello veřejného názvu DNS hello Application Gateway toohello API Management proxy hostitele.

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se tooAzure

```powershell
Login-AzureRmAccount
```

Ověření pomocí svých přihlašovacích údajů.<BR>

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná hello pro účet hello a vyberte jej.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu hello application gateway

Hello následující příklad ukazuje, jak hello toocreate virtuální sítě pomocí Správce prostředků.

### <a name="step-1"></a>Krok 1

Přiřaďte hello adresa rozsahu 10.0.0.0/24 toohello podsíť proměnné toobe použit pro službu Application Gateway při vytváření virtuální sítě.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Krok 2

Přiřaďte hello adresa rozsahu 10.0.1.0/24 toohello podsíť proměnné toobe používá pro správu rozhraní API při vytváření virtuální sítě.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Krok 3

Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **apim-appGw-RG** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Krok 4

Přiřaďte proměnnou podsítě pro další kroky hello

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Vytvoření služby API Management uvnitř nakonfigurován v režimu interní virtuální sítě

Hello následující příklad ukazuje, jak toocreate služby API Management ve virtuální síti nakonfigurované interní pouze pro přístup.

### <a name="step-1"></a>Krok 1
Vytvořte virtuální síť pro správu rozhraní API objekt, který používá podsíť hello $apimsubnetdata vytvořili výše.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Krok 2
Vytvoření služby API Management uvnitř hello virtuální sítě.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Po úspěšném provedení hello výše příkaz odkazovat příliš[tooaccess interní virtuální síť rozhraní API správy služby nutná konfigurace DNS](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess ho.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Nastavit vlastní název domény ve službě API Management

### <a name="step-1"></a>Krok 1
Nahrajte hello certifikát s privátním klíčem pro doménu hello. V tomto příkladu bude `*.contoso.net`. 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path too.pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>Krok 2
Po nahrání certifikátu hello vytvořit objekt konfigurace název hostitele pro proxy server hello s název hostitele `api.contoso.net`, jak hello příklad certifikát poskytuje autority pro hello `*.contoso.net` domény. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci hello

Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **apim-appGw-RG** pro oblast západní USA hello.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.

## <a name="create-application-gateway-configuration"></a>Vytvoření konfigurace brány aplikace

Všechny položky konfigurace, musí ho nastavit před vytvořením hello aplikační brány. Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Krok 2

Nakonfigurujte port front-end IP hello pro hello koncový bod veřejné IP adresy. Toto je hello port, který koncoví uživatelé připojit k.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>Krok 3

Nakonfigurujte veřejný koncový bod IP hello front-end IP adresu.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Krok 4

Nakonfigurujte hello certifikát pro hello Application Gateway používají toodecrypt a znovu zašifrovat přenosy hello procházející.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Krok 5

Vytvořte naslouchací proces protokolu HTTP hello pro hello Application Gateway. Přiřaďte hello front-endové konfigurace protokolu IP, portu a tooit certifikát ssl.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Krok 6

Vytvořit vlastní test paměti toohello služba API Management `ContosoApi` koncový bod domény proxy serveru. Cesta Hello `/status-0123456789abcdef` je výchozího koncového bodu stavu hostované na všechny služby API Management hello. Nastavit `api.contoso.net` jako toosecure název hostitele vlastní test paměti ho s certifikátem SSL.

> [!NOTE]
> název hostitele Hello `contosoapi.azure-api.net` hello výchozí proxy hostname-li je konfigurována s názvem služby `contosoapi` je vytvořen v veřejný Azure. 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Krok 7

Nahrajte certifikát hello toobe použít na hello povolen protokol SSL back-end fondu zdrojů. Toto je hello stejný certifikát, který jste zadali v kroku 4 výše.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path too.cer file>
```

### <a name="step-8"></a>Krok 8

Nakonfigurujte nastavení HTTP back-end pro hello Application Gateway. To zahrnuje nastavení časového limitu pro požadavek back-end, po jejímž uplynutí se zrušil. Tato hodnota se liší od časový limit testu hello.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Krok 9

Nakonfigurujte fond back-end IP adresy s názvem **apimbackend** s hello interní virtuální IP adresu hello služba API Management vytvořili výše.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Krok 10

Vytvořte nastavení pro fiktivní back-end (neexistující). Cesty tooAPI požadavky, které jsme nechcete tooexpose ze správy rozhraní API prostřednictvím brány aplikace bude dosáhl tento back-end a vrátit 404.

Nakonfigurujte nastavení protokolu HTTP pro fiktivní back-end hello.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Konfigurace fiktivní back-end **dummyBackendPool**, která odkazuje tooa plně kvalifikovaný název domény adresy **dummybackend.com**. Tato adresa plně kvalifikovaný název domény ve virtuální síti hello neexistuje.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Vytvoření pravidla nastavení tohoto hello Application Gateway bude používat ve výchozím nastavení, které odkazuje back-end neexistující toohello **dummybackend.com** v hello virtuální sítě.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>Krok 11

Konfigurovat pravidla cesty adresy URL pro hello back endové fondy. To umožňuje výběr jen některé z hello rozhraní API pro správu rozhraní API pro probíhá zveřejněné toohello veřejné. Například, pokud existují `Echo API` (/ echo /), `Calculator API` (/calc/) atd. Zkontrolujte pouze `Echo API` přístupné z Internetu). 

Hello následující příklad vytvoří jednoduché pravidlo pro hello "/ echo /" cesty směrování provozu toohello back-end "apimProxyBackendPool".

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Pokud cesta hello neodpovídá hello cesta pravidla chceme tooenable ze správy rozhraní API, hello pravidlo cesty mapy konfigurace nakonfiguruje taky výchozího fondu adres back-end s názvem **dummyBackendPool**. Například http://api.contoso.net/calc/ * přejde příliš**dummyBackendPool** definovaným jako hello výchozí fond pro zrušení odpovídající provoz.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

Hello výše krok zajistí, který pouze požadavky pro cestu hello "/ odezvu" jsou povoleny prostřednictvím hello Application Gateway. Tooother požadavky, které rozhraní API nakonfigurovaný ve službě API Management vyvolá výjimku chyby 404 ze Application Gateway při přístupu z Internetu hello. 

### <a name="step-12"></a>Krok 12

Vytvoření pravidla nastavení pro hello Application Gateway toouse směrování adres URL na základě cesty.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>Krok 13

Nakonfigurujte hello počet instancí a velikost hello Application Gateway. Tady používáme hello [firewall webových aplikací SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) pro zvýšení zabezpečení hello prostředků API Management.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>Krok 14

Nakonfigurujte toobe firewall webových aplikací v režimu "Prevence".
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

Vytvoření služby Application Gateway se všemi objekty konfigurace hello z předchozích kroků hello.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-hello-api-management-proxy-hostname-toohello-public-dns-name-of-hello-application-gateway-resource"></a>CNAME hello API Management proxy hostitele toohello veřejného názvu DNS hello prostředku aplikační brány

Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci. Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který nemusí být snadno toouse. 

Hello Application Gateway název DNS by měl být použité toocreate záznam CNAME, který ukazuje název hostitele proxy APIM hello (například `api.contoso.net` ve výše uvedených příkladech hello) toothis název DNS. tooconfigure hello front-endu záznam IP CNAME načíst hello podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress hello. použití Hello záznamů A nedoporučuje, protože hello VIP může změnit při restartu brány.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"></a> Souhrn
Nakonfigurovat ve virtuální síti Azure API Management poskytuje rozhraní jedna brána pro všechny nakonfigurované rozhraní API, ať už jsou hostované v místní nebo v cloudu hello. Integrace Application Gateway s API Management nabízí flexibilitu hello selektivně povolení konkrétní rozhraní API toobe dostupné na hello Internet, jakož i poskytnutí brány Firewall webových aplikací jako instance API Management tooyour front-endu.

##<a name="next-steps"></a> Další kroky
* Další informace o Azure Application Gateway
  * [Přehled brány aplikace](../application-gateway/application-gateway-introduction.md)
  * [Brány Firewall webových aplikací Application Gateway](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Aplikační bránu pomocí směrování na základě cesty](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Další informace o rozhraní API správy a virtuální sítě
  * [Použití služby API Management, které jsou k dispozici pouze v rámci hello virtuální sítě](api-management-using-with-internal-vnet.md)
  * [Použití služby API Management ve virtuální síti](api-management-using-with-vnet.md)
