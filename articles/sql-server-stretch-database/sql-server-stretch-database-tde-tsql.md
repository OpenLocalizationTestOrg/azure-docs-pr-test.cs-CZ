---
title: "Povolit transparentní šifrování dat pro funkce Stretch Database TSQL - Azure | Microsoft Docs"
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
ms.openlocfilehash: ed26c2b386e08b76f78b4a05e12c46d2b97c20f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a><span data-ttu-id="c7c3a-103">Povolit transparentní šifrování dat (šifrování TDE) pro funkce Stretch Database na Azure (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="c7c3a-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure (Transact-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7c3a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c7c3a-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="c7c3a-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="c7c3a-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="c7c3a-106">Transparentní šifrování šifrování dat (TDE) pomáhá chránit před ohrožením škodlivých aktivit provedením v reálném čase šifrování a dešifrování databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-106">Transparent Data Encryption (TDE) helps protect against the threat of malicious activity by performing real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application.</span></span>

<span data-ttu-id="c7c3a-107">Šifrování TDE zašifruje úložiště celé databáze pomocí symetrický klíč s názvem šifrovací klíč databáze.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-107">TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key.</span></span> <span data-ttu-id="c7c3a-108">Šifrovací klíč databáze je chráněn certifikát integrovaného serveru.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-108">The database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="c7c3a-109">Certifikát integrovaného serveru je jedinečný pro každý server Azure.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-109">The built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="c7c3a-110">Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="c7c3a-111">Obecný popis TDE, najdete v části [transparentní šifrování šifrování dat (TDE)].</span><span class="sxs-lookup"><span data-stu-id="c7c3a-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="c7c3a-112">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="c7c3a-112">Enabling Encryption</span></span>
<span data-ttu-id="c7c3a-113">Chcete-li povolit šifrování TDE Azure migrovat databázi, která ukládá data z databáze serveru SQL povolenou funkcí Stretch, provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="c7c3a-113">To enable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="c7c3a-114">Připojení k *hlavní* databáze na serveru Azure, který je hostitelem databáze pomocí přihlášení, který je správcem nebo člen **dbmanager** role v hlavní databázi</span><span class="sxs-lookup"><span data-stu-id="c7c3a-114">Connect to the *master* database on the Azure server hosting the database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="c7c3a-115">Spusťte následující příkaz k šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-115">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="c7c3a-116">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="c7c3a-116">Disabling Encryption</span></span>
<span data-ttu-id="c7c3a-117">Zakázat šifrování TDE Azure migrovat databázi, která ukládá data z databáze povolenou funkcí Stretch SQL Server, provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="c7c3a-117">To disable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="c7c3a-118">Připojení k *hlavní* databáze pomocí přihlášení, který je správcem nebo člen **dbmanager** role v hlavní databázi</span><span class="sxs-lookup"><span data-stu-id="c7c3a-118">Connect to the *master* database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="c7c3a-119">Spusťte následující příkaz k šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-119">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a><span data-ttu-id="c7c3a-120">Ověření šifrování</span><span class="sxs-lookup"><span data-stu-id="c7c3a-120">Verifying Encryption</span></span>
<span data-ttu-id="c7c3a-121">Pokud chcete ověřit, že stav šifrování pro databázi Azure, která ukládá data migrována z databáze povolenou funkcí Stretch SQL serveru, proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="c7c3a-121">To verify encryption status for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="c7c3a-122">Připojení k *hlavní* nebo instanci databáze pomocí přihlášení, který je správcem nebo člen **dbmanager** role v hlavní databázi</span><span class="sxs-lookup"><span data-stu-id="c7c3a-122">Connect to the *master* or instance database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="c7c3a-123">Spusťte následující příkaz k šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-123">Execute the following statement to encrypt the database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="c7c3a-124">Důsledkem ```1``` Určuje databázi chráněnou ```0``` Určuje databázi bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="c7c3a-124">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

<!--Anchors-->
<span data-ttu-id="c7c3a-125">[transparentní šifrování šifrování dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span><span class="sxs-lookup"><span data-stu-id="c7c3a-125">[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span></span>


<!--Image references-->

<!--Link references-->
