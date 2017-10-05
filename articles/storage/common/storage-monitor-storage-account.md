---
title: "Postup sledování účtu Azure Storage | Microsoft Docs"
description: "Naučte se monitorovat účet úložiště v Azure pomocí portálu Azure."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: marsma
ms.openlocfilehash: e8fbc4ecdffe62806019f494e1412cfedbccf71f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a><span data-ttu-id="f14a8-103">Monitorování účtu úložiště na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f14a8-103">Monitor a storage account in the Azure portal</span></span>

<span data-ttu-id="f14a8-104">[Azure Storage Analytics](../storage-analytics.md) poskytuje metriky pro všechny služby úložiště a protokoly pro objekty BLOB, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="f14a8-104">[Azure Storage Analytics](../storage-analytics.md) provides metrics for all storage services, and logs for blobs, queues, and tables.</span></span> <span data-ttu-id="f14a8-105">Můžete použít [portál Azure](https://portal.azure.com) ke konfiguraci, protokoly a metriky, které se zaznamenávají pro váš účet a konfigurace grafů, které poskytují vizuální reprezentace vašich dat metriky.</span><span class="sxs-lookup"><span data-stu-id="f14a8-105">You can use the [Azure portal](https://portal.azure.com) to configure which metrics and logs are recorded for your account, and configure charts that provide visual representations of your metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="f14a8-106">Existují náklady spojené s zkoumání dat monitorování na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f14a8-106">There are costs associated with examining monitoring data in the Azure portal.</span></span> <span data-ttu-id="f14a8-107">Další informace najdete v tématu [Storage Analytics a fakturace](/rest/api/storageservices/Storage-Analytics-and-Billing).</span><span class="sxs-lookup"><span data-stu-id="f14a8-107">For more information, see [Storage Analytics and Billing](/rest/api/storageservices/Storage-Analytics-and-Billing).</span></span>
>
> <span data-ttu-id="f14a8-108">Úložiště Azure File aktuálně podporuje metriky analytika úložiště, ale zatím nepodporuje protokolování.</span><span class="sxs-lookup"><span data-stu-id="f14a8-108">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>
> <span data-ttu-id="f14a8-109">Účty úložiště s typu replikaci z Zónově redundantní úložiště (ZRS) aktuálně nemají metriky nebo možnosti protokolování povoleny.</span><span class="sxs-lookup"><span data-stu-id="f14a8-109">Storage accounts with a replication type of Zone-Redundant Storage (ZRS) currently do not have the metrics or logging capability enabled.</span></span>
> 
> <span data-ttu-id="f14a8-110">Podrobné informace týkající se použití analytika úložiště a dalších nástrojů pro identifikovat, diagnostikovat a řešit problémy související s Azure Storage najdete v tématu [monitorování, Diagnostika a řešení Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f14a8-110">For an in-depth guide on using Storage Analytics and other tools to identify, diagnose, and troubleshoot Azure Storage-related issues, see [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span></span>
>

## <a name="configure-monitoring-for-a-storage-account"></a><span data-ttu-id="f14a8-111">Nakonfigurujte monitorování účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="f14a8-111">Configure monitoring for a storage account</span></span>

1. <span data-ttu-id="f14a8-112">V [portál Azure](https://portal.azure.com), vyberte **účty úložiště**, pak název účtu úložiště otevřete řídící panel účtu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-112">In the [Azure portal](https://portal.azure.com), select **Storage accounts**, then the storage account name to open the account dashboard.</span></span>
1. <span data-ttu-id="f14a8-113">Vyberte **diagnostiky** v **monitorování** části nabídky okna.</span><span class="sxs-lookup"><span data-stu-id="f14a8-113">Select **Diagnostics** in the **MONITORING** section of the menu blade.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. <span data-ttu-id="f14a8-115">Vyberte **typ** metriky dat pro každou **služby** chcete monitorovat a **zásady uchovávání informací** pro data.</span><span class="sxs-lookup"><span data-stu-id="f14a8-115">Select the **type** of metrics data for each **service** you wish to monitor, and the **retention policy** for the data.</span></span> <span data-ttu-id="f14a8-116">Můžete také zakázat monitorování nastavením **stav** k **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-116">You can also disable monitoring by setting **Status** to **Off**.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   <span data-ttu-id="f14a8-118">Existují dva typy metrik, které můžete povolit u každé služby, které jsou povolené ve výchozím nastavení pro nové účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="f14a8-118">There are two types of metrics you can enable for each service, both of which are enabled by default for new storage accounts:</span></span>

   * <span data-ttu-id="f14a8-119">**Agregační**: shromažďuje metriky například procenta vstupní/výstupní, dostupnosti, latence a úspěch.</span><span class="sxs-lookup"><span data-stu-id="f14a8-119">**Aggregate**: Collects metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="f14a8-120">Tyto metriky se shromažďují pro objekt blob, fronty, tabulky a souborové služby.</span><span class="sxs-lookup"><span data-stu-id="f14a8-120">These metrics are aggregated for the blob, queue, table, and file services.</span></span>
   * <span data-ttu-id="f14a8-121">**Na rozhraní API**: kromě agregovaná metrika shromažďuje stejnou sadu metriky pro každé operace úložiště v rozhraní API služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f14a8-121">**Per API**: In addition to the aggregate metrics, collects the same set of metrics for each storage operation in the Azure Storage service API.</span></span>

   <span data-ttu-id="f14a8-122">Chcete-li nastavit zásady uchovávání dat, přesunout **uchovávání dat (dny)** posuvníku nebo zadejte počet dnů od 1 do 365 uchovávání dat.</span><span class="sxs-lookup"><span data-stu-id="f14a8-122">To set the data retention policy, move the **Retention (days)** slider or enter the number of days of data to retain, from 1 to 365.</span></span> <span data-ttu-id="f14a8-123">Výchozí pro nové účty úložiště je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="f14a8-123">The default for new storage accounts is seven days.</span></span> <span data-ttu-id="f14a8-124">Pokud nechcete nastavit zásady uchovávání informací, zadejte nula.</span><span class="sxs-lookup"><span data-stu-id="f14a8-124">If you do not want to set a retention policy, enter zero.</span></span> <span data-ttu-id="f14a8-125">Pokud nejsou žádné zásady uchovávání informací, je na vás odstranit data sledování.</span><span class="sxs-lookup"><span data-stu-id="f14a8-125">If there is no retention policy, it is up to you to delete the monitoring data.</span></span>

   > [!WARNING]
   > <span data-ttu-id="f14a8-126">Budou se vám účtovat, když ručně odstranit data metriky.</span><span class="sxs-lookup"><span data-stu-id="f14a8-126">You are charged when you manually delete metrics data.</span></span> <span data-ttu-id="f14a8-127">V systému bez nákladů je odstranit zastaralé analytická data (data starší než vaše zásady uchovávání informací).</span><span class="sxs-lookup"><span data-stu-id="f14a8-127">Stale analytics data (data older than your retention policy) is deleted by the system at no cost.</span></span> <span data-ttu-id="f14a8-128">Doporučujeme vám, nastavení zásady uchovávání informací založené na tom, jak dlouho chcete zachovat úložiště analytická data pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="f14a8-128">We recommend setting a retention policy based on how long you want to retain storage analytics data for your account.</span></span> <span data-ttu-id="f14a8-129">V tématu [co poplatky, proveďte nimž dochází při povolení úložiště metriky?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f14a8-129">See [What charges do you incur when you enable storage metrics?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) for more information.</span></span>
   >

1. <span data-ttu-id="f14a8-130">Po dokončení konfigurace sledování, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-130">When you finish the monitoring configuration, select **Save**.</span></span>

<span data-ttu-id="f14a8-131">Výchozí sadu metriky se zobrazí v grafech v okně účtu úložiště, jakož i jednotlivé služby oken (objekt blob, fronty, tabulky a soubor).</span><span class="sxs-lookup"><span data-stu-id="f14a8-131">A default set of metrics is displayed in charts on the storage account blade, as well as the individual service blades (blob, queue, table, and file).</span></span> <span data-ttu-id="f14a8-132">Po povolení metrik pro službu, může trvat až jednu hodinu pro data zobrazí v jeho grafy.</span><span class="sxs-lookup"><span data-stu-id="f14a8-132">Once you've enabled metrics for a service, it may take up to an hour for data to appear in its charts.</span></span> <span data-ttu-id="f14a8-133">Můžete vybrat **upravit** na jakékoli metriky grafu pro [konfigurace metriky, které](#how-to-customize-metrics-charts) se zobrazí v grafu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-133">You can select **Edit** on any metric chart to [configure which metrics](#how-to-customize-metrics-charts) are displayed in the chart.</span></span>

<span data-ttu-id="f14a8-134">Shromažďování metrik a protokolování můžete zakázat nastavením **stav** k **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-134">You can disable metrics collection and logging by setting **Status** to **Off**.</span></span>

> [!NOTE]
> <span data-ttu-id="f14a8-135">Azure Storage používá [tabulky úložiště](../common/storage-introduction.md#table-storage) k uložení metriky pro váš účet úložiště a ukládá metriky v tabulkách ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-135">Azure Storage uses [table storage](../common/storage-introduction.md#table-storage) to store the metrics for your storage account, and stores the metrics in tables in your account.</span></span> <span data-ttu-id="f14a8-136">Další informace najdete v tématu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-136">For more information, see.</span></span> <span data-ttu-id="f14a8-137">[Ukládání metriky](../common/storage-analytics.md#how-metrics-are-stored).</span><span class="sxs-lookup"><span data-stu-id="f14a8-137">[How metrics are stored](../common/storage-analytics.md#how-metrics-are-stored).</span></span>
>

## <a name="customize-metrics-charts"></a><span data-ttu-id="f14a8-138">Přizpůsobení metriky grafy</span><span class="sxs-lookup"><span data-stu-id="f14a8-138">Customize metrics charts</span></span>

<span data-ttu-id="f14a8-139">Pomocí následujících kroků vyberte které úložiště metriky, chcete-li zobrazit v grafu metriky.</span><span class="sxs-lookup"><span data-stu-id="f14a8-139">Use the following procedure to choose which storage metrics to view in a metrics chart.</span></span> 

1. <span data-ttu-id="f14a8-140">Spusťte zobrazením grafu metriky úložiště na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f14a8-140">Start by displaying a storage metric chart in the Azure portal.</span></span> <span data-ttu-id="f14a8-141">Grafy můžete najít na **okně účtu úložiště** a v **metriky** okno pro jednotlivé služby (objekt blob, fronty, tabulce, soubor).</span><span class="sxs-lookup"><span data-stu-id="f14a8-141">You can find charts on the **storage account blade** and in the **Metrics** blade for an individual service (blob, queue, table, file).</span></span>

   <span data-ttu-id="f14a8-142">V tomto příkladu budeme pracovat s následující graf, který se zobrazí na **okně účtu úložiště**:</span><span class="sxs-lookup"><span data-stu-id="f14a8-142">In this example, we work with the following chart that appears on the **storage account blade**:</span></span>

   ![Výběr grafu na portálu Azure](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. <span data-ttu-id="f14a8-144">Klikněte na tlačítko kdekoli v rámci graf a otevřete **metrika** okno.</span><span class="sxs-lookup"><span data-stu-id="f14a8-144">Next, click anywhere within the chart to open the **Metric** blade.</span></span> <span data-ttu-id="f14a8-145">Vyberte **upravit graf** otevřete **upravit graf** okno.</span><span class="sxs-lookup"><span data-stu-id="f14a8-145">Select **Edit chart** to open the **Edit Chart** blade.</span></span>

   ![Upravit graf tlačítka v okně grafu](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. <span data-ttu-id="f14a8-147">Na **upravit graf** okně, vyberte **časový rozsah** metrik, které chcete zobrazit v grafu a **služby** (objektů blob, fronty, tabulky, soubor) jejichž metriky, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="f14a8-147">On the **Edit Chart** blade, select the **Time Range** of the metrics to display in the chart, and the **service** (blob, queue, table, file) whose metrics you wish to display.</span></span> <span data-ttu-id="f14a8-148">Zde jsme vybrali zobrazíte minulého týdne metriky pro služby objektů blob:</span><span class="sxs-lookup"><span data-stu-id="f14a8-148">Here we've selected to display the past week's metrics for the blob service:</span></span>

   ![Výběr časového rozsahu a služby v okně upravit graf](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. <span data-ttu-id="f14a8-150">Vyberte jednotlivých **metriky** jako měl zobrazit v grafu, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-150">Select the individual **metrics** you'd like displayed in the chart, then click **OK**.</span></span> <span data-ttu-id="f14a8-151">Například Zde jsme rozhodli jste se zobrazit *ContainerCount* a *ObjectCount* metriky:</span><span class="sxs-lookup"><span data-stu-id="f14a8-151">For example, here we've chosen to display the *ContainerCount* and *ObjectCount* metrics:</span></span>

   ![Jednotlivé metriky výběr v okně upravit graf](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

<span data-ttu-id="f14a8-153">Graf nastavení neovlivní kolekce, úložiště dat v účtu úložiště, zobrazování dat metrik monitorování nebo agregace.</span><span class="sxs-lookup"><span data-stu-id="f14a8-153">Your chart settings do not affect the collection, aggregation, or storage of monitoring data in the storage account, only the viewing of metrics data.</span></span>

### <a name="metrics-availability-in-charts"></a><span data-ttu-id="f14a8-154">Metriky dostupnosti v diagramech</span><span class="sxs-lookup"><span data-stu-id="f14a8-154">Metrics availability in charts</span></span>

<span data-ttu-id="f14a8-155">Seznam dostupné metriky změny podle služby, která jste vybrali v rozevíracím seznamu a typ jednotky grafu, který upravujete.</span><span class="sxs-lookup"><span data-stu-id="f14a8-155">The list of available metrics changes based on which service you've chosen in the drop-down, and the unit type of the chart you're editing.</span></span> <span data-ttu-id="f14a8-156">Například můžete vybrat metriky procento jako *PercentNetworkError* a *PercentThrottlingError* pouze v případě, že upravujete graf, který zobrazuje jednotky v procentech:</span><span class="sxs-lookup"><span data-stu-id="f14a8-156">For example, you can select percentage metrics like *PercentNetworkError* and *PercentThrottlingError* only if you're editing a chart that displays units in percentage:</span></span>

![Žádost o chybě procento grafu na portálu Azure](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a><span data-ttu-id="f14a8-158">Metriky řešení</span><span class="sxs-lookup"><span data-stu-id="f14a8-158">Metrics resolution</span></span>

<span data-ttu-id="f14a8-159">Metriky, které jste vybrali v Diagnostika Určuje rozlišení metriky, které jsou k dispozici pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="f14a8-159">The metrics you selected in Diagnostics determines the resolution of the metrics that are available for your account:</span></span>

* <span data-ttu-id="f14a8-160">**Agregační** monitorování poskytuje metriky, jako je například procenta vstupní/výstupní, dostupnosti, latence a úspěch.</span><span class="sxs-lookup"><span data-stu-id="f14a8-160">**Aggregate** monitoring provides metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="f14a8-161">Tyto metriky se shromažďují z objektu blob, table, souboru a fronty služby.</span><span class="sxs-lookup"><span data-stu-id="f14a8-161">These metrics are aggregated from the blob, table, file, and queue services.</span></span>
* <span data-ttu-id="f14a8-162">**Na rozhraní API** poskytuje lepší řešení s metriky, které jsou k dispozici pro operace jednotlivých úložiště navíc agregace úrovni služby.</span><span class="sxs-lookup"><span data-stu-id="f14a8-162">**Per API** provides finer resolution, with metrics available for individual storage operations, in addition to the service-level aggregates.</span></span>

## <a name="configure-metrics-alerts"></a><span data-ttu-id="f14a8-163">Konfigurace metrik výstrah</span><span class="sxs-lookup"><span data-stu-id="f14a8-163">Configure metrics alerts</span></span>

<span data-ttu-id="f14a8-164">Můžete vytvořit oznámení upozornění v případě, že bylo dosaženo prahové hodnoty pro metrika prostředků úložiště.</span><span class="sxs-lookup"><span data-stu-id="f14a8-164">You can create alerts to notify you when thresholds have been reached for storage resource metrics.</span></span>

1. <span data-ttu-id="f14a8-165">Otevřete **okno pravidla výstrah**, přejděte dolů k položce **monitorování** části **nabídky okno** a vyberte **výstrah pravidla**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-165">To open the **Alert rules blade**, scroll down to the **MONITORING** section of the **Menu blade** and select **Alert rules**.</span></span>
1. <span data-ttu-id="f14a8-166">Vyberte **přidat upozornění** otevřete **přidání pravidla výstrahy** okno</span><span class="sxs-lookup"><span data-stu-id="f14a8-166">Select **Add alert** to open the **Add an alert rule** blade</span></span>
1. <span data-ttu-id="f14a8-167">Vyberte **prostředků** (objektů blob, soubor, fronty, tabulka) z rozevíracího seznamu a zadejte **název** a **popis** pro nové pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="f14a8-167">Select a **Resource** (blob, file, queue, table) from the drop-down, and enter a **Name** and **Description** for your new alert rule.</span></span>
1. <span data-ttu-id="f14a8-168">Vyberte **metrika** pro který chcete přidat oznámení, výstrahu **podmínku**a **prahová hodnota**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-168">Select the **Metric** for which you'd like to add an alert, an alert **Condition**, and a **Threshold**.</span></span> <span data-ttu-id="f14a8-169">Prahová hodnota jednotky zadejte změny v závislosti na metriku, které jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="f14a8-169">The threshold unit type changes depending on the metric you've chosen.</span></span> <span data-ttu-id="f14a8-170">Například "count" je typ jednotky pro *ContainerCount*, při jednotku pro *PercentNetworkError* metrika je procento.</span><span class="sxs-lookup"><span data-stu-id="f14a8-170">For example, "count" is the unit type for *ContainerCount*, while the unit for the *PercentNetworkError* metric is a percentage.</span></span>
1. <span data-ttu-id="f14a8-171">Vyberte **období**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-171">Select the **Period**.</span></span> <span data-ttu-id="f14a8-172">Metriky, které dosáhnout nebo prahovou hodnotu překročit v období spustí výstrahu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-172">Metrics that reach or exceed the Threshold within the period trigger an alert.</span></span>
1. <span data-ttu-id="f14a8-173">(Volitelné) Konfigurace **e-mailu** a **Webhooku** oznámení.</span><span class="sxs-lookup"><span data-stu-id="f14a8-173">(Optional) Configure **Email** and **Webhook** notifications.</span></span> <span data-ttu-id="f14a8-174">Další informace o webhooků, najdete v části [konfigurace webhook, jehož na výstrahu Azure metriky](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f14a8-174">For more information on webhooks, see [Configure a webhook on an Azure metric alert](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="f14a8-175">Pokud neprovedete konfiguraci e-mailu nebo webhooku oznámení, zobrazí se výstrahy pouze na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f14a8-175">If you do not configure email or webhook notifications, alerts will appear only in the Azure portal.</span></span>

!['Přidejte pravidlo výstrahy, okno na portálu Azure](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a><span data-ttu-id="f14a8-177">Metriky grafy přidáte na řídicím panelu portálu</span><span class="sxs-lookup"><span data-stu-id="f14a8-177">Add metrics charts to the portal dashboard</span></span>

<span data-ttu-id="f14a8-178">Grafy metrik Azure Storage pro všechny účty úložiště můžete přidat do řídicího panelu portálu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-178">You can add Azure Storage metrics charts for any of your storage accounts to your portal dashboard.</span></span>

1. <span data-ttu-id="f14a8-179">Kliknutím vyberte **úprava řídicího panelu** při zobrazení řídicího panelu v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f14a8-179">Select click **Edit dashboard** while viewing your dashboard in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="f14a8-180">V **dlaždice Galerie**, vyberte **najít dlaždice podle** > **typu**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-180">In the **Tile Gallery**, select **Find tiles by** > **Type**.</span></span>
1. <span data-ttu-id="f14a8-181">Vyberte **typ** > **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-181">Select **Type** > **Storage accounts**.</span></span>
1. <span data-ttu-id="f14a8-182">V **prostředky**, vyberte účet úložiště, jejichž metriky, které chcete přidat do řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-182">In **Resources**, select the storage account whose metrics you wish to add to the dashboard.</span></span>
1. <span data-ttu-id="f14a8-183">Vyberte **kategorie** > **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-183">Select **Categories** > **Monitoring**.</span></span>
1. <span data-ttu-id="f14a8-184">Přetažení myší graf dlaždice do řídicího panelu pro metriku, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="f14a8-184">Drag-and-drop the chart tile onto your dashboard for the metric you'd like displayed.</span></span> <span data-ttu-id="f14a8-185">Opakujte pro všechny metriky, které si přejete zobrazené na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-185">Repeat for all metrics you'd like displayed on the dashboard.</span></span> <span data-ttu-id="f14a8-186">Na následujícím obrázku grafu "Objekty BLOB - celkový počet požadavků" je označený jako příklad, ale všechny grafy jsou k dispozici pro umístění na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="f14a8-186">In the following image, the "Blobs - Total requests" chart is highlighted as an example, but all the charts are available for placement on your dashboard.</span></span>

   ![Galerie dlaždice na portálu Azure](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. <span data-ttu-id="f14a8-188">Vyberte **provádí přizpůsobení** v horní části řídicího panelu po dokončení přidávání grafy.</span><span class="sxs-lookup"><span data-stu-id="f14a8-188">Select **Done customizing** near the top of the dashboard when you're done adding charts.</span></span>

<span data-ttu-id="f14a8-189">Po přidání grafy na řídicí panel, můžete dále přizpůsobit je popsaný v [přizpůsobení metriky grafy](#how-to-customize-metrics-charts).</span><span class="sxs-lookup"><span data-stu-id="f14a8-189">Once you've added charts to your dashboard, you can further customize them as described in [Customize metrics charts](#how-to-customize-metrics-charts).</span></span>

## <a name="configure-logging"></a><span data-ttu-id="f14a8-190">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="f14a8-190">Configure logging</span></span>

<span data-ttu-id="f14a8-191">Můžete určit, aby k ukládání protokolů diagnostiky pro čtení, zápisu a odstranit požadavky pro objekt blob, table a fronty služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f14a8-191">You can instruct Azure Storage to save diagnostics logs for read, write, and delete requests for the blob, table, and queue services.</span></span> <span data-ttu-id="f14a8-192">Zásady uchovávání dat, které nastavíte, platí také pro tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="f14a8-192">The data retention policy you set also applies to these logs.</span></span>

> [!NOTE]
> <span data-ttu-id="f14a8-193">Úložiště Azure File aktuálně podporuje metriky analytika úložiště, ale zatím nepodporuje protokolování.</span><span class="sxs-lookup"><span data-stu-id="f14a8-193">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>

1. <span data-ttu-id="f14a8-194">V [portál Azure](https://portal.azure.com), vyberte **účty úložiště**, potom na název účtu úložiště, otevřete okno účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f14a8-194">In the [Azure portal](https://portal.azure.com), select **Storage accounts**, then the name of the storage account to open the storage account blade.</span></span>
1. <span data-ttu-id="f14a8-195">Vyberte **diagnostiky** v **monitorování** části nabídky okna.</span><span class="sxs-lookup"><span data-stu-id="f14a8-195">Select **Diagnostics** in the **MONITORING** section of the menu blade.</span></span>

    ![Diagnostika položku nabídky v části sledování na portálu Azure.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. <span data-ttu-id="f14a8-197">Ujistěte se, **stav** je nastaven na **na**a vyberte **služby** pro které chcete povolit protokolování.</span><span class="sxs-lookup"><span data-stu-id="f14a8-197">Ensure **Status** is set to **On**, and select the **services** for which you'd like to enable logging.</span></span>

    ![Konfigurovat protokolování na portálu Azure.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. <span data-ttu-id="f14a8-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f14a8-199">Click **Save**.</span></span>

<span data-ttu-id="f14a8-200">Diagnostické protokoly jsou ukládány v kontejneru objektů blob s názvem $logs ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f14a8-200">The diagnostics logs are saved in a blob container named $logs in your storage account.</span></span> <span data-ttu-id="f14a8-201">Můžete zobrazit pomocí Průzkumníka úložiště jako data protokolu [Microsoft Storage Explorer](http://storageexplorer.com), nebo programově pomocí klientské knihovny pro úložiště nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f14a8-201">You can view the log data using a storage explorer like the [Microsoft Storage Explorer](http://storageexplorer.com), or programmatically using the storage client library or PowerShell.</span></span>

<span data-ttu-id="f14a8-202">Informace o Přistupování do kontejneru $logs najdete v tématu [povolení protokolování úložiště a přístup k datům protokolu](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span><span class="sxs-lookup"><span data-stu-id="f14a8-202">For information about accessing the $logs container, see [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f14a8-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f14a8-203">Next steps</span></span>

* <span data-ttu-id="f14a8-204">Najít další podrobnosti o [metriky, protokolování a fakturace](../storage-analytics.md) pro analytika úložiště.</span><span class="sxs-lookup"><span data-stu-id="f14a8-204">Find more details about [metrics, logging, and billing](../storage-analytics.md) for Storage Analytics.</span></span>
* <span data-ttu-id="f14a8-205">[Povolit daty metrik Azure Storage metriky a zobrazení](../storage-enable-and-view-metrics.md) pomocí prostředí PowerShell a programově pomocí C#.</span><span class="sxs-lookup"><span data-stu-id="f14a8-205">[Enable Azure Storage metrics and view metrics data](../storage-enable-and-view-metrics.md) by using PowerShell and programmatically with C#.</span></span>
