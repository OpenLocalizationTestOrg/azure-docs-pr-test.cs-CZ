---
title: aaaCopy Azure SQL database | Microsoft Docs
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
ms.openlocfilehash: 64a297d819d6da89600fda60abe8394ae405abfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-an-azure-sql-database"></a>Kopírování databáze Azure SQL

Databáze SQL Azure poskytuje několik metod pro vytváření stavu transakční konzistence kopie stávající Azure SQL databáze na buď hello stejný server nebo jiný server. Můžete kopírovat databázi SQL s využitím hello portálu Azure, PowerShell nebo T-SQL. 

## <a name="overview"></a>Přehled

Kopie databáze je snímek hello zdrojové databáze od hello čas požadavku kopírování hello. Můžete vybrat hello stejný server nebo jiný server, její úroveň služby a výkonu úroveň nebo úroveň různých výkonu v rámci hello stejné služby vrstvě (edice). Po dokončení kopírování hello stane plně funkční, nezávislé databáze. V tomto okamžiku můžete upgradovat nebo starší verzi tooany edition. Hello přihlášení, uživatele a oprávnění lze spravovat nezávisle.  

## <a name="logins-in-hello-database-copy"></a>Přihlášení v kopii databáze hello

Při kopírování databáze toohello stejného logického serveru, hello stejné přihlášení lze použít v obou databází. objekt, že používáte toocopy hello databázi Hello zabezpečení se změní na vlastníka databáze hello na hello novou databázi. Všichni uživatelé databáze, oprávnění k nim a jejich zabezpečení, které jsou identifikátory (SID) zkopírovat toohello kopie databáze.  

Při kopírování databáze tooa jiný logický server objektu na nový server hello hello zabezpečení se změní na vlastníka databáze hello na hello novou databázi. Pokud používáte [obsažené uživatelů databáze](sql-database-manage-logins.md) pro přístup k datům, zajistěte, aby obě hello primární a sekundární databáze mají vždycky hello okamžitě přístup stejné pověření uživatele, který po hello kopie dokončeno, bude její stejné hello přihlašovací údaje. 

Pokud používáte [Azure Active Directory](../active-directory/active-directory-whatis.md), můžete zcela eliminovat potřebu hello správy přihlašovací údaje v hello kopírování. Ale při kopírování hello databáze tooa nový server, přístup na základě přihlášení hello nemusí fungovat, protože hello přihlášení nejsou k dispozici na nový server hello. toolearn o správě přihlášení při kopírování databáze tooa jiný logický server, najdete v části [jak toomanage Azure SQL databáze zabezpečení po zotavení po havárii](sql-database-geo-replication-security-config.md). 

Po úspěšném provedení hello kopírování a předtím, než jsou přemapování jiných uživatelů, hello přihlášení, která iniciovala hello kopírování, hello vlastníka databáze, mohou přihlašovat pouze toohello novou databázi. najdete v části tooresolve přihlášení po dokončení operace kopírování hello [vyřešit přihlášení](#resolve-logins).

## <a name="copy-a-database-by-using-hello-azure-portal"></a>Kopírovat databázi pomocí hello portálu Azure

toocopy databáze pomocí hello portál Azure, otevřete hello stránky pro vaši databázi a potom klikněte na **kopie**. 

   ![Kopie databáze](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Kopírování databáze pomocí prostředí PowerShell

toocopy databáze pomocí prostředí PowerShell, použijte hello [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) rutiny. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Pro dokončení ukázkový skript, najdete v části [zkopírujte nový server databáze tooa](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Kopírovat databázi pomocí jazyka Transact-SQL

Přihlaste se toohello hlavní databáze s hello hlavní přihlášení na úrovni serveru nebo hello přihlášení, které vytvořil hello databázi chcete toocopy. Pro databázi kopírování toosucceed přihlášení, které nejsou hlavním přihlášením na úrovni serveru hello musí být členy role dbmanager hello. Další informace o připojování toohello serveru a přihlášení najdete v tématu [spravovat přihlášení](sql-database-manage-logins.md).

Spusťte kopírování hello zdrojová databáze s hello [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) příkaz. Provádění tohoto příkaz zahájí proces kopírování databáze hello. Vzhledem k tomu, že asynchronní proces kopírování databáze je, vrátí hello příkaz CREATE DATABASE před dokončením kopírování databáze hello.

### <a name="copy-a-sql-database-toohello-same-server"></a>Zkopírujte toohello databáze SQL stejný server
Přihlaste se toohello hlavní databáze s hello hlavní přihlášení na úrovni serveru nebo hello přihlášení, které vytvořil hello databázi chcete toocopy. Pro databázi kopírování toosucceed přihlášení, které nejsou hlavním přihlášením na úrovni serveru hello musí být členy role dbmanager hello.

Tento příkaz zkopíruje Database1 tooa novou databázi s názvem Database2 na hello stejný server. V závislosti na velikosti hello vaší databáze může trvat hello kopírování operace některé toocomplete čas.

    -- Execute on hello master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-tooa-different-server"></a>Zkopírujte jiný server SQL databáze tooa

Přihlaste se toohello hlavní databáze hello cílového serveru, kde je toobe vytvoření nové databáze hello hello databázového serveru SQL. Pomocí přihlášení, která má jako vlastníka databáze hello hello zdrojové databáze na serveru databáze SQL zdroje hello hello stejné jméno a heslo. Hello přihlášení na cílovém serveru hello musí také být členem hello dbmanager role nebo hello hlavní přihlášení na úrovni serveru.

Tento příkaz kopií Database1 server1 tooa novou databázi s názvem Database2 na serveru2. V závislosti na velikosti hello vaší databáze může trvat hello kopírování operace některé toocomplete čas.

    -- Execute on hello master database of hello target server (server2)
    -- Start copying from Server1 tooServer2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-hello-progress-of-hello-copying-operation"></a>Monitorovat průběh hello hello operace kopírování

Sledování procesu kopírování hello pomocí dotazu na zobrazení zobrazení sys.databases a sys.dm_database_copies hello. Při kopírování hello probíhá, hello **state_desc** sloupec hello zobrazení sys.databases zobrazení pro novou databázi hello je nastaven příliš**kopírování**.

* V případě selhání kopírování hello hello **state_desc** sloupec hello zobrazení sys.databases zobrazení pro novou databázi hello je nastaven příliš**podezření**. Provést hello příkaz DROP hello novou databázi a opakujte akci později.
* Pokud kopírování hello úspěšné, hello **state_desc** sloupec hello zobrazení sys.databases zobrazení pro novou databázi hello je nastaven příliš**ONLINE**. kopírování Hello je dokončena a novou databázi hello je standardní databázi, která lze změnit nezávislé hello zdrojové databáze.

> [!NOTE]
> Pokud se rozhodnete toocancel hello kopírování během probíhající, provést hello [příkazu DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) příkaz na hello novou databázi. Alternativně provádění příkazu DROP DATABASE hello ve zdrojové databázi hello zruší také hello kopírování procesu.
> 

## <a name="resolve-logins"></a>Vyřešení přihlášení

Po online na cílovém serveru hello hello novou databázi, použijte hello [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) příkaz tooremap hello uživatele z hello nové databáze toologins na cílovém serveru hello. tooresolve osamocené uživatele, najdete v části [řešení potíží s osamocených uživatelů](https://msdn.microsoft.com/library/ms175475.aspx). Viz také [jak toomanage Azure SQL databáze zabezpečení po zotavení po havárii](sql-database-geo-replication-security-config.md).

Všichni uživatelé v hello novou databázi zachovat hello oprávnění, která měly v hello zdrojové databáze. Hello uživatel, který zahájil kopie databáze hello se změní na vlastníka databáze hello hello nové databáze a je přiřazen nový identifikátor zabezpečení (SID). Po úspěšném provedení hello kopírování a předtím, než jsou přemapování jiných uživatelů, hello přihlášení, která iniciovala hello kopírování, hello vlastníka databáze, mohou přihlašovat pouze toohello novou databázi.

toolearn o správě uživatelů a přihlášení při kopírování databáze tooa jiný logický server, najdete v části [jak toomanage Azure SQL databáze zabezpečení po zotavení po havárii](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Další kroky

* Informace o přihlášení, naleznete v části [spravovat přihlášení](sql-database-manage-logins.md) a [jak toomanage Azure SQL databáze zabezpečení po zotavení po havárii](sql-database-geo-replication-security-config.md).
* tooexport databáze, najdete v části [exportovat hello databáze tooa souboru BACPAC](sql-database-export.md).
