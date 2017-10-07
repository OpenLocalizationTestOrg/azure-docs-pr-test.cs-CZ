---
title: "aaaConfigure služby Multi-Factor authentication - Azure SQL | Microsoft Docs"
description: "Pro databáze SQL a SQL Data Warehouse pomocí aplikace SSMS promítnou více ověřování."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/17/2017
ms.author: rickbyh
ms.openlocfilehash: acb275965f4199f7d5e1378d5077824a9bbc80e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Konfigurace vícefaktorového ověřování pro SQL Server Management Studio a Azure AD

Toto téma ukazuje, jak toouse Azure Active Directory vícefaktorové ověřování (MFA) se SQL Server Management Studio. Azure AD MFA lze použít při připojování SSMS nebo SqlPackage.exe tooAzure SQL Database a Azure SQL Data Warehouse.

Přehled služby databáze SQL Azure Multi-Factor authentication, naleznete v části [Universal ověřování se službou SQL Database a SQL Data Warehouse (SSMS podporu vícefaktorového ověřování)](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Kroky konfigurace

1. **Konfigurace Azure Active Directory** – Další informace najdete v tématu [Správa adresáře služby Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx), [integrace místních identit s Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Přidat vlastní tooAzure název domény AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure teď podporuje federační službou Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), a [Správa Azure AD pomocí prostředí Windows PowerShell ](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurace vícefaktorového ověřování** – podrobné pokyny najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md), [podmíněný přístup (MFA) s Azure SQL Database a datový sklad](sql-database-conditional-access.md). (Vyžaduje úplné podmíněného přístupu Premium Azure Active Directory (Azure AD). Omezené MFA je k dispozici s standardní Azure AD.)
3. **Konfigurace SQL Database nebo SQL Data Warehouse pro Azure AD Authentication** – podrobné pokyny najdete v tématu [tooSQL připojení databáze nebo SQL Data Warehouse pomocí pomocí Azure ověřování Active Directory](sql-database-aad-authentication.md).
4. **Stažení aplikace SSMS** – hello klientského počítače, stáhněte hello nejnovější aplikace SSMS, z [stáhnout SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Pro všechny funkce hello v tomto tématu použijte alespoň července 2017 verze 17.2.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Připojení pomocí universal ověřování pomocí SSMS

Hello následující kroky ukazují, jak tooconnect tooSQL databáze nebo SQL Data Warehouse pomocí hello nejnovější aplikace SSMS.

1. tooconnect pomocí Universal ověřování na hello **připojit tooServer** dialogové okno, vyberte **Universal s podpora vícefaktorového ověřování služby Active Directory -**. (Pokud se zobrazí **Universal ověřování služby Active Directory** nejsou na nejnovější verzi aplikace SSMS hello.)  
   ![1mfa-universal připojení][1]  
2. Dokončení hello **uživatelské jméno** pole s hello Azure Active Directory přihlašovací údaje ve formátu hello `user_name@domain.com`.  
   ![1mfa-universal-connect-user](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Pokud se připojujete jako uživatel guest, musíte kliknout na **možnosti**a na hello **vlastnost připojení** dialogové okno, dokončení hello **ID názvu nebo klienta domény AD** pole. Další informace najdete v tématu [Universal ověřování se službou SQL Database a SQL Data Warehouse (SSMS podporu vícefaktorového ověřování)](sql-database-ssms-mfa-authentication.md).
   ![aplikace ssms vícefaktorového ověřování klienta](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. Obvyklým pro SQL Database a SQL Data Warehouse, musíte kliknout na **možnosti** a zadejte databázi hello na hello **možnosti** dialogové okno. (Pokud hello připojení uživatele je uživatel typu Host (tj. joe@outlook.com), musíte políčko hello a přidat název nebo klienta ID domény hello aktuální AD jako součást možností. V tématu [Universal ověřování se službou SQL Database a SQL Data Warehouse (SSMS podporu vícefaktorového ověřování)]()(sql-database-ssms-mfa-authentication.md. Pak klikněte na tlačítko **Connect**.  
5. Když hello **přihlašovací účet tooyour** se zobrazí dialogové okno, zadejte hello účet a heslo vaší identity Azure Active Directory. Pokud je uživatel součástí domény sdružených se službou Azure AD není vyžadováno heslo.  
   ![2mfa-sign-in][2]  

   > [!NOTE]
   > Pro univerzální ověřování pomocí účtu, který nevyžaduje MFA připojíte se v tomto okamžiku. Vyžadování vícefaktorového ověřování uživatelů pokračujte hello následující kroky:
   >  
   
6. Může se objevit dvě dialogových oken nastavení vícefaktorového ověřování. Tento jednou operace závisí na správce MFA hello nastavení a proto mohou být nepovinné. Pro doménu povolené ověřování MFA tento krok je někdy předem definované (například hello domény vyžaduje toouse uživatele čipová karta a kód pin).  
   ![3mfa-instalace][3]  
7. druhý možné Hello jednou dialogové okno vám umožní tooselect hello podrobnosti o metodu ověřování. dostupné možnosti Hello jsou nakonfigurované správcem.  
   ![4mfa ověřte 1][4]  
8. Hello Azure Active Directory odešle hello potvrzení tooyou informace. Jakmile se zobrazí hello ověřovací kód, zadat ho do hello **zadejte ověřovací kód** pole a klikněte na tlačítko **přihlášení**.  
   ![5mfa ověřte 2][5]  

Po dokončení ověření SSMS připojí, obvykle za předpokladu platné přihlašovací údaje a přístup přes bránu firewall.

## <a name="next-steps"></a>Další kroky

* Přehled služby databáze SQL Azure Multi-Factor authentication, naleznete v části Universal ověřování s [SQL Database a SQL Data Warehouse (SSMS podporu vícefaktorového ověřování)](sql-database-ssms-mfa-authentication.md).
* Poskytnout ostatním přístup k databázi tooyour: [SQL databáze ověřování a autorizace: udělení přístupu](sql-database-manage-logins.md)  
Zajistěte, aby ostatní můžete připojit přes bránu firewall hello: [pomocí pravidla brány firewall konfigurovat Azure SQL Database úrovni server hello portálu Azure](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

