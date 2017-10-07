---
title: "připojení aaaTroubleshoot běžné problémy tooAzure databáze SQL"
description: "Kroky tooidentify a řešení běžných chyb připojení pro databázi SQL Azure."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: ac463d1c-aec8-443d-b66e-fa5eadcccfa8
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: eb5f2d7b123a76942c7e4a84a7f475344fbcb144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-connection-issues-tooazure-sql-database"></a>Řešení potíží s tooAzure problémy připojení databáze SQL
Když tooAzure hello připojení databáze SQL se nezdaří, zobrazí se [chybové zprávy](sql-database-develop-error-messages.md). Tento článek je centralizované téma, které vám pomůže vyřešit problémy s připojením k databázi SQL Azure. Zavádí [hello běžných příčin](#cause) z problémů s připojením, doporučuje [nástroje pro odstraňování potíží](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) který vám pomůže identity hello problém a poskytuje řešení problémů s kroky toosolve [ přechodné chyby](#troubleshoot-transient-errors) a [trvalé nebo jiných přechodná chyb](#troubleshoot-persistent-errors). 

Pokud narazíte na potíže s připojením hello, zkuste hello řešení potíží s kroky, které jsou popsané v tomto článku.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Příčina
Potíže s připojením, může být způsobeno kterékoli z následujících hello:

* Selhání tooapply osvědčené postupy a pokyny k návrhu během procesu návrhu aplikace hello.  V tématu [přehled vývoje SQL databáze](sql-database-develop-overview.md) tooget spuštěna.
* Změna konfigurace Azure SQL Database
* Nastavení brány firewall
* Časový limit připojení
* Nesprávné přihlašovací údaje
* Dosažen maximální limit na některé prostředky Azure SQL Database

Obecně platí tooAzure problémy připojení SQL Database můžou být klasifikované následujícím způsobem:

* [Přechodné chyby (krátkodobou nebo přerušované)](#troubleshoot-transient-errors)
* [Trvalé nebo jiných přechodná chyb (chyby, které pravidelně opakovat)](#troubleshoot-persistent-errors)

## <a name="try-hello-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Zkuste hello Poradce při potížích pro problémy s připojením k databázi SQL Azure
Pokud narazíte na chyby konkrétních připojení, zkuste [tento nástroj](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), které vám pomohou rychle identity, který se problém lze vyřešit.

## <a name="troubleshoot-transient-errors"></a>Řešení potíží s přechodné chyby

Když se aplikace připojí tooan Azure SQL database, zobrazí hello následující chybová zpráva:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry hello connection later. If hello problem persists, contact customer support, and provide them hello session tracing ID of <z>"
```

> [!NOTE]
> Tato chybová zpráva se nejčastěji přechodné (krátkodobou).
> 
> 

Tato chyba nastane, když hello databáze Azure se přesunout (nebo překonfigurovat) a jeho připojení toohello SQL database ztratí vaší aplikace. Události změny konfigurace databáze SQL dojít z důvodu plánované událostí (například upgrade softwaru), nebo neplánované událost (například síť procesu havárií, nebo Vyrovnávání zatížení). Většina události změny konfigurace jsou obecně krátkodobou a by se měly dokončit za méně než 60 sekund nejvíce. Tyto události však může občas trvat delší toofinish, například když velké transakce způsobí obnovení dlouhodobé.

### <a name="steps-tooresolve-transient-connectivity-issues"></a>Problémy s připojením přechodný tooresolve kroky

1. Zkontrolujte hello [řídicího panelu služby Microsoft Azure](https://azure.microsoft.com/status) pro všechny známé výpadkům, které došlo k chybě během hello doby, během které hello chyby nehlásí aplikace hello.
2. Aplikace, které se připojují tooa cloudové služby, jako je Azure SQL Database by měl očekávat pravidelné Rekonfigurace události a implementovat opakujte logiku toohandle tyto chyby místo zpřístupnění jako toousers chyby aplikace. Zkontrolujte hello [přechodné chyby](sql-database-connectivity-issues.md) části a hello osvědčené postupy a pokyny v návrhu [přehled vývoje SQL databáze](sql-database-develop-overview.md) Další informace a obecné opakujte strategie. Potom, najdete v části Ukázky kódu v [knihovny připojení k databázi SQL a SQL Server](sql-database-libraries.md) pro konkrétní.
3. Jako databázi blíží jeho omezení prostředků, může to vypadat toobe přechodný problém s připojením. V tématu [řešení potíží s výkonem](sql-database-troubleshoot-performance.md).
4. Pokud potíže s připojením k pokračovat, nebo pokud hello doba, pro kterou vaše aplikace chyby hello překročí 60 sekund nebo pokud se zobrazí více výskyty chyby hello v daný den, soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options) lokality.

## <a name="troubleshoot-persistent-errors"></a>Řešení potíží s trvalé chyby
Pokud aplikace hello trvale selže tooconnect tooAzure SQL Database, obvykle označuje potíže s jedním z následujících hello:

* Konfigurace brány firewall. Hello Azure SQL database nebo klienta brána firewall blokuje připojení tooAzure databáze SQL.
* Síťové změny konfigurace na straně klienta hello: například novou IP adresu nebo proxy server.
* Uživatelskou chybu: například chybně připojení parametrů, třeba název serveru hello hello připojovacího řetězce.

### <a name="steps-tooresolve-persistent-connectivity-issues"></a>Kroky tooresolve trvalé připojení problémy
1. Nastavit [pravidla brány firewall](sql-database-configure-firewall-settings.md) tooallow hello klienta IP adresu. Pro dočasné pro účely testování nastavte pravidla brány firewall pomocí 0.0.0.0 jako hello spuštění rozsah IP adres a pomocí 255.255.255.255 jako hello ukončení rozsah IP adres. Otevře se hello serveru tooall IP adresy. Pokud to řeší problém s připojením, toto pravidlo odebrat a vytvořte pravidlo brány firewall pro správně omezené IP adresu nebo rozsah adres. 
2. Na všechny brány firewall mezi klientem hello a hello Internetu Ujistěte se, že port 1433 je otevřený pro odchozí připojení. Zkontrolujte [konfigurace brány Windows Firewall tooAllow hello přístup k serveru SQL](https://msdn.microsoft.com/library/cc646023.aspx) a [hybridní Identity požadované porty a protokoly](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) pro další ukazatele související tooadditional porty, které budete potřebovat tooopen pro Ověřování Azure Active Directory.
3. Ověřte připojovací řetězec a další nastavení připojení. Najdete v části připojovací řetězec v hello hello [tématu problémy s připojením](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4. Zkontrolujte stav služby hello řídicím panelu. Pokud se domníváte, že je místní výpadku, přečtěte si téma [zotavit výpadku](sql-database-disaster-recovery.md) kroky toorecover tooa nové oblasti.

## <a name="next-steps"></a>Další kroky
* [Řešení potíží s výkonem databáze SQL Azure](sql-database-troubleshoot-performance.md)
* [Hledání hello dokumentaci k Microsoft Azure](http://azure.microsoft.com/search/documentation/)
* [Zobrazení hello nejnovější aktualizace služby Azure SQL Database toohello](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Další zdroje
* [Přehled vývoje SQL databáze](sql-database-develop-overview.md)
* [Obecné pokyny přechodné selhání zpracování](../best-practices-retry-general.md)
* [Knihovny připojení k databázi SQL a SQL Server](sql-database-libraries.md)

