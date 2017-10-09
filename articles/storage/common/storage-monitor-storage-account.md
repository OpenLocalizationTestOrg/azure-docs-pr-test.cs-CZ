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
ms.openlocfilehash: 9a939e0b5db687c1b7b7857399321f681df2056a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-storage-account-in-hello-azure-portal"></a>Monitorování účtu úložiště v hello portálu Azure

[Azure Storage Analytics](../storage-analytics.md) poskytuje metriky pro všechny služby úložiště a protokoly pro objekty BLOB, fronty a tabulky. Můžete použít hello [portál Azure](https://portal.azure.com) tooconfigure protokoly a metriky, které se zaznamenávají pro váš účet a konfigurace grafů, které poskytují vizuální reprezentace vašich dat metriky.

> [!NOTE]
> Existují náklady spojené s zkoumání dat monitorování v hello portálu Azure. Další informace najdete v tématu [Storage Analytics a fakturace](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Úložiště Azure File aktuálně podporuje metriky analytika úložiště, ale zatím nepodporuje protokolování.
>
> Účty úložiště s typu replikaci z Zónově redundantní úložiště (ZRS) aktuálně nemají hello metriky nebo možnosti protokolování povoleny.
> 
> Pro podrobné příručce pomocí analytika úložiště a dalších nástrojů tooidentify diagnostikovat a vyřešit problémy související s Azure Storage najdete v tématu [monitorování, Diagnostika a řešení Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>Nakonfigurujte monitorování účtu úložiště

1. V hello [portál Azure](https://portal.azure.com), vyberte **účty úložiště**, pak hello účet název tooopen hello účtu řídicího panelu úložiště.
1. Vyberte **diagnostiky** v hello **monitorování** části hello nabídky okna.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. Vyberte hello **typ** metriky dat pro každou **služby** chcete toomonitor a hello **zásady uchovávání informací** pro hello data. Můžete také zakázat monitorování nastavením **stav** příliš**vypnout**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   Existují dva typy metrik, které můžete povolit u každé služby, které jsou povolené ve výchozím nastavení pro nové účty úložiště:

   * **Agregační**: shromažďuje metriky například procenta vstupní/výstupní, dostupnosti, latence a úspěch. Tyto metriky se shromažďují pro hello blob, fronty, tabulky a souborové služby.
   * **Na rozhraní API**: V přidání toohello agregovaná metrika, shromažďuje hello stejnou sadu metriky pro každé operace úložiště v hello rozhraní API služby Azure Storage.

   tooset hello zásady uchovávání informací, přesunutí hello **uchovávání dat (dny)** posuvníku nebo zadejte hello počet dnů, za které tooretain dat, z 1 too365. Hello výchozí pro nové účty úložiště je sedm dní. Pokud nechcete, aby tooset zásady uchovávání informací, zadejte nula. Pokud nejsou žádné zásady uchovávání informací, zapnutý hello toodelete tooyou dat monitorování.

   > [!WARNING]
   > Budou se vám účtovat, když ručně odstranit data metriky. Zastaralé analytická data (data starší než vaše zásady uchovávání informací) je odstraněn hello systému bez nákladů. Doporučujeme vám, nastavení zásady uchovávání informací založené na tom, jak dlouho chcete tooretain úložiště analytická data pro váš účet. V tématu [co poplatky, proveďte nimž dochází při povolení úložiště metriky?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) Další informace.
   >

1. Po dokončení konfigurace monitorování hello, vyberte **Uložit**.

Výchozí sadu metriky se zobrazí v grafech v okně účtu úložiště hello, jakož i hello jednotlivé služby oken (objekt blob, fronty, tabulky a soubor). Po povolení metrik pro službu, může trvat až hodinu tooan tooappear data v jeho grafy. Můžete vybrat **upravit** žádné metrika grafu příliš[konfigurace metriky, které](#how-to-customize-metrics-charts) se zobrazí v grafu hello.

Shromažďování metrik a protokolování můžete zakázat nastavením **stav** příliš**vypnout**.

> [!NOTE]
> Azure Storage používá [tabulky úložiště](../common/storage-introduction.md#table-storage) toostore hello metriky pro váš účet úložiště a metriky hello úložiště v tabulkách ve vašem účtu. Další informace najdete v tématu. [Ukládání metriky](../common/storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Přizpůsobení metriky grafy

Použijte následující postup toochoose hello které tooview metriky úložiště v grafu metriky. 

1. Spusťte zobrazením grafu metriky úložiště v hello portálu Azure. Můžete najít grafy na hello **okně účtu úložiště** a v hello **metriky** okno pro jednotlivé služby (objekt blob, fronty, tabulce, soubor).

   V tomto příkladu budeme pracovat s hello následující graf, který se zobrazí na hello **okně účtu úložiště**:

   ![Výběr grafu na portálu Azure](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Klikněte na tlačítko kdekoli v rámci hello grafu tooopen hello **metrika** okno. Vyberte **upravit graf** tooopen hello **upravit graf** okno.

   ![Upravit graf tlačítka v okně grafu](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. Na hello **upravit graf** okně, vyberte hello **časový rozsah** z toodisplay hello metriky v grafu hello a hello **služby** (objektů blob, fronty, tabulky, soubor) jejichž metriky, které chcete toodisplay. Zde jsme vybrali toodisplay hello za týden metriky pro služby objektů blob hello:

   ![Výběr časového rozsahu a služby v okně upravit graf hello](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. Vyberte hello individuální **metriky** jako měl zobrazit v grafu hello, pak klikněte na tlačítko **OK**. Například Zde jsme zvolili jste toodisplay hello *ContainerCount* a *ObjectCount* metriky:

   ![Jednotlivé metriky výběr v okně upravit graf](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

Nastavení grafu není ovlivnit hello kolekce, agregace nebo úložiště dat v účtu úložiště hello monitorování, pouze hello zobrazování dat metriky.

### <a name="metrics-availability-in-charts"></a>Metriky dostupnosti v diagramech

Hello seznam změn dostupné metriky založený na služby, která jste zvolili v hello rozevíracího seznamu a typ jednotky hello hello grafu, který upravujete. Například můžete vybrat metriky procento jako *PercentNetworkError* a *PercentThrottlingError* pouze v případě, že upravujete graf, který zobrazuje jednotky v procentech:

![Žádost o chybě procento grafu v hello portálu Azure](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Metriky řešení

Hello metriky, které jste vybrali v Diagnostika určuje hello rozlišení hello metriky, které jsou k dispozici pro váš účet:

* **Agregační** monitorování poskytuje metriky, jako je například procenta vstupní/výstupní, dostupnosti, latence a úspěch. Tyto metriky se shromažďují z hello blob, table, souboru a služba fronty.
* **Na rozhraní API** poskytuje lepší řešení s metriky, které jsou k dispozici pro operace jednotlivých úložiště navíc toohello úrovně služby agregace.

## <a name="configure-metrics-alerts"></a>Konfigurace metrik výstrah

Můžete vytvořit výstrahy toonotify případě pro metrika prostředků úložiště bylo dosaženo prahové hodnoty.

1. tooopen hello **pravidla výstrah okno**, projděte dolů toohello **monitorování** části hello **nabídky okno** a vyberte **výstrah pravidla**.
1. Vyberte **přidat upozornění** tooopen hello **přidání pravidla výstrahy** okno
1. Vyberte **prostředků** (objektů blob, soubor, fronty, tabulka) z rozevíracího seznamu hello a zadejte **název** a **popis** pro nové pravidlo výstrahy.
1. Vyberte hello **metrika** pro kterou chcete tooadd výstrahu, výstrahu **podmínku**a **prahová hodnota**. Hello prahová hodnota jednotky typu změny v závislosti na hello metrika jste zvolili. Například "count" je hello typ jednotky pro *ContainerCount*, při hello jednotku pro hello *PercentNetworkError* metrika je procento.
1. Vyberte hello **období**. Metriky, které dosažení nebo překročení hello prahovou hodnotu v rámci hello období spustí výstrahu.
1. (Volitelné) Konfigurace **e-mailu** a **Webhooku** oznámení. Další informace o webhooků, najdete v části [konfigurace webhook, jehož na výstrahu Azure metriky](../../monitoring-and-diagnostics/insights-webhooks-alerts.md). Pokud nenakonfigurujete oznámení e-mailu nebo webhooku, výstrahy se objeví jenom v hello portálu Azure.

![Okno: Přidání pravidla výstrahy"v hello portálu Azure](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-toohello-portal-dashboard"></a>Přidat řídicí panel metriky grafy toohello portálu

Můžete přidat grafy metrik Azure Storage pro žádné úložiště účtů tooyour řídicího panelu portálu.

1. Kliknutím vyberte **úprava řídicího panelu** při zobrazení řídicího panelu v hello [portál Azure](https://portal.azure.com).
1. V hello **dlaždice Galerie**, vyberte **najít dlaždice podle** > **typu**.
1. Vyberte **typ** > **účty úložiště**.
1. V **prostředky**, vyberte účet úložiště hello jejichž metriky, které chcete tooadd toohello řídicího panelu.
1. Vyberte **kategorie** > **monitorování**.
1. Přetažení myší hello grafu dlaždice do řídicího panelu pro hello metriku, které chcete zobrazit. Opakujte pro všechny metriky, které si přejete zobrazené na řídicím panelu hello. V hello následující bitové kopie graf "Objekty BLOB - celkový počet požadavků" hello je označený jako příklad, ale všechny grafy hello jsou k dispozici pro umístění na řídicím panelu.

   ![Galerie dlaždice na portálu Azure](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. Vyberte **provádí přizpůsobení** v hello horní části řídicího panelu hello po dokončení přidávání grafy.

Po přidání řídicího panelu tooyour grafy, můžete dále přizpůsobit je popsaný v [přizpůsobení metriky grafy](#how-to-customize-metrics-charts).

## <a name="configure-logging"></a>Konfigurace protokolování

Můžete určit, aby Azure Storage toosave diagnostické protokoly pro čtení, zápisu a odstranit požadavky pro hello blob, tabulky a fronty služby. Hello zásad uchovávání dat, které nastavíte, platí také toothese protokoly.

> [!NOTE]
> Úložiště Azure File aktuálně podporuje metriky analytika úložiště, ale zatím nepodporuje protokolování.
>

1. V hello [portál Azure](https://portal.azure.com), vyberte **účty úložiště**, pak hello název účtu úložiště hello, tooopen okně účtu úložiště hello.
1. Vyberte **diagnostiky** v hello **monitorování** části hello nabídky okna.

    ![Diagnostika položka nabídky v části sledování v hello portálu Azure.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. Ujistěte se, **stav** je nastaven příliš**na**a vyberte hello **služby** pro kterou chcete tooenable protokolování.

    ![Konfigurovat protokolování na hello portálu Azure.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. Klikněte na **Uložit**.

Hello diagnostické protokoly jsou ukládány v kontejneru objektů blob s názvem $logs ve vašem účtu úložiště. Můžete zobrazit pomocí Průzkumníka úložiště jako hello data protokolu hello [Microsoft Storage Explorer](http://storageexplorer.com), nebo programově pomocí klientské knihovny pro úložiště hello nebo prostředí PowerShell.

Informace o přístupu k hello $logs kontejneru najdete v tématu [povolení protokolování úložiště a přístup k datům protokolu](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).

## <a name="next-steps"></a>Další kroky

* Najít další podrobnosti o [metriky, protokolování a fakturace](../storage-analytics.md) pro analytika úložiště.
* [Povolit daty metrik Azure Storage metriky a zobrazení](../storage-enable-and-view-metrics.md) pomocí prostředí PowerShell a programově pomocí C#.
