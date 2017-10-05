---
title: "Konfigurace protokolu SSL koncová s Azure Application Gateway | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat Application Gateway pomocí Azure Resource Manager PowerShell koncové SSL"
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
ms.openlocfilehash: 6d969d6a0c649c263e1d5bb99bdbceec484cb9a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfigurace protokolu SSL koncová s Application Gateway pomocí prostředí PowerShell

## <a name="overview"></a>Přehled

Aplikační brána podporuje koncová šifrování přenosů. Služba Application Gateway to provádí ukončením připojení protokolem SSL ve službě Application Gateway. Brána následně použije na provoz pravidla směrování, znovu zašifruje paket a předá tento paket do příslušného back-endu na základě nadefinovaných pravidel směrování. Každá odpověď webového serveru prochází ke koncovému uživateli stejným procesem.

Jiné funkce této aplikační brána podporuje, definování vlastních možností protokolu SSL. Aplikační brána podporuje, zakázání následující verze protokolu; **TLSv1.0**, **TLSv1.1**, a **TLSv1.2** jako dobře definující která sady mají být použity v pořadí podle preference šifer.  Další informace o konfigurovatelných možností protokolu SSL, najdete [zásady protokolu SSL přehled](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> Protokol SSL 2.0 a SSL 3.0 jsou ve výchozím nastavení zakázané a nemůže být povolena. Tyto považovány za nezabezpečená a nemůžete použít s aplikační brány.

![scénář image][scenario]

## <a name="scenario"></a>Scénář

V tomto scénáři zjistíte postup vytvoření služby application gateway pomocí protokolu SSL koncová pomocí prostředí PowerShell.

Tento scénář se:

* Vytvořte skupinu prostředků s názvem **appgw-rg**
* Vytvořit virtuální síť s názvem **appgwvnet** s adresním prostorem 10.0.0.0/16.
* Vytvořte dvě podsítě názvem **appgwsubnet** a **appsubnet**.
* Vytvoření brány malá aplikace podpora šifrování SSL koncová této verze protokoly SSL omezení a šifrovacích sad.

## <a name="before-you-begin"></a>Než začnete

Konfigurace protokolu SSL koncová s aplikační bránu, vyžaduje se certifikát pro bránu a certifikáty se vyžadují pro back-end serverů. Certifikát brány se používá k šifrování a dešifrování data odesílaná do pomocí protokolu SSL. Certifikát brány musí být ve formátu Personal Information Exchange (pfx). Tento formát souboru umožňuje pro export privátního klíče, který je vyžadován součástí služby application gateway k šifrování a dešifrování přenosů.

Pro šifrování SSL koncová back-end musí být seznam povolených adres s aplikační brány. K tomu je potřeba odesílání veřejný certifikát back-EndY aplikační brány. Tím se zajistí, že aplikační bránu pouze komunikuje s instancí známé back-end. Tato další zabezpečuje komunikaci začátku do konce.

Tento proces je popsán v následujících krocích:

## <a name="create-the-resource-group"></a>Vytvořte skupinu prostředků

Tato část vás provede procesem vytvoření skupiny prostředků, který obsahuje službu application gateway.

### <a name="step-1"></a>Krok 1

Přihlaste se k účtu Azure.

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Krok 2

Vyberte předplatné, které chcete použít pro tento scénář.

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a>Krok 3

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvoření virtuální sítě a podsítě pro službu Application Gateway

Následující příklad vytvoří virtuální síť a dvě podsítě. Jednu podsíť se používá k ukládání aplikační brány. Další podsítě se používá pro back-EndY hostování webové aplikace.

### <a name="step-1"></a>Krok 1

Přiřaďte rozsah adres pro podsíť použít pro službu Application Gateway sám sebe.

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> Podsítě konfigurované pro službu application gateway by měl být správnou velikost. Aplikační brány mohou být konfigurovány pro až 10 instancí. Každá instance trvá jednu IP adresu z podsítě. Příliš malé podsítě může nepříznivě ovlivnit škálování služby application gateway.
> 
> 

### <a name="step-2"></a>Krok 2

Přiřaďte rozsah adres, který se má použít pro back-end fondu adres.

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a>Krok 3

Vytvořte virtuální síť s podsítí definované v předchozích krocích.

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a>Krok 4

Načtení prostředků virtuální sítě a prostředky podsítě, který se má použít v následujících krocích:

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci

Vytvořte prostředek veřejné IP, který se má použít pro službu application gateway. Tato veřejná IP adresa se používá následující krok.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Aplikační brána nepodporuje použití veřejnou IP adresu vytvořit s definovaným popiskem domény. Je podporován pouze veřejné IP adresy s popiskem dynamicky vytvořené domény. Pokud budete potřebovat dns popisný název pro službu application gateway, se doporučuje použít záznam CNAME jako alias.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvořte objekt konfigurace aplikační brány 

Před vytvořením služby application gateway se nastavit všechny položky konfigurace. Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.

### <a name="step-1"></a>Krok 1

Vytvoření konfigurace IP aplikační brány, toto nastavení konfiguruje jaké podsíť používá aplikační bránu. Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě a směruje síťový provoz na IP adresy ve fondu back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a>Krok 2

Vytvořte konfiguraci front-end IP adresy, toto nastavení se mapuje na privátní nebo veřejné ip adresu front-endu služby application gateway. Následující krok přidruží veřejnou IP adresu v předchozím kroku s konfigurací front-end IP adresy.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a>Krok 3

Nakonfigurujte fond back-end IP adres pomocí IP adresy webových serverů back-end. Tyto IP adresy jsou IP adresy, které přijímají síťový provoz, který přichází z koncového bodu front-end IP adresy. Můžete nahradit následující adresy přidat koncové body vlastní aplikace IP adresu.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> Platný plně kvalifikovaný název domény (FQDN) je také platnou hodnotu místo ip adresu pro back-end serverů s přepínačem - BackendFqdns. 

### <a name="step-4"></a>Krok 4

Nakonfigurujte port front-end IP pro koncový bod veřejné IP adresy. Tento port je port, který koncoví uživatelé připojit k.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte certifikát pro službu application gateway. Tento certifikát se používá k dešifrování a znovu zašifrovat přenosy ve službě application gateway.

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

> [!NOTE]
> Tímto vzorovým kódem se nakonfiguruje certifikát používaný pro připojení SSL. Certifikát musí být ve formátu .pfx, heslo musí mít 4 až 12 znaků.

### <a name="step-6"></a>Krok 6

Vytvořte naslouchací proces protokolu HTTP pro službu application gateway. Přiřadíte konfigurace front-end IP adresu, port a certifikátu SSL se má použít.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a>Krok 7

Nahrajte certifikát, který se má použít na SSL povoleno back-end fondu zdrojů.

> [!NOTE]
> Získá výchozí kontroly veřejného klíče z **výchozí** vazby SSL na IP adrese a porovná ji sem zadáte hodnotu veřejného klíče obdrží hodnotě veřejných klíčů můžete back-end. Načtený veřejný klíč nemusí být nutně zamýšlená lokalita, na které přenosové toky **Pokud** používáte hlavičky hostitele a SNI na back-end. Pokud máte pochybnosti, navštivte https://127.0.0.1/ na back EndY k potvrzení, který certifikát se používá pro **výchozí** vazbu SSL. V této části použijte veřejný klíč z tohoto požadavku. Pokud používáte hlavičky hostitele a SNI na vazby HTTPS a neobdržíte odpověď a certifikát z žádost ruční prohlížeče na https://127.0.0.1/ na back EndY, musíte vytvořit vazbu výchozí SSL na back EndY. Pokud to neuděláte, selhání sondy a back-end není povolený.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> Certifikát zadaný v tomto kroku musí být veřejný klíč certifikátu pfx, nachází na back-end. Exportujte certifikátu (ne kořenový certifikát), nainstalovaný na serveru back-end v. CER formátu a použít ho v tomto kroku. Tento krok povolených programů back-end pomocí služby application gateway.

### <a name="step-8"></a>Krok 8

Nakonfigurujte nastavení http back-end služby application gateway. Přiřadíte certifikát odeslali v předchozím kroku nastavení protokolu http.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a>Krok 9

Vytvořte směrování pravidlo služby load balancer které konfiguruje chování nástroje pro vyrovnávání zatížení. V tomto příkladu se vytvoří pravidlo základní kruhové dotazování.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a>Krok 10

Nakonfigurujte velikost instance služby Application Gateway.  Dostupné velikosti jsou **standardní\_malé**, **standardní\_střední**, a **standardní\_velké**.  Pro kapacitu dostupné hodnoty jsou 1 až 10.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Počet instancí 1 lze zvolit pro účely testování. Je důležité vědět, že libovolný počet instancí v rámci dvě instance není předmětem smlouvě SLA a proto nedoporučují. Malé brány se mají použít pro vývoj testování a ne pro produkční účely.

### <a name="step-11"></a>Krok 11

Nakonfigurujte zásady protokolu SSL pro službu Application Gateway používat. Aplikační brána podporuje možnost nastavit minimální verze pro verze protokolu SSL.

Seznam verzí protokolu, které lze definovat jsou následující hodnoty.

* **TLSv1_0**
* **TLSv1_1**
* **TLSv1_2**

Nastaví verzi protokolu minimální **TLSv1_2** a umožňuje **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**a  **Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** pouze.

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-the-application-gateway"></a>Vytvoření aplikační brány

Pomocí všech předchozích kroků vytvořte aplikační bránu. Vytvoření brány je dlouhotrvající proces.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Omezit verzí protokolu SSL na existující aplikační brány

Předchozí kroky trvat vás vytváření aplikací pomocí protokolu SSL a provést tak kompletní a zakázání určitých verzí protokolu SSL. Následující příklad zakazuje určité zásady protokolu SSL na existující aplikační brány.

### <a name="step-1"></a>Krok 1

Načtení služby application gateway k aktualizaci.

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a>Krok 2

Definujte zásady protokolu SSL. V následujícím příkladu TLSv1.0 a TLSv1.1 jsou zakázány a šifrovací sada **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, a  **Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** jsou povoleny pouze ty.

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a>Krok 3

Nakonec aktualizujte bránu. Je důležité si uvědomit, že tento poslední krok je dlouho spuštěná úloha. Až skončíte, koncová SSL je nakonfigurován ve službě application gateway.

```powershell
$gw | Set-AzureRmApplicationGateway
```

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

Další informace o posílení zabezpečení webových aplikací pomocí brány Firewall webových aplikací prostřednictvím brány aplikace navštivte stránky [brány Firewall webových aplikací – přehled](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
