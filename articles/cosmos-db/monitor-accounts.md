---
title: "aaaMonitor Azure Cosmos DB požadavky a úložiště | Microsoft Docs"
description: "Zjistěte, jak toomonitor vaše Azure DB Cosmos účet pro metrik výkonu, například požadavky a chyby serveru a metriky využití, například spotřebu úložiště."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 4c6a2e6f-6e78-48e3-8dc6-f4498b235a9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: mimig
ms.openlocfilehash: aea029d10717236a573a080dab9d06d87f97f318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-cosmos-db-requests-usage-and-storage"></a>Sledování požadavků, využití a úložiště Azure Cosmos DB
Můžete monitorovat své účty Azure Cosmos DB hello [portál Azure](https://portal.azure.com/). U každého účtu Azure Cosmos DB jsou k dispozici obě metrik výkonu, například požadavky a chyby serveru a metriky využití, například spotřebu úložiště.

Metriky lze zobrazit v okně účtu hello hello nové okno metriky, nebo v Azure monitorování.

## <a name="view-performance-metrics-on-hello-metrics-blade"></a>Zobrazení výkonu metriky v okně metriky hello
1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **více služeb**, posuňte se příliš**databáze**, klikněte na tlačítko **Azure Cosmos DB**a potom klikněte na název hello hello Účet Azure Cosmos DB pro kterou chcete tooview metrik výkonu.
2. V nabídce hello prostředků v části **monitorování**, klikněte na tlačítko **metriky**.

Otevře se okno metriky Hello a můžete vybrat kolekci tooreview hello. Můžete zkontrolovat dostupnost, požadavků, propustnost a úložiště metriky a porovnat je toohello Azure Cosmos DB SLA.

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Zobrazení metriky výkonu pomocí sledování Azure
1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **monitorování** na hello vlevo.
2. V nabídce hello prostředků, klikněte na tlačítko **metriky**.
3. V hello **monitorování – metriky** okno, v hello **skupiny zdrojů** rozevírací nabídky vyberte hello skupiny prostředků spojené s účtem Azure Cosmos DB hello, které chcete toomonitor. 
4. V hello **prostředků** rozevírací nabídky, vyberte hello databáze účet toomonitor.
5. V seznamu hello **dostupné metriky**, vyberte toodisplay metriky hello. Použijte tlačítko CTRL hello toomulti – vybrat. 

    Vaše metriky se zobrazují na v hello **vykreslení** okno. 

## <a name="view-performance-metrics-on-hello-account-blade"></a>Zobrazení výkonu metriky v okně účtu hello
1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **více služeb**, posuňte se příliš**databáze**, klikněte na tlačítko **Azure Cosmos DB**a potom klikněte na název hello hello Účet Azure Cosmos DB pro kterou chcete tooview metrik výkonu.
2. Hello **monitorování** hello následující dlaždice ve výchozím nastavení zobrazí přehledu:
   
   * Celkový počet požadavků pro hello aktuálního dne.
   * Použít úložiště.
   
   Pokud vaše tabulka zobrazuje **nejsou dostupná žádná data** a domníváte se nacházejí data do databáze najdete v tématu hello [Poradce při potížích s](#troubleshooting) části.
   
   ![Snímek obrazovky hello přehledu monitorování, které uvádí požadavky hello a hello využití úložiště](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Kliknutím na hello **požadavky** nebo **kvóty využití** dlaždice otevře podrobné **metrika** okno.
4. Hello **metrika** okno se zobrazují podrobnosti o hello metriky, které jste vybrali.  V hello horní části okna hello je graf požadavků grafu zobrazena každou hodinu a nižší, než je tabulka, která znázorňuje hodnoty agregace pro omezenému a celkový počet požadavků.  Hello metriky okno také ukazuje hello seznam výstrah, které byly definované, která jsou filtrovaná toohello metriky, které se zobrazují v aktuálním okně metriky hello (tímto způsobem, pokud máte počet výstrah, se zobrazí pouze hello relevantní ty, které jsou uvedeny v tomto tématu).   
   
   ![Snímek obrazovky okna metriky hello, která zahrnuje omezuje požadavky](./media/monitor-accounts/documentdb-metric-blade.png)

## <a name="customize-performance-metric-views-in-hello-portal"></a>Přizpůsobit zobrazení metriky výkonu portálu hello
1. toocustomize hello metriky, které se zobrazují v konkrétní grafu, klikněte na graf tooopen hello v hello **metrika** okna a pak klikněte na tlačítko **upravit graf**.  
   ![Snímek obrazovky hello metriky okno Ovládací prvky se zvýrazněnou upravit graf](./media/monitor-accounts/madocdb3.png)
2. Na hello **upravit graf** okně jsou možnosti toomodify hello metriky, které se zobrazí v grafu hello, jakož i jejich časové rozmezí.  
   ![Snímek obrazovky okna upravit graf hello](./media/monitor-accounts/madocdb4.png)
3. toochange hello metriky zobrazené v rámci hello, jednoduše vyberte nebo zrušte hello metriky výkonu k dispozici a klikněte **OK** v hello dolní části okna hello.  
4. toochange hello časového rozsahu, vyberte jiný rozsah (například **vlastní**) a potom klikněte na **OK** v hello dolní části okna hello.  
   
   ![Snímek obrazovky znázorňující hello časový rozsah součástí hello upravit graf znázorňující způsob tooenter vlastní časový rozsah](./media/monitor-accounts/madocdb5.png)

## <a name="create-side-by-side-charts-in-hello-portal"></a>Vytváření grafů vedle sebe hello portálu
Hello portálu Azure můžete grafy metriky toocreate vedle sebe.  

1. První, klikněte pravým tlačítkem na graf hello toocopy a zaškrtněte **přizpůsobit**.
   
   ![Snímek obrazovky aplikace hello celkový počet požadavků grafu se zvýrazněnou možností přizpůsobení hello](./media/monitor-accounts/madocdb6.png)
2. Klikněte na tlačítko **klon** hello část hello toocopy nabídky a pak klikněte na tlačítko **provádí přizpůsobení**.
   
   ![Obrazovka snímek hello grafu celkový počet požadavků s hello klonování a provádět přizpůsobení možnosti zvýrazněná](./media/monitor-accounts/madocdb7.png)  

Tuto část může nyní považovat za jiné metriky část, přizpůsobení metriky a časový rozsah hello zobrazí v rámci hello.  Tímto způsobem, uvidíte dvě různé metriky grafu-souběžného v hello stejnou dobu.  
    ![Snímek obrazovky znázorňující hello celkový počet požadavků grafu a hello nové celkový počet požadavků za hodinu grafu](./media/monitor-accounts/madocdb8.png)  

## <a name="set-up-alerts-in-hello-portal"></a>Nastavení výstrah portálu hello
1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **více služeb**, klikněte na tlačítko **Azure Cosmos DB**a potom klikněte na název hello hello Azure Cosmos DB účtu, pro kterou chcete toosetup metriky výstrahy výkonu.
2. V nabídce hello prostředků, klikněte na tlačítko **pravidla výstrah** tooopen hello pravidla výstrah okno.  
   ![Snímek obrazovky hello výstraha pravidla vybrané části](./media/monitor-accounts/madocdb10.5.png)
3. V hello **výstrah pravidla** okně klikněte na tlačítko **přidat upozornění**.  
   ![Snímek obrazovky okna hello pravidla výstrah, pomocí tlačítka Přidat výstrahy hello zvýrazněná](./media/monitor-accounts/madocdb11.png)
4. V hello **přidání pravidla výstrahy** okno, zadejte:
   
   * Název Hello hello pravidlo výstrahy, které nastavujete.
   * Popis nové pravidlo výstrahy hello.
   * Hello Metrika pro pravidlo výstrahy hello.
   * Hello podmínku, prahovou hodnotu a období určující, když se aktivuje výstraha hello. Například server chyba počet větší než 5 přes hello posledních 15 minut.
   * Tom, zda text hello Správce služeb a coadministrators jsou e-mailem při aktivaci výstrahy hello.
   * Další e-mailové adresy pro oznámení o výstrahách.  
     ![Snímek obrazovky hello pravidlo výstrahy okna Přidat](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programatically"></a>Monitorování databáze Azure Cosmos programově
Dobrý den účet metriky na úrovni v hello portálu k dispozici, jako je například účet úložiště využití a celkový počet požadavků, nejsou k dispozici prostřednictvím hello rozhraní API pro DocumentDB. Data o využití na úrovni kolekce hello však můžete načíst pomocí rozhraní API DocumentDB hello. tooretrieve shromažďování dat na úrovni, hello následující:

* toouse hello rozhraní REST API, [provést GET na kolekci hello](https://msdn.microsoft.com/library/mt489073.aspx). v záhlaví x-ms-resource kvóty a x-ms-resource využití hello v hello odpověď se vrátí Hello kvóta a využití informace pro kolekci hello.
* toouse hello .NET SDK, pomocí hello [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) metoda, která vrátí hodnotu [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) obsahující počet použití vlastnosti, jako  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**a další.

tooaccess další metriky, použijte hello [Azure monitorování SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). K dispozici definice metrik můžete načíst pomocí volání:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Hello použití jednotlivé metriky tooretrieve dotazy následující formát:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Další informace najdete v tématu [načítání metrika prostředků prostřednictvím hello REST API služby Azure monitorování](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Všimněte si, že se přejmenoval "Azure Inights" "Azure sledování".  Tuto položku blogu odkazuje toohello starší název.

## <a name="troubleshooting"></a>Řešení potíží
Pokud monitorování dlaždice zobrazení hello **nejsou dostupná žádná data** zprávu a nedávno provedených požadavků nebo přidat databáze toohello data, můžete upravit hello dlaždice tooreflect hello posledního použití.

### <a name="edit-a-tile-toorefresh-current-data"></a>Upravit aktuální data dlaždice toorefresh
1. toocustomize hello metriky, které se zobrazují na určité části, klikněte na tlačítko hello grafu tooopen hello **metrika** okna a pak klikněte na tlačítko **upravit graf**.  
   ![Snímek obrazovky hello metriky okno Ovládací prvky se zvýrazněnou upravit graf](./media/monitor-accounts/madocdb3.png)
2. Na hello **upravit graf** okno, v hello **časový rozsah** klikněte na tlačítko **po hodině**a potom klikněte na **OK**.  
   ![Snímek obrazovky okna upravit graf hello s poslední hodinu vybrané](./media/monitor-accounts/documentdb-no-available-data-past-hour.png)
3. Dlaždice musí nyní aktualizuje aktuální data a využití.  
   ![Snímek obrazovky znázorňující hello aktualizovat celkový počet požadavků za hodinu dlaždice](./media/monitor-accounts/documentdb-no-available-data-fixed.png)

## <a name="next-steps"></a>Další kroky
toolearn Další informace o plánování kapacity Azure Cosmos DB, najdete v části hello [kalkulačky Plánovač kapacity Azure Cosmos DB](https://www.documentdb.com/capacityplanner).

