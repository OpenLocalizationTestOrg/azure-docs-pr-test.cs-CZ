---
title: "aaaCreate klasický vlastní test paměti - Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate vlastní sběru dat pro službu Application Gateway pomocí prostředí PowerShell v modelu nasazení classic hello"
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
ms.openlocfilehash: 68332367c99328bd6456b0c339923765637be986
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a><span data-ttu-id="d0ce2-103">Vytvoření vlastní test paměti pro Azure Application Gateway (klasické pomocí prostředí PowerShell)</span><span class="sxs-lookup"><span data-stu-id="d0ce2-103">Create a custom probe for Azure Application Gateway (classic) by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d0ce2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d0ce2-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="d0ce2-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0ce2-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="d0ce2-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0ce2-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="d0ce2-107">V tomto článku můžete přidat vlastní test paměti tooan existující aplikace bránu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-107">In this article, you add a custom probe tooan existing application gateway with PowerShell.</span></span> <span data-ttu-id="d0ce2-108">Vlastní testy paměti jsou užitečné pro aplikace, které mají na stránce Kontrola konkrétní stav nebo pro aplikace, které neposkytuje úspěšné odpovědi na hello výchozí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on hello default web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0ce2-109">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d0ce2-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d0ce2-110">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-110">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d0ce2-111">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-111">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="d0ce2-112">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d0ce2-112">Learn how too[perform these steps using hello Resource Manager model](application-gateway-create-probe-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a><span data-ttu-id="d0ce2-113">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="d0ce2-113">Create an application gateway</span></span>

<span data-ttu-id="d0ce2-114">toocreate aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="d0ce2-114">toocreate an application gateway:</span></span>

1. <span data-ttu-id="d0ce2-115">Vytvořte prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-115">Create an application gateway resource.</span></span>
2. <span data-ttu-id="d0ce2-116">Vytvořte konfigurační soubor XML nebo objekt konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-116">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="d0ce2-117">Potvrďte toohello konfigurace hello nově vytvořeného prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-117">Commit hello configuration toohello newly created application gateway resource.</span></span>

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a><span data-ttu-id="d0ce2-118">Vytvořte prostředek aplikační brány s vlastní test paměti</span><span class="sxs-lookup"><span data-stu-id="d0ce2-118">Create an application gateway resource with a custom probe</span></span>

<span data-ttu-id="d0ce2-119">toocreate hello brány, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-119">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="d0ce2-120">Fakturace hello brány se nespustí v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-120">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="d0ce2-121">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-121">Billing begins in a later step, when hello gateway is successfully started.</span></span>

<span data-ttu-id="d0ce2-122">Hello následující příklad vytvoří aplikační bránu pomocí virtuální sítě s názvem "testvnet1" a podsítě s názvem "subnet-1".</span><span class="sxs-lookup"><span data-stu-id="d0ce2-122">hello following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1".</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="d0ce2-123">byl vytvořen toovalidate, který hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-123">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> <span data-ttu-id="d0ce2-124">Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-124">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="d0ce2-125">Výchozí hodnota pro Hello *GatewaySize* je střední.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-125">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="d0ce2-126">Můžete zvolit malá, střední a velké.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-126">You can choose between Small, Medium, and Large.</span></span>
> 
> 

<span data-ttu-id="d0ce2-127">*Rezervovaná* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-127">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="d0ce2-128">Tyto hodnoty se vytvoří, jakmile hello brána v běžícím stavu hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-128">These values are created once hello gateway is in hello running state.</span></span>

### <a name="configure-an-application-gateway-by-using-xml"></a><span data-ttu-id="d0ce2-129">Nakonfigurujte aplikační bránu pomocí XML</span><span class="sxs-lookup"><span data-stu-id="d0ce2-129">Configure an application gateway by using XML</span></span>

<span data-ttu-id="d0ce2-130">V následujícím příkladu hello použijte tooconfigure souboru XML všech nastavení aplikační brány a potvrdíte je toohello prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-130">In hello following example, you use an XML file tooconfigure all application gateway settings and commit them toohello application gateway resource.</span></span>  

<span data-ttu-id="d0ce2-131">Zkopírujte následující text tooNotepad hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-131">Copy hello following text tooNotepad.</span></span>

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

<span data-ttu-id="d0ce2-132">Upravte hodnoty hello mezi hello závorkách pro položky konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-132">Edit hello values between hello parentheses for hello configuration items.</span></span> <span data-ttu-id="d0ce2-133">Uložte hello soubor s příponou .xml.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-133">Save hello file with extension .xml.</span></span>

<span data-ttu-id="d0ce2-134">Hello následující příklad ukazuje, jak toouse konfigurační soubor tooset až tooload brány aplikace hello vyrovnávat přenosy HTTP na veřejném portu 80 a odesílat přenos v síti tooback-end port 80 mezi dvě IP adresy pomocí vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-134">hello following example shows how toouse a configuration file tooset up hello application gateway tooload balance HTTP traffic on public port 80 and send network traffic tooback-end port 80 between two IP addresses by using a custom probe.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0ce2-135">Položka Hello protokolu Http nebo Https rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-135">hello protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="d0ce2-136">Nová položka konfigurace \<testu\> je přidána tooconfigure vlastní testy paměti.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-136">A new configuration item \<Probe\> is added tooconfigure custom probes.</span></span>

<span data-ttu-id="d0ce2-137">parametry konfigurace Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="d0ce2-137">hello configuration parameters are:</span></span>

|<span data-ttu-id="d0ce2-138">Parametr</span><span class="sxs-lookup"><span data-stu-id="d0ce2-138">Parameter</span></span>|<span data-ttu-id="d0ce2-139">Popis</span><span class="sxs-lookup"><span data-stu-id="d0ce2-139">Description</span></span>|
|---|---|
|<span data-ttu-id="d0ce2-140">**Název**</span><span class="sxs-lookup"><span data-stu-id="d0ce2-140">**Name**</span></span> |<span data-ttu-id="d0ce2-141">Název odkazu pro vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-141">Reference name for custom probe.</span></span> |
<span data-ttu-id="d0ce2-142">* **Protokol**</span><span class="sxs-lookup"><span data-stu-id="d0ce2-142">* **Protocol**</span></span> | <span data-ttu-id="d0ce2-143">Protokol použitý (možné hodnoty jsou protokolu HTTP nebo HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d0ce2-143">Protocol used (possible values are HTTP or HTTPS).</span></span>|
| <span data-ttu-id="d0ce2-144">**Hostitele** a **cesta**</span><span class="sxs-lookup"><span data-stu-id="d0ce2-144">**Host** and **Path**</span></span> | <span data-ttu-id="d0ce2-145">Dokončete cestu adresy URL, který je vyvolán hello hello stavu toodetermine brány aplikace hello instance.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-145">Complete URL path that is invoked by hello application gateway toodetermine hello health of hello instance.</span></span> <span data-ttu-id="d0ce2-146">Například pokud máte na webu http://contoso.com/, pak lze nakonfigurovat vlastní test paměti hello "http://contoso.com/path/custompath.htm" pro test kontroluje toohave úspěšné odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-146">For example, if you have a website http://contoso.com/, then hello custom probe can be configured for "http://contoso.com/path/custompath.htm" for probe checks toohave a successful HTTP response.</span></span>|
| <span data-ttu-id="d0ce2-147">**Interval**</span><span class="sxs-lookup"><span data-stu-id="d0ce2-147">**Interval**</span></span> | <span data-ttu-id="d0ce2-148">Nakonfiguruje hello testu interval kontroly v sekundách.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-148">Configures hello probe interval checks in seconds.</span></span>|
| <span data-ttu-id="d0ce2-149">**Časový limit**</span><span class="sxs-lookup"><span data-stu-id="d0ce2-149">**Timeout**</span></span> | <span data-ttu-id="d0ce2-150">Definuje časový limit testu hello pro kontrolu odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-150">Defines hello probe time-out for an HTTP response check.</span></span>|
| <span data-ttu-id="d0ce2-151">**UnhealthyThreshold**</span><span class="sxs-lookup"><span data-stu-id="d0ce2-151">**UnhealthyThreshold**</span></span> | <span data-ttu-id="d0ce2-152">Hello počet neúspěšných odpovědí HTTP potřeby tooflag hello back-end instance jako *není v pořádku*.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-152">hello number of failed HTTP responses needed tooflag hello back-end instance as *unhealthy*.</span></span>|

<span data-ttu-id="d0ce2-153">Název sondy Hello se odkazuje v hello \<BackendHttpSettings\> tooassign konfigurace, které fond back-end používá nastavení vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-153">hello probe name is referenced in hello \<BackendHttpSettings\> configuration tooassign which back-end pool uses custom probe settings.</span></span>

## <a name="add-a-custom-probe-tooan-existing-application-gateway"></a><span data-ttu-id="d0ce2-154">Přidat existující aplikace bránu tooan vlastní test paměti</span><span class="sxs-lookup"><span data-stu-id="d0ce2-154">Add a custom probe tooan existing application gateway</span></span>

<span data-ttu-id="d0ce2-155">Změny hello aktuální konfigurace služby application gateway vyžaduje tři kroky: získání hello aktuální konfigurační soubor XML, úpravě toohave vlastní test paměti a konfigurovat hello aplikační bránu pomocí nového nastavení XML hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-155">Changing hello current configuration of an application gateway requires three steps: Get hello current XML configuration file, modify toohave a custom probe, and configure hello application gateway with hello new XML settings.</span></span>

1. <span data-ttu-id="d0ce2-156">Získat soubor XML hello pomocí `Get-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-156">Get hello XML file by using `Get-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="d0ce2-157">Tato rutina exportuje hello konfigurace XML toobe upravit tooadd nastavení testu.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-157">This cmdlet exports hello configuration XML toobe modified tooadd a probe setting.</span></span>

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path toofile>"
  ```

1. <span data-ttu-id="d0ce2-158">V textovém editoru otevřete soubor XML hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-158">Open hello XML file in a text editor.</span></span> <span data-ttu-id="d0ce2-159">Přidat `<probe>` části po `<frontendport>`.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-159">Add a `<probe>` section after `<frontendport>`.</span></span>

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

  <span data-ttu-id="d0ce2-160">V části backendHttpSettings hello hello XML přidejte název sondy hello, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="d0ce2-160">In hello backendHttpSettings section of hello XML, add hello probe name as shown in hello following example:</span></span>

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

  <span data-ttu-id="d0ce2-161">Uložte soubor XML hello.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-161">Save hello XML file.</span></span>

1. <span data-ttu-id="d0ce2-162">Konfigurace brány aplikace hello aktualizace s hello nový soubor XML s použitím `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-162">Update hello application gateway configuration with hello new XML file by using `Set-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="d0ce2-163">Tato rutina aktualizuje aplikační bránu pomocí hello novou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="d0ce2-163">This cmdlet updates your application gateway with hello new configuration.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path toofile>"
```

## <a name="next-steps"></a><span data-ttu-id="d0ce2-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0ce2-164">Next steps</span></span>

<span data-ttu-id="d0ce2-165">Pokud chcete tooconfigure přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="d0ce2-165">If you want tooconfigure Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="d0ce2-166">Pokud chcete tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, najdete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="d0ce2-166">If you want tooconfigure an application gateway toouse with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

