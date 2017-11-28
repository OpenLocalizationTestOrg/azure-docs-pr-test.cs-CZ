---
title: "Analýza aaaLog Azure CDN | Microsoft Docs"
description: "Zákazníka můžete povolit analýzy protokolů pro Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 56e5a4fec46fd156cf38252732afb4522741d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a><span data-ttu-id="3985d-103">Diagnostické protokoly pro Azure CDN</span><span class="sxs-lookup"><span data-stu-id="3985d-103">Diagnostics Logs for Azure CDN</span></span>

<span data-ttu-id="3985d-104">Když povolíte CDN pro vaši aplikaci, bude pravděpodobně chcete toomonitor hello CDN využití, zkontrolujte stav hello vaší doručení a potenciální potíže.</span><span class="sxs-lookup"><span data-stu-id="3985d-104">After enabling CDN for your application, you will likely want toomonitor hello CDN usage, check hello health of your delivery, and troubleshoot potential issues.</span></span> <span data-ttu-id="3985d-105">Azure CDN poskytuje tyto možnosti se [CDN základní analýza](cdn-analyze-usage-patterns.md) a [diagnostických protokolů](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span><span class="sxs-lookup"><span data-stu-id="3985d-105">Azure CDN provides these capabilities with [CDN Core Analytics](cdn-analyze-usage-patterns.md) and [Diagnostic Logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span></span>

## <a name="cdn-core-analytics"></a><span data-ttu-id="3985d-106">Základní analýza CDN</span><span class="sxs-lookup"><span data-stu-id="3985d-106">CDN Core Analytics</span></span>
<span data-ttu-id="3985d-107">Jako aktuální uživatel Azure CDN Verizon standard nebo premium profilu jste už moct tooview základní analýza hello doplňkovém portálu přístupné přes hello "Manage" možnost hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3985d-107">As a current Azure CDN user with Verizon standard or premium profile, you are already able tooview core analytics in hello supplemental portal accessible via hello "Manage" option from hello Azure portal.</span></span> 

## <a name="azure-diagnostic-logs"></a><span data-ttu-id="3985d-108">Azure diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="3985d-108">Azure Diagnostic Logs</span></span>

<span data-ttu-id="3985d-109">Azure pomocí této nové funkce, můžete nyní zobrazit analýzu základní a uložit je do jedné nebo více cílů, včetně:</span><span class="sxs-lookup"><span data-stu-id="3985d-109">Azure With this new feature, you can now view core analytics and save them into one or more destinations including:</span></span>

 - <span data-ttu-id="3985d-110">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3985d-110">Azure Storage account</span></span>
 - <span data-ttu-id="3985d-111">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3985d-111">Azure Event Hubs</span></span>
 - [<span data-ttu-id="3985d-112">Úložiště analýzy protokolů OMS</span><span class="sxs-lookup"><span data-stu-id="3985d-112">OMS Log Analytics repository</span></span>](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 <span data-ttu-id="3985d-113">Tato funkce je k dispozici pro všechny koncové body CDN patřící tooVerizon (Standard a Premium) a profilů CDN Akamai (Standard).</span><span class="sxs-lookup"><span data-stu-id="3985d-113">This feature is available for all CDN endpoints belonging tooVerizon (Standard & Premium) and Akamai (Standard) CDN Profiles.</span></span>

<span data-ttu-id="3985d-114">Diagnostické protokoly povolení metrik tooexport základní informace o využití z vaší CDN koncový bod tooa řady zdrojů tak, aby můžete využívat požadovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="3985d-114">Diagnostics logs allow you tooexport basic usage metrics from your CDN endpoint tooa variety of sources so that you can consume them in a customized way.</span></span> <span data-ttu-id="3985d-115">Například můžete provést následující typy dat export hello:</span><span class="sxs-lookup"><span data-stu-id="3985d-115">For example, you can do hello following types of data export:</span></span>

- <span data-ttu-id="3985d-116">Exportovat data tooblob úložiště, exportovat tooCSV a generovat grafy v aplikaci excel.</span><span class="sxs-lookup"><span data-stu-id="3985d-116">Export data tooblob storage, export tooCSV, and generate graphs in excel.</span></span>
- <span data-ttu-id="3985d-117">Exportovat data tooevent rozbočovače a korelovat s daty z jiné služby azure.</span><span class="sxs-lookup"><span data-stu-id="3985d-117">Export data tooevent hubs and correlate with data from other azure services.</span></span>
- <span data-ttu-id="3985d-118">Exportovat data analýzy a zobrazení toolog dat ve vlastní pracovní prostor OMS</span><span class="sxs-lookup"><span data-stu-id="3985d-118">Export data toolog analytics and view data in your own OMS work space</span></span>

<span data-ttu-id="3985d-119">Hello následující obrázek ukazuje typické CDN základní analýza zobrazení na data.</span><span class="sxs-lookup"><span data-stu-id="3985d-119">hello following figure shows a typical CDN Core Analytics view into data.</span></span>

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/01_OMS-workspace.png)

<span data-ttu-id="3985d-121">*Obrázek 1 – základní analýza CDN zobrazení*</span><span class="sxs-lookup"><span data-stu-id="3985d-121">*Figure 1 - CDN Core Analytics view*</span></span>

<span data-ttu-id="3985d-122">Následující postup Hello projde hello schéma hello základní analytická data, kroky při povolení funkce hello a jejich předání toovarious cíle a spotřebě z těchto cílů.</span><span class="sxs-lookup"><span data-stu-id="3985d-122">hello following walkthrough goes through hello schema of hello core analytics data, steps involved in enabling hello feature and delivering them toovarious destinations, and consuming from these destinations.</span></span>

## <a name="enable-logging-with-azure-portal"></a><span data-ttu-id="3985d-123">Povolit protokolování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3985d-123">Enable logging with Azure portal</span></span>

> [!NOTE]
> <span data-ttu-id="3985d-124">Hello diagnostické protokoly jsou zapnuté **vypnout** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3985d-124">hello diagnostics logs are turned **off** by default.</span></span> 

<span data-ttu-id="3985d-125">Postupujte podle kroků hello tooenable protokolování s CDN základní analýza:</span><span class="sxs-lookup"><span data-stu-id="3985d-125">Follow hello steps below tooenable logging with CDN Core Analytics:</span></span>

<span data-ttu-id="3985d-126">Přihlaste se toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3985d-126">Sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="3985d-127">Pokud ještě nemáte CDN povolené pro pracovní postup [povolení Azure CDN](cdn-create-new-endpoint.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="3985d-127">If you don't already have CDN enabled for your workflow, [Enable Azure CDN](cdn-create-new-endpoint.md) before you continue.</span></span>

1. <span data-ttu-id="3985d-128">Hello portálu, přejděte příliš**profil CDN**.</span><span class="sxs-lookup"><span data-stu-id="3985d-128">In hello portal, navigate too**CDN profile**.</span></span>
2. <span data-ttu-id="3985d-129">Vyberte profil CDN a pak vyberte hello koncový bod CDN, které chcete tooenable **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="3985d-129">Select a CDN profile, then select hello CDN endpoint that you want tooenable **Diagnostics Logs**.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. <span data-ttu-id="3985d-131">Přejděte příliš**protokolů diagnostiky** okno pod **monitorování** část, pak změňte stav hello příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="3985d-131">Go too**Diagnostics Logs** blade Under **Monitoring** section, then change hello status too**On**.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a><span data-ttu-id="3985d-133">Povolit protokolování s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3985d-133">Enable logging with Azure Storage</span></span>
    
<span data-ttu-id="3985d-134">protokoly hello toostore toouse Azure Storage, vyberte **archivu účet úložiště tooa**, vyberte dní uchovávání dat a klikněte na **CoreAnalytics** pod **protokolu**.</span><span class="sxs-lookup"><span data-stu-id="3985d-134">toouse Azure Storage toostore hello logs, select **Archive tooa storage account**, select retention days, and click **CoreAnalytics** under **Log**.</span></span>

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

<span data-ttu-id="3985d-136">*Obrázek 2 – protokolování s Azure Storage*</span><span class="sxs-lookup"><span data-stu-id="3985d-136">*Figure 2 - Logging with Azure Storage*</span></span>

### <a name="logging-with-oms-log-analytics"></a><span data-ttu-id="3985d-137">Protokolování s OMS analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="3985d-137">Logging with OMS Log Analytics</span></span>

<span data-ttu-id="3985d-138">toouse analýzy protokolů OMS toostore hello protokoly, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3985d-138">toouse OMS Log Analytics toostore hello logs, follow these steps:</span></span>

1. <span data-ttu-id="3985d-139">Z hello **protokolů diagnostiky** okno pod **monitorování**, vyberte **odeslat tooLog Analytics** z</span><span class="sxs-lookup"><span data-stu-id="3985d-139">From hello **Diagnostics Logs** blade Under **Monitoring**, select **Send tooLog Analytics** from</span></span> 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. <span data-ttu-id="3985d-141">Konfigurace protokolování analýzy protokolů hello kliknutím na konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="3985d-141">Configure hello Log Analytics logging by clicking on Configure.</span></span> <span data-ttu-id="3985d-142">Tím přejdete tooa dialogové okno, kde můžete vybrat předchozí pracovního prostoru nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="3985d-142">This takes you tooa dialog where you can select a previous workspace or create a new one.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. <span data-ttu-id="3985d-144">Klikněte na tlačítko **vytvořit nový pracovní prostor**.</span><span class="sxs-lookup"><span data-stu-id="3985d-144">Click **Create New Workspace**.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/07_Create-new.png)

4. <span data-ttu-id="3985d-146">Dále musíte vybrat nový název pracovního prostoru, stávající předplatné, skupinu prostředků (nová nebo stávající), umístění a cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="3985d-146">Next you must select a new workspace name, existing subscription, resource group (new or existing), location, and pricing tier.</span></span> <span data-ttu-id="3985d-147">Máte možnost hello Připnutí tento řídicí panel tooyour konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3985d-147">You have hello option of pinning this configuration tooyour dashboard.</span></span> <span data-ttu-id="3985d-148">Klikněte na tlačítko OK toocomplete hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3985d-148">Click OK toocomplete hello configuration.</span></span>

    <span data-ttu-id="3985d-149">Dále byste měli vidět pracovního prostoru s názvy skupiny pracovním prostorem OMS a prostředků.</span><span class="sxs-lookup"><span data-stu-id="3985d-149">Next you should see your workspace with your OMS Workspace and Resource group names.</span></span> <span data-ttu-id="3985d-150">Názvy musí být jedinečný a použít pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="3985d-150">Names must be unique and can only use letters, numbers, and hyphens.</span></span> <span data-ttu-id="3985d-151">Nejsou povoleny mezery a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="3985d-151">Spaces and underscores are not allowed.</span></span> 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. <span data-ttu-id="3985d-153">Zobrazí další krátkou zprávu oznamující, že váš prostor byl vytvořen a vrátíte se tooyour protokolování konfigurační obrazovce.</span><span class="sxs-lookup"><span data-stu-id="3985d-153">You next get a short message saying that your workspace has been created and you are returned tooyour logging configuration screen.</span></span> <span data-ttu-id="3985d-154">Můžete potvrdit, hello název pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3985d-154">You can confirm hello name of your Log Analytics workspace.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    <span data-ttu-id="3985d-156">Jakmile nastavíte konfigurace analýzy protokolů hello, ujistěte se, že zaškrtnete políčko CoreAnalytics hello CDN protokolování.</span><span class="sxs-lookup"><span data-stu-id="3985d-156">Once you have set up hello Log Analytics configuration, make sure you also check hello CoreAnalytics box for CDN logging.</span></span>

6. <span data-ttu-id="3985d-157">Pokud všechno, co je tooyour libosti, klikněte na tlačítko hello **Uložit** tlačítka v horní části hello dialogové okno Konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-157">If everything is tooyour liking, click hello **Save** button at hello top of hello configuration dialog.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/10_Save-me.png)

    <span data-ttu-id="3985d-159">Hello **Uložit** tlačítko již není aktivní a že hello na nebo vypnutí teď ON, ale blue místo fialová.</span><span class="sxs-lookup"><span data-stu-id="3985d-159">hello **Save** button is no longer active and that hello ON/OFF button is now ON, but blue instead of purple.</span></span>

7. <span data-ttu-id="3985d-160">Pokud chcete toosee nový pracovní prostor OMS, přejděte tooyour portál Azure řídicí panel, klikněte na název hello pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3985d-160">If you want toosee your new OMS workspace, go tooyour Azure portal Dashboard, click hello name of your Log Analytics workspace.</span></span> <span data-ttu-id="3985d-161">Dále uvidíte pracovního prostoru (ujistěte se, že pracovní prostor OMS je označený na levé straně hello).</span><span class="sxs-lookup"><span data-stu-id="3985d-161">Next you will see your workspace (make sure that OMS Workspace is highlighted on hello left).</span></span> <span data-ttu-id="3985d-162">Klikněte na hello portálu OMS dlaždice toosee pracovního prostoru v úložišti OMS hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-162">Click on hello OMS Portal tile toosee your workspace in hello OMS repository.</span></span> 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    <span data-ttu-id="3985d-164">Úložiště OMS je nyní připraven toolog data.</span><span class="sxs-lookup"><span data-stu-id="3985d-164">Your OMS repository is now ready toolog data.</span></span> <span data-ttu-id="3985d-165">V pořadí tooconsume dat, je nutné použít [OMS řešení](#consuming-oms-log-analytics-data), zahrnuté později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3985d-165">In order tooconsume that data, you must use an [OMS Solution](#consuming-oms-log-analytics-data), covered later in this article.</span></span>

<span data-ttu-id="3985d-166">Další informace o protokolu data zpoždění [zde](#log-data-delays).</span><span class="sxs-lookup"><span data-stu-id="3985d-166">For more information about log data delays, go [here](#log-data-delays).</span></span>

## <a name="enable-logging-with-powershell"></a><span data-ttu-id="3985d-167">Povolení protokolování v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3985d-167">Enable logging with PowerShell</span></span>

<span data-ttu-id="3985d-168">Dole je příklad na tom, jak hello tooenable a získání diagnostických protokolů prostřednictvím rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3985d-168">Below is an example on how tooenable and get Diagnostic Logs via hello Azure PowerShell Cmdlets.</span></span>

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a><span data-ttu-id="3985d-169">Povolení diagnostických protokolů v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="3985d-169">Enabling Diagnostic Logs in a Storage Account</span></span>

<span data-ttu-id="3985d-170">Nejdřív se přihlaste a vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="3985d-170">First log in and select a subscription:</span></span>

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


<span data-ttu-id="3985d-171">tooEnable diagnostických protokolů v účtu úložiště, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="3985d-171">tooEnable Diagnostic Logs in a Storage Account, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
<span data-ttu-id="3985d-172">tooEnable protokolů diagnostiky v pracovním prostoru OMS, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="3985d-172">tooEnable Diagnostics Logs in an OMS workspace, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a><span data-ttu-id="3985d-173">Použití protokolů diagnostiky ze služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3985d-173">Consuming diagnostics logs from Azure Storage</span></span>
<span data-ttu-id="3985d-174">Tato část popisuje schéma hello hello CDN základní analýza, jak tyto jsou uspořádány v rámci účtu úložiště Azure a poskytuje ukázkový kód toodownload hello protokolů v souboru CSV tooa.</span><span class="sxs-lookup"><span data-stu-id="3985d-174">This section describes hello schema of hello CDN core analytics, how these are organized inside of an Azure Storage Account and provides sample code toodownload hello logs in tooa CSV file.</span></span>

### <a name="using-microsoft-azure-storage-explorer"></a><span data-ttu-id="3985d-175">Pomocí Průzkumníka úložiště Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="3985d-175">Using Microsoft Azure Storage Explorer</span></span>
<span data-ttu-id="3985d-176">Než se dostanete k hello základní analytická data z hello účet úložiště Azure, je nutné nejprve nástroj tooaccess hello obsah v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3985d-176">Before you can access hello core analytics data from hello Azure Storage Account, you first need a tool tooaccess hello contents in a storage account.</span></span> <span data-ttu-id="3985d-177">Hello trhu jsou k dispozici několik nástrojů, i když je hello ten, který doporučujeme hello Microsoft Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="3985d-177">While there are several tools available in hello market, hello one that we recommend is hello Microsoft Azure Storage Explorer.</span></span> <span data-ttu-id="3985d-178">Si můžete stáhnout z nástroj hello [zde](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="3985d-178">You can download hello tool from [here](http://storageexplorer.com/).</span></span> <span data-ttu-id="3985d-179">Po stažení a instalace softwaru hello konfigurace toouse hello stejný účet úložiště Azure, který byl nakonfigurovaný jako cílový toohello CDN diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="3985d-179">After downloading and installing hello software, configure it toouse hello same Azure Storage Account that was configured as a destination toohello CDN Diagnostics Logs.</span></span>

1.  <span data-ttu-id="3985d-180">Otevřete **Microsoft Azure Storage Explorer**</span><span class="sxs-lookup"><span data-stu-id="3985d-180">Open **Microsoft Azure Storage Explorer**</span></span>
2.  <span data-ttu-id="3985d-181">Najděte účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="3985d-181">Locate hello storage account</span></span>
3.  <span data-ttu-id="3985d-182">Přejděte toohello **"Kontejnery objektů Blob"** uzel v rámci toto úložiště účtu a rozbalte uzel hello</span><span class="sxs-lookup"><span data-stu-id="3985d-182">Go toohello **“Blob Containers”** node under this storage account and expand hello node</span></span>
4.  <span data-ttu-id="3985d-183">Vyberte hello kontejner s názvem **"insights-logs-coreanalytics"** a poklikejte na něj</span><span class="sxs-lookup"><span data-stu-id="3985d-183">Select hello container named **“insights-logs-coreanalytics”** and double-click it</span></span>
5.  <span data-ttu-id="3985d-184">Zobrazit výsledky až na hello pravém podokně počínaje hello první úrovně, který vypadá podobně jako **"resourceId ="**.</span><span class="sxs-lookup"><span data-stu-id="3985d-184">Results show up on hello right-hand pane starting with hello first level, which looks like **“resourceId=”**.</span></span> <span data-ttu-id="3985d-185">Pokračujte kliknutím na všechny hello způsobem, dokud neuvidíte hello souboru **PT1H.json**.</span><span class="sxs-lookup"><span data-stu-id="3985d-185">Continue clicking all hello way until you see hello file **PT1H.json**.</span></span> <span data-ttu-id="3985d-186">V tématu hello následující poznámka vysvětlení hello cesty.</span><span class="sxs-lookup"><span data-stu-id="3985d-186">See hello following note for explanation of hello path.</span></span>
6.  <span data-ttu-id="3985d-187">Každý objekt blob **PT1H.json** představuje hello analýzy protokolů pro jednu hodinu pro konkrétní koncový bod CDN nebo jeho vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="3985d-187">Each blob **PT1H.json** represents hello analytics logs for one hour for a specific CDN endpoint or its custom domain.</span></span>
7.  <span data-ttu-id="3985d-188">schéma Hello hello obsah tohoto souboru JSON je popsané v sekci hello schématu hello základní analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="3985d-188">hello schema of hello contents of this JSON file is described in hello section Schema of hello Core Analytics Logs</span></span>


> [!NOTE]
> <span data-ttu-id="3985d-189">**Formát cesty objektu BLOB**</span><span class="sxs-lookup"><span data-stu-id="3985d-189">**Blob path format**</span></span>
> 
> <span data-ttu-id="3985d-190">Základní analýza protokolů jsou generovány, každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="3985d-190">Core Analytics logs are generated every hour.</span></span> <span data-ttu-id="3985d-191">Všechna data pro jednu hodinu jsou shromážděných a uložených do jediného objektu Blob Azure jako datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="3985d-191">All data for an hour are collected and stored inside a single Azure Blob as a JSON payload.</span></span> <span data-ttu-id="3985d-192">Hello cesta toothis objektů Blob v Azure se zobrazí, jako kdyby je hierarchická struktura.</span><span class="sxs-lookup"><span data-stu-id="3985d-192">hello path toothis Azure Blob appears as if there is a hierarchical structure.</span></span> <span data-ttu-id="3985d-193">Toto je vzhledem k tomu, že nástroj Průzkumník úložiště hello interpretuje '/' za oddělovač adresářů a zobrazuje hierarchii hello ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="3985d-193">This is because hello Storage explorer tool interprets '/' as a directory separator and shows hello hierarchy for convenience.</span></span> <span data-ttu-id="3985d-194">Ve skutečnosti celou cestu hello právě představuje název objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-194">Actually, hello whole path just represents hello blob name.</span></span> <span data-ttu-id="3985d-195">Tento název objektu hello blob následuje hello následující zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="3985d-195">This name of hello blob follows hello following naming convention</span></span> 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

<span data-ttu-id="3985d-196">**Popis polí:**</span><span class="sxs-lookup"><span data-stu-id="3985d-196">**Description of fields:**</span></span>

|<span data-ttu-id="3985d-197">hodnota</span><span class="sxs-lookup"><span data-stu-id="3985d-197">value</span></span>|<span data-ttu-id="3985d-198">description</span><span class="sxs-lookup"><span data-stu-id="3985d-198">description</span></span>|
|-------|---------|
|<span data-ttu-id="3985d-199">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="3985d-199">Subscription ID</span></span>    |<span data-ttu-id="3985d-200">ID hello předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3985d-200">ID of hello Azure Subscription.</span></span> <span data-ttu-id="3985d-201">Toto je ve formátu Guid hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-201">This is in hello Guid format.</span></span>|
|<span data-ttu-id="3985d-202">Prostředek</span><span class="sxs-lookup"><span data-stu-id="3985d-202">Resource</span></span> |<span data-ttu-id="3985d-203">Název skupiny prostředků hello prostředků skupiny toowhich hello CDN patří.</span><span class="sxs-lookup"><span data-stu-id="3985d-203">Group Name   Name of hello resource group toowhich hello CDN resources belong.</span></span>|
|<span data-ttu-id="3985d-204">Název profilu</span><span class="sxs-lookup"><span data-stu-id="3985d-204">Profile Name</span></span> |<span data-ttu-id="3985d-205">Název hello profil CDN</span><span class="sxs-lookup"><span data-stu-id="3985d-205">Name of hello CDN Profile</span></span>|
|<span data-ttu-id="3985d-206">Název koncového bodu</span><span class="sxs-lookup"><span data-stu-id="3985d-206">Endpoint Name</span></span> |<span data-ttu-id="3985d-207">Název hello koncový bod CDN</span><span class="sxs-lookup"><span data-stu-id="3985d-207">Name of hello CDN Endpoint</span></span>|
|<span data-ttu-id="3985d-208">Rok</span><span class="sxs-lookup"><span data-stu-id="3985d-208">Year</span></span>|  <span data-ttu-id="3985d-209">reprezentace 4 číslice roku hello například 2017</span><span class="sxs-lookup"><span data-stu-id="3985d-209">4-digit representation of hello year for example, 2017</span></span>|
|<span data-ttu-id="3985d-210">Měsíc</span><span class="sxs-lookup"><span data-stu-id="3985d-210">Month</span></span>| <span data-ttu-id="3985d-211">2 číslice reprezentace číslo měsíce hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-211">2-digit representation of hello month number.</span></span> <span data-ttu-id="3985d-212">01 = leden... 12 = prosinec</span><span class="sxs-lookup"><span data-stu-id="3985d-212">01=January ... 12=December</span></span>|
|<span data-ttu-id="3985d-213">Den</span><span class="sxs-lookup"><span data-stu-id="3985d-213">Day</span></span>|   <span data-ttu-id="3985d-214">2 číslice reprezentace hello den v měsíci hello</span><span class="sxs-lookup"><span data-stu-id="3985d-214">2 digit representation of hello day of hello month</span></span>|
|<span data-ttu-id="3985d-215">PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="3985d-215">PT1H.json</span></span>| <span data-ttu-id="3985d-216">Skutečný soubor JSON se uloží hello analytická data</span><span class="sxs-lookup"><span data-stu-id="3985d-216">Actual JSON file where hello analytics data is stored</span></span>|

### <a name="exporting-hello-core-analytics-data-tooa-csv-file"></a><span data-ttu-id="3985d-217">Export hello základní analytická Data tooa souboru CSV</span><span class="sxs-lookup"><span data-stu-id="3985d-217">Exporting hello Core Analytics Data tooa CSV File</span></span>

<span data-ttu-id="3985d-218">toomake it snadno tooaccess hello základní analýza poskytujeme ukázkový kód pro nějaký nástroj, který umožňuje stahování souborů hello JSON do souboru nestrukturované oddělený čárkami ve formátu, který lze použít tooeasily vytvořit grafy nebo jiných agregací.</span><span class="sxs-lookup"><span data-stu-id="3985d-218">toomake it easy tooaccess hello Core Analytics, we provide a sample code for a tool, which allows downloading hello JSON files into a flat comma-separated file format, which can be used tooeasily create charts or other aggregations.</span></span>

<span data-ttu-id="3985d-219">Zde je, jak můžete použít nástroj hello:</span><span class="sxs-lookup"><span data-stu-id="3985d-219">Here is how you can use hello tool:</span></span>

1.  <span data-ttu-id="3985d-220">Po kliknutí hello githubu odkaz: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span><span class="sxs-lookup"><span data-stu-id="3985d-220">Visit hello github link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span></span>
2.  <span data-ttu-id="3985d-221">Stáhněte si kód hello</span><span class="sxs-lookup"><span data-stu-id="3985d-221">Download hello code</span></span>
3.  <span data-ttu-id="3985d-222">Postupujte podle pokynů toocompile a konfigurace</span><span class="sxs-lookup"><span data-stu-id="3985d-222">Follow instructions toocompile and configure</span></span>
4.  <span data-ttu-id="3985d-223">Spusťte nástroj hello</span><span class="sxs-lookup"><span data-stu-id="3985d-223">Run hello tool</span></span>
5.  <span data-ttu-id="3985d-224">Výsledný soubor CSV ukazuje hello analytická data v jednoduchých ploché hierarchii.</span><span class="sxs-lookup"><span data-stu-id="3985d-224">Resulting CSV file shows hello analytics data in a simple flat hierarchy.</span></span>

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a><span data-ttu-id="3985d-225">Použití protokolů diagnostiky z úložiště analýzy protokolů OMS</span><span class="sxs-lookup"><span data-stu-id="3985d-225">Consuming diagnostics logs from an OMS Log Analytics repository</span></span>
<span data-ttu-id="3985d-226">Analýzy protokolů je služba v Operations Management Suite (OMS), která monitoruje vaše cloudové a místní prostředí toomaintain jejich dostupnost a výkon.</span><span class="sxs-lookup"><span data-stu-id="3985d-226">Log Analytics is a service in Operations Management Suite (OMS) that monitors your cloud and on-premises environments toomaintain their availability and performance.</span></span> <span data-ttu-id="3985d-227">Shromáždí data generována prostředky ve vašich cloudových a místních prostředích a z dalších monitorování tooprovide analysis nástroje napříč více zdrojů.</span><span class="sxs-lookup"><span data-stu-id="3985d-227">It collects data generated by resources in your cloud and on-premises environments and from other monitoring tools tooprovide analysis across multiple sources.</span></span> 

<span data-ttu-id="3985d-228">toouse analýzy protokolů, musíte [povolit protokolování](#enable-logging-with-azure-storage) úložiště analýzy protokolů Azure OMS toohello, které je popsané výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3985d-228">toouse Log Analytics, you must [enable logging](#enable-logging-with-azure-storage) toohello Azure OMS Log Analytics repository, which is discussed earlier in this article.</span></span>

### <a name="using-hello-oms-repository"></a><span data-ttu-id="3985d-229">Pomocí hello OMS úložiště</span><span class="sxs-lookup"><span data-stu-id="3985d-229">Using hello OMS Repository</span></span>

 <span data-ttu-id="3985d-230">Následující diagram ukazuje hello architektura hello vstupy a výstupy úložiště hello Hello:</span><span class="sxs-lookup"><span data-stu-id="3985d-230">hello following diagram shows hello architecture of hello inputs and outputs of hello repository:</span></span>

![Úložiště analýzy protokolů OMS](./media/cdn-diagnostics-log/12_Repo-overview.png)

<span data-ttu-id="3985d-232">*Obrázek 3 - úložiště analýzy protokolů*</span><span class="sxs-lookup"><span data-stu-id="3985d-232">*Figure 3 - Log Analytics Repository*</span></span>

<span data-ttu-id="3985d-233">Hello data můžete zobrazit v mnoha různými způsoby pomocí řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="3985d-233">You can display hello data in a variety of ways by using Management Solutions.</span></span> <span data-ttu-id="3985d-234">Řešení pro správu můžete získat z hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span><span class="sxs-lookup"><span data-stu-id="3985d-234">You can obtain Management Solutions from hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span></span>

<span data-ttu-id="3985d-235">Řešení pro správu můžete nainstalovat z Azure marketplace kliknutím hello **ho získat** odkaz dole hello v jednotlivých řešení.</span><span class="sxs-lookup"><span data-stu-id="3985d-235">You can install management solutions from Azure marketplace by clicking hello **Get it now** link at hello bottom of each solution.</span></span>

### <a name="adding-an-oms-cdn-management-solution"></a><span data-ttu-id="3985d-236">Přidávání do řešení pro správu OMS CDN</span><span class="sxs-lookup"><span data-stu-id="3985d-236">Adding an OMS CDN Management Solution</span></span>

<span data-ttu-id="3985d-237">Postupujte podle těchto kroků tooadd řešení pro správu:</span><span class="sxs-lookup"><span data-stu-id="3985d-237">Follow these steps tooadd a Management Solution:</span></span>

1.   <span data-ttu-id="3985d-238">Pokud jste tak již neučinili, přihlaste se toohello portálu Azure pomocí svého předplatného Azure a přejděte tooyour řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="3985d-238">If you haven't already done so, sign in toohello Azure portal using your Azure subscription and go tooyour Dashboard.</span></span>
    <span data-ttu-id="3985d-239">![Řídicí panel Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="3985d-239">![Azure Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span></span>

2. <span data-ttu-id="3985d-240">V hello **nový** okno pod **Marketplace**, vyberte **monitorování + správu**.</span><span class="sxs-lookup"><span data-stu-id="3985d-240">In hello **New** blade under **Marketplace**, select **Monitoring + management**.</span></span>

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. <span data-ttu-id="3985d-242">V hello **monitorování + správu** okně klikněte na tlačítko **zobrazit všechny**.</span><span class="sxs-lookup"><span data-stu-id="3985d-242">In hello **Monitoring + management** blade, click **See all**.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/15_See-all.png)

4.  <span data-ttu-id="3985d-244">Vyhledejte CDN hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="3985d-244">Search for CDN in hello search box.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/16_Search-for.png)

5.  <span data-ttu-id="3985d-246">Vyberte **Azure CDN základní analýza**.</span><span class="sxs-lookup"><span data-stu-id="3985d-246">Select **Azure CDN Core Analytics**.</span></span> 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  <span data-ttu-id="3985d-248">Po kliknutí na **vytvořit**, nebudete vyzváni toocreate nový pracovní prostor OMS nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="3985d-248">After clicking **Create**, you will be asked toocreate a new OMS workspace or use an existing one.</span></span> 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  <span data-ttu-id="3985d-250">Vyberte pracovní prostor hello, kterou jste vytvořili před.</span><span class="sxs-lookup"><span data-stu-id="3985d-250">Select hello workspace you created before.</span></span> <span data-ttu-id="3985d-251">Pak musíte tooadd účet automation.</span><span class="sxs-lookup"><span data-stu-id="3985d-251">You then need tooadd an automation account.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/19_Add-automation.png)

8. <span data-ttu-id="3985d-253">Hello následující obrazovka ukazuje hello automatizace účet formuláře, které je nutné vyplnit.</span><span class="sxs-lookup"><span data-stu-id="3985d-253">hello following screen shows hello automation account form you must fill out.</span></span> 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/20_Automation.png)

9. <span data-ttu-id="3985d-255">Po vytvoření účtu automation hello jste připravené tooadd řešení.</span><span class="sxs-lookup"><span data-stu-id="3985d-255">Once you have created hello automation account, you are ready tooadd your solution.</span></span> <span data-ttu-id="3985d-256">Klikněte na tlačítko hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3985d-256">Click hello **Create** button.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/21_Ready.png)

10. <span data-ttu-id="3985d-258">Řešení byl přidán tooyour prostoru.</span><span class="sxs-lookup"><span data-stu-id="3985d-258">Your solution has now been added tooyour workspace.</span></span> <span data-ttu-id="3985d-259">Vraťte se zpátky tooyour portál Azure řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="3985d-259">Go back tooyour Azure portal Dashboard.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/22_Dashboard.png)

    <span data-ttu-id="3985d-261">Klikněte na pracovní prostor analýzy protokolů hello, které jste vytvořili toogo tooyour prostoru.</span><span class="sxs-lookup"><span data-stu-id="3985d-261">Click hello Log Analytics workspace you created toogo tooyour workspace.</span></span> 

11. <span data-ttu-id="3985d-262">Klikněte na tlačítko hello **portálu OMS** dlaždici toosee nové řešení na portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-262">Click hello **OMS Portal** tile toosee your new solution in hello OMS portal.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/23_workspace.png)

12. <span data-ttu-id="3985d-264">Na portálu OMS by teď měl vypadat jako hello následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="3985d-264">Your OMS portal should now look like hello following screen:</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/24_OMS-solution.png)

    <span data-ttu-id="3985d-266">Klikněte na jednu z hello dlaždice toosee několik zobrazení na vaše data.</span><span class="sxs-lookup"><span data-stu-id="3985d-266">Click one of hello tiles toosee several views into your data.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/25_Interior-view.png)

    <span data-ttu-id="3985d-268">Se posunete doleva nebo další správné toosee dlaždice představující jednotlivé zobrazení na hello data.</span><span class="sxs-lookup"><span data-stu-id="3985d-268">You can scroll left or right toosee further tiles representing individual views into hello data.</span></span> 

    <span data-ttu-id="3985d-269">Kliknutím na jedno hello dlaždic vám dává další podrobnosti o vaše data.</span><span class="sxs-lookup"><span data-stu-id="3985d-269">Clicking one of hello tiles gives you more details about your data.</span></span>

     ![Zobrazit všechno](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a><span data-ttu-id="3985d-271">Nabídky a cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="3985d-271">Offers and pricing tiers</span></span>

<span data-ttu-id="3985d-272">Můžete zobrazit nabídky a cenové úrovně pro řešení pro správu OMS [zde](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span><span class="sxs-lookup"><span data-stu-id="3985d-272">You can see offers and pricing tiers for OMS management solutions [here](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span></span>

### <a name="customizing-views"></a><span data-ttu-id="3985d-273">Přizpůsobení zobrazení</span><span class="sxs-lookup"><span data-stu-id="3985d-273">Customizing views</span></span>

<span data-ttu-id="3985d-274">Můžete přizpůsobit zobrazení hello do vašich dat pomocí hello **Návrhář zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="3985d-274">You can customize hello view into your data by using hello **View Designer**.</span></span> <span data-ttu-id="3985d-275">Přejděte na pracovní prostor OMS tooyour a začnete navrhovat kliknutím hello **Návrhář zobrazení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="3985d-275">Go tooyour OMS workspace and begin designing by clicking hello **View Designer** tile.</span></span>

![Návrhář zobrazení](./media/cdn-diagnostics-log/27_Designer.png)

<span data-ttu-id="3985d-277">Můžete přetáhnout a vyřadit typy grafů zleva hello a vyplňte informace o datových hello chcete tooanalyze na zbývajících hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-277">You can drag and drop types of charts from hello left and fill in hello data details you want tooanalyze on hello left.</span></span>

![Návrhář zobrazení](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a><span data-ttu-id="3985d-279">Zpoždění data protokolu</span><span class="sxs-lookup"><span data-stu-id="3985d-279">Log data delays</span></span>

<span data-ttu-id="3985d-280">Verizon protokolu data zpoždění</span><span class="sxs-lookup"><span data-stu-id="3985d-280">Verizon log data delays</span></span> | <span data-ttu-id="3985d-281">Akamai protokolu data zpoždění</span><span class="sxs-lookup"><span data-stu-id="3985d-281">Akamai log data delays</span></span>
--- | ---
<span data-ttu-id="3985d-282">Data protokolu Verizon je zpožděno 1 hodinu a trvat až toostart hodin too2 zobrazování po dokončení šíření koncový bod.</span><span class="sxs-lookup"><span data-stu-id="3985d-282">Verizon log data is 1 hour delayed, and take up too2 hours toostart appearing after endpoint propagation completion.</span></span> | <span data-ttu-id="3985d-283">Data protokolu Akamai je 24 hodin, zpoždění a zabírají toostart hodin too2 zobrazování, pokud byla vytvořena více než 24 hodinami.</span><span class="sxs-lookup"><span data-stu-id="3985d-283">Akamai log data is 24 hours delayed, and takes up too2 hours toostart appearing if it was created more than 24 hours ago.</span></span> <span data-ttu-id="3985d-284">Pokud byl nedávno vytvořen, může trvat až too25 hodin toostart hello protokoly, které jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="3985d-284">If it was recently created, it can take up too25 hours for hello logs toostart appearing.</span></span>

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a><span data-ttu-id="3985d-285">Typy protokolů diagnostiky pro CDN základní analýza</span><span class="sxs-lookup"><span data-stu-id="3985d-285">Diagnostic log types for CDN Core Analytics</span></span>

<span data-ttu-id="3985d-286">Nabízíme aktuálně pouze základní analýza protokoly, které obsahují metriky ukazující statistiky odpovědi HTTP a odchozí statistiky, jak je vidět z hello CDN POP nebo okraje.</span><span class="sxs-lookup"><span data-stu-id="3985d-286">We currently offer only Core Analytics logs, which contain metrics showing HTTP response statistics and egress statistics as seen from hello CDN POPs/edges.</span></span>

### <a name="core-analytics-metrics-details"></a><span data-ttu-id="3985d-287">Základní analýza metriky podrobnosti</span><span class="sxs-lookup"><span data-stu-id="3985d-287">Core Analytics Metrics Details</span></span>
<span data-ttu-id="3985d-288">Hello následující tabulka obsahuje seznam metriky, které jsou k dispozici v hello základní Analýza protokolů.</span><span class="sxs-lookup"><span data-stu-id="3985d-288">hello following table shows a list of metrics available in hello Core Analytics logs.</span></span> <span data-ttu-id="3985d-289">Ne všechny metriky jsou k dispozici od všech poskytovatelů, i když tyto rozdíly jsou minimální.</span><span class="sxs-lookup"><span data-stu-id="3985d-289">Not all metrics are available from all providers, although such differences are minimal.</span></span> <span data-ttu-id="3985d-290">Hello také následující tabulka ukazuje, pokud daná metrika je k dispozici od zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="3985d-290">hello following table also shows if a given metric is available from a provider.</span></span> <span data-ttu-id="3985d-291">Upozorňujeme, že jsou k dispozici pro jenom ty koncové body CDN mající přenosy na těchto hello metriky.</span><span class="sxs-lookup"><span data-stu-id="3985d-291">Please note that hello metrics are available for only those CDN endpoints that have traffic on them.</span></span>


|<span data-ttu-id="3985d-292">Metrika</span><span class="sxs-lookup"><span data-stu-id="3985d-292">Metric</span></span>                     | <span data-ttu-id="3985d-293">Popis</span><span class="sxs-lookup"><span data-stu-id="3985d-293">Description</span></span>   | <span data-ttu-id="3985d-294">Verizon</span><span class="sxs-lookup"><span data-stu-id="3985d-294">Verizon</span></span>  | <span data-ttu-id="3985d-295">Akamai</span><span class="sxs-lookup"><span data-stu-id="3985d-295">Akamai</span></span> 
|---------------------------|---------------|---|---|
| <span data-ttu-id="3985d-296">RequestCountTotal</span><span class="sxs-lookup"><span data-stu-id="3985d-296">RequestCountTotal</span></span>         |<span data-ttu-id="3985d-297">Celkový počet přístupů požadavek během tohoto období</span><span class="sxs-lookup"><span data-stu-id="3985d-297">Total number of request hits during this period</span></span>| <span data-ttu-id="3985d-298">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-298">Yes</span></span>  |<span data-ttu-id="3985d-299">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-299">Yes</span></span>   |
| <span data-ttu-id="3985d-300">RequestCountHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="3985d-300">RequestCountHttpStatus2xx</span></span> |<span data-ttu-id="3985d-301">Počet všech požadavků, které 2xx kód HTTP (např. 200, 202)</span><span class="sxs-lookup"><span data-stu-id="3985d-301">Count of all requests that resulted in a 2xx HTTP code (e.g. 200, 202)</span></span>              | <span data-ttu-id="3985d-302">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-302">Yes</span></span>  |<span data-ttu-id="3985d-303">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-303">Yes</span></span>   |
| <span data-ttu-id="3985d-304">RequestCountHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="3985d-304">RequestCountHttpStatus3xx</span></span> | <span data-ttu-id="3985d-305">Počet všech požadavků, které 3xx kód HTTP (např. 300, 302)</span><span class="sxs-lookup"><span data-stu-id="3985d-305">Count of all requests that resulted in a 3xx HTTP code (e.g. 300, 302)</span></span>              | <span data-ttu-id="3985d-306">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-306">Yes</span></span>  |<span data-ttu-id="3985d-307">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-307">Yes</span></span>   |
| <span data-ttu-id="3985d-308">RequestCountHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="3985d-308">RequestCountHttpStatus4xx</span></span> |<span data-ttu-id="3985d-309">Počet všech požadavků, které 4xx kód HTTP (např. 400, 404)</span><span class="sxs-lookup"><span data-stu-id="3985d-309">Count of all requests that resulted in a 4xx HTTP code (e.g. 400, 404)</span></span>               | <span data-ttu-id="3985d-310">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-310">Yes</span></span>   |<span data-ttu-id="3985d-311">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-311">Yes</span></span>   |
| <span data-ttu-id="3985d-312">RequestCountHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="3985d-312">RequestCountHttpStatus5xx</span></span> | <span data-ttu-id="3985d-313">Počet všech požadavků, jejichž výsledkem kód HTTP 5xx (např. 500, 504)</span><span class="sxs-lookup"><span data-stu-id="3985d-313">Count of all requests that resulted in a 5xx HTTP code (e.g. 500, 504)</span></span>              | <span data-ttu-id="3985d-314">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-314">Yes</span></span>  |<span data-ttu-id="3985d-315">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-315">Yes</span></span>   |
| <span data-ttu-id="3985d-316">RequestCountHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="3985d-316">RequestCountHttpStatusOthers</span></span> |  <span data-ttu-id="3985d-317">Počet všechny ostatní kódy HTTP (mimo 2xx 5xx)</span><span class="sxs-lookup"><span data-stu-id="3985d-317">Count of all other HTTP codes (outside of 2xx-5xx)</span></span> | <span data-ttu-id="3985d-318">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-318">Yes</span></span>  |<span data-ttu-id="3985d-319">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-319">Yes</span></span>   |
| <span data-ttu-id="3985d-320">RequestCountHttpStatus200</span><span class="sxs-lookup"><span data-stu-id="3985d-320">RequestCountHttpStatus200</span></span> | <span data-ttu-id="3985d-321">Počet všech požadavků, jejichž výsledkem 200 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="3985d-321">Count of all requests that resulted in a 200 HTTP code response</span></span>              |<span data-ttu-id="3985d-322">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-322">No</span></span>   |<span data-ttu-id="3985d-323">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-323">Yes</span></span>   |
| <span data-ttu-id="3985d-324">RequestCountHttpStatus206</span><span class="sxs-lookup"><span data-stu-id="3985d-324">RequestCountHttpStatus206</span></span> | <span data-ttu-id="3985d-325">Počet všech požadavků, jejichž výsledkem 206 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="3985d-325">Count of all requests that resulted in a 206 HTTP code response</span></span>              |<span data-ttu-id="3985d-326">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-326">No</span></span>   |<span data-ttu-id="3985d-327">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-327">Yes</span></span>   |
| <span data-ttu-id="3985d-328">RequestCountHttpStatus302</span><span class="sxs-lookup"><span data-stu-id="3985d-328">RequestCountHttpStatus302</span></span> | <span data-ttu-id="3985d-329">Počet všech požadavků, jejichž výsledkem 302 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="3985d-329">Count of all requests that resulted in a 302 HTTP code response</span></span>              |<span data-ttu-id="3985d-330">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-330">No</span></span>   |<span data-ttu-id="3985d-331">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-331">Yes</span></span>   |
| <span data-ttu-id="3985d-332">RequestCountHttpStatus304</span><span class="sxs-lookup"><span data-stu-id="3985d-332">RequestCountHttpStatus304</span></span> |  <span data-ttu-id="3985d-333">Počet všech požadavků, jejichž výsledkem 304 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="3985d-333">Count of all requests that resulted in a 304 HTTP code response</span></span>             |<span data-ttu-id="3985d-334">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-334">No</span></span>   |<span data-ttu-id="3985d-335">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-335">Yes</span></span>   |
| <span data-ttu-id="3985d-336">RequestCountHttpStatus404</span><span class="sxs-lookup"><span data-stu-id="3985d-336">RequestCountHttpStatus404</span></span> | <span data-ttu-id="3985d-337">Počet všech požadavků, jejichž výsledkem kódu odpovědi HTTP 404</span><span class="sxs-lookup"><span data-stu-id="3985d-337">Count of all requests that resulted in a 404 HTTP code response</span></span>              |<span data-ttu-id="3985d-338">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-338">No</span></span>   |<span data-ttu-id="3985d-339">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-339">Yes</span></span>   |
| <span data-ttu-id="3985d-340">RequestCountCacheHit</span><span class="sxs-lookup"><span data-stu-id="3985d-340">RequestCountCacheHit</span></span> |<span data-ttu-id="3985d-341">Počet všech požadavků, jejichž výsledkem požadavků mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3985d-341">Count of all requests that resulted in a Cache Hit.</span></span> <span data-ttu-id="3985d-342">To znamená, že zpracování hello asset přímo z hello POP toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="3985d-342">This means hello asset was served directly from hello POP toohello Client.</span></span>               | <span data-ttu-id="3985d-343">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-343">Yes</span></span>  |<span data-ttu-id="3985d-344">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-344">No</span></span>   |
| <span data-ttu-id="3985d-345">RequestCountCacheMiss</span><span class="sxs-lookup"><span data-stu-id="3985d-345">RequestCountCacheMiss</span></span> | <span data-ttu-id="3985d-346">Počet všech požadavků, která byla vygenerována v neúspěšnému přístupu do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3985d-346">Count of all requests that resulted in a Cache Miss.</span></span> <span data-ttu-id="3985d-347">To znamená hello asset nebyl nalezen na hello POP nejbližší toohello klienta a proto byla načtena z hello původu.</span><span class="sxs-lookup"><span data-stu-id="3985d-347">This means hello asset was not found on hello POP closest toohello client, and therefore was retrieved from hello Origin.</span></span>              |<span data-ttu-id="3985d-348">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-348">Yes</span></span>   | <span data-ttu-id="3985d-349">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-349">No</span></span>  |
| <span data-ttu-id="3985d-350">RequestCountCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="3985d-350">RequestCountCacheNoCache</span></span> | <span data-ttu-id="3985d-351">Počet všech požadavků tooan asset, který nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele tooa hranu hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-351">Count of all requests tooan asset that are prevented from being cached due tooa user configuration on hello edge.</span></span>              |<span data-ttu-id="3985d-352">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-352">Yes</span></span>   | <span data-ttu-id="3985d-353">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-353">No</span></span>  |
| <span data-ttu-id="3985d-354">RequestCountCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="3985d-354">RequestCountCacheUncacheable</span></span> | <span data-ttu-id="3985d-355">Počet všech požadavků tooassets, která nebudou moci ukládat do mezipaměti podle hello asset Cache-Control a vyprší platnost hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klientem hello HTTP</span><span class="sxs-lookup"><span data-stu-id="3985d-355">Count of all requests tooassets that are prevented from being cached by hello asset's Cache-Control and Expires headers, which indicate that it should not be cached on a POP or by hello HTTP client</span></span>                |<span data-ttu-id="3985d-356">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-356">Yes</span></span>   |<span data-ttu-id="3985d-357">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-357">No</span></span>   |
| <span data-ttu-id="3985d-358">RequestCountCacheOthers</span><span class="sxs-lookup"><span data-stu-id="3985d-358">RequestCountCacheOthers</span></span> | <span data-ttu-id="3985d-359">Počet všech požadavků stavem mezipaměti, které nejsou pokryty výše.</span><span class="sxs-lookup"><span data-stu-id="3985d-359">Count of all requests with cache status not covered by above.</span></span>              |<span data-ttu-id="3985d-360">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-360">Yes</span></span>   | <span data-ttu-id="3985d-361">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-361">No</span></span>  |
| <span data-ttu-id="3985d-362">EgressTotal</span><span class="sxs-lookup"><span data-stu-id="3985d-362">EgressTotal</span></span> | <span data-ttu-id="3985d-363">Odchozí přenosy dat v GB</span><span class="sxs-lookup"><span data-stu-id="3985d-363">Outbound data transfer in GB</span></span>              |<span data-ttu-id="3985d-364">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-364">Yes</span></span>   |<span data-ttu-id="3985d-365">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-365">Yes</span></span>   |
| <span data-ttu-id="3985d-366">EgressHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="3985d-366">EgressHttpStatus2xx</span></span> | <span data-ttu-id="3985d-367">Odchozí datové přenosy * pro odpovědi s stavové kódy HTTP 2xx v GB</span><span class="sxs-lookup"><span data-stu-id="3985d-367">Outbound data transfer* for responses with 2xx HTTP status codes in GB</span></span>            |<span data-ttu-id="3985d-368">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-368">Yes</span></span>   |<span data-ttu-id="3985d-369">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-369">No</span></span>   |
| <span data-ttu-id="3985d-370">EgressHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="3985d-370">EgressHttpStatus3xx</span></span> | <span data-ttu-id="3985d-371">Přenos odchozích dat pro odpovědi s stavové kódy HTTP 3xx v GB</span><span class="sxs-lookup"><span data-stu-id="3985d-371">Outbound data transfer for responses with 3xx HTTP status codes in GB</span></span>              |<span data-ttu-id="3985d-372">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-372">Yes</span></span>   |<span data-ttu-id="3985d-373">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-373">No</span></span>   |
| <span data-ttu-id="3985d-374">EgressHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="3985d-374">EgressHttpStatus4xx</span></span> | <span data-ttu-id="3985d-375">Přenos odchozích dat pro odpovědi s stavové kódy HTTP 4xx v GB</span><span class="sxs-lookup"><span data-stu-id="3985d-375">Outbound data transfer for responses with 4xx HTTP status codes in GB</span></span>               |<span data-ttu-id="3985d-376">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-376">Yes</span></span>   | <span data-ttu-id="3985d-377">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-377">No</span></span>  |
| <span data-ttu-id="3985d-378">EgressHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="3985d-378">EgressHttpStatus5xx</span></span> | <span data-ttu-id="3985d-379">Přenos odchozích dat pro odpovědi s stavové kódy HTTP 5xx v GB</span><span class="sxs-lookup"><span data-stu-id="3985d-379">Outbound data transfer for responses with 5xx HTTP status codes in GB</span></span>               |<span data-ttu-id="3985d-380">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-380">Yes</span></span>   |  <span data-ttu-id="3985d-381">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-381">No</span></span> |
| <span data-ttu-id="3985d-382">EgressHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="3985d-382">EgressHttpStatusOthers</span></span> | <span data-ttu-id="3985d-383">Odchozí přenosy dat pro odpovědi s ostatních stavových kódech HTTP v GB</span><span class="sxs-lookup"><span data-stu-id="3985d-383">Outbound data transfer for responses with other HTTP status codes in GB</span></span>                |<span data-ttu-id="3985d-384">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-384">Yes</span></span>   |<span data-ttu-id="3985d-385">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-385">No</span></span>   |
| <span data-ttu-id="3985d-386">EgressCacheHit</span><span class="sxs-lookup"><span data-stu-id="3985d-386">EgressCacheHit</span></span> |  <span data-ttu-id="3985d-387">Odchozí přenos dat pro odpovědi, které byly doručeny přímo z mezipaměti CDN hello na hello CDN POP od okrajů</span><span class="sxs-lookup"><span data-stu-id="3985d-387">Outbound data transfer for responses that were delivered directly from hello CDN cache on hello CDN POPs/Edges</span></span>  |<span data-ttu-id="3985d-388">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-388">Yes</span></span>   |  <span data-ttu-id="3985d-389">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-389">No</span></span> |
| <span data-ttu-id="3985d-390">EgressCacheMiss</span><span class="sxs-lookup"><span data-stu-id="3985d-390">EgressCacheMiss</span></span> | <span data-ttu-id="3985d-391">Přenos odchozích dat pro odpovědi, které nebyly na hello nejbližší POP server najít a načíst ze serveru původu hello</span><span class="sxs-lookup"><span data-stu-id="3985d-391">Outbound data transfer for responses that were not found on hello nearest POP server, and retrieved from hello origin server</span></span>              |<span data-ttu-id="3985d-392">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-392">Yes</span></span>   |  <span data-ttu-id="3985d-393">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-393">No</span></span> |
| <span data-ttu-id="3985d-394">EgressCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="3985d-394">EgressCacheNoCache</span></span> | <span data-ttu-id="3985d-395">Odchozí datové přenosy pro prostředky, které nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele tooa hranu hello.</span><span class="sxs-lookup"><span data-stu-id="3985d-395">Outbound data transfer for assets that are prevented from being cached due tooa user configuration on hello edge.</span></span>                |<span data-ttu-id="3985d-396">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-396">Yes</span></span>   |<span data-ttu-id="3985d-397">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-397">No</span></span>   |
| <span data-ttu-id="3985d-398">EgressCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="3985d-398">EgressCacheUncacheable</span></span> | <span data-ttu-id="3985d-399">Odchozí datové přenosy pro prostředky, které nebudou moci ukládat do mezipaměti hello asset Cache-Control nebo Expires hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klientem hello HTTP</span><span class="sxs-lookup"><span data-stu-id="3985d-399">Outbound data transfer for assets that are prevented from being cached by hello asset's Cache-Control and/or Expires headers, which indicate that it should not be cached on a POP or by hello HTTP client</span></span>                    |<span data-ttu-id="3985d-400">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-400">Yes</span></span>   | <span data-ttu-id="3985d-401">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-401">No</span></span>  |
| <span data-ttu-id="3985d-402">EgressCacheOthers</span><span class="sxs-lookup"><span data-stu-id="3985d-402">EgressCacheOthers</span></span> |  <span data-ttu-id="3985d-403">Odchozí datové přenosy s dalšími scénáři mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3985d-403">Outbound data transfers for other cache scenarios.</span></span>             |<span data-ttu-id="3985d-404">Ano</span><span class="sxs-lookup"><span data-stu-id="3985d-404">Yes</span></span>   | <span data-ttu-id="3985d-405">Ne</span><span class="sxs-lookup"><span data-stu-id="3985d-405">No</span></span>  |

<span data-ttu-id="3985d-406">* Odchozí přenosy dat odkazuje tootraffic doručit od CDN POP servery toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="3985d-406">*Outbound data transfer refers tootraffic delivered from CDN POP servers toohello client.</span></span>


### <a name="schema-of-hello-core-analytics-logs"></a><span data-ttu-id="3985d-407">Schéma hello základní analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="3985d-407">Schema of hello Core Analytics Logs</span></span> 

<span data-ttu-id="3985d-408">Všechny protokoly se ukládají ve formátu JSON a každá položka má následující hello níže schématu polí s řetězcem:</span><span class="sxs-lookup"><span data-stu-id="3985d-408">All logs are stored in JSON format and each entry has string fields following hello below schema:</span></span>

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of hello CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of hello domain for which hello statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

<span data-ttu-id="3985d-409">Kde hello čas představuje čas zahájení hello hello hodinu hranic, pro který je hlášen hello statistiky.</span><span class="sxs-lookup"><span data-stu-id="3985d-409">Where hello ‘time’ represents hello start time of hello hour boundary for which hello statistics is reported.</span></span> <span data-ttu-id="3985d-410">Pokud metriky není podporována zprostředkovatelem CDN, místo hodnotu double nebo celé číslo, bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="3985d-410">When a metric is not supported by a CDN provider, instead of a double or integer value, there will be a null value.</span></span> <span data-ttu-id="3985d-411">Tato hodnota null znamená hello absenci metriky a tento proces se liší od hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="3985d-411">This null value indicates hello absence of a metric, and this is different from a 0 value.</span></span> <span data-ttu-id="3985d-412">Všimněte si také, že bude jednu sadu tyto metriky na doménu na koncový bod hello nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="3985d-412">Also note that there will be one set of these metrics per domain configured on hello endpoint.</span></span>

<span data-ttu-id="3985d-413">Příklad vlastnosti níže:</span><span class="sxs-lookup"><span data-stu-id="3985d-413">Example properties below:</span></span>

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a><span data-ttu-id="3985d-414">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3985d-414">Additional resources</span></span>

* [<span data-ttu-id="3985d-415">Azure diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="3985d-415">Azure Diagnostic logs</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [<span data-ttu-id="3985d-416">Základní analýza prostřednictvím doplňkovém portálu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="3985d-416">Core analytics via Azure CDN supplemental portal</span></span>](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [<span data-ttu-id="3985d-417">Analýzy protokolů Azure OMS</span><span class="sxs-lookup"><span data-stu-id="3985d-417">Azure OMS Log Analytics</span></span>](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [<span data-ttu-id="3985d-418">Analýza protokolů Azure rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="3985d-418">Azure Log Analytics REST API</span></span>](https://docs.microsoft.com/en-us/rest/api/loganalytics)







