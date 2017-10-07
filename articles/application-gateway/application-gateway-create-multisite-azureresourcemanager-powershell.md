---
title: "aaaCreate aplikační brány pro hostování více webů | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfigurace služby Azure application gateway pro hostování několika webových aplikací na hello stejné bráně."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: bad9a76be0a73a7026a770630fa7156f6e5940c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Vytvoření služby application gateway pro hostování několika webových aplikací

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Hostování více lokality můžete toodeploy více než jednu webovou aplikaci na hello stejné aplikační brány. Přitom spoléhá na přítomnost Hlavička hostitele v hello příchozí požadavek HTTP, který naslouchací proces by přijímat přenosy toodetermine. naslouchací proces Hello potom směrovat přenosy tooappropriate back-end fondu podle konfigurace v definici pravidla hello hello brány. V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na hello indikace názvu serveru (SNI) rozšíření toochoose hello správné naslouchací proces pro hello webový provoz. Běžně používá pro více hostování lokality je tooload vyrovnávat požadavky pro fondy jiných webových domén toodifferent back-end serverů. Podobně jako více subdomény hello stejné kořenové domény může být také hostovaná v hello stejné aplikační brány.

## <a name="scenario"></a>Scénář

V následujícím příkladu hello, aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam. Podobně jako instalační program může být subdomény používané toohost jako app.contoso.com a blog.contoso.com.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. přidány servery Hello toohello fond back-end toouse hello Aplikační brána musí existovat nebo musí mít své koncové body vytvořené, buď ve virtuální síti hello v jiné podsíti, nebo s veřejné nebo virtuálními IP Adresami přiřazené.

## <a name="requirements"></a>Požadavky

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP. Můžete také použít plně kvalifikovaný název domény.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL). Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.
* **Pravidlo:** hello pravidlo váže naslouchací proces hello, hello fondu back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu. Pravidla se zpracovávají v pořadí hello, které jsou uvedeny a provoz se přesměruje prostřednictvím hello první pravidlo, které odpovídá bez ohledu na specifické podobě. Například pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces i na stejný port, hello pravidlo s hello naslouchací proces hello více lokalit musí být uvedený před hello pravidlo základní naslouchací proces hello-li pravidlo více lokalit toofunction hello jako byl očekáván.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Následující Hello se, že kroky hello toocreate aplikační brány:

1. Vytvoření skupiny prostředků pro Resource Manager
2. Vytvoření virtuální sítě, podsítě a veřejné IP adresy pro hello aplikační brány.
3. Vytvoření objektu konfigurace služby Application Gateway
4. Vytvoření prostředku služby Application Gateway

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny prostředků pro Resource Manager

Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello. Další informace najdete v [pomocí prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se tooAzure

```powershell
Login-AzureRmAccount
```
Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná hello pro účet hello.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Krok 3

Zvolte, které vaše toouse předplatných Azure.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Krok 4

Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

Případně můžete vytvořit také značky pro skupinu prostředků pro službu application gateway:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. Toto umístění se používá jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.

V předchozím příkladu hello, jsme vytvořili skupinu prostředků s názvem **appgw-RG** s umístění ve **západní USA**.

> [!NOTE]
> Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Navštivte [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md) Další informace.

## <a name="create-a-virtual-network-and-subnets"></a>Vytvoření virtuální sítě a podsítě

Následující příklad ukazuje, jak Hello toocreate virtuální síť pomocí Resource Manageru. Dvě podsítě jsou vytvořené v tomto kroku. první podsíť Hello je pro službu application gateway hello sám sebe. Aplikační brána vyžaduje vlastní podsíť toohold její instance. Pouze jiné službu application Gateway se dá nasadit v této podsíti. druhou podsíť Hello je použité toohold hello aplikace back-end serverů.

### <a name="step-1"></a>Krok 1

Přiřaďte hello adresa rozsahu 10.0.0.0/24 toohello podsíť proměnné toobe použité toohold hello aplikační brány.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>Krok 2

Přiřaďte hello adresa rozsahu 10.0.1.0/24 toohello Podsíť2 proměnné toobe používá pro hello back-endové fondy.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Krok 3

Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **appgw-rg** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>Krok 4

Přiřaďte proměnnou podsítě pro hello další kroky, které se vytvoří aplikační brána.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Vytvoření veřejné IP adresy pro front-end konfiguraci hello

Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **appgw-rg** pro oblast západní USA hello.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.

## <a name="create-application-gateway-configuration"></a>Vytvoření konfigurace brány aplikace

Před vytvořením brány aplikace hello máte tooset až všechny položky konfigurace. Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.

### <a name="step-1"></a>Krok 1

Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**. Při spuštění služby application gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres. Uvědomte si, že každá instance vyžaduje jednu IP adresu.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>Krok 2

Konfigurace služby fond hello back-end IP adresy s názvem **pool01** a **pool2** s IP adresami **134.170.185.46**, **134.170.188.221**, **134.170.185.50** pro **pool1** a **134.170.186.46**, **134.170.189.221**, **134.170.186.50**  pro **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

V tomto příkladu jsou dva back endové fondy tooroute síťového provozu založené na požadovaný server hello. Jeden fond přijímá provoz z lokality "contoso.com" a dalších fondu přijímá provoz z lokality "fabrikam.com". Máte tooreplace hello předcházející IP adresy tooadd koncovými body IP adresy vlastní aplikace. Místo interní IP adresy můžete pro back-end instance také použít veřejné IP adresy, plně kvalifikovaný název domény nebo síťový adaptér Virtuálního počítače. toospecify plně kvalifikované názvy domény místo IP adresy v prostředí PowerShell pomocí "-BackendFQDNs" parametr.

### <a name="step-3"></a>Krok 3

Konfigurace nastavení brány aplikace **poolsetting01** a **poolsetting02** hello Vyrovnávání zatížení síťových přenosů ve fondu back-end hello. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro hello back endové fondy. Každý fond back-end může mít vlastní nastavení fondu back-end.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Krok 4

Nakonfigurujte veřejný koncový bod IP hello front-end IP adresu.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Krok 5

Nakonfigurujte port front-end hello aplikační brány.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>Krok 6

Konfigurovat dva certifikáty SSL pro hello dva weby přidáme toosupport v tomto příkladu. Jeden certifikát je pro provoz contoso.com a hello jiné pro provoz fabrikam.com. Tyto certifikáty musí být z certifikační autority, která vydala certifikáty pro vaše weby. Certifikáty podepsané svým držitelem podporuje, ale nedoporučuje pro produkční provoz.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>Krok 7

V tomto příkladu nakonfigurujte dva naslouchací procesy pro hello dva webové servery. Tento krok nakonfiguruje hello naslouchací procesy pro veřejné IP adresy, portu a hostitele použít tooreceive příchozí přenosy. Parametr název hostitele je vyžadovaná pro podporu více lokality a musí být sada toohello odpovídající web, pro které hello příjem přenosů. Parametr RequireServerNameIndication je třeba nastavit tootrue pro weby, které potřebují podporu pro protokol SSL ve scénáři více hostitelů. Pokud se vyžaduje podpora protokolu SSL, musíte taky toospecify hello SSL certifikát to znamená provoz toosecure použité pro tuto webovou aplikaci. kombinace Hello FrontendIPConfiguration, FrontendPort a název hostitele musí být jedinečný tooa naslouchací proces. Každý naslouchací proces může podporovat jeden certifikát.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>Krok 8

Vytvořte dvě pravidla nastavení pro hello dva webové aplikace v tomto příkladu. Pravidlo sváže společně naslouchací procesy, back-endové fondy a nastavení protokolu http. Tento krok nakonfiguruje hello aplikace brány toouse základní pravidlo směrování, jednu pro každý web. Provoz webu tooeach přijme jeho nakonfigurované naslouchací proces a je pak přesměrované tooits nakonfigurovat fond back-end, pomocí zadané v hello BackendHttpSettings vlastnosti hello.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>Krok 9

Nakonfigurujte hello počet instancí a velikost hello aplikační brány.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

Vytvoření služby application gateway se všemi objekty konfigurace z předchozích kroků hello.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Zřizování brány aplikace je dlouhotrvající operace a některé toocomplete dobu trvat.
> 
> 

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

Zjistěte, jak tooprotect své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)

