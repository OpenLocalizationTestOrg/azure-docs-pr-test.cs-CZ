---
title: "Portál Azure: vytvoření a správa elastického fondu SQL Database | Microsoft Docs"
description: "Zjistěte, jak toouse hello portál Azure a SQL Database vestavěné inteligentní toomanage, sledování a optimální velikost výkon databáze toooptimize škálovatelné elastický fond a spravovat náklady."
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
ms.openlocfilehash: e0de952bc0c91177f64c04363630783d72435741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-hello-azure-portal"></a>Vytvoření a správa fondu elastické databáze s hello portálu Azure
Toto téma ukazuje, jak toocreate a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) s hello portálu Azure. Můžete také vytvořit a spravovat Azure elastický fond se [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md). Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

## <a name="create-an-elastic-pool"></a>Vytvoření elastického fondu 

Elastický fond můžete vytvořit dvěma způsoby. Můžete provést od začátku Pokud znáte hello fondu instalaci nebo spuštění s doporučením ze služby hello. SQL Database má vestavěné inteligentní, která doporučuje instalační program elastického fondu, pokud je cenově výhodnější založené na hello po telemetrii využití databází.

Můžete vytvořit více fondů na serveru, ale nemůžete přidat databáze z různých serverů do hello stejného fondu. 

> [!NOTE]
> Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview.  Verze GA pro elastické fondy se v této oblasti objeví co nejdřív.
>

### <a name="step-1-create-an-elastic-pool"></a>Krok 1: Vytvoření fondu elastické databáze

Vytvoření fondu elastické databáze z existující **server** okna portálu hello je hello nejjednodušší způsob, jak toomove existující databáze do pružného fondu.

> [!NOTE]
> Můžete také vytvořit fondu elastické databáze vyhledávání **elastický fond SQL** v hello **Marketplace** nebo kliknutím na tlačítko **+ přidat** v hello **SQL elastické fondy**Procházet okna. Budete mít toospecify nového nebo existujícího serveru přes tento fond zřizování pracovního postupu.
>
>

1. V hello [portál Azure](http://portal.azure.com/), klikněte na tlačítko **další služby**  **>**  **servery SQL**a potom klikněte na hello serveru, který obsahuje hello databáze má tooadd tooan elastického fondu.
2. Klikněte na **Nový fond**.

    ![Přidání serveru tooa fondu](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **- nebo -**

    Může se zobrazit zpráva, že jsou doporučené elastické fondy pro hello server. Klikněte na tlačítko hello zpráva toosee hello doporučuje vytvoření fondů založená na historické telemetrii využití databází, klikněte na tlačítko hello vrstvy toosee další podrobnosti a přizpůsobit hello fondu. V tématu [vysvětlení doporučení k fondům](#understand-elastic-pool-recommendations) dál v tomto tématu pro jak se provádí doporučení hello.

    ![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. Hello **elastický fond** se zobrazí okno, což je, kde zadáte hello nastavení fondu. Pokud jste klikli na **nový fond** v předchozím kroku hello hello cenová úroveň je **standardní** je vybrána ve výchozím nastavení a žádné databáze. Můžete vytvořit fond prázdný, nebo zadejte existující databáze z tohoto serveru toomove do fondu hello. Pokud vytváříte doporučený fond, hello doporučená cenovou úroveň, nastavení výkonu a jsou předem seznam databází, ale můžete je stále změnit.

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Zadejte název pro hello elastického fondu nebo ponechte výchozí hello.

### <a name="step-2-choose-a-pricing-tier"></a>Krok 2: Zvolte cenovou úroveň

Hello cenová úroveň fondu určuje hello funkce dostupné toohello elastics v hello fondu a hello maximální počet jednotek Edtu (eDTU MAX) a úložiště (GB) k dispozici tooeach databáze. Podrobnosti viz Úrovně služeb

Klikněte na tlačítko toochange hello cenovou úroveň pro fond hello **cenová úroveň**, klikněte na tlačítko hello cenové úrovně a pak klikněte na tlačítko **vyberte**.

> [!IMPORTANT]
> Pokud zvolíte hello cenová úroveň a potvrdíte svoji volbu kliknutím na **OK** v posledním kroku hello, nebude možné toochange hello cenovou úroveň fondu hello. toochange hello cenovou úroveň existujícího elastického fondu, vytvoření fondu elastické databáze v hello požadovanou cenovou úrovní a migrace hello databáze toothis nový fond.
>

![Výběr cenové úrovně](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-hello-pool"></a>Krok 3: Konfigurace fondu hello

Po nastavení hello cenové úrovně, klikněte na tlačítko Konfigurovat fond přidat databáze, sada fond hodnoty Edtu a úložiště (GB) a nastavíte hello min a max Edtu pro hello elastics ve fondu hello.

1. Klikněte na **Konfigurovat fond**
2. Vyberte databáze hello chcete tooadd toohello fondu. Tento krok je volitelný při vytváření fondu hello. Databáze lze přidat po vytvoření fondu hello.
    Klikněte na tlačítko tooadd databáze, **přidat databázi**, klikněte na tlačítko hello databáze má tooadd a pak klikněte na tlačítko hello **vyberte** tlačítko.

    ![Přidání databází](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Pokud pracujete s databází hello mají dostatek historických telemetrických dat, hello **Odhadované využití eDTU a GB** grafu a hello **skutečné využití eDTU** toohelp aktualizace pruhový graf provedete konfiguraci rozhodnutí. Navíc služba hello může zobrazovat toohelp zprávy doporučení je optimální velikost fondu hello. Viz část [Dynamická doporučení](#understand-elastic-pool-recommendations).

3. Pomocí ovládacích prvků hello na hello **konfigurace fondu** stránka tooexplore nastavení a konfigurovat fond. V tématu [omezení Elastických fondů](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) další podrobnosti o omezeních pro jednotlivé úrovně služby a najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md) podrobné pokyny k optimalizaci velikosti fondu elastické databáze. Další informace o nastavení fondu najdete v tématu [elastického fondu vlastnosti](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Klikněte na tlačítko **vyberte** v hello **konfigurace fondu** okno po změně nastavení.
5. Klikněte na tlačítko **OK** toocreate hello fondu.

## <a name="understand-elastic-pool-recommendations"></a>Vysvětlení doporučení k elastického fondu

Hello služba SQL Database vyhodnocuje historii využití a doporučí jeden nebo více fondů, pokud bude cenově výhodnější než použití izolované databáze. Každé doporučení je konfigurováno pro jedinečnou podmnožinu databází serveru hello, které co nejlépe vyhovovaly hello fondu.

![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Hello doporučení fondu zahrnuje:

- Cenovou úroveň pro hello fond (Basic, Standard, Premium nebo Premium RS)
- vhodnou hodnotu **POOL eDTU** (označovanou také jako Max eDTU pro fond),
- Hello **eDTU MAX** a **eDTU Min** na databázi
- Hello seznam doporučených databází pro fond hello

> [!IMPORTANT]
> Služba Hello bere hello posledních 30 dnech telemetrie v úvahu při doporučujeme fondy. Pro databáze toobe, který zařazena mezi doporučené fondu elastické databáze musí existovat alespoň 7 dní. Databáze, které již v elastickém fondu jsou, se nepovažují za kandidáty pro doporučení elastického fondu.
>

Hello služba vyhodnocuje potřebné prostředky a nákladů plynoucí z přesunutí hello jedné databáze na jednotlivých úrovních služby do fondů hello stejné úrovně. Například pro všechny databáze na serveru, které jsou v úrovni Standard, se zvažuje výhodnost jejich přesunu do elastického fondu také s úrovní Standard. To znamená, že služba hello neprovádí vrstvě mezi úrovněmi, například přesun standardní databáze ve fondu Premium.

Po přidání databází toohello fondu, doporučení se dynamicky vygeneruje, na základě historie využití hello hello databází, které jste vybrali. Tato doporučení jsou zobrazeny v hello eDTU a GB využití graf a v hlavičce doporučení hello horní části hello **konfigurace fondu** okno. Tato doporučení jsou určený tooassist můžete při vytváření fondu elastické databáze optimalizované pro vaše konkrétní databáze.

![dynamická doporučení](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a>Správa a sledování fondu elastické databáze

Můžete použít hello Azure portálu toomonitor a správa fondu elastické databáze a hello databáze ve fondu hello. Z portálu hello můžete sledovat využití hello fondu elastické databáze a databáze hello v tomto fondu. Můžete také nastavit sadu změn tooyour elastický fond a odeslat všechny změny v hello stejný čas. Tyto změny zahrnují přidávání nebo odebírání databáze, změna nastavení služby elastického fondu nebo změna nastavení databáze.

Hello následující obrázek znázorňuje příklad elastickém fondu. zobrazení Hello zahrnuje:

*  Grafy pro sledování využití prostředků hello elastického fondu a databáze hello obsažené ve fondu hello.
*  Hello **konfigurace** fondu tlačítko toomake změny toohello elastického fondu.
*  Hello **vytvořit databázi** tlačítko, která vytváří databázi a přidává ji toohello aktuální elastického fondu.
*  Elastické úlohy, které vám pomůžou spravovat velké počty databází pomocí spouštění skriptů Transact SQL pro všechny databáze v seznamu.

![Zobrazení fondu][2]

Můžete přejít tooa konkrétní fond toosee jeho využití prostředků. Ve výchozím nastavení hello fond je nakonfigurované tooshow využití úložiště a eDTU pro hello poslední hodinu. Hello graf může být nakonfigurované tooshow jiné metriky v různých časových oken.

1. Vyberte toowork elastický fond s.
2. V části **elastického fondu monitorování** je graf s názvem bez přípony **využití prostředků**. Klikněte na tlačítko hello grafu.

    ![Monitorování elastického fondu][3]

    Hello **metrika** otevře se okno, zobrazuje podrobný přehled o hello zadat metriky přes hello zadaný časový interval.   

    ![Okno Metrika][9]

### <a name="toocustomize-hello-chart-display"></a>zobrazení grafu toocustomize hello

Graf hello a hello metriky okno toodisplay můžete upravit jiné metriky jako procento, procento vstupů/výstupů dat a protokolu vstupně-výstupní operace % využití procesoru.

1. V okně metriky hello, klikněte na tlačítko **upravit**.

    ![Klikněte na tlačítko Upravit][6]

2. V hello **upravit graf** okně vyberte časový interval (po hodině, ještě dnes, nebo za minulý týden), nebo klikněte na **vlastní** tooselect libovolné datum v rozsahu v hello poslední dva týdny. Vyberte typ grafu hello (pruhu nebo čáry) a potom vyberte toomonitor prostředky hello.

   > [!Note]
   > Pouze metriky s hello tutéž jednotku lze zobrazit v hello grafu v hello stejný čas. Například pokud zvolíte možnost "eDTU procento" pak můžete zvolit pouze další metriky s procentem jako měrné jednotky hello.
   >

    ![Klikněte na tlačítko Upravit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. Pak klikněte na **OK**.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Spravovat a monitorovat databází v elastickém fondu

Jednotlivé databáze je možné monitorovat také pro potenciální problémy.

1. V části **elastické databáze monitorování**, je graf, který zobrazí metriky pro pět databáze. Ve výchozím nastavení, hello graf zobrazuje hello top 5 databáze ve fondu hello podle využití eDTU průměrná v hello poslední hodinu. Klikněte na tlačítko hello grafu.

    ![Monitorování elastického fondu][4]

2. Hello **využití prostředků databáze** otevře se okno. To poskytuje podrobný přehled o využití hello databáze ve fondu hello. Pomocí hello mřížky v dolní části okna hello hello, můžete vybrat všechny databáze ve fondu toodisplay hello jeho použití v grafu hello (až too5 databáze). Můžete také upravit hello metriky a časového okna zobrazí v grafu hello kliknutím **upravit graf**.

    ![Okna využití prostředků databáze][8]

### <a name="toocustomize-hello-view"></a>zobrazení toocustomize hello

1. V hello **databáze využití prostředků** okně klikněte na tlačítko **upravit graf**.

    ![Klikněte na tlačítko Upravit graf](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. V hello **upravit** grafu okně, vyberte časové rozmezí (za hodinu nebo za posledních 24 hodin) nebo klikněte na **vlastní** tooselect různých denně v hello po toodisplay 2 týdny.

    ![Klikněte na tlačítko Vlastní](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klikněte na tlačítko hello **porovnat databází tak, že** rozevírací tooselect jiné metriky toouse při porovnávání databáze.

    ![Upravit graf hello](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a>toomonitor tooselect databáze

V seznamu hello databáze v hello **využití prostředků databáze** okno, můžete vyhledat konkrétní databáze hello stránky v seznamu hello zaměřením nebo zadáním v hello název databáze. Použijte hello políčko tooselect hello databázi.

![Vyhledejte toomonitor databáze][7]


## <a name="add-an-alert-tooan-elastic-pool-resource"></a>Přidat prostředek výstrahy tooan elastického fondu

Můžete přidat tooan pravidla elastické se fond, který toopeople nebo výstrahu tooURL koncových bodů řetězce odeslání e-mailu, pokud se elastický fond hello dotkne prahovou hodnotu využití, které jste nastavili.

**tooadd prostředek tooany výstrahy:**

1. Klikněte na tlačítko hello **využití prostředků** grafu tooopen hello **metrika** okně klikněte na tlačítko **přidat upozornění**a potom vyplňte informace hello v hello **přidat oznámení pravidlo** okno (**prostředků** je automaticky nastavení fondu hello toobe pracujete s).
2. Zadejte **název** a **popis** identifikující tooyou hello výstrah a příjemce hello.
3. Vyberte **metrika** , které chcete tooalert hello seznamu.

    Graf Hello dynamicky znázorňuje využití prostředků pro tuto metriky toohelp, že si vyberete prahovou hodnotu.

4. Vyberte **podmínku** (větší než menší než atd) a **prahová hodnota**.
5. Vyberte **období** času, který hello metrika pravidlo je nutné splnit před hello výstrahy aktivační události.
6. Klikněte na **OK**.

Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).

## <a name="move-a-database-into-an-elastic-pool"></a>Přesun databáze do pružného fondu

Můžete přidat nebo odebrat databáze z existující fond. Hello databáze může být v jiných fondech. Však lze přidat pouze databáze, které jsou na hello stejný logický server.

1. V okně hello hello fondu v části **elastické databáze** klikněte na tlačítko **konfigurace fondu**.

    ![Klikněte na tlačítko Konfigurovat fond][1]

2. V hello **konfigurace fondu** okně klikněte na tlačítko **přidat toopool**.

    ![Klikněte na tlačítko Přidat toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. V hello **přidat databáze** okně, vyberte hello databáze nebo databáze tooadd toohello fondu. Pak klikněte na tlačítko **vyberte**.

    ![Vyberte tooadd databáze](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Hello **konfigurace fondu** okno teď seznamy hello databáze, které jste vybrali toobe přidali, se její stav nastavit příliš**čekající**.

    ![Přidání čekající fondu](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. V hello **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Přesunutí databáze z fondu elastické databáze

1. V hello **konfigurace fondu** okně, vyberte hello databáze nebo databáze tooremove.

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klikněte na tlačítko **odebrat z fondu**.

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Hello **konfigurace fondu** okno teď seznamy hello databáze, které jste vybrali toobe odebrány s její stav nastavit příliš**čekající**.

    ![Náhled databáze přidávání a odebírání](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. V hello **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Změňte nastavení výkonu elastického fondu

Při sledování využití prostředků hello elastického fondu, může se stát, že některé změny jsou potřeba. Možná hello fondu musí změnu omezení hello výkon nebo úložiště. Pravděpodobně budete chtít toochange hello nastavení databáze ve fondu hello. Můžete změnit nastavení hello hello fondu na všechny čas tooget hello nejlepší kompromis výkonu a nákladů. V tématu [při fondu elastické databáze slouží?](sql-database-elastic-pool.md) Další informace.

toochange hello Edtu nebo úložiště, omezuje na fond a Edtu na databázi:

1. Otevřete hello **konfigurace fondu** okno.

    V části **elastického fondu nastavení**, použijte buď nastavení fondu hello toochange posuvníku.

    ![Využití elastického fondu prostředků](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Při změně nastavení hello zobrazuje zobrazení hello hello odhadované měsíční náklady na změny hello.

    ![Aktualizace služby elastického fondu a měsíční náklady na nový](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>Latence operací elastického fondu
* Změna hello minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.
* Změna hello Edtu na fond závisí na celkovém množství místo využité všechny databáze ve fondu hello hello. Změny průměrně trvají méně než 90 minut na každých 100 GB. Například pokud celkové místo hello používá všechny databáze ve fondu hello je 200 GB, pak hello očekává, že latence pro změnu eDTU fondu hello na fondu je 3 hodiny nebo méně.

## <a name="next-steps"></a>Další kroky

- jaké fondu elastické databáze je, najdete v části toounderstand [elastického fondu SQL Database](sql-database-elastic-pool.md).
- Pokyny k používání elastické fondy najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md).
- toouse elastické úlohy toorun Transact-SQL skriptů pro libovolný počet databází ve fondu hello najdete [elastické úlohy přehled](sql-database-elastic-jobs-overview.md).
- najdete v části tooquery napříč libovolný počet databází ve fondu hello [elastické dotazu přehled](sql-database-elastic-query-overview.md).
- Transakce libovolný počet databází ve fondu hello, najdete v části [elastické transakce](sql-database-elastic-transactions-overview.md).


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
