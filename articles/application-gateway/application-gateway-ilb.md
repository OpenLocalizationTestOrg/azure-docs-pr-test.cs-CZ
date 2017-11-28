---
title: "aaaUsing Azure Application Gateway s interním nástrojem pro vyrovnávání zatížení | Microsoft Docs"
description: "Tato stránka obsahuje pokyny tooconfigure služby Azure Application Gateway s vyrovnáváním zatížení se vnitřní koncový bod"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 272ef84a02f92a8521c35aad6f1d9f9bf1675718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a><span data-ttu-id="24c96-103">Vytvoření brány Application Gateway s interním nástrojem Load Balancer (ILB)</span><span class="sxs-lookup"><span data-stu-id="24c96-103">Create an Application Gateway with an Internal Load Balancer (ILB)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="24c96-104">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="24c96-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="24c96-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="24c96-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="24c96-106">Application Gateway se dá nakonfigurovat s internetovým virtuální IP adresy nebo se toohello vnitřní koncový bod, nejsou viditelné internet, taky označovaný jako koncový bod interní nástroj pro vyrovnávání zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="24c96-106">Application Gateway can be configured with an internet facing virtual IP or with an internal end-point not exposed toohello internet, also known as Internal Load Balancer (ILB) endpoint.</span></span> <span data-ttu-id="24c96-107">Konfigurace hello brány s ILB je užitečná pro interní-obchodní aplikace, které nejsou viditelné toointernet.</span><span class="sxs-lookup"><span data-stu-id="24c96-107">Configuring hello gateway with an ILB is useful for internal line-of-business applications not exposed toointernet.</span></span> <span data-ttu-id="24c96-108">Je také užitečné pro/úrovně služeb v rámci vícevrstvé aplikace, který je spuštěn toointernet hranice nejsou viditelné zabezpečení, ale stále vyžadují distribuci zatížení kruhové dotazování, dlouhodobost relace nebo ukončení protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="24c96-108">It's also useful for services/tiers within a multi-tier application, which sits in a security boundary not exposed toointernet, but still require round robin load distribution, session stickiness, or SSL termination.</span></span> <span data-ttu-id="24c96-109">Tento článek vás provede kroky tooconfigure hello aplikační brány s ILB.</span><span class="sxs-lookup"><span data-stu-id="24c96-109">This article walks you through hello steps tooconfigure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="24c96-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="24c96-110">Before you begin</span></span>

1. <span data-ttu-id="24c96-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="24c96-111">Install latest version of hello Azure PowerShell cmdlets using hello Web Platform Installer.</span></span> <span data-ttu-id="24c96-112">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [stránky pro stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="24c96-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Download page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="24c96-113">Ověřte, zda máte funkční virtuální síť s platnou podsítí.</span><span class="sxs-lookup"><span data-stu-id="24c96-113">Verify that you have a working virtual network with valid subnet.</span></span>
3. <span data-ttu-id="24c96-114">Ověřte, zda máte back-end serverů ve virtuální síti hello nebo s veřejné nebo virtuálními IP Adresami přiřazen.</span><span class="sxs-lookup"><span data-stu-id="24c96-114">Verify that you have backend servers either in hello virtual network, or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="24c96-115">toocreate aplikační brány, proveďte následující kroky v uvedeném pořadí hello hello.</span><span class="sxs-lookup"><span data-stu-id="24c96-115">toocreate an application gateway, perform hello following steps in hello order listed.</span></span> 

1. [<span data-ttu-id="24c96-116">Vytvoření služby application gateway</span><span class="sxs-lookup"><span data-stu-id="24c96-116">Create an application gateway</span></span>](#create-a-new-application-gateway)
2. [<span data-ttu-id="24c96-117">Konfigurace brány hello</span><span class="sxs-lookup"><span data-stu-id="24c96-117">Configure hello gateway</span></span>](#configure-the-gateway)
3. [<span data-ttu-id="24c96-118">Konfigurace brány hello sady</span><span class="sxs-lookup"><span data-stu-id="24c96-118">Set hello gateway configuration</span></span>](#set-the-gateway-configuration)
4. [<span data-ttu-id="24c96-119">Spusťte bránu hello</span><span class="sxs-lookup"><span data-stu-id="24c96-119">Start hello gateway</span></span>](#start-the-gateway)
5. [<span data-ttu-id="24c96-120">Ověření hello brány</span><span class="sxs-lookup"><span data-stu-id="24c96-120">Verify hello gateway</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="24c96-121">Vytvoření služby application gateway:</span><span class="sxs-lookup"><span data-stu-id="24c96-121">Create an application gateway:</span></span>

<span data-ttu-id="24c96-122">**Brána hello toocreate**, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="24c96-122">**toocreate hello gateway**, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="24c96-123">Všimněte si, že fakturace brány hello se nespustí v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="24c96-123">Note that billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="24c96-124">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="24c96-124">Billing begins in a later step, when hello gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

<span data-ttu-id="24c96-125">**toovalidate** , byl vytvořen hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="24c96-125">**toovalidate** that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span> 

<span data-ttu-id="24c96-126">V ukázce hello *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="24c96-126">In hello sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="24c96-127">Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="24c96-127">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="24c96-128">Výchozí hodnota pro Hello *GatewaySize* je střední.</span><span class="sxs-lookup"><span data-stu-id="24c96-128">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="24c96-129">Malých a velkých jsou ostatní dostupné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="24c96-129">Small and Large are other available values.</span></span> <span data-ttu-id="24c96-130">*VIP* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="24c96-130">*Vip* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="24c96-131">Tyto soubory jsou vytvořeny po hello brána je v běžícím stavu hello.</span><span class="sxs-lookup"><span data-stu-id="24c96-131">These are created once hello gateway is in hello running state.</span></span> 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-hello-gateway"></a><span data-ttu-id="24c96-132">Konfigurace brány hello</span><span class="sxs-lookup"><span data-stu-id="24c96-132">Configure hello gateway</span></span>
<span data-ttu-id="24c96-133">Konfigurace brány aplikace se skládá z více hodnot.</span><span class="sxs-lookup"><span data-stu-id="24c96-133">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="24c96-134">může být vázáno Hello hodnoty společně tooconstruct hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="24c96-134">hello values can be tied together tooconstruct hello configuration.</span></span>

<span data-ttu-id="24c96-135">Hello hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="24c96-135">hello values are:</span></span>

* <span data-ttu-id="24c96-136">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="24c96-136">**Backend server pool:** hello list of IP addresses of hello backend servers.</span></span> <span data-ttu-id="24c96-137">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě, nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="24c96-137">hello IP addresses listed should either belong toohello VNet subnet, or should be a public IP/VIP.</span></span> 
* <span data-ttu-id="24c96-138">**Nastavení fondu serverů back-end:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="24c96-138">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="24c96-139">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="24c96-139">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="24c96-140">**Front-end Port:** Toto je veřejný port hello otevírá ve hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="24c96-140">**Frontend Port:** This port is hello public port opened on hello application gateway.</span></span> <span data-ttu-id="24c96-141">Provoz volá Tenhle port a pak získá přesměrovaného tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="24c96-141">Traffic hits this port, and then gets redirected tooone of hello backend servers.</span></span>
* <span data-ttu-id="24c96-142">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, ty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="24c96-142">**Listener:** hello listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> 
* <span data-ttu-id="24c96-143">**Pravidlo:** hello pravidlo váže naslouchací proces hello a fondu hello back-end serverů a definuje, jaký provoz back-end serveru fondu hello by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="24c96-143">**Rule:** hello rule binds hello listener and hello backend server pool and defines which backend server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="24c96-144">V současné době pouze hello *základní* pravidel je podporována.</span><span class="sxs-lookup"><span data-stu-id="24c96-144">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="24c96-145">Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="24c96-145">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="24c96-146">Konfiguraci můžete vytvořit buď tak, že vytvoříte objekt konfigurace, nebo pomocí konfiguračního souboru XML.</span><span class="sxs-lookup"><span data-stu-id="24c96-146">You can construct your configuration either by creating a configuration object, or by using a configuration XML file.</span></span> <span data-ttu-id="24c96-147">tooconstruct ukázkové konfiguraci pomocí konfiguračního souboru XML, použijte hello níže.</span><span class="sxs-lookup"><span data-stu-id="24c96-147">tooconstruct your configuration by using a configuration XML file, use hello sample below.</span></span>

<span data-ttu-id="24c96-148">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="24c96-148">Note hello following:</span></span>

* <span data-ttu-id="24c96-149">Hello *FrontendIPConfigurations* element popisuje podrobnosti ILB hello relevantní pro konfiguraci aplikační brány s ILB.</span><span class="sxs-lookup"><span data-stu-id="24c96-149">hello *FrontendIPConfigurations* element describes hello ILB details relevant for configuring Application Gateway with an ILB.</span></span> 
* <span data-ttu-id="24c96-150">IP front-endu Hello *typ* by mělo být nastavené too'Private.</span><span class="sxs-lookup"><span data-stu-id="24c96-150">hello Frontend IP *Type* should be set too'Private'</span></span>
* <span data-ttu-id="24c96-151">Hello *StaticIPAddress* by mělo být nastavené toohello potřeby interních IP, na které hello brány přijímá provoz.</span><span class="sxs-lookup"><span data-stu-id="24c96-151">hello *StaticIPAddress* should be set toohello desired internal IP on which hello gateway receives traffic.</span></span> <span data-ttu-id="24c96-152">Všimněte si, že hello *StaticIPAddress* prvek je volitelný.</span><span class="sxs-lookup"><span data-stu-id="24c96-152">Note that hello *StaticIPAddress* element is optional.</span></span> <span data-ttu-id="24c96-153">Pokud není sady, na k dispozici interní IP adresu z podsítě hello nasazení je vybrán.</span><span class="sxs-lookup"><span data-stu-id="24c96-153">If not set, an available internal IP from hello deployed subnet is chosen.</span></span> 
* <span data-ttu-id="24c96-154">Hello hodnotu hello *název* zadaný v elementu *FrontendIPConfiguration* by měly být použity hello HTTPListener na *FrontendIP* toohello toorefer – element FrontendIPConfiguration.</span><span class="sxs-lookup"><span data-stu-id="24c96-154">hello value of hello *Name* element specified in *FrontendIPConfiguration* should be used in hello HTTPListener's *FrontendIP* element toorefer toohello FrontendIPConfiguration.</span></span>
  
  <span data-ttu-id="24c96-155">**Ukázkový kód XML konfigurace**</span><span class="sxs-lookup"><span data-stu-id="24c96-155">**Configuration XML sample**</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendIP>fip1</FrontendIP>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```


## <a name="set-hello-gateway-configuration"></a><span data-ttu-id="24c96-156">Konfigurace brány hello sady</span><span class="sxs-lookup"><span data-stu-id="24c96-156">Set hello gateway configuration</span></span>
<span data-ttu-id="24c96-157">Dále budete nastavte hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="24c96-157">Next, you'll set hello application gateway.</span></span> <span data-ttu-id="24c96-158">Můžete použít hello `Set-AzureApplicationGatewayConfig` rutiny objekt konfigurace, nebo s konfiguračním souborem XML.</span><span class="sxs-lookup"><span data-stu-id="24c96-158">You can use hello `Set-AzureApplicationGatewayConfig` cmdlet with a configuration object, or with a configuration XML file.</span></span> 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-hello-gateway"></a><span data-ttu-id="24c96-159">Spusťte bránu hello</span><span class="sxs-lookup"><span data-stu-id="24c96-159">Start hello gateway</span></span>

<span data-ttu-id="24c96-160">Jakmile se nakonfiguruje brána hello, použijte hello `Start-AzureApplicationGateway` rutiny toostart hello brány.</span><span class="sxs-lookup"><span data-stu-id="24c96-160">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="24c96-161">Fakturace aplikační brány se spustí po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="24c96-161">Billing for an application gateway begins after hello gateway has been successfully started.</span></span> 

> [!NOTE]
> <span data-ttu-id="24c96-162">Hello `Start-AzureApplicationGateway` rutiny může trvat až toocomplete too15-20 minut.</span><span class="sxs-lookup"><span data-stu-id="24c96-162">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toocomplete.</span></span> 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="24c96-163">Ověření stavu brány hello</span><span class="sxs-lookup"><span data-stu-id="24c96-163">Verify hello gateway status</span></span>

<span data-ttu-id="24c96-164">Použití hello `Get-AzureApplicationGateway` rutiny toocheck hello stav brány.</span><span class="sxs-lookup"><span data-stu-id="24c96-164">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of gateway.</span></span> <span data-ttu-id="24c96-165">Pokud `Start-AzureApplicationGateway` byly úspěšné v předchozím kroku hello, by měla být hello stavu *systémem*, hello Vip a DnsName musí obsahovat platné položky.</span><span class="sxs-lookup"><span data-stu-id="24c96-165">If `Start-AzureApplicationGateway` succeeded in hello previous step, hello State should be *Running*, and hello Vip and DnsName should have valid entries.</span></span> <span data-ttu-id="24c96-166">Tento příklad ukazuje rutinu hello na prvním řádku hello, následuje výstup hello.</span><span class="sxs-lookup"><span data-stu-id="24c96-166">This sample shows hello cmdlet on hello first line, followed by hello output.</span></span> <span data-ttu-id="24c96-167">V této ukázce hello brány je spuštěná a připravená tootake přenosy.</span><span class="sxs-lookup"><span data-stu-id="24c96-167">In this sample, hello gateway is running, and is ready tootake traffic.</span></span> 

> [!NOTE]
> <span data-ttu-id="24c96-168">Hello aplikace brána je nakonfigurovaná tooaccept provoz na hello nakonfigurovaný koncový bod ILB 10.0.0.10 v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="24c96-168">hello application gateway is configured tooaccept traffic at hello configured ILB endpoint of 10.0.0.10 in this example.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   : 
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="24c96-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24c96-169">Next steps</span></span>
<span data-ttu-id="24c96-170">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="24c96-170">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="24c96-171">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="24c96-171">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="24c96-172">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="24c96-172">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

