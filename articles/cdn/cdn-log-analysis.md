---
title: "V protokolu analýzy Azure CDN | Microsoft Docs"
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
ms.openlocfilehash: 03ff74ae4e40d3f2279caaf4f73e9b4aac6a2ebb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a><span data-ttu-id="fe3e9-103">Diagnostické protokoly pro Azure CDN</span><span class="sxs-lookup"><span data-stu-id="fe3e9-103">Diagnostics Logs for Azure CDN</span></span>

<span data-ttu-id="fe3e9-104">Když povolíte CDN pro vaši aplikaci, bude pravděpodobně chtít monitorovat využití CDN, zkontrolujte stav vaší doručení a potenciální potíže.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-104">After enabling CDN for your application, you will likely want to monitor the CDN usage, check the health of your delivery, and troubleshoot potential issues.</span></span> <span data-ttu-id="fe3e9-105">Azure CDN poskytuje tyto možnosti se [CDN základní analýza](cdn-analyze-usage-patterns.md) a [diagnostických protokolů](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-105">Azure CDN provides these capabilities with [CDN Core Analytics](cdn-analyze-usage-patterns.md) and [Diagnostic Logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span></span>

## <a name="cdn-core-analytics"></a><span data-ttu-id="fe3e9-106">Základní analýza CDN</span><span class="sxs-lookup"><span data-stu-id="fe3e9-106">CDN Core Analytics</span></span>
<span data-ttu-id="fe3e9-107">Jako aktuální uživatel Azure CDN Verizon standard nebo premium profilu jste už moct zobrazovat základní analýza na doplňkovém portálu, který je přístupný prostřednictvím možnosti "Manage" z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-107">As a current Azure CDN user with Verizon standard or premium profile, you are already able to view core analytics in the supplemental portal accessible via the "Manage" option from the Azure portal.</span></span> 

## <a name="azure-diagnostic-logs"></a><span data-ttu-id="fe3e9-108">Azure diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="fe3e9-108">Azure Diagnostic Logs</span></span>

<span data-ttu-id="fe3e9-109">Azure pomocí této nové funkce, můžete nyní zobrazit analýzu základní a uložit je do jedné nebo více cílů, včetně:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-109">Azure With this new feature, you can now view core analytics and save them into one or more destinations including:</span></span>

 - <span data-ttu-id="fe3e9-110">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="fe3e9-110">Azure Storage account</span></span>
 - <span data-ttu-id="fe3e9-111">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="fe3e9-111">Azure Event Hubs</span></span>
 - [<span data-ttu-id="fe3e9-112">Úložiště analýzy protokolů OMS</span><span class="sxs-lookup"><span data-stu-id="fe3e9-112">OMS Log Analytics repository</span></span>](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 <span data-ttu-id="fe3e9-113">Tato funkce je k dispozici pro všechny koncové body CDN patřící do Verizon (Standard a Premium) a profilů CDN Akamai (Standard).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-113">This feature is available for all CDN endpoints belonging to Verizon (Standard & Premium) and Akamai (Standard) CDN Profiles.</span></span>

<span data-ttu-id="fe3e9-114">Diagnostické protokoly umožňují, aby můžete využívat požadovaným způsobem exportu metriky základní informace o využití z koncový bod CDN do různých zdrojů.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-114">Diagnostics logs allow you to export basic usage metrics from your CDN endpoint to a variety of sources so that you can consume them in a customized way.</span></span> <span data-ttu-id="fe3e9-115">Například můžete provést následující typy dat exportu:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-115">For example, you can do the following types of data export:</span></span>

- <span data-ttu-id="fe3e9-116">Export dat do úložiště objektů blob, exportovat do souboru CSV a generovat grafy v aplikaci excel.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-116">Export data to blob storage, export to CSV, and generate graphs in excel.</span></span>
- <span data-ttu-id="fe3e9-117">Exportovat data do centra událostí a korelovat s daty z jiné služby azure.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-117">Export data to event hubs and correlate with data from other azure services.</span></span>
- <span data-ttu-id="fe3e9-118">Exportovat data do protokolu analýzy a zobrazení dat ve vlastní pracovní prostor OMS</span><span class="sxs-lookup"><span data-stu-id="fe3e9-118">Export data to log analytics and view data in your own OMS work space</span></span>

<span data-ttu-id="fe3e9-119">Následující obrázek znázorňuje typické přehled CDN základní analýza data.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-119">The following figure shows a typical CDN Core Analytics view into data.</span></span>

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/01_OMS-workspace.png)

<span data-ttu-id="fe3e9-121">*Obrázek 1 – základní analýza CDN zobrazení*</span><span class="sxs-lookup"><span data-stu-id="fe3e9-121">*Figure 1 - CDN Core Analytics view*</span></span>

<span data-ttu-id="fe3e9-122">Následující návod projde schéma analytická data základní kroky při povolení funkce a jejich předání do různých umístění a spotřebě z těchto cílů.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-122">The following walkthrough goes through the schema of the core analytics data, steps involved in enabling the feature and delivering them to various destinations, and consuming from these destinations.</span></span>

## <a name="enable-logging-with-azure-portal"></a><span data-ttu-id="fe3e9-123">Povolit protokolování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe3e9-123">Enable logging with Azure portal</span></span>

> [!NOTE]
> <span data-ttu-id="fe3e9-124">Diagnostické protokoly jsou zapnuté **vypnout** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-124">The diagnostics logs are turned **off** by default.</span></span> 

<span data-ttu-id="fe3e9-125">Použijte následující postup povolení protokolování s CDN základní analýza:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-125">Follow the steps below to enable logging with CDN Core Analytics:</span></span>

<span data-ttu-id="fe3e9-126">Přihlaste se k webu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-126">Sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="fe3e9-127">Pokud ještě nemáte CDN povolené pro pracovní postup [povolení Azure CDN](cdn-create-new-endpoint.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-127">If you don't already have CDN enabled for your workflow, [Enable Azure CDN](cdn-create-new-endpoint.md) before you continue.</span></span>

1. <span data-ttu-id="fe3e9-128">Na portálu, přejděte na **profil CDN**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-128">In the portal, navigate to **CDN profile**.</span></span>
2. <span data-ttu-id="fe3e9-129">Vyberte profil CDN a pak vyberte koncového bodu CDN, který chcete povolit **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-129">Select a CDN profile, then select the CDN endpoint that you want to enable **Diagnostics Logs**.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. <span data-ttu-id="fe3e9-131">Přejděte na **protokolů diagnostiky** okno pod **monitorování** část, pak změňte na stavu **na**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-131">Go to **Diagnostics Logs** blade Under **Monitoring** section, then change the status to **On**.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a><span data-ttu-id="fe3e9-133">Povolit protokolování s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="fe3e9-133">Enable logging with Azure Storage</span></span>
    
<span data-ttu-id="fe3e9-134">Používání Azure Storage k ukládání protokolů, vyberte **archivu do účtu úložiště**, vyberte dní uchovávání a klikněte na tlačítko **CoreAnalytics** pod **protokolu**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-134">To use Azure Storage to store the logs, select **Archive to a storage account**, select retention days, and click **CoreAnalytics** under **Log**.</span></span>

![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

<span data-ttu-id="fe3e9-136">*Obrázek 2 – protokolování s Azure Storage*</span><span class="sxs-lookup"><span data-stu-id="fe3e9-136">*Figure 2 - Logging with Azure Storage*</span></span>

### <a name="logging-with-oms-log-analytics"></a><span data-ttu-id="fe3e9-137">Protokolování s OMS analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="fe3e9-137">Logging with OMS Log Analytics</span></span>

<span data-ttu-id="fe3e9-138">OMS Log Analytics k ukládání protokolů, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-138">To use OMS Log Analytics to store the logs, follow these steps:</span></span>

1. <span data-ttu-id="fe3e9-139">Z **protokolů diagnostiky** okno pod **monitorování**, vyberte **odeslat k analýze protokolů** z</span><span class="sxs-lookup"><span data-stu-id="fe3e9-139">From the **Diagnostics Logs** blade Under **Monitoring**, select **Send to Log Analytics** from</span></span> 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. <span data-ttu-id="fe3e9-141">Konfigurace protokolování analýzy protokolů kliknutím na konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-141">Configure the Log Analytics logging by clicking on Configure.</span></span> <span data-ttu-id="fe3e9-142">Tím přejdete do dialogového okna, kde můžete vybrat předchozí pracovního prostoru nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-142">This takes you to a dialog where you can select a previous workspace or create a new one.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. <span data-ttu-id="fe3e9-144">Klikněte na tlačítko **vytvořit nový pracovní prostor**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-144">Click **Create New Workspace**.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/07_Create-new.png)

4. <span data-ttu-id="fe3e9-146">Dále musíte vybrat nový název pracovního prostoru, stávající předplatné, skupinu prostředků (nová nebo stávající), umístění a cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-146">Next you must select a new workspace name, existing subscription, resource group (new or existing), location, and pricing tier.</span></span> <span data-ttu-id="fe3e9-147">Máte možnost Připnutí na řídicí panel tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-147">You have the option of pinning this configuration to your dashboard.</span></span> <span data-ttu-id="fe3e9-148">Klikněte na tlačítko OK k dokončení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-148">Click OK to complete the configuration.</span></span>

    <span data-ttu-id="fe3e9-149">Dále byste měli vidět pracovního prostoru s názvy skupiny pracovním prostorem OMS a prostředků.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-149">Next you should see your workspace with your OMS Workspace and Resource group names.</span></span> <span data-ttu-id="fe3e9-150">Názvy musí být jedinečný a použít pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-150">Names must be unique and can only use letters, numbers, and hyphens.</span></span> <span data-ttu-id="fe3e9-151">Nejsou povoleny mezery a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-151">Spaces and underscores are not allowed.</span></span> 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. <span data-ttu-id="fe3e9-153">Zobrazí další krátkou zprávu oznamující, že váš prostor byl vytvořen a vrátíte se na obrazovce konfigurace protokolování.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-153">You next get a short message saying that your workspace has been created and you are returned to your logging configuration screen.</span></span> <span data-ttu-id="fe3e9-154">Můžete potvrdit, název pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-154">You can confirm the name of your Log Analytics workspace.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    <span data-ttu-id="fe3e9-156">Jakmile jste nastavili konfigurace analýzy protokolů, zkontrolujte, zda že můžete taky zaškrtnout políčko CoreAnalytics pro protokolování CDN.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-156">Once you have set up the Log Analytics configuration, make sure you also check the CoreAnalytics box for CDN logging.</span></span>

6. <span data-ttu-id="fe3e9-157">Pokud všechno, co je titulků, klikněte na tlačítko **Uložit** tlačítka v horní části dialogového okna konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-157">If everything is to your liking, click the **Save** button at the top of the configuration dialog.</span></span>

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/10_Save-me.png)

    <span data-ttu-id="fe3e9-159">**Uložit** tlačítko již není aktivní a nyní je tlačítko Zapnout nebo vypnout v, ale blue místo fialová.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-159">The **Save** button is no longer active and that the ON/OFF button is now ON, but blue instead of purple.</span></span>

7. <span data-ttu-id="fe3e9-160">Pokud chcete zobrazit nové pracovní prostor OMS, přejděte na portálu Azure řídicí panel, klikněte na název pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-160">If you want to see your new OMS workspace, go to your Azure portal Dashboard, click the name of your Log Analytics workspace.</span></span> <span data-ttu-id="fe3e9-161">Dále uvidíte pracovního prostoru (ujistěte se, zda je na levé straně zvýrazněný pracovním prostorem OMS).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-161">Next you will see your workspace (make sure that OMS Workspace is highlighted on the left).</span></span> <span data-ttu-id="fe3e9-162">Klikněte na dlaždici portálu OMS zobrazíte pracovní prostor v úložišti OMS.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-162">Click on the OMS Portal tile to see your workspace in the OMS repository.</span></span> 

    ![Portál – protokolů diagnostiky](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    <span data-ttu-id="fe3e9-164">Úložiště OMS je nyní připraven k protokolovat data.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-164">Your OMS repository is now ready to log data.</span></span> <span data-ttu-id="fe3e9-165">Aby bylo možné využívat data, musíte použít [OMS řešení](#consuming-oms-log-analytics-data), zahrnuté později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-165">In order to consume that data, you must use an [OMS Solution](#consuming-oms-log-analytics-data), covered later in this article.</span></span>

<span data-ttu-id="fe3e9-166">Další informace o protokolu data zpoždění [zde](#log-data-delays).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-166">For more information about log data delays, go [here](#log-data-delays).</span></span>

## <a name="enable-logging-with-powershell"></a><span data-ttu-id="fe3e9-167">Povolení protokolování v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe3e9-167">Enable logging with PowerShell</span></span>

<span data-ttu-id="fe3e9-168">Dole je příklad o tom, jak povolit a získat diagnostické protokoly prostřednictvím rutin prostředí PowerShell Azure.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-168">Below is an example on how to enable and get Diagnostic Logs via the Azure PowerShell Cmdlets.</span></span>

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a><span data-ttu-id="fe3e9-169">Povolení diagnostických protokolů v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="fe3e9-169">Enabling Diagnostic Logs in a Storage Account</span></span>

<span data-ttu-id="fe3e9-170">Nejdřív se přihlaste a vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-170">First log in and select a subscription:</span></span>

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


<span data-ttu-id="fe3e9-171">K povolení diagnostických protokolů v účtu úložiště použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-171">To Enable Diagnostic Logs in a Storage Account, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
<span data-ttu-id="fe3e9-172">K povolení protokolů diagnostiky v pracovním prostoru OMS použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-172">To Enable Diagnostics Logs in an OMS workspace, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a><span data-ttu-id="fe3e9-173">Použití protokolů diagnostiky ze služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="fe3e9-173">Consuming diagnostics logs from Azure Storage</span></span>
<span data-ttu-id="fe3e9-174">Tato část popisuje schéma základní analýza CDN, jak tyto jsou uspořádány v rámci účtu úložiště Azure a poskytuje ukázkový kód pro stažení protokolů v souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-174">This section describes the schema of the CDN core analytics, how these are organized inside of an Azure Storage Account and provides sample code to download the logs in to a CSV file.</span></span>

### <a name="using-microsoft-azure-storage-explorer"></a><span data-ttu-id="fe3e9-175">Pomocí Průzkumníka úložiště Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fe3e9-175">Using Microsoft Azure Storage Explorer</span></span>
<span data-ttu-id="fe3e9-176">Než se dostanete k základní analytická data z účtu úložiště Azure, musíte nejprve nástroj pro přístup k obsahu v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-176">Before you can access the core analytics data from the Azure Storage Account, you first need a tool to access the contents in a storage account.</span></span> <span data-ttu-id="fe3e9-177">Na trhu jsou k dispozici několik nástrojů, i když je ten, který doporučujeme Microsoft Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-177">While there are several tools available in the market, the one that we recommend is the Microsoft Azure Storage Explorer.</span></span> <span data-ttu-id="fe3e9-178">Si můžete stáhnout nástroj z [zde](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-178">You can download the tool from [here](http://storageexplorer.com/).</span></span> <span data-ttu-id="fe3e9-179">Po stažení a instalaci softwaru, nakonfigurujte ji používat stejný účet úložiště Azure, který byl nakonfigurovaný jako cíl do protokolů diagnostiky CDN.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-179">After downloading and installing the software, configure it to use the same Azure Storage Account that was configured as a destination to the CDN Diagnostics Logs.</span></span>

1.  <span data-ttu-id="fe3e9-180">Otevřete **Microsoft Azure Storage Explorer**</span><span class="sxs-lookup"><span data-stu-id="fe3e9-180">Open **Microsoft Azure Storage Explorer**</span></span>
2.  <span data-ttu-id="fe3e9-181">Najděte účet úložiště</span><span class="sxs-lookup"><span data-stu-id="fe3e9-181">Locate the storage account</span></span>
3.  <span data-ttu-id="fe3e9-182">Přejděte na **"Kontejnery objektů Blob"** uzel v rámci toto úložiště účtu a rozbalte uzel</span><span class="sxs-lookup"><span data-stu-id="fe3e9-182">Go to the **“Blob Containers”** node under this storage account and expand the node</span></span>
4.  <span data-ttu-id="fe3e9-183">Vyberte kontejner s názvem **"insights-logs-coreanalytics"** a poklikejte na něj</span><span class="sxs-lookup"><span data-stu-id="fe3e9-183">Select the container named **“insights-logs-coreanalytics”** and double-click it</span></span>
5.  <span data-ttu-id="fe3e9-184">Zobrazit výsledky nahoru v pravém podokně počínaje první úroveň, který vypadá podobně jako **"resourceId ="**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-184">Results show up on the right-hand pane starting with the first level, which looks like **“resourceId=”**.</span></span> <span data-ttu-id="fe3e9-185">Pokračujte kliknutím na úplně, dokud naleznete v souboru **PT1H.json**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-185">Continue clicking all the way until you see the file **PT1H.json**.</span></span> <span data-ttu-id="fe3e9-186">Viz následující poznámka vysvětlení cesty.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-186">See the following note for explanation of the path.</span></span>
6.  <span data-ttu-id="fe3e9-187">Každý objekt blob **PT1H.json** představuje analýzy protokolů pro jednu hodinu pro konkrétní koncový bod CDN nebo jeho vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-187">Each blob **PT1H.json** represents the analytics logs for one hour for a specific CDN endpoint or its custom domain.</span></span>
7.  <span data-ttu-id="fe3e9-188">Schéma obsah tohoto souboru JSON je popsaný v části schéma základní analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="fe3e9-188">The schema of the contents of this JSON file is described in the section Schema of the Core Analytics Logs</span></span>


> [!NOTE]
> <span data-ttu-id="fe3e9-189">**Formát cesty objektu BLOB**</span><span class="sxs-lookup"><span data-stu-id="fe3e9-189">**Blob path format**</span></span>
> 
> <span data-ttu-id="fe3e9-190">Základní analýza protokolů jsou generovány, každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-190">Core Analytics logs are generated every hour.</span></span> <span data-ttu-id="fe3e9-191">Všechna data pro jednu hodinu jsou shromážděných a uložených do jediného objektu Blob Azure jako datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-191">All data for an hour are collected and stored inside a single Azure Blob as a JSON payload.</span></span> <span data-ttu-id="fe3e9-192">Cesta k této objektů Blob v Azure se zobrazí, jako kdyby je hierarchická struktura.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-192">The path to this Azure Blob appears as if there is a hierarchical structure.</span></span> <span data-ttu-id="fe3e9-193">Toto je vzhledem k tomu, že nástroj Průzkumník úložišť interpretuje '/' za oddělovač adresářů a zobrazuje hierarchii ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-193">This is because the Storage explorer tool interprets '/' as a directory separator and shows the hierarchy for convenience.</span></span> <span data-ttu-id="fe3e9-194">Ve skutečnosti celé cesty právě představuje název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-194">Actually, the whole path just represents the blob name.</span></span> <span data-ttu-id="fe3e9-195">Tento název objektu blob postupuje podle následující konvence</span><span class="sxs-lookup"><span data-stu-id="fe3e9-195">This name of the blob follows the following naming convention</span></span>   
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

<span data-ttu-id="fe3e9-196">**Popis polí:**</span><span class="sxs-lookup"><span data-stu-id="fe3e9-196">**Description of fields:**</span></span>

|<span data-ttu-id="fe3e9-197">hodnota</span><span class="sxs-lookup"><span data-stu-id="fe3e9-197">value</span></span>|<span data-ttu-id="fe3e9-198">Popis</span><span class="sxs-lookup"><span data-stu-id="fe3e9-198">description</span></span>|
|-------|---------|
|<span data-ttu-id="fe3e9-199">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="fe3e9-199">Subscription ID</span></span>    |<span data-ttu-id="fe3e9-200">ID předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-200">ID of the Azure Subscription.</span></span> <span data-ttu-id="fe3e9-201">Toto je ve formátu Guid.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-201">This is in the Guid format.</span></span>|
|<span data-ttu-id="fe3e9-202">Prostředek</span><span class="sxs-lookup"><span data-stu-id="fe3e9-202">Resource</span></span> |<span data-ttu-id="fe3e9-203">Název skupiny název skupiny prostředků, do které patří prostředky CDN.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-203">Group Name   Name of the resource group to which the CDN resources belong.</span></span>|
|<span data-ttu-id="fe3e9-204">Název profilu</span><span class="sxs-lookup"><span data-stu-id="fe3e9-204">Profile Name</span></span> |<span data-ttu-id="fe3e9-205">Název profilu CDN</span><span class="sxs-lookup"><span data-stu-id="fe3e9-205">Name of the CDN Profile</span></span>|
|<span data-ttu-id="fe3e9-206">Název koncového bodu</span><span class="sxs-lookup"><span data-stu-id="fe3e9-206">Endpoint Name</span></span> |<span data-ttu-id="fe3e9-207">Název koncového bodu CDN</span><span class="sxs-lookup"><span data-stu-id="fe3e9-207">Name of the CDN Endpoint</span></span>|
|<span data-ttu-id="fe3e9-208">Rok</span><span class="sxs-lookup"><span data-stu-id="fe3e9-208">Year</span></span>|  <span data-ttu-id="fe3e9-209">reprezentace 4 číslice roku například 2017</span><span class="sxs-lookup"><span data-stu-id="fe3e9-209">4-digit representation of the year for example, 2017</span></span>|
|<span data-ttu-id="fe3e9-210">Měsíc</span><span class="sxs-lookup"><span data-stu-id="fe3e9-210">Month</span></span>| <span data-ttu-id="fe3e9-211">2 číslice reprezentace číslo měsíce.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-211">2-digit representation of the month number.</span></span> <span data-ttu-id="fe3e9-212">01 = leden... 12 = prosinec</span><span class="sxs-lookup"><span data-stu-id="fe3e9-212">01=January ... 12=December</span></span>|
|<span data-ttu-id="fe3e9-213">Den</span><span class="sxs-lookup"><span data-stu-id="fe3e9-213">Day</span></span>|   <span data-ttu-id="fe3e9-214">2 číslice reprezentace den v měsíci</span><span class="sxs-lookup"><span data-stu-id="fe3e9-214">2 digit representation of the day of the month</span></span>|
|<span data-ttu-id="fe3e9-215">PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="fe3e9-215">PT1H.json</span></span>| <span data-ttu-id="fe3e9-216">Skutečný soubor JSON se uloží analytická data</span><span class="sxs-lookup"><span data-stu-id="fe3e9-216">Actual JSON file where the analytics data is stored</span></span>|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a><span data-ttu-id="fe3e9-217">Export základní analytická Data do souboru CSV</span><span class="sxs-lookup"><span data-stu-id="fe3e9-217">Exporting the Core Analytics Data to a CSV File</span></span>

<span data-ttu-id="fe3e9-218">Chcete-li snadný přístup k základní analýza, poskytujeme ukázkový kód pro nějaký nástroj, který umožňuje stahování souborů JSON do nestrukturované textový soubor s oddělovači souboru ve formátu, který lze snadno vytvářet grafy nebo jiných agregací.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-218">To make it easy to access the Core Analytics, we provide a sample code for a tool, which allows downloading the JSON files into a flat comma-separated file format, which can be used to easily create charts or other aggregations.</span></span>

<span data-ttu-id="fe3e9-219">Zde je, jak můžete použít nástroj:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-219">Here is how you can use the tool:</span></span>

1.  <span data-ttu-id="fe3e9-220">Po kliknutí na odkaz githubu: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span><span class="sxs-lookup"><span data-stu-id="fe3e9-220">Visit the github link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span></span>
2.  <span data-ttu-id="fe3e9-221">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="fe3e9-221">Download the code</span></span>
3.  <span data-ttu-id="fe3e9-222">Postupujte podle pokynů pro kompilaci a konfigurace</span><span class="sxs-lookup"><span data-stu-id="fe3e9-222">Follow instructions to compile and configure</span></span>
4.  <span data-ttu-id="fe3e9-223">Spusťte nástroj</span><span class="sxs-lookup"><span data-stu-id="fe3e9-223">Run the tool</span></span>
5.  <span data-ttu-id="fe3e9-224">Výsledný soubor CSV zobrazuje analytická data v jednoduchých ploché hierarchii.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-224">Resulting CSV file shows the analytics data in a simple flat hierarchy.</span></span>

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a><span data-ttu-id="fe3e9-225">Použití protokolů diagnostiky z úložiště analýzy protokolů OMS</span><span class="sxs-lookup"><span data-stu-id="fe3e9-225">Consuming diagnostics logs from an OMS Log Analytics repository</span></span>
<span data-ttu-id="fe3e9-226">Analýzy protokolů je služba v Operations Management Suite (OMS), který monitoruje své cloudové a místní prostředí k udržování své dostupnosti a výkonu.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-226">Log Analytics is a service in Operations Management Suite (OMS) that monitors your cloud and on-premises environments to maintain their availability and performance.</span></span> <span data-ttu-id="fe3e9-227">Shromažďuje data generovaná prostředky ve vašem cloudovém a místním prostředí a také data z dalších nástrojů pro monitorování a poskytuje analýzy napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-227">It collects data generated by resources in your cloud and on-premises environments and from other monitoring tools to provide analysis across multiple sources.</span></span> 

<span data-ttu-id="fe3e9-228">Chcete-li použít analýzy protokolů, je nutné [povolit protokolování](#enable-logging-with-azure-storage) do úložiště analýzy protokolů Azure OMS, které je popsané výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-228">To use Log Analytics, you must [enable logging](#enable-logging-with-azure-storage) to the Azure OMS Log Analytics repository, which is discussed earlier in this article.</span></span>

### <a name="using-the-oms-repository"></a><span data-ttu-id="fe3e9-229">Použití úložiště OMS</span><span class="sxs-lookup"><span data-stu-id="fe3e9-229">Using the OMS Repository</span></span>

 <span data-ttu-id="fe3e9-230">Následující diagram znázorňuje architekturu vstupy a výstupy úložiště:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-230">The following diagram shows the architecture of the inputs and outputs of the repository:</span></span>

![Úložiště analýzy protokolů OMS](./media/cdn-diagnostics-log/12_Repo-overview.png)

<span data-ttu-id="fe3e9-232">*Obrázek 3 - úložiště analýzy protokolů*</span><span class="sxs-lookup"><span data-stu-id="fe3e9-232">*Figure 3 - Log Analytics Repository*</span></span>

<span data-ttu-id="fe3e9-233">Data můžete zobrazit v mnoha různými způsoby pomocí řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-233">You can display the data in a variety of ways by using Management Solutions.</span></span> <span data-ttu-id="fe3e9-234">Můžete získat řešení pro správu z [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-234">You can obtain Management Solutions from the [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span></span>

<span data-ttu-id="fe3e9-235">Řešení pro správu můžete nainstalovat z Azure marketplace kliknutím **ho získat** odkaz na konci každé řešení.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-235">You can install management solutions from Azure marketplace by clicking the **Get it now** link at the bottom of each solution.</span></span>

### <a name="adding-an-oms-cdn-management-solution"></a><span data-ttu-id="fe3e9-236">Přidávání do řešení pro správu OMS CDN</span><span class="sxs-lookup"><span data-stu-id="fe3e9-236">Adding an OMS CDN Management Solution</span></span>

<span data-ttu-id="fe3e9-237">Použijte následující postup přidání řešení pro správu:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-237">Follow these steps to add a Management Solution:</span></span>

1.   <span data-ttu-id="fe3e9-238">Pokud jste tak již neučinili, přihlaste se k portálu Azure pomocí svého předplatného Azure a přejděte na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-238">If you haven't already done so, sign in to the Azure portal using your Azure subscription and go to your Dashboard.</span></span>
    <span data-ttu-id="fe3e9-239">![Řídicí panel Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-239">![Azure Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span></span>

2. <span data-ttu-id="fe3e9-240">V **nový** okno pod **Marketplace**, vyberte **monitorování + správu**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-240">In the **New** blade under **Marketplace**, select **Monitoring + management**.</span></span>

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. <span data-ttu-id="fe3e9-242">V **monitorování + správu** okně klikněte na tlačítko **zobrazit všechny**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-242">In the **Monitoring + management** blade, click **See all**.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/15_See-all.png)

4.  <span data-ttu-id="fe3e9-244">Vyhledejte CDN do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-244">Search for CDN in the search box.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/16_Search-for.png)

5.  <span data-ttu-id="fe3e9-246">Vyberte **Azure CDN základní analýza**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-246">Select **Azure CDN Core Analytics**.</span></span> 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  <span data-ttu-id="fe3e9-248">Po kliknutí na **vytvořit**, zobrazí se výzva k vytvoření nové pracovní prostor OMS nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-248">After clicking **Create**, you will be asked to create a new OMS workspace or use an existing one.</span></span> 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  <span data-ttu-id="fe3e9-250">Vyberte pracovní prostor, který jste vytvořili před.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-250">Select the workspace you created before.</span></span> <span data-ttu-id="fe3e9-251">Pak je potřeba přidat účet automation.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-251">You then need to add an automation account.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/19_Add-automation.png)

8. <span data-ttu-id="fe3e9-253">Na následující obrazovce se zobrazí formulář účet automation, který je nutné vyplnit.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-253">The following screen shows the automation account form you must fill out.</span></span> 

    ![Zobrazit všechno](./media/cdn-diagnostics-log/20_Automation.png)

9. <span data-ttu-id="fe3e9-255">Po vytvoření účtu automation, jste připraveni přidat řešení.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-255">Once you have created the automation account, you are ready to add your solution.</span></span> <span data-ttu-id="fe3e9-256">Klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-256">Click the **Create** button.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/21_Ready.png)

10. <span data-ttu-id="fe3e9-258">Řešení teď přidaná do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-258">Your solution has now been added to your workspace.</span></span> <span data-ttu-id="fe3e9-259">Přejděte zpět na portálu Azure řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-259">Go back to your Azure portal Dashboard.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/22_Dashboard.png)

    <span data-ttu-id="fe3e9-261">Klikněte na pracovní prostor analýzy protokolů, které jste vytvořili pro přejděte do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-261">Click the Log Analytics workspace you created to go to your workspace.</span></span> 

11. <span data-ttu-id="fe3e9-262">Klikněte **portálu OMS** dlaždice zobrazíte nové řešení na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-262">Click the **OMS Portal** tile to see your new solution in the OMS portal.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/23_workspace.png)

12. <span data-ttu-id="fe3e9-264">Na portálu OMS by teď měl vypadat jako následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-264">Your OMS portal should now look like the following screen:</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/24_OMS-solution.png)

    <span data-ttu-id="fe3e9-266">Klikněte na jednu z dlaždice zobrazíte několik zobrazení na vaše data.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-266">Click one of the tiles to see several views into your data.</span></span>

    ![Zobrazit všechno](./media/cdn-diagnostics-log/25_Interior-view.png)

    <span data-ttu-id="fe3e9-268">Můžete se posunete doleva nebo doprava zobrazíte další dlaždice představující jednotlivé zobrazení do data.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-268">You can scroll left or right to see further tiles representing individual views into the data.</span></span> 

    <span data-ttu-id="fe3e9-269">Kliknutím na jedno ze dlaždice získáte další informace o data.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-269">Clicking one of the tiles gives you more details about your data.</span></span>

     ![Zobrazit všechno](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a><span data-ttu-id="fe3e9-271">Nabídky a cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="fe3e9-271">Offers and pricing tiers</span></span>

<span data-ttu-id="fe3e9-272">Můžete zobrazit nabídky a cenové úrovně pro řešení pro správu OMS [zde](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span><span class="sxs-lookup"><span data-stu-id="fe3e9-272">You can see offers and pricing tiers for OMS management solutions [here](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span></span>

### <a name="customizing-views"></a><span data-ttu-id="fe3e9-273">Přizpůsobení zobrazení</span><span class="sxs-lookup"><span data-stu-id="fe3e9-273">Customizing views</span></span>

<span data-ttu-id="fe3e9-274">Zobrazení lze přizpůsobit do vašich dat pomocí **Návrhář zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-274">You can customize the view into your data by using the **View Designer**.</span></span> <span data-ttu-id="fe3e9-275">Přejděte do pracovního prostoru OMS a začnete navrhovat kliknutím **Návrhář zobrazení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-275">Go to your OMS workspace and begin designing by clicking the **View Designer** tile.</span></span>

![Návrhář zobrazení](./media/cdn-diagnostics-log/27_Designer.png)

<span data-ttu-id="fe3e9-277">Můžete přetáhnout a vyřadit typy grafů zleva a zadejte podrobnosti dat, který chcete analyzovat na levé straně.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-277">You can drag and drop types of charts from the left and fill in the data details you want to analyze on the left.</span></span>

![Návrhář zobrazení](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a><span data-ttu-id="fe3e9-279">Zpoždění data protokolu</span><span class="sxs-lookup"><span data-stu-id="fe3e9-279">Log data delays</span></span>

<span data-ttu-id="fe3e9-280">Verizon protokolu data zpoždění</span><span class="sxs-lookup"><span data-stu-id="fe3e9-280">Verizon log data delays</span></span> | <span data-ttu-id="fe3e9-281">Akamai protokolu data zpoždění</span><span class="sxs-lookup"><span data-stu-id="fe3e9-281">Akamai log data delays</span></span>
--- | ---
<span data-ttu-id="fe3e9-282">Data protokolu Verizon je zpožděno 1 hodinu a trvat až 2 hodin zahájíte zobrazování po dokončení šíření koncový bod.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-282">Verizon log data is 1 hour delayed, and take up to 2 hours to start appearing after endpoint propagation completion.</span></span> | <span data-ttu-id="fe3e9-283">Data protokolu Akamai je 24 hodin, zpoždění a trvá až 2 hodin start, zobrazování, pokud byla vytvořena více než 24 hodinami.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-283">Akamai log data is 24 hours delayed, and takes up to 2 hours to start appearing if it was created more than 24 hours ago.</span></span> <span data-ttu-id="fe3e9-284">Pokud byl nedávno vytvořen, může trvat až 25 hodin pro protokoly spuštění, které jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-284">If it was recently created, it can take up to 25 hours for the logs to start appearing.</span></span>

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a><span data-ttu-id="fe3e9-285">Typy protokolů diagnostiky pro CDN základní analýza</span><span class="sxs-lookup"><span data-stu-id="fe3e9-285">Diagnostic log types for CDN Core Analytics</span></span>

<span data-ttu-id="fe3e9-286">Nabízíme aktuálně pouze základní analýza protokoly, které obsahují metriky ukazující statistiky odpovědi HTTP a odchozí statistiky, jak je vidět z CDN POP nebo okrajů.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-286">We currently offer only Core Analytics logs, which contain metrics showing HTTP response statistics and egress statistics as seen from the CDN POPs/edges.</span></span>

### <a name="core-analytics-metrics-details"></a><span data-ttu-id="fe3e9-287">Základní analýza metriky podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fe3e9-287">Core Analytics Metrics Details</span></span>
<span data-ttu-id="fe3e9-288">Následující tabulka uvádí seznam metriky, které jsou k dispozici v základní Analýza protokolů.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-288">The following table shows a list of metrics available in the Core Analytics logs.</span></span> <span data-ttu-id="fe3e9-289">Ne všechny metriky jsou k dispozici od všech poskytovatelů, i když tyto rozdíly jsou minimální.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-289">Not all metrics are available from all providers, although such differences are minimal.</span></span> <span data-ttu-id="fe3e9-290">V následující tabulce také ukazuje, pokud daná metrika je k dispozici od zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-290">The following table also shows if a given metric is available from a provider.</span></span> <span data-ttu-id="fe3e9-291">Upozorňujeme, že jsou k dispozici pro jenom ty koncové body CDN mající přenosy na těchto metriky.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-291">Please note that the metrics are available for only those CDN endpoints that have traffic on them.</span></span>


|<span data-ttu-id="fe3e9-292">Metrika</span><span class="sxs-lookup"><span data-stu-id="fe3e9-292">Metric</span></span>                     | <span data-ttu-id="fe3e9-293">Popis</span><span class="sxs-lookup"><span data-stu-id="fe3e9-293">Description</span></span>   | <span data-ttu-id="fe3e9-294">Verizon</span><span class="sxs-lookup"><span data-stu-id="fe3e9-294">Verizon</span></span>  | <span data-ttu-id="fe3e9-295">Akamai</span><span class="sxs-lookup"><span data-stu-id="fe3e9-295">Akamai</span></span> 
|---------------------------|---------------|---|---|
| <span data-ttu-id="fe3e9-296">RequestCountTotal</span><span class="sxs-lookup"><span data-stu-id="fe3e9-296">RequestCountTotal</span></span>         |<span data-ttu-id="fe3e9-297">Celkový počet přístupů požadavek během tohoto období</span><span class="sxs-lookup"><span data-stu-id="fe3e9-297">Total number of request hits during this period</span></span>| <span data-ttu-id="fe3e9-298">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-298">Yes</span></span>  |<span data-ttu-id="fe3e9-299">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-299">Yes</span></span>   |
| <span data-ttu-id="fe3e9-300">RequestCountHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-300">RequestCountHttpStatus2xx</span></span> |<span data-ttu-id="fe3e9-301">Počet všech požadavků, které 2xx kód HTTP (např. 200, 202)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-301">Count of all requests that resulted in a 2xx HTTP code (e.g. 200, 202)</span></span>              | <span data-ttu-id="fe3e9-302">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-302">Yes</span></span>  |<span data-ttu-id="fe3e9-303">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-303">Yes</span></span>   |
| <span data-ttu-id="fe3e9-304">RequestCountHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-304">RequestCountHttpStatus3xx</span></span> | <span data-ttu-id="fe3e9-305">Počet všech požadavků, které 3xx kód HTTP (např. 300, 302)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-305">Count of all requests that resulted in a 3xx HTTP code (e.g. 300, 302)</span></span>              | <span data-ttu-id="fe3e9-306">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-306">Yes</span></span>  |<span data-ttu-id="fe3e9-307">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-307">Yes</span></span>   |
| <span data-ttu-id="fe3e9-308">RequestCountHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-308">RequestCountHttpStatus4xx</span></span> |<span data-ttu-id="fe3e9-309">Počet všech požadavků, které 4xx kód HTTP (např. 400, 404)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-309">Count of all requests that resulted in a 4xx HTTP code (e.g. 400, 404)</span></span>               | <span data-ttu-id="fe3e9-310">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-310">Yes</span></span>   |<span data-ttu-id="fe3e9-311">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-311">Yes</span></span>   |
| <span data-ttu-id="fe3e9-312">RequestCountHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-312">RequestCountHttpStatus5xx</span></span> | <span data-ttu-id="fe3e9-313">Počet všech požadavků, jejichž výsledkem kód HTTP 5xx (např. 500, 504)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-313">Count of all requests that resulted in a 5xx HTTP code (e.g. 500, 504)</span></span>              | <span data-ttu-id="fe3e9-314">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-314">Yes</span></span>  |<span data-ttu-id="fe3e9-315">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-315">Yes</span></span>   |
| <span data-ttu-id="fe3e9-316">RequestCountHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="fe3e9-316">RequestCountHttpStatusOthers</span></span> |  <span data-ttu-id="fe3e9-317">Počet všechny ostatní kódy HTTP (mimo 2xx 5xx)</span><span class="sxs-lookup"><span data-stu-id="fe3e9-317">Count of all other HTTP codes (outside of 2xx-5xx)</span></span> | <span data-ttu-id="fe3e9-318">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-318">Yes</span></span>  |<span data-ttu-id="fe3e9-319">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-319">Yes</span></span>   |
| <span data-ttu-id="fe3e9-320">RequestCountHttpStatus200</span><span class="sxs-lookup"><span data-stu-id="fe3e9-320">RequestCountHttpStatus200</span></span> | <span data-ttu-id="fe3e9-321">Počet všech požadavků, jejichž výsledkem 200 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="fe3e9-321">Count of all requests that resulted in a 200 HTTP code response</span></span>              |<span data-ttu-id="fe3e9-322">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-322">No</span></span>   |<span data-ttu-id="fe3e9-323">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-323">Yes</span></span>   |
| <span data-ttu-id="fe3e9-324">RequestCountHttpStatus206</span><span class="sxs-lookup"><span data-stu-id="fe3e9-324">RequestCountHttpStatus206</span></span> | <span data-ttu-id="fe3e9-325">Počet všech požadavků, jejichž výsledkem 206 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="fe3e9-325">Count of all requests that resulted in a 206 HTTP code response</span></span>              |<span data-ttu-id="fe3e9-326">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-326">No</span></span>   |<span data-ttu-id="fe3e9-327">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-327">Yes</span></span>   |
| <span data-ttu-id="fe3e9-328">RequestCountHttpStatus302</span><span class="sxs-lookup"><span data-stu-id="fe3e9-328">RequestCountHttpStatus302</span></span> | <span data-ttu-id="fe3e9-329">Počet všech požadavků, jejichž výsledkem 302 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="fe3e9-329">Count of all requests that resulted in a 302 HTTP code response</span></span>              |<span data-ttu-id="fe3e9-330">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-330">No</span></span>   |<span data-ttu-id="fe3e9-331">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-331">Yes</span></span>   |
| <span data-ttu-id="fe3e9-332">RequestCountHttpStatus304</span><span class="sxs-lookup"><span data-stu-id="fe3e9-332">RequestCountHttpStatus304</span></span> |  <span data-ttu-id="fe3e9-333">Počet všech požadavků, jejichž výsledkem 304 kód odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="fe3e9-333">Count of all requests that resulted in a 304 HTTP code response</span></span>             |<span data-ttu-id="fe3e9-334">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-334">No</span></span>   |<span data-ttu-id="fe3e9-335">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-335">Yes</span></span>   |
| <span data-ttu-id="fe3e9-336">RequestCountHttpStatus404</span><span class="sxs-lookup"><span data-stu-id="fe3e9-336">RequestCountHttpStatus404</span></span> | <span data-ttu-id="fe3e9-337">Počet všech požadavků, jejichž výsledkem kódu odpovědi HTTP 404</span><span class="sxs-lookup"><span data-stu-id="fe3e9-337">Count of all requests that resulted in a 404 HTTP code response</span></span>              |<span data-ttu-id="fe3e9-338">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-338">No</span></span>   |<span data-ttu-id="fe3e9-339">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-339">Yes</span></span>   |
| <span data-ttu-id="fe3e9-340">RequestCountCacheHit</span><span class="sxs-lookup"><span data-stu-id="fe3e9-340">RequestCountCacheHit</span></span> |<span data-ttu-id="fe3e9-341">Počet všech požadavků, jejichž výsledkem požadavků mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-341">Count of all requests that resulted in a Cache Hit.</span></span> <span data-ttu-id="fe3e9-342">To znamená, že zpracování asset přímo z POP do klienta.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-342">This means the asset was served directly from the POP to the Client.</span></span>               | <span data-ttu-id="fe3e9-343">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-343">Yes</span></span>  |<span data-ttu-id="fe3e9-344">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-344">No</span></span>   |
| <span data-ttu-id="fe3e9-345">RequestCountCacheMiss</span><span class="sxs-lookup"><span data-stu-id="fe3e9-345">RequestCountCacheMiss</span></span> | <span data-ttu-id="fe3e9-346">Počet všech požadavků, která byla vygenerována v neúspěšnému přístupu do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-346">Count of all requests that resulted in a Cache Miss.</span></span> <span data-ttu-id="fe3e9-347">To znamená asset nebyla nalezena na serveru POP nejbližší klientovi a proto byla načtena z tohoto počátku.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-347">This means the asset was not found on the POP closest to the client, and therefore was retrieved from the Origin.</span></span>              |<span data-ttu-id="fe3e9-348">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-348">Yes</span></span>   | <span data-ttu-id="fe3e9-349">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-349">No</span></span>  |
| <span data-ttu-id="fe3e9-350">RequestCountCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="fe3e9-350">RequestCountCacheNoCache</span></span> | <span data-ttu-id="fe3e9-351">Počet všech požadavků na prostředek, které nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele na hranici.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-351">Count of all requests to an asset that are prevented from being cached due to a user configuration on the edge.</span></span>              |<span data-ttu-id="fe3e9-352">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-352">Yes</span></span>   | <span data-ttu-id="fe3e9-353">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-353">No</span></span>  |
| <span data-ttu-id="fe3e9-354">RequestCountCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="fe3e9-354">RequestCountCacheUncacheable</span></span> | <span data-ttu-id="fe3e9-355">Počet všech požadavků na prostředky, které nebudou moci ukládat do mezipaměti asset Cache-Control a Expires hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klient HTTP</span><span class="sxs-lookup"><span data-stu-id="fe3e9-355">Count of all requests to assets that are prevented from being cached by the asset's Cache-Control and Expires headers, which indicate that it should not be cached on a POP or by the HTTP client</span></span>                |<span data-ttu-id="fe3e9-356">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-356">Yes</span></span>   |<span data-ttu-id="fe3e9-357">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-357">No</span></span>   |
| <span data-ttu-id="fe3e9-358">RequestCountCacheOthers</span><span class="sxs-lookup"><span data-stu-id="fe3e9-358">RequestCountCacheOthers</span></span> | <span data-ttu-id="fe3e9-359">Počet všech požadavků stavem mezipaměti, které nejsou pokryty výše.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-359">Count of all requests with cache status not covered by above.</span></span>              |<span data-ttu-id="fe3e9-360">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-360">Yes</span></span>   | <span data-ttu-id="fe3e9-361">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-361">No</span></span>  |
| <span data-ttu-id="fe3e9-362">EgressTotal</span><span class="sxs-lookup"><span data-stu-id="fe3e9-362">EgressTotal</span></span> | <span data-ttu-id="fe3e9-363">Odchozí přenosy dat v GB</span><span class="sxs-lookup"><span data-stu-id="fe3e9-363">Outbound data transfer in GB</span></span>              |<span data-ttu-id="fe3e9-364">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-364">Yes</span></span>   |<span data-ttu-id="fe3e9-365">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-365">Yes</span></span>   |
| <span data-ttu-id="fe3e9-366">EgressHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-366">EgressHttpStatus2xx</span></span> | <span data-ttu-id="fe3e9-367">Odchozí datové přenosy * pro odpovědi s stavové kódy HTTP 2xx v GB</span><span class="sxs-lookup"><span data-stu-id="fe3e9-367">Outbound data transfer* for responses with 2xx HTTP status codes in GB</span></span>            |<span data-ttu-id="fe3e9-368">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-368">Yes</span></span>   |<span data-ttu-id="fe3e9-369">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-369">No</span></span>   |
| <span data-ttu-id="fe3e9-370">EgressHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-370">EgressHttpStatus3xx</span></span> | <span data-ttu-id="fe3e9-371">Přenos odchozích dat pro odpovědi s stavové kódy HTTP 3xx v GB</span><span class="sxs-lookup"><span data-stu-id="fe3e9-371">Outbound data transfer for responses with 3xx HTTP status codes in GB</span></span>              |<span data-ttu-id="fe3e9-372">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-372">Yes</span></span>   |<span data-ttu-id="fe3e9-373">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-373">No</span></span>   |
| <span data-ttu-id="fe3e9-374">EgressHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-374">EgressHttpStatus4xx</span></span> | <span data-ttu-id="fe3e9-375">Přenos odchozích dat pro odpovědi s stavové kódy HTTP 4xx v GB</span><span class="sxs-lookup"><span data-stu-id="fe3e9-375">Outbound data transfer for responses with 4xx HTTP status codes in GB</span></span>               |<span data-ttu-id="fe3e9-376">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-376">Yes</span></span>   | <span data-ttu-id="fe3e9-377">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-377">No</span></span>  |
| <span data-ttu-id="fe3e9-378">EgressHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="fe3e9-378">EgressHttpStatus5xx</span></span> | <span data-ttu-id="fe3e9-379">Přenos odchozích dat pro odpovědi s stavové kódy HTTP 5xx v GB</span><span class="sxs-lookup"><span data-stu-id="fe3e9-379">Outbound data transfer for responses with 5xx HTTP status codes in GB</span></span>               |<span data-ttu-id="fe3e9-380">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-380">Yes</span></span>   |  <span data-ttu-id="fe3e9-381">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-381">No</span></span> |
| <span data-ttu-id="fe3e9-382">EgressHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="fe3e9-382">EgressHttpStatusOthers</span></span> | <span data-ttu-id="fe3e9-383">Odchozí přenosy dat pro odpovědi s ostatních stavových kódech HTTP v GB</span><span class="sxs-lookup"><span data-stu-id="fe3e9-383">Outbound data transfer for responses with other HTTP status codes in GB</span></span>                |<span data-ttu-id="fe3e9-384">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-384">Yes</span></span>   |<span data-ttu-id="fe3e9-385">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-385">No</span></span>   |
| <span data-ttu-id="fe3e9-386">EgressCacheHit</span><span class="sxs-lookup"><span data-stu-id="fe3e9-386">EgressCacheHit</span></span> |  <span data-ttu-id="fe3e9-387">Odchozí přenosy dat pro odpovědi, které byly doručeny přímo z mezipaměti CDN na CDN POP nebo okrajů</span><span class="sxs-lookup"><span data-stu-id="fe3e9-387">Outbound data transfer for responses that were delivered directly from the CDN cache on the CDN POPs/Edges</span></span>  |<span data-ttu-id="fe3e9-388">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-388">Yes</span></span>   |  <span data-ttu-id="fe3e9-389">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-389">No</span></span> |
| <span data-ttu-id="fe3e9-390">EgressCacheMiss</span><span class="sxs-lookup"><span data-stu-id="fe3e9-390">EgressCacheMiss</span></span> | <span data-ttu-id="fe3e9-391">Odchozí přenosy dat pro odpovědi, které nebyly na nejbližší server POP najít a načíst ze zdrojového serveru</span><span class="sxs-lookup"><span data-stu-id="fe3e9-391">Outbound data transfer for responses that were not found on the nearest POP server, and retrieved from the origin server</span></span>              |<span data-ttu-id="fe3e9-392">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-392">Yes</span></span>   |  <span data-ttu-id="fe3e9-393">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-393">No</span></span> |
| <span data-ttu-id="fe3e9-394">EgressCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="fe3e9-394">EgressCacheNoCache</span></span> | <span data-ttu-id="fe3e9-395">Přenos odchozích dat pro prostředky, které nebudou moci ukládat do mezipaměti z důvodu konfigurace uživatele na hranici.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-395">Outbound data transfer for assets that are prevented from being cached due to a user configuration on the edge.</span></span>                |<span data-ttu-id="fe3e9-396">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-396">Yes</span></span>   |<span data-ttu-id="fe3e9-397">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-397">No</span></span>   |
| <span data-ttu-id="fe3e9-398">EgressCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="fe3e9-398">EgressCacheUncacheable</span></span> | <span data-ttu-id="fe3e9-399">Odchozí datové přenosy pro prostředky, které nebudou moci ukládat do mezipaměti asset Cache-Control nebo Expires hlavičky, které označují, že by neměl být uložen do mezipaměti, na serveru POP nebo klient HTTP</span><span class="sxs-lookup"><span data-stu-id="fe3e9-399">Outbound data transfer for assets that are prevented from being cached by the asset's Cache-Control and/or Expires headers, which indicate that it should not be cached on a POP or by the HTTP client</span></span>                    |<span data-ttu-id="fe3e9-400">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-400">Yes</span></span>   | <span data-ttu-id="fe3e9-401">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-401">No</span></span>  |
| <span data-ttu-id="fe3e9-402">EgressCacheOthers</span><span class="sxs-lookup"><span data-stu-id="fe3e9-402">EgressCacheOthers</span></span> |  <span data-ttu-id="fe3e9-403">Odchozí datové přenosy s dalšími scénáři mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-403">Outbound data transfers for other cache scenarios.</span></span>             |<span data-ttu-id="fe3e9-404">Ano</span><span class="sxs-lookup"><span data-stu-id="fe3e9-404">Yes</span></span>   | <span data-ttu-id="fe3e9-405">Ne</span><span class="sxs-lookup"><span data-stu-id="fe3e9-405">No</span></span>  |

<span data-ttu-id="fe3e9-406">* Odchozí přenosy dat odkazuje na provoz klientovi předána ze serverů CDN POP.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-406">*Outbound data transfer refers to traffic delivered from CDN POP servers to the client.</span></span>


### <a name="schema-of-the-core-analytics-logs"></a><span data-ttu-id="fe3e9-407">Schéma základní analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="fe3e9-407">Schema of the Core Analytics Logs</span></span> 

<span data-ttu-id="fe3e9-408">Všechny protokoly se ukládají ve formátu JSON a každá položka má následující pole řetězce níže schématu:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-408">All logs are stored in JSON format and each entry has string fields following the below schema:</span></span>

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
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

<span data-ttu-id="fe3e9-409">Kde 'čas' představuje čas zahájení hodinu hranic, pro který je hlášen statistik.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-409">Where the ‘time’ represents the start time of the hour boundary for which the statistics is reported.</span></span> <span data-ttu-id="fe3e9-410">Pokud metriky není podporována zprostředkovatelem CDN, místo hodnotu double nebo celé číslo, bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-410">When a metric is not supported by a CDN provider, instead of a double or integer value, there will be a null value.</span></span> <span data-ttu-id="fe3e9-411">Tato hodnota null ukazuje na nepřítomnost metriky a tento proces se liší od hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-411">This null value indicates the absence of a metric, and this is different from a 0 value.</span></span> <span data-ttu-id="fe3e9-412">Všimněte si také, že bude jednu sadu tyto metriky v každé doméně nakonfigurovaný v koncovém bodě.</span><span class="sxs-lookup"><span data-stu-id="fe3e9-412">Also note that there will be one set of these metrics per domain configured on the endpoint.</span></span>

<span data-ttu-id="fe3e9-413">Příklad vlastnosti níže:</span><span class="sxs-lookup"><span data-stu-id="fe3e9-413">Example properties below:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="fe3e9-414">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fe3e9-414">Additional resources</span></span>

* [<span data-ttu-id="fe3e9-415">Azure diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="fe3e9-415">Azure Diagnostic logs</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [<span data-ttu-id="fe3e9-416">Základní analýza prostřednictvím doplňkovém portálu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="fe3e9-416">Core analytics via Azure CDN supplemental portal</span></span>](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [<span data-ttu-id="fe3e9-417">Analýzy protokolů Azure OMS</span><span class="sxs-lookup"><span data-stu-id="fe3e9-417">Azure OMS Log Analytics</span></span>](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [<span data-ttu-id="fe3e9-418">Analýza protokolů Azure rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="fe3e9-418">Azure Log Analytics REST API</span></span>](https://docs.microsoft.com/en-us/rest/api/loganalytics)







