---
title: "aaaUse analýzy protokolů a aplikaci víceklientské databáze SQL | Microsoft Docs"
description: "Instalace a použití analýzy protokolů (OMS) s hello Azure SQL Database ukázkové Wingtip SaaS aplikace"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: billgib; sstein
ms.openlocfilehash: fa9085ce3462939e66853faa2a3cd71e0f6c2581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setup-and-use-log-analytics-oms-with-hello-wingtip-saas-app"></a>Instalace a použití analýzy protokolů (OMS) s hello Wingtip SaaS aplikace

V tomto kurzu, nastavení a použití *analýzy protokolů ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* pro monitorování elastické fondy a databází. Vychází hello [sledování výkonu a správy kurzu](sql-database-saas-tutorial-performance-monitoring.md)a ukazuje, jak toouse *analýzy protokolů* tooaugment hello monitorování a výstrah, které jsou uvedeny v hello portálu Azure. Služba Log Analytics je zvlášť vhodná pro monitorování a upozorňování v různém rozsahu, protože podporuje stovky fondů a stovky tisíc databází. Poskytuje také jedno řešení pro monitorování, které může integrovat monitorování různých aplikací a služeb Azure napříč několika předplatnými Azure.

Co se v tomto kurzu naučíte:

> [!div class="checklist"]
> * Instalace a konfigurace Log Analytics (OMS)
> * Pomocí analýzy protokolů toomonitor fondy a databáze

toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

* Hello Wingtip SaaS aplikace je nasazená. toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

V tématu hello [sledování výkonu a správy kurzu](sql-database-saas-tutorial-performance-monitoring.md) diskuzi o hello SaaS scénáře a vzory a jejich vlivu hello požadavky na řešení monitorování.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Monitorování a správa výkonu pomocí Log Analytics (OMS)

Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech. Toto integrované monitorování a výstrah je konkrétní prostředky a vhodné pro malý počet prostředků, ale je méně rozsáhlé instalace skvěle hodí toomonitoring nebo pro zajištění jednotný pohled napříč různými prostředky a odběry.

Log Analytics je možné používat u velkoobjemových scénářů. Toto je samostatný služba Azure, která nabízí v porovnání s emitovaného diagnostické protokoly analýzy a telemetrie získané v pracovním prostoru analýzy protokolů, které může shromažďovat telemetrická data z mnoha služby a být použité tooquery a nastavit výstrahy. Log Analytics poskytuje integrovaný jazyk dotazů a nástroje vizualizace dat umožňující analýzy a vizualizaci provozních dat. Hello řešení analýzy SQL poskytuje několik předem definovaných elastický fond a databáze, monitorování a výstrah, zobrazení a dotazy a umožňuje přidávat své vlastní dotazy ad hoc a uložit tyto podle potřeby. OMS poskytuje také vlastní návrhář zobrazení.

Pracovní prostory a analýzy analytická řešení protokolu lze otevřít v hello portál Azure a v OMS. Hello portál Azure je hello novější přístupový bod, ale mohou být za portálu OMS hello v určité oblasti.

### <a name="start-hello-load-generator-toocreate-data-tooanalyze"></a>Spustit hello zatížení generátor toocreate data tooanalyze

1. Otevřete **ukázku PerformanceMonitoringAndManagement.ps1** v hello **prostředí PowerShell ISE**. Uchovávejte tento skript otevřete tak, jak chcete toorun několik scénářů generování zatížení hello během tohoto kurzu.
1. Pokud máte méně než pět klienty, zřídit dávky klienty tooprovide zajímavějšího monitorování kontextu:
   1. Nastavte **$DemoScenario = 1,** **Zřízení dávky tenantů**
   1. Stiskněte klávesu **F5** toorun hello skriptu.

1. Nastavte **$DemoScenario** = 2, **Generování normální intenzity zatížení (asi 40 DTU)**.
1. Stiskněte klávesu **F5** toorun hello skriptu.

## <a name="get-hello-wingtip-application-scripts"></a>Získat hello Wingtip aplikační skripty

Hello Wingtip lístky skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. Skript soubory jsou umístěny v hello [Learning moduly složky](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhnout hello **Learning moduly** místní počítač tooyour složky, údržby jeho strukturu složek.

## <a name="installing-and-configuring-log-analytics-and-hello-azure-sql-analytics-solution"></a>Instalace a konfigurace analýzy protokolů a hello řešení analýzy SQL Azure

Analýzy protokolů je samostatné služby, který vyžaduje toobe nakonfigurované. Log Analytics shromažďuje data a telemetrii protokolů a metriku v pracovním prostoru Log Analytics. Pracovní prostor je prostředek stejně jako ostatní prostředky v Azure a je potřeba ho vytvořit. Při hello prostoru nepotřebuje toobe vytvořené v hello stejné skupině prostředků jako aplikace hello je monitorování, tato možnost může hello nejvhodnější. V případě hello hello Wingtip SaaS aplikace to umožňuje toobe prostoru hello snadno odstranit aplikaci hello jednoduše odstraněním skupiny prostředků hello.

1. Otevřete... \\Learning moduly\\sledování výkonu a správy\\analýzy protokolů\\*ukázku LogAnalytics.ps1* v hello **prostředí PowerShell ISE**.
1. Stiskněte klávesu **F5** toorun hello skriptu.

V tomto okamžiku byste měli mít možnost Otevřít analýzy protokolů hello portálu Azure (nebo portálu OMS hello). To bude trvat několik minut, než telemetrie toobe shromážděny v hello pracovní prostor analýzy protokolů a toobecome viditelné. Hello delší že necháte hello systému shromažďování, že data hello zajímavějšího hello prostředí bude. Teď je vhodná doba toograb nápoj - jenom Ujistěte se, generátor zatížení hello je stále spuštěná!


## <a name="use-log-analytics-and-hello-sql-analytics-solution-toomonitor-pools-and-databases"></a>Pomocí analýzy protokolů a hello fondy toomonitor řešení SQL Analytics a databáze


V tomto cvičení otevřete analýzy protokolů a hello toolook portálu OMS na telemetrie hello shromážděných pro hello databáze a fondy.

1. Procházet toohello [portál Azure](https://portal.azure.com) a otevřete analýzy protokolů tak, že kliknete na další služby a potom najděte analýzy protokolů:

   ![otevření log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Vyberte pracovní prostor hello s názvem *wtploganalytics -&lt;uživatele&gt;*.

1. Vyberte **přehled** tooopen hello analýzy protokolů řešení v hello portálu Azure.
   ![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **Důležité**: může trvat několik minut, než je aktivní hello řešení. Buďte prosím trpěliví.

1. Klikněte na hello Azure SQL Analytics dlaždice tooopen ho.

    ![overview](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. Hello zobrazení v hello řešení okno posouvá do stran, s vlastní posuvníku v dolní části hello (aktualizace hello okně potřeby).

1. Prozkoumejte svislé hello různých zobrazení kliknutím na nich nebo na jednotlivé prostředky tooopen procházení explorer, kde můžete použít hello čas posuvníku v hello top doleva nebo klikněte na panel toofocus v na užší časovém intervalu. V tomto zobrazení si můžete vybrat jednotlivé databáze nebo fondy toofocus na konkrétní prostředky:

    ![graf](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Zpět v okně řešení hello Pokud se posunete toohello nejvíce vpravo uvidíte některé uložené dotazy, můžete kliknutím na tooopen a prozkoumat. Můžete je zkoušet měnit a můžete si uložit některé zajímavé dotazy, které vytvoříte, a později je znovu otevřít a používat s jinými prostředky.

1. Zpět v okně pracovní prostor analýzy protokolů hello vyberte portálu OMS tooopen hello řešení existuje.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. Na portálu OMS hello můžete nakonfigurovat výstrahy. Klikněte na hello výstrahy část hello databáze DTU zobrazení.

1. V hello protokol hledání se zobrazí zobrazení, které se vám zobrazí pruhový graf reprezentované metrik hello.

    ![prohledávání protokolů](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Pokud kliknete na výstrahu v panelu nástrojů hello bude možné toosee hello výstrah Konfigurace a můžete ho změnit.

    ![přidání pravidla výstrahy](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

Hello monitorování a generování výstrah v analýzy protokolů a OMS je založená na dotazech nad daty hello v hello prostoru, na rozdíl od hello výstrahy v okně každý prostředek, který je konkrétní prostředky. Proto můžete definovat výstrahu, která dohlíží na všechny databáze místo definování samostatné výstrahy pro každou databázi. Nebo můžete zapsat výstrahu, která používá složený dotaz přes více typů prostředků. Dotazy jsou omezena pouze hello data k dispozici v pracovním prostoru hello.

Analýzy protokolů pro databáze SQL je účtovat na základě objemu dat hello v prostoru hello. V tomto kurzu jste vytvořili volného prostoru, což je omezená too500MB za den. Po dosažení tohoto limitu data je již přidána toohello prostoru.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Instalace a konfigurace Log Analytics (OMS)
> * Pomocí analýzy protokolů toomonitor fondy a databáze

[Kurz Analýza tenanta](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které stavějí hello počáteční nasazení Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
