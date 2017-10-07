---
title: "doporučení výkonu aaaApply – Azure SQL Database | Microsoft Docs"
description: "Můžete použít hello Azure portálu toofind výkonu doporučení, které můžete optimalizovat výkon databáze SQL Azure nebo toocorrect některé problém uvedený v vaše úlohy."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 0b2234840fc3254d235db9e7d18f5bc912851c6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-and-apply-performance-recommendations"></a>Najít a použít doporučení výkonu

Můžete použít hello Azure portálu toofind výkonu doporučení, které můžete optimalizovat výkon databáze SQL Azure nebo toocorrect některé problém uvedený v vaše úlohy. **Výkon doporučení** stránka na portálu Azure vám umožňuje toofind hello nejvyšší doporučení podle jejich potenciální vliv. 

## <a name="viewing-recommendations"></a>Zobrazení doporučení

tooview a použít výkonu doporučení, třeba hello správné [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) oprávnění v Azure. **Čtečka**, **Přispěvatel databází SQL** oprávnění jsou požadované tooview doporučení a **vlastníka**, **Přispěvatel databází SQL** jsou požadována oprávnění tooexecute všechny akce; Vytvořit nebo vyřadit indexy a zrušit vytváření indexu.

Použijte následující postup toofind výkonu doporučení na portálu Azure hello:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Přejděte příliš**další služby** > **databází SQL**a vyberte svou databázi.
3. Přejděte příliš**výkonu doporučení** tooview dostupná doporučení pro vybranou databázi hello.

V tabulce hello podobné se toohello, té, kterou vidíte na následující obrázek hello výkonu doporučení jsou shonw:

![Doporučení](./media/sql-database-advisor-portal/recommendations.png)

Doporučení jsou seřazené podle jejich potenciální dopad na výkon do hello následujících kategorií:

| Dopad | Popis |
|:--- |:--- |
| Vysoký |Vysoký dopad na doporučení by měl poskytovat hello nejvýraznější dopad na výkon. |
| Střednědobé používání |Střední dopad doporučení by měla zvýšit výkon, ale není podstatně. |
| Nízký |Nízký dopad doporučení by měl poskytovat lepší výkon než bez, ale nemusí být výrazné vylepšení. |


> [!NOTE]
> Databáze SQL Azure vyžaduje toomonitor aktivity alespoň za den v pořadí tooidentify několik doporučení. Hello Azure SQL Database můžete snadněji optimalizovat konzistentní dotazu v případě vzorů, než je to možné na náhodných problematického shluky aktivity. Pokud doporučení nejsou k dispozici corrently, hello **výkonu doporučení** stránka obsahuje zprávu s vysvětlením, proč.
> 

Můžete také zobrazit stav hello hello historických operací. Vyberte stav nebo doporučení toosee další podrobnosti.

Tady je příklad "Vytvořit index" doporučení v hello portálu Azure.

![Vytvoření indexu](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Uplatňovat doporučení
Databáze SQL Azure vám dává plnou kontrolu nad jak doporučení jsou povolit pomocí některé z hello následující tři možnosti: 

* Používat jednotlivé doporučení jeden v daném okamžiku.
* Povolit hello automatické ladění tooautomatically platí doporučení.
* tooimplement doporučení ručně, spusťte hello doporučená skriptu T-SQL pro vaši databázi.

Vyberte všechny doporučení tooview jeho podrobnosti a pak klikněte na **zobrazit skript** tooreview hello přesný podrobné informace o tom, jak je vytvoření hello doporučení.

Hello databáze zůstane online, zatímco hello doporučení platí – pomocí výkonu doporučení nebo automatické ladění nikdy převede databázi do režimu offline.

### <a name="apply-an-individual-recommendation"></a>Jednotlivá doporučení použít
Můžete zkontrolovat a přijmout doporučení jeden najednou.

1. Na hello **doporučení** okně vyberte doporučení.
2. Na hello **podrobnosti** okně klikněte na tlačítko **použít** tlačítko.
   
    ![použít doporučení](./media/sql-database-advisor-portal/apply.png)

Vybrané doporučení se použijí v databázi hello.

### <a name="removing-recommendations-from-hello-list"></a>Odebrání doporučení ze seznamu hello
Pokud seznam doporučení obsahuje položky, které chcete tooremove hello seznamu, můžete zrušit hello doporučení:

1. Vyberte v seznamu hello doporučení **doporučení** tooopen hello podrobnosti.
2. Klikněte na tlačítko **zahodit** na hello **podrobnosti** okno.

V případě potřeby můžete přidat vyřazené položky zpět toohello **doporučení** seznamu:

1. Na hello **doporučení** okně klikněte na tlačítko **zobrazení zahozena**.
2. Vyberte položku zrušených ze seznamu tooview hello její podrobnosti.
3. Volitelně klikněte na **vrátit zpět zahodit** tooadd hello index back toohello hlavní seznam **doporučení**.


### <a name="enable-automatic-tuning"></a>Povolení automatického ladění
Hello Azure SQL Database tooimplement doporučení můžete nastavit automaticky. Dostupná doporučení budou automaticky použity. Jako s všechna doporučení spravuje hello služby pokud vlivu na výkon hello je záporný hello doporučení budou vráceny.

1. Na hello **doporučení** okně klikněte na tlačítko **automatické**:
   
    ![Nastavení služby Advisor](./media/sql-database-advisor-portal/settings.png)
2. Vyberte tooautomate akce:
   
    ![Doporučené indexy](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-hello-recommended-t-sql-script"></a>Ručně spustit hello doporučená skriptu T-SQL
Vyberte všechny doporučení a pak klikněte na tlačítko **zobrazit skript**. Spusťte tento skript pro vaši databázi toomanually použít hello doporučení.

*Indexy, které jsou spouštěny ručně nejsou sledovat a ověřit pro vlivu na výkon službou hello* , doporučuje se po vytvoření tooverify sledovat tyto indexy se poskytují zvýšení výkonu a upravit nebo odstranit je nezbytné. Podrobnosti o vytváření indexů najdete v tématu [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Zrušení doporučení
Doporučení, které jsou v **čekající**, **ověření**, nebo **úspěch** stav se dají zrušit. Doporučení se stavem **zpracování** nelze zrušit.

1. Vyberte doporučení v hello **ladění historie** oblasti tooopen hello **podrobnosti doporučení** okno.
2. Klikněte na tlačítko **zrušit** tooabort hello proces použití hello doporučení.

## <a name="monitoring-operations"></a>Monitorování operací
Použití doporučeným nemusí dojít okamžitě. portál Hello poskytuje podrobnosti týkající se stavu hello doporučení. možné stavy, které může být indexu se Hello následující:

| Status | Popis |
|:--- |:--- |
| Čekající na vyřízení |Platí doporučení, příkaz byl přijat a je naplánováno spuštění. |
| Provádění |Hello doporučení se právě používá. |
| Ověření |Bylo úspěšně aplikováno doporučení a hello služby je měření hello výhody. |
| Úspěch |Bylo úspěšně aplikováno doporučení a měří výhody. |
| Chyba |Došlo k chybě během procesu hello použití hello doporučení. Může to být dočasný problém, nebo může být tabulku toohello změnu schématu a skriptu hello již není platný. |
| Návrat |doporučení Hello bylo použito, ale je považována za nenáročných a automaticky zrušeny. |
| Vrátit zpět |Hello doporučení se vrátila. |

Kliknutím doporučení v procesu ze seznamu toosee hello další podrobnosti:

![Doporučené indexy](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Při vrácení doporučení
Pokud jste použili hello výkonu doporučení tooapply hello doporučení (což znamená, že nebyl spuštěn ručně skriptu hello T-SQL) se automaticky vrátí ho Pokud najde hello výkonu dopad toobe záporné. Pokud z nějakého důvodu chcete jednoduše toorevert doporučení můžete provést následující hello:

1. Vyberte úspěšně použité doporučení v hello **ladění historie** oblasti.
2. Klikněte na tlačítko **vrátit** na hello **podrobnosti o doporučení** okno.

![Doporučené indexy](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Monitorování vlivu na výkon doporučení indexu
Po doporučení jsou úspěšně implementováno (v současné době operace indexu a Parametrizace pouze dotazy doporučení) můžete kliknout na **přehled dotazu** na tooopen okno Podrobnosti doporučení hello [dotazu Přehled o výkonu](sql-database-query-performance.md) a zobrazit hello vlivu na výkon nejčastějších dotazů.

![Dopad na výkon monitorování](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Souhrn
Databáze SQL Azure poskytuje doporučení pro zlepšení výkonu databáze SQL. Tím, že poskytuje skriptů T-SQL, a také jednotlivé a plně automaticky, získat užitečné pomoc při optimalizaci vaši databázi a nakonec zlepšení výkonu dotazů.

## <a name="next-steps"></a>Další kroky
Sledovat vaše doporučení a pokračovat tooapply je toorefine výkonu. Databázové úlohy jsou dynamické a průběžně změnu. Databáze SQL Azure bude i nadále toomonitor a poskytovat doporučení, které může zlepšit výkon vaší databáze. 

* V tématu [automatické ladění](sql-database-automatic-tuning.md) hello toolearn Další informace o automatické ladění ve službě Azure SQL Database.
* V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.
* V tématu [Query Performance Insight](sql-database-query-performance.md) toolearn o zobrazení hello vlivu na výkon nejčastějších dotazů.

## <a name="additional-resources"></a>Další zdroje
* [Úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx)
* [VYTVOŘENÍ INDEXU](https://msdn.microsoft.com/library/ms188783.aspx)
* [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md)

