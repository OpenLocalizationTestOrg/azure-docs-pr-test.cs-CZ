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
# <a name="get-started-with-transparent-data-encryption-tde"></a>Začínáme s transparentní šifrování dat (TDE)
> [!div class="op_single_selector"]
> * [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
> * [Ověřování](sql-data-warehouse-authentication.md)
> * [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
> * [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Požadovaná oprávnění
tooenable transparentní dat šifrování (šifrování TDE), musíte být správce nebo člen hello dbmanager role.

## <a name="enabling-encryption"></a>Povolení šifrování
Postupujte podle těchto kroků tooenable TDE pro SQL Data Warehouse:

1. Připojit toohello *hlavní* databáze na serveru hello, který je hostitelem databáze hello pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello
2. Spusťte následující příkaz tooencrypt hello databáze hello.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Zakázáním šifrování
Postupujte podle těchto kroků toodisable TDE pro SQL Data Warehouse:

1. Připojit toohello *hlavní* databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello
2. Spusťte následující příkaz tooencrypt hello databáze hello.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Pozastavený SQL Data Warehouse musí obnovit před provedením změn nastavení šifrování TDE toohello.
> 
> 

## <a name="verifying-encryption"></a>Ověření šifrování
stav šifrování tooverify pro SQL Data Warehouse, postupujte podle kroků hello níže:

1. Připojit toohello *hlavní* nebo instanci databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello
2. Spusťte následující příkaz tooencrypt hello databáze hello.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Důsledkem ```1``` Určuje databázi chráněnou ```0``` Určuje databázi bez šifrování.

## <a name="encryption-dmvs"></a>Šifrování zobrazení dynamické správy
* [zobrazení Sys.Databases][sys.databases] 
* [Sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
