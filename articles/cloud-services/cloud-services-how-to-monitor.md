---
title: "Postup sledování cloudové služby | Microsoft Docs"
description: "Naučte se monitorovat cloudové služby pomocí portálu Azure classic."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: c369b22cf068a473343b006eb1b06fdd350d31db
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-monitor-cloud-services"></a><span data-ttu-id="51337-103">Jak monitorovat Cloud Services</span><span class="sxs-lookup"><span data-stu-id="51337-103">How to Monitor Cloud Services</span></span>
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

<span data-ttu-id="51337-104">Můžete sledovat `key` metrika výkonu pro vaše cloudové služby na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="51337-104">You can monitor `key` performance metrics for your cloud services in the Azure classic portal.</span></span> <span data-ttu-id="51337-105">Můžete nastavit úroveň sledování pro minimální a podrobné pro každou roli služby a můžete přizpůsobit monitorování zobrazí.</span><span class="sxs-lookup"><span data-stu-id="51337-105">You can set the level of monitoring to minimal and verbose for each service role, and can customize the monitoring displays.</span></span> <span data-ttu-id="51337-106">Podrobné monitorování data se ukládají v účtu úložiště, které je přístupné mimo portál.</span><span class="sxs-lookup"><span data-stu-id="51337-106">Verbose monitoring data is stored in a storage account, which you can access outside the portal.</span></span> 

<span data-ttu-id="51337-107">Monitorování zobrazí na portálu Azure classic jsou vysoce konfigurovatelné.</span><span class="sxs-lookup"><span data-stu-id="51337-107">Monitoring displays in the Azure classic portal are highly configurable.</span></span> <span data-ttu-id="51337-108">Můžete vybrat podle metrik, které chcete monitorovat v seznamu metriky na **monitorování** stránky a vy můžete zvolit, které metriky k vykreslení v metriky grafy na **monitorování** stránku a řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="51337-108">You can choose the metrics you want to monitor in the metrics list on the **Monitor** page, and you can choose which metrics to plot in metrics charts on the **Monitor** page and the dashboard.</span></span> 

## <a name="concepts"></a><span data-ttu-id="51337-109">Koncepty</span><span class="sxs-lookup"><span data-stu-id="51337-109">Concepts</span></span>
<span data-ttu-id="51337-110">Ve výchozím nastavení se minimální monitorování poskytuje pro novou cloudovou službu pomocí čítače výkonu získané z hostitelského operačního systému pro instance rolí (virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="51337-110">By default, minimal monitoring is provided for a new cloud service using performance counters gathered from the host operating system for the roles instances (virtual machines).</span></span> <span data-ttu-id="51337-111">Minimální metriky jsou omezeny na procento využití procesoru, Data v, Data se, propustnost čtení disku a zápis propustnost disku.</span><span class="sxs-lookup"><span data-stu-id="51337-111">The minimal metrics are limited to CPU Percentage, Data In, Data Out, Disk Read Throughput, and Disk Write Throughput.</span></span> <span data-ttu-id="51337-112">Podle konfigurace, podrobného sledování, může přijímat další metriky na základě dat o výkonu v rámci virtuálních počítačů (instance rolí).</span><span class="sxs-lookup"><span data-stu-id="51337-112">By configuring verbose monitoring, you can receive additional metrics based on performance data within the virtual machines (role instances).</span></span> <span data-ttu-id="51337-113">Podrobné metriky povolit blíže analyzovat problémy, ke kterým došlo během operací aplikace.</span><span class="sxs-lookup"><span data-stu-id="51337-113">The verbose metrics enable closer analysis of issues that occur during application operations.</span></span>

<span data-ttu-id="51337-114">Ve výchozím nastavení se data čítače výkonu z instancí role vzorků a přenést z role instance v intervalech 3 minut.</span><span class="sxs-lookup"><span data-stu-id="51337-114">By default performance counter data from role instances is sampled and transferred from the role instance at 3-minute intervals.</span></span> <span data-ttu-id="51337-115">Pokud povolíte podrobné monitorování, hrubý výkon čítač agregovaná data pro každou instanci role a napříč instancemi role pro každou roli v intervalech 5 minut, 1 hodina a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="51337-115">When you enable verbose monitoring, the raw performance counter data is aggregated for each role instance and across role instances for each role at intervals of 5 minutes, 1 hour, and 12 hours.</span></span> <span data-ttu-id="51337-116">Agregovaná data se vyprazdňují po 10 dní.</span><span class="sxs-lookup"><span data-stu-id="51337-116">The aggregated data is purged after 10 days.</span></span>

<span data-ttu-id="51337-117">Po povolení podrobného monitorování agregovaná data monitorování je ukládat do tabulek ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="51337-117">After you enable verbose monitoring, the aggregated monitoring data is stored in tables in your storage account.</span></span> <span data-ttu-id="51337-118">Pokud chcete povolit podrobné sledování pro roli, musíte nakonfigurovat připojovací řetězec diagnostiky odkazující na účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="51337-118">To enable verbose monitoring for a role, you must configure a diagnostics connection string that links to the storage account.</span></span> <span data-ttu-id="51337-119">Jiným účtům úložiště můžete použít pro různé role.</span><span class="sxs-lookup"><span data-stu-id="51337-119">You can use different storage accounts for different roles.</span></span>

<span data-ttu-id="51337-120">Povolení podrobného monitorování zvyšuje náklady na úložiště týkající se úložiště dat, přenos dat a úložiště transakce.</span><span class="sxs-lookup"><span data-stu-id="51337-120">Enabling verbose monitoring increases your storage costs related to data storage, data transfer, and storage transactions.</span></span> <span data-ttu-id="51337-121">Minimální monitorování nevyžaduje žádný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="51337-121">Minimal monitoring does not require a storage account.</span></span> <span data-ttu-id="51337-122">Data pro metriky, které jsou umístěné na minimální monitorování úrovni nejsou uložené v účtu úložiště, i v případě, že nastavíte úroveň monitorování na podrobné.</span><span class="sxs-lookup"><span data-stu-id="51337-122">The data for the metrics that are exposed at the minimal monitoring level are not stored in your storage account, even if you set the monitoring level to verbose.</span></span>

## <a name="how-to-configure-monitoring-for-cloud-services"></a><span data-ttu-id="51337-123">Postupy: Konfigurace monitorování pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="51337-123">How to: Configure monitoring for cloud services</span></span>
<span data-ttu-id="51337-124">Následující postupy použijte ke konfiguraci podrobné nebo minimální monitorování na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="51337-124">Use the following procedures to configure verbose or minimal monitoring in the Azure classic portal.</span></span> 

### <a name="before-you-begin"></a><span data-ttu-id="51337-125">Než začnete</span><span class="sxs-lookup"><span data-stu-id="51337-125">Before you begin</span></span>
* <span data-ttu-id="51337-126">Vytvoření *classic* účet úložiště pro ukládání dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="51337-126">Create a *classic* storage account to store the monitoring data.</span></span> <span data-ttu-id="51337-127">Jiným účtům úložiště můžete použít pro různé role.</span><span class="sxs-lookup"><span data-stu-id="51337-127">You can use different storage accounts for different roles.</span></span> <span data-ttu-id="51337-128">Další informace najdete v tématu [postup vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="51337-128">For more information, see [How to create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="51337-129">Povolte Azure Diagnostics pro role cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="51337-129">Enable Azure Diagnostics for your cloud service roles.</span></span> <span data-ttu-id="51337-130">V tématu [konfigurace diagnostiky pro cloudové služby](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="51337-130">See [Configuring Diagnostics for Cloud Services](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="51337-131">Zajistěte, aby byl diagnostiky připojovací řetězec v konfiguraci Role.</span><span class="sxs-lookup"><span data-stu-id="51337-131">Ensure that the diagnostics connection string is present in the Role configuration.</span></span> <span data-ttu-id="51337-132">Nelze povolit, podrobného sledování, dokud povolíte Azure Diagnostics a zahrnout diagnostiky připojovací řetězec do konfigurace rolí.</span><span class="sxs-lookup"><span data-stu-id="51337-132">You cannot turn on verbose monitoring until you enable Azure Diagnostics and include a diagnostics connection string in the Role configuration.</span></span>   

> [!NOTE]
> <span data-ttu-id="51337-133">Projektech zacílených na Azure SDK 2.5 automaticky neobsahovala diagnostiky připojovací řetězec v šabloně projektů.</span><span class="sxs-lookup"><span data-stu-id="51337-133">Projects targeting Azure SDK 2.5 did not automatically include the diagnostics connection string in the project template.</span></span> <span data-ttu-id="51337-134">Pro tyto projekty je nutné ručně přidat připojovací řetězec diagnostiky ke konfiguraci Role.</span><span class="sxs-lookup"><span data-stu-id="51337-134">For these projects, you need to manually add the diagnostics connection string to the Role configuration.</span></span>
> 
> 

<span data-ttu-id="51337-135">**Ručně přidat diagnostiky připojovací řetězec do konfigurace rolí**</span><span class="sxs-lookup"><span data-stu-id="51337-135">**To manually add diagnostics connection string to Role configuration**</span></span>

1. <span data-ttu-id="51337-136">Otevřete projekt cloudové služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51337-136">Open the Cloud Service project in Visual Studio</span></span>
2. <span data-ttu-id="51337-137">Dvakrát klikněte na **Role** otevřete roli návrháře a vyberte možnost **nastavení** karta</span><span class="sxs-lookup"><span data-stu-id="51337-137">Double-click on the **Role** to open the Role designer and select the **Settings** tab</span></span>
3. <span data-ttu-id="51337-138">Vyhledejte nastavení s názvem **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="51337-138">Look for a setting named **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**.</span></span> 
4. <span data-ttu-id="51337-139">Pokud toto nastavení není zadán, klikněte na **přidat nastavení** tlačítko přidejte ke konfiguraci a změnit typ pro nové nastavení, které **ConnectionString**</span><span class="sxs-lookup"><span data-stu-id="51337-139">If this setting is not present, click on the **Add Setting** button to add it to the configuration and change the type for the new setting to **ConnectionString**</span></span>
5. <span data-ttu-id="51337-140">Nastavit hodnotu pro připojovací řetězec kliknutím na **...**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51337-140">Set the value for connection string the by clicking on the **...** button.</span></span> <span data-ttu-id="51337-141">Tím se otevře dialogové okno, což vám umožní vybrat účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="51337-141">This will open up a dialog allowing you to select a storage account.</span></span>
   
    ![Nastavení sady Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a><span data-ttu-id="51337-143">Chcete-li změnit úroveň monitorování verbose nebo minimální</span><span class="sxs-lookup"><span data-stu-id="51337-143">To change the monitoring level to verbose or minimal</span></span>
1. <span data-ttu-id="51337-144">V [portál Azure classic](https://manage.windowsazure.com/), otevřete **konfigurace** stránky pro nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="51337-144">In the [Azure classic portal](https://manage.windowsazure.com/), open the **Configure** page for the cloud service deployment.</span></span>
2. <span data-ttu-id="51337-145">V **úroveň**, klikněte na tlačítko **podrobné** nebo **minimální**.</span><span class="sxs-lookup"><span data-stu-id="51337-145">In **Level**, click **Verbose** or **Minimal**.</span></span> 
3. <span data-ttu-id="51337-146">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="51337-146">Click **Save**.</span></span>

<span data-ttu-id="51337-147">Po zapnutí podrobného monitorování, měli byste začít zobrazuje data sledování na portálu Azure classic v rámci hodiny.</span><span class="sxs-lookup"><span data-stu-id="51337-147">After you turn on verbose monitoring, you should start seeing the monitoring data in the Azure classic portal within the hour.</span></span>

<span data-ttu-id="51337-148">Nezpracovaná data čítačů a agregovaná data monitorování jsou uložené v účtu úložiště v tabulkách kvalifikovaný identifikátor ID nasazení pro role.</span><span class="sxs-lookup"><span data-stu-id="51337-148">The raw performance counter data and aggregated monitoring data are stored in the storage account in tables qualified by the deployment ID for the roles.</span></span> 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a><span data-ttu-id="51337-149">Postupy: přijímat výstrahy metrik, které cloudové služby</span><span class="sxs-lookup"><span data-stu-id="51337-149">How to: Receive alerts for cloud service metrics</span></span>
<span data-ttu-id="51337-150">Může přijímat výstrahy založené na cloudové služby monitorování metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-150">You can receive alerts based on your cloud service monitoring metrics.</span></span> <span data-ttu-id="51337-151">Na **Management Services** stránky Azure classic portálu, můžete vytvořit pravidlo, které spustí výstrahu, když zvolíte metriku dosáhne hodnotu, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="51337-151">On the **Management Services** page of the Azure classic portal, you can create a rule to trigger an alert when the metric you choose reaches a value that you specify.</span></span> <span data-ttu-id="51337-152">Můžete také mít e-mailu odeslaného při aktivaci výstrahy.</span><span class="sxs-lookup"><span data-stu-id="51337-152">You can also choose to have email sent when the alert is triggered.</span></span> <span data-ttu-id="51337-153">Další informace najdete v tématu [postupy: příjem oznámení výstrah a spravovat pravidla výstrah v Azure](http://go.microsoft.com/fwlink/?LinkId=309356).</span><span class="sxs-lookup"><span data-stu-id="51337-153">For more information, see [How to: Receive Alert Notifications and Manage Alert Rules in Azure](http://go.microsoft.com/fwlink/?LinkId=309356).</span></span>

## <a name="how-to-add-metrics-to-the-metrics-table"></a><span data-ttu-id="51337-154">Postupy: Přidání metriky do tabulky se metriky</span><span class="sxs-lookup"><span data-stu-id="51337-154">How to: Add metrics to the metrics table</span></span>
1. <span data-ttu-id="51337-155">V [portál Azure classic](http://manage.windowsazure.com/), otevřete **monitorování** stránky pro cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="51337-155">In the [Azure classic portal](http://manage.windowsazure.com/), open the **Monitor** page for the cloud service.</span></span>
   
    <span data-ttu-id="51337-156">Ve výchozím nastavení zobrazí v tabulce metriky podmnožinu dostupné metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-156">By default, the metrics table displays a subset of the available metrics.</span></span> <span data-ttu-id="51337-157">Na obrázku výchozí podrobné metriky pro cloudovou službu, která je omezená na čítač výkonu paměť\počet MB k dispozici s daty agregován na úrovni role.</span><span class="sxs-lookup"><span data-stu-id="51337-157">The illustration shows the default verbose metrics for a cloud service, which is limited to the Memory\Available MBytes performance counter, with data aggregated at the role level.</span></span> <span data-ttu-id="51337-158">Použití **přidat metriky** vybrat další metriky agregační a úrovni role monitorovat na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="51337-158">Use **Add Metrics** to select additional aggregate and role-level metrics to monitor in the Azure classic portal.</span></span>
   
    ![Podrobné zobrazení](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. <span data-ttu-id="51337-160">Přidání metriky do tabulky metriky:</span><span class="sxs-lookup"><span data-stu-id="51337-160">To add metrics to the metrics table:</span></span>
   
   1. <span data-ttu-id="51337-161">Klikněte na tlačítko **přidat metriky** otevřete **zvolte metriky**, vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="51337-161">Click **Add Metrics** to open **Choose Metrics**, shown below.</span></span>
      
       <span data-ttu-id="51337-162">První dostupná metrika je rozbalit a zobrazit možnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="51337-162">The first available metric is expanded to show options that are available.</span></span> <span data-ttu-id="51337-163">Pro jednotlivé metriky možnost top zobrazuje agregovaná data monitorování u všech rolí.</span><span class="sxs-lookup"><span data-stu-id="51337-163">For each metric, the top option displays aggregated monitoring data for all roles.</span></span> <span data-ttu-id="51337-164">Kromě toho můžete zobrazit data pro jednotlivé role.</span><span class="sxs-lookup"><span data-stu-id="51337-164">In addition, you can choose individual roles to display data for.</span></span>
      
       ![Přidat metriky](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. <span data-ttu-id="51337-166">Chcete-li vybrat metriky pro zobrazení</span><span class="sxs-lookup"><span data-stu-id="51337-166">To select metrics to display</span></span>
      
      * <span data-ttu-id="51337-167">Klikněte na šipku dolů podle metriky rozšířit možnosti monitorování.</span><span class="sxs-lookup"><span data-stu-id="51337-167">Click the down arrow by the metric to expand the monitoring options.</span></span>
      * <span data-ttu-id="51337-168">Zaškrtněte políčko pro jednotlivé možnosti monitorování, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="51337-168">Select the check box for each monitoring option you want to display.</span></span>
        
        <span data-ttu-id="51337-169">Až 50 metriky můžete zobrazit v tabulce metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-169">You can display up to 50 metrics in the metrics table.</span></span>
        
        > [!TIP]
        > <span data-ttu-id="51337-170">Podrobné monitorování, může obsahovat seznam metriky desítek metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-170">In verbose monitoring, the metrics list can contain dozens of metrics.</span></span> <span data-ttu-id="51337-171">Zobrazíte posuvníku přejděte myší pravé straně dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="51337-171">To display a scrollbar, hover over the right side of the dialog box.</span></span> <span data-ttu-id="51337-172">Chcete-li filtrovat seznam, klikněte na ikonu hledání a zadejte text do vyhledávacího pole, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="51337-172">To filter the list, click the search icon, and enter text in the search box, as shown below.</span></span>
        > 
        > 
        
        ![Přidat metriky vyhledávání](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. <span data-ttu-id="51337-174">Po dokončení výběru metriky, klikněte na tlačítko OK (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="51337-174">After you finish selecting metrics, click OK (checkmark).</span></span>
   
    <span data-ttu-id="51337-175">Vybrané metriky se přidají do tabulky metriky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="51337-175">The selected metrics are added to the metrics table, as shown below.</span></span>
   
    ![monitorování metriky](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. <span data-ttu-id="51337-177">Pokud chcete odstranit metriky z tabulky metriky, klikněte na tlačítko metrika vyberte ho a potom klikněte na **odstranit metrika**.</span><span class="sxs-lookup"><span data-stu-id="51337-177">To delete a metric from the metrics table, click the metric to select it, and then click **Delete Metric**.</span></span> <span data-ttu-id="51337-178">(Jenom zobrazí **odstranit metrika** Pokud máte vybrané metriky.)</span><span class="sxs-lookup"><span data-stu-id="51337-178">(You only see **Delete Metric** when you have a metric selected.)</span></span>

### <a name="to-add-custom-metrics-to-the-metrics-table"></a><span data-ttu-id="51337-179">Chcete-li přidat vlastní metriky do tabulky metriky</span><span class="sxs-lookup"><span data-stu-id="51337-179">To add custom metrics to the metrics table</span></span>
<span data-ttu-id="51337-180">**Podrobné** monitorování úroveň obsahuje seznam výchozích metrik, které můžete monitorovat na portálu.</span><span class="sxs-lookup"><span data-stu-id="51337-180">The **Verbose** monitoring level provides a list of default metrics that you can monitor on the portal.</span></span> <span data-ttu-id="51337-181">Kromě těchto můžete sledovat všechny vlastní metriky nebo čítače výkonu, které jsou definované aplikace prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="51337-181">In addition to these you can monitor any custom metrics or performance counters defined by your application through the portal.</span></span>

<span data-ttu-id="51337-182">Následující postup předpokládá, že jste zapnuli **podrobné** monitorování úroveň a nakonfigurovali vaše aplikace při úklidu a přenos vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="51337-182">The following steps assume that you have turned on **Verbose** monitoring level and have configured your application to collect and transfer custom performance counters.</span></span> 

<span data-ttu-id="51337-183">Chcete-li zobrazit čítače výkonu vlastní na portálu budete muset aktualizovat konfiguraci v kontejneru ovládacího prvku wad:</span><span class="sxs-lookup"><span data-stu-id="51337-183">To display the custom performance counters in the portal you need to update the configuration in wad-control-container:</span></span>

1. <span data-ttu-id="51337-184">Otevřete wad. řízení kontejneru objektů blob v účtu úložiště diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="51337-184">Open the wad-control-container blob in your diagnostics storage account.</span></span> <span data-ttu-id="51337-185">K tomu můžete použít Visual Studio nebo jiné Průzkumníka úložiště.</span><span class="sxs-lookup"><span data-stu-id="51337-185">You can use Visual Studio or any other storage explorer to do this.</span></span>
   
    ![Průzkumníka serveru Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. <span data-ttu-id="51337-187">Přejděte cesta blobu pomocí vzoru **DeploymentId/RoleName/RoleInstance** najít konfiguraci pro instanci role.</span><span class="sxs-lookup"><span data-stu-id="51337-187">Navigate the blob path using the pattern **DeploymentId/RoleName/RoleInstance** to find the configuration for your role instance.</span></span> 
   
    ![Storage Explorer sady Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. <span data-ttu-id="51337-189">Stáhněte si konfigurační soubor pro instanci role a aktualizovat tak, aby obsahovala všechny vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="51337-189">Download the configuration file for your role instance and update it to include any custom performance counters.</span></span> <span data-ttu-id="51337-190">Například k monitorování *zápis disku bajtů/s* pro *jednotka C* přidejte následující pod **PerformanceCounters\Subscriptions** uzlu</span><span class="sxs-lookup"><span data-stu-id="51337-190">For example to monitor *Disk Write Bytes/sec* for the *C drive* add the following under **PerformanceCounters\Subscriptions** node</span></span>
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. <span data-ttu-id="51337-191">Změny uložte a nahrajte konfiguračního souboru zpět do stejného umístění přepsal existující soubor do objektu BLOB.</span><span class="sxs-lookup"><span data-stu-id="51337-191">Save the changes and upload the configuration file back to the same location overwriting the existing file in the blob.</span></span>
5. <span data-ttu-id="51337-192">Přepněte do režimu podrobné v konfiguraci portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="51337-192">Toggle to Verbose mode in the Azure classic portal configuration.</span></span> <span data-ttu-id="51337-193">Pokud jste již v režimu s komentářem bude mít k přepnutí na minimální a zpět na podrobné.</span><span class="sxs-lookup"><span data-stu-id="51337-193">If you were in Verbose mode already you will have to toggle to minimal and back to verbose.</span></span>
6. <span data-ttu-id="51337-194">Čítač výkonu vlastní bude nyní k dispozici v **přidat metriky** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51337-194">The custom performance counter will now be available in the **Add Metrics** dialog box.</span></span> 

## <a name="how-to-customize-the-metrics-chart"></a><span data-ttu-id="51337-195">Postupy: přizpůsobení metriky grafu</span><span class="sxs-lookup"><span data-stu-id="51337-195">How to: Customize the metrics chart</span></span>
1. <span data-ttu-id="51337-196">V tabulce metriky vyberte až 6 metriky k vykreslení v grafu metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-196">In the metrics table, select up to 6 metrics to plot on the metrics chart.</span></span> <span data-ttu-id="51337-197">Pokud chcete vybrat metriky, klikněte na zaškrtávací políčko na jeho levé straně.</span><span class="sxs-lookup"><span data-stu-id="51337-197">To select a metric, click the check box on its left side.</span></span> <span data-ttu-id="51337-198">K odebrání metriky grafu metriky, zrušte jeho zaškrtnutí políčka v tabulce metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-198">To remove a metric from the metrics chart, clear its check box in the metrics table.</span></span>
   
    <span data-ttu-id="51337-199">Při výběru metriky v tabulce metriky, podle metrik, které jsou přidány do grafu metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-199">As you select metrics in the metrics table, the metrics are added to the metrics chart.</span></span> <span data-ttu-id="51337-200">Na úzké zobrazení **n Další** rozevírací seznam obsahuje metriky hlavičky, které nebudou vyhovovat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51337-200">On a narrow display, an **n more** drop-down list contains metric headers that won't fit the display.</span></span>
2. <span data-ttu-id="51337-201">Chcete-li přepnout mezi zobrazením relativní hodnoty (konečná hodnota jenom pro jednotlivé metriky) a absolutní hodnoty (osy Y zobrazit), vyberte relativní nebo absolutní v horní části grafu.</span><span class="sxs-lookup"><span data-stu-id="51337-201">To switch between displaying relative values (final value only for each metric) and absolute values (Y axis displayed), select Relative or Absolute at the top of the chart.</span></span>
   
    ![Relativní nebo absolutní](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. <span data-ttu-id="51337-203">Chcete-li změnit časový rozsah grafu zobrazí metriky, vyberte 1 hod., 24 hodin nebo 7 dní v horní části grafu.</span><span class="sxs-lookup"><span data-stu-id="51337-203">To change the time range the metrics chart displays, select 1 hour, 24 hours, or 7 days at the top of the chart.</span></span>
   
    ![Období zobrazení monitorování](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    <span data-ttu-id="51337-205">V grafu metriky řídicí panel metoda pro vykreslení metriky se liší.</span><span class="sxs-lookup"><span data-stu-id="51337-205">On the dashboard metrics chart, the method for plotting metrics is different.</span></span> <span data-ttu-id="51337-206">Je k dispozici standardní sadu metriky a metriky přidávání nebo odebírání výběrem hlavičku metriky.</span><span class="sxs-lookup"><span data-stu-id="51337-206">A standard set of metrics is available, and metrics are added or removed by selecting the metric header.</span></span>

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a><span data-ttu-id="51337-207">Chcete-li přizpůsobit graf metriky na řídicím panelu</span><span class="sxs-lookup"><span data-stu-id="51337-207">To customize the metrics chart on the dashboard</span></span>
1. <span data-ttu-id="51337-208">Otevřete řídicí panel pro cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="51337-208">Open the dashboard for the cloud service.</span></span>
2. <span data-ttu-id="51337-209">Přidat nebo odebrat metriky z grafu:</span><span class="sxs-lookup"><span data-stu-id="51337-209">Add or remove metrics from the chart:</span></span>
   
   * <span data-ttu-id="51337-210">K vykreslení nové metriky, zaškrtněte políčko pro metriku v hlavičkách grafu.</span><span class="sxs-lookup"><span data-stu-id="51337-210">To plot a new metric, select the check box for the metric in the chart headers.</span></span> <span data-ttu-id="51337-211">Na úzké zobrazení, klikněte na šipku dolů ve  ***n* ?? metriky** k vykreslení metriky záhlaví oblast grafu nelze zobrazit.</span><span class="sxs-lookup"><span data-stu-id="51337-211">On a narrow display, click the down arrow by ***n*??metrics** to plot a metric the chart header area can't display.</span></span>
   * <span data-ttu-id="51337-212">Chcete-li odstranit metrika, který se vykreslí v grafu, zrušte zaškrtnutí políčka podle jeho záhlaví.</span><span class="sxs-lookup"><span data-stu-id="51337-212">To delete a metric that is plotted on the chart, clear the check box by its header.</span></span>
   
3. <span data-ttu-id="51337-213">Přepínání mezi **relativní** a **absolutní** zobrazí.</span><span class="sxs-lookup"><span data-stu-id="51337-213">Switch between **Relative** and **Absolute** displays.</span></span>
4. <span data-ttu-id="51337-214">Vyberte 1 hod., 24 hodin nebo 7 dní od data k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51337-214">Choose 1 hour, 24 hours, or 7 days of data to display.</span></span>

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a><span data-ttu-id="51337-215">Postupy: přístup podrobné monitorování data mimo portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="51337-215">How to: Access verbose monitoring data outside the Azure classic portal</span></span>
<span data-ttu-id="51337-216">Podrobné sledování dat je ukládat do tabulek v účtech úložiště, které zadáte pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="51337-216">Verbose monitoring data is stored in tables in the storage accounts that you specify for each role.</span></span> <span data-ttu-id="51337-217">Pro každé nasazení cloudové služby vytvoří se pro roli šesti tabulky.</span><span class="sxs-lookup"><span data-stu-id="51337-217">For each cloud service deployment, six tables are created for the role.</span></span> <span data-ttu-id="51337-218">Dvě tabulky se vytváří pro každý (5 minut, 1 hodina a 12 hodin).</span><span class="sxs-lookup"><span data-stu-id="51337-218">Two tables are created for each (5 minutes, 1 hour, and 12 hours).</span></span> <span data-ttu-id="51337-219">Některé z těchto tabulek ukládá agregace na úrovni role; v další tabulce ukládá agregací pro instance rolí.</span><span class="sxs-lookup"><span data-stu-id="51337-219">One of these tables stores role-level aggregations; the other table stores aggregations for role instances.</span></span> 

<span data-ttu-id="51337-220">Názvy tabulek mít následující formát:</span><span class="sxs-lookup"><span data-stu-id="51337-220">The table names have the following format:</span></span>

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

<span data-ttu-id="51337-221">Kde:</span><span class="sxs-lookup"><span data-stu-id="51337-221">where:</span></span>

* <span data-ttu-id="51337-222">*deploymentID* je identifikátor GUID přiřazený nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="51337-222">*deploymentID* is the GUID assigned to the cloud service deployment</span></span>
* <span data-ttu-id="51337-223">*aggregation_interval* = 5 M, 1 H nebo 12 H</span><span class="sxs-lookup"><span data-stu-id="51337-223">*aggregation_interval* = 5M, 1H, or 12H</span></span>
* <span data-ttu-id="51337-224">agregace na úrovni role = R</span><span class="sxs-lookup"><span data-stu-id="51337-224">role-level aggregations = R</span></span>
* <span data-ttu-id="51337-225">agregace pro instance rolí = RI</span><span class="sxs-lookup"><span data-stu-id="51337-225">aggregations for role instances = RI</span></span>

<span data-ttu-id="51337-226">Následující tabulky by například uložit podrobné data monitorování agregován v intervalu 1 hodin:</span><span class="sxs-lookup"><span data-stu-id="51337-226">For example, the following tables would store verbose monitoring data aggregated at 1-hour intervals:</span></span>

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
