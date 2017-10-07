---
title: "aaaAzure Key Vault řešení v Log Analytics | Microsoft Docs"
description: "Řešení Azure Key Vault hello můžete použít v tooreview analýzy protokolů, protokoly Azure Key Vault."
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
ms.openlocfilehash: 1c6eae26ded7ad55b0159a3be09cdc9901596298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a><span data-ttu-id="2c493-103">Azure Key Vault Analytics řešení v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="2c493-103">Azure Key Vault Analytics solution in Log Analytics</span></span>

![Symbol Key Vault](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

<span data-ttu-id="2c493-105">V tooreview analýzy protokolů, Azure Key Vault AuditEvent protokolů můžete použít hello Azure Key Vault řešení.</span><span class="sxs-lookup"><span data-stu-id="2c493-105">You can use hello Azure Key Vault solution in Log Analytics tooreview Azure Key Vault AuditEvent logs.</span></span>

<span data-ttu-id="2c493-106">toouse hello řešení, musíte tooenable protokolování Azure Key Vault diagnostiky a přímé hello pracovní prostor analýzy protokolů tooa diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="2c493-106">toouse hello solution, you need tooenable logging of Azure Key Vault diagnostics and direct hello diagnostics tooa Log Analytics workspace.</span></span> <span data-ttu-id="2c493-107">Není nutné toowrite hello protokoly tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="2c493-107">It is not necessary toowrite hello logs tooAzure Blob storage.</span></span>

> [!NOTE]
> <span data-ttu-id="2c493-108">V ledna 2017 podporované hello způsob odesílání protokolů z tooLog Key Vault, analýzy se změnila.</span><span class="sxs-lookup"><span data-stu-id="2c493-108">In January 2017, hello supported way of sending logs from Key Vault tooLog Analytics changed.</span></span> <span data-ttu-id="2c493-109">Pokud používáte hello Key Vault řešení zobrazuje *(nepoužívané)* v hello název označují příliš[migrace z řešení Key Vault staré hello](#migrating-from-the-old-key-vault-solution) kroky musíte toofollow.</span><span class="sxs-lookup"><span data-stu-id="2c493-109">If hello Key Vault solution you are using shows *(deprecated)* in hello title, refer too[migrating from hello old Key Vault solution](#migrating-from-the-old-key-vault-solution) for steps you need toofollow.</span></span>
>
>

## <a name="install-and-configure-hello-solution"></a><span data-ttu-id="2c493-110">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="2c493-110">Install and configure hello solution</span></span>
<span data-ttu-id="2c493-111">Použijte následující pokyny tooinstall hello a konfigurovat řešení Azure Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="2c493-111">Use hello following instructions tooinstall and configure hello Azure Key Vault solution:</span></span>

1. <span data-ttu-id="2c493-112">Povolit hello řešení Azure Key Vault z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="2c493-112">Enable hello Azure Key Vault solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="2c493-113">Povolte diagnostiku protokolování pro toomonitor prostředky hello Key Vault, pomocí buď hello [portál](#enable-key-vault-diagnostics-in-the-portal) nebo [prostředí PowerShell](#enable-key-vault-diagnostics-using-powershell)</span><span class="sxs-lookup"><span data-stu-id="2c493-113">Enable diagnostics logging for hello Key Vault resources toomonitor, using either hello [portal](#enable-key-vault-diagnostics-in-the-portal) or [PowerShell](#enable-key-vault-diagnostics-using-powershell)</span></span>

### <a name="enable-key-vault-diagnostics-in-hello-portal"></a><span data-ttu-id="2c493-114">Povolte diagnostiku Key Vault hello portálu</span><span class="sxs-lookup"><span data-stu-id="2c493-114">Enable Key Vault diagnostics in hello portal</span></span>

1. <span data-ttu-id="2c493-115">V hello portálu Azure přejděte toomonitor prostředků toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c493-115">In hello Azure portal, navigate toohello Key Vault resource toomonitor</span></span>
2. <span data-ttu-id="2c493-116">Vyberte *protokolů diagnostiky* tooopen hello následující stránky</span><span class="sxs-lookup"><span data-stu-id="2c493-116">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. <span data-ttu-id="2c493-118">Klikněte na tlačítko *zapněte diagnostiku* tooopen hello následující stránky</span><span class="sxs-lookup"><span data-stu-id="2c493-118">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. <span data-ttu-id="2c493-120">Klikněte na tlačítko tooturn na Diagnostika, *na* pod *stav*</span><span class="sxs-lookup"><span data-stu-id="2c493-120">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="2c493-121">Klikněte na políčko hello *odeslat tooLog Analytics*</span><span class="sxs-lookup"><span data-stu-id="2c493-121">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="2c493-122">Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="2c493-122">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="2c493-123">tooenable *AuditEvent* protokoly, klikněte na zaškrtávací políčko hello v protokolu</span><span class="sxs-lookup"><span data-stu-id="2c493-123">tooenable *AuditEvent* logs, click hello checkbox under Log</span></span>
8. <span data-ttu-id="2c493-124">Klikněte na tlačítko *Uložit* tooenable hello protokolování diagnostiky tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="2c493-124">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

### <a name="enable-key-vault-diagnostics-using-powershell"></a><span data-ttu-id="2c493-125">Povolit Key Vault diagnostiku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c493-125">Enable Key Vault diagnostics using PowerShell</span></span>
<span data-ttu-id="2c493-126">Hello následující skript prostředí PowerShell představuje příklad, jak toouse `Set-AzureRmDiagnosticSetting` tooenable diagnostické protokolování pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="2c493-126">hello following PowerShell script provides an example of how toouse `Set-AzureRmDiagnosticSetting` tooenable diagnostic logging for Key Vault:</span></span>
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a><span data-ttu-id="2c493-127">Zkontrolujte podrobnosti kolekce dat Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c493-127">Review Azure Key Vault data collection details</span></span>
<span data-ttu-id="2c493-128">Řešení Azure Key Vault shromažďuje diagnostické protokoly přímo ze hello Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2c493-128">Azure Key Vault solution collects diagnostics logs directly from hello Key Vault.</span></span>
<span data-ttu-id="2c493-129">Není nutné toowrite hello protokoly tooAzure úložiště objektů Blob a žádný agent je vyžadována pro shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="2c493-129">It is not necessary toowrite hello logs tooAzure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="2c493-130">Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se údaje pro Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2c493-130">hello following table shows data collection methods and other details about how data is collected for Azure Key Vault.</span></span>

| <span data-ttu-id="2c493-131">Platforma</span><span class="sxs-lookup"><span data-stu-id="2c493-131">Platform</span></span> | <span data-ttu-id="2c493-132">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="2c493-132">Direct agent</span></span> | <span data-ttu-id="2c493-133">Agent systémy Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="2c493-133">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="2c493-134">Azure</span><span class="sxs-lookup"><span data-stu-id="2c493-134">Azure</span></span> | <span data-ttu-id="2c493-135">Nástroj Operations Manager vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="2c493-135">Operations Manager required?</span></span> | <span data-ttu-id="2c493-136">Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="2c493-136">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="2c493-137">Četnost shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="2c493-137">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="2c493-138">Azure</span><span class="sxs-lookup"><span data-stu-id="2c493-138">Azure</span></span> |  |  |<span data-ttu-id="2c493-139">&#8226;</span><span class="sxs-lookup"><span data-stu-id="2c493-139">&#8226;</span></span> |  |  | <span data-ttu-id="2c493-140">v případě přijetí</span><span class="sxs-lookup"><span data-stu-id="2c493-140">on arrival</span></span> |

## <a name="use-azure-key-vault"></a><span data-ttu-id="2c493-141">Použití Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c493-141">Use Azure Key Vault</span></span>
<span data-ttu-id="2c493-142">Po jste [instalaci hello řešení](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), zobrazení dat Key Vault hello kliknutím hello **Azure Key Vault** dlaždice z hello **přehled** stránky analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="2c493-142">After you [install hello solution](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), view hello Key Vault data by clicking hello **Azure Key Vault** tile from hello **Overview** page of Log Analytics.</span></span>

![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

<span data-ttu-id="2c493-144">Po kliknutí na tlačítko hello **přehled** dlaždice, můžete zobrazit souhrny protokolů a pak zobrazit v toodetails pro hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="2c493-144">After you click hello **Overview** tile, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="2c493-145">Objem všechny operace trezoru klíčů v čase</span><span class="sxs-lookup"><span data-stu-id="2c493-145">Volume of all key vault operations over time</span></span>
* <span data-ttu-id="2c493-146">Se nezdařila operace svazky v čase</span><span class="sxs-lookup"><span data-stu-id="2c493-146">Failed operation volumes over time</span></span>
* <span data-ttu-id="2c493-147">Průměrná latence provozní operací</span><span class="sxs-lookup"><span data-stu-id="2c493-147">Average operational latency by operation</span></span>
* <span data-ttu-id="2c493-148">Kvality služeb pro operace s hello počet operací, které trvat víc než 1000 ms a seznam operací, které trvat víc než 1000 ms</span><span class="sxs-lookup"><span data-stu-id="2c493-148">Quality of service for operations with hello number of operations that take more than 1000 ms and a list of operations that take more than 1000 ms</span></span>

![Obrázek řídicí panel Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Obrázek řídicí panel Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="tooview-details-for-any-operation"></a><span data-ttu-id="2c493-151">Podrobnosti o tooview pro všechny operace</span><span class="sxs-lookup"><span data-stu-id="2c493-151">tooview details for any operation</span></span>
1. <span data-ttu-id="2c493-152">Na hello **přehled** klikněte na tlačítko hello **Azure Key Vault** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="2c493-152">On hello **Overview** page, click hello **Azure Key Vault** tile.</span></span>
2. <span data-ttu-id="2c493-153">Na hello **Azure Key Vault** řídicí panel, zkontrolujte souhrnné informace hello v jednom z okna hello a pak klikněte na jednu tooview podrobné informace o něm hello protokolu vyhledávání stránce.</span><span class="sxs-lookup"><span data-stu-id="2c493-153">On hello **Azure Key Vault** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information about it in hello log search page.</span></span>

    <span data-ttu-id="2c493-154">Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="2c493-154">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="2c493-155">Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2c493-155">You can also filter by facets toonarrow hello results.</span></span>

## <a name="log-analytics-records"></a><span data-ttu-id="2c493-156">Záznamy služby Log Analytics</span><span class="sxs-lookup"><span data-stu-id="2c493-156">Log Analytics records</span></span>
<span data-ttu-id="2c493-157">Hello Azure Key Vault řešení analyzuje záznamy, které mají typ **KeyVaults** shromážděná z [AuditEvent protokoly](../key-vault/key-vault-logging.md) v Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="2c493-157">hello Azure Key Vault solution analyzes records that have a type of **KeyVaults** that are collected from [AuditEvent logs](../key-vault/key-vault-logging.md) in Azure Diagnostics.</span></span>  <span data-ttu-id="2c493-158">Vlastnosti pro tyto záznamy jsou v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="2c493-158">Properties for these records are in hello following table:</span></span>  

| <span data-ttu-id="2c493-159">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2c493-159">Property</span></span> | <span data-ttu-id="2c493-160">Popis</span><span class="sxs-lookup"><span data-stu-id="2c493-160">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2c493-161">Typ</span><span class="sxs-lookup"><span data-stu-id="2c493-161">Type</span></span> |<span data-ttu-id="2c493-162">*AzureDiagnostics*</span><span class="sxs-lookup"><span data-stu-id="2c493-162">*AzureDiagnostics*</span></span> |
| <span data-ttu-id="2c493-163">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="2c493-163">SourceSystem</span></span> |<span data-ttu-id="2c493-164">*Azure*</span><span class="sxs-lookup"><span data-stu-id="2c493-164">*Azure*</span></span> |
| <span data-ttu-id="2c493-165">CallerIpAddress</span><span class="sxs-lookup"><span data-stu-id="2c493-165">CallerIpAddress</span></span> |<span data-ttu-id="2c493-166">IP adresa klienta hello, který vytvořil požadavek hello</span><span class="sxs-lookup"><span data-stu-id="2c493-166">IP address of hello client who made hello request</span></span> |
| <span data-ttu-id="2c493-167">Kategorie</span><span class="sxs-lookup"><span data-stu-id="2c493-167">Category</span></span> | <span data-ttu-id="2c493-168">*AuditEvent*</span><span class="sxs-lookup"><span data-stu-id="2c493-168">*AuditEvent*</span></span> |
| <span data-ttu-id="2c493-169">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="2c493-169">CorrelationId</span></span> |<span data-ttu-id="2c493-170">Volitelný GUID, který hello klienta můžete předat toocorrelate protokoly na straně klienta s protokoly na straně služby (Key Vault).</span><span class="sxs-lookup"><span data-stu-id="2c493-170">An optional GUID that hello client can pass toocorrelate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="2c493-171">DurationMs</span><span class="sxs-lookup"><span data-stu-id="2c493-171">DurationMs</span></span> |<span data-ttu-id="2c493-172">Čas, kdy trvalo požadavku REST API hello tooservice, v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="2c493-172">Time it took tooservice hello REST API request, in milliseconds.</span></span> <span data-ttu-id="2c493-173">Tentokrát nezahrnuje latenci sítě, takže hello čas, který budete vyhodnocovat na straně klienta hello se může lišit.</span><span class="sxs-lookup"><span data-stu-id="2c493-173">This time does not include network latency, so hello time that you measure on hello client side might not match this time.</span></span> |
| <span data-ttu-id="2c493-174">httpStatusCode_d</span><span class="sxs-lookup"><span data-stu-id="2c493-174">httpStatusCode_d</span></span> |<span data-ttu-id="2c493-175">Vrácený požadavek hello stavový kód HTTP (například *200*)</span><span class="sxs-lookup"><span data-stu-id="2c493-175">HTTP status code returned by hello request (for example, *200*)</span></span> |
| <span data-ttu-id="2c493-176">id_s</span><span class="sxs-lookup"><span data-stu-id="2c493-176">id_s</span></span> |<span data-ttu-id="2c493-177">Jedinečné ID požadavku hello</span><span class="sxs-lookup"><span data-stu-id="2c493-177">Unique ID of hello request</span></span> |
| <span data-ttu-id="2c493-178">identity_claim_appid_g</span><span class="sxs-lookup"><span data-stu-id="2c493-178">identity_claim_appid_g</span></span> | <span data-ttu-id="2c493-179">Identifikátor GUID pro id aplikace hello</span><span class="sxs-lookup"><span data-stu-id="2c493-179">GUID for hello application id</span></span> |
| <span data-ttu-id="2c493-180">OperationName</span><span class="sxs-lookup"><span data-stu-id="2c493-180">OperationName</span></span> |<span data-ttu-id="2c493-181">Název hello operace, jak je uvedeno v [protokolování Azure Key Vault](../key-vault/key-vault-logging.md)</span><span class="sxs-lookup"><span data-stu-id="2c493-181">Name of hello operation, as documented in [Azure Key Vault Logging](../key-vault/key-vault-logging.md)</span></span> |
| <span data-ttu-id="2c493-182">OperationVersion</span><span class="sxs-lookup"><span data-stu-id="2c493-182">OperationVersion</span></span> |<span data-ttu-id="2c493-183">Verze rozhraní REST API požadovaná klientem hello (například *2015-06-01*)</span><span class="sxs-lookup"><span data-stu-id="2c493-183">REST API version requested by hello client (for example *2015-06-01*)</span></span> |
| <span data-ttu-id="2c493-184">requestUri_s</span><span class="sxs-lookup"><span data-stu-id="2c493-184">requestUri_s</span></span> |<span data-ttu-id="2c493-185">Identifikátor URI požadavku hello</span><span class="sxs-lookup"><span data-stu-id="2c493-185">Uri of hello request</span></span> |
| <span data-ttu-id="2c493-186">Prostředek</span><span class="sxs-lookup"><span data-stu-id="2c493-186">Resource</span></span> |<span data-ttu-id="2c493-187">Název trezoru klíčů hello</span><span class="sxs-lookup"><span data-stu-id="2c493-187">Name of hello key vault</span></span> |
| <span data-ttu-id="2c493-188">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2c493-188">ResourceGroup</span></span> |<span data-ttu-id="2c493-189">Skupiny prostředků hello trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="2c493-189">Resource group of hello key vault</span></span> |
| <span data-ttu-id="2c493-190">ID prostředku</span><span class="sxs-lookup"><span data-stu-id="2c493-190">ResourceId</span></span> |<span data-ttu-id="2c493-191">ID prostředku Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2c493-191">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="2c493-192">Pro protokoly Key Vault to je ID prostředku Key Vault hello</span><span class="sxs-lookup"><span data-stu-id="2c493-192">For Key Vault logs, this is hello Key Vault resource ID.</span></span> |
| <span data-ttu-id="2c493-193">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="2c493-193">ResourceProvider</span></span> |<span data-ttu-id="2c493-194">*SPOLEČNOSTI MICROSOFT. KEYVAULT*</span><span class="sxs-lookup"><span data-stu-id="2c493-194">*MICROSOFT.KEYVAULT*</span></span> |
| <span data-ttu-id="2c493-195">ResourceType</span><span class="sxs-lookup"><span data-stu-id="2c493-195">ResourceType</span></span> | <span data-ttu-id="2c493-196">*TREZORY*</span><span class="sxs-lookup"><span data-stu-id="2c493-196">*VAULTS*</span></span> |
| <span data-ttu-id="2c493-197">ResultSignature</span><span class="sxs-lookup"><span data-stu-id="2c493-197">ResultSignature</span></span> |<span data-ttu-id="2c493-198">Stav protokolu HTTP (například *OK*)</span><span class="sxs-lookup"><span data-stu-id="2c493-198">HTTP status (for example, *OK*)</span></span> |
| <span data-ttu-id="2c493-199">ResultType</span><span class="sxs-lookup"><span data-stu-id="2c493-199">ResultType</span></span> |<span data-ttu-id="2c493-200">Výsledek požadavku REST API (například *úspěch*)</span><span class="sxs-lookup"><span data-stu-id="2c493-200">Result of REST API request (for example, *Success*)</span></span> |
| <span data-ttu-id="2c493-201">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="2c493-201">SubscriptionId</span></span> |<span data-ttu-id="2c493-202">ID předplatného Azure hello předplatného obsahující hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c493-202">Azure subscription ID of hello subscription containing hello Key Vault</span></span> |

## <a name="migrating-from-hello-old-key-vault-solution"></a><span data-ttu-id="2c493-203">Migrace z řešení Key Vault staré hello</span><span class="sxs-lookup"><span data-stu-id="2c493-203">Migrating from hello old Key Vault solution</span></span>
<span data-ttu-id="2c493-204">V ledna 2017 podporované hello způsob odesílání protokolů z tooLog Key Vault, analýzy se změnila.</span><span class="sxs-lookup"><span data-stu-id="2c493-204">In January 2017, hello supported way of sending logs from Key Vault tooLog Analytics changed.</span></span> <span data-ttu-id="2c493-205">Tyto změny poskytují hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2c493-205">These changes provide hello following advantages:</span></span>
+ <span data-ttu-id="2c493-206">Protokoly zapisují přímo tooLog Analytics bez hello potřebovat toouse účet úložiště</span><span class="sxs-lookup"><span data-stu-id="2c493-206">Logs are written directly tooLog Analytics without hello need toouse a storage account</span></span>
+ <span data-ttu-id="2c493-207">Menší latenci hello čase, kdy protokoly generované toothem, který je k dispozici v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="2c493-207">Less latency from hello time when logs are generated toothem being available in Log Analytics</span></span>
+ <span data-ttu-id="2c493-208">Méně kroků konfigurace</span><span class="sxs-lookup"><span data-stu-id="2c493-208">Fewer configuration steps</span></span>
+ <span data-ttu-id="2c493-209">Běžný formát pro všechny typy Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="2c493-209">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="2c493-210">toouse hello aktualizovat řešení:</span><span class="sxs-lookup"><span data-stu-id="2c493-210">toouse hello updated solution:</span></span>

1. [<span data-ttu-id="2c493-211">Konfigurace diagnostiky toobe odeslaný přímo tooLog Analytics Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c493-211">Configure diagnostics toobe sent directly tooLog Analytics from Key Vault</span></span>](#enable-key-vault-diagnostics-in-the-portal)  
2. <span data-ttu-id="2c493-212">Povolení hello Azure Key Vault řešení pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello Galerie řešení](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="2c493-212">Enable hello Azure Key Vault solution by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="2c493-213">Aktualizovat žádné uložené dotazy, řídicí panely nebo výstrahy toouse hello nový datový typ.</span><span class="sxs-lookup"><span data-stu-id="2c493-213">Update any saved queries, dashboards, or alerts toouse hello new data type</span></span>
  + <span data-ttu-id="2c493-214">Typ je změna z: KeyVaults tooAzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="2c493-214">Type is change from: KeyVaults tooAzureDiagnostics.</span></span> <span data-ttu-id="2c493-215">Můžete použít hello ResourceType toofilter tooKey protokoly trezoru.</span><span class="sxs-lookup"><span data-stu-id="2c493-215">You can use hello ResourceType toofilter tooKey Vault Logs.</span></span>
  - <span data-ttu-id="2c493-216">Místo: `Type=KeyVaults`, použijte`Type=AzureDiagnostics ResourceType=VAULTS`</span><span class="sxs-lookup"><span data-stu-id="2c493-216">Instead of: `Type=KeyVaults`, use `Type=AzureDiagnostics ResourceType=VAULTS`</span></span>
  + <span data-ttu-id="2c493-217">Pole: (názvy polí jsou malá a velká písmena)</span><span class="sxs-lookup"><span data-stu-id="2c493-217">Fields: (Field names are case-sensitive)</span></span>
  - <span data-ttu-id="2c493-218">Pro každé pole, které má příponu \_s, \_d, nebo \_g v hello názvu, změna hello první znak toolower případu</span><span class="sxs-lookup"><span data-stu-id="2c493-218">For any field that has a suffix of \_s, \_d, or \_g in hello name, change hello first character toolower case</span></span>
  - <span data-ttu-id="2c493-219">Pro každé pole, které má příponu \_o název, hello dat je rozdělená do jednotlivých polí podle hello vnořené názvy polí.</span><span class="sxs-lookup"><span data-stu-id="2c493-219">For any field that has a suffix of \_o in name, hello data is split into individual fields based on hello nested field names.</span></span> <span data-ttu-id="2c493-220">Například hello UPN volající hello je uložený v poli`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span><span class="sxs-lookup"><span data-stu-id="2c493-220">For example, hello UPN of hello caller is stored in a field `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span></span>
   - <span data-ttu-id="2c493-221">Pole CallerIpAddress změnit tooCallerIPAddress</span><span class="sxs-lookup"><span data-stu-id="2c493-221">Field CallerIpAddress changed tooCallerIPAddress</span></span>
   - <span data-ttu-id="2c493-222">Pole RemoteIPCountry je již k dispozici</span><span class="sxs-lookup"><span data-stu-id="2c493-222">Field RemoteIPCountry is no longer present</span></span>
4. <span data-ttu-id="2c493-223">Odebrat hello *klíč trezoru Analytics (nepoužívané)* řešení.</span><span class="sxs-lookup"><span data-stu-id="2c493-223">Remove hello *Key Vault Analytics (Deprecated)* solution.</span></span> <span data-ttu-id="2c493-224">Pokud používáte prostředí PowerShell, použijte`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="2c493-224">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span></span>

<span data-ttu-id="2c493-225">Data jsou shromažďována před hello změn není zobrazená v nové řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2c493-225">Data collected before hello change is not visible in hello new solution.</span></span> <span data-ttu-id="2c493-226">Můžete dál tooquery pro tato data pomocí hello starého typu a názvy polí.</span><span class="sxs-lookup"><span data-stu-id="2c493-226">You can continue tooquery for this data using hello old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="2c493-227">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="2c493-227">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="2c493-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c493-228">Next steps</span></span>
* <span data-ttu-id="2c493-229">Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobná data Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2c493-229">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Azure Key Vault data.</span></span>
