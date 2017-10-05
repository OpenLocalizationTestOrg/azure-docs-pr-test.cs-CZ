---
title: "Portál Azure: vytvoření a správa elastického fondu SQL Database | Microsoft Docs"
description: "Zjistěte, jak pomocí portálu Azure a SQL Database vestavěné inteligentní pro správu, monitorování a optimální velikost škálovatelné elastického fondu Správa nákladů a optimalizace výkonu databáze."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 06/06/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 4ffd1db31f42967dc7f07aa979898dddbb333641
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a>Vytvoření a správa fondu elastické databáze pomocí portálu Azure
Toto téma ukazuje, jak vytvořit a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí portálu Azure. Můžete také vytvořit a spravovat Azure elastický fond se [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), rozhraní API REST nebo [C#](sql-database-elastic-pool-manage-csharp.md). Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

## <a name="create-an-elastic-pool"></a>Vytvoření elastického fondu 

Elastický fond můžete vytvořit dvěma způsoby. Pokud znáte cílovou konfiguraci fondu, můžete ho vytvořit od začátku sami, nebo můžete začít s fondem, který vám doporučí služba. SQL Database má vestavěné inteligentní, která doporučuje instalační program elastického fondu, pokud je cenově výhodnější založené na posledních telemetrii využití pro své databáze.

Můžete vytvořit více fondů na serveru, ale nemůžete přidat databáze z různých serverů do stejného fondu. 

> [!NOTE]
> Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview.  Verze GA pro elastické fondy se v této oblasti objeví co nejdřív.
>

### <a name="step-1-create-an-elastic-pool"></a>Krok 1: Vytvoření fondu elastické databáze

Vytvoření fondu elastické databáze z existující **server** okna portálu je nejjednodušší způsob, jak přesunout existující databáze do pružného fondu.

> [!NOTE]
> Můžete také vytvořit fondu elastické databáze vyhledávání **elastický fond SQL** v **Marketplace** nebo kliknutím na tlačítko **+ přidat** v **SQL elastické fondy** Procházet okna. Budete moci zadat nové nebo existující server prostřednictvím tohoto fondu zřizování pracovního postupu.
>
>

1. V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **další služby**  **>**  **servery SQL**a potom klikněte na server obsahující databáze, kterou chcete přidat do fondu elastické databáze.
2. Klikněte na **Nový fond**.

    ![Přidejte fond na server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **- nebo -**

    Může se zobrazit zpráva, že jsou doporučené elastické fondy pro server. Kliknutím na zprávu zobrazíte doporučení k vytvoření fondů založená na historické telemetrii využití databází. Pak kliknutím na úroveň zobrazte další podrobnosti a možnosti přizpůsobení fondu. Podrobnější informace o doporučeních najdete v části [Vysvětlení doporučení k fondům](#understand-elastic-pool-recommendations) dále v tomto tématu.

    ![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. **Elastický fond** se zobrazí okno, což je, kde můžete určit nastavení fondu. Pokud jste klikli na **nový fond** v předchozím kroku, cenová úroveň je **standardní** je vybrána ve výchozím nastavení a žádné databáze. Můžete vytvořit prázdný fond nebo zadat sadu existujících databází z daného serveru, které se mají přesunout do fondu. Pokud vytváříte doporučený fond, předběžně vyplní doporučené cenovou úroveň, nastavení výkonu a seznam databází, ale můžete je stále změnit.

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Zadejte název elastického fondu nebo ponechte výchozí hodnotu.

### <a name="step-2-choose-a-pricing-tier"></a>Krok 2: Zvolte cenovou úroveň

Cenová úroveň fondu určuje funkce, které jsou k dispozici pro elastics ve fondu a maximální počet jednotek Edtu (eDTU MAX) a úložiště (GB) k dispozici pro každou databázi. Podrobnosti viz Úrovně služeb

Chcete-li změnit cenovou úroveň fondu, klikněte na **Cenová úroveň**, vyberte požadovanou cenovou úroveň a klikněte na **Vybrat**.

> [!IMPORTANT]
> Až vyberete cenovou úroveň a potvrdíte svoji volbu kliknutím na **OK** v posledním kroku, nebude už možné cenovou úroveň fondu změnit. Pokud chcete změnit cenovou úroveň existujícího elastického fondu, vytvoření fondu elastické databáze v s požadovanou cenovou úrovní a migrace databáze do tohoto nového fondu.
>

![Výběr cenové úrovně](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a>Krok 3: Konfigurace fondu

Po nastavení cenové úrovně, klikněte na tlačítko Konfigurovat fond přidat databáze, sada fond hodnoty Edtu a úložiště (GB) a nastavíte min a max Edtu pro elastics ve fondu.

1. Klikněte na **Konfigurovat fond**
2. Vyberte databáze, které chcete přidat do fondu. Tento krok je při vytváření fondu volitelný. Databáze můžete přidat i po vytvoření fondu.
    Klikněte na možnost **Přidat databázi**, pak na databáze, které chcete přidat a potom na tlačítko **Vybrat**.

    ![Přidání databází](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Pokud databáze, s kterými pracujete, mají dostatek historických telemetrických dat, graf **Odhadované eDTU a využití GB** a pruhový graf **Skutečné využití eDTU** se aktualizují, aby vám pomohly s rozhodováním o hodnotách konfigurace. Služba vám také může zobrazovat doporučení s cílem nastavit optimální velikost fondu. Viz část [Dynamická doporučení](#understand-elastic-pool-recommendations).

3. Pomocí ovládacích prvků na stránce **Konfigurace fondu** můžete zkontrolovat nastavení a konfigurovat fond. V tématu [omezení Elastických fondů](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) další podrobnosti o omezeních pro jednotlivé úrovně služby a najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md) podrobné pokyny k optimalizaci velikosti fondu elastické databáze. Další informace o nastavení fondu najdete v tématu [elastického fondu vlastnosti](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Po úpravě nastavení v okně **Konfigurace fondu** klikněte na tlačítko **Vybrat**.
5. Kliknutím na tlačítko **OK** vytvořte fond.

## <a name="understand-elastic-pool-recommendations"></a>Vysvětlení doporučení k elastického fondu

Služba SQL Database vyhodnocuje historii využití a doporučí použití jednoho nebo několika fondů, jakmile to začne být cenově výhodnější než použití izolované databáze. Každé doporučení je konfigurováno pro jedinečnou podmnožinu databází serveru, která je pro fond nejvhodnější.

![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Doporučení fondu zahrnuje:

- Cenovou úroveň pro fond (Basic, Standard, Premium nebo Premium RS)
- vhodnou hodnotu **POOL eDTU** (označovanou také jako Max eDTU pro fond),
- hodnoty **eDTU MAX** a **eDTU MIN** pro každou databázi,
- seznam doporučených databází pro fond.

> [!IMPORTANT]
> Při vytváření doporučení bere služba v úvahu telemetrická data za posledních 30 dní. Aby databáze mohla být zařazena mezi doporučené fondu elastické databáze musí existovat alespoň 7 dní. Databáze, které již v elastickém fondu jsou, se nepovažují za kandidáty pro doporučení elastického fondu.
>

Služba vyhodnocuje potřebné prostředky a cenovou výhodnost přesunu jednotlivých databází v každé úrovni služby do fondu ve stejné úrovni. Například pro všechny databáze na serveru, které jsou v úrovni Standard, se zvažuje výhodnost jejich přesunu do elastického fondu také s úrovní Standard. To znamená, že služba nikdy nenavrhne přesun databáze mezi úrovněmi, například přesun databáze s úrovní Standard do fondu s úrovní Premium.

Po přidání databází do fondu, doporučení se dynamicky vygeneruje, na základě historie využití databází, které jste vybrali. Tato doporučení jsou zobrazeny v eDTU a GB využití graf a v hlavičce doporučení v horní části **konfigurace fondu** okno. Tato doporučení jsou určeny k usnadnění vytváření fondu elastické databáze optimalizované pro vaše konkrétní databáze.

![dynamická doporučení](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a>Správa a sledování fondu elastické databáze

Na portálu Azure můžete použít ke sledování a správě fondu elastické databáze a databáze ve fondu. Z portálu můžete sledovat využití fondu elastické databáze a databáze v tomto fondu. Můžete také nastavit sadu změn do pružného fondu a odeslat všechny změny ve stejnou dobu. Tyto změny zahrnují přidávání nebo odebírání databáze, změna nastavení služby elastického fondu nebo změna nastavení databáze.

Následující obrázek znázorňuje příklad elastickém fondu. Zobrazení zahrnuje:

*  Grafy pro sledování využití prostředků elastický fond a databází obsažené ve fondu.
*  **Konfigurace** tlačítka fondu změny do elastického fondu.
*  **Vytvořit databázi** tlačítko, které vytvoří databázi a přidává ji k aktuální elastického fondu.
*  Elastické úlohy, které vám pomůžou spravovat velké počty databází pomocí spouštění skriptů Transact SQL pro všechny databáze v seznamu.

![Zobrazení fondu][2]

Můžete přejít na konkrétní fond zobrazíte jeho využití prostředků. Ve výchozím nastavení je fond konfigurována pro zobrazení využití úložiště a eDTU za poslední hodinu. Graf lze konfigurovat zobrazíte jiné metriky v různých časových oken.

1. Vyberte fondu elastické databáze pro práci s.
2. V části **elastického fondu monitorování** je graf s názvem bez přípony **využití prostředků**. Klikněte na graf.

    ![Monitorování elastického fondu][3]

    **Metrika** otevře se okno, zobrazuje podrobný přehled o metriku zadané v průběhu zadaný časový interval.   

    ![Okno Metrika][9]

### <a name="to-customize-the-chart-display"></a>Chcete-li přizpůsobit zobrazení grafu

Můžete upravit graf a v okně metriky zobrazíte další metriky jako procento, procento vstupů/výstupů dat a protokolu vstupně-výstupní operace % využití procesoru.

1. V okně metriky, klikněte na **upravit**.

    ![Klikněte na tlačítko Upravit][6]

2. V **upravit graf** okně vyberte časový interval (po hodině, ještě dnes, nebo za minulý týden), nebo klikněte na **vlastní** a vybrat libovolnou oblast datum v poslední dva týdny. Vyberte typ grafu (pruhu nebo čáry) a potom vyberte zdroje, které chcete monitorovat.

   > [!Note]
   > Pouze metriky se stejnou jednotkou míry lze zobrazit v grafu ve stejnou dobu. Například pokud zvolíte možnost "eDTU procento" pak můžete zvolit pouze další metriky s procentem jako měrné jednotky.
   >

    ![Klikněte na tlačítko Upravit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. Pak klikněte na **OK**.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Spravovat a monitorovat databází v elastickém fondu

Jednotlivé databáze je možné monitorovat také pro potenciální problémy.

1. V části **elastické databáze monitorování**, je graf, který zobrazí metriky pro pět databáze. Ve výchozím nastavení grafu zobrazí top 5 databáze ve fondu podle využití eDTU průměrná za poslední hodinu. Klikněte na graf.

    ![Monitorování elastického fondu][4]

2. **Využití prostředků databáze** otevře se okno. To poskytuje podrobný přehled o využití databáze ve fondu. Pomocí mřížky v dolní části okna, můžete vybrat všechny databáze ve fondu se mají zobrazit jeho použití v grafu (až 5 databáze). Můžete taky přizpůsobit okno metriky a času zobrazené v grafu kliknutím **upravit graf**.

    ![Okna využití prostředků databáze][8]

### <a name="to-customize-the-view"></a>Chcete-li přizpůsobit zobrazení

1. V **databáze využití prostředků** okně klikněte na tlačítko **upravit graf**.

    ![Klikněte na tlačítko Upravit graf](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. V **upravit** grafu okně, vyberte časové rozmezí (za hodinu nebo za posledních 24 hodin) nebo klikněte na **vlastní** chcete vybrat jiný den v posledních 2 týdny k zobrazení.

    ![Klikněte na tlačítko Vlastní](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klikněte na tlačítko **porovnat databází tak, že** rozevírací seznam a vyberte jiné metriky pro použití při porovnávání databáze.

    ![Upravit graf](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Chcete-li vybrat databáze k monitorování

V seznamu databáze **využití prostředků databáze** okno, můžete vyhledat konkrétní databáze tak, že vyhledá prostřednictvím stránky v seznamu nebo zadáním názvu databáze. Vyberte databázi, použijte zaškrtávací políčko.

![Vyhledejte databáze k monitorování][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a>Přidání oznámení na prostředek elastického fondu

Pravidla můžete přidat do pružného fondu, který odeslání e-mailu na osoby nebo výstrahu řetězce adresy URL koncových bodů, pokud se elastický fond dotkne prahovou hodnotu využití, které jste nastavili.

**Postup přidání upozornění k jakémukoli prostředku:**

1. Klikněte na tlačítko **využití prostředků** graf a otevřete **metrika** okně klikněte na tlačítko **přidat upozornění**a pak zadejte informace do **přidání pravidla výstrahy** okno (**prostředků** je automaticky nastavena si být fondu pracujete s).
2. Zadejte **název** a **popis** identifikující výstrahy pro vás i příjemce.
3. Vyberte **metrika** , kterou chcete výstrahu ze seznamu.

    Graf zobrazuje dynamicky využití prostředků pro tuto metriku, které vám pomohou zvolit prahovou hodnotu.

4. Vyberte **podmínku** (větší než menší než atd) a **prahová hodnota**.
5. Vyberte **období** času, který metriky pravidlo je nutné splnit před výstrahy aktivačních událostí.
6. Klikněte na **OK**.

Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).

## <a name="move-a-database-into-an-elastic-pool"></a>Přesun databáze do pružného fondu

Můžete přidat nebo odebrat databáze z existující fond. Databáze může být v jiných fondech. Ale můžete přidat pouze databáze, které jsou na stejného logického serveru.

1. V okně pro fond v části **elastické databáze** klikněte na tlačítko **konfigurace fondu**.

    ![Klikněte na tlačítko Konfigurovat fond][1]

2. V **konfigurace fondu** okně klikněte na tlačítko **přidat do fondu**.

    ![Klikněte na tlačítko Přidat do fondu](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. V **přidat databáze** okně vyberte databázi nebo databáze pro přidání do fondu. Pak klikněte na tlačítko **vyberte**.

    ![Vyberte databáze pro přidání](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    **Konfigurace fondu** okno teď zobrazuje databáze vybrané pro přidání, s její stav nastavit na **čekající**.

    ![Přidání čekající fondu](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. V **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Přesunutí databáze z fondu elastické databáze

1. V **konfigurace fondu** okně vyberte databázi nebo databáze k odebrání.

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klikněte na tlačítko **odebrat z fondu**.

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    **Konfigurace fondu** okno teď uvádí databázi vybrané k odstranění s její stav nastavit na **čekající**.

    ![Náhled databáze přidávání a odebírání](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. V **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Změňte nastavení výkonu elastického fondu

Při sledování využití prostředků elastického fondu, může se stát, že některé změny jsou potřeba. Možná fondu musí změnu omezení výkon nebo úložiště. Pravděpodobně budete chtít změnit nastavení databáze ve fondu. Můžete změnit nastavení fondu kdykoli získat nejlepší kompromis výkonu a nákladů. V tématu [při fondu elastické databáze slouží?](sql-database-elastic-pool.md) Další informace.

Chcete-li změnit omezení Edtu nebo úložiště na každý fond a Edtu na databázi:

1. Otevřete **konfigurace fondu** okno.

    V části **elastického fondu nastavení**, použijte buď posuvníku Chcete-li změnit nastavení fondu.

    ![Využití elastického fondu prostředků](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Při změně nastavení zobrazení ukazuje odhadované měsíční náklady změny.

    ![Aktualizace služby elastického fondu a měsíční náklady na nový](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>Latence operací elastického fondu
* Změna minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.
* Změna Edtu na fond, závisí na celkovém množství místo využité všechny databáze ve fondu. Změny průměrně trvají méně než 90 minut na každých 100 GB. Například pokud celkové místo používá všechny databáze ve fondu je 200 GB, pak očekávané latence pro změnu fondu eDTU na fond je 3 hodiny nebo méně.

## <a name="next-steps"></a>Další kroky

- Abyste zjistili, co je, najdete v elastickém fondu [elastického fondu SQL Database](sql-database-elastic-pool.md).
- Pokyny k používání elastické fondy najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md).
- Pokud chcete použít ke spuštění skriptů jazyka Transact-SQL pro libovolný počet databází ve fondu elastické úlohy, najdete v části [elastické úlohy přehled](sql-database-elastic-jobs-overview.md).
- K dotazování mezi libovolný počet databází ve fondu, najdete v části [elastické dotazu přehled](sql-database-elastic-query-overview.md).
- Transakce libovolný počet databází ve fondu, najdete v části [elastické transakce](sql-database-elastic-transactions-overview.md).


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
