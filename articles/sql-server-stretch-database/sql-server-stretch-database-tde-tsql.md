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
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Povolit transparentní šifrování dat (šifrování TDE) pro funkce Stretch Database na Azure (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparentní šifrování šifrování dat (TDE) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny toohello aplikace.

Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze. šifrovací klíč databáze Hello je chráněn certifikát integrovaného serveru. certifikát integrovaného serveru Hello je jedinečný pro každý server Azure. Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní. Obecný popis TDE, najdete v části [transparentní šifrování šifrování dat (TDE)].

## <a name="enabling-encryption"></a>Povolení šifrování
tooenable TDE pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:

1. Připojit toohello *hlavní* databáze na hello Azure hostování hello databáze serveru pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello
2. Spusťte následující příkaz tooencrypt hello databáze hello.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Zakázáním šifrování
toodisable TDE pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:

1. Připojit toohello *hlavní* databáze pomocí přihlášení, která je správce nebo členem hello **dbmanager** role v hlavní databázi hello
2. Spusťte následující příkaz tooencrypt hello databáze hello.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Ověření šifrování
stav šifrování tooverify pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:

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

<!--Anchors-->
[transparentní šifrování šifrování dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
