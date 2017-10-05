---
title: "Řešení Azure sítě analýzy v Log Analytics | Microsoft Docs"
description: "Řešení Azure Analytics sítě můžete použít v analýzy protokolů ke kontrole protokolech skupiny zabezpečení sítě Azure a Azure Application Gateway."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 06b67322b3812a668a515ecc357171ede1d85441
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a><span data-ttu-id="66aa1-103">Monitorování řešení v analýzy protokolů Azure sítě</span><span class="sxs-lookup"><span data-stu-id="66aa1-103">Azure networking monitoring solutions in Log Analytics</span></span>

<span data-ttu-id="66aa1-104">Analýzy protokolů nabízí následující řešení pro monitorování vaší sítí:</span><span class="sxs-lookup"><span data-stu-id="66aa1-104">Log Analytics offers the following solutions for monitoring your networks:</span></span>
* <span data-ttu-id="66aa1-105">Sledování výkonu sítě (NPM) na</span><span class="sxs-lookup"><span data-stu-id="66aa1-105">Network Performance Monitor (NPM) to</span></span>
 * <span data-ttu-id="66aa1-106">Monitorování stavu sítě</span><span class="sxs-lookup"><span data-stu-id="66aa1-106">Monitor the health of your network</span></span>
* <span data-ttu-id="66aa1-107">Azure Application Gateway analytics ke kontrole</span><span class="sxs-lookup"><span data-stu-id="66aa1-107">Azure Application Gateway analytics to review</span></span>
 * <span data-ttu-id="66aa1-108">Protokoly služby Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-108">Azure Application Gateway logs</span></span>
 * <span data-ttu-id="66aa1-109">Metriky Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-109">Azure Application Gateway metrics</span></span>
* <span data-ttu-id="66aa1-110">Skupina zabezpečení sítě Azure analytics ke kontrole</span><span class="sxs-lookup"><span data-stu-id="66aa1-110">Azure Network Security Group analytics to review</span></span>
 * <span data-ttu-id="66aa1-111">Protokoly skupinu zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="66aa1-111">Azure Network Security Group logs</span></span>

## <a name="network-performance-monitor-npm"></a><span data-ttu-id="66aa1-112">Sledování výkonu sítě (NPM)</span><span class="sxs-lookup"><span data-stu-id="66aa1-112">Network Performance Monitor (NPM)</span></span>

<span data-ttu-id="66aa1-113">[Sledování výkonu sítě](log-analytics-network-performance-monitor.md) řešení správy je monitorování řešení, která sleduje stav, dostupnosti a dostupnosti sítě sítě.</span><span class="sxs-lookup"><span data-stu-id="66aa1-113">The [Network Performance Monitor](log-analytics-network-performance-monitor.md) management solution is a network monitoring solution, that monitors the health, availability and reachability of networks.</span></span>  <span data-ttu-id="66aa1-114">Se používá k monitorování připojení mezi:</span><span class="sxs-lookup"><span data-stu-id="66aa1-114">It is used to monitor connectivity between:</span></span>

* <span data-ttu-id="66aa1-115">veřejný cloud a místní</span><span class="sxs-lookup"><span data-stu-id="66aa1-115">Public cloud and on-premises</span></span>
* <span data-ttu-id="66aa1-116">datových center a umístění uživatele (firemních pobočkách)</span><span class="sxs-lookup"><span data-stu-id="66aa1-116">Data centers and user locations (branch offices)</span></span>
* <span data-ttu-id="66aa1-117">podsítě hostování různé úrovně víceúrovňových aplikací.</span><span class="sxs-lookup"><span data-stu-id="66aa1-117">Subnets hosting various tiers of a multi-tiered application.</span></span>

<span data-ttu-id="66aa1-118">Další informace najdete v tématu [sledování výkonu sítě](log-analytics-network-performance-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="66aa1-118">For more information, see [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span></span>

## <a name="azure-application-gateway-and-network-security-group-analytics"></a><span data-ttu-id="66aa1-119">Analýza Azure Application Gateway a skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="66aa1-119">Azure Application Gateway and Network Security Group analytics</span></span>
<span data-ttu-id="66aa1-120">Použití řešení:</span><span class="sxs-lookup"><span data-stu-id="66aa1-120">To use the solutions:</span></span>
1. <span data-ttu-id="66aa1-121">Přidat do řešení pro správu k analýze protokolů a</span><span class="sxs-lookup"><span data-stu-id="66aa1-121">Add the management solution to Log Analytics, and</span></span>
2. <span data-ttu-id="66aa1-122">Povolte diagnostiku pro přesměrování diagnostiku do pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="66aa1-122">Enable diagnostics to direct the diagnostics to a Log Analytics workspace.</span></span> <span data-ttu-id="66aa1-123">Není nutné zapsat protokoly do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="66aa1-123">It is not necessary to write the logs to Azure Blob storage.</span></span>

<span data-ttu-id="66aa1-124">Diagnostika a odpovídající řešení můžete povolit pro jednoho nebo obou aplikační brány a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="66aa1-124">You can enable diagnostics and the corresponding solution for either one or both of Application Gateway and Networking Security Groups.</span></span>

<span data-ttu-id="66aa1-125">Pokud nepovolujte protokolování diagnostiky pro konkrétní typ prostředku, ale instalace řešení, jsou prázdné okna řídicí panel pro tento prostředek a zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="66aa1-125">If you do not enable diagnostic logging for a particular resource type, but install the solution, the dashboard blades for that resource are blank and display an error message.</span></span>

> [!NOTE]
> <span data-ttu-id="66aa1-126">V lednu 2017 podporované způsob odesílání protokolů z brány aplikace a skupiny zabezpečení sítě k analýze protokolů změnit.</span><span class="sxs-lookup"><span data-stu-id="66aa1-126">In January 2017, the supported way of sending logs from Application Gateways and Network Security Groups to Log Analytics changed.</span></span> <span data-ttu-id="66aa1-127">Pokud se zobrazí **Azure sítě Analytics (nepoužívané)** řešení, najdete [migrace z původního řešení sítě analýzy](#migrating-from-the-old-networking-analytics-solution) kroky je nutné postupovat.</span><span class="sxs-lookup"><span data-stu-id="66aa1-127">If you see the **Azure Networking Analytics (deprecated)** solution, refer to [migrating from the old Networking Analytics solution](#migrating-from-the-old-networking-analytics-solution) for steps you need to follow.</span></span>
>
>

## <a name="review-azure-networking-data-collection-details"></a><span data-ttu-id="66aa1-128">Zkontrolujte podrobnosti kolekce dat sítě Azure</span><span class="sxs-lookup"><span data-stu-id="66aa1-128">Review Azure networking data collection details</span></span>
<span data-ttu-id="66aa1-129">Analýza Azure Application Gateway a řešením pro správu analytics skupinu zabezpečení sítě shromažďování protokolů diagnostiky přímo z Azure Application Gateway a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="66aa1-129">The Azure Application Gateway analytics and the Network Security Group analytics management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups.</span></span> <span data-ttu-id="66aa1-130">Není nutné zapsat protokoly do úložiště objektů Blob v Azure a žádný agent je vyžadována pro shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="66aa1-130">It is not necessary to write the logs to Azure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="66aa1-131">Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Azure Application Gateway analýzy a analýzy skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="66aa1-131">The following table shows data collection methods and other details about how data is collected for Azure Application Gateway analytics and the Network Security Group analytics.</span></span>

| <span data-ttu-id="66aa1-132">Platforma</span><span class="sxs-lookup"><span data-stu-id="66aa1-132">Platform</span></span> | <span data-ttu-id="66aa1-133">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="66aa1-133">Direct agent</span></span> | <span data-ttu-id="66aa1-134">Agent systémy Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="66aa1-134">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="66aa1-135">Azure</span><span class="sxs-lookup"><span data-stu-id="66aa1-135">Azure</span></span> | <span data-ttu-id="66aa1-136">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="66aa1-136">Operations Manager required?</span></span> | <span data-ttu-id="66aa1-137">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="66aa1-137">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="66aa1-138">Četnost shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="66aa1-138">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="66aa1-139">Azure</span><span class="sxs-lookup"><span data-stu-id="66aa1-139">Azure</span></span> |  |  |<span data-ttu-id="66aa1-140">&#8226;</span><span class="sxs-lookup"><span data-stu-id="66aa1-140">&#8226;</span></span> |  |  |<span data-ttu-id="66aa1-141">Při zaznamenání</span><span class="sxs-lookup"><span data-stu-id="66aa1-141">when logged</span></span> |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a><span data-ttu-id="66aa1-142">Analýza řešení služby Azure Application Gateway v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="66aa1-142">Azure Application Gateway analytics solution in Log Analytics</span></span>

![Azure Application Gateway Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="66aa1-144">Tyto protokoly jsou podporovány pro Application Gateway:</span><span class="sxs-lookup"><span data-stu-id="66aa1-144">The following logs are supported for Application Gateways:</span></span>

* <span data-ttu-id="66aa1-145">ApplicationGatewayAccessLog</span><span class="sxs-lookup"><span data-stu-id="66aa1-145">ApplicationGatewayAccessLog</span></span>
* <span data-ttu-id="66aa1-146">ApplicationGatewayPerformanceLog</span><span class="sxs-lookup"><span data-stu-id="66aa1-146">ApplicationGatewayPerformanceLog</span></span>
* <span data-ttu-id="66aa1-147">ApplicationGatewayFirewallLog</span><span class="sxs-lookup"><span data-stu-id="66aa1-147">ApplicationGatewayFirewallLog</span></span>

<span data-ttu-id="66aa1-148">Pro Application Gateway se podporují následující metriky:</span><span class="sxs-lookup"><span data-stu-id="66aa1-148">The following metrics are supported for Application Gateways:</span></span>

* <span data-ttu-id="66aa1-149">propustnost 5 minut</span><span class="sxs-lookup"><span data-stu-id="66aa1-149">5 minute throughput</span></span>

### <a name="install-and-configure-the-solution"></a><span data-ttu-id="66aa1-150">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="66aa1-150">Install and configure the solution</span></span>
<span data-ttu-id="66aa1-151">Použijte následující pokyny k instalaci a konfiguraci řešení analýzy Azure Application Gateway:</span><span class="sxs-lookup"><span data-stu-id="66aa1-151">Use the following instructions to install and configure the Azure Application Gateway analytics solution:</span></span>

1. <span data-ttu-id="66aa1-152">Povolit řešení Azure Application Gateway analýzy z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="66aa1-152">Enable the Azure Application Gateway analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="66aa1-153">Povolte protokolování pro diagnostiku [Application Gateway](../application-gateway/application-gateway-diagnostics.md) chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="66aa1-153">Enable diagnostics logging for the [Application Gateways](../application-gateway/application-gateway-diagnostics.md) you want to monitor.</span></span>

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a><span data-ttu-id="66aa1-154">Povolte diagnostiku Azure Application Gateway na portálu</span><span class="sxs-lookup"><span data-stu-id="66aa1-154">Enable Azure Application Gateway diagnostics in the portal</span></span>

1. <span data-ttu-id="66aa1-155">Na portálu Azure přejděte k prostředku aplikační brány k monitorování</span><span class="sxs-lookup"><span data-stu-id="66aa1-155">In the Azure portal, navigate to the Application Gateway resource to monitor</span></span>
2. <span data-ttu-id="66aa1-156">Vyberte *protokolů diagnostiky* otevřete na následující stránce</span><span class="sxs-lookup"><span data-stu-id="66aa1-156">Select *Diagnostics logs* to open the following page</span></span>

   ![bitové kopie prostředku Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. <span data-ttu-id="66aa1-158">Klikněte na tlačítko *zapněte diagnostiku* otevřete na následující stránce</span><span class="sxs-lookup"><span data-stu-id="66aa1-158">Click *Turn on diagnostics* to open the following page</span></span>

   ![bitové kopie prostředku Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. <span data-ttu-id="66aa1-160">Chcete-li zapněte diagnostiku, klikněte na tlačítko *na* pod *stav*</span><span class="sxs-lookup"><span data-stu-id="66aa1-160">To turn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="66aa1-161">Klikněte na zaškrtávací políčko pro *poslat analýzy protokolů*</span><span class="sxs-lookup"><span data-stu-id="66aa1-161">Click the checkbox for *Send to Log Analytics*</span></span>
6. <span data-ttu-id="66aa1-162">Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="66aa1-162">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="66aa1-163">Klikněte na zaškrtávací políčko v části **protokolu** pro každý typ protokolu ke shromažďování</span><span class="sxs-lookup"><span data-stu-id="66aa1-163">Click the checkbox under **Log** for each of the log types to collect</span></span>
8. <span data-ttu-id="66aa1-164">Klikněte na tlačítko *Uložit* povolení protokolování diagnostiky k analýze protokolů</span><span class="sxs-lookup"><span data-stu-id="66aa1-164">Click *Save* to enable the logging of diagnostics to Log Analytics</span></span>

#### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="66aa1-165">Povolte diagnostiku sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="66aa1-165">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="66aa1-166">Následující skript prostředí PowerShell poskytuje příklad toho, jak povolit protokolování pro application Gateway diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="66aa1-166">The following PowerShell script provides an example of how to enable diagnostic logging for application gateways.</span></span>

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a><span data-ttu-id="66aa1-167">Použití Azure Application Gateway analytics</span><span class="sxs-lookup"><span data-stu-id="66aa1-167">Use Azure Application Gateway analytics</span></span>
![Obrázek analytics dlaždici Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

<span data-ttu-id="66aa1-169">Po kliknutí **Azure Application Gateway analytics** dlaždici v přehledu, můžete zobrazit souhrny souborů protokolu a přejít k podrobnostem podrobnostech v těchto kategoriích:</span><span class="sxs-lookup"><span data-stu-id="66aa1-169">After you click the **Azure Application Gateway analytics** tile on the Overview, you can view summaries of your logs and then drill in to details for the following categories:</span></span>

* <span data-ttu-id="66aa1-170">Přístup k aplikaci brány protokolů</span><span class="sxs-lookup"><span data-stu-id="66aa1-170">Application Gateway Access logs</span></span>
  * <span data-ttu-id="66aa1-171">Chyby klienta a serveru pro službu Application Gateway přístup k protokolům</span><span class="sxs-lookup"><span data-stu-id="66aa1-171">Client and server errors for Application Gateway access logs</span></span>
  * <span data-ttu-id="66aa1-172">Počet požadavků za hodinu pro každý Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-172">Requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="66aa1-173">Neúspěšné požadavky za hodinu pro každý Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-173">Failed requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="66aa1-174">Chyby podle uživatelského agenta pro Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-174">Errors by user agent for Application Gateways</span></span>
* <span data-ttu-id="66aa1-175">Výkon brány aplikace</span><span class="sxs-lookup"><span data-stu-id="66aa1-175">Application Gateway performance</span></span>
  * <span data-ttu-id="66aa1-176">Stav hostitele pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-176">Host health for Application Gateway</span></span>
  * <span data-ttu-id="66aa1-177">Maximální a 95. percentil pro službu Application Gateway neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="66aa1-177">Maximum and 95th percentile for Application Gateway failed requests</span></span>

![Obrázek panelu analýzy Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Obrázek panelu analýzy Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

<span data-ttu-id="66aa1-180">Na **analytics Azure Application Gateway** řídicí panel, zkontrolujte souhrnné informace v jednom z okna a pak klikněte na jednu Chcete-li zobrazit podrobné informace na stránce vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="66aa1-180">On the **Azure Application Gateway analytics** dashboard, review the summary information in one of the blades, and then click one to view detailed information on the log search page.</span></span>

<span data-ttu-id="66aa1-181">Na všech stránkách vyhledávání protokolu můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="66aa1-181">On any of the log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="66aa1-182">Můžete také filtrovat podle omezující vlastnosti výsledky upřesněte.</span><span class="sxs-lookup"><span data-stu-id="66aa1-182">You can also filter by facets to narrow the results.</span></span>


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a><span data-ttu-id="66aa1-183">Skupina zabezpečení sítě Azure analytics řešení v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="66aa1-183">Azure Network Security Group analytics solution in Log Analytics</span></span>

![Skupina zabezpečení sítě Azure Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="66aa1-185">Tyto protokoly jsou podporovány pro skupiny zabezpečení sítě:</span><span class="sxs-lookup"><span data-stu-id="66aa1-185">The following logs are supported for network security groups:</span></span>

* <span data-ttu-id="66aa1-186">NetworkSecurityGroupEvent</span><span class="sxs-lookup"><span data-stu-id="66aa1-186">NetworkSecurityGroupEvent</span></span>
* <span data-ttu-id="66aa1-187">NetworkSecurityGroupRuleCounter</span><span class="sxs-lookup"><span data-stu-id="66aa1-187">NetworkSecurityGroupRuleCounter</span></span>

### <a name="install-and-configure-the-solution"></a><span data-ttu-id="66aa1-188">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="66aa1-188">Install and configure the solution</span></span>
<span data-ttu-id="66aa1-189">Použijte následující pokyny k instalaci a konfiguraci řešení analýzy sítě Azure:</span><span class="sxs-lookup"><span data-stu-id="66aa1-189">Use the following instructions to install and configure the Azure Networking Analytics solution:</span></span>

1. <span data-ttu-id="66aa1-190">Povolit řešení analýzy skupinu zabezpečení sítě Azure z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="66aa1-190">Enable the Azure Network Security Group analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="66aa1-191">Povolte protokolování pro diagnostiku [skupinu zabezpečení sítě](../virtual-network/virtual-network-nsg-manage-log.md) prostředky, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="66aa1-191">Enable diagnostics logging for the [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md) resources you want to monitor.</span></span>

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a><span data-ttu-id="66aa1-192">Povolte diagnostiku skupiny zabezpečení sítě Azure na portálu</span><span class="sxs-lookup"><span data-stu-id="66aa1-192">Enable Azure network security group diagnostics in the portal</span></span>

1. <span data-ttu-id="66aa1-193">Na portálu Azure přejděte k prostředku skupinu zabezpečení sítě k monitorování</span><span class="sxs-lookup"><span data-stu-id="66aa1-193">In the Azure portal, navigate to the Network Security Group resource to monitor</span></span>
2. <span data-ttu-id="66aa1-194">Vyberte *protokolů diagnostiky* otevřete na následující stránce</span><span class="sxs-lookup"><span data-stu-id="66aa1-194">Select *Diagnostics logs* to open the following page</span></span>

   ![bitové kopie prostředku skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. <span data-ttu-id="66aa1-196">Klikněte na tlačítko *zapněte diagnostiku* otevřete na následující stránce</span><span class="sxs-lookup"><span data-stu-id="66aa1-196">Click *Turn on diagnostics* to open the following page</span></span>

   ![bitové kopie prostředku skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. <span data-ttu-id="66aa1-198">Chcete-li zapněte diagnostiku, klikněte na tlačítko *na* pod *stav*</span><span class="sxs-lookup"><span data-stu-id="66aa1-198">To turn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="66aa1-199">Klikněte na zaškrtávací políčko pro *poslat analýzy protokolů*</span><span class="sxs-lookup"><span data-stu-id="66aa1-199">Click the checkbox for *Send to Log Analytics*</span></span>
6. <span data-ttu-id="66aa1-200">Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="66aa1-200">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="66aa1-201">Klikněte na zaškrtávací políčko v části **protokolu** pro každý typ protokolu ke shromažďování</span><span class="sxs-lookup"><span data-stu-id="66aa1-201">Click the checkbox under **Log** for each of the log types to collect</span></span>
8. <span data-ttu-id="66aa1-202">Klikněte na tlačítko *Uložit* povolení protokolování diagnostiky k analýze protokolů</span><span class="sxs-lookup"><span data-stu-id="66aa1-202">Click *Save* to enable the logging of diagnostics to Log Analytics</span></span>

### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="66aa1-203">Povolte diagnostiku sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="66aa1-203">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="66aa1-204">Následující skript prostředí PowerShell představuje příklad, jak povolit protokolování pro skupiny zabezpečení sítě diagnostiky</span><span class="sxs-lookup"><span data-stu-id="66aa1-204">The following PowerShell script provides an example of how to enable diagnostic logging for network security groups</span></span>
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a><span data-ttu-id="66aa1-205">Použijte skupinu zabezpečení sítě Azure analytics</span><span class="sxs-lookup"><span data-stu-id="66aa1-205">Use Azure Network Security Group analytics</span></span>
<span data-ttu-id="66aa1-206">Po kliknutí **skupinu zabezpečení sítě Azure analytics** dlaždici v přehledu, můžete zobrazit souhrny souborů protokolu a přejít k podrobnostem podrobnostech v těchto kategoriích:</span><span class="sxs-lookup"><span data-stu-id="66aa1-206">After you click the **Azure Network Security Group analytics** tile on the Overview, you can view summaries of your logs and then drill in to details for the following categories:</span></span>

* <span data-ttu-id="66aa1-207">Skupina zabezpečení sítě blokované toky</span><span class="sxs-lookup"><span data-stu-id="66aa1-207">Network security group blocked flows</span></span>
  * <span data-ttu-id="66aa1-208">Pravidla skupiny zabezpečení sítě s blokované toky</span><span class="sxs-lookup"><span data-stu-id="66aa1-208">Network security group rules with blocked flows</span></span>
  * <span data-ttu-id="66aa1-209">Adresy MAC s blokované toky</span><span class="sxs-lookup"><span data-stu-id="66aa1-209">MAC addresses with blocked flows</span></span>
* <span data-ttu-id="66aa1-210">Skupina zabezpečení sítě povolené toky</span><span class="sxs-lookup"><span data-stu-id="66aa1-210">Network security group allowed flows</span></span>
  * <span data-ttu-id="66aa1-211">Pravidla skupiny zabezpečení sítě s povolenou toky</span><span class="sxs-lookup"><span data-stu-id="66aa1-211">Network security group rules with allowed flows</span></span>
  * <span data-ttu-id="66aa1-212">Adresy MAC s povolenou toky</span><span class="sxs-lookup"><span data-stu-id="66aa1-212">MAC addresses with allowed flows</span></span>

![Obrázek panelu analýzy skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Obrázek panelu analýzy skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

<span data-ttu-id="66aa1-215">Na **skupinu zabezpečení sítě Azure analytics** řídicí panel, zkontrolujte souhrnné informace v jednom z okna a pak klikněte na jednu Chcete-li zobrazit podrobné informace na stránce vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="66aa1-215">On the **Azure Network Security Group analytics** dashboard, review the summary information in one of the blades, and then click one to view detailed information on the log search page.</span></span>

<span data-ttu-id="66aa1-216">Na všech stránkách vyhledávání protokolu můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="66aa1-216">On any of the log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="66aa1-217">Můžete také filtrovat podle omezující vlastnosti výsledky upřesněte.</span><span class="sxs-lookup"><span data-stu-id="66aa1-217">You can also filter by facets to narrow the results.</span></span>

## <a name="migrating-from-the-old-networking-analytics-solution"></a><span data-ttu-id="66aa1-218">Migrace z původního řešení sítě analýzy</span><span class="sxs-lookup"><span data-stu-id="66aa1-218">Migrating from the old Networking Analytics solution</span></span>
<span data-ttu-id="66aa1-219">V ledna 2017 podporované způsob odesílání protokolů z Azure Application Gateway a skupiny zabezpečení sítě Azure k Log Analytics změnil.</span><span class="sxs-lookup"><span data-stu-id="66aa1-219">In January 2017, the supported way of sending logs from Azure Application Gateways and Azure Network Security Groups to Log Analytics changed.</span></span> <span data-ttu-id="66aa1-220">Tyto změny poskytovat následující výhody:</span><span class="sxs-lookup"><span data-stu-id="66aa1-220">These changes provide the following advantages:</span></span>
+ <span data-ttu-id="66aa1-221">Protokoly zapisují přímo k Log Analytics, aniž by bylo nutné použít účet úložiště</span><span class="sxs-lookup"><span data-stu-id="66aa1-221">Logs are written directly to Log Analytics without the need to use a storage account</span></span>
+ <span data-ttu-id="66aa1-222">Menší latenci od času po vygenerování protokoly jim je k dispozici v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="66aa1-222">Less latency from the time when logs are generated to them being available in Log Analytics</span></span>
+ <span data-ttu-id="66aa1-223">Méně kroků konfigurace</span><span class="sxs-lookup"><span data-stu-id="66aa1-223">Fewer configuration steps</span></span>
+ <span data-ttu-id="66aa1-224">Běžný formát pro všechny typy Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="66aa1-224">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="66aa1-225">Použití aktualizované řešení:</span><span class="sxs-lookup"><span data-stu-id="66aa1-225">To use the updated solutions:</span></span>

1. [<span data-ttu-id="66aa1-226">Konfiguraci diagnostiky k odeslání přímo k Log Analytics z Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="66aa1-226">Configure diagnostics to be sent directly to Log Analytics from Azure Application Gateways</span></span>](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [<span data-ttu-id="66aa1-227">Konfiguraci diagnostiky k odeslání přímo k Log Analytics ze skupin zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="66aa1-227">Configure diagnostics to be sent directly to Log Analytics from Azure Network Security Groups</span></span>](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. <span data-ttu-id="66aa1-228">Povolit *Azure Application Gateway Analytics* a *Analytics skupiny zabezpečení sítě Azure* řešení pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="66aa1-228">Enable the *Azure Application Gateway Analytics* and the *Azure Network Security Group Analytics* solution by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="66aa1-229">Aktualizovat žádné uložené dotazy, řídicí panely nebo výstrahy používat nový datový typ.</span><span class="sxs-lookup"><span data-stu-id="66aa1-229">Update any saved queries, dashboards, or alerts to use the new data type</span></span>
  + <span data-ttu-id="66aa1-230">Typ je AzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="66aa1-230">Type is to AzureDiagnostics.</span></span> <span data-ttu-id="66aa1-231">Příkaz ResourceType můžete filtrovat, aby Azure síťových protokolů.</span><span class="sxs-lookup"><span data-stu-id="66aa1-231">You can use the ResourceType to filter to Azure networking logs.</span></span>

    | <span data-ttu-id="66aa1-232">Namísto:</span><span class="sxs-lookup"><span data-stu-id="66aa1-232">Instead of:</span></span> | <span data-ttu-id="66aa1-233">Použití:</span><span class="sxs-lookup"><span data-stu-id="66aa1-233">Use:</span></span> |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + <span data-ttu-id="66aa1-234">Pro každé pole, které má příponu \_s, \_d, nebo \_g v názvu, změna po prvním znaku na malá písmena</span><span class="sxs-lookup"><span data-stu-id="66aa1-234">For any field that has a suffix of \_s, \_d, or \_g in the name, change the first character to lower case</span></span>
   + <span data-ttu-id="66aa1-235">Pro každé pole, které má příponu \_o název, data je rozdělená do jednotlivých polí na základě názvů vnořená pole.</span><span class="sxs-lookup"><span data-stu-id="66aa1-235">For any field that has a suffix of \_o in name, the data is split into individual fields based on the nested field names.</span></span>
4. <span data-ttu-id="66aa1-236">Odeberte *Analytics sítě Azure (nepoužívané)* řešení.</span><span class="sxs-lookup"><span data-stu-id="66aa1-236">Remove the *Azure Networking Analytics (Deprecated)* solution.</span></span>
  + <span data-ttu-id="66aa1-237">Pokud používáte prostředí PowerShell, použijte`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="66aa1-237">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span></span>

<span data-ttu-id="66aa1-238">Data jsou shromažďována předtím, než tato změna není zobrazená v nové řešení.</span><span class="sxs-lookup"><span data-stu-id="66aa1-238">Data collected before the change is not visible in the new solution.</span></span> <span data-ttu-id="66aa1-239">Můžete pokračovat se dotázat na tato data pomocí starého typu a názvy polí.</span><span class="sxs-lookup"><span data-stu-id="66aa1-239">You can continue to query for this data using the old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="66aa1-240">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="66aa1-240">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="66aa1-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66aa1-241">Next steps</span></span>
* <span data-ttu-id="66aa1-242">Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) zobrazíte podrobné Azure diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="66aa1-242">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed Azure diagnostics data.</span></span>
