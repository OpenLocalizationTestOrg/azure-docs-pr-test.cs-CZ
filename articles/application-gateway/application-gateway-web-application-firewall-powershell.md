---
title: "Konfigurace brány firewall webových aplikací – Azure Application Gateway | Microsoft Docs"
description: "Tento článek obsahuje pokyny o tom, jak začít používat brány firewall webových aplikací na existující nebo nové aplikační brány."
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
ms.openlocfilehash: dab489a1c9fb7d4a51b9ccbce543b209bec23575
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurace brány firewall webových aplikací na nový nebo existující Application Gateway

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Zjistěte, jak vytvořit webovou aplikaci povoleno Application Gateway s branou firewall nebo přidání brány firewall webových aplikací do existující aplikační brány.

Brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu. Application Gateway poskytuje funkce doručování řadiče (ADC) mnoho aplikací se včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, vrstvu secure Sockets Layer (SSL) snižování zátěže, sondy vlastní stavu, podporu pro více lokalit a mnohé další. Úplný seznam podporovaných funkcích naleznete: [Přehled služby Application Gateway](application-gateway-introduction.md).

Tento článek ukazuje postup [přidání brány firewall webových aplikací do existující aplikace bránu](#add-web-application-firewall-to-an-existing-application-gateway) a [vytvoření služby Application Gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).

![scénář image][scenario]

## <a name="waf-configuration-differences"></a>Rozdíly konfigurace firewall webových aplikací

Pokud jste si přečetli [vytvoření služby Application Gateway pomocí prostředí PowerShell](application-gateway-create-gateway-arm.md), rozumíte SKU nastavení ke konfiguraci při vytváření služby Application Gateway. Firewall webových aplikací poskytuje další nastavení můžete definovat při konfiguraci verze SKU na aplikační brány. Neexistují žádné další změny, které provedete ve službě Application Gateway sám sebe.

| **Nastavení** | **Podrobnosti**
|---|---|
|**SKU** |Normální Application Gateway bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké** velikosti. Se zavedením firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**. Malé aplikačních bran firewall webových aplikací nepodporuje.|
|**Vrstvy** | Dostupné hodnoty jsou **standardní** nebo **firewall webových aplikací**. Při použití brány firewall webových aplikací, **firewall webových aplikací** musí být vybrána.|
|**Režim** | Toto nastavení je režim firewall webových aplikací. Povolené hodnoty jsou **detekce** a **prevence**. Pokud je v režimu zjišťování nastavení firewall webových aplikací, všechny hrozby jsou uloženy v souboru protokolu. V režimu prevence události jsou protokolovány ale útočník přijme 403 neoprávněným odpověď ze služby Application Gateway.|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Přidání brány firewall webových aplikací do existující aplikační brány

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

1. Přihlaste se k účtu Azure.

    ```powershell
    Login-AzureRmAccount
    ```

2. Vyberte předplatné, které chcete použít pro tento scénář.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Načtěte brány, kterou přidáváte brány firewall webových aplikací na.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. Nakonfigurujte sku webové aplikace brány firewall. Dostupné velikosti jsou **firewall webových aplikací\_velké** a **firewall webových aplikací\_střední**. Při použití brány firewall webových aplikací vrstva musí být **firewall webových aplikací**, kapacita musí být potvrzen, při nastavování verze sku.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. Nakonfigurujte nastavení firewall webových aplikací, jak jsou definovány v následujícím příkladu:

   Pro **FirewallMode**, dostupné hodnoty jsou prevence a zjišťování.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. Aktualizujte aplikační bránu s nastavením, které jsou definované v předchozím kroku.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Tento příkaz aktualizuje aplikační bránu pomocí brány firewall webových aplikací. Navštivte [diagnostics Application Gateway](application-gateway-diagnostics.md) pochopit, jak zobrazit protokoly pro službu Application Gateway. Vzhledem k zabezpečení povaze firewall webových aplikací protokoly třeba, aby pravidelně pochopit postavení zabezpečení webových aplikací.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření služby Application Gateway pomocí brány firewall webových aplikací

Následující postup vás provede celý proces od začátku do konce pro vytvoření služby Application Gateway pomocí brány firewall webových aplikací.

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

1. Přihlaste se k Azure spuštěním `Login-AzureRmAccount`, zobrazí se výzva k ověření pomocí svých přihlašovacích údajů.

1. Zkontrolujte předplatná pro příslušný účet spuštěním`Get-AzureRmSubscription`

1. Zvolte předplatné Azure, které chcete použít.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pro službu Application Gateway.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy k vytvoření služby Application Gateway používá stejnou skupinu prostředků.

V předchozím příkladu jsme vytvořili skupinu prostředků s názvem "appgw-RG" a umístěním "Západní USA".

> [!NOTE]
> Pokud potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přečtěte si téma [vytvoření služby Application Gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).

### <a name="configure-virtual-network"></a>Konfigurace virtuální sítě

Aplikační brána vyžaduje vlastní podsíť. V tomto kroku vytvoříte virtuální síť s adresní prostor 10.0.0.0/16 a dvě podsítě, jeden pro službu Application Gateway a jeden pro členy fondu back-end.

```powershell
# Create a subnet configuration object for the Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for Application Gateway instances. With a smaller subnet, you may not be able to add more instance of your Application Gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a>Konfigurace veřejné IP adresy

Aby bylo možné zpracovávat požadavky externí, aplikační brána vyžaduje veřejnou IP adresu. Tato veřejná IP adresa nesmí mít `DomainNameLabel` definovaná používat službu Application Gateway.

```powershell
# Create a public IP address for use with the Application Gateway. Defining the domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Nakonfigurujte aplikační bránu

```powershell
# Create a IP configuration. This configures what subnet the Application Gateway uses. When Application Gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool to hold the addresses or NICs for the application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authenication certificate that will be used to communicate with the backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Conifugre the backend HTTP settings to be used to define how traffic is routed to the backend pool. The authenication certificate used in the previous step is added to the backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration to associate the public IP address with the Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the Application Gateway. This certificate is used to decrypt and re-encrypt the traffic on the Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the Application Gateway. Assign the front-end ip configuration, port, and ssl certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures the load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure the waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the Application Gateway utilizing all the previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Application Gateway vytvoří s konfigurací brány firewall základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="get-application-gateway-dns-name"></a>Získat název DNS brány aplikace

Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci. Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který není popisný. Zajistěte, aby koncoví uživatelé mohou dosáhl aplikační bránu, záznamu CNAME použít tak, aby odkazoval koncový bod veřejné aplikační brány. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Pokud chcete konfigurovat alias, načíst podrobnosti o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress připojit k službě Application Gateway. Název DNS služby Application Gateway by měl použít k vytvoření záznamu CNAME, který ukazuje dva webové aplikace k tomuto názvu DNS. Použití záznamů A se nedoporučuje, protože VIP může změnit při restartování služby Application Gateway.

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

Naučte se konfigurovat protokolování diagnostiky, do protokolu událostí, které jsou zjištěna nebo nebylo možné provést pomocí brány firewall webových aplikací navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
