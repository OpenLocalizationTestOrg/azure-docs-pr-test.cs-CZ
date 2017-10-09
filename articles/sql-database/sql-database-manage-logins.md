---
title: "aaaAzure SQL přihlášení a uživatele | Microsoft Docs"
description: "Další informace o správě zabezpečení SQL Database, konkrétně jak toomanage databáze zabezpečení přístupu a přihlášení prostřednictvím účtu hlavní hello úrovni serveru."
keywords: "zabezpečení databáze SQL,správa zabezpečení databáze,zabezpečení přihlášení,zabezpečení databáze,přístup k databázi"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 0a65a93f-d5dc-424b-a774-7ed62d996f8c
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/23/2017
ms.author: rickbyh
ms.openlocfilehash: dff889b9fed09146a10008c0d11ca9e71d91df5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="controlling-and-granting-database-access"></a>Řízení a udělování přístupu k databázi

Když nakonfigurovaná pravidla brány firewall, může osoby připojit tooa SQL Database jako jeden z účtů správce hello, jako vlastník databáze hello nebo jako uživatel databáze v databázi hello.  

>  [!NOTE]  
>  Toto téma se týká tooAzure SQL server a databáze SQL Database a SQL Data Warehouse tooboth, které jsou vytvořené na server Azure SQL hello. Pro jednoduchost databáze SQL se používá k odkazování tooboth SQL Database a SQL Data Warehouse. 
>

> [!TIP]
> Podívejte se kurz [zabezpečení vaší databázi SQL Azure](sql-database-security-tutorial.md).
>


## <a name="unrestricted-administrative-accounts"></a>Neomezené účty pro správu
Jako správci fungují dva účty pro správu (**Správce serveru** a **Správce Active Directory**). Otevřete tyto účty správce pro SQL server, tooidentify hello portál Azure a přejděte toohello vlastnosti systému SQL server.

![Správci SQL serveru](./media/sql-database-manage-logins/sql-admins.png)

- **Správce serveru**   
Když vytvoříte Azure SQL server, musíte určit **Přihlášení správce serveru**. Systému SQL server vytvoří tento účet jako přihlašovací údaje v hlavní databázi hello. Tento účet používá pro připojení ověřování SQL Serveru (uživatelské jméno a heslo). Existovat může jenom jeden z těchto účtů.   
- **Správce Azure Active Directory**   
Jako správce je možné nakonfigurovat jeden účet Azure Active Directory, a to buď individuální účet, nebo účet skupiny zabezpečení. Je volitelné tooconfigure správce Azure AD, ale musí být nakonfigurované správce Azure AD, pokud chcete, aby toouse Azure AD účty tooconnect tooSQL databáze. Další informace o konfiguraci Azure Active Directory přístup, najdete v části [tooSQL připojení databáze nebo SQL Data Warehouse pomocí pomocí Azure ověřování Active Directory](sql-database-aad-authentication.md) a [SSMS podpora pro Azure AD MFA s SQL Databáze a SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).
 

Hello **správce serveru** a **správce Azure AD** účty má hello následující vlastnosti:
- Toto jsou hello pouze účty, které můžou automaticky připojit tooany SQL Database na serveru hello. (tooconnect tooa uživatele databáze, jiné účty musí být buď musí vlastník hello hello databáze, nebo musí mít uživatelský účet v hello uživatelské databáze.)
- Tyto účty zadejte uživatelských databází jako hello `dbo` uživatele a mají všechna oprávnění hello v hello uživatelské databáze. (hello vlastník databáze uživatel také zadá hello databáze jako hello `dbo` uživatele.) 
- Tyto účty nezadávejte hello `master` databáze jako hello `dbo` uživatele a mají omezenou oprávnění v předloze. 
- Tyto účty nejsou členy hello standard systému SQL Server `sysadmin` pevné role serveru, která není k dispozici v databázi SQL.  
- Tyto účty mohou vytvářet, měnit a odstraňovat databáze, přihlášení, uživatele hlavní databáze a pravidla brány firewall na úrovni serveru.
- Tyto účty můžete přidávat a odebírat členy toohello `dbmanager` a `loginmanager` role.
- Tyto účty můžete zobrazit hello `sys.sql_logins` systémové tabulky.

### <a name="configuring-hello-firewall"></a>Konfigurace brány firewall hello
Pokud brána firewall na úrovni serveru hello je konfigurován pro jednotlivé IP adresy nebo rozsahu, hello **správce serveru SQL** a hello **správce Azure Active Directory** může připojit toohello hlavní databázi a všechny hello uživatelské databáze. Počáteční Hello brány firewall na úrovni serveru můžete nakonfigurovat přes hello [portál Azure](sql-database-get-started-portal.md)pomocí [prostředí PowerShell](sql-database-get-started-powershell.md) nebo pomocí hello [REST API](https://msdn.microsoft.com/library/azure/dn505712.aspx). Po vytvoření připojení můžete konfigurovat další pravidla brány firewall na úrovni serveru také pomocí jazyka [Transact-SQL](sql-database-configure-firewall-settings.md).

### <a name="administrator-access-path"></a>Cesta pro přístup správce
Pokud brána firewall na úrovni serveru hello je správně nakonfigurovaná, hello **správce serveru SQL** a hello **správce Azure Active Directory** se mohou připojit pomocí klienta nástroje, jako je SQL Server Management Studio nebo SQL Server Data Tools. Jenom nejnovější nástroje pro hello poskytují všechny hello funkce a možnosti. Hello následující diagram znázorňuje typickou konfiguraci pro hello dva účty správce.

![Cesta pro přístup správce](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

Pokud používáte k portu v bráně firewall hello úrovni serveru, se mohou připojovat správci tooany databáze SQL.

### <a name="connecting-tooa-database-by-using-sql-server-management-studio"></a>Připojení databáze tooa pomocí SQL Server Management Studio
Návod při vytváření serveru, databáze a pravidla brány firewall na úrovni serveru a pomocí SQL Server Management Studio tooquery databáze, najdete v části [Začínáme s Azure SQL Database servery, databáze a pravidla brány firewall pomocí hello portálu Azure a SQL Server Management Studio](sql-database-get-started-portal.md).

> [!IMPORTANT]
> Se doporučuje hello vždy použít nejnovější verzi tooremain Management Studio synchronizovat se službou aktualizace tooMicrosoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="additional-server-level-administrative-roles"></a>Další správní role na úrovni serveru
Kromě toho toohello úrovni serveru pro správu rolí jak jsme vysvětlili výše, SQL Database nabízí dvě s omezeným přístupem pro správu role v hello hlavní databázi toowhich uživatelské účty můžete přidat, udělte oprávnění tooeither vytváření databází nebo spravovat přihlášení.

### <a name="database-creators"></a>Autoři databází
Jeden z těchto rolí pro správu je hello **dbmanager** role. Členové této role mohou vytvářet nové databáze. toouse této role, vytvořte uživatele v hello `master` databáze a poté přidejte hello uživatele toohello **dbmanager** role databáze. toocreate databáze, hello uživatel musí být uživatel založené na přihlášení systému SQL Server v hlavní databázi hello nebo obsažené uživatel databáze založené na uživatele služby Azure Active Directory.

1. Pomocí účtu správce, připojte toohello hlavní databázi.
2. Volitelný krok: Vytvořte přihlášení ověřování systému SQL Server, pomocí hello [vytvořit přihlášení](https://msdn.microsoft.com/library/ms189751.aspx) příkaz. Ukázka příkazu:
   
   ```
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```
   
   > [!NOTE]
   > Při vytváření přihlášení nebo uživatele databáze s omezením použijte silné heslo. Další informace najdete v tématu [Silná hesla](https://msdn.microsoft.com/library/ms161962.aspx).
    
   výkon tooimprove přihlášení (objekty úrovni serveru) jsou dočasně uloženy v mezipaměti na úrovni databáze hello. toorefresh hello ověřování mezipaměti, najdete v části [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

3. V hlavní databázi hello, vytvořte uživatele pomocí hello [vytvořit uživatele](https://msdn.microsoft.com/library/ms173463.aspx) příkaz. Hello uživatel může být uživatel databáze obsažené ověřování Azure Active Directory, (Pokud jste nakonfigurovali prostředí pro ověřování Azure AD), nebo uživatel ověřování obsažené databáze systému SQL Server nebo uživatele ověřování systému SQL Server podle SQL Přihlášení k serveru ověřování (vytvořili v předchozím kroku hello.) Ukázky příkazů:
   
   ```
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
   CREATE USER Tran WITH PASSWORD = '<strong_password>';
   CREATE USER Mary FROM LOGIN Mary; 
   ```

4. Přidání nového uživatele hello toohello **dbmanager** databázové role pomocí hello [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx) příkaz. Ukázky příkazů:
   
   ```
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```
   
   > [!NOTE]
   > Hello dbmanager je role databáze v hlavní databázi, takže lze přidat pouze role dbmanager toohello uživatele databáze. Nelze přidat roli toodatabase úrovni přihlášení na úrovni serveru.
    
5. V případě potřeby nakonfigurujte uživatele nový tooconnect hello služby brány firewall pravidla tooallow. (hello nového uživatele může být pokryté komponentami existující pravidlo brány firewall.)

Nyní hello uživatel může připojit toohello hlavní databázi a můžete vytvářet nové databáze. Vytvoření databáze hello účet Hello stane hello vlastník databáze hello.

### <a name="login-managers"></a>Správci přihlášení
Hello jiných rolí správce je role správce hello přihlášení. Členové této role můžete vytvořit nové přihlášení v hlavní databázi hello. Pokud chcete, můžete dokončit hello stejný postup (vytvořit přihlašovací údaje a uživatel a přidejte uživatele toohello **loginmanager** role) tooenable uživatel toocreate nových přihlášení v předloze hello. Obvykle přihlášení nejsou nutné jako úroveň databáze Microsoft doporučuje použití uživatele databáze s omezením, které ověření v hello místo použití založené na přihlášení uživatele. Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="non-administrator-users"></a>Uživatelé bez oprávnění správce
Obecně platí účtů bez oprávnění správce potřebovat přístup k toohello hlavní databázi. Vytvořit uživatele databáze s omezením na úrovni databáze hello pomocí hello [vytvořit uživatele (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx) příkaz. Hello uživatel může být uživatel databáze obsažené ověřování Azure Active Directory, (Pokud jste nakonfigurovali prostředí pro ověřování Azure AD), nebo uživatel ověřování obsažené databáze systému SQL Server nebo uživatele ověřování systému SQL Server podle SQL Přihlášení k serveru ověřování (vytvořili v předchozím kroku hello.) Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx). 

Uživatelé toocreate připojit toohello databáze a spusťte příkazy podobné toohello následující příklady:

```
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Pouze jeden hello správci nebo hello vlastník databáze hello na začátku můžete vytvořit uživatele. tooauthorize další uživatele toocreate noví uživatelé, udělte této vybraného uživatele hello `ALTER ANY USER` oprávnění pomocí příkazu:

```
GRANT ALTER ANY USER tooMary;
```

toogive další uživatelé úplné řízení databáze hello, díky kterým členem hello **db_owner** pevné databázové role pomocí hello `ALTER ROLE` příkaz.

> [!NOTE]
> Hello nejběžnější důvod toocreate databáze uživatelů podle přihlášení, je při ověřování uživatele systému SQL Server, které potřebují přístup k databázím toomultiple máte. Založené na přihlášení uživatele jsou vázané toohello přihlášení a jenom jedno heslo, které bude zachována pro účely tohoto přihlášení. Uživatelé databází s omezením jsou v jednotlivých databázích jednotlivými entitami a pro každého se v každé databázi udržuje vlastní heslo. To může být pro uživatele databází s omezením matoucí, pokud neudržují stejná hesla.

### <a name="configuring-hello-database-level-firewall"></a>Konfigurace brány firewall na úrovni databáze hello
Jako osvědčený postup uživatelé bez oprávnění správce by měl mít přístup pouze prostřednictvím hello brány firewall toohello databáze, které používají. Místo autorizace jejich IP adresy pomocí hello na úrovni serveru brány firewall a poskytnete jim přístup tooall databáze, použijte hello [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) příkaz tooconfigure hello úroveň databáze brány firewall. brány firewall na úrovni databáze Hello nelze konfigurovat pomocí portálu hello.

### <a name="non-administrator-access-path"></a>Cesta pro přístup uživatelů bez oprávnění správce
Pokud brána firewall na úrovni databáze hello je správně nakonfigurovaná, hello databáze uživatelé se mohou připojit pomocí klienta nástroje, jako je SQL Server Management Studio nebo SQL Server Data Tools. Jenom nejnovější nástroje pro hello poskytují všechny hello funkce a možnosti. Hello následující diagram znázorňuje cestu typické přístup bez oprávnění správce.

![Cesta pro přístup uživatelů bez oprávnění správce](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Skupiny a role
Správa efektivní přístup využívá oprávnění přiřazená toogroups a role místo jednotlivým uživatelům. 

- Pokud používáte ověřování pomocí Azure Active Directory, přidejte uživatele služby Azure Active Directory do skupiny Azure Active Directory. Vytvořte uživatele databáze s omezením pro skupinu hello. Umístit do jednoho nebo více uživatelů databáze [databázové role](https://msdn.microsoft.com/library/ms189121) a pak mu přiřaďte [oprávnění](https://msdn.microsoft.com/library/ms191291.aspx) toohello databázové role.

- Při použití ověřování SQL serveru, vytvořte uživatele databáze s omezením v databázi hello. Umístit do jednoho nebo více uživatelů databáze [databázové role](https://msdn.microsoft.com/library/ms189121) a pak mu přiřaďte [oprávnění](https://msdn.microsoft.com/library/ms191291.aspx) toohello databázové role.

předdefinované role hello Hello databázové role může být například **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter**, a **db_denydatareader**. **db_owner** je běžně používané toogrant úplná oprávnění tooonly několik uživatelů. Hello jiné pevné databázové role jsou užitečné pro získání jednoduchou databázi v vývoj rychle, ale nejsou doporučuje pro většinu provozních databází. Například hello **db_datareader** pevné databázové role uděluje přístup pro čtení tooevery tabulky v databázi hello, který je obvykle více než je nezbytně nutné. Je mnohem lepší hello toouse [vytvořit ROLI](https://msdn.microsoft.com/library/ms187936.aspx) toocreate příkaz vlastní uživatelské role databáze a pečlivě udělit každé role hello nejnižší oprávnění, které jsou nezbytné pro potřeby podniku hello. Pokud je uživatel členem více rolí, jejich agregovat hello oprávnění je všechny.

## <a name="permissions"></a>Oprávnění
Ve službě SQL Database je dostupných více než 100 oprávnění, která můžete jednotlivě přidělit nebo zamítnout. Mnohá z těchto oprávnění jsou vnořená. Například hello `UPDATE` oprávnění na schéma zahrnuje hello `UPDATE` oprávnění jednotlivé tabulky v tomto schématu. Stejně jako většina systémů oprávnění přepíše hello odepření oprávnění grant. Kvůli povaze hello vnořené a hello počet oprávnění může trvat pečlivě toodesign studie příslušná oprávnění systému tooproperly chránit vaši databázi. Začněte s hello seznam oprávnění v [oprávnění (databázový stroj)](https://msdn.microsoft.com/library/ms191291.aspx) a zkontrolovat hello [plakát velikost obrázku](http://go.microsoft.com/fwlink/?LinkId=229142) hello oprávnění.


### <a name="considerations-and-restrictions"></a>Důležité informace a omezení
Při správě přihlášení a uživatele v databázi SQL, zvažte následující hello:

* Musí být připojené toohello **hlavní** databáze při provádění hello `CREATE/ALTER/DROP DATABASE` příkazy.   
* Hello databáze uživatele odpovídající toohello **správce serveru** přihlášení nelze změnit ani vyřadit. 
* Čeština – je výchozí jazyk hello hello **správce serveru** přihlášení.
* Pouze správci hello (**správce serveru** přihlášení nebo správce Azure AD) a hello členy hello **dbmanager** databázové role v hello **hlavní** databáze mít oprávnění tooexecute hello `CREATE DATABASE` a `DROP DATABASE` příkazy.
* Musí být připojené toohello hlavní databázi, při provádění hello `CREATE/ALTER/DROP LOGIN` příkazy. Nedoporučuje se používat přihlášení. Použijte raději databázové uživatele s omezením.
* tooconnect tooa uživatele databáze, je nutné zadat název hello hello databáze v hello připojovací řetězec.
* Pouze hello úrovni serveru členy hlavní přihlášení a hello hello **loginmanager** databázové role v hello **hlavní** databáze mít oprávnění tooexecute hello `CREATE LOGIN`, `ALTER LOGIN`, a `DROP LOGIN` příkazy.
* Při provádění hello `CREATE/ALTER/DROP LOGIN` a `CREATE/ALTER/DROP DATABASE` příkazy v aplikaci ADO.NET pomocí parametrizované příkazy není povoleno. Další informace viz [Příkazy a parametry](https://msdn.microsoft.com/library/ms254953.aspx).
* Při provádění hello `CREATE/ALTER/DROP DATABASE` a `CREATE/ALTER/DROP LOGIN` příkazy, každý z těchto příkazů musí být hello pouze příkazem v dávce Transact-SQL. V opačném případě dojde k chybě. Například hello následující Transact-SQL ověří, zda text hello databázi existuje. Pokud existuje, `DROP DATABASE` zavolá se příkaz tooremove hello databáze. Protože hello `DROP DATABASE` příkaz není hello jediným příkazem v dávce hello, provádění hello následující příkaz jazyka Transact-SQL skončí chybou.

  ```
  IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
  DROP DATABASE [database_name];
  GO
  ```

* Při provádění hello `CREATE USER` příkaz s hello `FOR/FROM LOGIN` možnost, musí být hello pouze příkazem v dávce Transact-SQL.
* Při provádění hello `ALTER USER` příkaz s hello `WITH LOGIN` možnost, musí být hello pouze příkazem v dávce Transact-SQL.
* příliš`CREATE/ALTER/DROP` uživatel vyžaduje hello `ALTER ANY USER` oprávnění v databázi hello.
* Při hello vlastníka role databáze pokusí tooadd nebo odeberte jiný uživatel tooor databáze z tohoto databázové role, může dojít k následující chybě hello: **uživatele nebo roli 'Name' neexistuje v této databázi.** K této chybě dojde, protože uživatel hello není viditelné toohello vlastníka. tooresolve-li tento problém, udělte hello roli vlastníka hello `VIEW DEFINITION` oprávnění uživatele hello. 


## <a name="next-steps"></a>Další kroky

- toolearn Další informace o pravidla brány firewall, najdete v části [Firewall databáze Azure SQL](sql-database-firewall-configure.md).
- Přehled všech funkcí zabezpečení hello SQL Database, najdete v části [Přehled zabezpečení SQL](sql-database-security-overview.md).
- Podívejte se kurz [zabezpečení vaší databázi SQL Azure](sql-database-security-tutorial.md).
- Informace o zobrazeních a uložených procedurách najdete v tématu [Vytváření zobrazení a uložených procedur](https://msdn.microsoft.com/library/ms365311.aspx).
- Informace o udělení přístupu tooa databázový objekt najdete v tématu [tooa udělení přístupu k objektu databáze](https://msdn.microsoft.com/library/ms365327.aspx)
