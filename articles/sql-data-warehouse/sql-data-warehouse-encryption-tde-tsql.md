---
title: "Transparentní šifrování dat v datovém skladu SQL (T-SQL) | Microsoft Docs"
description: "Transparentní šifrování dat (šifrování TDE) v datovém skladu SQL (T-SQL)"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 74c9032aababdce91ed617cd7a4c628915b42504
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a><span data-ttu-id="10d04-103">Začínáme s transparentní šifrování dat (TDE)</span><span class="sxs-lookup"><span data-stu-id="10d04-103">Get started with Transparent Data Encryption (TDE)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="10d04-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="10d04-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="10d04-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="10d04-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="10d04-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="10d04-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="10d04-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="10d04-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="10d04-108">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="10d04-108">Required Permssions</span></span>
<span data-ttu-id="10d04-109">Pokud chcete povolit šifrování transparentní dat (TDE), musíte být správce nebo člen dbmanager role.</span><span class="sxs-lookup"><span data-stu-id="10d04-109">To enable Transparent Data Encryption (TDE), you must be an administrator or a member of the dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="10d04-110">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="10d04-110">Enabling Encryption</span></span>
<span data-ttu-id="10d04-111">Postupujte podle těchto kroků, které chcete povolit šifrování TDE pro SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="10d04-111">Follow these steps to enable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="10d04-112">Připojení k *hlavní* databáze na serveru, který hostuje databázi pomocí přihlášení, které je správce nebo člen **dbmanager** role v hlavní databázi</span><span class="sxs-lookup"><span data-stu-id="10d04-112">Connect to the *master* database on the server hosting the database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="10d04-113">Spusťte následující příkaz k šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="10d04-113">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="10d04-114">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="10d04-114">Disabling Encryption</span></span>
<span data-ttu-id="10d04-115">Postupujte podle těchto kroků zakázat šifrování TDE pro SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="10d04-115">Follow these steps to disable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="10d04-116">Připojení k *hlavní* databáze pomocí přihlášení, který je správcem nebo člen **dbmanager** role v hlavní databázi</span><span class="sxs-lookup"><span data-stu-id="10d04-116">Connect to the *master* database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="10d04-117">Spusťte následující příkaz k šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="10d04-117">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> <span data-ttu-id="10d04-118">Pozastavený SQL Data Warehouse musí být obnoven, před prováděním změn nastavení šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="10d04-118">A paused SQL Data Warehouse must be resumed before making changes to the TDE settings.</span></span>
> 
> 

## <a name="verifying-encryption"></a><span data-ttu-id="10d04-119">Ověření šifrování</span><span class="sxs-lookup"><span data-stu-id="10d04-119">Verifying Encryption</span></span>
<span data-ttu-id="10d04-120">Pokud chcete ověřit stav šifrování pro SQL Data Warehouse, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="10d04-120">To verify encryption status for a SQL Data Warehouse, follow the steps below:</span></span>

1. <span data-ttu-id="10d04-121">Připojení k *hlavní* nebo instanci databáze pomocí přihlášení, který je správcem nebo člen **dbmanager** role v hlavní databázi</span><span class="sxs-lookup"><span data-stu-id="10d04-121">Connect to the *master* or instance database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="10d04-122">Spusťte následující příkaz k šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="10d04-122">Execute the following statement to encrypt the database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="10d04-123">Důsledkem ```1``` Určuje databázi chráněnou ```0``` Určuje databázi bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="10d04-123">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

## <a name="encryption-dmvs"></a><span data-ttu-id="10d04-124">Šifrování zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="10d04-124">Encryption DMVs</span></span>
* <span data-ttu-id="10d04-125">[zobrazení Sys.Databases][sys.databases]</span><span class="sxs-lookup"><span data-stu-id="10d04-125">[sys.databases][sys.databases]</span></span> 
* <span data-ttu-id="10d04-126">[Sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="10d04-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
