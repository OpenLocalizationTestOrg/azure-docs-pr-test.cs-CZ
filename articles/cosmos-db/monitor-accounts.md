---
title: "Sledování požadavků Azure Cosmos DB a úložiště | Microsoft Docs"
description: "Naučte se monitorovat účtu Azure Cosmos DB metrik výkonu, například požadavky a chyby serveru a metriky využití, například spotřebu úložiště."
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
ms.openlocfilehash: 0ca652d31d6c50124f87916b4486d8279075f106
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-azure-cosmos-db-requests-usage-and-storage"></a>Sledování požadavků, využití a úložiště Azure Cosmos DB
Můžete monitorovat své účty Azure Cosmos DB [portál Azure](https://portal.azure.com/). U každého účtu Azure Cosmos DB jsou k dispozici obě metrik výkonu, například požadavky a chyby serveru a metriky využití, například spotřebu úložiště.

Metriky lze zobrazit v okně účtu nové okno metriky, nebo v Azure monitorování.

## <a name="view-performance-metrics-on-the-metrics-blade"></a>Zobrazení výkonu metriky v okně metriky
1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **více služeb**, přejděte na **databáze**, klikněte na tlačítko **Azure Cosmos DB**a potom klikněte na název účtu Azure Cosmos DB, pro který chcete zobrazit metrik výkonu.
2. V nabídce prostředků najdete v části **monitorování**, klikněte na tlačítko **metriky**.

Otevře se okno metriky a můžete vybrat kolekci, chcete-li zkontrolovat. Můžete zkontrolovat dostupnost, požadavků, propustnost a úložiště metriky a porovnejte je s SLA DB Cosmos Azure.

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Zobrazení metriky výkonu pomocí sledování Azure
1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **monitorování** na panelu vlevo.
2. V nabídce prostředků, klikněte na tlačítko **metriky**.
3. V **monitorování – metriky** okno v **skupiny zdrojů** rozevírací nabídce vyberte skupinu prostředků, které jsou přidružené k účtu Azure Cosmos DB, které chcete monitorovat. 
4. V **prostředků** rozevírací nabídce vyberte možnost databáze účet k monitorování.
5. V seznamu **dostupné metriky**, vybrat metriky pro zobrazení. Pomocí tlačítka CTRL k vícenásobným výběrem. 

    Vaše metriky se zobrazují na v **vykreslení** okno. 

## <a name="view-performance-metrics-on-the-account-blade"></a>Zobrazení výkonu metriky v okně účtu
1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **více služeb**, přejděte na **databáze**, klikněte na tlačítko **Azure Cosmos DB**a potom klikněte na název účtu Azure Cosmos DB, pro který chcete zobrazit metrik výkonu.
2. **Monitorování** přehledu ve výchozím nastavení zobrazí tyto dlaždice:
   
   * Celkový počet požadavků aktuálního dne.
   * Použít úložiště.
   
   Pokud vaše tabulka zobrazuje **nejsou dostupná žádná data** a domníváte se nacházejí data v databázi, najdete v článku [Poradce při potížích s](#troubleshooting) části.
   
   ![Snímek obrazovky přehledu monitorování, které uvádí požadavky a využití úložiště](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Kliknutím na **požadavky** nebo **kvóty využití** dlaždice otevře podrobné **metrika** okno.
4. **Metrika** okno se zobrazují podrobnosti o podle metrik, které jste vybrali.  V horní části okna je graf požadavků grafu zobrazena každou hodinu a nižší, než je tabulka, která znázorňuje hodnoty agregace pro omezenému a celkový počet požadavků.  Okno metriky také ukazuje seznam výstrah, které byly definovány, filtruje tak, aby metriky, které se zobrazí v okně metriky aktuální (tímto způsobem, pokud máte počet výstrah, se zobrazí pouze relevantní těm, které jsou uvedeny v tomto tématu).   
   
   ![Snímek obrazovky okna metriky, včetně omezení žádostí](./media/monitor-accounts/documentdb-metric-blade.png)

## <a name="customize-performance-metric-views-in-the-portal"></a>Přizpůsobit zobrazení metriky výkonu portálu
1. Chcete-li přizpůsobit metriky, které se zobrazí v konkrétní grafu, klikněte na graf a otevřete ho v **metrika** okna a pak klikněte na tlačítko **upravit graf**.  
   ![Snímek obrazovky prvků metriky okno se zvýrazněnou upravit graf](./media/monitor-accounts/madocdb3.png)
2. Na **upravit graf** okně jsou možnosti změnit metriky, které se zobrazí v grafu, jakož i jejich časové rozmezí.  
   ![Snímek obrazovky okna upravit graf](./media/monitor-accounts/madocdb4.png)
3. Pokud chcete změnit metriky zobrazené v části, jednoduše vyberte metriku výkonu k dispozici nebo zrušte klikněte **OK** v dolní části okna.  
4. Chcete-li změnit časový rozsah, vyberte jiný rozsah (například **vlastní**) a potom klikněte na **OK** v dolní části okna.  
   
   ![Snímek obrazovky časový rozsah součástí v okně upravit graf znázorňující zadejte vlastní časový rozsah](./media/monitor-accounts/madocdb5.png)

## <a name="create-side-by-side-charts-in-the-portal"></a>Vytváření grafů vedle sebe na portálu
Na portálu Azure můžete vytvořit metriky grafů vedle sebe.  

1. První, klikněte pravým tlačítkem na graf, které chcete kopírovat a vyberte **přizpůsobit**.
   
   ![Snímek obrazovky celkový počet požadavků grafu se zvýrazněnou možností přizpůsobení](./media/monitor-accounts/madocdb6.png)
2. Klikněte na tlačítko **klon** v nabídce zkopírujte část, a potom klikněte na **provádí přizpůsobení**.
   
   ![Obrazovka snímek grafu celkový počet požadavků s klonu a provádět přizpůsobení možnosti zvýrazněná](./media/monitor-accounts/madocdb7.png)  

Tuto část může nyní považovat za jakékoli jiné metriky část přizpůsobení metriky a časový rozsah, v části zobrazí.  Tímto způsobem, uvidíte dvě různé metriky grafu-souběžného ve stejnou dobu.  
    ![Snímek obrazovky graf celkový počet požadavků a nové celkový počet požadavků za hodinu grafu](./media/monitor-accounts/madocdb8.png)  

## <a name="set-up-alerts-in-the-portal"></a>Nastavit výstrahy na portálu
1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **více služeb**, klikněte na tlačítko **Azure Cosmos DB**a potom klikněte na název účtu Azure Cosmos DB, pro kterou chcete nastavit upozornění metriky výkonu.
2. V nabídce prostředků, klikněte na tlačítko **pravidla výstrah** otevřete okno pravidla výstrah.  
   ![Snímek obrazovky výstrahy pravidla vybrané části](./media/monitor-accounts/madocdb10.5.png)
3. V **výstrah pravidla** okně klikněte na tlačítko **přidat upozornění**.  
   ![Snímek obrazovky okna pravidla výstrah pomocí tlačítka Přidat výstrahy zvýrazněná](./media/monitor-accounts/madocdb11.png)
4. V **přidání pravidla výstrahy** okno, zadejte:
   
   * Název pravidla výstrahy, které nastavíte.
   * Popis nové pravidlo výstrahy.
   * Metrika pro pravidlo výstrahy.
   * Podmínka, prahovou hodnotu a období určující, když se aktivuje výstrahu. Například server chyba počet větší než 5 za posledních 15 minut.
   * Jestli správce služeb a coadministrators jsou e-mailem při aktivuje výstrahu.
   * Další e-mailové adresy pro oznámení o výstrahách.  
     ![Snímek obrazovky s možností přidat okno pravidlo výstrahy](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programatically"></a>Monitorování databáze Azure Cosmos programově
Účet úrovně metriky dostupné na portálu, jako je například využití a celkový počet požadavků na účet úložiště, nejsou k dispozici prostřednictvím rozhraní API DocumentDB. Data o využití na úrovni kolekce však můžete načíst pomocí rozhraní API DocumentDB. Načtení dat na úrovni kolekce, postupujte takto:

* Chcete-li použít rozhraní API REST [provádět GET na kolekci](https://msdn.microsoft.com/library/mt489073.aspx). V záhlaví x-ms-resource kvóty a x-ms-resource využití v odpovědi se vrátí informace kvóta a využití pro kolekci.
* Chcete-li používat sadu .NET SDK, použijte [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) metoda, která vrátí hodnotu [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) obsahující počet použití vlastnosti, jako **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**a další.

Chcete-li získat přístup k další metriky, použijte [Azure monitorování SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). K dispozici definice metrik můžete načíst pomocí volání:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Dotazy k načtení jednotlivé metriky použijte následující formát:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Další informace najdete v tématu [načítání prostředků metriky přes REST API Azure monitorování](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Všimněte si, že se přejmenoval "Azure Inights" "Azure sledování".  Tuto položku blogu odkazuje na starší název.

## <a name="troubleshooting"></a>Řešení potíží
Pokud monitorování dlaždice zobrazovaný **nejsou dostupná žádná data** zprávu a nedávno provedených požadavků nebo přidání dat do databáze, můžete upravit na dlaždici tak, aby odrážela posledního použití.

### <a name="edit-a-tile-to-refresh-current-data"></a>Upravit dlaždici aktualizovat aktuální data
1. Chcete-li přizpůsobit metriky, které do určité části zobrazení, klikněte na graf a otevřete **metrika** okna a pak klikněte na tlačítko **upravit graf**.  
   ![Snímek obrazovky prvků metriky okno se zvýrazněnou upravit graf](./media/monitor-accounts/madocdb3.png)
2. Na **upravit graf** okno v **časový rozsah** klikněte na tlačítko **po hodině**a potom klikněte na **OK**.  
   ![Snímek obrazovky okna upravit graf s vybrána poslední hodinu](./media/monitor-accounts/documentdb-no-available-data-past-hour.png)
3. Dlaždice musí nyní aktualizuje aktuální data a využití.  
   ![Snímek obrazovky znázorňující aktualizovaný celkový počet požadavků za hodinu dlaždice](./media/monitor-accounts/documentdb-no-available-data-fixed.png)

## <a name="next-steps"></a>Další kroky
Další informace o plánování kapacity Azure Cosmos DB, najdete v článku [kalkulačky Plánovač kapacity Azure Cosmos DB](https://www.documentdb.com/capacityplanner).

