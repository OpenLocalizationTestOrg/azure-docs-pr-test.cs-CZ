---
title: "Zabezpečení databáze v SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 6ea45c40bc428282faf24b4a08f8b0d345adb3fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-a-database-in-sql-data-warehouse"></a><span data-ttu-id="f7d0b-103">Zabezpečení databáze v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f7d0b-103">Secure a database in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7d0b-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="f7d0b-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="f7d0b-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="f7d0b-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="f7d0b-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="f7d0b-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="f7d0b-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="f7d0b-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="f7d0b-108">Tento článek vás provede základy zabezpečení databáze Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-108">This article walks through the basics of securing your Azure SQL Data Warehouse database.</span></span> <span data-ttu-id="f7d0b-109">Konkrétně tento článek vám pomůžou začít s prostředky pro omezení přístupu, ochrany dat a monitorování aktivit v databázi.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-109">In particular, this article will get you started with resources for limiting access, protecting data, and monitoring activities on a database.</span></span>

## <a name="connection-security"></a><span data-ttu-id="f7d0b-110">Zabezpečení připojení</span><span class="sxs-lookup"><span data-stu-id="f7d0b-110">Connection security</span></span>
<span data-ttu-id="f7d0b-111">Zabezpečení připojení spočívá v použití pravidel brány firewall a šifrovaného připojení k omezení a zabezpečení připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-111">Connection Security refers to how you restrict and secure connections to your database using firewall rules and connection encryption.</span></span>

<span data-ttu-id="f7d0b-112">Server i databáze používají pravidla brány firewall k zamítnutí pokusů o připojení z IP adres, které nejsou výslovně povolené.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-112">Firewall rules are used by both the server and the database to reject connection attempts from IP addresses that have not been explicitly whitelisted.</span></span> <span data-ttu-id="f7d0b-113">Povolit připojení z vaší aplikace nebo klientský počítač veřejnou IP adresu, musíte nejprve vytvořit pravidlo brány firewall na úrovni serveru pomocí portálu Azure, rozhraní API REST nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-113">To allow connections from your application or client machine's public IP address, you must first create a server-level firewall rule using the Azure Portal, REST API, or PowerShell.</span></span> <span data-ttu-id="f7d0b-114">Doporučujeme co nejvíce omezit rozsah IP adres povolených v serverové bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-114">As a best practice, you should restrict the IP address ranges allowed through your server firewall as much as possible.</span></span>  <span data-ttu-id="f7d0b-115">Pro přístup k Azure SQL Data Warehouse z místního počítače, zkontrolujte, zda že v bráně firewall na vaší sítí a místním počítači umožňuje odchozí komunikaci na portu TCP 1433.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-115">To access Azure SQL Data Warehouse from your local computer, ensure the firewall on your network and local computer allows outgoing communication on TCP port 1433.</span></span>  <span data-ttu-id="f7d0b-116">Další informace najdete v tématu [brány firewall databáze Azure SQL Database][Azure SQL Database firewall], [příkaz sp_set_firewall_rule][sp_set_firewall_rule], a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-116">For more information, see [Azure SQL Database firewall][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="f7d0b-117">Ve výchozím nastavení jsou šifrované připojení k SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-117">Connections to your SQL Data Warehouse are encrypted by default.</span></span>  <span data-ttu-id="f7d0b-118">Změny nastavení připojení můžete zakázat šifrování se ignorují.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-118">Modifying connection settings to disable encryption are ignored.</span></span>

## <a name="authentication"></a><span data-ttu-id="f7d0b-119">Authentication</span><span class="sxs-lookup"><span data-stu-id="f7d0b-119">Authentication</span></span>
<span data-ttu-id="f7d0b-120">Ověřování se týká způsobu, jakým prokážete svou identitu při připojování k databázi.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-120">Authentication refers to how you prove your identity when connecting to the database.</span></span> <span data-ttu-id="f7d0b-121">SQL Data Warehouse aktuálně podporuje ověřování systému SQL Server s uživatelským jménem a heslem, jakož i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-121">SQL Data Warehouse currently supports SQL Server Authentication with a username and password as well as a Azure Active Directory.</span></span> 

<span data-ttu-id="f7d0b-122">Když jste vytvářeli logický server databáze, zadali jste uživatelské jméno a heslo účtu „server admin“.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-122">When you created the logical server for your database, you specified a "server admin" login with a username and password.</span></span> <span data-ttu-id="f7d0b-123">Pomocí těchto přihlašovacích údajů, můžete ověřovat pro všechny databáze na tomto serveru jako vlastníka databáze, nebo "dbo" pomocí ověřování systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-123">Using these credentials, you can authenticate to any database on that server as the database owner, or "dbo" through SQL Server Authentication.</span></span>

<span data-ttu-id="f7d0b-124">Však jako osvědčený postup, by měl uživatele ve vaší organizaci použít jiný účet k ověření.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-124">However, as a best practice, your organization’s users should use a different account to authenticate.</span></span> <span data-ttu-id="f7d0b-125">Tímto způsobem můžete omezit oprávnění udělená aplikaci a snížení rizika škodlivých aktivit, v případě, že kód aplikace bude zranitelný vůči útoku Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-125">This way you can limit the permissions granted to the application and reduce the risks of malicious activity in case your application code is vulnerable to a SQL injection attack.</span></span> 

<span data-ttu-id="f7d0b-126">Chcete-li vytvořit uživatele ověřeného SQL Server, připojte se ke **hlavní** databáze na serveru s vaše přihlašovací jméno správce serveru a vytvořte nové přihlašovací údaje serveru.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-126">To create a SQL Server Authenticated user, connect to the **master** database on your server with your server admin login and create a new server login.</span></span>  <span data-ttu-id="f7d0b-127">Kromě toho je vhodné vytvořit uživateli v hlavní databázi pro uživatele Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-127">Additionally, it is a good idea to create a user in the master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="f7d0b-128">Vytváření uživatele v předloze umožňuje uživateli přihlášení pomocí nástroje, například aplikace SSMS bez určení názvu databáze.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-128">Creating a user in master allows a user to login using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="f7d0b-129">Také to umožňuje, aby uživatelé používali Průzkumník objektů pokud chcete zobrazit všechny databáze na serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-129">It also allows them to use the object explorer to view all databases on a SQL server.</span></span>

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="f7d0b-130">Připojte se k vaší **databázi SQL Data Warehouse** s vaše přihlašovací jméno správce serveru a vytvořte uživatele databáze založené na přihlášení server, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-130">Then, connect to your **SQL Data Warehouse database** with your server admin login and create a database user based on the server login you just created.</span></span>

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="f7d0b-131">Pokud uživatel bude to další operace, jako je vytvoření přihlášení nebo vytvoření nových databází, bude také potřebovat pro přiřazení `Loginmanager` a `dbmanager` role v hlavní databázi.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-131">If a user will be doing additional operations such as creating logins or creating new databases, they will also need to be assigned to the `Loginmanager` and `dbmanager` roles in the master database.</span></span> <span data-ttu-id="f7d0b-132">Další informace o těchto dalších rolí a ověřování k databázi SQL, najdete v části [Správa databází a přihlašovacích údajů ve službě Azure SQL Database][Managing databases and logins in Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-132">For more information on these additonal roles and authenticating to a SQL Database, see [Managing databases and logins in Azure SQL Database][Managing databases and logins in Azure SQL Database].</span></span>  <span data-ttu-id="f7d0b-133">Další informace o službě Azure AD pro SQL Data Warehouse, najdete v části [připojení k SQL Data Warehouse pomocí pomocí Azure ověřování služby Active Directory][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-133">For more details on Azure AD for SQL Data Warehouse, see [Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].</span></span>

## <a name="authorization"></a><span data-ttu-id="f7d0b-134">Autorizace</span><span class="sxs-lookup"><span data-stu-id="f7d0b-134">Authorization</span></span>
<span data-ttu-id="f7d0b-135">Autorizace odkazuje na co můžete dělat v databázi Azure SQL Data Warehouse a toto se řídí členství v rolích váš uživatelský účet a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-135">Authorization refers to what you can do within an Azure SQL Data Warehouse database, and this is controlled by your user account's role memberships and permissions.</span></span> <span data-ttu-id="f7d0b-136">Doporučený postup je udělit uživatelům co nejmenší možná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-136">As a best practice, you should grant users the least privileges necessary.</span></span> <span data-ttu-id="f7d0b-137">Azure SQL Data Warehouse to výrazně usnadňuje ke správě s rolemi v T-SQL:</span><span class="sxs-lookup"><span data-stu-id="f7d0b-137">Azure SQL Data Warehouse makes this easy to manage with roles in T-SQL:</span></span>

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

<span data-ttu-id="f7d0b-138">Účet správce serveru, který používáte k připojení, je členem skupiny db_owner. Tato skupina může s databází provádět všechny operace.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-138">The server admin account you are connecting with is a member of db_owner, which has authority to do anything within the database.</span></span> <span data-ttu-id="f7d0b-139">Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-139">Save this account for deploying schema upgrades and other management operations.</span></span> <span data-ttu-id="f7d0b-140">Použijte účet „ApplicationUser“, který má omezenější oprávnění a umožňuje připojit se z aplikace k databázi s nejnižšími oprávněními, jaké aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-140">Use the "ApplicationUser" account with more limited permissions to connect from your application to the database with the least privileges needed by your application.</span></span>

<span data-ttu-id="f7d0b-141">Existují i další způsoby, jak ještě více omezit možnosti uživatele služby Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="f7d0b-141">There are ways to further limit what a user can do with Azure SQL Database:</span></span>

* <span data-ttu-id="f7d0b-142">Podrobné [oprávnění] [ Permissions] umožňují řízení operací, které můžete u jednotlivých sloupců tabulky, zobrazení, procedury a další objekty v databázi.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-142">Granular [Permissions][Permissions] let you control which operations you can do on individual columns, tables, views, procedures, and other objects in the database.</span></span> <span data-ttu-id="f7d0b-143">Pomocí oprávnění na podrobné úrovni mít většina řízení a přidělte minimální oprávnění, které jsou nezbytné.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-143">Use granular permissions to have the most control and grant the minimum permissions necessary.</span></span> <span data-ttu-id="f7d0b-144">Systém granulární oprávnění je poněkud složité a bude vyžadovat některé studie efektivně používat.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-144">The granular permission system is somewhat complicated and will require some study to use effectively.</span></span>
* <span data-ttu-id="f7d0b-145">[Rolí databáze] [ Database roles] jiného, než db_datareader a db_datawriter lze použít k vytvoření výkonnější aplikace uživatelské účty nebo méně výkonná účty pro správu.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-145">[Database roles][Database roles] other than db_datareader and db_datawriter can be used to create more powerful application user accounts or less powerful management accounts.</span></span> <span data-ttu-id="f7d0b-146">Předdefinované pevné databázové role poskytují snadný způsob, jak udělit oprávnění, ale může mít za následek přidělení více oprávnění, než je potřeba.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-146">The built-in fixed database roles provide an easy way to grant permissions, but can result in granting more permissions than are necessary.</span></span>
* <span data-ttu-id="f7d0b-147">[Uložené procedury] [ Stored procedures] umožňují omezit akce, které můžete provést na databázi.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-147">[Stored procedures][Stored procedures] can be used to limit the actions that can be taken on the database.</span></span>

<span data-ttu-id="f7d0b-148">Správa databází a logických serverů na portálu Azure Classic nebo v rozhraní API Azure Resource Manageru se řídí tím, jaké role má uživatelský účet na portálu přiřazené.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-148">Managing databases and logical servers from the Azure Classic Portal or using the Azure Resource Manager API is controlled by your portal user account's role assignments.</span></span> <span data-ttu-id="f7d0b-149">Další informace v tomto tématu najdete v tématu [řízení přístupu na základě Role v Azure Portal][Role-based access control in Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-149">For more information on this topic, see [Role-based access control in Azure Portal][Role-based access control in Azure Portal].</span></span>

## <a name="encryption"></a><span data-ttu-id="f7d0b-150">Šifrování</span><span class="sxs-lookup"><span data-stu-id="f7d0b-150">Encryption</span></span>
<span data-ttu-id="f7d0b-151">Azure SQL Data Warehouse transparentní dat šifrování (TDE) pomáhá chránit před ohrožením škodlivých aktivit provedením v reálném čase šifrování a dešifrování dat v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-151">Azure SQL Data Warehouse Transparent Data Encryption (TDE) helps protect against the threat of malicious activity by performing real-time encryption and decryption of your data at rest.</span></span>  <span data-ttu-id="f7d0b-152">Při šifrování databáze, přidružených záloh a souborů protokolů transakci jsou šifrované bez nutnosti změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-152">When you encrypt your database, associated backups and transaction log files are encrypted without requiring any changes to your applications.</span></span> <span data-ttu-id="f7d0b-153">Šifrování TDE zašifruje úložiště celé databáze pomocí symetrický klíč s názvem šifrovací klíč databáze.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-153">TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key.</span></span> <span data-ttu-id="f7d0b-154">V databázi SQL šifrovací klíč databáze je chráněn certifikát integrovaného serveru.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-154">In SQL Database the database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="f7d0b-155">Certifikát integrovaného serveru je jedinečný pro každý server databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-155">The built-in server certificate is unique for each SQL Database server.</span></span> <span data-ttu-id="f7d0b-156">Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-156">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="f7d0b-157">Šifrovacího algoritmu používaného funkcí SQL Data Warehouse je AES 256.</span><span class="sxs-lookup"><span data-stu-id="f7d0b-157">The encryption algorithm used by SQL Data Warehouse is AES-256.</span></span> <span data-ttu-id="f7d0b-158">Obecný popis TDE, najdete v části [transparentní šifrování dat][Transparent Data Encryption].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-158">For a general description of TDE, see [Transparent Data Encryption][Transparent Data Encryption].</span></span>

<span data-ttu-id="f7d0b-159">Můžete šifrovat databázi pomocí [portálu Azure] [ Encryption with Portal] nebo [T-SQL][Encryption with TSQL].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-159">You can encrypt your database using the [Azure Portal][Encryption with Portal] or [T-SQL][Encryption with TSQL].</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7d0b-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7d0b-160">Next steps</span></span>
<span data-ttu-id="f7d0b-161">Podrobnosti a příklady o připojení k SQL Data Warehouse pomocí různých protokolů, najdete v tématu [připojit k SQL Data Warehouse][Connect to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="f7d0b-161">For details and examples on connecting to your SQL Data Warehouse with different protocols, see [Connect to SQL Data Warehouse][Connect to SQL Data Warehouse].</span></span>

<!--Image references-->

<!--Article references-->
[Connect to SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

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
