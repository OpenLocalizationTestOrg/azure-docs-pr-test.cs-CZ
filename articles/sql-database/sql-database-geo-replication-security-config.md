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
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a><span data-ttu-id="71028-103">Konfigurovat a spravovat zabezpečení Azure SQL Database geografické obnovení nebo převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="71028-103">Configure and manage Azure SQL Database security for geo-restore or failover</span></span> 

> [!NOTE]
> <span data-ttu-id="71028-104">[Aktivní geografickou replikací](sql-database-geo-replication-overview.md) je nyní k dispozici pro všechny databáze v všechny úrovně služeb.</span><span class="sxs-lookup"><span data-stu-id="71028-104">[Active geo-replication](sql-database-geo-replication-overview.md) is now available for all databases in all service tiers.</span></span>
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a><span data-ttu-id="71028-105">Přehled požadavků ověřování pro zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="71028-105">Overview of authentication requirements for disaster recovery</span></span>
<span data-ttu-id="71028-106">Toto téma popisuje tooconfigure požadavky hello ověřování a řízení [aktivní geografickou replikaci](sql-database-geo-replication-overview.md) a hello kroky požadované tooset uživatel přístup toohello sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="71028-106">This topic describes hello authentication requirements tooconfigure and control [active geo-replication](sql-database-geo-replication-overview.md) and hello steps required tooset up user access toohello secondary database.</span></span> <span data-ttu-id="71028-107">Také popisuje, jak povolit přístup toohello obnovené databáze po použití [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore).</span><span class="sxs-lookup"><span data-stu-id="71028-107">It also describes how enable access toohello recovered database after using [geo-restore](sql-database-recovery-using-backups.md#geo-restore).</span></span> <span data-ttu-id="71028-108">Další informace o možnostech obnovení najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="71028-108">For more information on recovery options, see [Business Continuity Overview](sql-database-business-continuity.md).</span></span>

## <a name="disaster-recovery-with-contained-users"></a><span data-ttu-id="71028-109">Zotavení po havárii s omezením uživateli</span><span class="sxs-lookup"><span data-stu-id="71028-109">Disaster recovery with contained users</span></span>
<span data-ttu-id="71028-110">Na rozdíl od tradičních uživatelé uživatele zcela spravuje sama hello databáze, která musí být namapován toologins v hlavní databázi hello.</span><span class="sxs-lookup"><span data-stu-id="71028-110">Unlike traditional users, which must be mapped toologins in hello master database, a contained user is managed completely by hello database itself.</span></span> <span data-ttu-id="71028-111">To má dvě výhody.</span><span class="sxs-lookup"><span data-stu-id="71028-111">This has two benefits.</span></span> <span data-ttu-id="71028-112">V modulu snap-in scénáře zotavení po havárii hello hello moci uživatelé nadále tooconnect toohello novou primární databázi nebo databázi hello obnovit pomocí geografické obnovení bez jakékoli dodatečné konfigurace, protože databáze hello spravuje hello uživatelé.</span><span class="sxs-lookup"><span data-stu-id="71028-112">In hello disaster recovery scenario, hello users can continue tooconnect toohello new primary database or hello database recovered using geo-restore without any additional configuration, because hello database manages hello users.</span></span> <span data-ttu-id="71028-113">Existují také potenciální škálovatelnost a výkon výhody z této konfigurace z hlediska přihlášení.</span><span class="sxs-lookup"><span data-stu-id="71028-113">There are also potential scalability and performance benefits from this configuration from a login perspective.</span></span> <span data-ttu-id="71028-114">Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="71028-114">For more information, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span> 

<span data-ttu-id="71028-115">hlavní kompromis Hello je, že je náročnější Správa procesu obnovení po havárii hello ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="71028-115">hello main trade-off is that managing hello disaster recovery process at scale is more challenging.</span></span> <span data-ttu-id="71028-116">Pokud máte více databází, že použití hello obsaženo stejné přihlášení, udržování hello přihlašovacích údajů pomocí mohou uživatelé v několika databází negate hello výhod uživatelů s omezením.</span><span class="sxs-lookup"><span data-stu-id="71028-116">When you have multiple databases that use hello same login, maintaining hello credentials using contained users in multiple databases may negate hello benefits of contained users.</span></span> <span data-ttu-id="71028-117">Například hello heslo otočení zásad vyžaduje, že změny konzistentně ve více databází spíše než změna hello heslo pro přihlášení hello jednou v hlavní databázi hello.</span><span class="sxs-lookup"><span data-stu-id="71028-117">For example, hello password rotation policy requires that changes be made consistently in multiple databases rather than changing hello password for hello login once in hello master database.</span></span> <span data-ttu-id="71028-118">Z tohoto důvodu, pokud máte více databází, že hello používá stejné uživatelské jméno a heslo, se nedoporučuje používat uživatelů s omezením.</span><span class="sxs-lookup"><span data-stu-id="71028-118">For this reason, if you have multiple databases that use hello same user name and password, using contained users is not recommended.</span></span> 

## <a name="how-tooconfigure-logins-and-users"></a><span data-ttu-id="71028-119">Jak tooconfigure přihlášení a uživatele</span><span class="sxs-lookup"><span data-stu-id="71028-119">How tooconfigure logins and users</span></span>
<span data-ttu-id="71028-120">Pokud používáte přihlášení a uživatele (místo uživatelů s omezením), je nutné provést další kroky tooinsure této hello existovat stejné přihlášení v hlavní databázi hello.</span><span class="sxs-lookup"><span data-stu-id="71028-120">If you are using logins and users (rather than contained users), you must take extra steps tooinsure that hello same logins exist in hello master database.</span></span> <span data-ttu-id="71028-121">Hello následující oddíly popisují hello kroky související se situací a další důležité informace.</span><span class="sxs-lookup"><span data-stu-id="71028-121">hello following sections outline hello steps involved and additional considerations.</span></span>

### <a name="set-up-user-access-tooa-secondary-or-recovered-database"></a><span data-ttu-id="71028-122">Nastavení uživatelského přístupu tooa sekundární nebo obnovené databáze</span><span class="sxs-lookup"><span data-stu-id="71028-122">Set up user access tooa secondary or recovered database</span></span>
<span data-ttu-id="71028-123">Hello sekundární databáze toobe použitelné jako sekundární databázi jen pro čtení a tooensure řádného přístup toohello nové primární databáze nebo hello databázi obnovit pomocí geografické obnovení, hello hlavní databáze hello cílový server musí mít hello příslušná bezpečnostní konfigurace na místě před hello obnovení.</span><span class="sxs-lookup"><span data-stu-id="71028-123">In order for hello secondary database toobe usable as a read-only secondary database, and tooensure proper access toohello new primary database or hello database recovered using geo-restore, hello master database of hello target server must have hello appropriate security configuration in place before hello recovery.</span></span>

<span data-ttu-id="71028-124">Hello určitá oprávnění pro jednotlivé kroky jsou popsané dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="71028-124">hello specific permissions for each step are described later in this topic.</span></span>

<span data-ttu-id="71028-125">Příprava tooa přístup uživatele sekundární geografická replikace je třeba provést v rámci konfigurace geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="71028-125">Preparing user access tooa geo-replication secondary should be performed as part configuring geo-replication.</span></span> <span data-ttu-id="71028-126">Příprava přístup uživatelů toohello geografické obnovení databází provést kdykoli po online původní server hello (např. jako součást procházení hello zotavení po Havárii).</span><span class="sxs-lookup"><span data-stu-id="71028-126">Preparing user access toohello geo-restored databases should be performed at any time when hello original server is online (e.g. as part of hello DR drill).</span></span>

> [!NOTE]
> <span data-ttu-id="71028-127">Pokud nedodržíte over nebo geografické obnovení tooa serveru, který nemá správně nakonfigurovanou přihlášení tooit přístupu bude omezený toohello účet správce serveru.</span><span class="sxs-lookup"><span data-stu-id="71028-127">If you fail over or geo-restore tooa server that does not have properly configured logins, access tooit will be limited toohello server admin account.</span></span>
> 
> 

<span data-ttu-id="71028-128">Nastavení přihlášení na cílový server hello zahrnuje tři kroky uvedené níže:</span><span class="sxs-lookup"><span data-stu-id="71028-128">Setting up logins on hello target server involves three steps outlined below:</span></span>

#### <a name="1-determine-logins-with-access-toohello-primary-database"></a><span data-ttu-id="71028-129">1. Určení přihlášení s primární databází access toohello:</span><span class="sxs-lookup"><span data-stu-id="71028-129">1. Determine logins with access toohello primary database:</span></span>
<span data-ttu-id="71028-130">prvním krokem Hello procesu hello je toodetermine které přihlášení musí být duplicitní na cílovém serveru hello.</span><span class="sxs-lookup"><span data-stu-id="71028-130">hello first step of hello process is toodetermine which logins must be duplicated on hello target server.</span></span> <span data-ttu-id="71028-131">To je provedeno pomocí pár příkazů SELECT, jednu v hello logickou hlavní databázi na zdrojovém serveru hello a druhou v samotné primární databáze hello.</span><span class="sxs-lookup"><span data-stu-id="71028-131">This is accomplished with a pair of SELECT statements, one in hello logical master database on hello source server and one in hello primary database itself.</span></span>

<span data-ttu-id="71028-132">Pouze hello správce serveru nebo člen hello **LoginManager** role serveru můžete určit hello přihlášení na zdrojovém serveru hello s hello následující příkaz SELECT.</span><span class="sxs-lookup"><span data-stu-id="71028-132">Only hello server admin or a member of hello **LoginManager** server role can determine hello logins on hello source server with hello following SELECT statement.</span></span> 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

<span data-ttu-id="71028-133">Pouze členové databázové role db_owner hello, hello dbo uživatele nebo správce serveru, můžete určit všechny objekty uživatele hello databázi v primární databázi hello.</span><span class="sxs-lookup"><span data-stu-id="71028-133">Only a member of hello db_owner database role, hello dbo user, or server admin, can determine all of hello database user principals in hello primary database.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-hello-sid-for-hello-logins-identified-in-step-1"></a><span data-ttu-id="71028-134">2. Najde hello SID pro přihlášení hello identifikovaného v kroku 1:</span><span class="sxs-lookup"><span data-stu-id="71028-134">2. Find hello SID for hello logins identified in step 1:</span></span>
<span data-ttu-id="71028-135">Tak, že porovnáte hello výstup hello dotazy z předchozí části hello a hello odpovídající identifikátory SID, můžete namapovat hello server přihlášení toodatabase uživatele.</span><span class="sxs-lookup"><span data-stu-id="71028-135">By comparing hello output of hello queries from hello previous section and matching hello SIDs, you can map hello server login toodatabase user.</span></span> <span data-ttu-id="71028-136">Přihlášení, která mají uživatelé databáze s odpovídajícím SID mít uživatel přístup toothat databázi jako tento uživatel databáze hlavní.</span><span class="sxs-lookup"><span data-stu-id="71028-136">Logins that have a database user with a matching SID have user access toothat database as that database user principal.</span></span> 

<span data-ttu-id="71028-137">Hello následujícího dotazu lze použít toosee všechny objekty hello uživatele a jejich identifikátory SID v databázi.</span><span class="sxs-lookup"><span data-stu-id="71028-137">hello following query can be used toosee all of hello user principals and their SIDs in a database.</span></span> <span data-ttu-id="71028-138">Tento dotaz můžete spustit jenom člen správce hello db_owner databáze role nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="71028-138">Only a member of hello db_owner database role or server admin can run this query.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> <span data-ttu-id="71028-139">Hello **INFORMATION_SCHEMA** a **sys** uživatelů, kteří mají *NULL* SID a hello **hosta** SID je **0x00**.</span><span class="sxs-lookup"><span data-stu-id="71028-139">hello **INFORMATION_SCHEMA** and **sys** users have *NULL* SIDs, and hello **guest** SID is **0x00**.</span></span> <span data-ttu-id="71028-140">Hello **dbo** SID může začínat *0x01060000000001648000000000048454*, pokud Tvůrce databází hello Dobrý den, správce serveru místo členem **DbManager**.</span><span class="sxs-lookup"><span data-stu-id="71028-140">hello **dbo** SID may start with *0x01060000000001648000000000048454*, if hello database creator was hello server admin instead of a member of **DbManager**.</span></span>
> 
> 

#### <a name="3-create-hello-logins-on-hello-target-server"></a><span data-ttu-id="71028-141">3. Vytvoření přihlášení hello na cílovém serveru hello:</span><span class="sxs-lookup"><span data-stu-id="71028-141">3. Create hello logins on hello target server:</span></span>
<span data-ttu-id="71028-142">Hello posledním krokem je toogo toohello cílový server nebo servery a generovat hello přihlášení s hello vhodné identifikátory SID.</span><span class="sxs-lookup"><span data-stu-id="71028-142">hello last step is toogo toohello target server, or servers, and generate hello logins with hello appropriate SIDs.</span></span> <span data-ttu-id="71028-143">Základní syntaxe Hello je následující.</span><span class="sxs-lookup"><span data-stu-id="71028-143">hello basic syntax is as follows.</span></span>

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> <span data-ttu-id="71028-144">Pokud chcete toogrant uživatel přístup toohello sekundární, ale není toohello primární, můžete to udělat změnou hello přihlášení uživatele na primárním serveru hello pomocí následující syntaxe hello.</span><span class="sxs-lookup"><span data-stu-id="71028-144">If you want toogrant user access toohello secondary, but not toohello primary, you can do that by altering hello user login on hello primary server by using hello following syntax.</span></span>
> 
> <span data-ttu-id="71028-145">PŘÍKAZ ALTER LOGIN <login name> ZAKÁZAT</span><span class="sxs-lookup"><span data-stu-id="71028-145">ALTER LOGIN <login name> DISABLE</span></span>
> 
> <span data-ttu-id="71028-146">ZAKÁZAT nemění hello heslo, můžete ji v případě potřeby vždy povolit.</span><span class="sxs-lookup"><span data-stu-id="71028-146">DISABLE doesn’t change hello password, so you can always enable it if needed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="71028-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71028-147">Next steps</span></span>
* <span data-ttu-id="71028-148">Další informace o správě přístupu k databázi a přihlášení najdete v tématu [zabezpečení SQL Database: správu přístupu a přihlašovací zabezpečení databáze](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="71028-148">For more information on managing database access and logins, see [SQL Database security: Manage database access and login security](sql-database-manage-logins.md).</span></span>
* <span data-ttu-id="71028-149">Další informace o uživatele databáze s omezením naleznete v tématu [obsažené databáze uživatelé – provádění vaše databáze přenosné](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="71028-149">For more information on contained database users, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span>
* <span data-ttu-id="71028-150">Informace o používání a konfiguraci aktivní geografickou replikaci, najdete v článku [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="71028-150">For information about using and configuring active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>
* <span data-ttu-id="71028-151">Informace o použití geografické obnovení najdete v tématu [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore)</span><span class="sxs-lookup"><span data-stu-id="71028-151">For information about using geo-restore, see [geo-restore](sql-database-recovery-using-backups.md#geo-restore)</span></span>

