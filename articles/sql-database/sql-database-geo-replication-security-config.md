---
title: "aaaConfigure zabezpečení databáze SQL Azure pro zotavení po havárii | Microsoft Docs"
description: "Toto téma popisuje důležité informace o zabezpečení pro konfiguraci a správu zabezpečení po obnovení databáze nebo sekundární server tooa převzetí služeb při selhání v případě hello výpadku datacentra nebo jiných po havárii"
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: c7c898c9-69d4-4e16-8b7e-720bbb3353dd
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/13/2016
ms.author: sashan
ms.openlocfilehash: c3172568e1b8ad2a53958200aa6cf19b4a9434ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Konfigurovat a spravovat zabezpečení Azure SQL Database geografické obnovení nebo převzetí služeb při selhání 

> [!NOTE]
> [Aktivní geografickou replikací](sql-database-geo-replication-overview.md) je nyní k dispozici pro všechny databáze v všechny úrovně služeb.
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a>Přehled požadavků ověřování pro zotavení po havárii
Toto téma popisuje tooconfigure požadavky hello ověřování a řízení [aktivní geografickou replikaci](sql-database-geo-replication-overview.md) a hello kroky požadované tooset uživatel přístup toohello sekundární databáze. Také popisuje, jak povolit přístup toohello obnovené databáze po použití [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore). Další informace o možnostech obnovení najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).

## <a name="disaster-recovery-with-contained-users"></a>Zotavení po havárii s omezením uživateli
Na rozdíl od tradičních uživatelé uživatele zcela spravuje sama hello databáze, která musí být namapován toologins v hlavní databázi hello. To má dvě výhody. V modulu snap-in scénáře zotavení po havárii hello hello moci uživatelé nadále tooconnect toohello novou primární databázi nebo databázi hello obnovit pomocí geografické obnovení bez jakékoli dodatečné konfigurace, protože databáze hello spravuje hello uživatelé. Existují také potenciální škálovatelnost a výkon výhody z této konfigurace z hlediska přihlášení. Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx). 

hlavní kompromis Hello je, že je náročnější Správa procesu obnovení po havárii hello ve velkém měřítku. Pokud máte více databází, že použití hello obsaženo stejné přihlášení, udržování hello přihlašovacích údajů pomocí mohou uživatelé v několika databází negate hello výhod uživatelů s omezením. Například hello heslo otočení zásad vyžaduje, že změny konzistentně ve více databází spíše než změna hello heslo pro přihlášení hello jednou v hlavní databázi hello. Z tohoto důvodu, pokud máte více databází, že hello používá stejné uživatelské jméno a heslo, se nedoporučuje používat uživatelů s omezením. 

## <a name="how-tooconfigure-logins-and-users"></a>Jak tooconfigure přihlášení a uživatele
Pokud používáte přihlášení a uživatele (místo uživatelů s omezením), je nutné provést další kroky tooinsure této hello existovat stejné přihlášení v hlavní databázi hello. Hello následující oddíly popisují hello kroky související se situací a další důležité informace.

### <a name="set-up-user-access-tooa-secondary-or-recovered-database"></a>Nastavení uživatelského přístupu tooa sekundární nebo obnovené databáze
Hello sekundární databáze toobe použitelné jako sekundární databázi jen pro čtení a tooensure řádného přístup toohello nové primární databáze nebo hello databázi obnovit pomocí geografické obnovení, hello hlavní databáze hello cílový server musí mít hello příslušná bezpečnostní konfigurace na místě před hello obnovení.

Hello určitá oprávnění pro jednotlivé kroky jsou popsané dále v tomto tématu.

Příprava tooa přístup uživatele sekundární geografická replikace je třeba provést v rámci konfigurace geografická replikace. Příprava přístup uživatelů toohello geografické obnovení databází provést kdykoli po online původní server hello (např. jako součást procházení hello zotavení po Havárii).

> [!NOTE]
> Pokud nedodržíte over nebo geografické obnovení tooa serveru, který nemá správně nakonfigurovanou přihlášení tooit přístupu bude omezený toohello účet správce serveru.
> 
> 

Nastavení přihlášení na cílový server hello zahrnuje tři kroky uvedené níže:

#### <a name="1-determine-logins-with-access-toohello-primary-database"></a>1. Určení přihlášení s primární databází access toohello:
prvním krokem Hello procesu hello je toodetermine které přihlášení musí být duplicitní na cílovém serveru hello. To je provedeno pomocí pár příkazů SELECT, jednu v hello logickou hlavní databázi na zdrojovém serveru hello a druhou v samotné primární databáze hello.

Pouze hello správce serveru nebo člen hello **LoginManager** role serveru můžete určit hello přihlášení na zdrojovém serveru hello s hello následující příkaz SELECT. 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

Pouze členové databázové role db_owner hello, hello dbo uživatele nebo správce serveru, můžete určit všechny objekty uživatele hello databázi v primární databázi hello.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-hello-sid-for-hello-logins-identified-in-step-1"></a>2. Najde hello SID pro přihlášení hello identifikovaného v kroku 1:
Tak, že porovnáte hello výstup hello dotazy z předchozí části hello a hello odpovídající identifikátory SID, můžete namapovat hello server přihlášení toodatabase uživatele. Přihlášení, která mají uživatelé databáze s odpovídajícím SID mít uživatel přístup toothat databázi jako tento uživatel databáze hlavní. 

Hello následujícího dotazu lze použít toosee všechny objekty hello uživatele a jejich identifikátory SID v databázi. Tento dotaz můžete spustit jenom člen správce hello db_owner databáze role nebo serveru.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> Hello **INFORMATION_SCHEMA** a **sys** uživatelů, kteří mají *NULL* SID a hello **hosta** SID je **0x00**. Hello **dbo** SID může začínat *0x01060000000001648000000000048454*, pokud Tvůrce databází hello Dobrý den, správce serveru místo členem **DbManager**.
> 
> 

#### <a name="3-create-hello-logins-on-hello-target-server"></a>3. Vytvoření přihlášení hello na cílovém serveru hello:
Hello posledním krokem je toogo toohello cílový server nebo servery a generovat hello přihlášení s hello vhodné identifikátory SID. Základní syntaxe Hello je následující.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Pokud chcete toogrant uživatel přístup toohello sekundární, ale není toohello primární, můžete to udělat změnou hello přihlášení uživatele na primárním serveru hello pomocí následující syntaxe hello.
> 
> PŘÍKAZ ALTER LOGIN <login name> ZAKÁZAT
> 
> ZAKÁZAT nemění hello heslo, můžete ji v případě potřeby vždy povolit.
> 
> 

## <a name="next-steps"></a>Další kroky
* Další informace o správě přístupu k databázi a přihlášení najdete v tématu [zabezpečení SQL Database: správu přístupu a přihlašovací zabezpečení databáze](sql-database-manage-logins.md).
* Další informace o uživatele databáze s omezením naleznete v tématu [obsažené databáze uživatelé – provádění vaše databáze přenosné](https://msdn.microsoft.com/library/ff929188.aspx).
* Informace o používání a konfiguraci aktivní geografickou replikaci, najdete v článku [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)
* Informace o použití geografické obnovení najdete v tématu [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore)

