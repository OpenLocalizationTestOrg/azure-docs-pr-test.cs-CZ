---
title: "aaaAzure řešení sítě analýzy v Log Analytics | Microsoft Docs"
description: "Můžete použít hello řešení Azure sítě analýzy protokolů skupiny zabezpečení sítě Azure tooreview analýzy protokolů a protokoly Azure Application Gateway."
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
ms.openlocfilehash: 3674189786bacccc82e6708e78f14c92178e6676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a><span data-ttu-id="368c8-103">Monitorování řešení v analýzy protokolů Azure sítě</span><span class="sxs-lookup"><span data-stu-id="368c8-103">Azure networking monitoring solutions in Log Analytics</span></span>

<span data-ttu-id="368c8-104">Analýzy protokolů nabízí hello následující řešení pro monitorování vaší sítí:</span><span class="sxs-lookup"><span data-stu-id="368c8-104">Log Analytics offers hello following solutions for monitoring your networks:</span></span>
* <span data-ttu-id="368c8-105">Sledování výkonu sítě (NPM) na</span><span class="sxs-lookup"><span data-stu-id="368c8-105">Network Performance Monitor (NPM) to</span></span>
 * <span data-ttu-id="368c8-106">Sledování stavu hello vaší sítě</span><span class="sxs-lookup"><span data-stu-id="368c8-106">Monitor hello health of your network</span></span>
* <span data-ttu-id="368c8-107">Tooreview analytics Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-107">Azure Application Gateway analytics tooreview</span></span>
 * <span data-ttu-id="368c8-108">Protokoly služby Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-108">Azure Application Gateway logs</span></span>
 * <span data-ttu-id="368c8-109">Metriky Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-109">Azure Application Gateway metrics</span></span>
* <span data-ttu-id="368c8-110">Skupina zabezpečení sítě Azure analytics tooreview</span><span class="sxs-lookup"><span data-stu-id="368c8-110">Azure Network Security Group analytics tooreview</span></span>
 * <span data-ttu-id="368c8-111">Protokoly skupinu zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="368c8-111">Azure Network Security Group logs</span></span>

## <a name="network-performance-monitor-npm"></a><span data-ttu-id="368c8-112">Sledování výkonu sítě (NPM)</span><span class="sxs-lookup"><span data-stu-id="368c8-112">Network Performance Monitor (NPM)</span></span>

<span data-ttu-id="368c8-113">Hello [sledování výkonu sítě](log-analytics-network-performance-monitor.md) řešení správy je monitorování řešení, která sleduje stav hello, dostupnosti a dostupnosti sítě sítě.</span><span class="sxs-lookup"><span data-stu-id="368c8-113">hello [Network Performance Monitor](log-analytics-network-performance-monitor.md) management solution is a network monitoring solution, that monitors hello health, availability and reachability of networks.</span></span>  <span data-ttu-id="368c8-114">Jedná se o použitých toomonitor připojení mezi:</span><span class="sxs-lookup"><span data-stu-id="368c8-114">It is used toomonitor connectivity between:</span></span>

* <span data-ttu-id="368c8-115">veřejný cloud a místní</span><span class="sxs-lookup"><span data-stu-id="368c8-115">Public cloud and on-premises</span></span>
* <span data-ttu-id="368c8-116">datových center a umístění uživatele (firemních pobočkách)</span><span class="sxs-lookup"><span data-stu-id="368c8-116">Data centers and user locations (branch offices)</span></span>
* <span data-ttu-id="368c8-117">podsítě hostování různé úrovně víceúrovňových aplikací.</span><span class="sxs-lookup"><span data-stu-id="368c8-117">Subnets hosting various tiers of a multi-tiered application.</span></span>

<span data-ttu-id="368c8-118">Další informace najdete v tématu [sledování výkonu sítě](log-analytics-network-performance-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="368c8-118">For more information, see [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span></span>

## <a name="azure-application-gateway-and-network-security-group-analytics"></a><span data-ttu-id="368c8-119">Analýza Azure Application Gateway a skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="368c8-119">Azure Application Gateway and Network Security Group analytics</span></span>
<span data-ttu-id="368c8-120">toouse hello řešení:</span><span class="sxs-lookup"><span data-stu-id="368c8-120">toouse hello solutions:</span></span>
1. <span data-ttu-id="368c8-121">Přidat hello správu řešení tooLog analýzy, a</span><span class="sxs-lookup"><span data-stu-id="368c8-121">Add hello management solution tooLog Analytics, and</span></span>
2. <span data-ttu-id="368c8-122">Povolte diagnostiku toodirect hello diagnostiky tooa pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="368c8-122">Enable diagnostics toodirect hello diagnostics tooa Log Analytics workspace.</span></span> <span data-ttu-id="368c8-123">Není nutné toowrite hello protokoly tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="368c8-123">It is not necessary toowrite hello logs tooAzure Blob storage.</span></span>

<span data-ttu-id="368c8-124">Diagnostika a řešení odpovídající hello můžete povolit pro jednoho nebo obou aplikační brány a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="368c8-124">You can enable diagnostics and hello corresponding solution for either one or both of Application Gateway and Networking Security Groups.</span></span>

<span data-ttu-id="368c8-125">Pokud jste nepovolujte protokolování diagnostiky pro konkrétní typ prostředku, ale instalaci hello řešení, hello řídicí panel oken pro tento prostředek jsou prázdné a zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="368c8-125">If you do not enable diagnostic logging for a particular resource type, but install hello solution, hello dashboard blades for that resource are blank and display an error message.</span></span>

> [!NOTE]
> <span data-ttu-id="368c8-126">V ledna 2017 podporované hello způsob odesílání protokolů z tooLog aplikačních bran a skupiny zabezpečení sítě, analýza změnit.</span><span class="sxs-lookup"><span data-stu-id="368c8-126">In January 2017, hello supported way of sending logs from Application Gateways and Network Security Groups tooLog Analytics changed.</span></span> <span data-ttu-id="368c8-127">Pokud se zobrazí hello **Azure sítě Analytics (nepoužívané)** řešení, najdete příliš[migrace z řešení sítě analýzy staré hello](#migrating-from-the-old-networking-analytics-solution) kroky musíte toofollow.</span><span class="sxs-lookup"><span data-stu-id="368c8-127">If you see hello **Azure Networking Analytics (deprecated)** solution, refer too[migrating from hello old Networking Analytics solution](#migrating-from-the-old-networking-analytics-solution) for steps you need toofollow.</span></span>
>
>

## <a name="review-azure-networking-data-collection-details"></a><span data-ttu-id="368c8-128">Zkontrolujte podrobnosti kolekce dat sítě Azure</span><span class="sxs-lookup"><span data-stu-id="368c8-128">Review Azure networking data collection details</span></span>
<span data-ttu-id="368c8-129">Hello Azure Application Gateway analýzy a řešení pro správu hello skupinu zabezpečení sítě analytics shromažďování protokolů diagnostiky přímo z Azure Application Gateway a skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="368c8-129">hello Azure Application Gateway analytics and hello Network Security Group analytics management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups.</span></span> <span data-ttu-id="368c8-130">Není nutné toowrite hello protokoly tooAzure úložiště objektů Blob a žádný agent je vyžadována pro shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="368c8-130">It is not necessary toowrite hello logs tooAzure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="368c8-131">Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Azure Application Gateway analýzy a analýzy hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="368c8-131">hello following table shows data collection methods and other details about how data is collected for Azure Application Gateway analytics and hello Network Security Group analytics.</span></span>

| <span data-ttu-id="368c8-132">Platforma</span><span class="sxs-lookup"><span data-stu-id="368c8-132">Platform</span></span> | <span data-ttu-id="368c8-133">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="368c8-133">Direct agent</span></span> | <span data-ttu-id="368c8-134">Agent systémy Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="368c8-134">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="368c8-135">Azure</span><span class="sxs-lookup"><span data-stu-id="368c8-135">Azure</span></span> | <span data-ttu-id="368c8-136">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="368c8-136">Operations Manager required?</span></span> | <span data-ttu-id="368c8-137">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="368c8-137">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="368c8-138">Četnost shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="368c8-138">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="368c8-139">Azure</span><span class="sxs-lookup"><span data-stu-id="368c8-139">Azure</span></span> |  |  |<span data-ttu-id="368c8-140">&#8226;</span><span class="sxs-lookup"><span data-stu-id="368c8-140">&#8226;</span></span> |  |  |<span data-ttu-id="368c8-141">Při zaznamenání</span><span class="sxs-lookup"><span data-stu-id="368c8-141">when logged</span></span> |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a><span data-ttu-id="368c8-142">Analýza řešení služby Azure Application Gateway v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="368c8-142">Azure Application Gateway analytics solution in Log Analytics</span></span>

![Azure Application Gateway Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="368c8-144">Hello následující protokoly jsou podporovány pro Application Gateway:</span><span class="sxs-lookup"><span data-stu-id="368c8-144">hello following logs are supported for Application Gateways:</span></span>

* <span data-ttu-id="368c8-145">ApplicationGatewayAccessLog</span><span class="sxs-lookup"><span data-stu-id="368c8-145">ApplicationGatewayAccessLog</span></span>
* <span data-ttu-id="368c8-146">ApplicationGatewayPerformanceLog</span><span class="sxs-lookup"><span data-stu-id="368c8-146">ApplicationGatewayPerformanceLog</span></span>
* <span data-ttu-id="368c8-147">ApplicationGatewayFirewallLog</span><span class="sxs-lookup"><span data-stu-id="368c8-147">ApplicationGatewayFirewallLog</span></span>

<span data-ttu-id="368c8-148">pro Application Gateway se podporují Hello následující metriky:</span><span class="sxs-lookup"><span data-stu-id="368c8-148">hello following metrics are supported for Application Gateways:</span></span>

* <span data-ttu-id="368c8-149">propustnost 5 minut</span><span class="sxs-lookup"><span data-stu-id="368c8-149">5 minute throughput</span></span>

### <a name="install-and-configure-hello-solution"></a><span data-ttu-id="368c8-150">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="368c8-150">Install and configure hello solution</span></span>
<span data-ttu-id="368c8-151">Použijte následující pokyny tooinstall hello a řešení analytics Azure Application Gateway hello nakonfigurovat:</span><span class="sxs-lookup"><span data-stu-id="368c8-151">Use hello following instructions tooinstall and configure hello Azure Application Gateway analytics solution:</span></span>

1. <span data-ttu-id="368c8-152">Povolit řešení analýzy Azure Application Gateway hello z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="368c8-152">Enable hello Azure Application Gateway analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="368c8-153">Zapnutí protokolování diagnostiky hello [Application Gateway](../application-gateway/application-gateway-diagnostics.md) chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="368c8-153">Enable diagnostics logging for hello [Application Gateways](../application-gateway/application-gateway-diagnostics.md) you want toomonitor.</span></span>

#### <a name="enable-azure-application-gateway-diagnostics-in-hello-portal"></a><span data-ttu-id="368c8-154">Povolte diagnostiku Azure Application Gateway hello portálu</span><span class="sxs-lookup"><span data-stu-id="368c8-154">Enable Azure Application Gateway diagnostics in hello portal</span></span>

1. <span data-ttu-id="368c8-155">V hello portálu Azure přejděte toomonitor prostředků toohello Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-155">In hello Azure portal, navigate toohello Application Gateway resource toomonitor</span></span>
2. <span data-ttu-id="368c8-156">Vyberte *protokolů diagnostiky* tooopen hello následující stránky</span><span class="sxs-lookup"><span data-stu-id="368c8-156">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![bitové kopie prostředku Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. <span data-ttu-id="368c8-158">Klikněte na tlačítko *zapněte diagnostiku* tooopen hello následující stránky</span><span class="sxs-lookup"><span data-stu-id="368c8-158">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![bitové kopie prostředku Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. <span data-ttu-id="368c8-160">Klikněte na tlačítko tooturn na Diagnostika, *na* pod *stav*</span><span class="sxs-lookup"><span data-stu-id="368c8-160">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="368c8-161">Klikněte na políčko hello *odeslat tooLog Analytics*</span><span class="sxs-lookup"><span data-stu-id="368c8-161">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="368c8-162">Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="368c8-162">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="368c8-163">Klikněte na zaškrtávací políčko hello pod **protokolu** pro každou toocollect typy protokolu hello</span><span class="sxs-lookup"><span data-stu-id="368c8-163">Click hello checkbox under **Log** for each of hello log types toocollect</span></span>
8. <span data-ttu-id="368c8-164">Klikněte na tlačítko *Uložit* tooenable hello protokolování diagnostiky tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="368c8-164">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

#### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="368c8-165">Povolte diagnostiku sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="368c8-165">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="368c8-166">Hello následující skript prostředí PowerShell představuje příklad, jak tooenable diagnostické protokolování pro application Gateway.</span><span class="sxs-lookup"><span data-stu-id="368c8-166">hello following PowerShell script provides an example of how tooenable diagnostic logging for application gateways.</span></span>

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a><span data-ttu-id="368c8-167">Použití Azure Application Gateway analytics</span><span class="sxs-lookup"><span data-stu-id="368c8-167">Use Azure Application Gateway analytics</span></span>
![Obrázek analytics dlaždici Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

<span data-ttu-id="368c8-169">Po kliknutí na tlačítko hello **Azure Application Gateway analytics** dlaždici na hello přehled, můžete zobrazení souhrnných informací o protokoly a potom přejít k podrobnostem toodetails pro hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="368c8-169">After you click hello **Azure Application Gateway analytics** tile on hello Overview, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="368c8-170">Přístup k aplikaci brány protokolů</span><span class="sxs-lookup"><span data-stu-id="368c8-170">Application Gateway Access logs</span></span>
  * <span data-ttu-id="368c8-171">Chyby klienta a serveru pro službu Application Gateway přístup k protokolům</span><span class="sxs-lookup"><span data-stu-id="368c8-171">Client and server errors for Application Gateway access logs</span></span>
  * <span data-ttu-id="368c8-172">Počet požadavků za hodinu pro každý Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-172">Requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="368c8-173">Neúspěšné požadavky za hodinu pro každý Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-173">Failed requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="368c8-174">Chyby podle uživatelského agenta pro Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-174">Errors by user agent for Application Gateways</span></span>
* <span data-ttu-id="368c8-175">Výkon brány aplikace</span><span class="sxs-lookup"><span data-stu-id="368c8-175">Application Gateway performance</span></span>
  * <span data-ttu-id="368c8-176">Stav hostitele pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-176">Host health for Application Gateway</span></span>
  * <span data-ttu-id="368c8-177">Maximální a 95. percentil pro službu Application Gateway neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="368c8-177">Maximum and 95th percentile for Application Gateway failed requests</span></span>

![Obrázek panelu analýzy Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Obrázek panelu analýzy Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

<span data-ttu-id="368c8-180">Na hello **Azure Application Gateway analytics** řídicí panel, zkontrolujte souhrnné informace hello v jednom z okna hello a pak klikněte na jednu tooview podrobné informace na stránce vyhledávání protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="368c8-180">On hello **Azure Application Gateway analytics** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information on hello log search page.</span></span>

<span data-ttu-id="368c8-181">Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="368c8-181">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="368c8-182">Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="368c8-182">You can also filter by facets toonarrow hello results.</span></span>


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a><span data-ttu-id="368c8-183">Skupina zabezpečení sítě Azure analytics řešení v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="368c8-183">Azure Network Security Group analytics solution in Log Analytics</span></span>

![Skupina zabezpečení sítě Azure Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="368c8-185">pro skupiny zabezpečení sítě jsou podporovány následující protokoly Hello:</span><span class="sxs-lookup"><span data-stu-id="368c8-185">hello following logs are supported for network security groups:</span></span>

* <span data-ttu-id="368c8-186">NetworkSecurityGroupEvent</span><span class="sxs-lookup"><span data-stu-id="368c8-186">NetworkSecurityGroupEvent</span></span>
* <span data-ttu-id="368c8-187">NetworkSecurityGroupRuleCounter</span><span class="sxs-lookup"><span data-stu-id="368c8-187">NetworkSecurityGroupRuleCounter</span></span>

### <a name="install-and-configure-hello-solution"></a><span data-ttu-id="368c8-188">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="368c8-188">Install and configure hello solution</span></span>
<span data-ttu-id="368c8-189">Použijte následující pokyny tooinstall hello a nakonfigurujte řešení hello analýzy sítě Azure:</span><span class="sxs-lookup"><span data-stu-id="368c8-189">Use hello following instructions tooinstall and configure hello Azure Networking Analytics solution:</span></span>

1. <span data-ttu-id="368c8-190">Povolit řešení analýzy hello skupinu zabezpečení sítě Azure z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="368c8-190">Enable hello Azure Network Security Group analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="368c8-191">Zapnutí protokolování diagnostiky hello [skupinu zabezpečení sítě](../virtual-network/virtual-network-nsg-manage-log.md) zdroje, které má toomonitor.</span><span class="sxs-lookup"><span data-stu-id="368c8-191">Enable diagnostics logging for hello [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md) resources you want toomonitor.</span></span>

### <a name="enable-azure-network-security-group-diagnostics-in-hello-portal"></a><span data-ttu-id="368c8-192">Povolte diagnostiku skupiny zabezpečení sítě Azure hello portálu</span><span class="sxs-lookup"><span data-stu-id="368c8-192">Enable Azure network security group diagnostics in hello portal</span></span>

1. <span data-ttu-id="368c8-193">V hello portálu Azure přejděte toomonitor prostředků toohello skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="368c8-193">In hello Azure portal, navigate toohello Network Security Group resource toomonitor</span></span>
2. <span data-ttu-id="368c8-194">Vyberte *protokolů diagnostiky* tooopen hello následující stránky</span><span class="sxs-lookup"><span data-stu-id="368c8-194">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![bitové kopie prostředku skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. <span data-ttu-id="368c8-196">Klikněte na tlačítko *zapněte diagnostiku* tooopen hello následující stránky</span><span class="sxs-lookup"><span data-stu-id="368c8-196">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![bitové kopie prostředku skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. <span data-ttu-id="368c8-198">Klikněte na tlačítko tooturn na Diagnostika, *na* pod *stav*</span><span class="sxs-lookup"><span data-stu-id="368c8-198">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="368c8-199">Klikněte na políčko hello *odeslat tooLog Analytics*</span><span class="sxs-lookup"><span data-stu-id="368c8-199">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="368c8-200">Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="368c8-200">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="368c8-201">Klikněte na zaškrtávací políčko hello pod **protokolu** pro každou toocollect typy protokolu hello</span><span class="sxs-lookup"><span data-stu-id="368c8-201">Click hello checkbox under **Log** for each of hello log types toocollect</span></span>
8. <span data-ttu-id="368c8-202">Klikněte na tlačítko *Uložit* tooenable hello protokolování diagnostiky tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="368c8-202">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="368c8-203">Povolte diagnostiku sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="368c8-203">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="368c8-204">Hello následující skript prostředí PowerShell představuje příklad, jak tooenable diagnostické protokolování pro skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="368c8-204">hello following PowerShell script provides an example of how tooenable diagnostic logging for network security groups</span></span>
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a><span data-ttu-id="368c8-205">Použijte skupinu zabezpečení sítě Azure analytics</span><span class="sxs-lookup"><span data-stu-id="368c8-205">Use Azure Network Security Group analytics</span></span>
<span data-ttu-id="368c8-206">Po kliknutí na tlačítko hello **skupinu zabezpečení sítě Azure analytics** dlaždici na hello přehled, můžete zobrazení souhrnných informací o protokoly a potom přejít k podrobnostem toodetails pro hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="368c8-206">After you click hello **Azure Network Security Group analytics** tile on hello Overview, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="368c8-207">Skupina zabezpečení sítě blokované toky</span><span class="sxs-lookup"><span data-stu-id="368c8-207">Network security group blocked flows</span></span>
  * <span data-ttu-id="368c8-208">Pravidla skupiny zabezpečení sítě s blokované toky</span><span class="sxs-lookup"><span data-stu-id="368c8-208">Network security group rules with blocked flows</span></span>
  * <span data-ttu-id="368c8-209">Adresy MAC s blokované toky</span><span class="sxs-lookup"><span data-stu-id="368c8-209">MAC addresses with blocked flows</span></span>
* <span data-ttu-id="368c8-210">Skupina zabezpečení sítě povolené toky</span><span class="sxs-lookup"><span data-stu-id="368c8-210">Network security group allowed flows</span></span>
  * <span data-ttu-id="368c8-211">Pravidla skupiny zabezpečení sítě s povolenou toky</span><span class="sxs-lookup"><span data-stu-id="368c8-211">Network security group rules with allowed flows</span></span>
  * <span data-ttu-id="368c8-212">Adresy MAC s povolenou toky</span><span class="sxs-lookup"><span data-stu-id="368c8-212">MAC addresses with allowed flows</span></span>

![Obrázek panelu analýzy skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Obrázek panelu analýzy skupinu zabezpečení sítě Azure](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

<span data-ttu-id="368c8-215">Na hello **skupinu zabezpečení sítě Azure analytics** řídicí panel, zkontrolujte souhrnné informace hello v jednom z okna hello a pak klikněte na jednu tooview podrobné informace na stránce vyhledávání protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="368c8-215">On hello **Azure Network Security Group analytics** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information on hello log search page.</span></span>

<span data-ttu-id="368c8-216">Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="368c8-216">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="368c8-217">Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="368c8-217">You can also filter by facets toonarrow hello results.</span></span>

## <a name="migrating-from-hello-old-networking-analytics-solution"></a><span data-ttu-id="368c8-218">Migrace z řešení sítě analýzy staré hello</span><span class="sxs-lookup"><span data-stu-id="368c8-218">Migrating from hello old Networking Analytics solution</span></span>
<span data-ttu-id="368c8-219">V ledna 2017 podporované hello způsob odesílání protokolů z Azure Application Gateway a skupiny zabezpečení sítě Azure tooLog Analytics se změnila.</span><span class="sxs-lookup"><span data-stu-id="368c8-219">In January 2017, hello supported way of sending logs from Azure Application Gateways and Azure Network Security Groups tooLog Analytics changed.</span></span> <span data-ttu-id="368c8-220">Tyto změny poskytují hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="368c8-220">These changes provide hello following advantages:</span></span>
+ <span data-ttu-id="368c8-221">Protokoly zapisují přímo tooLog Analytics bez hello potřebovat toouse účet úložiště</span><span class="sxs-lookup"><span data-stu-id="368c8-221">Logs are written directly tooLog Analytics without hello need toouse a storage account</span></span>
+ <span data-ttu-id="368c8-222">Menší latenci hello čase, kdy protokoly generované toothem, který je k dispozici v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="368c8-222">Less latency from hello time when logs are generated toothem being available in Log Analytics</span></span>
+ <span data-ttu-id="368c8-223">Méně kroků konfigurace</span><span class="sxs-lookup"><span data-stu-id="368c8-223">Fewer configuration steps</span></span>
+ <span data-ttu-id="368c8-224">Běžný formát pro všechny typy Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="368c8-224">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="368c8-225">toouse hello aktualizovat řešení:</span><span class="sxs-lookup"><span data-stu-id="368c8-225">toouse hello updated solutions:</span></span>

1. [<span data-ttu-id="368c8-226">Konfigurace diagnostiky toobe odeslaných tooLog Analytics přímo z Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="368c8-226">Configure diagnostics toobe sent directly tooLog Analytics from Azure Application Gateways</span></span>](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [<span data-ttu-id="368c8-227">Konfigurace diagnostiky toobe odeslaný přímo tooLog Analytics skupin zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="368c8-227">Configure diagnostics toobe sent directly tooLog Analytics from Azure Network Security Groups</span></span>](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. <span data-ttu-id="368c8-228">Povolit hello *Azure Application Gateway Analytics* a hello *Analytics skupiny zabezpečení sítě Azure* řešení pomocí procesu hello popsané v [řešení přidat analýzy protokolů z Hello Galerie řešení](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="368c8-228">Enable hello *Azure Application Gateway Analytics* and hello *Azure Network Security Group Analytics* solution by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="368c8-229">Aktualizovat žádné uložené dotazy, řídicí panely nebo výstrahy toouse hello nový datový typ.</span><span class="sxs-lookup"><span data-stu-id="368c8-229">Update any saved queries, dashboards, or alerts toouse hello new data type</span></span>
  + <span data-ttu-id="368c8-230">Typ je tooAzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="368c8-230">Type is tooAzureDiagnostics.</span></span> <span data-ttu-id="368c8-231">Můžete použít hello ResourceType toofilter tooAzure síťových protokolů.</span><span class="sxs-lookup"><span data-stu-id="368c8-231">You can use hello ResourceType toofilter tooAzure networking logs.</span></span>

    | <span data-ttu-id="368c8-232">Namísto:</span><span class="sxs-lookup"><span data-stu-id="368c8-232">Instead of:</span></span> | <span data-ttu-id="368c8-233">Použití:</span><span class="sxs-lookup"><span data-stu-id="368c8-233">Use:</span></span> |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + <span data-ttu-id="368c8-234">Pro každé pole, které má příponu \_s, \_d, nebo \_g v hello názvu, změna hello první znak toolower případu</span><span class="sxs-lookup"><span data-stu-id="368c8-234">For any field that has a suffix of \_s, \_d, or \_g in hello name, change hello first character toolower case</span></span>
   + <span data-ttu-id="368c8-235">Pro každé pole, které má příponu \_o název, hello dat je rozdělená do jednotlivých polí podle hello vnořené názvy polí.</span><span class="sxs-lookup"><span data-stu-id="368c8-235">For any field that has a suffix of \_o in name, hello data is split into individual fields based on hello nested field names.</span></span>
4. <span data-ttu-id="368c8-236">Odebrat hello *Analytics sítě Azure (nepoužívané)* řešení.</span><span class="sxs-lookup"><span data-stu-id="368c8-236">Remove hello *Azure Networking Analytics (Deprecated)* solution.</span></span>
  + <span data-ttu-id="368c8-237">Pokud používáte prostředí PowerShell, použijte`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="368c8-237">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span></span>

<span data-ttu-id="368c8-238">Data jsou shromažďována před hello změn není zobrazená v nové řešení hello.</span><span class="sxs-lookup"><span data-stu-id="368c8-238">Data collected before hello change is not visible in hello new solution.</span></span> <span data-ttu-id="368c8-239">Můžete dál tooquery pro tato data pomocí hello starého typu a názvy polí.</span><span class="sxs-lookup"><span data-stu-id="368c8-239">You can continue tooquery for this data using hello old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="368c8-240">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="368c8-240">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="368c8-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="368c8-241">Next steps</span></span>
* <span data-ttu-id="368c8-242">Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobné Azure diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="368c8-242">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Azure diagnostics data.</span></span>
