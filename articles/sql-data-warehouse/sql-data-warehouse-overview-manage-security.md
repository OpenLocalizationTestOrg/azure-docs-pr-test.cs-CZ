---
title: "aaaSecure databáze v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro zabezpečení databáze v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 8fa2f5ca-4cf5-4418-99a2-4dc745799850
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 5ef4d760e00be46bfe7808bc95dbe1e4b3a51165
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-database-in-sql-data-warehouse"></a><span data-ttu-id="d02cd-103">Zabezpečení databáze v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="d02cd-103">Secure a database in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d02cd-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d02cd-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="d02cd-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="d02cd-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="d02cd-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="d02cd-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="d02cd-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="d02cd-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="d02cd-108">Tento článek vás provede základy hello zabezpečení databáze Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d02cd-108">This article walks through hello basics of securing your Azure SQL Data Warehouse database.</span></span> <span data-ttu-id="d02cd-109">Konkrétně tento článek vám pomůžou začít s prostředky pro omezení přístupu, ochrany dat a monitorování aktivit v databázi.</span><span class="sxs-lookup"><span data-stu-id="d02cd-109">In particular, this article will get you started with resources for limiting access, protecting data, and monitoring activities on a database.</span></span>

## <a name="connection-security"></a><span data-ttu-id="d02cd-110">Zabezpečení připojení</span><span class="sxs-lookup"><span data-stu-id="d02cd-110">Connection security</span></span>
<span data-ttu-id="d02cd-111">Zabezpečení připojení odkazuje toohow můžete omezit a zabezpečené připojení tooyour databázi pomocí pravidla brány firewall a šifrování připojení.</span><span class="sxs-lookup"><span data-stu-id="d02cd-111">Connection Security refers toohow you restrict and secure connections tooyour database using firewall rules and connection encryption.</span></span>

<span data-ttu-id="d02cd-112">Pravidla brány firewall jsou používány obě hello serveru a hello databáze tooreject pokusy o připojení z IP adres, které nebyly explicitně seznam povolených adres.</span><span class="sxs-lookup"><span data-stu-id="d02cd-112">Firewall rules are used by both hello server and hello database tooreject connection attempts from IP addresses that have not been explicitly whitelisted.</span></span> <span data-ttu-id="d02cd-113">tooallow připojení z vaší aplikace nebo klientský počítač veřejnou IP adresu, musíte nejdřív vytvořit pravidlo brány firewall na úrovni serveru pomocí hello portálu Azure, rozhraní API REST nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d02cd-113">tooallow connections from your application or client machine's public IP address, you must first create a server-level firewall rule using hello Azure Portal, REST API, or PowerShell.</span></span> <span data-ttu-id="d02cd-114">Jako osvědčený postup měli omezit hello rozsahy IP adres povoleny prostřednictvím brány firewall serveru co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="d02cd-114">As a best practice, you should restrict hello IP address ranges allowed through your server firewall as much as possible.</span></span>  <span data-ttu-id="d02cd-115">tooaccess Azure SQL Data Warehouse z místního počítače, ujistěte se, že brána firewall hello na vaší sítí a místním počítači umožňuje odchozí komunikaci na portu TCP 1433.</span><span class="sxs-lookup"><span data-stu-id="d02cd-115">tooaccess Azure SQL Data Warehouse from your local computer, ensure hello firewall on your network and local computer allows outgoing communication on TCP port 1433.</span></span>  <span data-ttu-id="d02cd-116">Další informace najdete v tématu [brány firewall databáze Azure SQL Database][Azure SQL Database firewall], [příkaz sp_set_firewall_rule][sp_set_firewall_rule], a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="d02cd-116">For more information, see [Azure SQL Database firewall][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="d02cd-117">Ve výchozím nastavení jsou šifrované připojení tooyour SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d02cd-117">Connections tooyour SQL Data Warehouse are encrypted by default.</span></span>  <span data-ttu-id="d02cd-118">Úprava nastavení toodisable připojení šifrování se ignorují.</span><span class="sxs-lookup"><span data-stu-id="d02cd-118">Modifying connection settings toodisable encryption are ignored.</span></span>

## <a name="authentication"></a><span data-ttu-id="d02cd-119">Authentication</span><span class="sxs-lookup"><span data-stu-id="d02cd-119">Authentication</span></span>
<span data-ttu-id="d02cd-120">Ověřování odkazuje toohow prokázání své identity při připojování toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="d02cd-120">Authentication refers toohow you prove your identity when connecting toohello database.</span></span> <span data-ttu-id="d02cd-121">SQL Data Warehouse aktuálně podporuje ověřování systému SQL Server s uživatelským jménem a heslem, jakož i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d02cd-121">SQL Data Warehouse currently supports SQL Server Authentication with a username and password as well as a Azure Active Directory.</span></span> 

<span data-ttu-id="d02cd-122">Když vytvoříte hello logický server pro databázi, jste zadali "serveru admin" přihlášení s uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="d02cd-122">When you created hello logical server for your database, you specified a "server admin" login with a username and password.</span></span> <span data-ttu-id="d02cd-123">Pomocí těchto přihlašovacích údajů, můžete ověřovat tooany databáze na tomto serveru jako vlastník databáze hello nebo "dbo" pomocí ověřování systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d02cd-123">Using these credentials, you can authenticate tooany database on that server as hello database owner, or "dbo" through SQL Server Authentication.</span></span>

<span data-ttu-id="d02cd-124">Jako osvědčený postup, ale měli uživatele ve vaší organizaci použít tooauthenticate jiný účet.</span><span class="sxs-lookup"><span data-stu-id="d02cd-124">However, as a best practice, your organization’s users should use a different account tooauthenticate.</span></span> <span data-ttu-id="d02cd-125">Tímto způsobem můžete omezit hello oprávnění udělená aplikaci toohello a snížení rizika hello škodlivých aktivit v případě kód aplikace je snadno napadnutelný tooa útok prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="d02cd-125">This way you can limit hello permissions granted toohello application and reduce hello risks of malicious activity in case your application code is vulnerable tooa SQL injection attack.</span></span> 

<span data-ttu-id="d02cd-126">toocreate uživatel ověřený Server SQL, připojit toohello **hlavní** databáze na serveru s vaše přihlašovací jméno správce serveru a vytvořte nové přihlašovací údaje serveru.</span><span class="sxs-lookup"><span data-stu-id="d02cd-126">toocreate a SQL Server Authenticated user, connect toohello **master** database on your server with your server admin login and create a new server login.</span></span>  <span data-ttu-id="d02cd-127">Kromě toho je vhodné toocreate uživatele v hlavní databázi hello pro uživatele Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d02cd-127">Additionally, it is a good idea toocreate a user in hello master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="d02cd-128">Vytváření uživatele v předloze umožňuje uživatele toologin, pomocí nástroje, například aplikace SSMS bez zadání názvu databáze.</span><span class="sxs-lookup"><span data-stu-id="d02cd-128">Creating a user in master allows a user toologin using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="d02cd-129">Umožňuje také jejich toouse hello object explorer tooview všechny databáze na serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="d02cd-129">It also allows them toouse hello object explorer tooview all databases on a SQL server.</span></span>

```sql
-- Connect toomaster database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="d02cd-130">Připojte tooyour **databázi SQL Data Warehouse** s vaše přihlašovací jméno správce serveru a vytvořte uživatele databáze založené na přihlášení server hello jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d02cd-130">Then, connect tooyour **SQL Data Warehouse database** with your server admin login and create a database user based on hello server login you just created.</span></span>

```sql
-- Connect tooSQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="d02cd-131">Pokud uživatel bude to další operace, jako je vytvoření přihlášení nebo vytvoření nových databází, budou také potřebovat toobe přiřazené toohello `Loginmanager` a `dbmanager` role v hlavní databázi hello.</span><span class="sxs-lookup"><span data-stu-id="d02cd-131">If a user will be doing additional operations such as creating logins or creating new databases, they will also need toobe assigned toohello `Loginmanager` and `dbmanager` roles in hello master database.</span></span> <span data-ttu-id="d02cd-132">Další informace o těchto dalších rolí a ověřovací tooa SQL Database, najdete v části [Správa databází a přihlašovacích údajů ve službě Azure SQL Database][Managing databases and logins in Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="d02cd-132">For more information on these additonal roles and authenticating tooa SQL Database, see [Managing databases and logins in Azure SQL Database][Managing databases and logins in Azure SQL Database].</span></span>  <span data-ttu-id="d02cd-133">Další informace o službě Azure AD pro SQL Data Warehouse, najdete v části [připojení tooSQL datového skladu pomocí pomocí Azure Active Directory, ověřování][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].</span><span class="sxs-lookup"><span data-stu-id="d02cd-133">For more details on Azure AD for SQL Data Warehouse, see [Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].</span></span>

## <a name="authorization"></a><span data-ttu-id="d02cd-134">Autorizace</span><span class="sxs-lookup"><span data-stu-id="d02cd-134">Authorization</span></span>
<span data-ttu-id="d02cd-135">Autorizace odkazuje toowhat můžete provést v rámci databázi Azure SQL Data Warehouse, a tím je řízeno oprávnění a členství v rolích váš uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="d02cd-135">Authorization refers toowhat you can do within an Azure SQL Data Warehouse database, and this is controlled by your user account's role memberships and permissions.</span></span> <span data-ttu-id="d02cd-136">Jako osvědčený postup byste měli udělit uživatelům hello nejnižší oprávnění potřebná.</span><span class="sxs-lookup"><span data-stu-id="d02cd-136">As a best practice, you should grant users hello least privileges necessary.</span></span> <span data-ttu-id="d02cd-137">Azure SQL Data Warehouse umožňuje tento snadno toomanage s rolemi v T-SQL:</span><span class="sxs-lookup"><span data-stu-id="d02cd-137">Azure SQL Data Warehouse makes this easy toomanage with roles in T-SQL:</span></span>

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser tooread data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser toowrite data
```

<span data-ttu-id="d02cd-138">Hello účet správce serveru, ke kterému se připojujete se členem role db_owner, který má autority toodo nic v rámci hello databáze.</span><span class="sxs-lookup"><span data-stu-id="d02cd-138">hello server admin account you are connecting with is a member of db_owner, which has authority toodo anything within hello database.</span></span> <span data-ttu-id="d02cd-139">Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy.</span><span class="sxs-lookup"><span data-stu-id="d02cd-139">Save this account for deploying schema upgrades and other management operations.</span></span> <span data-ttu-id="d02cd-140">Použít účet "ApplicationUser" hello s omezenější tooconnect oprávnění z vaší aplikace toohello databáze pomocí hello nejnižších oprávnění vyžadované vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d02cd-140">Use hello "ApplicationUser" account with more limited permissions tooconnect from your application toohello database with hello least privileges needed by your application.</span></span>

<span data-ttu-id="d02cd-141">Existují způsoby toofurther limit co může uživatel provádět s Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="d02cd-141">There are ways toofurther limit what a user can do with Azure SQL Database:</span></span>

* <span data-ttu-id="d02cd-142">Podrobné [oprávnění] [ Permissions] umožňují řízení operací, které můžete u jednotlivých sloupců tabulky, zobrazení, procedury a další objekty v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="d02cd-142">Granular [Permissions][Permissions] let you control which operations you can do on individual columns, tables, views, procedures, and other objects in hello database.</span></span> <span data-ttu-id="d02cd-143">Použití oprávnění na podrobné úrovni toohave hello většina řízení a přidělte minimální oprávnění hello nezbytné.</span><span class="sxs-lookup"><span data-stu-id="d02cd-143">Use granular permissions toohave hello most control and grant hello minimum permissions necessary.</span></span> <span data-ttu-id="d02cd-144">systém granulární oprávnění Hello je poněkud složité a bude vyžadovat některé studie toouse efektivně.</span><span class="sxs-lookup"><span data-stu-id="d02cd-144">hello granular permission system is somewhat complicated and will require some study toouse effectively.</span></span>
* <span data-ttu-id="d02cd-145">[Rolí databáze] [ Database roles] jiného, než db_datareader a db_datawriter můžou být použité toocreate výkonnější aplikace uživatelské účty nebo méně výkonná účty pro správu.</span><span class="sxs-lookup"><span data-stu-id="d02cd-145">[Database roles][Database roles] other than db_datareader and db_datawriter can be used toocreate more powerful application user accounts or less powerful management accounts.</span></span> <span data-ttu-id="d02cd-146">Hello předdefinované pevné databázové role poskytují toogrant oprávnění snadný způsob, ale může mít za následek přidělení více oprávnění, než je potřeba.</span><span class="sxs-lookup"><span data-stu-id="d02cd-146">hello built-in fixed database roles provide an easy way toogrant permissions, but can result in granting more permissions than are necessary.</span></span>
* <span data-ttu-id="d02cd-147">[Uložené procedury] [ Stored procedures] lze použít toolimit hello akce, které můžete provést na databázi hello.</span><span class="sxs-lookup"><span data-stu-id="d02cd-147">[Stored procedures][Stored procedures] can be used toolimit hello actions that can be taken on hello database.</span></span>

<span data-ttu-id="d02cd-148">Správa databází a logické servery z hello portálu Azure Classic nebo pomocí rozhraní API služby Azure Resource Manager hello řídí přiřazení rolí portálu uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="d02cd-148">Managing databases and logical servers from hello Azure Classic Portal or using hello Azure Resource Manager API is controlled by your portal user account's role assignments.</span></span> <span data-ttu-id="d02cd-149">Další informace v tomto tématu najdete v tématu [řízení přístupu na základě Role v Azure Portal][Role-based access control in Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="d02cd-149">For more information on this topic, see [Role-based access control in Azure Portal][Role-based access control in Azure Portal].</span></span>

## <a name="encryption"></a><span data-ttu-id="d02cd-150">Šifrování</span><span class="sxs-lookup"><span data-stu-id="d02cd-150">Encryption</span></span>
<span data-ttu-id="d02cd-151">Azure SQL Data Warehouse transparentní dat šifrování (TDE) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování dat v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="d02cd-151">Azure SQL Data Warehouse Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of your data at rest.</span></span>  <span data-ttu-id="d02cd-152">Při šifrování databáze, přidružených záloh a souborů protokolů transakci jsou šifrované bez nutnosti jakékoli změny tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="d02cd-152">When you encrypt your database, associated backups and transaction log files are encrypted without requiring any changes tooyour applications.</span></span> <span data-ttu-id="d02cd-153">Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze.</span><span class="sxs-lookup"><span data-stu-id="d02cd-153">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="d02cd-154">V databázi SQL hello databáze šifrovací klíč je chráněný certifikát integrovaného serveru.</span><span class="sxs-lookup"><span data-stu-id="d02cd-154">In SQL Database hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="d02cd-155">certifikát integrovaného serveru Hello je jedinečný pro každý server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="d02cd-155">hello built-in server certificate is unique for each SQL Database server.</span></span> <span data-ttu-id="d02cd-156">Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="d02cd-156">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="d02cd-157">Hello šifrovacího algoritmu používaného funkcí SQL Data Warehouse je AES 256.</span><span class="sxs-lookup"><span data-stu-id="d02cd-157">hello encryption algorithm used by SQL Data Warehouse is AES-256.</span></span> <span data-ttu-id="d02cd-158">Obecný popis TDE, najdete v části [transparentní šifrování dat][Transparent Data Encryption].</span><span class="sxs-lookup"><span data-stu-id="d02cd-158">For a general description of TDE, see [Transparent Data Encryption][Transparent Data Encryption].</span></span>

<span data-ttu-id="d02cd-159">Můžete šifrovat databázi pomocí hello [portálu Azure] [ Encryption with Portal] nebo [T-SQL][Encryption with TSQL].</span><span class="sxs-lookup"><span data-stu-id="d02cd-159">You can encrypt your database using hello [Azure Portal][Encryption with Portal] or [T-SQL][Encryption with TSQL].</span></span>

## <a name="next-steps"></a><span data-ttu-id="d02cd-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d02cd-160">Next steps</span></span>
<span data-ttu-id="d02cd-161">Podrobnosti a příklady o připojení tooyour SQL Data Warehouse pomocí různých protokolů najdete v tématu [připojit tooSQL datového skladu][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="d02cd-161">For details and examples on connecting tooyour SQL Data Warehouse with different protocols, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

<!--Image references-->

<!--Article references-->
[Connect tooSQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Database roles]: https://msdn.microsoft.com/library/ms189121.aspx
[Managing databases and logins in Azure SQL Database]: https://msdn.microsoft.com/library/ee336235.aspx
[Permissions]: https://msdn.microsoft.com/library/ms191291.aspx
[Stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Role-based access control in Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
