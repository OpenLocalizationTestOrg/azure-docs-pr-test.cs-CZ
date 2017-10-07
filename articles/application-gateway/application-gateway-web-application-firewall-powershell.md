---
title: "brány firewall v protokolu aaaConfigure webových aplikací – Azure Application Gateway | Microsoft Docs"
description: "Tento článek obsahuje pokyny k jak toostart pomocí webové aplikace brány firewall na existující nebo nové aplikační brány."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: bd2a69901f0ec0d6551d633e2588b74c3ab59a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurace brány firewall webových aplikací na nový nebo existující Application Gateway

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Zjistěte, jak toocreate brány firewall webových aplikací povoleno aplikační bránu, nebo přidejte webové aplikace brány firewall tooan existující Application Gateway.

Hello brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně. Application Gateway poskytuje funkce doručování řadiče (ADC) mnoho aplikací se včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, vrstvu secure Sockets Layer (SSL) snižování zátěže, sondy vlastní stavu, podporu pro více lokalit a mnohé další. toofind úplný seznam podporovaných funkcích, navštivte: [Přehled služby Application Gateway](application-gateway-introduction.md).

Hello následující článek ukazuje, jak příliš[webové aplikace brány firewall tooan existující aplikační brány přidat](#add-web-application-firewall-to-an-existing-application-gateway) a [vytvoření služby Application Gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).

![scénář image][scenario]

## <a name="waf-configuration-differences"></a>Rozdíly konfigurace firewall webových aplikací

Pokud jste si přečetli [vytvoření služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway-arm.md), rozumíte hello SKU nastavení tooconfigure při vytváření služby Application Gateway. Firewall webových aplikací poskytuje další nastavení toodefine při konfiguraci hello SKU na aplikační brány. Neexistují žádné další změny, které provedete na hello samotné bráně aplikace.

| **Nastavení** | **Podrobnosti**
|---|---|
|**SKU** |Normální Application Gateway bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké** velikosti. Se zavedením hello firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**. Malé aplikačních bran firewall webových aplikací nepodporuje.|
|**Vrstvy** | Hello dostupné jsou hodnoty **standardní** nebo **firewall webových aplikací**. Při použití brány firewall webových aplikací, **firewall webových aplikací** musí být vybrána.|
|**Režim** | Toto nastavení je režim hello firewall webových aplikací. Povolené hodnoty jsou **detekce** a **prevence**. Pokud je v režimu zjišťování nastavení firewall webových aplikací, všechny hrozby jsou uloženy v souboru protokolu. V režimu prevence události jsou protokolovány ale hello útočník obdrží 403 neoprávněným odpověď z hello Application Gateway.|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a>Přidání webové aplikace brány firewall tooan existující Application Gateway

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

1. Přihlaste se tooyour účet Azure.

    ```powershell
    Login-AzureRmAccount
    ```

2. Vyberte hello toouse předplatné pro tento scénář.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Načtěte hello brány, který chcete přidat brány firewall webových aplikací na.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. Nakonfigurujte skladová položka brány firewall hello webové aplikace. jsou k dispozici velikosti Hello **firewall webových aplikací\_velké** a **firewall webových aplikací\_střední**. Při použití brány firewall webových aplikací hello vrstva musí být **firewall webových aplikací**, hello kapacita musí být potvrzen, při nastavování hello sku.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. Nakonfigurujte nastavení hello firewall webových aplikací, jak jsou definovány v hello následující ukázka:

   Pro **FirewallMode**, jsou k dispozici hodnoty hello prevence a zjišťování.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. Aktualizujte hello Application Gateway s nastavením hello definované v předchozím kroku hello.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Tento příkaz aktualizuje hello Application Gateway pomocí brány firewall webových aplikací. Navštivte [diagnostics Application Gateway](application-gateway-diagnostics.md) toounderstand jak tooview protokoly pro službu Application Gateway. Z důvodu zabezpečení povaze toohello firewall webových aplikací zkontrolovat toobe třeba protokoly pravidelně toounderstand hello postavení zabezpečení webových aplikací.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření služby Application Gateway pomocí brány firewall webových aplikací

Hello následující kroky vás prostřednictvím hello celý proces od začátku tooend pro vytvoření služby Application Gateway pomocí brány firewall webových aplikací.

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

1. Přihlaste se spuštěním tooAzure `Login-AzureRmAccount`, jsou výzvami tooauthenticate pomocí svých přihlašovacích údajů.

1. Zkontrolujte předplatná hello pro účet hello spuštěním`Get-AzureRmSubscription`

1. Zvolte, které vaše toouse předplatných Azure.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pro hello Application Gateway.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy, které používá toocreate na aplikační brána hello stejnou skupinu prostředků.

V předchozím příkladu hello jsme vytvořili skupinu prostředků s názvem "Appgw-RG" a umístěním "západní USA."

> [!NOTE]
> Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).

### <a name="configure-virtual-network"></a>Konfigurace virtuální sítě

Aplikační brána vyžaduje vlastní podsíť. V tomto kroku vytvoříte virtuální síť s adresní prostor 10.0.0.0/16 a dvě podsítě – jednu pro hello aplikační brány a jeden pro členy fondu back-end.

```powershell
# Create a subnet configuration object for hello Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in hello subnet for Application Gateway instances. With a smaller subnet, you may not be able tooadd more instance of your Application Gateway in hello future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for hello backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create hello virtual network with hello previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a>Konfigurace veřejné IP adresy

Aplikační brána v pořadí toohandle externí požadavků, vyžaduje veřejnou IP adresu. Tato veřejná IP adresa nesmí mít `DomainNameLabel` definované toobe používané hello Application Gateway.

```powershell
# Create a public IP address for use with hello Application Gateway. Defining hello domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-hello-application-gateway"></a>Konfigurace hello Application Gateway

```powershell
# Create a IP configuration. This configures what subnet hello Application Gateway uses. When Application Gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool toohold hello addresses or NICs for hello application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload hello authenication certificate that will be used toocommunicate with hello backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path too.cer file>

# Conifugre hello backend HTTP settings toobe used toodefine how traffic is routed toohello backend pool. hello authenication certificate used in hello previous step is added toohello backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port toobe used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration tooassociate hello public IP address with hello Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure hello certificate for hello Application Gateway. This certificate is used toodecrypt and re-encrypt hello traffic on hello Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>

# Create hello HTTP listener for hello Application Gateway. Assign hello front-end ip configuration, port, and ssl certificate toouse.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures hello load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU of hello Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define hello SSL policy toouse
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure hello waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create hello Application Gateway utilizing all hello previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Application Gateway, které jsou vytvořené pomocí konfigurace brány firewall hello základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="get-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci. Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který není popisný. tooensure koncoví uživatelé mohou dosáhl hello Application Gateway, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello Application Gateway. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure alias, načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello Application Gateway. název DNS Hello Application Gateway by měl být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS. použití Hello záznamů A nedoporučuje, protože hello VIP může změnit při restartování služby Application Gateway.

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

Zjistěte, jak protokolování diagnostiky tooconfigure, toolog hello události, které jsou zjištěna nebo předcházet pomocí brány firewall webových aplikací, navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
