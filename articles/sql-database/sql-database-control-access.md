---
title: "aaaGranting přístup tooAzure databáze SQL | Microsoft Docs"
description: "Informace o udělení přístupu tooMicrosoft Azure SQL Database."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/06/2017
ms.author: rickbyh
ms.openlocfilehash: 4c32fafa7e98b731ff2f9bf4666da7e4a3145286
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-access-control"></a>Řízení přístupu ke službě Azure SQL Database
tooprovide zabezpečení, databáze SQL řídí přístup s pravidla brány firewall omezení připojení pomocí IP adresy, vyžadování uživatelé tooprove mechanismy ověřování své identity a autorizace mechanismy omezení uživatelů toospecific akce a data. 

> [!IMPORTANT]
> Přehled funkcí zabezpečení hello SQL Database, najdete v části [Přehled zabezpečení SQL](sql-database-security-overview.md). Podívejte se kurz [zabezpečení vaší databázi SQL Azure](sql-database-security-tutorial.md).

## <a name="firewall-and-firewall-rules"></a>Brána firewall a pravidla brány firewall
Microsoft Azure SQL Database poskytuje relační databázovou službu pro aplikace Azure a další internetové aplikace. toohelp chránit vaše data, brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění. brány firewall Hello uděluje přístup toodatabases podle hello pocházející IP adresu každého požadavku. Další informace najdete v tématu [Přehled pravidel brány firewall služby Azure SQL Database](sql-database-firewall-configure.md).

Hello služby Azure SQL Database je pouze k dispozici prostřednictvím portu TCP 1433. tooaccess databáze SQL z vašeho počítače, ujistěte se, že klientský počítač brány firewall umožňuje odchozí komunikaci TCP na portu TCP 1433. Pokud ho jiné aplikace nepotřebují, zablokujte na portu TCP 1433 příchozí připojení. 

Jako součást procesu připojení hello připojení z virtuální počítače Azure jsou přesměrovaného tooa jinou IP adresu a port, jedinečné pro každou roli pracovního procesu. číslo portu Hello je v rozsahu hello od 11000 too11999. Další informace o portech TCP naleznete v tématu [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database2](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Authentication

SQL Database podporuje dva typy ověřování:

* **Ověřování SQL,** které používá uživatelské jméno a heslo. Když vytvoříte hello logický server pro databázi, jste zadali "serveru admin" přihlášení s uživatelské jméno a heslo. Pomocí těchto přihlašovacích údajů, můžete ověřovat tooany databáze na tomto serveru jako vlastník databáze hello nebo "dbo." 
* **Ověřování pomocí Azure Active Directory,** které používá identity spravované v Azure Active Directory a je podporované u spravovaných a integrovaných domén. [Kdykoliv to půjde](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode), použijte ověřování pomocí Active Directory (integrované zabezpečení). Pokud chcete toouse ověřování Azure Active Directory, musíte vytvořit jiný správce serveru názvem hello "Azure AD admin", který je povolen tooadminister Azure AD Uživatelé a skupiny. Tento správce také smí provádět všechny operace jako běžný správce serveru. V tématu [připojení tooSQL databáze pomocí pomocí Azure Active Directory Authentication](sql-database-aad-authentication.md) pro návod, jak toocreate tooenable Správce služby Azure AD ověřování Azure Active Directory.

Hello databázový stroj zavře připojení, která zůstat v nečinnosti déle než 30 minut. Hello připojení znovu musí přihlášení před použitím. Nepřetržitě aktivních připojení tooSQL databáze vyžadovat opětovné ověření (provádí hello databázový stroj) minimálně každých 10 hodin. databázový stroj Hello pokusy o opětovné ověření pomocí hesla původně odeslaného hello a není třeba žádný uživatelský vstup. Z důvodů výkonu při vytvoření nového hesla v databázi SQL, hello připojení není vyjednání, i když hello připojení je ukončeno z důvodu tooconnection sdružování. To se liší od chování hello místní SQL Server. Pokud hello heslo bylo změněno, protože byl původně autorizován hello připojení, musí být ukončen hello připojení a vytvořit nové připojení pomocí hello nové heslo. Uživatel s hello `KILL DATABASE CONNECTION` oprávnění můžete explicitně ukončit připojení tooSQL databáze pomocí hello [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql) příkaz.

Uživatelské účty lze vytvořit v hlavní databázi hello a může být přiřazeno oprávnění v všechny databáze na serveru hello, nebo je lze vytvořit v hello databáze (nazývané uživatelů s omezením). Informace o vytváření a správě přihlašování najdete v tématu [Správa přihlašování](sql-database-manage-logins.md). přenositelnost tooenhance a scalabilty, použijte škálovatelnost tooenhance uživatelé databáze s omezením. Další informace o uživatelích s omezením najdete v tématech [Uživatelé databáze s omezením – vytvoření přenosné databáze](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), [CREATE USER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) a [Databáze s omezením](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases).

Jako osvědčený postup by aplikace měla použít vyhrazený účet tooauthenticate – tímto způsobem můžete omezit hello oprávnění udělená aplikaci toohello a snížení rizika hello škodlivých aktivit v případě kód aplikace je snadno napadnutelný tooa Injektáž SQL útok. Hello doporučený postup je toocreate [uživatele databáze s omezením](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), což umožňuje vaší aplikaci tooauthenticate přímo toohello databáze. 

## <a name="authorization"></a>Autorizace

Autorizace odkazuje toowhat může uživatel provádět v rámci Azure SQL Database, a tím je řízeno váš uživatelský účet databáze [členství v rolích](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) a [oprávnění na úrovni objektů](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine). Jako osvědčený postup byste měli udělit uživatelům hello nejnižší oprávnění potřebná. Hello účet správce serveru, ke kterému se připojujete se členem role db_owner, který má autority toodo nic v rámci hello databáze. Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy. Použít účet "ApplicationUser" hello s omezenější tooconnect oprávnění z vaší aplikace toohello databáze pomocí hello nejnižších oprávnění vyžadované vaší aplikace. Další informace najdete v tématu [Správa přihlašování](sql-database-manage-logins.md).

Obvykle potřebujete jenom správci přístup toohello `master` databáze. Rutiny přístup tooeach uživatele databáze by měla být prostřednictvím uživatelé bez oprávnění správce databáze s omezením vytvoření v každé databázi. Použijete-li uživatelé databáze s omezením, není nutné toocreate přihlášení v hello `master` databáze. Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable).

Měli seznámit s hello následující funkce, které lze použít toolimit nebo zvýšení oprávnění:   
* [Zosobnění](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) a [podepisování modulu](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) můžou být použité toosecurely zvýšení oprávnění dočasně.
* K omezení řádků, ke kterým má uživatel přístup, můžete použít [zabezpečení na úrovni řádku](https://docs.microsoft.com/sql/relational-databases/security/row-level-security).
* [Maskování dat](sql-database-dynamic-data-masking-get-started.md) lze použít toolimit ohrožení citlivých dat.
* [Uložené procedury](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) lze použít toolimit hello akce, které můžete provést na databázi hello.

## <a name="next-steps"></a>Další kroky

- Přehled funkcí zabezpečení hello SQL Database, najdete v části [Přehled zabezpečení SQL](sql-database-security-overview.md).
- toolearn Další informace o pravidla brány firewall, najdete v části [pravidla brány Firewall](sql-database-firewall-configure.md).
- toolearn o uživatelích a přihlášení, najdete v části [spravovat přihlášení](sql-database-manage-logins.md). 
- Informace Proaktivní monitorování, naleznete v [auditování databáze](sql-database-auditing.md) a [detekce hrozeb databáze SQL](sql-database-threat-detection.md).
- Podívejte se kurz [zabezpečení vaší databázi SQL Azure](sql-database-security-tutorial.md).
