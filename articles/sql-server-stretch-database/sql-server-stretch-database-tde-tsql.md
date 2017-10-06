---
title: "aaaEnable transparentní šifrování dat pro funkci Stretch Database TSQL - Azure | Microsoft Docs"
description: "Povolit transparentní šifrování dat (šifrování TDE) pro SQL Server Stretch Database na Azure TSQL"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
ms.openlocfilehash: a9ba23649656fb344480d79438a1115f0eb353bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a><span data-ttu-id="c7ff4-103">Povolit transparentní šifrování dat (šifrování TDE) pro funkce Stretch Database na Azure (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="c7ff4-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure (Transact-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7ff4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c7ff4-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="c7ff4-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="c7ff4-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="c7ff4-106">Transparentní šifrování šifrování dat (TDE) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-106">Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of hello database, associated backups, and transaction log files at rest without requiring changes toohello application.</span></span>

<span data-ttu-id="c7ff4-107">Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-107">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="c7ff4-108">šifrovací klíč databáze Hello je chráněn certifikát integrovaného serveru.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-108">hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="c7ff4-109">certifikát integrovaného serveru Hello je jedinečný pro každý server Azure.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-109">hello built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="c7ff4-110">Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="c7ff4-111">Obecný popis TDE, najdete v části [transparentní šifrování šifrování dat (TDE)].</span><span class="sxs-lookup"><span data-stu-id="c7ff4-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="c7ff4-112">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="c7ff4-112">Enabling Encryption</span></span>
<span data-ttu-id="c7ff4-113">tooenable TDE pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="c7ff4-113">tooenable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="c7ff4-114">Připojit toohello *hlavní* databáze na hello Azure hostování hello databáze serveru pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello</span><span class="sxs-lookup"><span data-stu-id="c7ff4-114">Connect toohello *master* database on hello Azure server hosting hello database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="c7ff4-115">Spusťte následující příkaz tooencrypt hello databáze hello.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-115">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="c7ff4-116">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="c7ff4-116">Disabling Encryption</span></span>
<span data-ttu-id="c7ff4-117">toodisable TDE pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="c7ff4-117">toodisable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="c7ff4-118">Připojit toohello *hlavní* databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello</span><span class="sxs-lookup"><span data-stu-id="c7ff4-118">Connect toohello *master* database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="c7ff4-119">Spusťte následující příkaz tooencrypt hello databáze hello.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-119">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a><span data-ttu-id="c7ff4-120">Ověření šifrování</span><span class="sxs-lookup"><span data-stu-id="c7ff4-120">Verifying Encryption</span></span>
<span data-ttu-id="c7ff4-121">stav šifrování tooverify pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:</span><span class="sxs-lookup"><span data-stu-id="c7ff4-121">tooverify encryption status for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="c7ff4-122">Připojit toohello *hlavní* nebo instanci databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello</span><span class="sxs-lookup"><span data-stu-id="c7ff4-122">Connect toohello *master* or instance database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="c7ff4-123">Spusťte následující příkaz tooencrypt hello databáze hello.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-123">Execute hello following statement tooencrypt hello database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="c7ff4-124">Důsledkem ```1``` Určuje databázi chráněnou ```0``` Určuje databázi bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="c7ff4-124">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

<!--Anchors-->
[transparentní šifrování šifrování dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
