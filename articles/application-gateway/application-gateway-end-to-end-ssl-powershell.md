---
title: aaaConfigure end tooend SSL s Azure Application Gateway | Microsoft Docs
description: "Tento článek popisuje, jak tooconfigure ukončení tooend SSL s Application Gateway pomocí Azure Resource Manager PowerShell"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 7c280478e143d309e7665219441cbee8c81d9a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-end-tooend-ssl-with-application-gateway-using-powershell"></a>Nakonfigurovat koncové tooend SSL Application Gateway pomocí prostředí PowerShell

## <a name="overview"></a>Přehled

Aplikace podporuje brány ukončení tooend šifrování přenosů. Aplikační brána dosahuje tím, že se ukončuje připojení SSL hello na hello aplikační brány. Brána Hello poté použije pravidla směrování hello toohello provoz, znovu je zašifruje hello paketů a předá hello paketu toohello odpovídající back-end na základě pravidel směrování hello definované. Odpověď od hello webový server, na které se prochází hello stejný proces back toohello koncového uživatele.

Jiné funkce této aplikační brána podporuje, definování vlastních možností protokolu SSL. Aplikační brána podporuje následující verze protokolu; zakázání hello **TLSv1.0**, **TLSv1.1**, a **TLSv1.2** také definování hello, který šifer sady toouse a hello pořadí podle priority.  toolearn Další informace o konfigurovatelných možností protokolu SSL, navštivte [zásady protokolu SSL přehled](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> Protokol SSL 2.0 a SSL 3.0 jsou ve výchozím nastavení zakázané a nemůže být povolena. Tyto považovány za nezabezpečená a nejsou možné toobe použít s aplikační brány.

![scénář image][scenario]

## <a name="scenario"></a>Scénář

V tomto scénáři zjistíte, jak toocreate brány aplikace pomocí ukončení tooend SSL pomocí prostředí PowerShell.

Tento scénář se:

* Vytvořte skupinu prostředků s názvem **appgw-rg**
* Vytvořit virtuální síť s názvem **appgwvnet** s adresním prostorem 10.0.0.0/16.
* Vytvořte dvě podsítě názvem **appgwsubnet** a **appsubnet**.
* Vytvořte malá aplikace brány podpůrné tooend SSL šifrování této verze protokoly SSL omezení a šifrovacích sad.

## <a name="before-you-begin"></a>Než začnete

tooconfigure end tooend SSL s aplikační bránu, vyžaduje se certifikát pro bránu hello a certifikáty se vyžadují pro hello back-end serverů. certifikát brány Hello je použité tooencrypt a dešifrování hello provoz odeslaný tooit pomocí protokolu SSL. certifikát brány Hello musí toobe formát Personal Information Exchange (pfx). Tento formát souboru umožňuje pro hello privátní klíče toobe exportovali, které je požadované hello aplikace brány tooperform hello šifrování a dešifrování přenosů.

Pro element end tooend SSL šifrování hello back-end musí být seznam povolených adres s aplikační brány. K tomu je potřeba odesílání hello veřejný certifikát hello back-EndY toohello aplikační brány. Tím se zajistí, že tento aplikační brána hello pouze komunikuje s instancí známé back-end. Tato další zabezpečuje komunikaci tooend end hello.

Tento proces je popsán v hello následující kroky:

## <a name="create-hello-resource-group"></a>Vytvoření hello skupiny prostředků

Tato část vás provede procesem vytvoření skupiny prostředků, který obsahuje hello aplikační brány.

### <a name="step-1"></a>Krok 1

Přihlaste se tooyour účet Azure.

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Krok 2

Vyberte hello toouse předplatné pro tento scénář.

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a>Krok 3

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu hello application gateway

Hello následující příklad vytvoří virtuální síť a dvě podsítě. Jednu podsíť je použité toohold hello aplikační brány. Hello jiné podsítě se používá pro hello back-EndY hostování hello webovou aplikaci.

### <a name="step-1"></a>Krok 1

Přiřaďte rozsah adres pro podsíť hello použít pro hello Application Gateway, sám sebe.

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> Podsítě konfigurované pro službu application gateway by měl být správnou velikost. Aplikační bránu můžete nakonfigurovat pro až too10 instance. Každá instance trvá jednu IP adresu z podsítě hello. Příliš malé podsítě může nepříznivě ovlivnit škálování služby application gateway.
> 
> 

### <a name="step-2"></a>Krok 2

Přiřadíte toobe rozsah adres použitý pro hello back-endových adres.

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a>Krok 3

Vytvořte virtuální síť s podsítí hello definované v předchozích krocích hello.

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a>Krok 4

Načtení hello virtuální sítě prostředků a podsíť prostředky toobe použít v hello následující kroky:

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci hello

Vytvoření veřejné toobe prostředků IP použít pro službu hello application gateway. Tato veřejná IP adresa se používá následující krok.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Aplikační brána nepodporuje použití hello veřejnou IP adresu vytvořit s definovaným popiskem domény. Je podporován pouze veřejné IP adresy s popiskem dynamicky vytvořené domény. Pokud budete potřebovat dns popisný název hello aplikační bránu, je vhodné toouse záznam CNAME záznam jako alias.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvořte objekt konfigurace aplikační brány 

Všechny položky konfigurace jsou nastavené před vytvořením hello aplikační brány. Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.

### <a name="step-1"></a>Krok 1

Vytvoření konfigurace IP aplikační brány, toto nastavení konfiguruje, jaké používá podsíť hello aplikace brány. Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě hello a směruje síťový provoz toohello IP adresy ve fondu back-end IP hello. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a>Krok 2

Vytvořte konfiguraci front-end IP adresy, toto nastavení se mapuje privátní nebo veřejné ip adresy toohello front-endu hello aplikační brány. Následující krok Hello přidruží hello veřejnou IP adresu v předchozím kroku s konfigurací front-end IP adresy hello hello.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a>Krok 3

Nakonfigurujte hello back-end IP adres fondu IP adres hello hello back-end webových serverů. Tyto IP adresy jsou hello IP adresy, které přijímají hello síťový provoz, který přichází z hello koncového bodu front-end IP adresy. Nahraďte hello následující IP adresy tooadd koncovými body IP adresy vlastní aplikace.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> Platný plně kvalifikovaný název domény (FQDN) je také platnou hodnotu místo ip adresu pro hello back-end serverů pomocí přepínače - BackendFqdns hello. 

### <a name="step-4"></a>Krok 4

Nakonfigurujte port front-end IP hello pro hello koncový bod veřejné IP adresy. Toto je hello port, který koncoví uživatelé připojit k.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte certifikát hello hello aplikační brány. Tento certifikát je použité toodecrypt a znovu zašifrovat přenosy hello na hello aplikační brány.

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

> [!NOTE]
> Tato ukázka nakonfiguruje hello certifikátu používaného pro připojení SSL. certifikát Hello musí toobe ve formátu .pfx a hello heslo musí být mezi 4 znaky too12.

### <a name="step-6"></a>Krok 6

Vytvořte naslouchací proces protokolu HTTP hello hello aplikační brány. Přiřaďte hello konfiguraci front-end IP adresy, portu a toouse certifikát SSL.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a>Krok 7

Nahrajte certifikát hello toobe použít na hello SSL s povoleným back-end fondu zdrojů.

> [!NOTE]
> získá výchozí kontroly Hello hello veřejný klíč z hello **výchozí** vazby SSL na hello back-end na IP adrese a porovnává hello hodnota veřejného klíče obdrží hodnota toohello veřejného klíče tady zadáte. Hello načtené veřejný klíč nemusí být nutně hello určené lokality toowhich přenosové toky **Pokud** používáte hlavičky hostitele a SNI na hello back-end. Pokud máte pochybnosti, navštivte https://127.0.0.1/ na tooconfirm hello back EndY, který certifikát se používá pro hello **výchozí** vazbu SSL. V této části použijte hello veřejný klíč z tohoto požadavku. Pokud používáte hlavičky hostitele a SNI na vazby HTTPS a neobdržíte odpověď a certifikát z toohttps://127.0.0.1/ požadavek ruční prohlížeče na hello back EndY, musíte vytvořit vazbu výchozí SSL na back EndY hello. Pokud to neuděláte, selhání sondy a hello back-end není povolený.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> zadaný v tomto kroku Hello certifikát by měl být hello veřejný klíč certifikátu pfx hello v back-end hello. Exportujte certifikátu hello (ne hello kořenový certifikát) nainstalován na back-end server hello. CER formátu a použít ho v tomto kroku. Tento krok povolených programů hello back-end s hello aplikační brány.

### <a name="step-8"></a>Krok 8

Nakonfigurujte nastavení http back-end hello aplikační brány. Přiřadíte certifikát hello nahráli v předchozím kroku nastavení http toohello hello.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a>Krok 9

Vytvořte směrování pravidlo služby load balancer které konfiguruje chování nástroje pro vyrovnávání zatížení hello. V tomto příkladu se vytvoří pravidlo základní kruhové dotazování.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a>Krok 10

Nakonfigurujte velikost instance hello hello aplikační brány.  jsou k dispozici velikosti Hello **standardní\_malé**, **standardní\_střední**, a **standardní\_velké**.  Pro kapacitu hello dostupné hodnoty jsou 1 až 10.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Počet instancí 1 lze zvolit pro účely testování. Je důležité tooknow, aby všechny instance počet v části dvě instance není předmětem hello SLA a proto nedoporučují. Malé brány jsou toobe použít pro vývoj testování a ne pro produkční účely.

### <a name="step-11"></a>Krok 11

Nakonfigurujte toobe zásady protokolu SSL hello používá na hello Application Gateway. Aplikační brána podporuje hello možnost tooset na minimální verzi pro verze protokolu SSL.

Hello následující hodnoty jsou seznam verze protokolu, které lze definovat.

* **TLSv1_0**
* **TLSv1_1**
* **TLSv1_2**

Nastaví hello minimální protocol verze příliš**TLSv1_2** a umožňuje **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_ SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**a **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** pouze.

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-hello-application-gateway"></a>Vytvoření hello Application Gateway

Pomocí všechny hello předchozích kroků vytvořte hello Application Gateway. Vytvoření Hello hello brány je dlouhotrvající proces.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Omezit verzí protokolu SSL na existující aplikační brány

Hello předchozí kroky vás vytváření aplikací s end tooend SSL a zakázání určitých verzí protokolu SSL. Hello následující příklad zakazuje určité zásady protokolu SSL na existující aplikační brány.

### <a name="step-1"></a>Krok 1

Načtěte tooupdate brány aplikace hello.

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a>Krok 2

Definujte zásady protokolu SSL. V následujícím příkladu hello, TLSv1.0 a TLSv1.1 jsou zakázány a hello šifrovací sady **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, a  **Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** jsou povoleny pouze ty hello.

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a>Krok 3

Nakonec aktualizujte hello brány. Je důležité toonote, že tento poslední krok je dlouho spuštěná úloha. Až skončíte, end tooend SSL je nakonfigurován na hello aplikační brány.

```powershell
$gw | Set-AzureRmApplicationGateway
```

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

Další informace o posílení zabezpečení hello webových aplikací pomocí brány Firewall webových aplikací prostřednictvím brány aplikace navštivte stránky [brány Firewall webových aplikací – přehled](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
