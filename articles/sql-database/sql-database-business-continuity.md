---
title: "aaaCloud provozní kontinuitu – obnovení databáze – databáze SQL | Microsoft Docs"
description: "Zjistěte, jak Azure SQL Database podporuje provozní kontinuitu v cloudu a obnovení databází a jak pomáhá udržovat klíčové cloudové aplikace v chodu."
keywords: "provozní kontinuita, provozní kontinuita v cloudu, zotavení databáze po havárii, obnovení databáze"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2017
ms.author: sashan
ms.openlocfilehash: c9a6ff86fbbc04ce839a4fca79594b573b71644c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Přehled provozní kontinuity se službou Azure SQL Database

Tento přehled popisuje hello možnosti, které poskytuje Azure SQL Database pro provozní kontinuitu a zotavení po havárii. Další informace o možnostech, doporučení a kurzy pro obnovení z rušivý událostí, které může dojít ke ztrátě dat nebo způsobit, že vaše databáze a aplikace toobecome není k dispozici. Jaké toodo informace, pokud uživatele nebo aplikace chyba ovlivňuje integritu dat, oblast Azure má výpadek nebo pokud vaše aplikace vyžaduje údržby.

## <a name="sql-database-features-that-you-can-use-tooprovide-business-continuity"></a>Funkce databáze SQL, které můžete použít tooprovide kontinuity podnikových procesů

SQL Database poskytuje řadu funkcí provozní kontinuity, včetně automatizovaných záloh a volitelné replikace databáze. Každá má jiné vlastnosti ohledně odhadovaného času obnovení (ERT) a potenciální ztráty dat posledních transakcí. Jakmile tyto možnosti pochopíte, můžete si mezi nimi vybírat a ve většině scénářů je spolu kombinovat a používat pro různé scénáře. Když budete vyvíjet plánu pro kontinuitu podnikových, je nutné toounderstand hello maximální přijatelnou dobu, než aplikace hello plně obnoví po nepříjemným událostem hello – to je cíli času obnovení (RTO). Musíte taky toounderstand hello maximální množství nejnovější data aktualizací (časový interval) hello aplikace může tolerovat ke ztrátám při obnovování po nepříjemným událostem hello – to je cíl bodu obnovení (RPO).

Následující tabulka porovnává Hello hello vložit a plánovaný bod obnovení pro hello tři nejběžnější scénářů.

| Schopnost | Úroveň Basic | Úroveň Standard | Úroveň Premium |
| --- | --- | --- | --- |
| Obnovení k určitému bodu v čase ze zálohy |Libovolný bod obnovení do 7 dní |Libovolný bod obnovení do 35 dní |Libovolný bod obnovení do 35 dní |
| Geografické obnovení ze zálohy geograficky replikované |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |
| Obnovení z úložiště záloh Azure Backup Vault |ERT < 12 h, RPO < 1 týden |ERT < 12 h, RPO < 1 týden |ERT < 12 h, RPO < 1 týden |
| Aktivní geografickou replikaci |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |

### <a name="use-database-backups-toorecover-a-database"></a>Použít databázi toorecover zálohování databáze

SQL Database automaticky provede kombinaci databáze úplné zálohování každý týden, databáze rozdílové zálohy každou hodinu a transakce protokolu zálohování každých pět – deset minut tooprotect vaši firmu před ztrátou data. Tyto zálohy se ukládají v geograficky redundantní úložiště pro 35 dní u databází v hello Standard a Premium úrovně služeb a pro databáze na úrovních základní služby hello 7 dní. Další informace najdete v tématu [úrovních služeb](sql-database-service-tiers.md). Doba uchování hello vaší vrstvy služby nesplňuje požadavky vaší organizace, můžete zvýšit období uchování hello [Změna úrovně služby hello](sql-database-service-tiers.md). Hello databáze úplné a rozdílové zálohy jsou také replikované tooa [spárované datového centra](../best-practices-availability-paired-regions.md) pro ochranu proti výpadku datacentra. Další informace najdete v tématu [automatické zálohování databází](sql-database-automated-backups.md).

Pokud doba uchování předdefinované hello není dostatečná pro aplikaci, můžete ji rozšířit tak, že nakonfigurujete zásady dlouhodobé uchovávání informací databáze. Další informace najdete v tématu [dlouhodobé uchovávání](sql-database-long-term-retention.md).

Můžete použít tyto databáze automatické zálohování toorecover databáze z různých rušivý událostí, jak v rámci datového centra a tooanother datového centra. Pomocí automatické zálohování databází, hello odhadovaný čas obnovení závisí na několika faktorech včetně hello celkový počet databází obnovování hello stejné oblasti v hello stejný čas, hello velikost databáze, hello velikosti protokolu transakcí a šířku pásma sítě. Doba obnovení Hello je obvykle menší než 12 hodin. Při obnovování tooanother datové oblasti, hello potenciální ztrátě dat je omezená too1 hodinu a hello geograficky redundantní úložiště záloh hodinové rozdílové databáze.

> [!IMPORTANT]
> pomocí toorecover automatizované zálohování, musíte být členem role Přispěvatel SQL Server hello nebo vlastník předplatného hello – viz [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md). Můžete ho obnovit pomocí hello portálu Azure, PowerShell nebo hello REST API. Nelze použít jazyk Transact-SQL.
>

Automatizované zálohování použijte pro zajištění provozní kontinuity a jako mechanismus obnovení, pokud vaše aplikace:

* Není považována za klíčovou.
* Nemá vazbu SLA - výpadek 24 hodin nebo již nemá za následek finanční odpovědnosti.
* S nízkou rychlostí změny dat (nízkou transakce za hodinu) a ke ztrátě až tooan je hodina změny přijatelné únikem.
* Je citlivá na změny nákladů.

Pokud potřebujete rychlejší obnovení, použijte [aktivní geografickou replikací](sql-database-geo-replication-overview.md) (popsané dále). Pokud potřebujete toobe možné toorecover data z období starší než 35 dní, použijte [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-in-preview-tooreduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Použít active geografická replikace a automatické převzetí služeb při selhání skupiny (v preview) tooreduce obnovení čas a omezení ztrátě dat přidružené k obnovení

Kromě toho toousing zálohy databáze pro databázi obnovení, pokud dojde k přerušení obchodní, můžete použít [aktivní geografickou replikací](sql-database-geo-replication-overview.md) tooconfigure databáze toohave až toofour čitelný sekundární databáze v oblastech hello z vaší volba. Tyto sekundární databáze jsou synchronizovány s databází primární hello pomocí mechanismus asynchronní replikaci. Tato funkce je použité tooprotect proti přerušení obchodní, pokud dojde k výpadku datacentra nebo během upgradu aplikace. Aktivní geografickou replikací může být také použít tooprovide lepší výkon dotazů pro uživatele toogeographically rozmístěnými dotazy jen pro čtení.

automatizované tooenable a transparentní převzetí služeb při selhání by měl uspořádání geograficky replikované databáze do skupiny pomocí hello [-automatické převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md) funkce SQL Database (-preview).

Pokud primární databáze hello neočekávaně přejde do režimu offline nebo potřebujete tootake ho do offline režimu pro údržbu aktivity, můžete rychle podporovat primární sekundární toobecome hello (také nazývané převzetí služeb při selhání) a nakonfigurovat aplikace tooconnect toohello povýší primární. Pokud vaše aplikace se připojuje toohello databází pomocí naslouchací proces skupiny hello převzetí služeb při selhání, nepotřebujete konfigurace o řetězce připojení SQL hello toochange po převzetí služeb při selhání. U plánovaného převzetí služeb při selhání nedojde ke ztrátě dat. S neplánované převzetí služeb při selhání může být některé malé množství ztráty dat pro velmi poslední transakce z důvodu toohello povaze asynchronní replikaci. Použití skupin automatické převzetí služeb při selhání (v preview), můžete přizpůsobit hello převzetí služeb při selhání zásad toominimize hello potenciální ztrátě dat. Po selhání můžete později navrácení služeb po obnovení – přejde do režimu online podle plánu tooa nebo když hello datového centra. Ve všech případech se uživatelé prostředí malé množství výpadek je potřeba tooreconnect.

> [!IMPORTANT]
> toouse aktivní geografickou replikaci a skupiny automatické převzetí služeb při selhání (v preview), musíte buď být hello vlastník předplatného nebo mít oprávnění správce v systému SQL Server. Můžete nakonfigurovat a převzít pomocí hello portál Azure, prostředí PowerShell nebo hello REST API pomocí oprávnění předplatného Azure nebo pomocí jazyka Transact-SQL s oprávněními systému SQL Server.
> 

Použijte active geografická replikace a automatické převzetí služeb při selhání skupiny (ve verzi preview), pokud vaše aplikace splňuje některé z těchto kritérií:

* Je zvlášť důležitá.
* Má smlouvu SLA, která nepovoluje výpadek delší než 24 hodin.
* Výpadek může mít za následek finanční odpovědnosti.
* Pracuje s vysokou mírou změn dat a ztráta dat za jednu hodinu je nepřijatelná.
* Hello další náklady na aktivní geografickou replikací je nižší než hello potenciální finanční odpovědnosti a přidružené ke ztrátě firmy.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Obnovení databáze po chybě uživatele nebo aplikace
* Nikdo není dokonalý! Uživatel může omylem odstranit některá data, nedopatřením smazat důležitou tabulku nebo dokonce smazat celou databázi. Nebo aplikace omylem přepsat dobrých dat s chybná data z důvodu tooan aplikace vadou.

V takovém scénáři máte následující možnosti obnovení.

### <a name="perform-a-point-in-time-restore"></a>Obnovení k určitému bodu v čase
Hello automatizované zálohování toorecover kopii vaší databáze tooa známé dobré bodu v čase, můžete použít za předpokladu, že čas je v rámci doby uchování databáze hello. Po obnovení databáze hello, můžete nahradit původní databáze hello hello obnovit databáze nebo zkopírování dat hello potřeby z hello obnovit data do původní databáze hello. Pokud hello databáze používá aktivní geografickou replikaci, doporučujeme kopírování dat vyžaduje hello z kopie hello obnovení do původní databáze hello. Pokud je původní databáze hello nahradit hello obnovit databázi, potřebujete tooreconfigure a poté proveďte opakovanou synchronizaci aktivní geografickou replikaci (který může trvat poměrně nějakou dobu pro velké databáze).

Další informace a podrobné pokyny k obnovení databáze tooa časovému současně pomocí hello portál Azure nebo pomocí prostředí PowerShell, najdete v tématu [obnovení bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore). Obnovení nelze provést pomocí jazyka Transact-SQL.

### <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze
Pokud hello databáze, se odstraní, ale nebyl odstraněn hello logického serveru, můžete obnovit hello odstranit databázi toohello bodu, kdy byla odstraněna. Tím se obnoví toohello zálohování databáze stejný logický SQL server, ze kterého byla odstraněna. Můžete obnovit pomocí hello původní název nebo zadejte nový název nebo hello obnovit databázi.

Další informace a podrobné pokyny k obnovení odstraněné databáze hello pomocí portálu Azure nebo pomocí prostředí PowerShell, najdete v části [obnovit odstraněnou databázi](sql-database-recovery-using-backups.md#deleted-database-restore). Obnovení nelze provést pomocí jazyka Transact-SQL.

> [!IMPORTANT]
> Je-li logický server hello odstraněna, nelze obnovit odstraněnou databázi.
>
>

### <a name="restore-from-azure-backup-vault"></a>Obnovení z úložiště záloh Azure Backup Vault
Pokud došlo k chybě hello ztráty dat mimo hello aktuální dobu uchování pro automatizované zálohování a je databáze nakonfigurovaná pro dlouhodobé uchovávání, můžete obnovit ze zálohy týdně v trezoru zálohování Azure tooa novou databázi. V tomto okamžiku můžete nahradit původní databáze hello hello obnovit databáze nebo zkopírování dat hello potřeby z hello obnovit databázi do původní databáze hello. Pokud potřebujete tooretrieve starší verzi upgradu databáze před tooa hlavní aplikace, splnit žádost od auditory nebo právní pořadí, že můžete vytvořit databázi pomocí úplné zálohy uložené v hello úložiště záloh Azure.  Další informace najdete v tématu [Dlouhodobé uchovávání](sql-database-long-term-retention.md).

## <a name="recover-a-database-tooanother-region-from-an-azure-regional-data-center-outage"></a>Obnovení databáze tooanother oblast výpadku center Azure místní data
<!-- Explain this scenario -->

Přestože je taková situace výjimečná, i u datového centra Azure může dojít k výpadku. Při výpadku dojde k narušení provozu, které může trvat jen několik minut nebo až několik hodin.

* Jednou z možností je toowait pro vaši databázi toocome zpět do režimu online při hello data center výpadku. Tento postup funguje pro aplikace, které si může dovolit toohave hello databázi do režimu offline. Například vývojový projekt nebo bezplatnou zkušební verzi nepotřebujete toowork na neustále. Je-li v datovém centru k výpadku, si nejste jisti, jak dlouho může poslední hello výpadku, proto tato možnost funguje jenom v případě nepotřebujete databáze nějakou dobu.
* Další možností je selhání tooeither přes tooanother datové oblasti, pokud používáte aktivní geografickou replikaci nebo hello obnovit databázi pomocí zálohy geograficky redundantní databáze (geografické obnovení). Převzetí služeb při selhání trvá jenom pár sekund, při obnovení databáze ze zálohy trvá hodin.

Při provedení akce, jak dlouho trvá jste toorecover a kolik ztrátě dat, která vám vznikne závisí na tom, jak se rozhodnete toouse tyto funkce kontinuity obchodních ve vaší aplikaci. Skutečně můžete se rozhodnout toouse kombinaci zálohy databáze a aktivní geografickou replikací v závislosti na požadavcích vaší aplikace. Diskuzi o aspektech návrhu aplikací pro samostatné databáze a pro elastické fondy pomocí těchto funkcí provozní kontinuity najdete v tématech [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie zotavení po havárii elastických fondů](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Hello následující části obsahují základní informace o toorecover kroky hello pomocí zálohy databáze nebo aktivní geografickou replikací. Podrobné kroky, včetně plánování požadavky, postup obnovení post a informace o tom, jak toosimulate výpadku tooperform zotavení po havárii zobrazit další podrobnosti najdete v tématu [obnovení databáze serveru SQL výpadku](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Příprava na výpadek
Bez ohledu na to hello firmy kontinuity funkce, kterou používáte musíte:

* Identifikovat a připravit cílový server hello, včetně pravidel brány firewall na úrovni serveru, přihlášení a oprávnění na úrovni hlavní databázi.
* Určit, jak tooredirect klientů a klienta aplikace toohello nový server
* Zdokumentovat další závislosti, například nastavení auditování a výstrahy

Pokud není připravíte správně, přináší aplikace online po převzetí služeb při selhání nebo obnovení databáze bude vyžadovat čas navíc a pravděpodobně také vyžadují, řešení potíží s v době přízvuk - chybný kombinaci.

### <a name="fail-over-tooa-geo-replicated-secondary-database"></a>Selhání tooa geograficky replikované sekundární databáze
Pokud se používá aktivní geografickou replikaci a skupiny automatické převzetí služeb při selhání (v preview) jako váš mechanismus obnovení, můžete nakonfigurovat zásady pro automatické převzetí služeb při selhání nebo použít [ruční převzetí služeb při selhání](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-database). Po zahájení hello převzetí služeb při selhání příčiny hello sekundární toobecome hello nové primární a připravené toorecord nové transakce a reagovat tooqueries - s minimálními ztrátami dat hello dat, ještě nebyl replikován. Informace o navrhování hello proces převzetí služeb při selhání najdete v tématu [návrhu aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Pokud hello datového centra přejde do režimu online původní základní barvy hello automaticky znovu připojit toohello nový primární a sekundární databáze je k. Pokud potřebujete toorelocate hello primární back toohello původní oblast, můžete spustit plánované převzetí služeb při selhání ručně (navrácení služeb po obnovení). 
> 

### <a name="perform-a-geo-restore"></a>Provedení geografické obnovení
Pokud používáte automatizované zálohování s replikací geograficky redundantní úložiště jako váš mechanismus obnovení [zahájení obnovení databáze pomocí geografické obnovení](sql-database-disaster-recovery.md#recover-using-geo-restore). Obnovení obvykle probíhá v rámci 12 hodin – ztráty dat až hodinu tooone určen při hello poslední hodinové rozdílové zálohy s přijatých a replikované. Dokud se nedokončí obnovení hello, hello databáze je nelze toorecord jakékoli transakce nebo reagovat tooany dotazy. Když to obnoví databázi toohello poslední dostupný bod v čase, obnovení hello geograficky sekundární tooany bodu v čase není aktuálně podporován.

> [!NOTE]
> Pokud hello datového centra přejde do režimu online, před přepnutím aplikace přes toohello obnovené databáze, můžete zrušit hello obnovení.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Provedení úloh po převzetí služeb při selhání nebo obnovení
Po obnovení z buď mechanismus obnovení je třeba provést hello po další úkoly předtím, než uživatelé a aplikace jsou, zálohování a spuštění:

* Přesměrovat klienty a klienta aplikace toohello nový server a obnovené databáze
* Ujistěte se, pravidla brány firewall na příslušné úrovni serveru jsou nastavené pro tooconnect uživatelé (nebo použijte [brány firewall úroveň databáze](sql-database-firewall-configure.md#creating-and-managing-firewall-rules))
* Ujistěte se, že se používají odpovídající přihlášení a oprávnění na úrovni hlavní databáze (nebo použijte [obsažené uživatelé](https://msdn.microsoft.com/library/ff929188.aspx))
* Podle potřeby nakonfigurujte auditování.
* Podle potřeby nakonfigurujte výstrahy.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Upgrade aplikace s minimálními výpadky
Někdy aplikace musí být převedeno do režimu offline z důvodu plánované údržby, jako je například upgradu aplikace. [Spravovat aplikace upgrady](sql-database-manage-application-rolling-upgrade.md) popisuje, jak tooenable aktivní geografickou replikací toouse vrácení upgrady odstávka cloudové aplikace toominimize během upgradu a zadejte cestu obnovení, pokud dojde k chybě. 

## <a name="next-steps"></a>Další kroky
Diskuzi o aspektech návrhu aplikací pro samostatné databáze a pro elastické fondy najdete v tématech [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie zotavení po havárii elastických fondů](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
