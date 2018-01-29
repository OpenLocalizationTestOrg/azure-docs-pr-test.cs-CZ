---
title: "Zotavení po havárii pro databáze SQL | Microsoft Docs"
description: "Naučte se obnovit databázi z místního datového centra výpadku nebo chyby s Azure SQL Database aktivní geografickou replikaci a možnosti geografické obnovení."
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
ms.workload: On Demand
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 224c0b9f12595ec6cdc65e3d397fb62dba504d06
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Obnova databáze SQL Azure nebo převzetí služeb při selhání na sekundární
Azure SQL Database nabízí následující možnosti pro zotavení po výpadku:

* [Aktivní geografickou replikaci a převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md)
* [Geografické obnovení](sql-database-recovery-using-backups.md#point-in-time-restore)

Další informace o obchodních scénářů kontinuity a podpora těchto scénářů funkcí najdete v tématu [kontinuity podnikových procesů](sql-database-business-continuity.md).

> [!NOTE]
> Pokud používáte zónově redundantní prémiových databází nebo fondy, je automatizované procesu obnovení a zbytek tomto materiálu nevztahuje. 

### <a name="prepare-for-the-event-of-an-outage"></a>Příprava pro událost výpadku
Pro úspěšné dokončení obnovení na jiné datové oblasti pomocí skupin převzetí služeb při selhání nebo geograficky redundantní zálohy, je nutné připravit server v jiném datovém centru výpadku stane novým primárním serverem by potřeba nastat a také mít dobře definované kroků popsaných a otestovat zajistit bezproblémovou obnovení. Tyto kroky přípravy zahrnují:

* Identifikujte logický server v jiné oblasti stane novým primárním serverem. U geografické obnovení, bude obvykle na server v [spárované oblast](../best-practices-availability-paired-regions.md) pro oblast, ve kterém je umístěna databáze. Tím ušetříte náklady na další provozu během operací geografické obnovení.
* Identifikovat a volitelně můžete definovat pravidla brány firewall na úrovni serveru, na potřeba uživatelům přístup k nové primární databáze.
* Zjistěte, jak chcete přesměrovat uživatele na nový primární server, jako třeba změnou připojovací řetězce nebo tak, že změníte záznamy DNS.
* Identifikovat a volitelně vytvořte přihlášení, které musí být k dispozici v hlavní databázi na nový primární server a ujistěte se, že tyto přihlášení má příslušná oprávnění v hlavní databázi, pokud existuje. Další informace najdete v tématu [zabezpečení SQL Database po zotavení po havárii](sql-database-geo-replication-security-config.md)
* Určete pravidla výstrah, které bude potřeba aktualizovat k mapování na nový primární databáze.
* Konfigurace auditování v aktuální primární databázi dokumentů
* Provedení [postupu zotavení po havárii](sql-database-disaster-recovery-drills.md). Pro simulaci výpadku pro geografické obnovení, můžete odstranit nebo přejmenovat databázi zdrojové způsobí selhání připojení aplikace. Pro simulaci výpadku pomocí skupin převzetí služeb při selhání, můžete zakázat webové aplikace nebo virtuálního počítače připojené k databázi nebo databázi převzetí služeb při selhání dojít k chybám připojení aplikace.

## <a name="when-to-initiate-recovery"></a>Když zahájíte obnovení
Aplikace má dopad na operaci obnovení. Je potřeba změnit připojovací řetězec SQL nebo přesměrování pomocí DNS a může vést ke ztrátě trvalá data. Proto by měl v době, kdy se výpadek je pravděpodobně poslední déle, než plánovanou dobu obnovení vaší aplikace. Při nasazení aplikace do produkčního prostředí doporučujeme provádět pravidelné sledování stavu aplikací a použít k vyhodnocení, zda je oprávněná obnovení těchto datových bodů:

1. Chyba trvalé připojení z aplikační vrstvě do databáze.
2. Portál Azure zobrazí výstrahu o incidentu v oblasti s dopadem na širokou.

> [!NOTE]
> Pokud používáte skupiny převzetí služeb při selhání a zvolit automatické převzetí služeb při selhání, procesu obnovení je automatické a transparentní pro aplikace. 

V závislosti na vaší aplikace tolerance k výpadku a jejich možná obchodní odpovědnosti můžete zvážit následující možnosti obnovení.

Použití [získat obnovitelné databáze](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) Chcete-li získat nejnovější bod obnovení geograficky replikované.

## <a name="wait-for-service-recovery"></a>Počkejte obnovení
Pracovní týmy Azure usilovně k obnovení dostupnost služby, jak rychle co možná, ale v závislosti na kořenovém způsobit, že může trvat hodiny nebo dny.  Pokud vaše aplikace může tolerovat významné výpadky je jednoduše počkat na dokončení obnovení. V takovém případě není třeba žádné akce z vaší strany. Zobrazí aktuální stav služby na našem [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/). Po obnovení oblasti se obnoví dostupnosti vaší aplikace.

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>Převzetí služeb při selhání do geograficky replikované sekundární server ve skupině pro převzetí služeb při selhání
Pokud obchodní odpovědnosti může způsobit výpadek vaší aplikace by měla pomocí skupin převzetí služeb při selhání. Umožní aplikaci pro rychlé obnovení dostupnosti v jiné oblasti v případě výpadku. Zjistěte, jak [konfigurovat převzetí služeb při selhání skupiny](sql-database-geo-replication-portal.md).

Chcete-li obnovit dostupnosti databází, které potřebujete k zahájení převzetí služeb při selhání na sekundární server pomocí jedné z podporovaných metod.

K převzetí služeb při selhání do geograficky replikované sekundární databáze, použijte jednu z následujících příručkách:

* [Převzetí služeb při selhání do geograficky replikované sekundární server pomocí portálu Azure](sql-database-geo-replication-portal.md)
* [Převzetí služeb při selhání na sekundární server pomocí prostředí PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)

## <a name="recover-using-geo-restore"></a>Obnovit pomocí geografické obnovení
Pokud aplikace výpadek nevede odpovědnosti firmy můžete použít [geografické obnovení](sql-database-recovery-using-backups.md) jako metodu obnovení databází vaší aplikace. Vytvoří kopii databáze z jeho nejnovější geograficky redundantní zálohy.

## <a name="configure-your-database-after-recovery"></a>Nakonfigurujte databázi po obnovení
Pokud používáte geografické obnovení obnovit výpadku, musí se ujistěte, že připojení k nové databáze je správně nakonfigurované tak, aby funkce normální aplikací lze obnovit. Toto je kontrolní seznam úloh připravit provozním obnovené databáze.

### <a name="update-connection-strings"></a>Aktualizovat připojovací řetězce
Protože obnovené databáze se bude nacházet v jiném serveru, je potřeba aktualizovat připojovací řetězec vaší aplikace tak, aby odkazoval na tomto serveru.

Další informace o změně připojovací řetězce, najdete v příslušném vývojovém jazyk pro vaše [knihovnu připojení](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Konfigurace pravidel brány Firewall
Budete muset Ujistěte se, jestli pravidla brány firewall nakonfigurované na serveru a v databázi shodují s těmi, které byly nakonfigurované na primární server a primární databáze. Další informace najdete v tématu [postupy: Konfigurace nastavení brány Firewall (Azure SQL Database)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Konfigurace přihlášení a uživatele databáze
Musíte zajistit, že všechny přihlášení, které používá vaše aplikace existuje v serveru, který je hostitelem obnovené databáze. Další informace najdete v tématu [konfigurace zabezpečení pro geografická replikace](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Měli konfigurace a otestování vašich pravidlech brány firewall serveru a přihlášení (a jejich oprávnění) během postupu zotavení po havárii. Tyto objekty na úrovni serveru a jejich konfigurace nemusí být k dispozici během se výpadek.
> 
> 

### <a name="setup-telemetry-alerts"></a>Instalační program telemetrie výstrahy
Budete muset Ujistěte se, že nastavení existující pravidlo výstrahy jsou aktualizovány k mapování na obnovené databáze a jiný server.

Další informace o databázi pravidla výstrah najdete v tématu [přijímat oznámení výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) a [sledovat stav služeb](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Povolení auditování
Pokud auditování je nutné pro přístup k databázi, budete muset povolit auditování po obnovení databáze. Další informace najdete v tématu [auditování databáze](sql-database-auditing.md).

## <a name="next-steps"></a>Další kroky
* Další informace o Azure SQL Database automatizované zálohování najdete v tématu [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* Další informace o obchodních scénářů návrhu a obnovení kontinuity najdete v tématu [kontinuity scénáře](sql-database-business-continuity.md)
* Další informace o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba](sql-database-recovery-using-backups.md)

