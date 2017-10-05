---
title: "Azure Key Vault řešení v Log Analytics | Microsoft Docs"
description: "Řešení Azure Key Vault v analýzy protokolů můžete použít ke kontrole protokoly Azure Key Vault."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 651586e0846ffb22a23e64b73c2cc614980d9b92
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a><span data-ttu-id="f6a06-103">Azure Key Vault Analytics řešení v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="f6a06-103">Azure Key Vault Analytics solution in Log Analytics</span></span>

![Symbol Key Vault](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

<span data-ttu-id="f6a06-105">Řešení Azure Key Vault v Log Analytics můžete využít ke kontrole protokolů AuditEvent služby Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f6a06-105">You can use the Azure Key Vault solution in Log Analytics to review Azure Key Vault AuditEvent logs.</span></span>

<span data-ttu-id="f6a06-106">Pokud chcete používat řešení, musíte povolit protokolování diagnostiky Azure Key Vault a přímé diagnostiku do pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f6a06-106">To use the solution, you need to enable logging of Azure Key Vault diagnostics and direct the diagnostics to a Log Analytics workspace.</span></span> <span data-ttu-id="f6a06-107">Není nutné zapsat protokoly do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="f6a06-107">It is not necessary to write the logs to Azure Blob storage.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a06-108">V ledna 2017 podporované způsob odesílání protokolů z trezoru klíč k Log Analytics změnil.</span><span class="sxs-lookup"><span data-stu-id="f6a06-108">In January 2017, the supported way of sending logs from Key Vault to Log Analytics changed.</span></span> <span data-ttu-id="f6a06-109">Je-li řešení Key Vault, kterou používáte ukazuje *(zastaralé)* v názvu se označují [migrace z původního řešení Key Vault](#migrating-from-the-old-key-vault-solution) muset postupovat podle kroků.</span><span class="sxs-lookup"><span data-stu-id="f6a06-109">If the Key Vault solution you are using shows *(deprecated)* in the title, refer to [migrating from the old Key Vault solution](#migrating-from-the-old-key-vault-solution) for steps you need to follow.</span></span>
>
>

## <a name="install-and-configure-the-solution"></a><span data-ttu-id="f6a06-110">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="f6a06-110">Install and configure the solution</span></span>
<span data-ttu-id="f6a06-111">Použijte následující pokyny k instalaci a konfiguraci řešení Azure Key Vault:</span><span class="sxs-lookup"><span data-stu-id="f6a06-111">Use the following instructions to install and configure the Azure Key Vault solution:</span></span>

1. <span data-ttu-id="f6a06-112">Povolit řešení Azure Key Vault z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="f6a06-112">Enable the Azure Key Vault solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="f6a06-113">Povolit protokolování diagnostiky pro Key Vault prostředky. Pokud chcete monitorovat, buď pomocí [portál](#enable-key-vault-diagnostics-in-the-portal) nebo [prostředí PowerShell](#enable-key-vault-diagnostics-using-powershell)</span><span class="sxs-lookup"><span data-stu-id="f6a06-113">Enable diagnostics logging for the Key Vault resources to monitor, using either the [portal](#enable-key-vault-diagnostics-in-the-portal) or [PowerShell](#enable-key-vault-diagnostics-using-powershell)</span></span>

### <a name="enable-key-vault-diagnostics-in-the-portal"></a><span data-ttu-id="f6a06-114">Povolte diagnostiku Key Vault na portálu</span><span class="sxs-lookup"><span data-stu-id="f6a06-114">Enable Key Vault diagnostics in the portal</span></span>

1. <span data-ttu-id="f6a06-115">Na portálu Azure přejděte k prostředku Key Vault k monitorování</span><span class="sxs-lookup"><span data-stu-id="f6a06-115">In the Azure portal, navigate to the Key Vault resource to monitor</span></span>
2. <span data-ttu-id="f6a06-116">Vyberte *protokolů diagnostiky* otevřete na následující stránce</span><span class="sxs-lookup"><span data-stu-id="f6a06-116">Select *Diagnostics logs* to open the following page</span></span>

   ![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. <span data-ttu-id="f6a06-118">Klikněte na tlačítko *zapněte diagnostiku* otevřete na následující stránce</span><span class="sxs-lookup"><span data-stu-id="f6a06-118">Click *Turn on diagnostics* to open the following page</span></span>

   ![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. <span data-ttu-id="f6a06-120">Chcete-li zapněte diagnostiku, klikněte na tlačítko *na* pod *stav*</span><span class="sxs-lookup"><span data-stu-id="f6a06-120">To turn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="f6a06-121">Klikněte na zaškrtávací políčko pro *poslat analýzy protokolů*</span><span class="sxs-lookup"><span data-stu-id="f6a06-121">Click the checkbox for *Send to Log Analytics*</span></span>
6. <span data-ttu-id="f6a06-122">Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="f6a06-122">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="f6a06-123">Chcete-li povolit *AuditEvent* protokoly, klikněte na zaškrtávací políčko v protokolu</span><span class="sxs-lookup"><span data-stu-id="f6a06-123">To enable *AuditEvent* logs, click the checkbox under Log</span></span>
8. <span data-ttu-id="f6a06-124">Klikněte na tlačítko *Uložit* povolení protokolování diagnostiky k analýze protokolů</span><span class="sxs-lookup"><span data-stu-id="f6a06-124">Click *Save* to enable the logging of diagnostics to Log Analytics</span></span>

### <a name="enable-key-vault-diagnostics-using-powershell"></a><span data-ttu-id="f6a06-125">Povolit Key Vault diagnostiku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6a06-125">Enable Key Vault diagnostics using PowerShell</span></span>
<span data-ttu-id="f6a06-126">Následující skript prostředí PowerShell představuje příklad, jak pomocí `Set-AzureRmDiagnosticSetting` povolení diagnostických protokolování pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="f6a06-126">The following PowerShell script provides an example of how to use `Set-AzureRmDiagnosticSetting` to enable diagnostic logging for Key Vault:</span></span>
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a><span data-ttu-id="f6a06-127">Zkontrolujte podrobnosti kolekce dat Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f6a06-127">Review Azure Key Vault data collection details</span></span>
<span data-ttu-id="f6a06-128">Řešení Azure Key Vault shromažďuje diagnostické protokoly přímo z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="f6a06-128">Azure Key Vault solution collects diagnostics logs directly from the Key Vault.</span></span>
<span data-ttu-id="f6a06-129">Není nutné zapsat protokoly do úložiště objektů Blob v Azure a žádný agent je vyžadována pro shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="f6a06-129">It is not necessary to write the logs to Azure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="f6a06-130">Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se údaje pro Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f6a06-130">The following table shows data collection methods and other details about how data is collected for Azure Key Vault.</span></span>

| <span data-ttu-id="f6a06-131">Platforma</span><span class="sxs-lookup"><span data-stu-id="f6a06-131">Platform</span></span> | <span data-ttu-id="f6a06-132">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="f6a06-132">Direct agent</span></span> | <span data-ttu-id="f6a06-133">Agent systémy Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="f6a06-133">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="f6a06-134">Azure</span><span class="sxs-lookup"><span data-stu-id="f6a06-134">Azure</span></span> | <span data-ttu-id="f6a06-135">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="f6a06-135">Operations Manager required?</span></span> | <span data-ttu-id="f6a06-136">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="f6a06-136">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="f6a06-137">Četnost shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="f6a06-137">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f6a06-138">Azure</span><span class="sxs-lookup"><span data-stu-id="f6a06-138">Azure</span></span> |  |  |<span data-ttu-id="f6a06-139">&#8226;</span><span class="sxs-lookup"><span data-stu-id="f6a06-139">&#8226;</span></span> |  |  | <span data-ttu-id="f6a06-140">v případě přijetí</span><span class="sxs-lookup"><span data-stu-id="f6a06-140">on arrival</span></span> |

## <a name="use-azure-key-vault"></a><span data-ttu-id="f6a06-141">Použití Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f6a06-141">Use Azure Key Vault</span></span>
<span data-ttu-id="f6a06-142">Po můžete [nainstalovat řešení](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), zobrazit data Key Vault kliknutím **Azure Key Vault** dlaždice z **přehled** stránky analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="f6a06-142">After you [install the solution](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), view the Key Vault data by clicking the **Azure Key Vault** tile from the **Overview** page of Log Analytics.</span></span>

![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

<span data-ttu-id="f6a06-144">Po kliknutí **přehled** dlaždici, můžete zobrazit souhrny souborů protokolu a pak přejdete podrobnostech v těchto kategoriích:</span><span class="sxs-lookup"><span data-stu-id="f6a06-144">After you click the **Overview** tile, you can view summaries of your logs and then drill in to details for the following categories:</span></span>

* <span data-ttu-id="f6a06-145">Objem všechny operace trezoru klíčů v čase</span><span class="sxs-lookup"><span data-stu-id="f6a06-145">Volume of all key vault operations over time</span></span>
* <span data-ttu-id="f6a06-146">Se nezdařila operace svazky v čase</span><span class="sxs-lookup"><span data-stu-id="f6a06-146">Failed operation volumes over time</span></span>
* <span data-ttu-id="f6a06-147">Průměrná latence provozní operací</span><span class="sxs-lookup"><span data-stu-id="f6a06-147">Average operational latency by operation</span></span>
* <span data-ttu-id="f6a06-148">Kvality služeb pro operace s počet operací, které trvat víc než 1000 ms a seznam operací, které trvat víc než 1000 ms</span><span class="sxs-lookup"><span data-stu-id="f6a06-148">Quality of service for operations with the number of operations that take more than 1000 ms and a list of operations that take more than 1000 ms</span></span>

![Obrázek řídicí panel Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Obrázek řídicí panel Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a><span data-ttu-id="f6a06-151">Chcete-li zobrazit podrobnosti pro všechny operace</span><span class="sxs-lookup"><span data-stu-id="f6a06-151">To view details for any operation</span></span>
1. <span data-ttu-id="f6a06-152">Na **přehled** klikněte na tlačítko **Azure Key Vault** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="f6a06-152">On the **Overview** page, click the **Azure Key Vault** tile.</span></span>
2. <span data-ttu-id="f6a06-153">Na **Azure Key Vault** řídicí panel, zkontrolujte souhrnné informace v jednom z okna a pak klikněte na jednu k zobrazení podrobných informací o něm na stránce vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="f6a06-153">On the **Azure Key Vault** dashboard, review the summary information in one of the blades, and then click one to view detailed information about it in the log search page.</span></span>

    <span data-ttu-id="f6a06-154">Na všech stránkách vyhledávání protokolu můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="f6a06-154">On any of the log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="f6a06-155">Můžete také filtrovat podle omezující vlastnosti výsledky upřesněte.</span><span class="sxs-lookup"><span data-stu-id="f6a06-155">You can also filter by facets to narrow the results.</span></span>

## <a name="log-analytics-records"></a><span data-ttu-id="f6a06-156">Záznamy služby Log Analytics</span><span class="sxs-lookup"><span data-stu-id="f6a06-156">Log Analytics records</span></span>
<span data-ttu-id="f6a06-157">Řešení Azure Key Vault analyzuje záznamy, které mají typ **KeyVaults** shromážděná z [AuditEvent protokoly](../key-vault/key-vault-logging.md) v Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="f6a06-157">The Azure Key Vault solution analyzes records that have a type of **KeyVaults** that are collected from [AuditEvent logs](../key-vault/key-vault-logging.md) in Azure Diagnostics.</span></span>  <span data-ttu-id="f6a06-158">Vlastnosti pro tyto záznamy jsou v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="f6a06-158">Properties for these records are in the following table:</span></span>  

| <span data-ttu-id="f6a06-159">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f6a06-159">Property</span></span> | <span data-ttu-id="f6a06-160">Popis</span><span class="sxs-lookup"><span data-stu-id="f6a06-160">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="f6a06-161">Typ</span><span class="sxs-lookup"><span data-stu-id="f6a06-161">Type</span></span> |<span data-ttu-id="f6a06-162">*AzureDiagnostics*</span><span class="sxs-lookup"><span data-stu-id="f6a06-162">*AzureDiagnostics*</span></span> |
| <span data-ttu-id="f6a06-163">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="f6a06-163">SourceSystem</span></span> |<span data-ttu-id="f6a06-164">*Azure*</span><span class="sxs-lookup"><span data-stu-id="f6a06-164">*Azure*</span></span> |
| <span data-ttu-id="f6a06-165">CallerIpAddress</span><span class="sxs-lookup"><span data-stu-id="f6a06-165">CallerIpAddress</span></span> |<span data-ttu-id="f6a06-166">IP adresa klienta, který vytvořil požadavek</span><span class="sxs-lookup"><span data-stu-id="f6a06-166">IP address of the client who made the request</span></span> |
| <span data-ttu-id="f6a06-167">Kategorie</span><span class="sxs-lookup"><span data-stu-id="f6a06-167">Category</span></span> | <span data-ttu-id="f6a06-168">*AuditEvent*</span><span class="sxs-lookup"><span data-stu-id="f6a06-168">*AuditEvent*</span></span> |
| <span data-ttu-id="f6a06-169">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="f6a06-169">CorrelationId</span></span> |<span data-ttu-id="f6a06-170">Volitelný GUID, který může klient předat pro korelaci protokolů na straně klienta s protokoly na straně služby (Key Vault).</span><span class="sxs-lookup"><span data-stu-id="f6a06-170">An optional GUID that the client can pass to correlate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="f6a06-171">DurationMs</span><span class="sxs-lookup"><span data-stu-id="f6a06-171">DurationMs</span></span> |<span data-ttu-id="f6a06-172">Doba trvání obsloužení požadavku REST API v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="f6a06-172">Time it took to service the REST API request, in milliseconds.</span></span> <span data-ttu-id="f6a06-173">Tentokrát nezahrnuje latenci sítě, takže čas, který budete vyhodnocovat na straně klienta se může lišit.</span><span class="sxs-lookup"><span data-stu-id="f6a06-173">This time does not include network latency, so the time that you measure on the client side might not match this time.</span></span> |
| <span data-ttu-id="f6a06-174">httpStatusCode_d</span><span class="sxs-lookup"><span data-stu-id="f6a06-174">httpStatusCode_d</span></span> |<span data-ttu-id="f6a06-175">Stavový kód HTTP vrácených požadavkem (například *200*)</span><span class="sxs-lookup"><span data-stu-id="f6a06-175">HTTP status code returned by the request (for example, *200*)</span></span> |
| <span data-ttu-id="f6a06-176">id_s</span><span class="sxs-lookup"><span data-stu-id="f6a06-176">id_s</span></span> |<span data-ttu-id="f6a06-177">Jedinečné ID požadavku</span><span class="sxs-lookup"><span data-stu-id="f6a06-177">Unique ID of the request</span></span> |
| <span data-ttu-id="f6a06-178">identity_claim_appid_g</span><span class="sxs-lookup"><span data-stu-id="f6a06-178">identity_claim_appid_g</span></span> | <span data-ttu-id="f6a06-179">Identifikátor GUID pro id aplikace</span><span class="sxs-lookup"><span data-stu-id="f6a06-179">GUID for the application id</span></span> |
| <span data-ttu-id="f6a06-180">OperationName</span><span class="sxs-lookup"><span data-stu-id="f6a06-180">OperationName</span></span> |<span data-ttu-id="f6a06-181">Název operace, jak je uvedeno v [protokolování Azure Key Vault](../key-vault/key-vault-logging.md)</span><span class="sxs-lookup"><span data-stu-id="f6a06-181">Name of the operation, as documented in [Azure Key Vault Logging](../key-vault/key-vault-logging.md)</span></span> |
| <span data-ttu-id="f6a06-182">OperationVersion</span><span class="sxs-lookup"><span data-stu-id="f6a06-182">OperationVersion</span></span> |<span data-ttu-id="f6a06-183">Verze rozhraní REST API požadovaná klientem (například *2015-06-01*)</span><span class="sxs-lookup"><span data-stu-id="f6a06-183">REST API version requested by the client (for example *2015-06-01*)</span></span> |
| <span data-ttu-id="f6a06-184">requestUri_s</span><span class="sxs-lookup"><span data-stu-id="f6a06-184">requestUri_s</span></span> |<span data-ttu-id="f6a06-185">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="f6a06-185">Uri of the request</span></span> |
| <span data-ttu-id="f6a06-186">Prostředek</span><span class="sxs-lookup"><span data-stu-id="f6a06-186">Resource</span></span> |<span data-ttu-id="f6a06-187">Název trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="f6a06-187">Name of the key vault</span></span> |
| <span data-ttu-id="f6a06-188">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f6a06-188">ResourceGroup</span></span> |<span data-ttu-id="f6a06-189">Skupina prostředků služby key vault</span><span class="sxs-lookup"><span data-stu-id="f6a06-189">Resource group of the key vault</span></span> |
| <span data-ttu-id="f6a06-190">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="f6a06-190">ResourceId</span></span> |<span data-ttu-id="f6a06-191">ID prostředku Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f6a06-191">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="f6a06-192">Pro protokoly Key Vault to je ID prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f6a06-192">For Key Vault logs, this is the Key Vault resource ID.</span></span> |
| <span data-ttu-id="f6a06-193">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="f6a06-193">ResourceProvider</span></span> |<span data-ttu-id="f6a06-194">*SPOLEČNOSTI MICROSOFT. KEYVAULT*</span><span class="sxs-lookup"><span data-stu-id="f6a06-194">*MICROSOFT.KEYVAULT*</span></span> |
| <span data-ttu-id="f6a06-195">ResourceType</span><span class="sxs-lookup"><span data-stu-id="f6a06-195">ResourceType</span></span> | <span data-ttu-id="f6a06-196">*TREZORY*</span><span class="sxs-lookup"><span data-stu-id="f6a06-196">*VAULTS*</span></span> |
| <span data-ttu-id="f6a06-197">ResultSignature</span><span class="sxs-lookup"><span data-stu-id="f6a06-197">ResultSignature</span></span> |<span data-ttu-id="f6a06-198">Stav protokolu HTTP (například *OK*)</span><span class="sxs-lookup"><span data-stu-id="f6a06-198">HTTP status (for example, *OK*)</span></span> |
| <span data-ttu-id="f6a06-199">ResultType</span><span class="sxs-lookup"><span data-stu-id="f6a06-199">ResultType</span></span> |<span data-ttu-id="f6a06-200">Výsledek požadavku REST API (například *úspěch*)</span><span class="sxs-lookup"><span data-stu-id="f6a06-200">Result of REST API request (for example, *Success*)</span></span> |
| <span data-ttu-id="f6a06-201">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="f6a06-201">SubscriptionId</span></span> |<span data-ttu-id="f6a06-202">ID předplatného Azure předplatného obsahující Key Vault</span><span class="sxs-lookup"><span data-stu-id="f6a06-202">Azure subscription ID of the subscription containing the Key Vault</span></span> |

## <a name="migrating-from-the-old-key-vault-solution"></a><span data-ttu-id="f6a06-203">Migrace z původního řešení Key Vault</span><span class="sxs-lookup"><span data-stu-id="f6a06-203">Migrating from the old Key Vault solution</span></span>
<span data-ttu-id="f6a06-204">V ledna 2017 podporované způsob odesílání protokolů z trezoru klíč k Log Analytics změnil.</span><span class="sxs-lookup"><span data-stu-id="f6a06-204">In January 2017, the supported way of sending logs from Key Vault to Log Analytics changed.</span></span> <span data-ttu-id="f6a06-205">Tyto změny poskytovat následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f6a06-205">These changes provide the following advantages:</span></span>
+ <span data-ttu-id="f6a06-206">Protokoly zapisují přímo k Log Analytics, aniž by bylo nutné použít účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f6a06-206">Logs are written directly to Log Analytics without the need to use a storage account</span></span>
+ <span data-ttu-id="f6a06-207">Menší latenci od času po vygenerování protokoly jim je k dispozici v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="f6a06-207">Less latency from the time when logs are generated to them being available in Log Analytics</span></span>
+ <span data-ttu-id="f6a06-208">Méně kroků konfigurace</span><span class="sxs-lookup"><span data-stu-id="f6a06-208">Fewer configuration steps</span></span>
+ <span data-ttu-id="f6a06-209">Běžný formát pro všechny typy Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="f6a06-209">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="f6a06-210">Chcete-li použít aktualizované řešení:</span><span class="sxs-lookup"><span data-stu-id="f6a06-210">To use the updated solution:</span></span>

1. [<span data-ttu-id="f6a06-211">Konfiguraci diagnostiky k odeslání přímo k Log Analytics z Key Vault</span><span class="sxs-lookup"><span data-stu-id="f6a06-211">Configure diagnostics to be sent directly to Log Analytics from Key Vault</span></span>](#enable-key-vault-diagnostics-in-the-portal)  
2. <span data-ttu-id="f6a06-212">Povolení Azure Key Vault řešení pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="f6a06-212">Enable the Azure Key Vault solution by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="f6a06-213">Aktualizovat žádné uložené dotazy, řídicí panely nebo výstrahy používat nový datový typ.</span><span class="sxs-lookup"><span data-stu-id="f6a06-213">Update any saved queries, dashboards, or alerts to use the new data type</span></span>
  + <span data-ttu-id="f6a06-214">Typ je změna z: KeyVaults k AzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="f6a06-214">Type is change from: KeyVaults to AzureDiagnostics.</span></span> <span data-ttu-id="f6a06-215">Příkaz ResourceType můžete filtrovat, aby protokoly Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f6a06-215">You can use the ResourceType to filter to Key Vault Logs.</span></span>
  - <span data-ttu-id="f6a06-216">Místo: `Type=KeyVaults`, použijte`Type=AzureDiagnostics ResourceType=VAULTS`</span><span class="sxs-lookup"><span data-stu-id="f6a06-216">Instead of: `Type=KeyVaults`, use `Type=AzureDiagnostics ResourceType=VAULTS`</span></span>
  + <span data-ttu-id="f6a06-217">Pole: (názvy polí jsou malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="f6a06-217">Fields: (Field names are case-sensitive)</span></span>
  - <span data-ttu-id="f6a06-218">Pro každé pole, které má příponu \_s, \_d, nebo \_g v názvu, změna po prvním znaku na malá písmena</span><span class="sxs-lookup"><span data-stu-id="f6a06-218">For any field that has a suffix of \_s, \_d, or \_g in the name, change the first character to lower case</span></span>
  - <span data-ttu-id="f6a06-219">Pro každé pole, které má příponu \_o název, data je rozdělená do jednotlivých polí na základě názvů vnořená pole.</span><span class="sxs-lookup"><span data-stu-id="f6a06-219">For any field that has a suffix of \_o in name, the data is split into individual fields based on the nested field names.</span></span> <span data-ttu-id="f6a06-220">Například hlavní název uživatele volajícího je uložený v poli`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span><span class="sxs-lookup"><span data-stu-id="f6a06-220">For example, the UPN of the caller is stored in a field `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span></span>
   - <span data-ttu-id="f6a06-221">Pole CallerIpAddress změnit tak, aby CallerIPAddress</span><span class="sxs-lookup"><span data-stu-id="f6a06-221">Field CallerIpAddress changed to CallerIPAddress</span></span>
   - <span data-ttu-id="f6a06-222">Pole RemoteIPCountry je již k dispozici</span><span class="sxs-lookup"><span data-stu-id="f6a06-222">Field RemoteIPCountry is no longer present</span></span>
4. <span data-ttu-id="f6a06-223">Odeberte *klíč trezoru Analytics (nepoužívané)* řešení.</span><span class="sxs-lookup"><span data-stu-id="f6a06-223">Remove the *Key Vault Analytics (Deprecated)* solution.</span></span> <span data-ttu-id="f6a06-224">Pokud používáte prostředí PowerShell, použijte`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="f6a06-224">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span></span>

<span data-ttu-id="f6a06-225">Data jsou shromažďována předtím, než tato změna není zobrazená v nové řešení.</span><span class="sxs-lookup"><span data-stu-id="f6a06-225">Data collected before the change is not visible in the new solution.</span></span> <span data-ttu-id="f6a06-226">Můžete pokračovat se dotázat na tato data pomocí starého typu a názvy polí.</span><span class="sxs-lookup"><span data-stu-id="f6a06-226">You can continue to query for this data using the old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f6a06-227">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f6a06-227">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="f6a06-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6a06-228">Next steps</span></span>
* <span data-ttu-id="f6a06-229">Použití [hledání přihlásit analýzy protokolů](log-analytics-log-searches.md) na podrobnější data Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f6a06-229">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed Azure Key Vault data.</span></span>
