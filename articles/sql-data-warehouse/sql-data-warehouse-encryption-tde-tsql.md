---
title: "aaaTransparent šifrování dat v datovém skladu SQL (T-SQL) | Microsoft Docs"
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
ms.openlocfilehash: 3894431c76f14b217f3a6b9a42dbf2f4d216bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a><span data-ttu-id="31a5b-103">Začínáme s transparentní šifrování dat (TDE)</span><span class="sxs-lookup"><span data-stu-id="31a5b-103">Get started with Transparent Data Encryption (TDE)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="31a5b-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="31a5b-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="31a5b-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="31a5b-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="31a5b-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="31a5b-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="31a5b-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="31a5b-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="31a5b-108">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="31a5b-108">Required Permssions</span></span>
<span data-ttu-id="31a5b-109">tooenable transparentní dat šifrování (šifrování TDE), musíte být správce nebo člen hello dbmanager role.</span><span class="sxs-lookup"><span data-stu-id="31a5b-109">tooenable Transparent Data Encryption (TDE), you must be an administrator or a member of hello dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="31a5b-110">Povolení šifrování</span><span class="sxs-lookup"><span data-stu-id="31a5b-110">Enabling Encryption</span></span>
<span data-ttu-id="31a5b-111">Postupujte podle těchto kroků tooenable TDE pro SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="31a5b-111">Follow these steps tooenable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="31a5b-112">Připojit toohello *hlavní* databáze na serveru hello, který je hostitelem databáze hello pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello</span><span class="sxs-lookup"><span data-stu-id="31a5b-112">Connect toohello *master* database on hello server hosting hello database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="31a5b-113">Spusťte následující příkaz tooencrypt hello databáze hello.</span><span class="sxs-lookup"><span data-stu-id="31a5b-113">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="31a5b-114">Zakázáním šifrování</span><span class="sxs-lookup"><span data-stu-id="31a5b-114">Disabling Encryption</span></span>
<span data-ttu-id="31a5b-115">Postupujte podle těchto kroků toodisable TDE pro SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="31a5b-115">Follow these steps toodisable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="31a5b-116">Připojit toohello *hlavní* databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello</span><span class="sxs-lookup"><span data-stu-id="31a5b-116">Connect toohello *master* database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="31a5b-117">Spusťte následující příkaz tooencrypt hello databáze hello.</span><span class="sxs-lookup"><span data-stu-id="31a5b-117">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> <span data-ttu-id="31a5b-118">Pozastavený SQL Data Warehouse musí obnovit před provedením změn nastavení šifrování TDE toohello.</span><span class="sxs-lookup"><span data-stu-id="31a5b-118">A paused SQL Data Warehouse must be resumed before making changes toohello TDE settings.</span></span>
> 
> 

## <a name="verifying-encryption"></a><span data-ttu-id="31a5b-119">Ověření šifrování</span><span class="sxs-lookup"><span data-stu-id="31a5b-119">Verifying Encryption</span></span>
<span data-ttu-id="31a5b-120">stav šifrování tooverify pro SQL Data Warehouse, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="31a5b-120">tooverify encryption status for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="31a5b-121">Připojit toohello *hlavní* nebo instanci databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello</span><span class="sxs-lookup"><span data-stu-id="31a5b-121">Connect toohello *master* or instance database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="31a5b-122">Spusťte následující příkaz tooencrypt hello databáze hello.</span><span class="sxs-lookup"><span data-stu-id="31a5b-122">Execute hello following statement tooencrypt hello database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="31a5b-123">Důsledkem ```1``` Určuje databázi chráněnou ```0``` Určuje databázi bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="31a5b-123">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

## <a name="encryption-dmvs"></a><span data-ttu-id="31a5b-124">Šifrování zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="31a5b-124">Encryption DMVs</span></span>
* <span data-ttu-id="31a5b-125">[zobrazení Sys.Databases][sys.databases]</span><span class="sxs-lookup"><span data-stu-id="31a5b-125">[sys.databases][sys.databases]</span></span> 
* <span data-ttu-id="31a5b-126">[Sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="31a5b-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
