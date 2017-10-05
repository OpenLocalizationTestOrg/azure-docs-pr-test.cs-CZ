---
title: "Azure Application Gateway pomocí nástroje pro vyrovnávání zatížení pro vnitřní | Microsoft Docs"
description: "Tato stránka obsahuje pokyny ke konfiguraci služby Azure Application Gateway s vyrovnáváním zatížení se vnitřní koncový bod"
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
ms.openlocfilehash: d6f3af61934c8c645be1f2c6b4c056fc7ee2e3aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a><span data-ttu-id="f38dd-103">Vytvoření brány Application Gateway s interním nástrojem Load Balancer (ILB)</span><span class="sxs-lookup"><span data-stu-id="f38dd-103">Create an Application Gateway with an Internal Load Balancer (ILB)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f38dd-104">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="f38dd-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="f38dd-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="f38dd-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="f38dd-106">Application Gateway se dá nakonfigurovat s internetovým virtuální IP adresy nebo se vnitřní koncový bod není přístup k Internetu, také známé jako koncový bod interní nástroj pro vyrovnávání zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="f38dd-106">Application Gateway can be configured with an internet facing virtual IP or with an internal end-point not exposed to the internet, also known as Internal Load Balancer (ILB) endpoint.</span></span> <span data-ttu-id="f38dd-107">Konfigurace brány pomocí ILB je užitečná pro interní-obchodní aplikace, které nejsou viditelné k Internetu.</span><span class="sxs-lookup"><span data-stu-id="f38dd-107">Configuring the gateway with an ILB is useful for internal line-of-business applications not exposed to internet.</span></span> <span data-ttu-id="f38dd-108">Je také užitečné pro/úrovně služeb v rámci vícevrstvé aplikace, která je umístěna v rámci hranice zabezpečení nejsou viditelné k Internetu, ale stále vyžadují distribuci zatížení kruhové dotazování, dlouhodobost relace nebo ukončení protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="f38dd-108">It's also useful for services/tiers within a multi-tier application, which sits in a security boundary not exposed to internet, but still require round robin load distribution, session stickiness, or SSL termination.</span></span> <span data-ttu-id="f38dd-109">Tenhle článek vás provede kroky konfigurace aplikační brány s ILB.</span><span class="sxs-lookup"><span data-stu-id="f38dd-109">This article walks you through the steps to configure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f38dd-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f38dd-110">Before you begin</span></span>

1. <span data-ttu-id="f38dd-111">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí služby instalace webové platformy.</span><span class="sxs-lookup"><span data-stu-id="f38dd-111">Install latest version of the Azure PowerShell cmdlets using the Web Platform Installer.</span></span> <span data-ttu-id="f38dd-112">Můžete stáhnout a nainstalovat nejnovější verzi **prostředí Windows PowerShell** části [stránky pro stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f38dd-112">You can download and install the latest version from the **Windows PowerShell** section of the [Download page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="f38dd-113">Ověřte, zda máte funkční virtuální síť s platnou podsítí.</span><span class="sxs-lookup"><span data-stu-id="f38dd-113">Verify that you have a working virtual network with valid subnet.</span></span>
3. <span data-ttu-id="f38dd-114">Ověřte, zda máte back-end serverů ve virtuální síti nebo s veřejné nebo virtuálními IP Adresami přiřazen.</span><span class="sxs-lookup"><span data-stu-id="f38dd-114">Verify that you have backend servers either in the virtual network, or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="f38dd-115">K vytvoření aplikační brány, proveďte následující kroky v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="f38dd-115">To create an application gateway, perform the following steps in the order listed.</span></span> 

1. [<span data-ttu-id="f38dd-116">Vytvoření služby application gateway</span><span class="sxs-lookup"><span data-stu-id="f38dd-116">Create an application gateway</span></span>](#create-a-new-application-gateway)
2. [<span data-ttu-id="f38dd-117">Konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="f38dd-117">Configure the gateway</span></span>](#configure-the-gateway)
3. [<span data-ttu-id="f38dd-118">Nastavení konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="f38dd-118">Set the gateway configuration</span></span>](#set-the-gateway-configuration)
4. [<span data-ttu-id="f38dd-119">Spusťte bránu</span><span class="sxs-lookup"><span data-stu-id="f38dd-119">Start the gateway</span></span>](#start-the-gateway)
5. [<span data-ttu-id="f38dd-120">Ověření brány</span><span class="sxs-lookup"><span data-stu-id="f38dd-120">Verify the gateway</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="f38dd-121">Vytvoření služby application gateway:</span><span class="sxs-lookup"><span data-stu-id="f38dd-121">Create an application gateway:</span></span>

<span data-ttu-id="f38dd-122">**Chcete-li vytvořit bránu**, použijte `New-AzureApplicationGateway` rutinu a nahraďte hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="f38dd-122">**To create the gateway**, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="f38dd-123">Všimněte si, že fakturace brány se nespustí v tomhle okamžiku.</span><span class="sxs-lookup"><span data-stu-id="f38dd-123">Note that billing for the gateway does not start at this point.</span></span> <span data-ttu-id="f38dd-124">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="f38dd-124">Billing begins in a later step, when the gateway is successfully started.</span></span>

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

<span data-ttu-id="f38dd-125">**K ověření** , vytvoření brány, můžete použít `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="f38dd-125">**To validate** that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span> 

<span data-ttu-id="f38dd-126">V ukázce *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="f38dd-126">In the sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="f38dd-127">Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="f38dd-127">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="f38dd-128">Výchozí hodnota *GatewaySize* je Medium (Střední).</span><span class="sxs-lookup"><span data-stu-id="f38dd-128">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="f38dd-129">Malých a velkých jsou ostatní dostupné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f38dd-129">Small and Large are other available values.</span></span> <span data-ttu-id="f38dd-130">*VIP* a *DnsName* se zobrazují jako prázdné, protože brána se ještě nespustilo.</span><span class="sxs-lookup"><span data-stu-id="f38dd-130">*Vip* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="f38dd-131">Vytvoří se, jakmile bude brána v běžícím stavu.</span><span class="sxs-lookup"><span data-stu-id="f38dd-131">These are created once the gateway is in the running state.</span></span> 

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

## <a name="configure-the-gateway"></a><span data-ttu-id="f38dd-132">Konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="f38dd-132">Configure the gateway</span></span>
<span data-ttu-id="f38dd-133">Konfigurace brány aplikace se skládá z více hodnot.</span><span class="sxs-lookup"><span data-stu-id="f38dd-133">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="f38dd-134">Může být vázáno hodnoty společně k vytvoření konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f38dd-134">The values can be tied together to construct the configuration.</span></span>

<span data-ttu-id="f38dd-135">Hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="f38dd-135">The values are:</span></span>

* <span data-ttu-id="f38dd-136">**Fond back-end serverů:** seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="f38dd-136">**Backend server pool:** The list of IP addresses of the backend servers.</span></span> <span data-ttu-id="f38dd-137">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo musí být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="f38dd-137">The IP addresses listed should either belong to the VNet subnet, or should be a public IP/VIP.</span></span> 
* <span data-ttu-id="f38dd-138">**Nastavení fondu serverů back-end:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="f38dd-138">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="f38dd-139">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="f38dd-139">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="f38dd-140">**Front-end Port:** Toto je veřejný port otevřít ve službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="f38dd-140">**Frontend Port:** This port is the public port opened on the application gateway.</span></span> <span data-ttu-id="f38dd-141">Když provoz dorazí na tento port, přesměruje se na některý ze serverů back-end.</span><span class="sxs-lookup"><span data-stu-id="f38dd-141">Traffic hits this port, and then gets redirected to one of the backend servers.</span></span>
* <span data-ttu-id="f38dd-142">**Naslouchací proces:** naslouchací proces má front-end port, protokol (Http nebo Https, ty jsou malá a velká písmena) a název certifikátu SSL (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="f38dd-142">**Listener:** The listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> 
* <span data-ttu-id="f38dd-143">**Pravidlo:** pravidlo váže naslouchací proces a fond back-end serverů a definuje, kterému fondu back-end serverů se provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="f38dd-143">**Rule:** The rule binds the listener and the backend server pool and defines which backend server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="f38dd-144">V tuhle chvíli se podporuje jenom *základní* pravidlo.</span><span class="sxs-lookup"><span data-stu-id="f38dd-144">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="f38dd-145">*Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="f38dd-145">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="f38dd-146">Konfiguraci můžete vytvořit buď tak, že vytvoříte objekt konfigurace, nebo pomocí konfiguračního souboru XML.</span><span class="sxs-lookup"><span data-stu-id="f38dd-146">You can construct your configuration either by creating a configuration object, or by using a configuration XML file.</span></span> <span data-ttu-id="f38dd-147">Chcete-li vytvořit konfiguraci pomocí konfiguračního souboru XML, použijte následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="f38dd-147">To construct your configuration by using a configuration XML file, use the sample below.</span></span>

<span data-ttu-id="f38dd-148">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="f38dd-148">Note the following:</span></span>

* <span data-ttu-id="f38dd-149">*FrontendIPConfigurations* element popisuje relevantní pro konfiguraci aplikační brány s ILB ILB podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f38dd-149">The *FrontendIPConfigurations* element describes the ILB details relevant for configuring Application Gateway with an ILB.</span></span> 
* <span data-ttu-id="f38dd-150">IP front-endu *typu* musí být nastavena na "Privátní"</span><span class="sxs-lookup"><span data-stu-id="f38dd-150">The Frontend IP *Type* should be set to 'Private'</span></span>
* <span data-ttu-id="f38dd-151">*StaticIPAddress* musí být nastavená na požadované interních IP, na kterém přijímá brána provoz.</span><span class="sxs-lookup"><span data-stu-id="f38dd-151">The *StaticIPAddress* should be set to the desired internal IP on which the gateway receives traffic.</span></span> <span data-ttu-id="f38dd-152">Všimněte si, že *StaticIPAddress* prvek je volitelný.</span><span class="sxs-lookup"><span data-stu-id="f38dd-152">Note that the *StaticIPAddress* element is optional.</span></span> <span data-ttu-id="f38dd-153">Pokud není sady, je zvolen k dispozici interní IP adresy z nasazené podsítě.</span><span class="sxs-lookup"><span data-stu-id="f38dd-153">If not set, an available internal IP from the deployed subnet is chosen.</span></span> 
* <span data-ttu-id="f38dd-154">Hodnota *název* zadaný v elementu *FrontendIPConfiguration* by měly být použity HTTPListener *FrontendIP* element, který bude odkazovat na FrontendIPConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f38dd-154">The value of the *Name* element specified in *FrontendIPConfiguration* should be used in the HTTPListener's *FrontendIP* element to refer to the FrontendIPConfiguration.</span></span>
  
  <span data-ttu-id="f38dd-155">**Ukázkový kód XML konfigurace**</span><span class="sxs-lookup"><span data-stu-id="f38dd-155">**Configuration XML sample**</span></span>
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


## <a name="set-the-gateway-configuration"></a><span data-ttu-id="f38dd-156">Nastavení konfigurace brány</span><span class="sxs-lookup"><span data-stu-id="f38dd-156">Set the gateway configuration</span></span>
<span data-ttu-id="f38dd-157">Budete dále nastavte aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="f38dd-157">Next, you'll set the application gateway.</span></span> <span data-ttu-id="f38dd-158">Můžete použít `Set-AzureApplicationGatewayConfig` rutiny objekt konfigurace, nebo s konfiguračním souborem XML.</span><span class="sxs-lookup"><span data-stu-id="f38dd-158">You can use the `Set-AzureApplicationGatewayConfig` cmdlet with a configuration object, or with a configuration XML file.</span></span> 

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

## <a name="start-the-gateway"></a><span data-ttu-id="f38dd-159">Spusťte bránu</span><span class="sxs-lookup"><span data-stu-id="f38dd-159">Start the gateway</span></span>

<span data-ttu-id="f38dd-160">Jakmile se brána nakonfiguruje, pomocí rutiny `Start-AzureApplicationGateway` ji spusťte.</span><span class="sxs-lookup"><span data-stu-id="f38dd-160">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="f38dd-161">Fakturace aplikační brány se spustí až po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="f38dd-161">Billing for an application gateway begins after the gateway has been successfully started.</span></span> 

> [!NOTE]
> <span data-ttu-id="f38dd-162">`Start-AzureApplicationGateway` Rutiny může trvat až 15-20 minut.</span><span class="sxs-lookup"><span data-stu-id="f38dd-162">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to complete.</span></span> 
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

## <a name="verify-the-gateway-status"></a><span data-ttu-id="f38dd-163">Ověřte stav brány.</span><span class="sxs-lookup"><span data-stu-id="f38dd-163">Verify the gateway status</span></span>

<span data-ttu-id="f38dd-164">Použití `Get-AzureApplicationGateway` rutiny a zkontrolujte stav brány.</span><span class="sxs-lookup"><span data-stu-id="f38dd-164">Use the `Get-AzureApplicationGateway` cmdlet to check the status of gateway.</span></span> <span data-ttu-id="f38dd-165">Pokud `Start-AzureApplicationGateway` byly úspěšné v předchozím kroku, musí být stav *systémem*, a virtuální IP adresy a DnsName musí obsahovat platné položky.</span><span class="sxs-lookup"><span data-stu-id="f38dd-165">If `Start-AzureApplicationGateway` succeeded in the previous step, the State should be *Running*, and the Vip and DnsName should have valid entries.</span></span> <span data-ttu-id="f38dd-166">Tento příklad ukazuje rutinu na prvním řádku, následovanou výstupem.</span><span class="sxs-lookup"><span data-stu-id="f38dd-166">This sample shows the cmdlet on the first line, followed by the output.</span></span> <span data-ttu-id="f38dd-167">V této ukázce brána je spuštěná a je připraven přijmout provoz.</span><span class="sxs-lookup"><span data-stu-id="f38dd-167">In this sample, the gateway is running, and is ready to take traffic.</span></span> 

> [!NOTE]
> <span data-ttu-id="f38dd-168">Application gateway je nakonfigurovat tak, aby přijímal provoz na nakonfigurovaný koncový bod ILB 10.0.0.10 v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="f38dd-168">The application gateway is configured to accept traffic at the configured ILB endpoint of 10.0.0.10 in this example.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f38dd-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f38dd-169">Next steps</span></span>
<span data-ttu-id="f38dd-170">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="f38dd-170">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="f38dd-171">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="f38dd-171">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="f38dd-172">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="f38dd-172">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

