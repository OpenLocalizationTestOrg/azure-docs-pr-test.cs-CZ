---
title: "Návrh službu s vysokou dostupností pomocí Azure SQL Database | Microsoft Docs"
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
ms.openlocfilehash: 40fe0ae04eb94322356ed19773512e3bc383639c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Navrhování služeb s vysokou dostupností pomocí Azure SQL Database

Při vytváření a nasazování služeb s vysokou dostupností v databázi SQL Azure, je použít [převzetí služeb při selhání skupiny a aktivní geografickou replikací](sql-database-geo-replication-overview.md) k poskytování odolnost proti místní chyby a závažné výpadky a povolení rychlého obnovení sekundární databáze. Tento článek se zaměřuje na běžných vzorů aplikace a popisuje výhody a kompromis jednotlivých možností v závislosti na požadavcích nasazení aplikace, smlouvu o úrovni služeb cílíte, provoz latenci a náklady. Informace o aktivní geografickou replikaci s elastické fondy najdete v tématu [strategie zotavení po havárii elastický fond](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Vzor návrhu 1: nasazení aktivní – pasivní pro zotavení po havárii cloudu s společně umístěné databáze
Tato možnost je nejvhodnější pro aplikace s následujícími charakteristikami:

* Aktivní instance v jedné oblasti Azure
* Silné závislost na čtení a zápis (RW) přístup k datům
* Není platný kvůli náklady na latenci a přenosy dat mezi oblastmi připojení mezi webovou aplikací a databáze    

Topologie nasazení aplikace v tomto případě je optimalizovaná pro zpracování místní havárie, při všechny součásti aplikace dopad musí převzetí služeb při selhání jako jednotku. Geografická redundance aplikační logiku a databáze se replikují do jiné oblasti, ale nejsou použity pro zatížení aplikace za normálních podmínek. Aplikace v sekundární oblasti by měl být nakonfigurována pro použití připojovacího řetězce SQL do sekundární databáze. Správce provozu je nastavit tak, aby použít [metody směrování převzetí služeb při selhání](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [!NOTE]
> [Azure traffic Manageru](../traffic-manager/traffic-manager-overview.md) se používá v tomto článku obrázek pouze pro účely. Můžete použít jakéhokoli řešení vyrovnávání zatížení, které podporuje metodu směrování převzetí služeb při selhání.    
>

Následující diagram znázorňuje tuto konfiguraci před výpadku.

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Po výpadku v primární oblasti služba SQL Database zjistí, že primární databáze není dostupný a spustit převzetí služeb při selhání pro sekundární databázi založenou na parametrech zásad automatické převzetí služeb při selhání. V závislosti na vaší smlouvě SLA aplikace můžete rozhodnout nakonfigurovat období odkladu mezi detekce se výpadek a převzetí služeb při selhání, sám sebe. Konfigurace období odkladu snižuje riziko ztráty dat pro případy, kdy se výpadek je závažné a dostupnosti v oblasti není možné obnovit rychle. Pokud převzetí služeb při selhání koncový bod je inicializován nástrojem traffic Manageru před převzetí služeb při selhání skupina aktivuje převzetí služeb při selhání databáze, webová aplikace není možné se připojit k databázi. Aplikace zopakovat pokus o připojení automaticky úspěšné ihned po dokončení převzetí služeb při selhání databázi. 

> [!NOTE]
> K dosažení plně koordinované převzetí služeb při selhání aplikace a databáze, doporučujeme navrhnout vlastní metoda monitorování a použít ruční převzetí služeb při selhání koncových bodů webové aplikace a databáze.
>

Po dokončení převzetí služeb při selhání koncových bodů aplikace a databáze, aplikace bude znovu spusťte zpracování žádostí uživatele v oblasti B a zůstane společně umístěné s databází, protože primární databáze je nyní v oblasti B. Tento scénář je znázorněn v následujícím diagramu. Ve všech diagramech plné čáry znamenat aktivních připojení, tečkovaná čáry označují pozastavenou připojení a zastavení přihlásí znamenat akce aktivační události.

![Geografická replikace: Převzetí služeb při selhání do sekundární databáze. Zálohování dat aplikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

V případě výpadku v sekundární oblasti odkaz replikace mezi primární a sekundární databáze je pozastavená, ale převzetí služeb při selhání není aktivována, protože není ovlivněná primární databáze. Dostupnost aplikace není v tomto případě změnila, ale aplikace funguje zveřejněné a proto vyšší riziko v případě obou oblastí nezdaří za sebou.

> [!NOTE]
> Pro zotavení po havárii doporučujeme konfigurace nasazení aplikace na omezenou dvou oblastí. Je to proto, že většina Azure zeměpisných má jenom dvě oblasti. Tato konfigurace nebude chránit vaše aplikace v případě selhání souběžných závažné obou oblastí.  V nepravděpodobnému takové selhání, můžete obnovit vaše databáze v třetí oblast pomocí [geografické obnovení operace](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

Jakmile se výpadek zmírnit, sekundární databázi se automaticky znovu synchronizovat s primárním serverem. Během synchronizace z hlavních může mít mírně dopad na výkon v závislosti na množství dat, který je potřeba synchronizovat. Následující diagram znázorňuje výpadku v sekundární oblasti.

![Sekundární databáze synchronizována s primární. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

Klíč **výhody** tohoto vzoru návrhu jsou:

* Stejné webové aplikace je nasazený na obou oblastí bez jakékoli konfigurace specifické pro oblast a žádná další logiku reagování převzetí služeb při selhání. 
* Výkon aplikace nebude ovlivněné převzetí služeb při selhání jako webové aplikace a databáze jsou vždy společně umístěné.

Hlavní **kompromis** je, že instance redundantní aplikace v sekundární oblast se používá pouze pro zotavení po havárii.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Vzor návrhu 2: nasazení aktivní aktivní pro vyrovnávání zatížení aplikace
Tato možnost obnovení po havárii cloudu je nejvhodnější pro aplikace s následujícími charakteristikami:

* Vysoký poměr čtení databáze na zápisů
* Databáze latence čtení je pro činnost koncového uživatele větší význam než latence zápisu 
* Jen pro čtení logiky je možné oddělit z logiku pro čtení a zápis pomocí různých připojovací řetězec
* Jen pro čtení logiku nezávisí na plně synchronizován pomocí nejnovější aktualizace dat  

Pokud vaše aplikace tyto charakteristiky, Vyrovnávání zatížení připojení koncových uživatelů napříč více instancí aplikace v různých oblastech podstatně zlepšit celkové činnost koncového uživatele. Dva z oblastí měl by být vybrán jako dvojice zotavení po Havárii a převzetí služeb při selhání skupiny by měla obsahovat databáze v těchto oblastech. Každá oblast implementovat, Vyrovnávání zatížení, musí mít aktivní instance aplikace s logiku pro čtení a zápis (RW) připojení ke koncovému bodu naslouchací proces pro čtení a zápis skupiny převzetí služeb při selhání. Bude zaručit, že převzetí služeb při selhání bude automaticky v případě primární databáze je ovlivní případný výpadek. Jen pro čtení logiku (RO) ve webové aplikaci připojovat přímo k databázi v této oblasti. Správce provozu by měly být nastavené používat [směrování výkonu](../traffic-manager/traffic-manager-configure-performance-routing-method.md) s [koncový bod monitorování](../traffic-manager/traffic-manager-monitoring.md) povolená pro každou instanci aplikace.

Jako vzor #1 musíte zvážit nasazení podobné monitorování aplikací. Ale na rozdíl od vzoru #1 monitorování aplikace nebude zodpovědná za aktivaci převzetí služeb při selhání koncový bod.

> [!NOTE]
> Tento vzor používá více než jedné sekundární databáze, pouze sekundární v oblasti B se použije pro převzetí služeb při selhání a musí být součástí skupiny převzetí služeb při selhání.
>

Správce provozu musí být nakonfigurovaný pro výkon směrování pro přímé připojení uživatele do instance aplikace na nejbližší geografické umístění uživatele. Následující obrázek znázorňuje tuto konfiguraci před výpadku.

![Žádný výpadek: výkonu směrování nejbližší aplikace. Geografická replikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Pokud v oblasti A je zjištěn výpadek databáze, převzetí služeb při selhání skupiny automaticky zahájit převzetí služeb při selhání primární databáze v oblasti A sekundární v oblasti B. Je také automaticky aktualizuje koncový bod naslouchací proces pro čtení a zápis do oblasti B tak připojení pro čtení a zápis ve webové aplikaci nebude mít vliv na. Správce provozu vyloučí offline koncový bod do směrovací tabulky, ale bude pokračovat směrování provozu koncového uživatele na zbývající online instance. Připojovací řetězce SQL jen pro čtení nebude mít vliv na jako vždy ukazovaly na databázi ve stejné oblasti. 

Následující diagram znázorňuje novou konfiguraci po převzetí služeb při selhání.

![Konfigurace po převzetí služeb při selhání. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

V případě výpadku v jednom z sekundární oblasti traffic Manageru se automaticky odeberou offline koncový bod v této oblasti ze směrovací tabulky. Kanál replikace do sekundární databáze v této oblasti bude pozastaveno. Protože zbývající oblasti získat další uživatelské provoz v tomto scénáři, aplikace bude mít dopad na výkon při se výpadek. Jakmile se výpadek zmírnit, budou se sekundární databáze v oblasti ovlivněné okamžitě synchronizovat s primární. Během synchronizace z hlavních může mít mírně dopad na výkon v závislosti na množství dat, který je potřeba synchronizovat. Následující diagram znázorňuje výpadku v oblasti B.

![Výpadek v sekundární oblasti. Cloud zotavení po havárii – geografická replikace.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Klíč **využít** tohoto návrhu vzor je napříč více sekundární databáze k dosažení koncového uživatele optimální výkon, je možné škálovat zatížení aplikace. **Kompromisy** této možnosti jsou:

* Čtení a zápis připojení mezi instancemi aplikace a databáze mají různou latenci a náklady
* Výkon aplikace je ovlivněná během se výpadek

> [!NOTE]
> Podobný postup lze použít k přesměrování zpracování úloh specializované úlohy, jako je například vytváření sestav úlohy, nástroje business intelligence nebo zálohování. Obvykle se tyto úlohy spotřebovávat prostředky významné databáze proto se doporučuje, že určíte jednu sekundární databáze pro ně s úrovní výkonu a očekávané zatížení.
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Vzor návrhu 3: nasazení aktivní – pasivní pro uchovávání dat
Tato možnost je nejvhodnější pro aplikace s následujícími charakteristikami:

* Jakoukoli ztrátu dat, je vysoký obchodní riziko. Převzetí služeb při selhání databázi můžete použít jako poslední možnost jen v případě, že je závažné se výpadek.
* Aplikace podporuje jen pro čtení a čtení a zápis režimy operací a můžou fungovat v "režimu jen pro čtení" pro určitou dobu.

V tomto vzoru přepínačů aplikace do režimu jen pro čtení, když dochází k chybám časového limitu připojení pro čtení a zápis. Webová aplikace je nasazen na obou oblastí a jsou připojení ke koncovému bodu naslouchací proces pro čtení a zápis a jiné připojení ke koncovému bodu naslouchací proces jen pro čtení. Traffic manager by měly být nastavené používat [směrování převzetí služeb při selhání](../traffic-manager/traffic-manager-configure-failover-routing-method.md) s [koncový bod monitorování](../traffic-manager/traffic-manager-monitoring.md) povolené pro koncový bod aplikace v každé oblasti.

Následující obrázek znázorňuje tuto konfiguraci před výpadku.

![Aktivní – pasivní nasazení před převzetí služeb při selhání. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Když správce provozu zjistí selhání připojení oblasti A, automaticky přepíná provozu generovaného uživateli na instanci aplikace v oblasti B. Pomocí tohoto vzoru je důležité nastavit období odkladu ztráty dat. dostatečně vysokou hodnotu, například 24 hodin. Zajistí, aby ztrátě dat se vyloučilo, pokud se výpadek zmírnit v této době. Když je aktivován webové aplikace v oblasti B se spustí operace čtení a zápis selhání. V tomto okamžiku by měl přepnout do režimu jen pro čtení. V tomto režimu budou automaticky směrované požadavky na sekundární databázi. V případě závažného selhání nebude se výpadek omezeny během poskytnuté lhůty a převzetí služeb při selhání skupiny se aktivuje převzetí služeb při selhání. Po který naslouchací proces pro čtení a zápis bude k dispozici a volání k němu zastaví se nedaří. Tento koncept je znázorněn podle následující diagram.

![Výpadek: Aplikace v režimu jen pro čtení. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Pokud se výpadek v primární oblasti zmírnit během poskytnuté lhůty, traffic Manageru zjistí obnovení připojení v primární oblasti a přepíná provozu generovaného uživateli zpět do instance aplikace v oblasti A. Tuto instanci aplikace obnoví a funguje v režimu pro čtení a zápis v databázi primární oblasti A.

V případě výpadku v oblasti B traffic Manageru zjistí selhání koncový bod aplikace v oblasti B a převzetí služeb při selhání naslouchací proces skupiny přepínačů jen pro čtení a oblasti A. Tímto výpadkem nemá negativní vliv na prostředí pro koncové uživatele, ale primární databáze se zveřejní během se výpadek. Tento koncept je znázorněn podle následující diagram.

![Výpadek: Sekundární databáze. Cloud zotavení po havárii.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Jakmile se výpadek zmírnit, sekundární databáze je okamžitě synchronizovány s primárním serverem a je jen pro čtení naslouchací proces přepnout zpět do sekundární databáze v oblasti B. Během synchronizace z hlavních může mít mírně dopad na výkon v závislosti na množství dat, který je potřeba synchronizovat.

Tento vzor návrhu má několik **výhody**:

* Ní nedochází ke ztrátě dat při dočasných výpadků.
* Výpadek závisí pouze na tom, jak rychle traffic Manageru zjistí chybu připojení, které je možné konfigurovat.

**Kompromis** je:

* Aplikace musí být schopen fungují v režimu jen pro čtení.

> [!NOTE]
> V případě výpadku trvalé služeb v oblasti ručně aktivovat převzetí služeb při selhání databáze a přijměte ztrátou dat. Aplikace bude funkční v sekundární oblasti s přístupem pro čtení a zápis do databáze.
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Plánování kontinuity obchodních: Zvolte návrh aplikace pro zotavení po havárii cloudu
Strategie zotavení po havárii konkrétní cloudové můžete kombinovat nebo rozšířit tyto vzory návrhu na nejlépe vyhovovat potřebám vaší aplikace.  Jak už bylo zmíněno dříve, strategie, kterou zvolíte je založena na smlouvě SLA můžete chtít nabízet zákazníkům a topologii nasazení aplikace. Seznamů vaše rozhodnutí, následující tabulka porovnává možnosti založené na odhadované únikem nebo obnovení bodu cíl (RPO) a odhadovaná doba obnovení (Vložit).

| vzor | PLÁNOVANÝ BOD OBNOVENÍ | VLOŽIT |
|:--- |:--- |:--- |
| Aktivní – pasivní nasazení pro zotavení po havárii s přístupem společně umístěné databáze |Přístup pro čtení a zápis < 5 s |Čas detekce selhání + TTL pro DNS |
| Aktivní aktivní nasazení pro aplikaci služby Vyrovnávání zatížení |Přístup pro čtení a zápis < 5 s |Čas detekce selhání + TTL pro DNS |
| Aktivní – pasivní nasazení pro uchovávání dat |Jen pro čtení. < 5 s | Jen pro čtení = 0 |
||Přístup pro čtení a zápis = nula. | Přístup pro čtení a zápis = čas detekce selhání + období odkladu ztráty dat. |
|||

## <a name="next-steps"></a>Další kroky
* Další informace o Azure SQL Database automatizované zálohování najdete v tématu [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)
* Další informace o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba](sql-database-recovery-using-backups.md)
* Další informace o možnosti rychlejší obnovení najdete v tématu [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)  
* Další informace o použití automatizované zálohování pro archivaci, najdete v části [kopírování databáze](sql-database-copy.md)
* Informace o aktivní geografickou replikaci s elastické fondy najdete v tématu [strategie zotavení po havárii elastický fond](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
