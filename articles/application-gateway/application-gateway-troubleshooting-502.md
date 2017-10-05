---
title: "Řešení chyb Azure aplikační brány chybný brány (502) | Microsoft Docs"
description: "Zjistěte, jak vyřešit chyby 502 brány aplikace"
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
ms.openlocfilehash: cbf9c552c4818b3925f449081539f1db6d61918e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a><span data-ttu-id="938e2-103">Řešení potíží s chybami Chybná brána v aplikační brány</span><span class="sxs-lookup"><span data-stu-id="938e2-103">Troubleshooting bad gateway errors in Application Gateway</span></span>

<span data-ttu-id="938e2-104">Informace o řešení potíží s Chybná brána (502) chyby přijaté při použití aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="938e2-104">Learn how to troubleshoot bad gateway (502) errors received when using application gateway.</span></span>

## <a name="overview"></a><span data-ttu-id="938e2-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="938e2-105">Overview</span></span>

<span data-ttu-id="938e2-106">Po dokončení konfigurace aplikační brány, jeden chyb, které uživatelé setkat je "Chyba serveru: 502 – Webový server obdržel neplatnou odpověď době, kdy fungoval jako brána nebo proxy server".</span><span class="sxs-lookup"><span data-stu-id="938e2-106">After configuring an application gateway, one of the errors that users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server".</span></span> <span data-ttu-id="938e2-107">K této chybě může dojít z následujících důvodů hlavní:</span><span class="sxs-lookup"><span data-stu-id="938e2-107">This error may happen due to the following main reasons:</span></span>

* <span data-ttu-id="938e2-108">Služba Azure Application Gateway [fond back-end není nakonfigurován nebo prázdný](#empty-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="938e2-108">Azure Application Gateway's [back-end pool is not configured or empty](#empty-backendaddresspool).</span></span>
* <span data-ttu-id="938e2-109">Žádný z virtuálních počítačů nebo instancí v [Škálovací sady virtuálního počítače jsou v pořádku](#unhealthy-instances-in-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="938e2-109">None of the VMs or instances in [VM Scale Set are healthy](#unhealthy-instances-in-backendaddresspool).</span></span>
* <span data-ttu-id="938e2-110">Virtuální počítače nebo instance Škálovací sady virtuálního počítače back-end jsou [neodpovídá výchozí kontroly stavu](#problems-with-default-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="938e2-110">Back-end VMs or instances of VM Scale Set are [not responding to the default health probe](#problems-with-default-health-probe.md).</span></span>
* <span data-ttu-id="938e2-111">Neplatný nebo nesprávný [konfigurace sondy vlastní stavu](#problems-with-custom-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="938e2-111">Invalid or improper [configuration of custom health probes](#problems-with-custom-health-probe.md).</span></span>
* <span data-ttu-id="938e2-112">[Žádosti o vypršení časového limitu nebo problémy s připojením k](#request-time-out) s požadavky uživatelů.</span><span class="sxs-lookup"><span data-stu-id="938e2-112">[Request time out or connectivity issues](#request-time-out) with user requests.</span></span>

## <a name="empty-backendaddresspool"></a><span data-ttu-id="938e2-113">Prázdný BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="938e2-113">Empty BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="938e2-114">Příčina</span><span class="sxs-lookup"><span data-stu-id="938e2-114">Cause</span></span>

<span data-ttu-id="938e2-115">Pokud službu Application Gateway nemá žádné virtuální počítače nebo sady škálování virtuálního počítače nakonfigurované ve fondu adres back-end, nemůžete provádět směrování každá zákazníka žádost a vyvolá chybu Chybná brána.</span><span class="sxs-lookup"><span data-stu-id="938e2-115">If the Application Gateway has no VMs or VM Scale Set configured in the back-end address pool, it cannot route any customer request and throws a bad gateway error.</span></span>

### <a name="solution"></a><span data-ttu-id="938e2-116">Řešení</span><span class="sxs-lookup"><span data-stu-id="938e2-116">Solution</span></span>

<span data-ttu-id="938e2-117">Ujistěte se, že ve fondu back-end není prázdná.</span><span class="sxs-lookup"><span data-stu-id="938e2-117">Ensure that the back-end address pool is not empty.</span></span> <span data-ttu-id="938e2-118">To lze provést prostřednictvím prostředí PowerShell, rozhraní příkazového řádku nebo portálu.</span><span class="sxs-lookup"><span data-stu-id="938e2-118">This can be done either via PowerShell, CLI, or portal.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

<span data-ttu-id="938e2-119">Výstup z předchozí rutiny by měl obsahovat fond adres back-end není prázdný.</span><span class="sxs-lookup"><span data-stu-id="938e2-119">The output from the preceding cmdlet should contain non-empty back-end address pool.</span></span> <span data-ttu-id="938e2-120">Tady je příklad, kde, dvěma fondy jsou vráceny, které jsou nakonfigurované plně kvalifikovaný název domény nebo IP adresy pro back-end virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="938e2-120">Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs.</span></span> <span data-ttu-id="938e2-121">Stav zřizování BackendAddressPool musí být 'bylo úspěšné".</span><span class="sxs-lookup"><span data-stu-id="938e2-121">The provisioning state of the BackendAddressPool must be 'Succeeded'.</span></span>

<span data-ttu-id="938e2-122">BackendAddressPoolsText:</span><span class="sxs-lookup"><span data-stu-id="938e2-122">BackendAddressPoolsText:</span></span>

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

## <a name="unhealthy-instances-in-backendaddresspool"></a><span data-ttu-id="938e2-123">Není v pořádku instance v BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="938e2-123">Unhealthy instances in BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="938e2-124">Příčina</span><span class="sxs-lookup"><span data-stu-id="938e2-124">Cause</span></span>

<span data-ttu-id="938e2-125">Pokud jsou všechny instance BackendAddressPool není v pořádku, aplikační brána nebude mít žádné back-end pro směrování požadavku uživatele na.</span><span class="sxs-lookup"><span data-stu-id="938e2-125">If all the instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end to route user request to.</span></span> <span data-ttu-id="938e2-126">Případě to mohou být také při instance back-end jsou v pořádku, ale nemáte požadovanou aplikaci nasadit.</span><span class="sxs-lookup"><span data-stu-id="938e2-126">This could also be the case when back-end instances are healthy but do not have the required application deployed.</span></span>

### <a name="solution"></a><span data-ttu-id="938e2-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="938e2-127">Solution</span></span>

<span data-ttu-id="938e2-128">Zajistěte, aby instance jsou v pořádku a je aplikace správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="938e2-128">Ensure that the instances are healthy and the application is properly configured.</span></span> <span data-ttu-id="938e2-129">Zkontrolujte, jestli jsou instance back-end schopné reagovat na použití příkazu ping z jiného virtuálního počítače ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="938e2-129">Check if the back-end instances are able to respond to a ping from another VM in the same VNet.</span></span> <span data-ttu-id="938e2-130">Pokud nakonfigurované veřejný koncový bod, zajistěte, aby byl žádost prohlížeče na webovou aplikaci obsluhovatelná.</span><span class="sxs-lookup"><span data-stu-id="938e2-130">If configured with a public end point, ensure that a browser request to the web application is serviceable.</span></span>

## <a name="problems-with-default-health-probe"></a><span data-ttu-id="938e2-131">Problémy s výchozí kontroly stavu</span><span class="sxs-lookup"><span data-stu-id="938e2-131">Problems with default health probe</span></span>

### <a name="cause"></a><span data-ttu-id="938e2-132">Příčina</span><span class="sxs-lookup"><span data-stu-id="938e2-132">Cause</span></span>

<span data-ttu-id="938e2-133">chyby 502 může být také časté indikátory, že výchozí kontroly stavu není dosažitelné virtuálních počítačů v back-end.</span><span class="sxs-lookup"><span data-stu-id="938e2-133">502 errors can also be frequent indicators that the default health probe is not able to reach back-end VMs.</span></span> <span data-ttu-id="938e2-134">Při zřízení instance aplikační brány automaticky nakonfiguruje výchozí kontrolu stavu pro každý BackendAddressPool pomocí vlastností BackendHttpSetting.</span><span class="sxs-lookup"><span data-stu-id="938e2-134">When an Application Gateway instance is provisioned, it automatically configures a default health probe to each BackendAddressPool using properties of the BackendHttpSetting.</span></span> <span data-ttu-id="938e2-135">Žádný uživatelský vstup je potřeba nastavit tento test.</span><span class="sxs-lookup"><span data-stu-id="938e2-135">No user input is required to set this probe.</span></span> <span data-ttu-id="938e2-136">Konkrétně Pokud je nakonfigurovaná pravidlo Vyrovnávání zatížení, se provádí přidružení mezi BackendHttpSetting a BackendAddressPool.</span><span class="sxs-lookup"><span data-stu-id="938e2-136">Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool.</span></span> <span data-ttu-id="938e2-137">Výchozí kontroly je nakonfigurován pro každou z těchto přidružení a Aplikační brána zahájí pravidelné stavu zkontrolujte připojení k jednotlivým instancím v BackendAddressPool na port zadaný v elementu BackendHttpSetting.</span><span class="sxs-lookup"><span data-stu-id="938e2-137">A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection to each instance in the BackendAddressPool at the port specified in the BackendHttpSetting element.</span></span> <span data-ttu-id="938e2-138">Následující tabulka uvádí hodnoty přidružené k výchozí kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="938e2-138">Following table lists the values associated with the default health probe.</span></span>

| <span data-ttu-id="938e2-139">Vlastnost testu</span><span class="sxs-lookup"><span data-stu-id="938e2-139">Probe property</span></span> | <span data-ttu-id="938e2-140">Hodnota</span><span class="sxs-lookup"><span data-stu-id="938e2-140">Value</span></span> | <span data-ttu-id="938e2-141">Popis</span><span class="sxs-lookup"><span data-stu-id="938e2-141">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="938e2-142">Adresa URL testu</span><span class="sxs-lookup"><span data-stu-id="938e2-142">Probe URL</span></span> |<span data-ttu-id="938e2-143">http://127.0.0.1/</span><span class="sxs-lookup"><span data-stu-id="938e2-143">http://127.0.0.1/</span></span> |<span data-ttu-id="938e2-144">Cesta adresy URL</span><span class="sxs-lookup"><span data-stu-id="938e2-144">URL path</span></span> |
| <span data-ttu-id="938e2-145">Interval</span><span class="sxs-lookup"><span data-stu-id="938e2-145">Interval</span></span> |<span data-ttu-id="938e2-146">30</span><span class="sxs-lookup"><span data-stu-id="938e2-146">30</span></span> |<span data-ttu-id="938e2-147">Interval testu paměti v sekundách</span><span class="sxs-lookup"><span data-stu-id="938e2-147">Probe interval in seconds</span></span> |
| <span data-ttu-id="938e2-148">Časový limit</span><span class="sxs-lookup"><span data-stu-id="938e2-148">Time-out</span></span> |<span data-ttu-id="938e2-149">30</span><span class="sxs-lookup"><span data-stu-id="938e2-149">30</span></span> |<span data-ttu-id="938e2-150">Časový limit testu v sekundách</span><span class="sxs-lookup"><span data-stu-id="938e2-150">Probe time-out in seconds</span></span> |
| <span data-ttu-id="938e2-151">Prahová hodnota špatného stavu</span><span class="sxs-lookup"><span data-stu-id="938e2-151">Unhealthy threshold</span></span> |<span data-ttu-id="938e2-152">3</span><span class="sxs-lookup"><span data-stu-id="938e2-152">3</span></span> |<span data-ttu-id="938e2-153">Počet opakování testu.</span><span class="sxs-lookup"><span data-stu-id="938e2-153">Probe retry count.</span></span> <span data-ttu-id="938e2-154">Back-end serverů je označena po počet po sobě jdoucích test selhání dosáhne prahová hodnota špatného stavu.</span><span class="sxs-lookup"><span data-stu-id="938e2-154">The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="938e2-155">Řešení</span><span class="sxs-lookup"><span data-stu-id="938e2-155">Solution</span></span>

* <span data-ttu-id="938e2-156">Zajistěte, aby výchozí web je nakonfigurován a naslouchá na 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="938e2-156">Ensure that a default site is configured and is listening at 127.0.0.1.</span></span>
* <span data-ttu-id="938e2-157">Pokud BackendHttpSetting určuje jiný port než 80, by měl být výchozí web nakonfigurován pro naslouchání na tomto portu.</span><span class="sxs-lookup"><span data-stu-id="938e2-157">If BackendHttpSetting specifies a port other than 80, the default site should be configured to listen at that port.</span></span>
* <span data-ttu-id="938e2-158">Volání http://127.0.0.1:port by měl vrátit kód výsledku HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="938e2-158">The call to http://127.0.0.1:port should return an HTTP result code of 200.</span></span> <span data-ttu-id="938e2-159">To má být vrácen v daném časovém limitu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="938e2-159">This should be returned within the 30 sec time-out period.</span></span>
* <span data-ttu-id="938e2-160">Zkontrolujte, zda je otevřený port nakonfigurovaný a že neexistují žádná pravidla brány firewall nebo skupiny zabezpečení sítě Azure, která blokují příchozí nebo odchozí přenosy na portu nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="938e2-160">Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on the port configured.</span></span>
* <span data-ttu-id="938e2-161">Pokud virtuální počítače Azure classic nebo Cloudová služba se používá s plně kvalifikovaný název domény nebo veřejné IP adresy, ujistěte se, že odpovídající [koncový bod](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) je otevřen.</span><span class="sxs-lookup"><span data-stu-id="938e2-161">If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that the corresponding [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) is opened.</span></span>
* <span data-ttu-id="938e2-162">Pokud virtuální počítač je nakonfigurován pomocí Azure Resource Manager a mimo virtuální síť, kde je nasazená aplikační bránu, [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) musí být nakonfigurována pro povolení přístupu na požadovaný port.</span><span class="sxs-lookup"><span data-stu-id="938e2-162">If the VM is configured via Azure Resource Manager and is outside the VNet where Application Gateway is deployed, [Network Security Group](../virtual-network/virtual-networks-nsg.md) must be configured to allow access on the desired port.</span></span>

## <a name="problems-with-custom-health-probe"></a><span data-ttu-id="938e2-163">Problémy s vlastní stav testu</span><span class="sxs-lookup"><span data-stu-id="938e2-163">Problems with custom health probe</span></span>

### <a name="cause"></a><span data-ttu-id="938e2-164">Příčina</span><span class="sxs-lookup"><span data-stu-id="938e2-164">Cause</span></span>

<span data-ttu-id="938e2-165">Sondy vlastní stavu povolit větší flexibilita výchozí zjišťování chování.</span><span class="sxs-lookup"><span data-stu-id="938e2-165">Custom health probes allow additional flexibility to the default probing behavior.</span></span> <span data-ttu-id="938e2-166">Pokud používáte vlastní testy paměti, uživatelé můžou konfigurovat interval testu, adresu URL a cesty k testování a kolik neúspěšných odpovědí tak, aby přijímal před označením fond back-end instance jako chybné.</span><span class="sxs-lookup"><span data-stu-id="938e2-166">When using custom probes, users can configure the probe interval, the URL, and path to test, and how many failed responses to accept before marking the back-end pool instance as unhealthy.</span></span> <span data-ttu-id="938e2-167">Jsou přidány následující další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="938e2-167">The following additional properties are added.</span></span>

| <span data-ttu-id="938e2-168">Vlastnost testu</span><span class="sxs-lookup"><span data-stu-id="938e2-168">Probe property</span></span> | <span data-ttu-id="938e2-169">Popis</span><span class="sxs-lookup"><span data-stu-id="938e2-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="938e2-170">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="938e2-170">Name</span></span> |<span data-ttu-id="938e2-171">Název kontroly.</span><span class="sxs-lookup"><span data-stu-id="938e2-171">Name of the probe.</span></span> <span data-ttu-id="938e2-172">Tento název se používá k odkazování na test v nastavení HTTP back-end.</span><span class="sxs-lookup"><span data-stu-id="938e2-172">This name is used to refer to the probe in back-end HTTP settings.</span></span> |
| <span data-ttu-id="938e2-173">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="938e2-173">Protocol</span></span> |<span data-ttu-id="938e2-174">Protokol používaný k odesílání sonda.</span><span class="sxs-lookup"><span data-stu-id="938e2-174">Protocol used to send the probe.</span></span> <span data-ttu-id="938e2-175">Tato kontrola používá protokol definované v nastavení HTTP back-end</span><span class="sxs-lookup"><span data-stu-id="938e2-175">The probe uses the protocol defined in the back-end HTTP settings</span></span> |
| <span data-ttu-id="938e2-176">Hostitel</span><span class="sxs-lookup"><span data-stu-id="938e2-176">Host</span></span> |<span data-ttu-id="938e2-177">Název hostitele k odeslání test.</span><span class="sxs-lookup"><span data-stu-id="938e2-177">Host name to send the probe.</span></span> <span data-ttu-id="938e2-178">Použít pouze v případě, že na Application Gateway je nakonfigurováno více lokalit.</span><span class="sxs-lookup"><span data-stu-id="938e2-178">Applicable only when multi-site is configured on Application Gateway.</span></span> <span data-ttu-id="938e2-179">To se liší od název hostitele virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="938e2-179">This is different from VM host name.</span></span> |
| <span data-ttu-id="938e2-180">Cesta</span><span class="sxs-lookup"><span data-stu-id="938e2-180">Path</span></span> |<span data-ttu-id="938e2-181">Relativní cesta kontroly.</span><span class="sxs-lookup"><span data-stu-id="938e2-181">Relative path of the probe.</span></span> <span data-ttu-id="938e2-182">Platná cesta spustí z '/'.</span><span class="sxs-lookup"><span data-stu-id="938e2-182">The valid path starts from '/'.</span></span> <span data-ttu-id="938e2-183">Posílá sonda \<protokol\>://\<hostitele\>:\<port\>\<cesta\></span><span class="sxs-lookup"><span data-stu-id="938e2-183">The probe is sent to \<protocol\>://\<host\>:\<port\>\<path\></span></span> |
| <span data-ttu-id="938e2-184">Interval</span><span class="sxs-lookup"><span data-stu-id="938e2-184">Interval</span></span> |<span data-ttu-id="938e2-185">Interval testu paměti v sekundách.</span><span class="sxs-lookup"><span data-stu-id="938e2-185">Probe interval in seconds.</span></span> <span data-ttu-id="938e2-186">Toto je časový interval mezi dvě po sobě jdoucích sondy.</span><span class="sxs-lookup"><span data-stu-id="938e2-186">This is the time interval between two consecutive probes.</span></span> |
| <span data-ttu-id="938e2-187">Časový limit</span><span class="sxs-lookup"><span data-stu-id="938e2-187">Time-out</span></span> |<span data-ttu-id="938e2-188">Časový limit testu v sekundách.</span><span class="sxs-lookup"><span data-stu-id="938e2-188">Probe time-out in seconds.</span></span> <span data-ttu-id="938e2-189">Pokud není platnou odpověď v rámci tento časový limit, tato kontrola je označeno jeho selhání.</span><span class="sxs-lookup"><span data-stu-id="938e2-189">If a valid response is not received within this time-out period, the probe is marked as failed.</span></span> |
| <span data-ttu-id="938e2-190">Prahová hodnota špatného stavu</span><span class="sxs-lookup"><span data-stu-id="938e2-190">Unhealthy threshold</span></span> |<span data-ttu-id="938e2-191">Počet opakování testu.</span><span class="sxs-lookup"><span data-stu-id="938e2-191">Probe retry count.</span></span> <span data-ttu-id="938e2-192">Back-end serverů je označena po počet po sobě jdoucích test selhání dosáhne prahová hodnota špatného stavu.</span><span class="sxs-lookup"><span data-stu-id="938e2-192">The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="938e2-193">Řešení</span><span class="sxs-lookup"><span data-stu-id="938e2-193">Solution</span></span>

<span data-ttu-id="938e2-194">Ověřte, zda vlastní stav testu správně nakonfigurovaná jako v předchozí tabulce.</span><span class="sxs-lookup"><span data-stu-id="938e2-194">Validate that the Custom Health Probe is configured correctly as the preceding table.</span></span> <span data-ttu-id="938e2-195">Kromě předchozích kroků řešení potíží zkontrolujte také následující:</span><span class="sxs-lookup"><span data-stu-id="938e2-195">In addition to the preceding troubleshooting steps, also ensure the following:</span></span>

* <span data-ttu-id="938e2-196">Zajistěte, aby tato kontrola je správně zadán dle [průvodce](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="938e2-196">Ensure that the probe is correctly specified as per the [guide](application-gateway-create-probe-ps.md).</span></span>
* <span data-ttu-id="938e2-197">Pokud aplikace brána je nakonfigurovaná pro jednu lokalitu, ve výchozím nastavení hostitele název musí být zadán jako "127.0.0.1", pokud nebudou jinak nakonfigurovaná v vlastní test paměti.</span><span class="sxs-lookup"><span data-stu-id="938e2-197">If Application Gateway is configured for a single site, by default the Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.</span></span>
* <span data-ttu-id="938e2-198">Ujistěte se, že volání http://\<hostitele\>:\<port\>\<cesta\> vrátí výsledek kód HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="938e2-198">Ensure that a call to http://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.</span></span>
* <span data-ttu-id="938e2-199">Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci přijatelný rozsah.</span><span class="sxs-lookup"><span data-stu-id="938e2-199">Ensure that Interval, Time-out and UnhealtyThreshold are within the acceptable ranges.</span></span>
* <span data-ttu-id="938e2-200">Pokud sběru dat pomocí protokolu HTTPS, ujistěte se, že back-end serveru nevyžaduje SNI nakonfigurováním certifikát fallback na samotném serveru back-end.</span><span class="sxs-lookup"><span data-stu-id="938e2-200">If using an HTTPS probe, make sure that the backend server doesn't require SNI by configuring a fallback certificate on the backend server itself.</span></span> 
* <span data-ttu-id="938e2-201">Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci přijatelný rozsah.</span><span class="sxs-lookup"><span data-stu-id="938e2-201">Ensure that Interval, Time-out, and UnhealtyThreshold are within the acceptable ranges.</span></span>

## <a name="request-time-out"></a><span data-ttu-id="938e2-202">Časový limit požadavku</span><span class="sxs-lookup"><span data-stu-id="938e2-202">Request time out</span></span>

### <a name="cause"></a><span data-ttu-id="938e2-203">Příčina</span><span class="sxs-lookup"><span data-stu-id="938e2-203">Cause</span></span>

<span data-ttu-id="938e2-204">Po přijetí požadavku uživatele Aplikační brána nakonfigurovaná pravidla se vztahují na žádost a směruje na fond back-end instance.</span><span class="sxs-lookup"><span data-stu-id="938e2-204">When a user request is received, Application Gateway applies the configured rules to the request and routes it to a back-end pool instance.</span></span> <span data-ttu-id="938e2-205">Čeká konfigurovat interval času pro odpověď z back-end instance.</span><span class="sxs-lookup"><span data-stu-id="938e2-205">It waits for a configurable interval of time for a response from the back-end instance.</span></span> <span data-ttu-id="938e2-206">Ve výchozím nastavení je tento interval **30 sekund**.</span><span class="sxs-lookup"><span data-stu-id="938e2-206">By default, this interval is **30 seconds**.</span></span> <span data-ttu-id="938e2-207">Pokud Application Gateway neobdrží odpověď z back-end aplikace v tomto intervalu, by žádost uživatele zobrazí chyby 502.</span><span class="sxs-lookup"><span data-stu-id="938e2-207">If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.</span></span>

### <a name="solution"></a><span data-ttu-id="938e2-208">Řešení</span><span class="sxs-lookup"><span data-stu-id="938e2-208">Solution</span></span>

<span data-ttu-id="938e2-209">Aplikační brána umožňuje uživatelům nakonfigurovat toto nastavení prostřednictvím BackendHttpSetting, který lze potom použít na různých fondů.</span><span class="sxs-lookup"><span data-stu-id="938e2-209">Application Gateway allows users to configure this setting via BackendHttpSetting, which can be then applied to different pools.</span></span> <span data-ttu-id="938e2-210">Různé fondy back-end může mít různé BackendHttpSetting a proto různé žádosti časový limit nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="938e2-210">Different back-end pools can have different BackendHttpSetting and hence different request time out configured.</span></span>

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a><span data-ttu-id="938e2-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="938e2-211">Next steps</span></span>

<span data-ttu-id="938e2-212">Pokud předchozí kroky není problém vyřešit, otevřete [lístek podpory](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="938e2-212">If the preceding steps do not resolve the issue, open a [support ticket](https://azure.microsoft.com/support/options/).</span></span>

