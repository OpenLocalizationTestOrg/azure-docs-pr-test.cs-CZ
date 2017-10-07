---
title: "upgrady aplikací aaaRolling – Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak toouse geografická replikace databáze SQL Azure toosupport online upgrady cloudové aplikace."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 58f42859-1e37-463c-a3d8-a3ca2e867148
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/16/2016
ms.author: sashan
ms.openlocfilehash: 18c56300916d129bff141624cc5c416b500408d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Správa postupné upgrady cloudových aplikací pomocí SQL Database aktivní geografickou replikaci
> [!NOTE]
> [Aktivní geografickou replikací](sql-database-geo-replication-overview.md) je nyní k dispozici pro všechny databáze na všech úrovních.
> 

Zjistěte, jak toouse [geografická replikace](sql-database-geo-replication-overview.md) v databázi SQL tooenable vrácení upgrady cloudových aplikací. Protože upgrade je rušivý operace, je nutné součást vaší firmy kontinuity plánování a návrhu. V tomto článku, který se podíváme na dvě různé metody Orchestrace hello procesu upgradu a popisují výhody hello a kompromis jednotlivých možností. Pro účely tohoto článku hello použijeme jednoduchou aplikaci, která se skládá z jedné databáze webu připojené tooa jako jeho datové vrstvy. Naším cílem je tooupgrade verze 1 hello aplikace tooversion 2 bez žádný významný dopad na hello činnost koncového uživatele. 

Při vyhodnocování hello možnosti upgradu je třeba zvážit hello následující faktory:

* Dopad na dostupnost aplikace během upgradu. Jak dlouho může být funkce aplikace hello omezené nebo snížení.
* Možnost tooroll zpět v případě selhání upgradu.
* Chyba zabezpečení aplikace hello během upgradu hello dojde k nesouvisejícími závažné chybě.
* Celkový počet dolaru náklady.  To zahrnuje další redundanci a přírůstkové náklady hello dočasné komponenty používané stránkami hello procesu upgradu. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Upgrade aplikací, které jsou závislé na zálohování databází můžete pro zotavení po havárii
Pokud vaše aplikace spoléhá na automatické zálohování databází a používá geografické obnovení pro zotavení po havárii, je obvykle nasazené tooa jedné oblasti Azure. V takovém případě procesu upgradu hello zahrnuje, vytvoření účastnící se hello upgrade zálohování nasazení všech součástí aplikace. toominimize hello přerušení koncového uživatele bude využívat Azure provoz Manager (WATM) s profilem hello převzetí služeb při selhání.  Hello následující diagram znázorňuje proces upgradu předchozí toohello hello provozní prostředí. Hello koncový bod <i>contoso-1.azurewebsites.net</i> představuje produkční slot hello aplikace vyžadující toobe upgradovat. tooenable hello možnost tooroll zpět hello upgrade, musí vytvořit slot fáze s plně synchronizované kopie aplikace hello. Hello následující kroky jsou požadované tooprepare hello aplikace hello upgrade:

1. Vytvořte pro hello upgrade fáze slot. toodo, který vytvořit sekundární databázi (1) a nasadit identické webu v hello stejné oblasti Azure. Monitorování sekundární toosee hello, pokud dokončení synchronizace replik indexů proces hello.
2. Vytvoření profilu převzetí služeb při selhání v WATM s <i>contoso-1.azurewebsites.net</i> jako koncový bod online a <i>contoso-2.azurewebsites.net</i> v režimu offline. 

> [!NOTE]
> Poznámka: hello přípravné kroky nebude mít vliv hello aplikace hello produkční slot a může fungovat v režimu úplný přístup.
>  

![Konfigurace replikace přejít databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Po dokončení hello přípravné kroky hello aplikace je připravena k vlastním upgradem hello. Hello následující diagram znázorňuje hello kroky v procesu upgradu hello. 

1. Nastavit primární databáze hello hello produkčním slotu tooread pouze v režimu (3). Tím se zaručí, že hello produkční instanci aplikace hello (V1) zůstane jen pro čtení během upgradu hello, takže si hello odchylkami dat mezi hello V1 a V2 instance databáze.  
2. Odpojte hello sekundární databáze pomocí hello plánované ukončení režimu (4). Vytvoří kopii plně synchronizované nezávislé hello primární databáze. Tato databáze budou upgradovány.
3. Zapnutí režimu zápisu tooread hello primární databáze a spusťte skript upgradu hello ve slotu fáze hello (5).     

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Pokud hello upgrade úspěšně dokončen jste nyní připraven tooswitch hello koncoví uživatelé toohello připravený kopie hello aplikace. Nyní se stane hello produkční slot aplikace hello.  To zahrnuje několik další kroky, jak je znázorněno na následujícím diagramu hello.

1. Přepnout online koncový bod hello v profilu WATM hello příliš<i>contoso-2.azurewebsites.net</i>, která verze toohello V2 body hello webové stránky (6). Nyní bude hello produkční slot s hello V2 aplikace a provoz hello koncového uživatele je směrovanou tooit.  
2. Pokud již nepotřebujete součásti aplikace hello V1, abyste mohli bezpečně odeberte je (7).   

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Pokud neproběhne hello upgradu, například z důvodu chyby tooan ve skriptu upgradu hello, hello fáze slotu považovat za ohrožení zabezpečení. tooroll zpět hello toohello před upgradem stav aplikace můžete jednoduše vrátit hello aplikace hello produkčním slotu toofull přístup. Hello kroky jsou uvedeny v diagramu další hello.    

1. Nastavit režim tooread zápisu kopírování databáze hello (8). Tato akce obnoví hello úplné V1 funkčně v hello produkční slot.
2. Proveďte analýza hello hlavní příčiny a odebrat součásti hello ohrožení zabezpečení ve slotu fáze hello (9). 

V tomto okamžiku aplikace hello je plně funkční a lze je opakovat postup upgradu hello.

> [!NOTE]
> Hello vrácení nevyžaduje změny v profilu WATM jako již odkazuje příliš<i>contoso-1.azurewebsites.net</i> jako hello aktivní koncový bod.
> 
> 

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

klíč Hello **využít** této možnosti je, že můžete upgradovat aplikaci v jedné oblasti používáte sadu jednoduchých kroků. náklady na dolaru Hello hello upgradu je relativně nízký. Hello hlavní **kompromis** je, pokud dojde k závažné chybě během hello upgradu hello obnovení toohello před upgradem stavu bude zahrnovat opakované nasazení aplikace hello v jiné oblasti a obnovení databáze hello z zálohování pomocí geografické obnovení. Tento proces bude způsobit významné výpadky.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Upgrade aplikací, které jsou závislé na geografické replikace databáze pro zotavení po havárii
Pokud vaše aplikace využívá geografická replikace pro kontinuitu podnikových procesů, je nasazené tooat minimálně dva různé oblasti s aktivní nasazení v primární oblasti a pohotovostní nasazení v oblasti zálohování. Kromě toho toohello faktory již bylo zmíněno dříve, procesu upgradu hello musí zaručit, že:

* aplikace Hello i nadále chráněn před závažné selhání za všech okolností během procesu upgradu hello
* geograficky redundantní komponenty aplikace hello Hello upgradují paralelně s komponentami active hello

tooachieve těchto cílů využije hello převzetí služeb při selhání Azure provoz Manager (WATM) profil s jednu aktivní a tři zálohy koncové body.  Hello následující diagram znázorňuje proces upgradu předchozí toohello hello provozní prostředí. Hello weby <i>contoso-1.azurewebsites.net</i> a <i>contoso-dr.azurewebsites.net</i> představují produkční slot hello aplikace s úplné geografická redundance. tooenable hello možnost tooroll zpět hello upgrade, musí vytvořit slot fáze s plně synchronizované kopie aplikace hello. Vzhledem k tomu, že budete potřebovat tooensure, hello aplikace můžete rychle obnovit v případě, že během procesu upgradu hello dojde k závažné chybě, hello fáze slotu musí toobe geograficky redundantní také. Hello následující kroky jsou požadované tooprepare hello aplikace hello upgrade:

1. Vytvořte pro hello upgrade fáze slot. toodo, který vytvořit sekundární databázi (1) a nasadit identické kopii hello webovou stránku hello stejné oblasti Azure. Monitorování sekundární toosee hello, pokud dokončení synchronizace replik indexů proces hello.
2. Vytvoření geograficky redundantní sekundární databáze ve slotu fáze hello geografickou replikací hello sekundární databáze toohello zálohování oblast (to se označuje jako "zřetězené geografická replikace"). Monitorování zálohování sekundární toosee hello, pokud hello synchronizace replik indexů proces je dokončené (3).
3. Vytvořit kopii pohotovostní hello webové stránky v oblasti zálohování hello a propojit jej toohello geograficky redundantní sekundární (4).  
4. Přidat další koncové body hello <i>contoso-2.azurewebsites.net</i> a <i>contoso-3.azurewebsites.net</i> toohello profil převzetí služeb při selhání v WATM jako offline koncové body (5). 

> [!NOTE]
> Poznámka: hello přípravné kroky nebude mít vliv hello aplikace hello produkční slot a může fungovat v režimu úplný přístup.
> 
> 

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Po dokončení kroků přípravy hello slot fáze hello je připraven k upgradu hello. Hello následující diagram znázorňuje postup upgradu hello.

1. Nastavit primární databáze hello hello produkčním slotu tooread pouze v režimu (6). Tím se zaručí, že hello produkční instanci aplikace hello (V1) zůstane jen pro čtení během upgradu hello, takže si hello odchylkami dat mezi hello V1 a V2 instance databáze.  
2. Odpojit hello sekundární databáze v hello pomocí stejné oblasti hello plánované ukončení režimu (7). Vytvoří kopii plně synchronizované nezávislé hello primární databázi, která se automaticky stane primární po ukončení hello. Tato databáze budou upgradovány.
3. Zapnout hello primární databázi v režimu zápisu tooread slotu fáze hello a spusťte skript pro upgrade hello (8).    

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Pokud hello upgrade úspěšně dokončen jste nyní připraven tooswitch hello koncoví uživatelé toohello V2 verze aplikace hello. Hello následující diagram znázorňuje hello kroky.

1. Přepínač hello aktivní koncový bod v profilu WATM hello příliš<i>contoso-2.azurewebsites.net</i>, které teď body toohello V2 verzi hello webový server (9). Nyní bude produkční slot s hello V2 aplikace a provoz koncového uživatele je řízené tooit. 
2. Pokud již nepotřebujete hello V1 aplikace, abyste mohli bezpečně odeberte ji (10 a 11).  

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Pokud neproběhne hello upgradu, například z důvodu chyby tooan ve skriptu upgradu hello, hello fáze slotu považovat za ohrožení zabezpečení. tooroll zpět hello toohello před upgradem stav aplikace můžete jednoduše vrátit aplikace hello toousing v produkční slot hello s úplným přístupem. Hello kroky jsou uvedeny v diagramu další hello.    

1. Sada hello kopie primární databázi v režimu tooread zápis pozici provozního hello (12). Tato akce obnoví hello úplné V1 funkčně v hello produkční slot.
2. Proveďte analýza hello hlavní příčiny a odebrat součásti hello ohrožení zabezpečení ve fázi slotu hello (13 a 14). 

V tomto okamžiku aplikace hello je plně funkční a lze je opakovat postup upgradu hello.

> [!NOTE]
> Hello vrácení nevyžaduje změny v profilu WATM jako již odkazuje příliš <i>contoso-1.azurewebsites.net</i> jako hello aktivní koncový bod.
> 
> 

![Konfigurace geografická replikace databáze SQL. Cloud zotavení po havárii.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

klíč Hello **využít** této možnosti je, aniž by to ohrozilo vaší kontinuity podnikových procesů během upgradu hello můžete upgradovat hello aplikace a její geograficky redundantní kopie paralelně. Hello hlavní **kompromis** je, že vyžaduje dvojité redundance jednotlivých součástí aplikace a proto způsobuje vyšší dolaru náklady. Zahrnuje také složitější pracovního postupu. 

## <a name="summary"></a>Souhrn
Hello dva upgradu metody popsané v článku hello se liší v složitost a hello dolaru náklady, ale obě soustředit na minimalizovat hello čas, kdy hello koncového uživatele je omezena pouze tooread operace. Tento čas je přímo definované hello doba trvání upgradu skriptu hello. Nezávisí na velikost databáze hello, vrstvy hello služeb, můžete pokusit, hello konfiguraci webového serveru a dalších faktorů, které nelze snadno řídit. Je to proto, že všechny hello přípravné kroky jsou odpojené od postup upgradu hello a lze provést bez dopadu na produkční aplikace hello. Hello efektivitu upgradovací skript pro hello je hello klíčovým faktorem, který určuje hello činnost koncového uživatele při upgradech. Proto hello nejlepší způsob je možné zvýšit je vaše úsilí se zaměříte na provedení upgradu skriptu hello co nejúčinnější.  

## <a name="next-steps"></a>Další kroky
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md).
* toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy pro automatické](sql-database-recovery-using-backups.md).
* toolearn o rychlejší možnosti obnovení, najdete v části [aktivní geografickou replikací](sql-database-geo-replication-overview.md).


