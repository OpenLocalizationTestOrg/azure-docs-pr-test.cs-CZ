---
title: aaaConnect tooAzure SQL Data Warehouse sqlcmd | Microsoft Docs
description: "Použijte [sqlcmd] [sqlcmd] nástroj příkazového řádku tooconnect tooand dotaz Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 0334df7b969da1966ba29c97f835a2dc9e383e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sqlcmd"></a>Připojit tooSQL datového skladu pomocí sqlcmd
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Použití [sqlcmd] [ sqlcmd] tooand tooconnect nástroj příkazového řádku dotaz Azure SQL Data Warehouse.  

## <a name="1-connect"></a>1. Připojení
tooget začít s [sqlcmd][sqlcmd], otevřete hello příkazový řádek a zadejte **sqlcmd** následuje hello připojovací řetězec pro vaši databázi SQL Data Warehouse. Hello připojovací řetězec potřebuje hello následující parametry:

* **Server (-S):** serveru v podobě hello `<`název serveru`>`. database.windows.net
* **Database (-d):** Název databáze
* **Enable Quoted Identifiers (-I):** identifikátory v uvozovkách, musí být povoleno tooconnect instanci SQL Data Warehouse tooa.

toouse ověřování systému SQL Server, je třeba tooadd hello uživatelského jména a hesla parametry:

* **Uživatel (-U):** uživatel serveru v podobě hello `<`uživatele`>`
* **Heslo (-P):** heslo, které jsou přidružené uživateli hello.

Připojovací řetězec může například vypadat hello následující:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory integrované ověřování toouse, je třeba tooadd hello Azure Active Directory parametry:

* **Azure Active Directory Authentication (-G):** Pro ověřování používat Azure Active Directory

Připojovací řetězec může například vypadat hello následující:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Je třeba příliš[povolení ověřování Azure Active Directory](sql-data-warehouse-authentication.md) tooauthenticate pomocí služby Active Directory.
> 
> 

## <a name="2-query"></a>2. Dotaz
Po připojení můžete použít všechny podporované příkazy jazyka Transact-SQL proti hello.  V tomto příkladu jsou dotazy zadávány v interaktivním režimu.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Tyto další příklady ukazují, jak můžete své dotazy spustit v dávkovém režimu pomocí možnosti -Q hello nebo potrubí vaší toosqlcmd SQL.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Další kroky
V tématu [dokumentaci k sqlcmd] [ sqlcmd] Další informace o podrobnosti o možnostech hello k dispozici v sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
