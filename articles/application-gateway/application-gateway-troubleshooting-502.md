---
title: "chyby Azure Application Gateway chybný brány (502) aaaTroubleshoot | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot aplikace brány 502 chyby"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: a50f736ac157256a53f6fbe3a208f0c11dd58f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a><span data-ttu-id="88ac9-103">Řešení potíží s chybami Chybná brána v aplikační brány</span><span class="sxs-lookup"><span data-stu-id="88ac9-103">Troubleshooting bad gateway errors in Application Gateway</span></span>

<span data-ttu-id="88ac9-104">Zjistěte, jak tootroubleshoot Chybná brána (502) chyby přijaté při použití aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="88ac9-104">Learn how tootroubleshoot bad gateway (502) errors received when using application gateway.</span></span>

## <a name="overview"></a><span data-ttu-id="88ac9-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="88ac9-105">Overview</span></span>

<span data-ttu-id="88ac9-106">Po dokončení konfigurace aplikační brány, jeden hello chyb, které uživatelé setkat je "Chyba serveru: 502 – Webový server obdržel neplatnou odpověď době, kdy fungoval jako brána nebo proxy server".</span><span class="sxs-lookup"><span data-stu-id="88ac9-106">After configuring an application gateway, one of hello errors that users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server".</span></span> <span data-ttu-id="88ac9-107">K této chybě může dojít z důvodu následující toohello hlavních důvodů:</span><span class="sxs-lookup"><span data-stu-id="88ac9-107">This error may happen due toohello following main reasons:</span></span>

* <span data-ttu-id="88ac9-108">Služba Azure Application Gateway [fond back-end není nakonfigurován nebo prázdný](#empty-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="88ac9-108">Azure Application Gateway's [back-end pool is not configured or empty](#empty-backendaddresspool).</span></span>
* <span data-ttu-id="88ac9-109">Žádný z virtuálních počítačů hello nebo instancí v [Škálovací sady virtuálního počítače jsou v pořádku](#unhealthy-instances-in-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="88ac9-109">None of hello VMs or instances in [VM Scale Set are healthy](#unhealthy-instances-in-backendaddresspool).</span></span>
* <span data-ttu-id="88ac9-110">Virtuální počítače nebo instance Škálovací sady virtuálního počítače back-end jsou [neodpovídá toohello výchozí stav kontroly](#problems-with-default-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="88ac9-110">Back-end VMs or instances of VM Scale Set are [not responding toohello default health probe](#problems-with-default-health-probe.md).</span></span>
* <span data-ttu-id="88ac9-111">Neplatný nebo nesprávný [konfigurace sondy vlastní stavu](#problems-with-custom-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="88ac9-111">Invalid or improper [configuration of custom health probes](#problems-with-custom-health-probe.md).</span></span>
* <span data-ttu-id="88ac9-112">[Žádosti o vypršení časového limitu nebo problémy s připojením k](#request-time-out) s požadavky uživatelů.</span><span class="sxs-lookup"><span data-stu-id="88ac9-112">[Request time out or connectivity issues](#request-time-out) with user requests.</span></span>

## <a name="empty-backendaddresspool"></a><span data-ttu-id="88ac9-113">Prázdný BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="88ac9-113">Empty BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="88ac9-114">Příčina</span><span class="sxs-lookup"><span data-stu-id="88ac9-114">Cause</span></span>

<span data-ttu-id="88ac9-115">Pokud hello Application Gateway žádné virtuální počítače nebo Škálovací sady virtuálního počítače ve fondu adres back-end hello nakonfigurován, nemůžete provádět směrování každá zákazníka žádost a vyvolá chybu Chybná brána.</span><span class="sxs-lookup"><span data-stu-id="88ac9-115">If hello Application Gateway has no VMs or VM Scale Set configured in hello back-end address pool, it cannot route any customer request and throws a bad gateway error.</span></span>

### <a name="solution"></a><span data-ttu-id="88ac9-116">Řešení</span><span class="sxs-lookup"><span data-stu-id="88ac9-116">Solution</span></span>

<span data-ttu-id="88ac9-117">Ujistěte se, že fond back-end adres hello není prázdná.</span><span class="sxs-lookup"><span data-stu-id="88ac9-117">Ensure that hello back-end address pool is not empty.</span></span> <span data-ttu-id="88ac9-118">To lze provést prostřednictvím prostředí PowerShell, rozhraní příkazového řádku nebo portálu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-118">This can be done either via PowerShell, CLI, or portal.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

<span data-ttu-id="88ac9-119">výstup Hello hello předcházející rutinu by měl obsahovat fond adres back-end není prázdný.</span><span class="sxs-lookup"><span data-stu-id="88ac9-119">hello output from hello preceding cmdlet should contain non-empty back-end address pool.</span></span> <span data-ttu-id="88ac9-120">Tady je příklad, kde, dvěma fondy jsou vráceny, které jsou nakonfigurované plně kvalifikovaný název domény nebo IP adresy pro back-end virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88ac9-120">Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs.</span></span> <span data-ttu-id="88ac9-121">Hello stav hello BackendAddressPool zřizování musí být 'bylo úspěšné".</span><span class="sxs-lookup"><span data-stu-id="88ac9-121">hello provisioning state of hello BackendAddressPool must be 'Succeeded'.</span></span>

<span data-ttu-id="88ac9-122">BackendAddressPoolsText:</span><span class="sxs-lookup"><span data-stu-id="88ac9-122">BackendAddressPoolsText:</span></span>

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a><span data-ttu-id="88ac9-123">Není v pořádku instance v BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="88ac9-123">Unhealthy instances in BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="88ac9-124">Příčina</span><span class="sxs-lookup"><span data-stu-id="88ac9-124">Cause</span></span>

<span data-ttu-id="88ac9-125">Pokud jsou všechny instance hello BackendAddressPool není v pořádku, aplikační brána nebude mít žádné back-end tooroute požadavku uživatele na.</span><span class="sxs-lookup"><span data-stu-id="88ac9-125">If all hello instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end tooroute user request to.</span></span> <span data-ttu-id="88ac9-126">Také to může být případ hello Pokud instance back-end jsou v pořádku, ale nemají hello požadované aplikace nasazena.</span><span class="sxs-lookup"><span data-stu-id="88ac9-126">This could also be hello case when back-end instances are healthy but do not have hello required application deployed.</span></span>

### <a name="solution"></a><span data-ttu-id="88ac9-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="88ac9-127">Solution</span></span>

<span data-ttu-id="88ac9-128">Zkontrolujte, zda jsou v pořádku hello instancí a hello aplikace správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="88ac9-128">Ensure that hello instances are healthy and hello application is properly configured.</span></span> <span data-ttu-id="88ac9-129">Kontrola, pokud instance back-end hello jsou možné toorespond tooa ping z jiného virtuálního počítače v hello stejnou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="88ac9-129">Check if hello back-end instances are able toorespond tooa ping from another VM in hello same VNet.</span></span> <span data-ttu-id="88ac9-130">Pokud nakonfigurované veřejný koncový bod, zajistěte, aby byl prohlížeče žádost toohello webovou aplikaci obsluhovatelná.</span><span class="sxs-lookup"><span data-stu-id="88ac9-130">If configured with a public end point, ensure that a browser request toohello web application is serviceable.</span></span>

## <a name="problems-with-default-health-probe"></a><span data-ttu-id="88ac9-131">Problémy s výchozí kontroly stavu</span><span class="sxs-lookup"><span data-stu-id="88ac9-131">Problems with default health probe</span></span>

### <a name="cause"></a><span data-ttu-id="88ac9-132">Příčina</span><span class="sxs-lookup"><span data-stu-id="88ac9-132">Cause</span></span>

<span data-ttu-id="88ac9-133">chyby 502 může být také časté indikátory, které hello výchozí kontroly stavu nebylo možné tooreach back-end virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88ac9-133">502 errors can also be frequent indicators that hello default health probe is not able tooreach back-end VMs.</span></span> <span data-ttu-id="88ac9-134">Při zřizování je instance aplikační brány, výchozí stav testu tooeach BackendAddressPool automaticky konfiguruje pomocí vlastnosti hello BackendHttpSetting.</span><span class="sxs-lookup"><span data-stu-id="88ac9-134">When an Application Gateway instance is provisioned, it automatically configures a default health probe tooeach BackendAddressPool using properties of hello BackendHttpSetting.</span></span> <span data-ttu-id="88ac9-135">Žádný uživatel vstup je požadovaná tooset tento test.</span><span class="sxs-lookup"><span data-stu-id="88ac9-135">No user input is required tooset this probe.</span></span> <span data-ttu-id="88ac9-136">Konkrétně Pokud je nakonfigurovaná pravidlo Vyrovnávání zatížení, se provádí přidružení mezi BackendHttpSetting a BackendAddressPool.</span><span class="sxs-lookup"><span data-stu-id="88ac9-136">Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool.</span></span> <span data-ttu-id="88ac9-137">Výchozí kontroly je nakonfigurován pro každý z těchto přidružení a spouští Application Gateway pravidelné stavu Kontrola připojení tooeach instance v hello BackendAddressPool v hello port zadaný v elementu BackendHttpSetting hello.</span><span class="sxs-lookup"><span data-stu-id="88ac9-137">A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection tooeach instance in hello BackendAddressPool at hello port specified in hello BackendHttpSetting element.</span></span> <span data-ttu-id="88ac9-138">Následující tabulka uvádí hello hodnoty přidružené k hello výchozí stav kontroly.</span><span class="sxs-lookup"><span data-stu-id="88ac9-138">Following table lists hello values associated with hello default health probe.</span></span>

| <span data-ttu-id="88ac9-139">Vlastnost testu</span><span class="sxs-lookup"><span data-stu-id="88ac9-139">Probe property</span></span> | <span data-ttu-id="88ac9-140">Hodnota</span><span class="sxs-lookup"><span data-stu-id="88ac9-140">Value</span></span> | <span data-ttu-id="88ac9-141">Popis</span><span class="sxs-lookup"><span data-stu-id="88ac9-141">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="88ac9-142">Adresa URL testu</span><span class="sxs-lookup"><span data-stu-id="88ac9-142">Probe URL</span></span> |<span data-ttu-id="88ac9-143">http://127.0.0.1/</span><span class="sxs-lookup"><span data-stu-id="88ac9-143">http://127.0.0.1/</span></span> |<span data-ttu-id="88ac9-144">Cesta adresy URL</span><span class="sxs-lookup"><span data-stu-id="88ac9-144">URL path</span></span> |
| <span data-ttu-id="88ac9-145">Interval</span><span class="sxs-lookup"><span data-stu-id="88ac9-145">Interval</span></span> |<span data-ttu-id="88ac9-146">30</span><span class="sxs-lookup"><span data-stu-id="88ac9-146">30</span></span> |<span data-ttu-id="88ac9-147">Interval testu paměti v sekundách</span><span class="sxs-lookup"><span data-stu-id="88ac9-147">Probe interval in seconds</span></span> |
| <span data-ttu-id="88ac9-148">Časový limit</span><span class="sxs-lookup"><span data-stu-id="88ac9-148">Time-out</span></span> |<span data-ttu-id="88ac9-149">30</span><span class="sxs-lookup"><span data-stu-id="88ac9-149">30</span></span> |<span data-ttu-id="88ac9-150">Časový limit testu v sekundách</span><span class="sxs-lookup"><span data-stu-id="88ac9-150">Probe time-out in seconds</span></span> |
| <span data-ttu-id="88ac9-151">Prahová hodnota špatného stavu</span><span class="sxs-lookup"><span data-stu-id="88ac9-151">Unhealthy threshold</span></span> |<span data-ttu-id="88ac9-152">3</span><span class="sxs-lookup"><span data-stu-id="88ac9-152">3</span></span> |<span data-ttu-id="88ac9-153">Počet opakování testu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-153">Probe retry count.</span></span> <span data-ttu-id="88ac9-154">Hello back-end serveru je označena po počet po sobě jdoucích test selhání hello dosáhne hello prahová hodnota špatného stavu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-154">hello back-end server is marked down after hello consecutive probe failure count reaches hello unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="88ac9-155">Řešení</span><span class="sxs-lookup"><span data-stu-id="88ac9-155">Solution</span></span>

* <span data-ttu-id="88ac9-156">Zajistěte, aby výchozí web je nakonfigurován a naslouchá na 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="88ac9-156">Ensure that a default site is configured and is listening at 127.0.0.1.</span></span>
* <span data-ttu-id="88ac9-157">Pokud BackendHttpSetting určuje jiný port než 80, má být hello výchozí web nakonfigurovaných toolisten na tomto portu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-157">If BackendHttpSetting specifies a port other than 80, hello default site should be configured toolisten at that port.</span></span>
* <span data-ttu-id="88ac9-158">Hello toohttp://127.0.0.1:port volání by měl vrátit kód výsledku HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="88ac9-158">hello call toohttp://127.0.0.1:port should return an HTTP result code of 200.</span></span> <span data-ttu-id="88ac9-159">To má být vrácen v časovém limitu 30 sekund hello.</span><span class="sxs-lookup"><span data-stu-id="88ac9-159">This should be returned within hello 30 sec time-out period.</span></span>
* <span data-ttu-id="88ac9-160">Zkontrolujte, zda je otevřený port nakonfigurovaný a že neexistují žádná pravidla brány firewall nebo skupiny zabezpečení sítě Azure, která blokují příchozí nebo odchozí přenosy na portu hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="88ac9-160">Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on hello port configured.</span></span>
* <span data-ttu-id="88ac9-161">Pokud virtuální počítače Azure classic nebo Cloudová služba se používá s plně kvalifikovaný název domény nebo veřejné IP adresy, ujistěte se, že hello odpovídající [koncový bod](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) je otevřen.</span><span class="sxs-lookup"><span data-stu-id="88ac9-161">If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that hello corresponding [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) is opened.</span></span>
* <span data-ttu-id="88ac9-162">Pokud hello virtuální počítač je nakonfigurován pomocí Azure Resource Manager a mimo virtuální síť, kde je nasazená aplikační bránu, hello [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) musí být nakonfigurované tooallow přístup na hello požadovaného portu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-162">If hello VM is configured via Azure Resource Manager and is outside hello VNet where Application Gateway is deployed, [Network Security Group](../virtual-network/virtual-networks-nsg.md) must be configured tooallow access on hello desired port.</span></span>

## <a name="problems-with-custom-health-probe"></a><span data-ttu-id="88ac9-163">Problémy s vlastní stav testu</span><span class="sxs-lookup"><span data-stu-id="88ac9-163">Problems with custom health probe</span></span>

### <a name="cause"></a><span data-ttu-id="88ac9-164">Příčina</span><span class="sxs-lookup"><span data-stu-id="88ac9-164">Cause</span></span>

<span data-ttu-id="88ac9-165">Sondy vlastní stavu Povolit výchozí toohello větší flexibilita zjišťování chování.</span><span class="sxs-lookup"><span data-stu-id="88ac9-165">Custom health probes allow additional flexibility toohello default probing behavior.</span></span> <span data-ttu-id="88ac9-166">Pokud používáte vlastní testy paměti, uživatelé můžou konfigurovat interval testu hello, hello adresu URL a cestu tootest a kolik neúspěšných odpovědí tooaccept před označením hello fond back-end instance jako chybné.</span><span class="sxs-lookup"><span data-stu-id="88ac9-166">When using custom probes, users can configure hello probe interval, hello URL, and path tootest, and how many failed responses tooaccept before marking hello back-end pool instance as unhealthy.</span></span> <span data-ttu-id="88ac9-167">budou přidány Hello následující další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="88ac9-167">hello following additional properties are added.</span></span>

| <span data-ttu-id="88ac9-168">Vlastnost testu</span><span class="sxs-lookup"><span data-stu-id="88ac9-168">Probe property</span></span> | <span data-ttu-id="88ac9-169">Popis</span><span class="sxs-lookup"><span data-stu-id="88ac9-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="88ac9-170">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="88ac9-170">Name</span></span> |<span data-ttu-id="88ac9-171">Název testu hello.</span><span class="sxs-lookup"><span data-stu-id="88ac9-171">Name of hello probe.</span></span> <span data-ttu-id="88ac9-172">Tento název je použité toorefer toohello testu v nastavení HTTP back-end.</span><span class="sxs-lookup"><span data-stu-id="88ac9-172">This name is used toorefer toohello probe in back-end HTTP settings.</span></span> |
| <span data-ttu-id="88ac9-173">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="88ac9-173">Protocol</span></span> |<span data-ttu-id="88ac9-174">Používá protokol toosend hello testu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-174">Protocol used toosend hello probe.</span></span> <span data-ttu-id="88ac9-175">Test Hello používá protokol hello definované v nastavení HTTP back-end hello</span><span class="sxs-lookup"><span data-stu-id="88ac9-175">hello probe uses hello protocol defined in hello back-end HTTP settings</span></span> |
| <span data-ttu-id="88ac9-176">Hostitel</span><span class="sxs-lookup"><span data-stu-id="88ac9-176">Host</span></span> |<span data-ttu-id="88ac9-177">Hostitele název toosend hello testu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-177">Host name toosend hello probe.</span></span> <span data-ttu-id="88ac9-178">Použít pouze v případě, že na Application Gateway je nakonfigurováno více lokalit.</span><span class="sxs-lookup"><span data-stu-id="88ac9-178">Applicable only when multi-site is configured on Application Gateway.</span></span> <span data-ttu-id="88ac9-179">To se liší od název hostitele virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="88ac9-179">This is different from VM host name.</span></span> |
| <span data-ttu-id="88ac9-180">Cesta</span><span class="sxs-lookup"><span data-stu-id="88ac9-180">Path</span></span> |<span data-ttu-id="88ac9-181">Relativní cesta hello kontroly.</span><span class="sxs-lookup"><span data-stu-id="88ac9-181">Relative path of hello probe.</span></span> <span data-ttu-id="88ac9-182">platná cesta Hello spustí z '/'.</span><span class="sxs-lookup"><span data-stu-id="88ac9-182">hello valid path starts from '/'.</span></span> <span data-ttu-id="88ac9-183">Test Hello je odeslán příliš\<protokol\>://\<hostitele\>:\<port\>\<cesta\></span><span class="sxs-lookup"><span data-stu-id="88ac9-183">hello probe is sent too\<protocol\>://\<host\>:\<port\>\<path\></span></span> |
| <span data-ttu-id="88ac9-184">Interval</span><span class="sxs-lookup"><span data-stu-id="88ac9-184">Interval</span></span> |<span data-ttu-id="88ac9-185">Interval testu paměti v sekundách.</span><span class="sxs-lookup"><span data-stu-id="88ac9-185">Probe interval in seconds.</span></span> <span data-ttu-id="88ac9-186">Toto je hello časový interval mezi dvě po sobě jdoucích sondy.</span><span class="sxs-lookup"><span data-stu-id="88ac9-186">This is hello time interval between two consecutive probes.</span></span> |
| <span data-ttu-id="88ac9-187">Časový limit</span><span class="sxs-lookup"><span data-stu-id="88ac9-187">Time-out</span></span> |<span data-ttu-id="88ac9-188">Časový limit testu v sekundách.</span><span class="sxs-lookup"><span data-stu-id="88ac9-188">Probe time-out in seconds.</span></span> <span data-ttu-id="88ac9-189">Pokud není platnou odpověď v rámci tento časový limit, hello probe je označen jako se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="88ac9-189">If a valid response is not received within this time-out period, hello probe is marked as failed.</span></span> |
| <span data-ttu-id="88ac9-190">Prahová hodnota špatného stavu</span><span class="sxs-lookup"><span data-stu-id="88ac9-190">Unhealthy threshold</span></span> |<span data-ttu-id="88ac9-191">Počet opakování testu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-191">Probe retry count.</span></span> <span data-ttu-id="88ac9-192">Hello back-end serveru je označena po počet po sobě jdoucích test selhání hello dosáhne hello prahová hodnota špatného stavu.</span><span class="sxs-lookup"><span data-stu-id="88ac9-192">hello back-end server is marked down after hello consecutive probe failure count reaches hello unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="88ac9-193">Řešení</span><span class="sxs-lookup"><span data-stu-id="88ac9-193">Solution</span></span>

<span data-ttu-id="88ac9-194">Ověřte, že hello sběru dat stavu vlastní je správně nakonfigurován jako hello předcházející tabulce.</span><span class="sxs-lookup"><span data-stu-id="88ac9-194">Validate that hello Custom Health Probe is configured correctly as hello preceding table.</span></span> <span data-ttu-id="88ac9-195">Kromě toho toohello předchozí řešení potíží, ujistěte se také hello následující:</span><span class="sxs-lookup"><span data-stu-id="88ac9-195">In addition toohello preceding troubleshooting steps, also ensure hello following:</span></span>

* <span data-ttu-id="88ac9-196">Ujistěte se, že test hello je správně zadán podle hello [průvodce](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="88ac9-196">Ensure that hello probe is correctly specified as per hello [guide](application-gateway-create-probe-ps.md).</span></span>
* <span data-ttu-id="88ac9-197">Pokud aplikace brána je nakonfigurovaná pro jednu lokalitu, ve výchozím nastavení hello hostitele název musí být zadán jako "127.0.0.1", pokud nebudou jinak nakonfigurovaná v vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="88ac9-197">If Application Gateway is configured for a single site, by default hello Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.</span></span>
* <span data-ttu-id="88ac9-198">Ujistěte se, že volání toohttp: / /\<hostitele\>:\<port\>\<cesta\> vrátí výsledek kód HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="88ac9-198">Ensure that a call toohttp://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.</span></span>
* <span data-ttu-id="88ac9-199">Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci hello přijatelný rozsah.</span><span class="sxs-lookup"><span data-stu-id="88ac9-199">Ensure that Interval, Time-out and UnhealtyThreshold are within hello acceptable ranges.</span></span>
* <span data-ttu-id="88ac9-200">Pokud sběru dat pomocí protokolu HTTPS, ujistěte se, že tento server back-end hello nevyžaduje SNI nakonfigurováním certifikát fallback na samotném serveru back-end hello.</span><span class="sxs-lookup"><span data-stu-id="88ac9-200">If using an HTTPS probe, make sure that hello backend server doesn't require SNI by configuring a fallback certificate on hello backend server itself.</span></span> 
* <span data-ttu-id="88ac9-201">Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci hello přijatelný rozsah.</span><span class="sxs-lookup"><span data-stu-id="88ac9-201">Ensure that Interval, Time-out, and UnhealtyThreshold are within hello acceptable ranges.</span></span>

## <a name="request-time-out"></a><span data-ttu-id="88ac9-202">Časový limit požadavku</span><span class="sxs-lookup"><span data-stu-id="88ac9-202">Request time out</span></span>

### <a name="cause"></a><span data-ttu-id="88ac9-203">Příčina</span><span class="sxs-lookup"><span data-stu-id="88ac9-203">Cause</span></span>

<span data-ttu-id="88ac9-204">Po přijetí požadavku uživatele Application Gateway platí hello nakonfigurovaná pravidla toohello požadavku a nasměruje ho fond back-end instance tooa.</span><span class="sxs-lookup"><span data-stu-id="88ac9-204">When a user request is received, Application Gateway applies hello configured rules toohello request and routes it tooa back-end pool instance.</span></span> <span data-ttu-id="88ac9-205">Čeká konfigurovat interval času pro odpověď z back-end instance hello.</span><span class="sxs-lookup"><span data-stu-id="88ac9-205">It waits for a configurable interval of time for a response from hello back-end instance.</span></span> <span data-ttu-id="88ac9-206">Ve výchozím nastavení je tento interval **30 sekund**.</span><span class="sxs-lookup"><span data-stu-id="88ac9-206">By default, this interval is **30 seconds**.</span></span> <span data-ttu-id="88ac9-207">Pokud Application Gateway neobdrží odpověď z back-end aplikace v tomto intervalu, by žádost uživatele zobrazí chyby 502.</span><span class="sxs-lookup"><span data-stu-id="88ac9-207">If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.</span></span>

### <a name="solution"></a><span data-ttu-id="88ac9-208">Řešení</span><span class="sxs-lookup"><span data-stu-id="88ac9-208">Solution</span></span>

<span data-ttu-id="88ac9-209">Aplikační brána umožňuje uživatelům tooconfigure toto nastavení prostřednictvím BackendHttpSetting, který lze potom použít toodifferent fondy.</span><span class="sxs-lookup"><span data-stu-id="88ac9-209">Application Gateway allows users tooconfigure this setting via BackendHttpSetting, which can be then applied toodifferent pools.</span></span> <span data-ttu-id="88ac9-210">Různé fondy back-end může mít různé BackendHttpSetting a proto různé žádosti časový limit nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="88ac9-210">Different back-end pools can have different BackendHttpSetting and hence different request time out configured.</span></span>

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a><span data-ttu-id="88ac9-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88ac9-211">Next steps</span></span>

<span data-ttu-id="88ac9-212">Pokud předchozí kroky hello nevyřeší hello problém, otevřete [lístek podpory](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="88ac9-212">If hello preceding steps do not resolve hello issue, open a [support ticket](https://azure.microsoft.com/support/options/).</span></span>

