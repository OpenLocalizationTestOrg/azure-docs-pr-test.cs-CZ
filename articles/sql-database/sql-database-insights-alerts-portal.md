---
title: "výstrahy databáze SQL Azure portálu toocreate aaaUse | Microsoft Docs"
description: "Použijte hello portálu toocreate Azure SQL Database výstrahy, které můžete aktivovat oznámení nebo automatizace při splnění zadané podmínky hello."
author: aamalvea
manager: jhubbard
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: 4e494b130a26c4cdf42445cb49648fce9bf4d300
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-toocreate-alerts-for-azure-sql-database-and-data-warehouse"></a>Použití portálu Azure toocreate výstrahy pro Azure SQL Database a datového skladu

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak hello tooset do Azure SQL Database a datový sklad výstrah pomocí portálu Azure. Tento článek také obsahuje osvědčené postupy pro nastavení výstrah tečky.    

Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.

* **Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech. To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.    
* **Aktivity protokolu události** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k překročení určitého počtu událostí.

Můžete nakonfigurovat výstrahy toodo hello následující při aktivaci:

* odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci
* odesílání e-mailů tooadditional e-mailů, které zadáte.
* Volat webhook, jehož

Můžete nakonfigurovat a získat informace o použití pravidla výstrah

* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [rozhraní příkazového řádku (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a>Vytvořit pravidlo výstrahy na metrika s hello portálu Azure
1. V hello [portál](https://portal.azure.com/), vyhledejte hello prostředků, které vás zajímají monitorování a vyberte ho.
2. Tento krok je jiný pro databázi SQL a elastické fondy a datovým Skladem SQL: 

   - **Databáze SQL & pouze elastické fondy**: vyberte **výstrahy** nebo **výstrah pravidla** pod hello části monitorování. Ikona a textu Hello se mohou mírně lišit pro různé prostředky.  
   
     ![Monitorování](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **SQL DW pouze**: vyberte **monitorování** pod hello části Běžné úlohy. Klikněte na tlačítko hello **DWU využití** grafu.

     ![BĚŽNÉ ÚLOHY](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Vyberte hello **přidat upozornění** příkazů a vyplňte pole hello.
   
    ![Přidání oznámení](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.
5. Vyberte hello **metrika** toomonitor a pak vyberte **podmínku** a **prahová hodnota** hodnotu metriky hello. Také si zvolili hello **období** času, který hello metrika pravidlo je nutné splnit před hello výstrahy aktivační události. Tak například pokud používáte období hello "PT5M" a upozornění hledá procesoru vyšší než 80 %, hello výstraha se aktivuje při hello procesoru bylo konzistentně výše 80 % 5 minut. Jakmile dojde k první aktivační událost hello, se znovu spustí, když hello procesoru zůstává nižší než 80 % 5 minut. Hello procesoru měření dochází každých 1 minuta.   
6. Zkontrolujte **e-mailu vlastníky...**  Pokud chcete, aby správci a spolusprávci toobe e-mailem při hello výstrahy aktivuje.
7. Pokud chcete další e-mailů tooreceive oznámení, když hello výstrahy je aktivována, je přidat v hello **email(s) další správce** pole. Oddělte více e-mailů středníky -  *email@contoso.com;email2@contoso.com*
8. Umístit do platný identifikátor URI v hello **Webhooku** pole Pokud se má volat při hello výstrahy aktivuje.
9. Vyberte **OK** při done toocreate hello výstrahy.   

Během několika minut hello výstraha je aktivní a jak se popisuje výš se aktivuje.

## <a name="managing-your-alerts"></a>Správa upozornění
Po vytvoření výstrahy, můžete ji vybrat a:

* Zobrazte graf zobrazující hello metriky prahovou hodnotu a hello skutečnými hodnotami z hello předchozí den.
* Upravit nebo odstranit.
* **Zakázat** nebo **povolit** ho chcete-li zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy.


## <a name="sql-database-alert-values"></a>Hodnoty výstrah databáze SQL

| Typ prostředku | Název metriky | Popisný název | Typ agregace | Minimální výstrahy časový interval|
| --- | --- | --- | --- | --- |
| Databáze SQL | cpu_percent | Procento CPU | Průměr | 5 minut |
| Databáze SQL | physical_data_read_percent | Procento datových V/V | Průměr | 5 minut |
| Databáze SQL | log_write_percent | Procento vstupně-výstupní operace protokolu | Průměr | 5 minut |
| Databáze SQL | dtu_consumption_percent | Procento DTU | Průměr | 5 minut |
| Databáze SQL | Úložiště | Velikost celkový databáze | Maximální počet | 30 minut |
| Databáze SQL | connection_successful | Úspěšné připojení | Celkem | 10 minut |
| Databáze SQL | connection_failed | Neúspěšné připojení | Celkem | 10 minut |
| Databáze SQL | blocked_by_firewall | Blokováno bránou Firewall | Celkem | 10 minut |
| Databáze SQL | zablokování | Zablokování | Celkem | 10 minut |
| Databáze SQL | storage_percent | Procento velikosti databáze | Maximální počet | 30 minut |
| Databáze SQL | xtp_storage_percent | Percent(Preview) úložiště OLTP v paměti | Průměr | 5 minut |
| Databáze SQL | workers_percent | Procento pracovních procesů | Průměr | 5 minut |
| Databáze SQL | sessions_percent | Procento relací | Průměr | 5 minut |
| Databáze SQL | dtu_limit | Omezení jednotek DTU | Průměr | 5 minut |
| Databáze SQL | dtu_used | Použít DTU | Průměr | 5 minut |
||||||
| Elastický fond | cpu_percent | Procento CPU | Průměr | 10 minut |
| Elastický fond | physical_data_read_percent | Procento datových V/V | Průměr | 10 minut |
| Elastický fond | log_write_percent | Procento vstupně-výstupní operace protokolu | Průměr | 10 minut |
| Elastický fond | dtu_consumption_percent | Procento DTU | Průměr | 10 minut |
| Elastický fond | storage_percent | Procento úložiště | Průměr | 10 minut |
| Elastický fond | workers_percent | Procento pracovních procesů | Průměr | 10 minut |
| Elastický fond | eDTU_limit | omezení eDTU | Průměr | 10 minut |
| Elastický fond | storage_limit | Limit úložiště | Průměr | 10 minut |
| Elastický fond | eDTU_used | eDTU použít | Průměr | 10 minut |
| Elastický fond | storage_used | Využité úložiště | Průměr | 10 minut |
||||||               
| Datový sklad SQL | cpu_percent | Procento CPU | Průměr | 10 minut |
| Datový sklad SQL | physical_data_read_percent | Procento datových V/V | Průměr | 10 minut |
| Datový sklad SQL | Úložiště | Velikost celkový databáze | Maximální počet | 10 minut |
| Datový sklad SQL | connection_successful | Úspěšné připojení | Celkem | 10 minut |
| Datový sklad SQL | connection_failed | Neúspěšné připojení | Celkem | 10 minut |
| Datový sklad SQL | blocked_by_firewall | Blokováno bránou Firewall | Celkem | 10 minut |
| Datový sklad SQL | service_level_objective | Cíl na úrovni služby databáze hello | Celkem | 10 minut |
| Datový sklad SQL | dwu_limit | dwu limit | Maximální počet | 10 minut |
| Datový sklad SQL | dwu_consumption_percent | Procento DWU | Průměr | 10 minut |
| Datový sklad SQL | dwu_used | DWU použít | Průměr | 10 minut |
||||||


## <a name="next-steps"></a>Další kroky
* [Získat přehled o Azure monitorování](../monitoring-and-diagnostics/monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.
* Další informace o [konfigurace webhooky ve výstrahách](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Získat [přehled diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a shromažďovat podrobné metriky vysoká frekvence vaší služby.
* Získat [přehled metriky kolekce](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
