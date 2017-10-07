---
title: "ověření služby Active Directory aaaAzure - SQL Azure (přehled) | Microsoft Docs"
description: "Další informace o tom toouse Azure Active Directory k ověřování připojení k SQL Database a SQL Data Warehouse"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/11/2017
ms.author: rickbyh
ms.openlocfilehash: 7a63a162653b65294e11d3fa5bf39c320e742854
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-or-sql-data-warehouse"></a>Pomocí ověřování Azure Active Directory k ověřování připojení k SQL Database nebo SQL Data Warehouse
Ověřování Azure Active Directory je mechanismus připojující tooMicrosoft Azure SQL Database a [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) pomocí identit v Azure Active Directory (Azure AD). Při ověřování Azure AD můžete centrálně spravovat hello identity uživatelů, databáze a další služby Microsoftu v jednom centrálním místě. Centrální správa ID umožňuje uživatelům toomanage databáze na jednom místě a zjednodušuje správu oprávnění. Například hello následující výhody:

* Poskytuje alternativní tooSQL ověření serveru.
* Pomáhá zastavit šíření hello identit uživatelů serverů databáze.
* Umožňuje otočení heslo na jednom místě
* Zákazníci můžete spravovat oprávnění databáze pomocí externí skupiny (AAD).
* Ukládání hesel se může eliminovat tím, že umožňuje integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure Active Directory.
* Azure AD ověřování pomocí identity tooauthenticate uživatelé databáze s omezením na úrovni databáze hello.
* Azure AD podporuje ověřování na základě tokenu pro připojování tooSQL databáze aplikace.
* Ověřování služby Azure AD podporuje služby AD FS (federation domény) nebo ověřování nativní uživatele a heslo pro místní Azure Active Directory bez synchronizace domény.  
* Azure AD podporuje připojení z SQL Server Management Studio, které používají Universal ověřování služby Active Directory, která zahrnuje Multi-Factor Authentication (MFA).  Vícefaktorové ověřování zahrnuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textová zpráva, čipové karty s PIN kód nebo oznámení mobilní aplikace. Další informace najdete v tématu [SSMS podpora pro Azure AD MFA s SQL Database a SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).  

>  [!NOTE]  
>  Připojení tooSQL Server běžící na virtuálním počítači Azure se nepodporuje, pomocí účtu Azure Active Directory. Místo toho používejte doménu účtu služby Active Directory.  

kroky konfigurace Hello zahrnout hello následující tooconfigure postupy a pomocí ověřování Azure Active Directory.

1. Vytvořit a naplnit Azure AD.
2. Volitelné: Přidružení nebo změňte hello služby active directory, který je aktuálně přidružena předplatného Azure.
3. Vytvoření správce Azure Active Directory pro server Azure SQL nebo [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. Nakonfigurujte klientské počítače.
5. Vytvořte uživatele databáze s omezením vaší databáze namapované tooAzure identit AD.
6. Připojte databáze tooyour pomocí identit Azure AD.

> [!NOTE]
> toolearn jak toocreate a naplnit Azure AD a potom nakonfigurovat Azure AD s Azure SQL Database a SQL Data Warehouse, najdete v části [konfigurovat Azure AD s Azure SQL Database](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Architektura vztahu důvěryhodnosti
Hello následující vysokoúrovňový diagram shrnuje architektury řešení hello používání ověřování Azure AD s Azure SQL Database. Hello koncepty použity tooSQL datového skladu. považuje za toosupport heslo nativní uživatele Azure AD, pouze hello cloudové části a Azure AD nebo Azure SQL Database. toosupport federovaný ověřování (nebo uživatele a heslo pro přihlašovací údaje systému Windows), je vyžadován hello komunikace se službou AD FS bloku. šipky Hello označují způsobů komunikace.

![diagram ověřování aad][1]

Hello následující diagram označuje hello federace, důvěryhodnosti a hostitelské vztahy, které umožňují klientovi tooconnect tooa databáze odesláním token. Hello token ověření Azure AD a důvěřují hello databáze. Odběratel 1 může představovat Azure Active Directory s nativní uživateli nebo Azure AD s federovanými uživateli. Odběratel 2 představuje možné řešení včetně importované uživatele; v tomto příkladu pocházejících z federované Azure Active Directory se službou AD FS se synchronizovat se službou Azure Active Directory. Je důležité toounderstand, který přístup k databázi tooa pomocí ověřování Azure AD vyžaduje, že je tento hello hostování předplatné přidružené toohello Azure AD. Hello stejného předplatného musí být použité toocreate hello hostování hello systému SQL Server Azure SQL Database nebo SQL Data Warehouse.

![předplatné relace][2]

## <a name="administrator-structure"></a>Struktura správce
Pokud používáte ověřování Azure AD, jsou dva účty správce pro server SQL Database hello. Hello původní správce systému SQL Server a správce hello Azure AD. Hello koncepty použity tooSQL datového skladu. Pouze správce hello založené na účet služby Azure AD můžete vytvořit hello první uživatel databáze Azure AD, které jsou obsažené v uživatelské databázi. přihlášení správce Hello Azure AD může být Azure Active Directory nebo skupině služby Azure AD. Když správce hello je účet skupiny, může sloužit kteréhokoli člena skupiny povolení více správců Azure AD pro instanci systému SQL Server hello. Použití účtu skupiny jako správce rozšiřuje možnosti správy tím, že jste toocentrally přidávat a odebírat členy skupiny ve službě Azure AD beze změny hello uživatele nebo oprávnění v databázi SQL. Kdykoli se dá nakonfigurovat jenom jeden správce Azure AD (uživatele či skupinu).

![Struktura správce][3]

## <a name="permissions"></a>Oprávnění
toocreate noví uživatelé, musí mít hello `ALTER ANY USER` oprávnění v databázi hello. Hello `ALTER ANY USER` oprávnění lze udělit tooany uživatele databáze. Hello `ALTER ANY USER` oprávnění je také držené hello účtů správce serveru a uživatele databáze s hello `CONTROL ON DATABASE` nebo `ALTER ON DATABASE` oprávnění pro tuto databázi a členové hello `db_owner` role databáze.

toocreate uživatele databáze s omezením v Azure SQL Database nebo SQL Data Warehouse, je nutné připojit databázi toohello pomocí Azure AD identity. toocreate hello první databáze s omezením uživatele, je nutné připojit toohello databáze pomocí Správce Azure AD (který je hello vlastník databáze hello). Tento postup je znázorněn v kroku 4 a 5 níže. Ověřování služby Azure AD je možné, pokud byla vytvořena Dobrý den, správce Azure AD pro server Azure SQL Database nebo SQL Data Warehouse pouze. Pokud dobrý den, správce Azure Active Directory byla odebrána ze serveru hello, můžete stávající uživatele Azure Active Directory předtím vytvořili v systému SQL Server již připojit toohello databáze pomocí svých přihlašovacích údajů Azure Active Directory.

## <a name="azure-ad-features-and-limitations"></a>Funkce Azure AD a omezení
v Azure SQL server nebo SQL Data Warehouse se dá zřídit Hello následující členy Azure AD:

* Nativní členy: člen vytvořené ve službě Azure AD v hello spravované doméně nebo v doméně zákazníka. Další informace najdete v tématu [přidat vlastní tooAzure název domény AD](../active-directory/active-directory-add-domain.md).
* Federované domény členy: člen vytvořené v Azure AD s federovanou doménu. Další informace najdete v tématu [Microsoft Azure teď podporuje federační službou Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
* Importované členy z jiných Azure AD, kteří jsou členy nativní nebo federované domény.
* Active Directory skupiny vytvořené jako skupin zabezpečení.

Účty Microsoft (například outlook.com, hotmail.com, live.com) nebo jiné účty hosta (například gmail.com, yahoo.com) nejsou podporovány. Pokud se můžete přihlásit příliš[https://login.live.com](https://login.live.com) pomocí hello účtu a hesla, pak se pomocí účtu Microsoft, což není podporováno pro ověřování Azure AD pro databáze SQL Azure nebo Azure SQL Data Warehouse.

## <a name="connecting-using-azure-ad-identities"></a>Připojení pomocí identit Azure AD

Ověřování Azure Active Directory podporuje hello následující metody připojování tooa databáze pomocí Azure AD identity:

* Pomocí integrovaného ověřování systému Windows
* Pomocí služby Azure AD hlavní název a heslo
* Pomocí ověření pomocí tokenu aplikace

### <a name="additional-considerations"></a>Další aspekty

* možnosti správy tooenhance, doporučujeme zřídit vyhrazené Azure AD jako správce.   
* Pouze jeden správce Azure AD (uživatele či skupinu) lze nakonfigurovat pro server Azure SQL nebo Azure SQL Data Warehouse kdykoli.   
* Pouze správce Azure AD pro SQL Server můžete nejprve připojit toohello server Azure SQL nebo Azure SQL Data Warehouse pomocí účtu Azure Active Directory. Správce služby Active Directory Hello můžete nakonfigurovat další služby Azure AD databázi uživatelů.   
* Doporučujeme, abyste nastavení too30 hello připojení časový limit v sekundách.   
* SQL Server 2016 Management Studio a SQL Server Data Tools pro Visual Studio 2015 (verze 14.0.60311.1April 2016 nebo novější) podporují ověřování Azure Active Directory. (Ověřování azure AD podporuje hello **zprostředkovatel dat .NET Framework pro SQL Server**; minimální verze rozhraní .NET Framework 4.6). Proto hello nejnovější verze těchto nástrojů a aplikací na datové vrstvě (DAC a .bacpac) můžete použít ověřování Azure AD.   
* [ODBC verze 13.1](https://www.microsoft.com/download/details.aspx?id=53339) podporuje ověřování Azure Active Directory, ale `bcp.exe` nelze se připojit pomocí ověřování Azure Active Directory, protože používá poskytovatele starší ODBC.   
* `sqlcmd`podporuje začátku ověřování Azure Active Directory s verzí 13.1 dostupné z hello [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server Data Tools pro Visual Studio 2015 vyžaduje alespoň hello. dubna 2016 verzi hello nástrojů Data (verze 14.0.60311.1). Aktuálně nejsou Azure AD uživatelům zobrazí v Průzkumníku objektů rozšíření SSDT. Jako alternativní řešení, zobrazení hello uživatelů v [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
* [6.0 ovladač JDBC Microsoft pro systém SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) ověřování podporuje Azure AD. Další informace naleznete v [nastavení vlastnosti připojení hello](https://msdn.microsoft.com/library/ms378988.aspx).   
* PolyBase se nemůže ověřit pomocí ověřování Azure AD.   
* Ověřování služby Azure AD podporuje pro databázi SQL hello portál Azure **Import databáze** a **Export databáze** okna. Import a export pomocí ověřování Azure AD je podporováno také z hello příkaz prostředí PowerShell.   
* Ověřování služby Azure AD je podporováno pro databázi SQL a SQL Data Warehouse pomocí rozhraní příkazového řádku. Další informace najdete v tématu [konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](sql-database-aad-authentication-configure.md) a [SQL Server - az sql server](https://docs.microsoft.com/en-us/cli/azure/sql/server).

## <a name="next-steps"></a>Další kroky
- toolearn jak toocreate a naplnit Azure AD a potom nakonfigurovat Azure AD s Azure SQL Database nebo Azure SQL Data Warehouse, najdete v části [konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](sql-database-aad-authentication-configure.md).
- Přehled řízení a přístupu pro SQL Database najdete v tématu věnovaném [řízení a přístupu k SQL Database](sql-database-control-access.md).
- Přehled přihlášení, uživatelů a databázových rolí ve službě SQL Database najdete v tématu věnovaném [přihlášením, uživatelům a databázovým rolím](sql-database-manage-logins.md).
- Další informace o objektech zabezpečení databáze najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
- Další informace o databázových rolích najdete v tématu věnovaném [databázovým rolím](https://msdn.microsoft.com/library/ms189121.aspx).
- Další informace o pravidlech brány firewall pro SQL Database najdete v tématu [Pravidla brány firewall služby SQL Database](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

