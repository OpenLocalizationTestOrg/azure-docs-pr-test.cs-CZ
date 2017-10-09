---
title: "aaaSQL zotavení po havárii databáze | Microsoft Docs"
description: "Zjistěte, jak hello toorecover databáze z místního datového centra výpadku nebo chyby s Azure SQL Database aktivní geografickou replikaci a možnosti geografické obnovení."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 4800960e-3f9d-40ce-9e55-fb7f2784c067
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/14/2017
ms.author: sashan
ms.openlocfilehash: bae08485863067748107ec4808e52d8e88e2de0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-database-or-failover-tooa-secondary"></a>Obnovení databáze SQL Azure nebo převzetí služeb při selhání tooa sekundární
Azure SQL Database nabízí následující možnosti pro zotavení po výpadku hello:

* [Aktivní geografická replikace](sql-database-geo-replication-overview.md)
* [Geografické obnovení](sql-database-recovery-using-backups.md#point-in-time-restore)

toolearn o obchodní kontinuity scénáře a hello funkce pro podporu těchto scénářů najdete v části [kontinuity podnikových procesů](sql-database-business-continuity.md).

### <a name="prepare-for-hello-event-of-an-outage"></a>Příprava pro událost hello výpadku
Pro úspěšné dokončení obnovení tooanother datové oblasti pomocí aktivní geografickou replikaci nebo geograficky redundantní zálohy musíte tooprepare na server v jiné data center výpadku toobecome hello novým primárním serverem by měl hello musí nastat a také mít dobře definované kroky zdokumentovaných a otestovaná tooensure smooth obnovení. Tyto kroky přípravy zahrnují:

* Identifikujte hello logický server v jiné oblasti toobecome hello novým primárním serverem. S aktivní geografickou replikací bude alespoň jeden a případně všechny sekundární servery hello. U geografické obnovení, bude obvykle na server v hello [spárované oblast](../best-practices-availability-paired-regions.md) hello oblasti, ve kterém je umístěna databáze.
* Identifikovat a volitelně můžete definovat, pravidla brány firewall na úrovni serveru hello potřeby na pro uživatele tooaccess hello nové primární databáze.
* Zjistěte, jak budete tooredirect uživatelé toohello novým primárním serverem, například změnou připojovací řetězce nebo tak, že změníte záznamy DNS.
* Identifikovat a volitelně vytvořit, hello přihlášení, které musí být k dispozici v hlavní databázi hello na hello novým primárním serverem a ujistěte se, že tyto přihlášení má příslušná oprávnění v hlavní databázi hello, pokud existuje. Další informace najdete v tématu [zabezpečení SQL Database po zotavení po havárii](sql-database-geo-replication-security-config.md)
* Určete pravidla výstrah, které bude nutné toobe aktualizované toomap toohello nové primární databáze.
* Konfigurace auditování dokumentu hello na aktuální primární databázi hello
* Provedení [postupu zotavení po havárii](sql-database-disaster-recovery-drills.md). toosimulate na výpadku pro geografické obnovení, můžete odstranit nebo přejmenovat připojení selhání toocause aplikace hello zdrojové databáze. toosimulate výpadku pro aktivní geografickou replikaci, můžete zakázat hello webovou aplikaci nebo virtuální počítač připojen toohello databáze nebo převzetí služeb při selhání hello databáze toocause aplikace chyby připojení.

## <a name="when-tooinitiate-recovery"></a>Při obnovení tooinitiate
operace obnovení Hello ovlivňuje aplikace hello. Je potřeba změnit připojovací řetězec SQL hello nebo přesměrování pomocí DNS a může vést ke ztrátě trvalá data. Proto je třeba jej provést jenom v případě výpadku hello je pravděpodobně toolast delší než plánovanou dobu obnovení vaší aplikace. Po hello aplikace nasazené tooproduction měli provádět pravidelné sledování stavu aplikací hello a použijte hello následující data, která je oprávněná tooassert body, které hello obnovení:

1. Selhání trvalé připojení ze hello aplikační vrstvy toohello databáze.
2. Hello portál Azure zobrazí výstrahu o incidentu v oblasti hello s dopadem na širokou.
3. server Azure SQL Database Hello je označen jako degradovaný.

V závislosti na vaší aplikace tolerance toodowntime a jejich možná obchodní odpovědnosti můžete zvážit následující možnosti obnovení hello.

Použití hello [získat obnovitelné databáze](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) nejnovější bod obnovení geograficky replikované tooget hello.

## <a name="wait-for-service-recovery"></a>Počkejte obnovení
Hello Azure týmy spolupracovat usilovně dostupnost služeb toorestore jak rychle co možná, ale v závislosti na hello příčiny může trvat hodiny nebo dny.  Pokud vaše aplikace může tolerovat významné výpadky je jednoduše vyčkat toocomplete obnovení hello. V takovém případě není třeba žádné akce z vaší strany. Zobrazí aktuální stav služby hello na našem [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/). Po obnovení hello hello oblasti se obnoví dostupnosti vaší aplikace.

## <a name="fail-over-toogeo-replicated-secondary-database"></a>Selhání toogeo replikované sekundární databáze
Pokud obchodní odpovědnosti může způsobit výpadek vaší aplikace by měla být pomocí geograficky replikované databáze v aplikaci. Povolí dostupnosti obnovení tooquickly hello aplikace v jiné oblasti v případě výpadku. Zjistěte, jak příliš[konfigurace geografická replikace](sql-database-geo-replication-portal.md).

dostupnost toorestore hello database(s) budete potřebovat tooinitiate hello sekundární geograficky replikované společnosti převzetí služeb při selhání toohello pomocí jedné z metod hello podporována.

Použijte jednu z následujících průvodců toofail přes tooa geograficky replikované sekundární databáze hello:

* [Selhání tooa geograficky replikované sekundární pomocí portálu Azure](sql-database-geo-replication-portal.md)
* [Převzetí služeb při selhání tooa geograficky replikované sekundární pomocí prostředí PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
* [Selhání tooa geograficky replikované sekundární pomocí T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Obnovit pomocí geografické obnovení
Pokud aplikace výpadek nevede odpovědnosti firmy můžete použít [geografické obnovení](sql-database-recovery-using-backups.md) jako metoda toorecover databází vaší aplikace. Vytvoří kopii databáze hello z jeho nejnovější geograficky redundantní zálohy.

## <a name="configure-your-database-after-recovery"></a>Nakonfigurujte databázi po obnovení
Pokud používáte geografická replikace převzetí služeb při selhání nebo geografické obnovení toorecover výpadku, musí se ujistěte, že hello připojení toohello nové databáze je správně nakonfigurované tak, aby funkce normální aplikace hello lze obnovit. To je kontrolní seznam úloh tooget připraven provozním obnovené databáze.

### <a name="update-connection-strings"></a>Aktualizovat připojovací řetězce
Protože obnovené databáze se bude nacházet v jiném serveru, musíte tooupdate vaší aplikace připojovací řetězec toopoint toothat serveru.

Další informace o změně připojovací řetězce, najdete v části hello příslušném vývojovém jazyk pro vaše [knihovnu připojení](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Konfigurace pravidel brány Firewall
Je nutné toomake konfigurováno hello pravidla brány firewall na serveru a na shodu databáze hello ty, které byly nakonfigurovány na primární server hello a primární databáze. Další informace najdete v tématu [postupy: Konfigurace nastavení brány Firewall (Azure SQL Database)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Konfigurace přihlášení a uživatele databáze
Je nutné toomake jistotu, že všechny přihlášení hello používá vaše aplikace existuje v hello serveru, který je hostitelem obnovené databáze. Další informace najdete v tématu [konfigurace zabezpečení pro geografická replikace](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Měli konfigurace a otestování vašich pravidlech brány firewall serveru a přihlášení (a jejich oprávnění) během postupu zotavení po havárii. Tyto objekty na úrovni serveru a jejich konfigurace nemusí být k dispozici při výpadku hello.
> 
> 

### <a name="setup-telemetry-alerts"></a>Instalační program telemetrie výstrahy
Je nutné toomake se, že nastavení existující pravidlo výstrahy jsou aktualizované toomap toohello obnovené databáze a hello jiný server.

Další informace o databázi pravidla výstrah najdete v tématu [přijímat oznámení výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) a [sledovat stav služeb](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Povolení auditování
Pokud auditování je požadovaná tooaccess vaší databáze, je nutné tooenable auditování po obnovení databáze hello. Další informace najdete v tématu [auditování databáze](sql-database-auditing.md).

## <a name="next-steps"></a>Další kroky
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* toolearn o kontinuity návrhu a obnovení v organizacích, najdete v části [kontinuity scénáře](sql-database-business-continuity.md)
* toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba hello](sql-database-recovery-using-backups.md)

