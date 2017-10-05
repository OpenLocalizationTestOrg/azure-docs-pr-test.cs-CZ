---
title: "Zkopírujte Azure SQL database | Microsoft Docs"
description: "Vytvořte kopii databáze Azure SQL"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5aaf6bcd-3839-49b5-8c77-cbdf786e359b
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 8c1e3c80b9f24089dc99463d6ea8ae5d0ea7b19d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="copy-an-azure-sql-database"></a><span data-ttu-id="7570e-103">Kopírování databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="7570e-103">Copy an Azure SQL database</span></span>

<span data-ttu-id="7570e-104">Azure SQL Database nabízí několik metod pro vytváření stavu transakční konzistence kopii existující databázi Azure SQL na stejném serveru nebo jiný server.</span><span class="sxs-lookup"><span data-stu-id="7570e-104">Azure SQL Database provides several methods for creating a transactionally consistent copy of an existing Azure SQL database on either the same server or a different server.</span></span> <span data-ttu-id="7570e-105">Můžete kopírovat databázi SQL pomocí portálu Azure, PowerShell nebo T-SQL.</span><span class="sxs-lookup"><span data-stu-id="7570e-105">You can copy a SQL database by using the Azure portal, PowerShell, or T-SQL.</span></span> 

## <a name="overview"></a><span data-ttu-id="7570e-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="7570e-106">Overview</span></span>

<span data-ttu-id="7570e-107">Kopie databáze je snímek zdrojové databáze od času požadavku kopírování.</span><span class="sxs-lookup"><span data-stu-id="7570e-107">A database copy is a snapshot of the source database as of the time of the copy request.</span></span> <span data-ttu-id="7570e-108">Můžete vybrat stejný server nebo jiný server, jeho vrstvy služeb a úroveň výkonu nebo úroveň různých výkonu v rámci stejné služby vrstvě (edice).</span><span class="sxs-lookup"><span data-stu-id="7570e-108">You can select the same server or a different server, its service tier and performance level, or a different performance level within the same service tier (edition).</span></span> <span data-ttu-id="7570e-109">Po dokončení kopírování bude databáze plně funkční, nezávislé.</span><span class="sxs-lookup"><span data-stu-id="7570e-109">After the copy is complete, it becomes a fully functional, independent database.</span></span> <span data-ttu-id="7570e-110">V tomto okamžiku můžete upgradovat nebo ponížit na libovolná edice.</span><span class="sxs-lookup"><span data-stu-id="7570e-110">At this point, you can upgrade or downgrade it to any edition.</span></span> <span data-ttu-id="7570e-111">Přihlášení, uživatelů a oprávnění lze spravovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="7570e-111">The logins, users, and permissions can be managed independently.</span></span>  

## <a name="logins-in-the-database-copy"></a><span data-ttu-id="7570e-112">Přihlášení v kopii databáze</span><span class="sxs-lookup"><span data-stu-id="7570e-112">Logins in the database copy</span></span>

<span data-ttu-id="7570e-113">Při kopírování databáze do stejného logického serveru, stejné přihlášení lze použít v obou databází.</span><span class="sxs-lookup"><span data-stu-id="7570e-113">When you copy a database to the same logical server, the same logins can be used on both databases.</span></span> <span data-ttu-id="7570e-114">Objekt, že slouží ke zkopírování databáze zabezpečení se změní na vlastníka databáze na novou databázi.</span><span class="sxs-lookup"><span data-stu-id="7570e-114">The security principal you use to copy the database becomes the database owner on the new database.</span></span> <span data-ttu-id="7570e-115">Všichni uživatelé databáze, oprávnění k nim a jejich identifikátory zabezpečení (SID) se zkopírují do kopie databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-115">All database users, their permissions, and their security identifiers (SIDs) are copied to the database copy.</span></span>  

<span data-ttu-id="7570e-116">Při kopírování databáze do různých logického serveru, na novém serveru objekt zabezpečení se změní na vlastníka databáze na novou databázi.</span><span class="sxs-lookup"><span data-stu-id="7570e-116">When you copy a database to a different logical server, the security principal on the new server becomes the database owner on the new database.</span></span> <span data-ttu-id="7570e-117">Pokud používáte [obsažené uživatelů databáze](sql-database-manage-logins.md) pro přístup k datům, zajistěte, že primární a sekundární databáze vždy měl stejných uživatelských přihlašovacích údajů, tak, aby po dokončení kopírování budete mít okamžitě přístup se stejnými pověřeními.</span><span class="sxs-lookup"><span data-stu-id="7570e-117">If you use [contained database users](sql-database-manage-logins.md) for data access, ensure that both the primary and secondary databases always have the same user credentials, so that after the copy is complete you can immediately access it with the same credentials.</span></span> 

<span data-ttu-id="7570e-118">Pokud používáte [Azure Active Directory](../active-directory/active-directory-whatis.md), můžete zcela eliminovat potřebu správy přihlašovací údaje do kopie.</span><span class="sxs-lookup"><span data-stu-id="7570e-118">If you use [Azure Active Directory](../active-directory/active-directory-whatis.md), you can completely eliminate the need for managing credentials in the copy.</span></span> <span data-ttu-id="7570e-119">Ale při kopírování databáze na nový server, přístup na základě přihlášení nemusí fungovat, protože přihlášení nejsou k dispozici na novém serveru.</span><span class="sxs-lookup"><span data-stu-id="7570e-119">However, when you copy the database to a new server, the login-based access might not work, because the logins do not exist on the new server.</span></span> <span data-ttu-id="7570e-120">Další informace o správě přihlášení při kopírovat databázi na jiný logický Server najdete v tématu [jak spravovat zabezpečení databáze Azure SQL po obnovení po havárii](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-120">To learn about managing logins when you copy a database to a different logical server, see [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span> 

<span data-ttu-id="7570e-121">Po úspěšném provedení kopírování a předtím, než jsou přemapování jiných uživatelů, pouze přihlášení, která iniciovala kopírování, vlastník databáze může přihlásit k nové databázi.</span><span class="sxs-lookup"><span data-stu-id="7570e-121">After the copying succeeds and before other users are remapped, only the login that initiated the copying, the database owner, can log in to the new database.</span></span> <span data-ttu-id="7570e-122">Po dokončení operace kopírování, odstranění přihlášení, naleznete v článku [vyřešit přihlášení](#resolve-logins).</span><span class="sxs-lookup"><span data-stu-id="7570e-122">To resolve logins after the copying operation is complete, see [Resolve logins](#resolve-logins).</span></span>

## <a name="copy-a-database-by-using-the-azure-portal"></a><span data-ttu-id="7570e-123">Kopírovat databázi pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7570e-123">Copy a database by using the Azure portal</span></span>

<span data-ttu-id="7570e-124">Kopírování databáze pomocí portálu Azure, otevřete stránku pro vaši databázi a pak klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="7570e-124">To copy a database by using the Azure portal, open the page for your database, and then click **Copy**.</span></span> 

   ![Kopie databáze](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a><span data-ttu-id="7570e-126">Kopírování databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7570e-126">Copy a database by using PowerShell</span></span>

<span data-ttu-id="7570e-127">Při kopírování databáze pomocí prostředí PowerShell, použijte [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7570e-127">To copy a database by using PowerShell, use the [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet.</span></span> 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

<span data-ttu-id="7570e-128">Pro dokončení ukázkový skript, najdete v části [kopírovat databázi na nový server](scripts/sql-database-copy-database-to-new-server-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-128">For a complete sample script, see [Copy a database to a new server](scripts/sql-database-copy-database-to-new-server-powershell.md).</span></span>

## <a name="copy-a-database-by-using-transact-sql"></a><span data-ttu-id="7570e-129">Kopírovat databázi pomocí jazyka Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="7570e-129">Copy a database by using Transact-SQL</span></span>

<span data-ttu-id="7570e-130">Přihlaste se k hlavní databázi pomocí hlavního přihlášení úrovni serveru nebo přihlášení, který vytvořili databázi, kterou chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="7570e-130">Log in to the master database with the server-level principal login or the login that created the database you want to copy.</span></span> <span data-ttu-id="7570e-131">Přihlášení, které nejsou úrovni serveru hlavní databáze kopírování proběhla úspěšně, musí být členy dbmanager role.</span><span class="sxs-lookup"><span data-stu-id="7570e-131">For database copying to succeed, logins that are not the server-level principal must be members of the dbmanager role.</span></span> <span data-ttu-id="7570e-132">Další informace o připojení k serveru a přihlášení najdete v tématu [spravovat přihlášení](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-132">For more information about logins and connecting to the server, see [Manage logins](sql-database-manage-logins.md).</span></span>

<span data-ttu-id="7570e-133">Spusťte kopírování zdrojová databáze s [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7570e-133">Start copying the source database with the [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) statement.</span></span> <span data-ttu-id="7570e-134">Provádění tento příkaz spustí proces kopírování databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-134">Executing this statement initiates the database copying process.</span></span> <span data-ttu-id="7570e-135">Vzhledem k tomu, že asynchronní proces kopírování databáze je, vrátí příkaz CREATE DATABASE před dokončením kopírování databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-135">Because copying a database is an asynchronous process, the CREATE DATABASE statement returns before the database copying is complete.</span></span>

### <a name="copy-a-sql-database-to-the-same-server"></a><span data-ttu-id="7570e-136">Kopírovat databázi SQL na stejném serveru</span><span class="sxs-lookup"><span data-stu-id="7570e-136">Copy a SQL database to the same server</span></span>
<span data-ttu-id="7570e-137">Přihlaste se k hlavní databázi pomocí hlavního přihlášení úrovni serveru nebo přihlášení, který vytvořili databázi, kterou chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="7570e-137">Log in to the master database with the server-level principal login or the login that created the database you want to copy.</span></span> <span data-ttu-id="7570e-138">Přihlášení, které nejsou úrovni serveru hlavní databáze kopírování proběhla úspěšně, musí být členy dbmanager role.</span><span class="sxs-lookup"><span data-stu-id="7570e-138">For database copying to succeed, logins that are not the server-level principal must be members of the dbmanager role.</span></span>

<span data-ttu-id="7570e-139">Tento příkaz zkopíruje Database1 pro novou databázi s názvem Database2 na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="7570e-139">This command copies Database1 to a new database named Database2 on the same server.</span></span> <span data-ttu-id="7570e-140">V závislosti na velikosti vaší databáze kopírování operace může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="7570e-140">Depending on the size of your database, the copying operation might take some time to complete.</span></span>

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a><span data-ttu-id="7570e-141">Kopírovat databázi SQL na jiný server</span><span class="sxs-lookup"><span data-stu-id="7570e-141">Copy a SQL database to a different server</span></span>

<span data-ttu-id="7570e-142">Přihlaste se k databázi hlavního cílového serveru, serveru databáze SQL, kde má být vytvořen nové databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-142">Log in to the master database of the destination server, the SQL database server where the new database is to be created.</span></span> <span data-ttu-id="7570e-143">Pomocí přihlášení, která má stejné jméno a heslo jako vlastník databáze zdrojové databáze na zdrojovém serveru databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="7570e-143">Use a login that has the same name and password as the database owner of the source database on the source SQL database server.</span></span> <span data-ttu-id="7570e-144">Přihlášení na cílovém serveru musí také být členem dbmanager role nebo hlavní přihlášení na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="7570e-144">The login on the destination server must also be a member of the dbmanager role or be the server-level principal login.</span></span>

<span data-ttu-id="7570e-145">Tento příkaz zkopíruje Database1 na serveru1 pro novou databázi s názvem Database2 na serveru2.</span><span class="sxs-lookup"><span data-stu-id="7570e-145">This command copies Database1 on server1 to a new database named Database2 on server2.</span></span> <span data-ttu-id="7570e-146">V závislosti na velikosti vaší databáze kopírování operace může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="7570e-146">Depending on the size of your database, the copying operation might take some time to complete.</span></span>

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a><span data-ttu-id="7570e-147">Sledujte průběh operace kopírování</span><span class="sxs-lookup"><span data-stu-id="7570e-147">Monitor the progress of the copying operation</span></span>

<span data-ttu-id="7570e-148">Monitorujte proces kopírování pomocí dotazu na zobrazení sys.databases a sys.dm_database_copies zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7570e-148">Monitor the copying process by querying the sys.databases and sys.dm_database_copies views.</span></span> <span data-ttu-id="7570e-149">Probíhá kopírování, **state_desc** sloupec zobrazení sys.databases zobrazení pro novou databázi je nastavený na **kopírování**.</span><span class="sxs-lookup"><span data-stu-id="7570e-149">While the copying is in progress, the **state_desc** column of the sys.databases view for the new database is set to **COPYING**.</span></span>

* <span data-ttu-id="7570e-150">Pokud je kopírování selže, **state_desc** sloupec zobrazení sys.databases zobrazení pro novou databázi je nastavený na **podezření**.</span><span class="sxs-lookup"><span data-stu-id="7570e-150">If the copying fails, the **state_desc** column of the sys.databases view for the new database is set to **SUSPECT**.</span></span> <span data-ttu-id="7570e-151">Spusťte příkaz DROP na novou databázi a opakujte akci později.</span><span class="sxs-lookup"><span data-stu-id="7570e-151">Execute the DROP statement on the new database, and try again later.</span></span>
* <span data-ttu-id="7570e-152">Pokud je kopírování úspěšné, **state_desc** sloupec zobrazení sys.databases zobrazení pro novou databázi je nastavený na **ONLINE**.</span><span class="sxs-lookup"><span data-stu-id="7570e-152">If the copying succeeds, the **state_desc** column of the sys.databases view for the new database is set to **ONLINE**.</span></span> <span data-ttu-id="7570e-153">Kopírování je dokončena a nové databáze je standardní databázi, která lze změnit nezávislé na zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-153">The copying is complete, and the new database is a regular database that can be changed independent of the source database.</span></span>

> [!NOTE]
> <span data-ttu-id="7570e-154">Pokud se rozhodnete zrušit kopírování během probíhající, provést [příkazu DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) příkaz na nové databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-154">If you decide to cancel the copying while it is in progress, execute the [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) statement on the new database.</span></span> <span data-ttu-id="7570e-155">Alternativně provádění příkazu DROP DATABASE na zdrojové databáze zruší také proces kopírování.</span><span class="sxs-lookup"><span data-stu-id="7570e-155">Alternatively, executing the DROP DATABASE statement on the source database also cancels the copying process.</span></span>
> 

## <a name="resolve-logins"></a><span data-ttu-id="7570e-156">Vyřešení přihlášení</span><span class="sxs-lookup"><span data-stu-id="7570e-156">Resolve logins</span></span>

<span data-ttu-id="7570e-157">Po nové databáze je online na cílovém serveru, použijte [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) příkaz přemapování uživatelům přihlášení na cílovém serveru z nové databáze.</span><span class="sxs-lookup"><span data-stu-id="7570e-157">After the new database is online on the destination server, use the [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) statement to remap the users from the new database to logins on the destination server.</span></span> <span data-ttu-id="7570e-158">Osamocené uživatele vyřešit, najdete v tématu [řešení potíží s osamocených uživatelů](https://msdn.microsoft.com/library/ms175475.aspx).</span><span class="sxs-lookup"><span data-stu-id="7570e-158">To resolve orphaned users, see [Troubleshoot Orphaned Users](https://msdn.microsoft.com/library/ms175475.aspx).</span></span> <span data-ttu-id="7570e-159">Viz také [jak spravovat zabezpečení databáze Azure SQL po obnovení po havárii](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-159">See also [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span>

<span data-ttu-id="7570e-160">Všichni uživatelé v nové databáze zachovat oprávnění, která měla ve zdrojové databázi.</span><span class="sxs-lookup"><span data-stu-id="7570e-160">All users in the new database retain the permissions that they had in the source database.</span></span> <span data-ttu-id="7570e-161">Uživatel, který zahájil kopírování databáze se stane vlastník databáze nové databáze a je přiřazen nový identifikátor zabezpečení (SID).</span><span class="sxs-lookup"><span data-stu-id="7570e-161">The user who initiated the database copy becomes the database owner of the new database and is assigned a new security identifier (SID).</span></span> <span data-ttu-id="7570e-162">Po úspěšném provedení kopírování a předtím, než jsou přemapování jiných uživatelů, pouze přihlášení, která iniciovala kopírování, vlastník databáze může přihlásit k nové databázi.</span><span class="sxs-lookup"><span data-stu-id="7570e-162">After the copying succeeds and before other users are remapped, only the login that initiated the copying, the database owner, can log in to the new database.</span></span>

<span data-ttu-id="7570e-163">Další informace o správě uživatelů a přihlášení při kopírování databáze do různých logického serveru najdete v tématu [jak spravovat zabezpečení databáze Azure SQL po obnovení po havárii](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-163">To learn about managing users and logins when you copy a database to a different logical server, see [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7570e-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7570e-164">Next steps</span></span>

* <span data-ttu-id="7570e-165">Informace o přihlášení, naleznete v části [spravovat přihlášení](sql-database-manage-logins.md) a [jak spravovat zabezpečení databáze Azure SQL po obnovení po havárii](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-165">For information about logins, see [Manage logins](sql-database-manage-logins.md) and [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span>
* <span data-ttu-id="7570e-166">Exportu databáze, najdete v článku [databázi exportovat do souboru BACPAC](sql-database-export.md).</span><span class="sxs-lookup"><span data-stu-id="7570e-166">To export a database, see [Export the database to a BACPAC](sql-database-export.md).</span></span>