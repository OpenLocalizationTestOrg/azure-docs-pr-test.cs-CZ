---
title: "službu s vysokou dostupností aaaDesign používání Azure SQL Database | Microsoft Docs"
description: "Další informace o návrhu aplikace pro používání Azure SQL Database služeb s vysokou dostupností."
keywords: "cloud zotavení po havárii, řešení zotavení po havárii, zálohování dat aplikace, geografická replikace, obchodní kontinuity plánování"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: e8a346ac-dd08-41e7-9685-46cebca04582
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 04/21/2017
ms.author: sashan
ms.openlocfilehash: 815f754ba7014cd8a1108a2d84c2a8f71d7030a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Navrhování služeb s vysokou dostupností pomocí Azure SQL Database

Při vytváření a nasazování služeb s vysokou dostupností v databázi SQL Azure, je použít [převzetí služeb při selhání skupiny a aktivní geografickou replikací](sql-database-geo-replication-overview.md) selhání tooregional tooprovide odolnost a závažné výpadky a povolit rychlé obnovení toohello sekundární databáze. Tento článek se zaměřuje na běžných vzorů aplikace a popisuje výhody hello a kompromis jednotlivých možností v závislosti na požadavcích nasazení hello aplikace, hello smlouvu o úrovni služeb cílíte, provoz latenci a náklady. Informace o aktivní geografickou replikaci s elastické fondy najdete v tématu [strategie zotavení po havárii elastický fond](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Vzor návrhu 1: nasazení aktivní – pasivní pro zotavení po havárii cloudu s společně umístěné databáze
Tato možnost je nejvhodnější pro aplikace s hello následující vlastnosti:

* Aktivní instance v jedné oblasti Azure
* Silné závislost na toodata přístup pro čtení a zápis (RW)
* Připojení mezi oblastmi mezi hello webové aplikace a hello databáze není platný kvůli toolatency a provoz náklady    

V takovém případě topologie nasazení aplikace hello je optimalizovaná pro zpracování místní havárie, při dopad na všechny součásti aplikace a potřebujete toofailover jako jednotku. Geografická redundance hello aplikační logiku a hello databáze jsou replikované tooanother oblasti, ale nejsou použity pro zatížení aplikace hello za normálních podmínek hello. aplikace Hello v sekundární oblasti hello by měl být nakonfigurovaný toouse připojovací řetězec toohello sekundární databáze SQL. Správce provozu je nastavený toouse [metody směrování převzetí služeb při selhání](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [!NOTE]
> [Azure traffic Manageru](../traffic-manager/traffic-manager-overview.md) se používá v tomto článku obrázek pouze pro účely. Můžete použít jakéhokoli řešení vyrovnávání zatížení, které podporuje metodu směrování převzetí služeb při selhání.    
>

Hello následující diagram znázorňuje tuto konfiguraci před výpadku.

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Po výpadku v primární oblasti hello služby SQL Database hello zjišťuje, tuto primární databázi hello není dostupný a aktivovat převzetí služeb při selhání toohello sekundární databáze na základě parametrů hello zásad hello automatické převzetí služeb při selhání. V závislosti na vaší smlouvě SLA aplikace můžete rozhodnout, tooconfigure a období odkladu mezi detekce hello hello výpadku a hello převzetí služeb při selhání sám sebe. Konfigurace období odkladu snižuje riziko hello data ztrátě toohello případy, kdy je závažné hello výpadku a dostupnosti v oblasti hello nelze obnovit rychle. Pokud převzetí služeb při selhání hello koncový bod je iniciovaná hello traffic Manageru před hello převzetí služeb při selhání skupiny aktivace hello převzetí služeb při selhání databáze hello, není možné tooreconnect toohello databáze hello webové aplikace. Pokus o tooreconnect Hello aplikace automaticky úspěšné ihned po dokončení převzetí služeb při selhání hello databáze. 

> [!NOTE]
> tooachieve plně koordinované převzetí služeb při selhání aplikace hello a hello databáze, doporučujeme navrhnout vlastní metoda monitorování a použít ruční převzetí služeb při selhání koncových bodů hello webové aplikace a hello databází.
>

Po dokončení převzetí služeb při selhání hello koncových bodů aplikace hello i hello databázi aplikace hello znovu začít zpracovávat požadavky uživatelů hello v oblasti hello B a zůstane umístěné společně se službou hello databáze, protože hello primární databáze je nyní v oblast B. Tento scénář je znázorněn v následujícím diagramu hello. Ve všech diagramech plné čáry znamenat aktivních připojení, tečkovaná čáry označují pozastavenou připojení a zastavení přihlásí znamenat akce aktivační události.

![Geografická replikace: Databáze toosecondary převzetí služeb při selhání. Zálohování dat aplikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

V případě výpadku v sekundární oblasti hello hello odkaz replikace mezi hello primární a sekundární databáze hello je pozastaven, ale hello převzetí služeb při selhání není aktivována, protože není ovlivněná hello primární databáze. dostupnost aplikace Hello není v tomto případě změnila, ale aplikace hello funguje zveřejněné a proto vyšší riziko v případě obou oblastí nezdaří za sebou.

> [!NOTE]
> Pro zotavení po havárii doporučujeme hello konfigurace s oblastí tootwo omezené nasazení aplikace. Důvodem je, že většina z hello zeměpisných oblastí Azure mají pouze dvou oblastí. Tato konfigurace nebude chránit vaše aplikace v případě selhání souběžných závažné obou oblastí.  V nepravděpodobnému takové selhání, můžete obnovit vaše databáze v třetí oblast pomocí [geografické obnovení operace](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

Po výpadku hello zmírnit, hello sekundární databáze bude automaticky znovu synchronizovat s hello primární. Během synchronizace může mít výkon hello primární mírně dopad v závislosti na hello množství dat, který potřebuje toobe synchronizované. Hello následující diagram znázorňuje výpadku v sekundární oblasti hello.

![Sekundární databáze synchronizována s primární. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

klíč Hello **výhody** tohoto vzoru návrhu jsou:

* Hello stejné webové aplikace je nasazená tooboth oblastí bez jakékoli konfigurace specifické pro oblast a žádná další logiku tooreact toohello převzetí služeb při selhání. 
* výkon aplikace Hello nebude ovlivněné převzetí služeb při selhání jako hello webové aplikace a databáze hello jsou vždy společně umístěné.

Hello hlavní **kompromis** je, že aplikace hello redundantní instance v sekundární oblasti hello se používá pouze pro zotavení po havárii.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Vzor návrhu 2: nasazení aktivní aktivní pro vyrovnávání zatížení aplikace
Tato možnost obnovení po havárii cloudu je nejvhodnější pro aplikace s hello následující vlastnosti:

* Vysoký poměr databáze čte toowrites
* Databáze latence čtení je pro činnost koncového uživatele hello větší význam než latence zápisu u hello 
* Jen pro čtení logiky je možné oddělit z logiku pro čtení a zápis pomocí různých připojovací řetězec
* Jen pro čtení logiku nezávisí na data právě plně synchronizována s nejnovějšími aktualizacemi hello  

Pokud vaše aplikace mají tyto charakteristiky, Vyrovnávání zatížení připojení koncových uživatelů hello napříč více instancí aplikace v různých oblastech podstatně zlepšit hello celkové činnost koncového uživatele. Dva oblastí hello měl by být vybrán jako hello pár zotavení po Havárii a hello převzetí služeb při selhání skupiny by měla obsahovat hello databáze v těchto oblastech. tooimplement Vyrovnávání zatížení, každou oblast, musí mít aktivní instance aplikace hello hello pro čtení a zápis (RW) logiku připojené toohello naslouchací proces pro čtení a zápis koncový bod hello převzetí služeb při selhání skupiny. Zaručí, že převzetí hello bude automaticky v případě hello primární databáze je ovlivní případný výpadek. Hello jen pro čtení logiky (RO) ve webové aplikaci hello by měl připojit přímo toohello databáze v této oblasti. Správce provozu by měly být nastavené toouse [směrování výkonu](../traffic-manager/traffic-manager-configure-performance-routing-method.md) s [koncový bod monitorování](../traffic-manager/traffic-manager-monitoring.md) povolená pro každou instanci aplikace.

Jako vzor #1 musíte zvážit nasazení podobné monitorování aplikací. Ale na rozdíl od vzoru #1 hello monitorování aplikace nebude zodpovědná za aktivaci hello koncový bod převzetí služeb při selhání.

> [!NOTE]
> Tento vzor používá více než jedné sekundární databáze, pouze hello sekundární v oblasti B se použije pro převzetí služeb při selhání a musí být součástí skupiny hello převzetí služeb při selhání.
>

Správce provozu musí být nakonfigurovaný pro výkon směrování toodirect hello uživatele připojení toohello aplikace instance nejbližší toohello zeměpisné umístění uživatele. Hello následující diagram znázorňuje tuto konfiguraci před výpadku.

![Žádný výpadek: výkonu směrování toonearest aplikace. Geografická replikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Pokud v oblasti hello A je zjištěn výpadek databáze, hello převzetí služeb při selhání skupiny automaticky zahájit převzetí služeb při selhání hello primární databáze v sekundární oblasti A toohello v oblasti B. Hello naslouchací proces pro čtení a zápis koncový bod tooregion B se také automaticky aktualizuje, připojení pro čtení a zápis v hello webová aplikace nebude mít vliv. Správce provozu Hello offline koncový bod hello vyloučí je z hello směrovací tabulky, ale bude pokračovat směrování hello koncového uživatele provoz toohello zbývající online instance. Hello jen pro čtení připojovací řetězce SQL nebude mít vliv na jako budou vždy bod toohello databáze v hello stejné oblasti. 

Hello následující diagram znázorňuje hello novou konfiguraci po převzetí služeb při selhání hello.

![Konfigurace po převzetí služeb při selhání. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

V případě výpadku v jednom z sekundární oblasti hello hello traffic Manageru se automaticky odeberou hello offline koncový bod v této oblasti z hello směrovací tabulky. Hello replikace kanál toohello sekundární databáze v této oblasti bude pozastaveno. Protože zbývající oblasti hello získat další uživatelské provoz v tomto scénáři, bude ovlivněn výkon aplikace hello během výpadku hello. Po výpadku hello zmírnit, hello sekundární databáze v oblasti ovlivněné hello okamžitě synchronizovány se hello primární. Během hello synchronizace výkon hello primární mírně ovlivní v závislosti na hello množství dat, který potřebuje toobe synchronizovány. Hello následující diagram znázorňuje výpadku v oblasti B.

![Výpadek v sekundární oblasti. Cloud zotavení po havárii – geografická replikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

klíč Hello **využít** tohoto návrhu vzor je, že je možné škálovat hello aplikace zatížení napříč více sekundárních tooachieve hello koncového uživatele optimální výkon. Hello **kompromisy** této možnosti jsou:

* Čtení a zápis připojení mezi instancemi aplikace hello a databáze mají různou latenci a náklady
* Během výpadku hello je ovlivněn výkon aplikace

> [!NOTE]
> Podobný postup lze použít toooffload specializuje úlohy, jako například reporting úlohy, nástroje business intelligence nebo zálohování. Obvykle se tyto úlohy spotřebovávat prostředky významné databáze proto doporučujeme vyhradit jeden hello sekundární databáze pro ně hello výkonu úrovně odpovídající toohello předpokládaného zatížení.
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Vzor návrhu 3: nasazení aktivní – pasivní pro uchovávání dat
Tato možnost je nejvhodnější pro aplikace s hello následující vlastnosti:

* Jakoukoli ztrátu dat, je vysoký obchodní riziko. Hello databáze převzetí služeb při selhání můžete použít jako poslední možnost jen v případě výpadku hello je závažné.
* aplikace Hello podporuje jen pro čtení a čtení a zápis režimy operací a můžou fungovat v "režimu jen pro čtení" pro určitou dobu.

V tomto vzoru aplikace hello přepínačů jen tooread režimu spouštění hello připojení pro čtení a zápis, dochází k chybám časového limitu. Hello webové aplikace je nasazená tooboth oblasti a zahrnují koncového bodu připojení toohello naslouchací proces pro čtení a zápis a koncového bodu jen pro čtení naslouchací proces toohello jiné připojení. Hello Traffic manager by měly být nastavené toouse [směrování převzetí služeb při selhání](../traffic-manager/traffic-manager-configure-failover-routing-method.md) s [koncový bod monitorování](../traffic-manager/traffic-manager-monitoring.md) povolené pro koncový bod aplikace hello v každé oblasti.

Hello následující diagram znázorňuje tuto konfiguraci před výpadku.

![Aktivní – pasivní nasazení před převzetí služeb při selhání. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Zjistí-li hello traffic Manageru tooregion selhání připojení A, automaticky nepřepne instanci aplikace na uživatele provoz toohello v oblasti B. Pomocí tohoto vzoru je důležité nastavit období odkladu hello s ztrátě tooa dostatečně vysoká hodnota dat, například 24 hodin. Bude zajištěno, že ztráty dat je zabráníte zmírnit hello výpadku v této době. Když je aktivován hello webové aplikace v oblasti hello B se spustí operace čtení a zápis hello selhání. V tomto okamžiku se musí přepnout toohello režimu jen pro čtení. V tento režim hello požadavky budou automaticky směrované toohello sekundární databáze. V případě závažného selhání hello hello nebude výpadku omezeny v období odkladu hello a převzetí služeb při selhání skupiny hello aktivuje hello převzetí služeb při selhání. Po této hello bude k dispozici naslouchací proces pro čtení a zápis a hello volání tooit přestane selhání. Tento koncept je znázorněn podle hello následující diagram.

![Výpadek: Aplikace v režimu jen pro čtení. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Pokud v období odkladu hello zmírnit hello výpadku v hello primární oblasti, traffic Manageru zjistí hello obnovení připojení v primární oblasti hello a přepínače instanci aplikace na uživatele provoz back toohello v oblasti A. Tuto instanci aplikace obnoví a funguje v režimu pro čtení a zápis pomocí hello primární databáze v oblasti A.

V případě výpadku v oblasti hello B zjistí hello traffic Manageru hello selhání aplikace hello koncový bod v oblasti B a hello převzetí služeb při selhání skupiny přepínačů hello jen pro čtení naslouchací proces tooregion A. Tímto výpadkem neovlivní hello činnost koncového uživatele, ale primární databáze hello zveřejní během výpadku hello. Tento koncept je znázorněn podle hello následující diagram.

![Výpadek: Sekundární databáze. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Po výpadku hello zmírnit, hello sekundární databáze je okamžitě synchronizovány s hello primární a hello jen pro čtení listener je vypnuté back toohello sekundární databáze v oblasti B. Během synchronizace může mít výkon hello primární mírně dopad v závislosti na hello množství dat, který potřebuje toobe synchronizovány.

Tento vzor návrhu má několik **výhody**:

* Ní nedochází ke ztrátě dat při hello dočasných výpadků.
* Výpadek závisí pouze na tom, jak rychle traffic Manageru zjistí hello chybu připojení, které je možné konfigurovat.

Hello **kompromis** je:

* Aplikace musí být schopný toooperate v režimu jen pro čtení.

> [!NOTE]
> V případě výpadku trvalé služeb v oblasti hello ručně aktivovat převzetí služeb při selhání databáze a přijměte ztráty dat hello. aplikace Hello bude funkční v sekundární oblasti hello s databází toohello přístup pro čtení a zápis.
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Plánování kontinuity obchodních: Zvolte návrh aplikace pro zotavení po havárii cloudu
Strategie zotavení po havárii konkrétní cloudové můžete kombinovat nebo rozšířit tyto vzory návrhu toobest plnění hello potřebám vaší aplikace.  Jak už bylo zmíněno dříve, hello strategie, které zvolíte je založena na hello SLA chtít toooffer tooyour zákazníci a hello topologie nasazení aplikace. toohelp Průvodce vaše rozhodnutí, hello následující tabulka porovnává možnosti hello na základě hello odhadované dat výpadku nebo obnovení cíl bodu (RPO) a obnovení odhadovanou dobu (Vložit).

| vzor | PLÁNOVANÝ BOD OBNOVENÍ | VLOŽIT |
|:--- |:--- |:--- |
| Aktivní – pasivní nasazení pro zotavení po havárii s přístupem společně umístěné databáze |Přístup pro čtení a zápis < 5 s |Čas detekce selhání + TTL pro DNS |
| Aktivní aktivní nasazení pro aplikaci služby Vyrovnávání zatížení |Přístup pro čtení a zápis < 5 s |Čas detekce selhání + TTL pro DNS |
| Aktivní – pasivní nasazení pro uchovávání dat |Jen pro čtení. < 5 s | Jen pro čtení = 0 |
||Přístup pro čtení a zápis = nula. | Přístup pro čtení a zápis = čas detekce selhání + období odkladu ztráty dat. |
|||

## <a name="next-steps"></a>Další kroky
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)
* toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba hello](sql-database-recovery-using-backups.md)
* toolearn o rychlejší možnosti obnovení, najdete v části [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)  
* toolearn o použití automatizované zálohování pro archivaci, najdete v části [kopírování databáze](sql-database-copy.md)
* Informace o aktivní geografickou replikaci s elastické fondy najdete v tématu [strategie zotavení po havárii elastický fond](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
