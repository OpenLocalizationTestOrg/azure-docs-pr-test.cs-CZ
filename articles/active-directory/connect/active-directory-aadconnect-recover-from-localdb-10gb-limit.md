---
title: "Azure AD Connect: Jak omezit toorecover z LocalDB 10GB problém | Microsoft Docs"
description: "Toto téma popisuje, jak toorecover synchronizace Azure AD Connect služby zjistí LocalDB 10GB-li omezit problém."
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 7b8ce6e19b68837639017bb0315eda4b924d525a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-how-toorecover-from-localdb-10-gb-limit"></a>Azure AD Connect: Jak toorecover z limitu LocalDB 10 GB
Azure AD Connect vyžaduje dat identity toostore databáze systému SQL Server. Můžete buď použít výchozí hello, SQL Server 2012 Express LocalDB nainstalované službou Azure AD Connect, nebo použijte vlastní úplné SQL. SQL Server Express má omezení velikosti 10 GB. Pokud při použití LocalDB dosáhnete tohoto limitu, synchronizační služba Azure AD Connect se už nemůže spustit ani správně synchronizovat. Tento článek obsahuje kroky obnovení hello.

## <a name="symptoms"></a>Příznaky
Existují dvě běžné příznaky:

* Služba Azure AD Connect synchronizace **běží** ale selže toosynchronize s *"zastavena database-disk-full (úplné)* chyby.

* Služba Azure AD Connect synchronizace **je nelze toostart**. Když zkusíte toostart hello služby, se nezdaří s událostí 6323 a chybovou zprávou *"hello serveru došlo k chybě systému SQL Server je nedostatek místa na disku."*

## <a name="short-term-recovery-steps"></a>Postup pro krátkodobé obnovení
Tato část obsahuje hello kroky tooreclaim DB místo požadované pro službu Azure AD Connect synchronizace tooresume operaci. Hello krokům patří:
1. [Zjistit stav služby synchronizace hello](#determine-the-synchronization-service-status)
2. [Zmenšení hello databáze](#shrink-the-database)
3. [Odstranění spustit data historie](#delete-run-history-data)
4. [Zkrátit dobu uchování historie spouštění dat](#shorten-retention-period-for-run-history-data)

### <a name="determine-hello-synchronization-service-status"></a>Zjistit stav služby synchronizace hello
Nejdřív zjistěte, zda hello synchronizační služba je stále spuštěn, nebo není:

1. Přihlaste se tooyour server Azure AD Connect jako správce.

2. Přejděte příliš**správce řízení služeb**.

3. Zkontrolujte stav hello **Microsoft Azure AD Sync**.


4. Pokud je spuštěná, zastavte nebo restartujte službu hello. Přeskočit [zmenšení hello databáze](#shrink-the-database) krok a přejděte příliš[odstranění spustit data o historii](#delete-run-history-data) krok.

5. Pokud není spuštěna, zkuste toostart hello služby. Pokud služba hello spustí úspěšně, přeskočte [zmenšení hello databáze](#shrink-the-database) krok a přejděte příliš[odstranění spustit data o historii](#delete-run-history-data) krok. Jinak, pokračujte [zmenšení hello databáze](#shrink-the-database) krok.

### <a name="shrink-hello-database"></a>Zmenšení hello databáze
Použijte toofree operace zmenšení hello až dostatek DB místo toostart hello synchronizační služba. Se uvolní místo DB odebráním prázdné znaky v databázi hello. Tento krok je typu best effort, protože není zaručeno, lze vždy zotavit prostor. toolearn Další informace o operaci zmenšení, přečtěte si tento článek [zmenšit databázi](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Tento krok přeskočte, pokud vám toorun hello synchronizační služba. Jak může vést toopoor výkonu z důvodu tooincreased fragmentace se nedoporučuje tooshrink hello databázi SQL.

Název Hello hello databáze vytvořené pro Azure AD Connect je **ADSync**. tooperform operace zmenšení, musíte být přihlášení jako hello sysadmin nebo vlastník databáze hello databáze. Během instalace služby Azure AD Connect hello následující účty jsou udělena práva správce systému:
* Místní správci
* Hello uživatelský účet, který byl instalace použité toorun Azure AD Connect.
* Hello účet synchronizační služby, který se používá jako hello operační kontext služby Azure AD Connect synchronizace.
* Hello místní skupiny ADSyncAdmins, který byl vytvořen během instalace.

1. Zálohování databáze hello zkopírováním **ADSync.mdf** a **ADSync_log.ldf** soubory umístěné v `%ProgramFiles%\program files\Microsoft Azure AD Sync\Data` tooa bezpečného umístění.

2. Spusťte novou relaci prostředí PowerShell.

3. Přejděte toofolder `%ProgramFiles%\Program Files\Microsoft SQL Server\110\Tools\Binn`.

4. Spustit **sqlcmd** nástroj spuštěním příkazu hello `./SQLCMD.EXE -S “(localdb)\.\ADSync” -U <Username> -P <Password>`, pomocí hello přihlašovací údaje správce systému nebo hello databáze DBO.

5. tooshrink hello databáze, v příkazovém řádku sqlcmd hello (1 >), zadejte `DBCC Shrinkdatabase(ADSync,1);`, za nímž následují `GO` v dalším řádku hello.

6. Pokud je hello operace úspěšná, opakujte toostart hello synchronizační služba. Pokud budete moct začít hello synchronizační služby, přejděte příliš[odstranění spustit data o historii](#delete-run-history-data) krok. Pokud ne, obraťte se na podporu.

### <a name="delete-run-history-data"></a>Odstranění spustit data historie
Ve výchozím nastavení Azure AD Connect zachová až data historie spouštění za tooseven dnů. V tomto kroku jsme odstranit hello spustit místo tooreclaim DB data historie tak, aby služba Azure AD Connect synchronizaci můžete spustit synchronizaci opakovat.

1.  Spustit **Synchronization Service Manager** podle přejdete → tooSTART synchronizační služba.

2.  Přejděte toohello **Operations** kartě.

3.  V části **akce**, vyberte **zrušte spustí**...

4.  Můžete buď zvolit **vymazat všechny spustí** nebo **Clear spuštěná před... <date>**  možnost. Doporučuje se spustíte tak, že smažete spustit historie data, která jsou starší než dvou dnů. Pokud budete pokračovat toorun do DB velikost problém, zvolte hello **vymazat všechny spustí** možnost.

### <a name="shorten-retention-period-for-run-history-data"></a>Zkrátit dobu uchování historie spouštění dat
Tento krok je tooreduce hello pravděpodobnost vzniku hello k problému s limitem 10 GB po několik synchronizačních cyklů.

1. Otevřete novou relaci prostředí PowerShell.

2. Spustit `Get-ADSyncScheduler` a poznamenejte si hello PurgeRunHistoryInterval vlastnosti, která určuje hello aktuální dobu uchování.

3. Spustit `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` tooset hello uchování období tootwo dnů. Doba uchování hello podle potřeby upravte.

## <a name="long-term-solution--migrate-toofull-sql"></a>Dlouhodobé řešení – migrací toofull SQL
Obecně platí, je problém hello naznačuje výslednou, že velikost 10 GB databáze již není dostatečná pro Azure AD Connect toosynchronize vaší místní služby Active Directory tooAzure AD. Doporučuje se, že jste přepnuli toousing hello plnou verzi systému SQL server. Hello LocalDB existujícího nasazení Azure AD Connect nelze nahradit přímo s databází hello hello úplné verze SQL. Místo toho je nutné nasadit nový server Azure AD Connect s hello plnou verzí systému SQL. Doporučuje se provést migraci dráha kde nasazení hello nový server Azure AD Connect (se databáze SQL) jako server pracovní další toohello existující server Azure AD Connect (se LocalDB). 
* Pokyny k jak tooconfigure vzdálené SQL službou Azure AD Connect najdete tooarticle [vlastní instalace Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom).
* Pokyny pro migraci dráha pro upgrade Azure AD Connect, najdete v části tooarticle [Azure AD Connect: Upgrade z předchozí verze toohello nejnovější](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration).

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
