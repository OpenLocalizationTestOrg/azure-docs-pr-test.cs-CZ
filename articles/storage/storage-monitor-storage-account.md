---
title: "aaaHow toomonitor účtu Azure Storage | Microsoft Docs"
description: "Zjistěte, jak hello toomonitor účet úložiště v Azure pomocí portálu Azure."
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
ms.openlocfilehash: 782873648fdbccc0191a074cd4044f97af8b7c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-storage-account-in-hello-azure-portal"></a><span data-ttu-id="ccd03-103">Monitorování účtu úložiště v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ccd03-103">Monitor a storage account in hello Azure portal</span></span>

<span data-ttu-id="ccd03-104">[Azure Storage Analytics](storage-analytics.md) poskytuje metriky pro všechny služby úložiště a protokoly pro objekty BLOB, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="ccd03-104">[Azure Storage Analytics](storage-analytics.md) provides metrics for all storage services, and logs for blobs, queues, and tables.</span></span> <span data-ttu-id="ccd03-105">Můžete použít hello [portál Azure](https://portal.azure.com) tooconfigure protokoly a metriky, které se zaznamenávají pro váš účet a konfigurace grafů, které poskytují vizuální reprezentace vašich dat metriky.</span><span class="sxs-lookup"><span data-stu-id="ccd03-105">You can use hello [Azure portal](https://portal.azure.com) tooconfigure which metrics and logs are recorded for your account, and configure charts that provide visual representations of your metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="ccd03-106">Existují náklady spojené s zkoumání dat monitorování v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ccd03-106">There are costs associated with examining monitoring data in hello Azure portal.</span></span> <span data-ttu-id="ccd03-107">Další informace najdete v tématu [Storage Analytics a fakturace](/rest/api/storageservices/Storage-Analytics-and-Billing).</span><span class="sxs-lookup"><span data-stu-id="ccd03-107">For more information, see [Storage Analytics and Billing](/rest/api/storageservices/Storage-Analytics-and-Billing).</span></span>
>
> <span data-ttu-id="ccd03-108">Úložiště Azure File aktuálně podporuje metriky analytika úložiště, ale zatím nepodporuje protokolování.</span><span class="sxs-lookup"><span data-stu-id="ccd03-108">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>
> <span data-ttu-id="ccd03-109">Účty úložiště s typu replikaci z Zónově redundantní úložiště (ZRS) aktuálně nemají hello metriky nebo možnosti protokolování povoleny.</span><span class="sxs-lookup"><span data-stu-id="ccd03-109">Storage accounts with a replication type of Zone-Redundant Storage (ZRS) currently do not have hello metrics or logging capability enabled.</span></span>
> 
> <span data-ttu-id="ccd03-110">Pro podrobné příručce pomocí analytika úložiště a dalších nástrojů tooidentify diagnostikovat a vyřešit problémy související s Azure Storage najdete v tématu [monitorování, Diagnostika a řešení Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="ccd03-110">For an in-depth guide on using Storage Analytics and other tools tooidentify, diagnose, and troubleshoot Azure Storage-related issues, see [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>
>

## <a name="configure-monitoring-for-a-storage-account"></a><span data-ttu-id="ccd03-111">Nakonfigurujte monitorování účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="ccd03-111">Configure monitoring for a storage account</span></span>

1. <span data-ttu-id="ccd03-112">V hello [portál Azure](https://portal.azure.com), vyberte **účty úložiště**, pak hello účet název tooopen hello účtu řídicího panelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ccd03-112">In hello [Azure portal](https://portal.azure.com), select **Storage accounts**, then hello storage account name tooopen hello account dashboard.</span></span>
1. <span data-ttu-id="ccd03-113">Vyberte **diagnostiky** v hello **monitorování** části hello nabídky okna.</span><span class="sxs-lookup"><span data-stu-id="ccd03-113">Select **Diagnostics** in hello **MONITORING** section of hello menu blade.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. <span data-ttu-id="ccd03-115">Vyberte hello **typ** metriky dat pro každou **služby** chcete toomonitor a hello **zásady uchovávání informací** pro hello data.</span><span class="sxs-lookup"><span data-stu-id="ccd03-115">Select hello **type** of metrics data for each **service** you wish toomonitor, and hello **retention policy** for hello data.</span></span> <span data-ttu-id="ccd03-116">Můžete také zakázat monitorování nastavením **stav** příliš**vypnout**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-116">You can also disable monitoring by setting **Status** too**Off**.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   <span data-ttu-id="ccd03-118">Existují dva typy metrik, které můžete povolit u každé služby, které jsou povolené ve výchozím nastavení pro nové účty úložiště:</span><span class="sxs-lookup"><span data-stu-id="ccd03-118">There are two types of metrics you can enable for each service, both of which are enabled by default for new storage accounts:</span></span>

   * <span data-ttu-id="ccd03-119">**Agregační**: shromažďuje metriky například procenta vstupní/výstupní, dostupnosti, latence a úspěch.</span><span class="sxs-lookup"><span data-stu-id="ccd03-119">**Aggregate**: Collects metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="ccd03-120">Tyto metriky se shromažďují pro hello blob, fronty, tabulky a souborové služby.</span><span class="sxs-lookup"><span data-stu-id="ccd03-120">These metrics are aggregated for hello blob, queue, table, and file services.</span></span>
   * <span data-ttu-id="ccd03-121">**Na rozhraní API**: V přidání toohello agregovaná metrika, shromažďuje hello stejnou sadu metriky pro každé operace úložiště v hello rozhraní API služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ccd03-121">**Per API**: In addition toohello aggregate metrics, collects hello same set of metrics for each storage operation in hello Azure Storage service API.</span></span>

   <span data-ttu-id="ccd03-122">tooset hello zásady uchovávání informací, přesunutí hello **uchovávání dat (dny)** posuvníku nebo zadejte hello počet dnů, za které tooretain dat, z 1 too365.</span><span class="sxs-lookup"><span data-stu-id="ccd03-122">tooset hello data retention policy, move hello **Retention (days)** slider or enter hello number of days of data tooretain, from 1 too365.</span></span> <span data-ttu-id="ccd03-123">Hello výchozí pro nové účty úložiště je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="ccd03-123">hello default for new storage accounts is seven days.</span></span> <span data-ttu-id="ccd03-124">Pokud nechcete, aby tooset zásady uchovávání informací, zadejte nula.</span><span class="sxs-lookup"><span data-stu-id="ccd03-124">If you do not want tooset a retention policy, enter zero.</span></span> <span data-ttu-id="ccd03-125">Pokud nejsou žádné zásady uchovávání informací, zapnutý hello toodelete tooyou dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="ccd03-125">If there is no retention policy, it is up tooyou toodelete hello monitoring data.</span></span>

   > [!WARNING]
   > <span data-ttu-id="ccd03-126">Budou se vám účtovat, když ručně odstranit data metriky.</span><span class="sxs-lookup"><span data-stu-id="ccd03-126">You are charged when you manually delete metrics data.</span></span> <span data-ttu-id="ccd03-127">Zastaralé analytická data (data starší než vaše zásady uchovávání informací) je odstraněn hello systému bez nákladů.</span><span class="sxs-lookup"><span data-stu-id="ccd03-127">Stale analytics data (data older than your retention policy) is deleted by hello system at no cost.</span></span> <span data-ttu-id="ccd03-128">Doporučujeme vám, nastavení zásady uchovávání informací založené na tom, jak dlouho chcete tooretain úložiště analytická data pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="ccd03-128">We recommend setting a retention policy based on how long you want tooretain storage analytics data for your account.</span></span> <span data-ttu-id="ccd03-129">V tématu [co poplatky, proveďte nimž dochází při povolení úložiště metriky?](storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ccd03-129">See [What charges do you incur when you enable storage metrics?](storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) for more information.</span></span>
   >

1. <span data-ttu-id="ccd03-130">Po dokončení konfigurace monitorování hello, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-130">When you finish hello monitoring configuration, select **Save**.</span></span>

<span data-ttu-id="ccd03-131">Výchozí sadu metriky se zobrazí v grafech v okně účtu úložiště hello, jakož i hello jednotlivé služby oken (objekt blob, fronty, tabulky a soubor).</span><span class="sxs-lookup"><span data-stu-id="ccd03-131">A default set of metrics is displayed in charts on hello storage account blade, as well as hello individual service blades (blob, queue, table, and file).</span></span> <span data-ttu-id="ccd03-132">Po povolení metrik pro službu, může trvat až hodinu tooan tooappear data v jeho grafy.</span><span class="sxs-lookup"><span data-stu-id="ccd03-132">Once you've enabled metrics for a service, it may take up tooan hour for data tooappear in its charts.</span></span> <span data-ttu-id="ccd03-133">Můžete vybrat **upravit** žádné metrika grafu příliš[konfigurace metriky, které](#how-to-customize-metrics-charts) se zobrazí v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="ccd03-133">You can select **Edit** on any metric chart too[configure which metrics](#how-to-customize-metrics-charts) are displayed in hello chart.</span></span>

<span data-ttu-id="ccd03-134">Shromažďování metrik a protokolování můžete zakázat nastavením **stav** příliš**vypnout**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-134">You can disable metrics collection and logging by setting **Status** too**Off**.</span></span>

> [!NOTE]
> <span data-ttu-id="ccd03-135">Azure Storage používá [tabulky úložiště](storage-introduction.md#table-storage) toostore hello metriky pro váš účet úložiště a metriky hello úložiště v tabulkách ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="ccd03-135">Azure Storage uses [table storage](storage-introduction.md#table-storage) toostore hello metrics for your storage account, and stores hello metrics in tables in your account.</span></span> <span data-ttu-id="ccd03-136">Další informace najdete v tématu.</span><span class="sxs-lookup"><span data-stu-id="ccd03-136">For more information, see.</span></span> <span data-ttu-id="ccd03-137">[Ukládání metriky](storage-analytics.md#how-metrics-are-stored).</span><span class="sxs-lookup"><span data-stu-id="ccd03-137">[How metrics are stored](storage-analytics.md#how-metrics-are-stored).</span></span>
>

## <a name="customize-metrics-charts"></a><span data-ttu-id="ccd03-138">Přizpůsobení metriky grafy</span><span class="sxs-lookup"><span data-stu-id="ccd03-138">Customize metrics charts</span></span>

<span data-ttu-id="ccd03-139">Použijte následující postup toochoose hello které tooview metriky úložiště v grafu metriky.</span><span class="sxs-lookup"><span data-stu-id="ccd03-139">Use hello following procedure toochoose which storage metrics tooview in a metrics chart.</span></span> 

1. <span data-ttu-id="ccd03-140">Spusťte zobrazením grafu metriky úložiště v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ccd03-140">Start by displaying a storage metric chart in hello Azure portal.</span></span> <span data-ttu-id="ccd03-141">Můžete najít grafy na hello **okně účtu úložiště** a v hello **metriky** okno pro jednotlivé služby (objekt blob, fronty, tabulce, soubor).</span><span class="sxs-lookup"><span data-stu-id="ccd03-141">You can find charts on hello **storage account blade** and in hello **Metrics** blade for an individual service (blob, queue, table, file).</span></span>

   <span data-ttu-id="ccd03-142">V tomto příkladu budeme pracovat s hello následující graf, který se zobrazí na hello **okně účtu úložiště**:</span><span class="sxs-lookup"><span data-stu-id="ccd03-142">In this example, we work with hello following chart that appears on hello **storage account blade**:</span></span>

   ![Výběr grafu na portálu Azure](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. <span data-ttu-id="ccd03-144">Klikněte na tlačítko kdekoli v rámci hello grafu tooopen hello **metrika** okno.</span><span class="sxs-lookup"><span data-stu-id="ccd03-144">Next, click anywhere within hello chart tooopen hello **Metric** blade.</span></span> <span data-ttu-id="ccd03-145">Vyberte **upravit graf** tooopen hello **upravit graf** okno.</span><span class="sxs-lookup"><span data-stu-id="ccd03-145">Select **Edit chart** tooopen hello **Edit Chart** blade.</span></span>

   ![Upravit graf tlačítka v okně grafu](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. <span data-ttu-id="ccd03-147">Na hello **upravit graf** okně, vyberte hello **časový rozsah** z toodisplay hello metriky v grafu hello a hello **služby** (objektů blob, fronty, tabulky, soubor) jejichž metriky, které chcete toodisplay.</span><span class="sxs-lookup"><span data-stu-id="ccd03-147">On hello **Edit Chart** blade, select hello **Time Range** of hello metrics toodisplay in hello chart, and hello **service** (blob, queue, table, file) whose metrics you wish toodisplay.</span></span> <span data-ttu-id="ccd03-148">Zde jsme vybrali toodisplay hello za týden metriky pro služby objektů blob hello:</span><span class="sxs-lookup"><span data-stu-id="ccd03-148">Here we've selected toodisplay hello past week's metrics for hello blob service:</span></span>

   ![Výběr časového rozsahu a služby v okně upravit graf hello](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. <span data-ttu-id="ccd03-150">Vyberte hello individuální **metriky** jako měl zobrazit v grafu hello, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-150">Select hello individual **metrics** you'd like displayed in hello chart, then click **OK**.</span></span> <span data-ttu-id="ccd03-151">Například Zde jsme zvolili jste toodisplay hello *ContainerCount* a *ObjectCount* metriky:</span><span class="sxs-lookup"><span data-stu-id="ccd03-151">For example, here we've chosen toodisplay hello *ContainerCount* and *ObjectCount* metrics:</span></span>

   ![Jednotlivé metriky výběr v okně upravit graf](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

<span data-ttu-id="ccd03-153">Nastavení grafu není ovlivnit hello kolekce, agregace nebo úložiště dat v účtu úložiště hello monitorování, pouze hello zobrazování dat metriky.</span><span class="sxs-lookup"><span data-stu-id="ccd03-153">Your chart settings do not affect hello collection, aggregation, or storage of monitoring data in hello storage account, only hello viewing of metrics data.</span></span>

### <a name="metrics-availability-in-charts"></a><span data-ttu-id="ccd03-154">Metriky dostupnosti v diagramech</span><span class="sxs-lookup"><span data-stu-id="ccd03-154">Metrics availability in charts</span></span>

<span data-ttu-id="ccd03-155">Hello seznam změn dostupné metriky založený na služby, která jste zvolili v hello rozevíracího seznamu a typ jednotky hello hello grafu, který upravujete.</span><span class="sxs-lookup"><span data-stu-id="ccd03-155">hello list of available metrics changes based on which service you've chosen in hello drop-down, and hello unit type of hello chart you're editing.</span></span> <span data-ttu-id="ccd03-156">Například můžete vybrat metriky procento jako *PercentNetworkError* a *PercentThrottlingError* pouze v případě, že upravujete graf, který zobrazuje jednotky v procentech:</span><span class="sxs-lookup"><span data-stu-id="ccd03-156">For example, you can select percentage metrics like *PercentNetworkError* and *PercentThrottlingError* only if you're editing a chart that displays units in percentage:</span></span>

![Žádost o chybě procento grafu v hello portálu Azure](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a><span data-ttu-id="ccd03-158">Metriky řešení</span><span class="sxs-lookup"><span data-stu-id="ccd03-158">Metrics resolution</span></span>

<span data-ttu-id="ccd03-159">Hello metriky, které jste vybrali v Diagnostika určuje hello rozlišení hello metriky, které jsou k dispozici pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="ccd03-159">hello metrics you selected in Diagnostics determines hello resolution of hello metrics that are available for your account:</span></span>

* <span data-ttu-id="ccd03-160">**Agregační** monitorování poskytuje metriky, jako je například procenta vstupní/výstupní, dostupnosti, latence a úspěch.</span><span class="sxs-lookup"><span data-stu-id="ccd03-160">**Aggregate** monitoring provides metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="ccd03-161">Tyto metriky se shromažďují z hello blob, table, souboru a služba fronty.</span><span class="sxs-lookup"><span data-stu-id="ccd03-161">These metrics are aggregated from hello blob, table, file, and queue services.</span></span>
* <span data-ttu-id="ccd03-162">**Na rozhraní API** poskytuje lepší řešení s metriky, které jsou k dispozici pro operace jednotlivých úložiště navíc toohello úrovně služby agregace.</span><span class="sxs-lookup"><span data-stu-id="ccd03-162">**Per API** provides finer resolution, with metrics available for individual storage operations, in addition toohello service-level aggregates.</span></span>

## <a name="configure-metrics-alerts"></a><span data-ttu-id="ccd03-163">Konfigurace metrik výstrah</span><span class="sxs-lookup"><span data-stu-id="ccd03-163">Configure metrics alerts</span></span>

<span data-ttu-id="ccd03-164">Můžete vytvořit výstrahy toonotify případě pro metrika prostředků úložiště bylo dosaženo prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ccd03-164">You can create alerts toonotify you when thresholds have been reached for storage resource metrics.</span></span>

1. <span data-ttu-id="ccd03-165">tooopen hello **pravidla výstrah okno**, projděte dolů toohello **monitorování** části hello **nabídky okno** a vyberte **výstrah pravidla**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-165">tooopen hello **Alert rules blade**, scroll down toohello **MONITORING** section of hello **Menu blade** and select **Alert rules**.</span></span>
1. <span data-ttu-id="ccd03-166">Vyberte **přidat upozornění** tooopen hello **přidání pravidla výstrahy** okno</span><span class="sxs-lookup"><span data-stu-id="ccd03-166">Select **Add alert** tooopen hello **Add an alert rule** blade</span></span>
1. <span data-ttu-id="ccd03-167">Vyberte **prostředků** (objektů blob, soubor, fronty, tabulka) z rozevíracího seznamu hello a zadejte **název** a **popis** pro nové pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ccd03-167">Select a **Resource** (blob, file, queue, table) from hello drop-down, and enter a **Name** and **Description** for your new alert rule.</span></span>
1. <span data-ttu-id="ccd03-168">Vyberte hello **metrika** pro kterou chcete tooadd výstrahu, výstrahu **podmínku**a **prahová hodnota**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-168">Select hello **Metric** for which you'd like tooadd an alert, an alert **Condition**, and a **Threshold**.</span></span> <span data-ttu-id="ccd03-169">Hello prahová hodnota jednotky typu změny v závislosti na hello metrika jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="ccd03-169">hello threshold unit type changes depending on hello metric you've chosen.</span></span> <span data-ttu-id="ccd03-170">Například "count" je hello typ jednotky pro *ContainerCount*, při hello jednotku pro hello *PercentNetworkError* metrika je procento.</span><span class="sxs-lookup"><span data-stu-id="ccd03-170">For example, "count" is hello unit type for *ContainerCount*, while hello unit for hello *PercentNetworkError* metric is a percentage.</span></span>
1. <span data-ttu-id="ccd03-171">Vyberte hello **období**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-171">Select hello **Period**.</span></span> <span data-ttu-id="ccd03-172">Metriky, které dosažení nebo překročení hello prahovou hodnotu v rámci hello období spustí výstrahu.</span><span class="sxs-lookup"><span data-stu-id="ccd03-172">Metrics that reach or exceed hello Threshold within hello period trigger an alert.</span></span>
1. <span data-ttu-id="ccd03-173">(Volitelné) Konfigurace **e-mailu** a **Webhooku** oznámení.</span><span class="sxs-lookup"><span data-stu-id="ccd03-173">(Optional) Configure **Email** and **Webhook** notifications.</span></span> <span data-ttu-id="ccd03-174">Další informace o webhooků, najdete v části [konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="ccd03-174">For more information on webhooks, see [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="ccd03-175">Pokud nenakonfigurujete oznámení e-mailu nebo webhooku, výstrahy se objeví jenom v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ccd03-175">If you do not configure email or webhook notifications, alerts will appear only in hello Azure portal.</span></span>

![Okno: Přidání pravidla výstrahy"v hello portálu Azure](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-toohello-portal-dashboard"></a><span data-ttu-id="ccd03-177">Přidat řídicí panel metriky grafy toohello portálu</span><span class="sxs-lookup"><span data-stu-id="ccd03-177">Add metrics charts toohello portal dashboard</span></span>

<span data-ttu-id="ccd03-178">Můžete přidat grafy metrik Azure Storage pro žádné úložiště účtů tooyour řídicího panelu portálu.</span><span class="sxs-lookup"><span data-stu-id="ccd03-178">You can add Azure Storage metrics charts for any of your storage accounts tooyour portal dashboard.</span></span>

1. <span data-ttu-id="ccd03-179">Kliknutím vyberte **úprava řídicího panelu** při zobrazení řídicího panelu v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ccd03-179">Select click **Edit dashboard** while viewing your dashboard in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="ccd03-180">V hello **dlaždice Galerie**, vyberte **najít dlaždice podle** > **typu**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-180">In hello **Tile Gallery**, select **Find tiles by** > **Type**.</span></span>
1. <span data-ttu-id="ccd03-181">Vyberte **typ** > **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-181">Select **Type** > **Storage accounts**.</span></span>
1. <span data-ttu-id="ccd03-182">V **prostředky**, vyberte účet úložiště hello jejichž metriky, které chcete tooadd toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="ccd03-182">In **Resources**, select hello storage account whose metrics you wish tooadd toohello dashboard.</span></span>
1. <span data-ttu-id="ccd03-183">Vyberte **kategorie** > **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-183">Select **Categories** > **Monitoring**.</span></span>
1. <span data-ttu-id="ccd03-184">Přetažení myší hello grafu dlaždice do řídicího panelu pro hello metriku, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="ccd03-184">Drag-and-drop hello chart tile onto your dashboard for hello metric you'd like displayed.</span></span> <span data-ttu-id="ccd03-185">Opakujte pro všechny metriky, které si přejete zobrazené na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ccd03-185">Repeat for all metrics you'd like displayed on hello dashboard.</span></span> <span data-ttu-id="ccd03-186">V hello následující bitové kopie graf "Objekty BLOB - celkový počet požadavků" hello je označený jako příklad, ale všechny grafy hello jsou k dispozici pro umístění na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="ccd03-186">In hello following image, hello "Blobs - Total requests" chart is highlighted as an example, but all hello charts are available for placement on your dashboard.</span></span>

   ![Galerie dlaždice na portálu Azure](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. <span data-ttu-id="ccd03-188">Vyberte **provádí přizpůsobení** v hello horní části řídicího panelu hello po dokončení přidávání grafy.</span><span class="sxs-lookup"><span data-stu-id="ccd03-188">Select **Done customizing** near hello top of hello dashboard when you're done adding charts.</span></span>

<span data-ttu-id="ccd03-189">Po přidání řídicího panelu tooyour grafy, můžete dále přizpůsobit je popsaný v [přizpůsobení metriky grafy](#how-to-customize-metrics-charts).</span><span class="sxs-lookup"><span data-stu-id="ccd03-189">Once you've added charts tooyour dashboard, you can further customize them as described in [Customize metrics charts](#how-to-customize-metrics-charts).</span></span>

## <a name="configure-logging"></a><span data-ttu-id="ccd03-190">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="ccd03-190">Configure logging</span></span>

<span data-ttu-id="ccd03-191">Můžete určit, aby Azure Storage toosave diagnostické protokoly pro čtení, zápisu a odstranit požadavky pro hello blob, tabulky a fronty služby.</span><span class="sxs-lookup"><span data-stu-id="ccd03-191">You can instruct Azure Storage toosave diagnostics logs for read, write, and delete requests for hello blob, table, and queue services.</span></span> <span data-ttu-id="ccd03-192">Hello zásad uchovávání dat, které nastavíte, platí také toothese protokoly.</span><span class="sxs-lookup"><span data-stu-id="ccd03-192">hello data retention policy you set also applies toothese logs.</span></span>

> [!NOTE]
> <span data-ttu-id="ccd03-193">Úložiště Azure File aktuálně podporuje metriky analytika úložiště, ale zatím nepodporuje protokolování.</span><span class="sxs-lookup"><span data-stu-id="ccd03-193">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>

1. <span data-ttu-id="ccd03-194">V hello [portál Azure](https://portal.azure.com), vyberte **účty úložiště**, pak hello název účtu úložiště hello, tooopen okně účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ccd03-194">In hello [Azure portal](https://portal.azure.com), select **Storage accounts**, then hello name of hello storage account tooopen hello storage account blade.</span></span>
1. <span data-ttu-id="ccd03-195">Vyberte **diagnostiky** v hello **monitorování** části hello nabídky okna.</span><span class="sxs-lookup"><span data-stu-id="ccd03-195">Select **Diagnostics** in hello **MONITORING** section of hello menu blade.</span></span>

    ![Diagnostika položka nabídky v části sledování v hello portálu Azure.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. <span data-ttu-id="ccd03-197">Ujistěte se, **stav** je nastaven příliš**na**a vyberte hello **služby** pro kterou chcete tooenable protokolování.</span><span class="sxs-lookup"><span data-stu-id="ccd03-197">Ensure **Status** is set too**On**, and select hello **services** for which you'd like tooenable logging.</span></span>

    ![Konfigurovat protokolování na hello portálu Azure.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. <span data-ttu-id="ccd03-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ccd03-199">Click **Save**.</span></span>

<span data-ttu-id="ccd03-200">Hello diagnostické protokoly jsou ukládány v kontejneru objektů blob s názvem $logs ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ccd03-200">hello diagnostics logs are saved in a blob container named $logs in your storage account.</span></span> <span data-ttu-id="ccd03-201">Můžete zobrazit pomocí Průzkumníka úložiště jako hello data protokolu hello [Microsoft Storage Explorer](http://storageexplorer.com), nebo programově pomocí klientské knihovny pro úložiště hello nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccd03-201">You can view hello log data using a storage explorer like hello [Microsoft Storage Explorer](http://storageexplorer.com), or programmatically using hello storage client library or PowerShell.</span></span>

<span data-ttu-id="ccd03-202">Informace o přístupu k hello $logs kontejneru najdete v tématu [povolení protokolování úložiště a přístup k datům protokolu](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span><span class="sxs-lookup"><span data-stu-id="ccd03-202">For information about accessing hello $logs container, see [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ccd03-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccd03-203">Next steps</span></span>

* <span data-ttu-id="ccd03-204">Najít další podrobnosti o [metriky, protokolování a fakturace](storage-analytics.md) pro analytika úložiště.</span><span class="sxs-lookup"><span data-stu-id="ccd03-204">Find more details about [metrics, logging, and billing](storage-analytics.md) for Storage Analytics.</span></span>
* <span data-ttu-id="ccd03-205">[Povolit daty metrik Azure Storage metriky a zobrazení](storage-enable-and-view-metrics.md) pomocí prostředí PowerShell a programově pomocí C#.</span><span class="sxs-lookup"><span data-stu-id="ccd03-205">[Enable Azure Storage metrics and view metrics data](storage-enable-and-view-metrics.md) by using PowerShell and programmatically with C#.</span></span>
