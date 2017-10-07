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
# <a name="secure-a-database-in-sql-data-warehouse"></a>Zabezpečení databáze v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
> * [Ověřování](sql-data-warehouse-authentication.md)
> * [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
> * [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Tento článek vás provede základy hello zabezpečení databáze Azure SQL Data Warehouse. Konkrétně tento článek vám pomůžou začít s prostředky pro omezení přístupu, ochrany dat a monitorování aktivit v databázi.

## <a name="connection-security"></a>Zabezpečení připojení
Zabezpečení připojení odkazuje toohow můžete omezit a zabezpečené připojení tooyour databázi pomocí pravidla brány firewall a šifrování připojení.

Pravidla brány firewall jsou používány obě hello serveru a hello databáze tooreject pokusy o připojení z IP adres, které nebyly explicitně seznam povolených adres. tooallow připojení z vaší aplikace nebo klientský počítač veřejnou IP adresu, musíte nejdřív vytvořit pravidlo brány firewall na úrovni serveru pomocí hello portálu Azure, rozhraní API REST nebo PowerShell. Jako osvědčený postup měli omezit hello rozsahy IP adres povoleny prostřednictvím brány firewall serveru co nejvíc.  tooaccess Azure SQL Data Warehouse z místního počítače, ujistěte se, že brána firewall hello na vaší sítí a místním počítači umožňuje odchozí komunikaci na portu TCP 1433.  Další informace najdete v tématu [brány firewall databáze Azure SQL Database][Azure SQL Database firewall], [příkaz sp_set_firewall_rule][sp_set_firewall_rule], a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Ve výchozím nastavení jsou šifrované připojení tooyour SQL Data Warehouse.  Úprava nastavení toodisable připojení šifrování se ignorují.

## <a name="authentication"></a>Authentication
Ověřování odkazuje toohow prokázání své identity při připojování toohello databáze. SQL Data Warehouse aktuálně podporuje ověřování systému SQL Server s uživatelským jménem a heslem, jakož i Azure Active Directory. 

Když vytvoříte hello logický server pro databázi, jste zadali "serveru admin" přihlášení s uživatelské jméno a heslo. Pomocí těchto přihlašovacích údajů, můžete ověřovat tooany databáze na tomto serveru jako vlastník databáze hello nebo "dbo" pomocí ověřování systému SQL Server.

Jako osvědčený postup, ale měli uživatele ve vaší organizaci použít tooauthenticate jiný účet. Tímto způsobem můžete omezit hello oprávnění udělená aplikaci toohello a snížení rizika hello škodlivých aktivit v případě kód aplikace je snadno napadnutelný tooa útok prostřednictvím injektáže SQL. 

toocreate uživatel ověřený Server SQL, připojit toohello **hlavní** databáze na serveru s vaše přihlašovací jméno správce serveru a vytvořte nové přihlašovací údaje serveru.  Kromě toho je vhodné toocreate uživatele v hlavní databázi hello pro uživatele Azure SQL Data Warehouse. Vytváření uživatele v předloze umožňuje uživatele toologin, pomocí nástroje, například aplikace SSMS bez zadání názvu databáze.  Umožňuje také jejich toouse hello object explorer tooview všechny databáze na serveru SQL server.

```sql
-- Connect toomaster database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Připojte tooyour **databázi SQL Data Warehouse** s vaše přihlašovací jméno správce serveru a vytvořte uživatele databáze založené na přihlášení server hello jste právě vytvořili.

```sql
-- Connect tooSQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Pokud uživatel bude to další operace, jako je vytvoření přihlášení nebo vytvoření nových databází, budou také potřebovat toobe přiřazené toohello `Loginmanager` a `dbmanager` role v hlavní databázi hello. Další informace o těchto dalších rolí a ověřovací tooa SQL Database, najdete v části [Správa databází a přihlašovacích údajů ve službě Azure SQL Database][Managing databases and logins in Azure SQL Database].  Další informace o službě Azure AD pro SQL Data Warehouse, najdete v části [připojení tooSQL datového skladu pomocí pomocí Azure Active Directory, ověřování][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Autorizace
Autorizace odkazuje toowhat můžete provést v rámci databázi Azure SQL Data Warehouse, a tím je řízeno oprávnění a členství v rolích váš uživatelský účet. Jako osvědčený postup byste měli udělit uživatelům hello nejnižší oprávnění potřebná. Azure SQL Data Warehouse umožňuje tento snadno toomanage s rolemi v T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser tooread data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser toowrite data
```

Hello účet správce serveru, ke kterému se připojujete se členem role db_owner, který má autority toodo nic v rámci hello databáze. Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy. Použít účet "ApplicationUser" hello s omezenější tooconnect oprávnění z vaší aplikace toohello databáze pomocí hello nejnižších oprávnění vyžadované vaší aplikace.

Existují způsoby toofurther limit co může uživatel provádět s Azure SQL Database:

* Podrobné [oprávnění] [ Permissions] umožňují řízení operací, které můžete u jednotlivých sloupců tabulky, zobrazení, procedury a další objekty v databázi hello. Použití oprávnění na podrobné úrovni toohave hello většina řízení a přidělte minimální oprávnění hello nezbytné. systém granulární oprávnění Hello je poněkud složité a bude vyžadovat některé studie toouse efektivně.
* [Rolí databáze] [ Database roles] jiného, než db_datareader a db_datawriter můžou být použité toocreate výkonnější aplikace uživatelské účty nebo méně výkonná účty pro správu. Hello předdefinované pevné databázové role poskytují toogrant oprávnění snadný způsob, ale může mít za následek přidělení více oprávnění, než je potřeba.
* [Uložené procedury] [ Stored procedures] lze použít toolimit hello akce, které můžete provést na databázi hello.

Správa databází a logické servery z hello portálu Azure Classic nebo pomocí rozhraní API služby Azure Resource Manager hello řídí přiřazení rolí portálu uživatelského účtu. Další informace v tomto tématu najdete v tématu [řízení přístupu na základě Role v Azure Portal][Role-based access control in Azure Portal].

## <a name="encryption"></a>Šifrování
Azure SQL Data Warehouse transparentní dat šifrování (TDE) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování dat v klidovém stavu.  Při šifrování databáze, přidružených záloh a souborů protokolů transakci jsou šifrované bez nutnosti jakékoli změny tooyour aplikace. Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze. V databázi SQL hello databáze šifrovací klíč je chráněný certifikát integrovaného serveru. certifikát integrovaného serveru Hello je jedinečný pro každý server databáze SQL. Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní. Hello šifrovacího algoritmu používaného funkcí SQL Data Warehouse je AES 256. Obecný popis TDE, najdete v části [transparentní šifrování dat][Transparent Data Encryption].

Můžete šifrovat databázi pomocí hello [portálu Azure] [ Encryption with Portal] nebo [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Další kroky
Podrobnosti a příklady o připojení tooyour SQL Data Warehouse pomocí různých protokolů najdete v tématu [připojit tooSQL datového skladu][Connect tooSQL Data Warehouse].

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
