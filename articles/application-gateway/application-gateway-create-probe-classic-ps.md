---
title: "Vytvořit vlastní test paměti - Azure Application Gateway - klasický prostředí PowerShell | Microsoft Docs"
description: "Naučte se vytvářet vlastní test paměti pro službu Application Gateway pomocí prostředí PowerShell v modelu nasazení classic"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: bf190741b10c10e885d927ad21a9f2b25107943f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a><span data-ttu-id="2c573-103">Vytvoření vlastní test paměti pro Azure Application Gateway (klasické pomocí prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="2c573-103">Create a custom probe for Azure Application Gateway (classic) by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c573-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c573-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="2c573-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c573-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="2c573-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c573-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="2c573-107">V tomto článku přidejte vlastní test paměti existující application gateway pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c573-107">In this article, you add a custom probe to an existing application gateway with PowerShell.</span></span> <span data-ttu-id="2c573-108">Vlastní testy paměti jsou užitečné pro aplikace, které mají na stránce Kontrola konkrétní stav nebo pro aplikace, které neposkytuje úspěšné odpovědi na výchozí webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2c573-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on the default web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c573-109">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2c573-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="2c573-110">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="2c573-110">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="2c573-111">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2c573-111">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="2c573-112">Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2c573-112">Learn how to [perform these steps using the Resource Manager model](application-gateway-create-probe-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a><span data-ttu-id="2c573-113">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="2c573-113">Create an application gateway</span></span>

<span data-ttu-id="2c573-114">Pro vytvoření nové aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="2c573-114">To create an application gateway:</span></span>

1. <span data-ttu-id="2c573-115">Vytvořte prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2c573-115">Create an application gateway resource.</span></span>
2. <span data-ttu-id="2c573-116">Vytvořte konfigurační soubor XML nebo objekt konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2c573-116">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="2c573-117">Potvrďte konfiguraci nově vytvořeného prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2c573-117">Commit the configuration to the newly created application gateway resource.</span></span>

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a><span data-ttu-id="2c573-118">Vytvořte prostředek aplikační brány s vlastní test paměti</span><span class="sxs-lookup"><span data-stu-id="2c573-118">Create an application gateway resource with a custom probe</span></span>

<span data-ttu-id="2c573-119">Pokud chcete vytvořit bránu, použijte rutinu `New-AzureApplicationGateway` a zadejte vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2c573-119">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="2c573-120">Fakturace brány se nespustí v tomhle okamžiku.</span><span class="sxs-lookup"><span data-stu-id="2c573-120">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="2c573-121">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="2c573-121">Billing begins in a later step, when the gateway is successfully started.</span></span>

<span data-ttu-id="2c573-122">Následující příklad vytvoří aplikační bránu pomocí virtuální sítě s názvem „testvnet1“ a podsítě s názvem „subnet-1“.</span><span class="sxs-lookup"><span data-stu-id="2c573-122">The following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1".</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="2c573-123">Pokud chcete ověřit vytvoření brány, můžete použít rutinu `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="2c573-123">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> <span data-ttu-id="2c573-124">Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="2c573-124">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="2c573-125">Výchozí hodnota *GatewaySize* je Medium (Střední).</span><span class="sxs-lookup"><span data-stu-id="2c573-125">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="2c573-126">Můžete zvolit malá, střední a velké.</span><span class="sxs-lookup"><span data-stu-id="2c573-126">You can choose between Small, Medium, and Large.</span></span>
> 
> 

<span data-ttu-id="2c573-127">Hodnoty *VirtualIPs* a *DnsName* se zobrazují jako prázdné, protože se brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="2c573-127">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="2c573-128">Tyto hodnoty se vytvoří, jakmile je brána v běžícím stavu.</span><span class="sxs-lookup"><span data-stu-id="2c573-128">These values are created once the gateway is in the running state.</span></span>

### <a name="configure-an-application-gateway-by-using-xml"></a><span data-ttu-id="2c573-129">Nakonfigurujte aplikační bránu pomocí XML</span><span class="sxs-lookup"><span data-stu-id="2c573-129">Configure an application gateway by using XML</span></span>

<span data-ttu-id="2c573-130">V následujícím příkladu použijete soubor XML k nakonfigurování všech nastavení aplikační brány a potvrdíte je pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2c573-130">In the following example, you use an XML file to configure all application gateway settings and commit them to the application gateway resource.</span></span>  

<span data-ttu-id="2c573-131">Zkopírujte následující text do Poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="2c573-131">Copy the following text to Notepad.</span></span>

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

<span data-ttu-id="2c573-132">Upravte hodnoty položek konfigurace v závorkách.</span><span class="sxs-lookup"><span data-stu-id="2c573-132">Edit the values between the parentheses for the configuration items.</span></span> <span data-ttu-id="2c573-133">Uložte soubor s příponou .xml.</span><span class="sxs-lookup"><span data-stu-id="2c573-133">Save the file with extension .xml.</span></span>

<span data-ttu-id="2c573-134">Následující příklad ukazuje, jak pomocí konfiguračního souboru k nastavení aplikační brány, aby vyrovnávala zatížení provozu HTTP na veřejném portu 80 a odesílat přenos v síti na back-end port 80 mezi dvě IP adresy pomocí vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="2c573-134">The following example shows how to use a configuration file to set up the application gateway to load balance HTTP traffic on public port 80 and send network traffic to back-end port 80 between two IP addresses by using a custom probe.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c573-135">Položka protokolu Http nebo Https rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="2c573-135">The protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="2c573-136">Nová položka konfigurace \<testu\> se přidá do nakonfigurovat vlastní testy paměti.</span><span class="sxs-lookup"><span data-stu-id="2c573-136">A new configuration item \<Probe\> is added to configure custom probes.</span></span>

<span data-ttu-id="2c573-137">Parametry konfigurace jsou:</span><span class="sxs-lookup"><span data-stu-id="2c573-137">The configuration parameters are:</span></span>

|<span data-ttu-id="2c573-138">Parametr</span><span class="sxs-lookup"><span data-stu-id="2c573-138">Parameter</span></span>|<span data-ttu-id="2c573-139">Popis</span><span class="sxs-lookup"><span data-stu-id="2c573-139">Description</span></span>|
|---|---|
|<span data-ttu-id="2c573-140">**Název**</span><span class="sxs-lookup"><span data-stu-id="2c573-140">**Name**</span></span> |<span data-ttu-id="2c573-141">Název odkazu pro vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="2c573-141">Reference name for custom probe.</span></span> |
<span data-ttu-id="2c573-142">* **Protokol**</span><span class="sxs-lookup"><span data-stu-id="2c573-142">* **Protocol**</span></span> | <span data-ttu-id="2c573-143">Protokol použitý (možné hodnoty jsou protokolu HTTP nebo HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2c573-143">Protocol used (possible values are HTTP or HTTPS).</span></span>|
| <span data-ttu-id="2c573-144">**Hostitele** a **cesta**</span><span class="sxs-lookup"><span data-stu-id="2c573-144">**Host** and **Path**</span></span> | <span data-ttu-id="2c573-145">Dokončete cestu adresy URL, která je volána bránou aplikace k určení stavu instance.</span><span class="sxs-lookup"><span data-stu-id="2c573-145">Complete URL path that is invoked by the application gateway to determine the health of the instance.</span></span> <span data-ttu-id="2c573-146">Například pokud máte web http://contoso.com/, poté vlastní test paměti můžete nakonfigurovat pro "http://contoso.com/path/custompath.htm" pro test kontroly tak, aby měl úspěšné odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c573-146">For example, if you have a website http://contoso.com/, then the custom probe can be configured for "http://contoso.com/path/custompath.htm" for probe checks to have a successful HTTP response.</span></span>|
| <span data-ttu-id="2c573-147">**Interval**</span><span class="sxs-lookup"><span data-stu-id="2c573-147">**Interval**</span></span> | <span data-ttu-id="2c573-148">Nakonfiguruje interval kontroly testu v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2c573-148">Configures the probe interval checks in seconds.</span></span>|
| <span data-ttu-id="2c573-149">**Časový limit**</span><span class="sxs-lookup"><span data-stu-id="2c573-149">**Timeout**</span></span> | <span data-ttu-id="2c573-150">Definuje časový limit testu pro kontrolu odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c573-150">Defines the probe time-out for an HTTP response check.</span></span>|
| <span data-ttu-id="2c573-151">**UnhealthyThreshold**</span><span class="sxs-lookup"><span data-stu-id="2c573-151">**UnhealthyThreshold**</span></span> | <span data-ttu-id="2c573-152">Počet neúspěšných odpovědí HTTP, které jsou potřebné pro příznak instance back-end jako *není v pořádku*.</span><span class="sxs-lookup"><span data-stu-id="2c573-152">The number of failed HTTP responses needed to flag the back-end instance as *unhealthy*.</span></span>|

<span data-ttu-id="2c573-153">Název sondy odkazuje \<BackendHttpSettings\> konfigurace přiřadit kterému fondu back-end používá nastavení vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="2c573-153">The probe name is referenced in the \<BackendHttpSettings\> configuration to assign which back-end pool uses custom probe settings.</span></span>

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a><span data-ttu-id="2c573-154">Přidat vlastní test paměti existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="2c573-154">Add a custom probe to an existing application gateway</span></span>

<span data-ttu-id="2c573-155">Změna aktuální konfiguraci služby application gateway vyžaduje tři kroky: získání aktuálního konfiguračního souboru XML, upravit tak, aby měl vlastní test paměti a nakonfigurujte aplikační bránu s novým nastavením XML.</span><span class="sxs-lookup"><span data-stu-id="2c573-155">Changing the current configuration of an application gateway requires three steps: Get the current XML configuration file, modify to have a custom probe, and configure the application gateway with the new XML settings.</span></span>

1. <span data-ttu-id="2c573-156">Načíst soubor XML s použitím `Get-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="2c573-156">Get the XML file by using `Get-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="2c573-157">Tato rutina exportuje konfiguraci XML tak, aby upravit tak, aby přidat nastavení testu.</span><span class="sxs-lookup"><span data-stu-id="2c573-157">This cmdlet exports the configuration XML to be modified to add a probe setting.</span></span>

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
  ```

1. <span data-ttu-id="2c573-158">V textovém editoru otevřete soubor XML.</span><span class="sxs-lookup"><span data-stu-id="2c573-158">Open the XML file in a text editor.</span></span> <span data-ttu-id="2c573-159">Přidat `<probe>` části po `<frontendport>`.</span><span class="sxs-lookup"><span data-stu-id="2c573-159">Add a `<probe>` section after `<frontendport>`.</span></span>

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  <span data-ttu-id="2c573-160">V oddílu backendHttpSettings XML přidejte název sondy, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2c573-160">In the backendHttpSettings section of the XML, add the probe name as shown in the following example:</span></span>

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  <span data-ttu-id="2c573-161">Uložte soubor XML.</span><span class="sxs-lookup"><span data-stu-id="2c573-161">Save the XML file.</span></span>

1. <span data-ttu-id="2c573-162">Aktualizace konfigurace brány aplikace pomocí nového souboru XML s použitím `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="2c573-162">Update the application gateway configuration with the new XML file by using `Set-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="2c573-163">Tato rutina aktualizuje aplikační bránu s novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2c573-163">This cmdlet updates your application gateway with the new configuration.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a><span data-ttu-id="2c573-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c573-164">Next steps</span></span>

<span data-ttu-id="2c573-165">Pokud chcete konfigurovat přesměrování zpracování Secure Sockets Layer (SSL), přečtěte si téma [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2c573-165">If you want to configure Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="2c573-166">Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="2c573-166">If you want to configure an application gateway to use with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

