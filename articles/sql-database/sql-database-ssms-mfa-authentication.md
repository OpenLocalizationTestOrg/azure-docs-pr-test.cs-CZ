---
title: aaaMulti Multi-Factor authentication - Azure SQL | Microsoft Docs
description: "Pro databáze SQL a SQL Data Warehouse pomocí aplikace SSMS promítnou více ověřování."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: fbd6e644-0520-439c-8304-2e4fb6d6eb91
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2017
ms.author: rickbyh
ms.openlocfilehash: 57ef63b7c7f2c5cf64c3e1ee194d865ee5b14177
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>Univerzální ověřování se službou SQL Database a SQL Data Warehouse (SSMS podporu vícefaktorového ověřování)
Azure SQL Database a Azure SQL Data Warehouse podporují připojení pomocí SQL Server Management Studio (SSMS) *Universal ověřování služby Active Directory*. 
**Stáhnout hello nejnovější aplikace SSMS** – hello klientského počítače, stáhněte nejnovější verzi aplikace SSMS, hello z [stáhnout SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Pro všechny funkce hello v tomto tématu použijte alespoň července 2017 verze 17.2.  Hello nejnovější dialogové okno připojení, vypadá podobně jako tento: ![1mfa universal připojit](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "Completes hello uživatele název pole.")  

## <a name="hello-five-authentication-options"></a>Možnosti ověřování Hello pět  
- Univerzální ověřování služby Active Directory podporuje dvě metody neinteraktivního ověřování hello (`Active Directory - Password` ověřování a `Active Directory - Integrated` ověřování). Neinteraktivně `Active Directory - Password` a `Active Directory - Integrated` metody ověřování lze použít v mnoha různých aplikacích (ADO.NET, JDBC, ODBC, atd.). Tyto dvě metody nikdy výsledkem automaticky otevíraná okna.

- `Active Directory - Universal with MFA`ověřování je interaktivní metoda, která podporuje také *Azure Multi-Factor Authentication* (MFA). Azure MFA pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování s rozsahem možnosti snadno ověření (telefonní hovor, textová zpráva, čipové karty s PIN kód nebo oznámení mobilní aplikace), povolení uživatelé toochoose hello požadovaným způsobem. Interaktivní vícefaktorového ověřování s Azure AD může způsobit v místním dialogovém okně pro ověření.

Popis služby Multi-Factor Authentication najdete v tématu [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).
Postup konfigurace najdete v tématu [nakonfigurovat databázi SQL Azure Multi-Factor authentication pro SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD domain Název nebo klienta parametr ID   

Počínaje [SSMS verze 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), uživatelé, které jsou importovány do hello aktuální službě Active Directory z jiných Azure Active Directory jako uživatele typu Host, můžete zadat název domény hello Azure AD, nebo ID klienta, když se připojují. Uživatele typu Host zahrnete uživatele pozvat z jiné služby Active Directory Azure, účty Microsoft, jako jsou outlook.com, hotmail.com, live.com nebo jiné účty jako gmail.com. Tyto informace umožňují **Active Universal Directory s ověřováním MFA** tooidentify hello správná autorita pro ověřování. Tato možnost je také toosupport požadované účty Microsoft (MSA), jako jsou outlook.com, hotmail.com, live.com nebo – účet spravované služby účty. ID všechny tyto uživatele, kteří chtějí toobe ověřen pomocí ověřování Universal musí zadat jeho název domény služby Azure AD nebo klienta. Tento parametr představuje hello aktuální Azure AD domain Název nebo klienta ID hello Azure Server je spojena s. Například, pokud Azure Server souvisí s Azure AD domain `contosotest.onmicrosoft.com` kde uživatele `joe@contosodev.onmicrosoft.com` je umístěn jako importované uživatele z Azure AD domain `contosodev.onmicrosoft.com`, hello tooauthenticate vyžaduje název domény je tento uživatel `contosotest.onmicrosoft.com`. Když uživatel hello nativní uživatel hello propojené služby Azure AD tooAzure serveru a není účet účet spravované služby, je požadováno ID názvu nebo klienta žádné domény. Parametr hello tooenter (počínaje SSMS verze 17.2), v hello **připojit tooDatabase** dialogové okno, dialogové okno dokončení hello, výběr **Universal pomocí vícefaktorového ověřování služby Active Directory -** ověřování, Klikněte na tlačítko **možnosti**, dokončení hello **uživatelské jméno** pole a pak klikněte na tlačítko hello **vlastnosti připojení** kartě. Zkontrolujte hello **ID názvu nebo klienta domény AD** pole a zadejte ověřovací autorita, jako je název domény hello (**contosotest.onmicrosoft.com**) nebo hello GUID hello ID klienta.  
   ![aplikace ssms vícefaktorového ověřování klienta](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-toobusiness-support"></a>Podpora toobusiness obchodní Azure AD   
Azure AD Uživatelé podporuje pro scénáře Azure AD B2B jako uživatele typu Host (najdete v části [co je spolupráce Azure B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) může pouze jako součást členové skupiny vytvořené v aktuální Azure AD a namapovat ručně připojit tooSQL databáze a datový sklad SQL pomocí hello Transact-SQL `CREATE USER` příkaz v na danou databázi. Například pokud `steve@gmail.com` je pozvané tooAzure AD `contosotest` (s Azure Ad domain hello `contosotest.onmicrosoft.com`), Azure AD jako skupinu `usergroup` musí být vytvořen v hello Azure AD, která obsahuje hello `steve@gmail.com` člen. Potom tuto skupinu musí vytvořit pro konkrétní databázi (tj. databáze) Správce Azure AD SQL nebo Azure AD DBO spuštěním Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` příkaz. Po vytvoření uživatele databáze hello hello uživatele `steve@gmail.com` příliš se může přihlásit`MyDatabase` pomocí možnosti ověřování aplikace SSMS hello `Active Directory – Universal with MFA support`. Hello usergroup, ve výchozím nastavení, má pouze hello připojit oprávnění a všechny další přístup k datům, který bude nutné toobe v hello obvyklým způsobem. Upozorňujeme, že uživatel `steve@gmail.com` jako uživatel guest musí políčko hello a přidejte název domény hello AD `contosotest.onmicrosoft.com` v hello SSMS **vlastnost připojení** dialogové okno. Hello **ID názvu nebo klienta domény AD** možnost je podporována pouze pro hello Universal s možností připojení vícefaktorového ověřování, jinak je šedá.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Univerzální omezení ověřování pro SQL Database a SQL Data Warehouse
* Aplikace SSMS a SqlPackage.exe jsou pouze nástroje hello aktuálně povoleno pro MFA prostřednictvím Universal ověřování služby Active Directory.
* Aplikace SSMS verze 17.2, podporuje více uživatelů souběžný přístup pomocí Universal ověřování pomocí vícefaktorového ověřování. Verze 17,0 a 17.1, omezený do protokolu pro instanci aplikace SSMS pomocí Universal ověřování tooa jednoho účtu Azure Active Directory. toolog v jako jiný účet služby Azure AD, je nutné použít jiná instance aplikace SSMS. (Toto omezení je omezená tooActive Directory Universal ověřování, se můžete přihlásit toodifferent servery pomocí ověřování hesla Active Directory, integrované ověřování Active Directory nebo ověřování systému SQL Server).
* Aplikace SSMS podporuje univerzální ověřování služby Active Directory pro vizualizaci Průzkumník objektů, Editor dotazů a úložiště dotazů.
* Aplikace SSMS verze 17.2 poskytuje podporu DacFx Průvodce pro Export/Extract nebo nasadit Data databáze. Konkrétní uživatel ověřen pomocí dialogu hello počáteční ověřování pomocí Universal ověřování, hello Průvodce DacFx funkce hello stejný způsobem používaným pro všechny ostatní metody ověřování.
* Hello návrháře tabulky SSMS nepodporuje univerzální ověřování.
* Kromě toho, že musíte použít podporovanou verzi aplikace SSMS nejsou žádné další požadavky na software pro univerzální ověřování služby Active Directory.  
* verze Hello Active Directory Authentication Library (ADAL) pro univerzální ověřování se aktualizované tooits nejnovější ADAL.dll 3.13.9 dostupné vydané verzi. V tématu [knihovna ověřování Active Directory 3.14.1](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).


## <a name="next-steps"></a>Další kroky

- Postup konfigurace najdete v tématu [nakonfigurovat databázi SQL Azure Multi-Factor authentication pro SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
- Poskytnout ostatním přístup k databázi tooyour: [SQL databáze ověřování a autorizace: udělení přístupu](sql-database-manage-logins.md)  
- Zajistěte, aby ostatní můžete připojit přes bránu firewall hello: [pomocí pravidla brány firewall konfigurovat Azure SQL Database úrovni server hello portálu Azure](sql-database-configure-firewall-settings.md)  
- [Konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](sql-database-aad-authentication-configure.md)  
- [Architektura aplikace datové vrstvy Microsoft SQL Server (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)  
- [Import soubor tooa souboru BACPAC novou databázi SQL Azure](../sql-database/sql-database-import.md)  
- [Exportujte soubor souboru BACPAC tooa databáze Azure SQL](../sql-database/sql-database-export.md)  
- C# rozhraní [IUniversalAuthProvider rozhraní](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
