---
title: "Konfigurace zabezpečení databáze SQL Azure pro zotavení po havárii | Microsoft Docs"
description: "Toto téma popisuje důležité informace o zabezpečení pro konfiguraci a správu zabezpečení po obnovení databáze nebo převzetí služeb při selhání na sekundární server v případě výpadku datacentra nebo jiných po havárii"
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
ms.openlocfilehash: de5e1732dab570b80692efcdd08e4ed2d8c98800
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a><span data-ttu-id="a0cf8-103">Konfigurovat a spravovat zabezpečení Azure SQL Database geografické obnovení nebo převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="a0cf8-103">Configure and manage Azure SQL Database security for geo-restore or failover</span></span> 

> [!NOTE]
> <span data-ttu-id="a0cf8-104">[Aktivní geografickou replikací](sql-database-geo-replication-overview.md) je nyní k dispozici pro všechny databáze v všechny úrovně služeb.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-104">[Active geo-replication](sql-database-geo-replication-overview.md) is now available for all databases in all service tiers.</span></span>
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a><span data-ttu-id="a0cf8-105">Přehled požadavků ověřování pro zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="a0cf8-105">Overview of authentication requirements for disaster recovery</span></span>
<span data-ttu-id="a0cf8-106">Toto téma popisuje požadavky na ověřování, konfigurovat a řídit [aktivní geografickou replikaci](sql-database-geo-replication-overview.md) a kroky potřebné k nastavení přístup uživatelů k sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-106">This topic describes the authentication requirements to configure and control [active geo-replication](sql-database-geo-replication-overview.md) and the steps required to set up user access to the secondary database.</span></span> <span data-ttu-id="a0cf8-107">Také popisuje, jak povolit přístup k obnovené databáze po použití [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-107">It also describes how enable access to the recovered database after using [geo-restore](sql-database-recovery-using-backups.md#geo-restore).</span></span> <span data-ttu-id="a0cf8-108">Další informace o možnostech obnovení najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-108">For more information on recovery options, see [Business Continuity Overview](sql-database-business-continuity.md).</span></span>

## <a name="disaster-recovery-with-contained-users"></a><span data-ttu-id="a0cf8-109">Zotavení po havárii s omezením uživateli</span><span class="sxs-lookup"><span data-stu-id="a0cf8-109">Disaster recovery with contained users</span></span>
<span data-ttu-id="a0cf8-110">Na rozdíl od tradičních uživatele, které musí být namapován na přihlášení v hlavní databázi, je uživatele zcela spravuje samotná databáze.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-110">Unlike traditional users, which must be mapped to logins in the master database, a contained user is managed completely by the database itself.</span></span> <span data-ttu-id="a0cf8-111">To má dvě výhody.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-111">This has two benefits.</span></span> <span data-ttu-id="a0cf8-112">Ve scénáři zotavení po havárii uživatelé můžete pokračovat pro připojení k nové primární databáze nebo databáze obnovena pomocí geografické obnovení bez jakékoli dodatečné konfigurace, protože databáze spravuje uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-112">In the disaster recovery scenario, the users can continue to connect to the new primary database or the database recovered using geo-restore without any additional configuration, because the database manages the users.</span></span> <span data-ttu-id="a0cf8-113">Existují také potenciální škálovatelnost a výkon výhody z této konfigurace z hlediska přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-113">There are also potential scalability and performance benefits from this configuration from a login perspective.</span></span> <span data-ttu-id="a0cf8-114">Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-114">For more information, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span> 

<span data-ttu-id="a0cf8-115">Hlavní kompromis je, že Správa procesu obnovení po havárii ve velkém měřítku je další náročné.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-115">The main trade-off is that managing the disaster recovery process at scale is more challenging.</span></span> <span data-ttu-id="a0cf8-116">Pokud máte více databází, které používají stejné přihlašovací údaje, může zachování přihlašovací údaje ve více databází pomocí uživatelů s omezením negate výhod uživatelů s omezením.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-116">When you have multiple databases that use the same login, maintaining the credentials using contained users in multiple databases may negate the benefits of contained users.</span></span> <span data-ttu-id="a0cf8-117">Například otočení zásady hesel vyžaduje, aby změny konzistentně ve více databází místo Změna hesla pro přihlášení jednou v hlavní databázi.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-117">For example, the password rotation policy requires that changes be made consistently in multiple databases rather than changing the password for the login once in the master database.</span></span> <span data-ttu-id="a0cf8-118">Z tohoto důvodu, pokud máte více databází, které používají stejné uživatelské jméno a heslo, uživatelů s omezením se nedoporučuje používat.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-118">For this reason, if you have multiple databases that use the same user name and password, using contained users is not recommended.</span></span> 

## <a name="how-to-configure-logins-and-users"></a><span data-ttu-id="a0cf8-119">Postup konfigurace přihlášení a uživatele</span><span class="sxs-lookup"><span data-stu-id="a0cf8-119">How to configure logins and users</span></span>
<span data-ttu-id="a0cf8-120">Pokud používáte přihlášení a uživatele (místo uživatelů s omezením), je nutné provést další kroky zajistit, že v hlavní databázi existují stejné přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-120">If you are using logins and users (rather than contained users), you must take extra steps to insure that the same logins exist in the master database.</span></span> <span data-ttu-id="a0cf8-121">Následující oddíly popisují kroky související se situací a další důležité informace.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-121">The following sections outline the steps involved and additional considerations.</span></span>

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a><span data-ttu-id="a0cf8-122">Nastavit přístup uživatelů k sekundární nebo obnovené databáze</span><span class="sxs-lookup"><span data-stu-id="a0cf8-122">Set up user access to a secondary or recovered database</span></span>
<span data-ttu-id="a0cf8-123">V pořadí pro sekundární databázi možné používat jako sekundární databáze jen pro čtení a k zajištění řádného přístup k nové primární databáze nebo databáze obnovit pomocí geografické obnovení hlavní databáze cílového serveru musí mít příslušná bezpečnostní konfigurace na místě před obnovení.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-123">In order for the secondary database to be usable as a read-only secondary database, and to ensure proper access to the new primary database or the database recovered using geo-restore, the master database of the target server must have the appropriate security configuration in place before the recovery.</span></span>

<span data-ttu-id="a0cf8-124">Konkrétní oprávnění pro jednotlivé kroky jsou popsané dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-124">The specific permissions for each step are described later in this topic.</span></span>

<span data-ttu-id="a0cf8-125">Příprava přístup uživatelů k sekundární geografická replikace je třeba provést v rámci konfigurace geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-125">Preparing user access to a geo-replication secondary should be performed as part configuring geo-replication.</span></span> <span data-ttu-id="a0cf8-126">Příprava uživatelský přístup k databázím geografické obnovení provést kdykoli po online původního serveru (např. jako součást postupu zotavení po Havárii).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-126">Preparing user access to the geo-restored databases should be performed at any time when the original server is online (e.g. as part of the DR drill).</span></span>

> [!NOTE]
> <span data-ttu-id="a0cf8-127">Pokud jste převzetí služeb při selhání nebo geografické obnovení na server, který nemá správně nakonfigurovanou přihlášení, přístup k němu bude omezena na účet správce serveru.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-127">If you fail over or geo-restore to a server that does not have properly configured logins, access to it will be limited to the server admin account.</span></span>
> 
> 

<span data-ttu-id="a0cf8-128">Nastavení přihlášení na cílovém serveru zahrnuje tři kroky uvedené níže:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-128">Setting up logins on the target server involves three steps outlined below:</span></span>

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a><span data-ttu-id="a0cf8-129">1. Určení přihlášení s přístupem k primární databázi:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-129">1. Determine logins with access to the primary database:</span></span>
<span data-ttu-id="a0cf8-130">Prvním krokem procesu je určit, které přihlášení musí být duplicitní na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-130">The first step of the process is to determine which logins must be duplicated on the target server.</span></span> <span data-ttu-id="a0cf8-131">To je provedeno pomocí pár příkazů SELECT, jeden v logické hlavní databázi na zdrojovém serveru a jeden v primární databázi, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-131">This is accomplished with a pair of SELECT statements, one in the logical master database on the source server and one in the primary database itself.</span></span>

<span data-ttu-id="a0cf8-132">Pouze správce serveru nebo člen **LoginManager** role serveru můžete určit přihlášení na zdrojovém serveru s následující příkaz SELECT.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-132">Only the server admin or a member of the **LoginManager** server role can determine the logins on the source server with the following SELECT statement.</span></span> 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

<span data-ttu-id="a0cf8-133">Pouze členové databázové role db_owner, dbo uživatele nebo správce serveru, můžete určit všechny objekty uživatele databáze v primární databázi.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-133">Only a member of the db_owner database role, the dbo user, or server admin, can determine all of the database user principals in the primary database.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a><span data-ttu-id="a0cf8-134">2. Zjistit SID pro přihlášení identifikovaného v kroku 1:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-134">2. Find the SID for the logins identified in step 1:</span></span>
<span data-ttu-id="a0cf8-135">Porovnání výstup dotazy z předchozí části a odpovídající identifikátory SID, můžete namapovat server přihlášení uživatele databáze.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-135">By comparing the output of the queries from the previous section and matching the SIDs, you can map the server login to database user.</span></span> <span data-ttu-id="a0cf8-136">Přihlášení, která mají uživatelé databáze s odpovídajícím SID mít uživatel přístup k databázi jako tento uživatel databáze hlavní.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-136">Logins that have a database user with a matching SID have user access to that database as that database user principal.</span></span> 

<span data-ttu-id="a0cf8-137">Následující dotaz slouží k zobrazení všech objektů uživatele a jejich identifikátory SID v databázi.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-137">The following query can be used to see all of the user principals and their SIDs in a database.</span></span> <span data-ttu-id="a0cf8-138">Tento dotaz můžete spustit jenom člen správce databáze db_owner role nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-138">Only a member of the db_owner database role or server admin can run this query.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> <span data-ttu-id="a0cf8-139">**INFORMATION_SCHEMA** a **sys** uživatelů, kteří mají *NULL* identifikátory SID a **hosta** SID je **0x00**.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-139">The **INFORMATION_SCHEMA** and **sys** users have *NULL* SIDs, and the **guest** SID is **0x00**.</span></span> <span data-ttu-id="a0cf8-140">**Dbo** SID může začínat *0x01060000000001648000000000048454*, pokud byla databáze Tvůrce správce serveru místo členem **DbManager**.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-140">The **dbo** SID may start with *0x01060000000001648000000000048454*, if the database creator was the server admin instead of a member of **DbManager**.</span></span>
> 
> 

#### <a name="3-create-the-logins-on-the-target-server"></a><span data-ttu-id="a0cf8-141">3. Vytvoření přihlášení na cílovém serveru:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-141">3. Create the logins on the target server:</span></span>
<span data-ttu-id="a0cf8-142">Posledním krokem je můžete přejít na cílový server nebo servery a generovat přihlášení s odpovídající identifikátory SID.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-142">The last step is to go to the target server, or servers, and generate the logins with the appropriate SIDs.</span></span> <span data-ttu-id="a0cf8-143">Základní syntaxe je následující.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-143">The basic syntax is as follows.</span></span>

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> <span data-ttu-id="a0cf8-144">Pokud chcete udělit přístup uživatelům sekundární, ale není primární, můžete to udělat změnou přihlášení uživatele na primárním serveru pomocí následující syntaxe.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-144">If you want to grant user access to the secondary, but not to the primary, you can do that by altering the user login on the primary server by using the following syntax.</span></span>
> 
> <span data-ttu-id="a0cf8-145">PŘÍKAZ ALTER LOGIN <login name> ZAKÁZAT</span><span class="sxs-lookup"><span data-stu-id="a0cf8-145">ALTER LOGIN <login name> DISABLE</span></span>
> 
> <span data-ttu-id="a0cf8-146">ZAKÁZAT nezmění heslo, takže můžete vždy ji povolit v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-146">DISABLE doesn’t change the password, so you can always enable it if needed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a0cf8-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0cf8-147">Next steps</span></span>
* <span data-ttu-id="a0cf8-148">Další informace o správě přístupu k databázi a přihlášení najdete v tématu [zabezpečení SQL Database: správu přístupu a přihlašovací zabezpečení databáze](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-148">For more information on managing database access and logins, see [SQL Database security: Manage database access and login security](sql-database-manage-logins.md).</span></span>
* <span data-ttu-id="a0cf8-149">Další informace o uživatele databáze s omezením naleznete v tématu [obsažené databáze uživatelé – provádění vaše databáze přenosné](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-149">For more information on contained database users, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span>
* <span data-ttu-id="a0cf8-150">Informace o používání a konfiguraci aktivní geografickou replikaci, najdete v článku [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a0cf8-150">For information about using and configuring active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>
* <span data-ttu-id="a0cf8-151">Informace o použití geografické obnovení najdete v tématu [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore)</span><span class="sxs-lookup"><span data-stu-id="a0cf8-151">For information about using geo-restore, see [geo-restore](sql-database-recovery-using-backups.md#geo-restore)</span></span>

