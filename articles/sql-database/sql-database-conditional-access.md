---
title: "aaaConditional Access – Azure SQL Database a Data Warehouse | Microsoft dokumentů"
description: "Zjistěte, jak tooconfigure podmíněný přístup pro Azure SQL Database a datového skladu."
services: sql-database
author: BYHAM
manager: jhubbard
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: rickbyh
ms.openlocfilehash: f49f4708c0f1b3cad1539d630c2efd919f8ece68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Podmíněný přístup (MFA) s Azure SQL Database a datového skladu  

SQL Database a SQL Data Warehouse podporují podmíněný přístup společnosti Microsoft. Dobrý den, jak následující kroky zobrazit tooconfigure SQL Database tooenforce zásady podmíněného přístupu.  

## <a name="prerequisites"></a>Požadavky  
- Je nutné nakonfigurovat ověřování Azure Active Directory toosupport SQL Database nebo SQL Data Warehouse. Konkrétní pokyny najdete v tématu [konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](sql-database-aad-authentication-configure.md).  
- Pokud je povoleno ověřování Multi-Factor authentication, je nutné připojit s v podporované nástroje, například hello nejnovější aplikace SSMS. Další informace najdete v tématu [nakonfigurovat databázi SQL Azure Multi-Factor authentication pro SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Konfigurace certifikační Autority pro Azure SQL DB nebo datového skladu  
1.  Vyberte přihlašovací toohello portál, **Azure Active Directory**a potom vyberte **podmíněného přístupu**. Další informace najdete v tématu [technické informace o Azure Active Directory podmíněného přístupu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference).  
  ![okno podmíněného přístupu](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2.  V hello **zásady podmíněného přístupu** okně klikněte na tlačítko **nové zásady**, zadejte název a potom klikněte na **konfigurovat pravidla**.  
3.  V části **přiřazení**, vyberte **uživatelů a skupin**, zkontrolujte **vyberte uživatele a skupiny**a potom vyberte hello uživatele nebo skupinu pro podmíněný přístup. Klikněte na tlačítko **vyberte**a potom klikněte na **provádí** tooaccept váš výběr.  
  ![Vyberte uživatele a skupiny](./media/sql-database-conditional-access/select-users-and-groups.png)  

4.  Vyberte **cloudových aplikací**, klikněte na tlačítko **vybrat aplikace**. Zobrazí všechny aplikace dostupné pro podmíněný přístup. Vyberte **Azure SQL Database**, v dolní části hello klikněte na tlačítko **vyberte**a potom klikněte na **provádí**.  
  ![Vyberte databázi SQL](./media/sql-database-conditional-access/select-sql-database.png)  
  Pokud nemůžete najít **Azure SQL Database** uvedené v následující třetí snímek obrazovky hello, dokončete hello následující kroky:   
  - Přihlaste se instance Azure SQL DB nebo DW tooyour pomocí aplikace SSMS pomocí účtu správce AAD.  
  - Spustit `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
  - Přihlaste se tooAAD a ověřte, že Azure SQL Database a datového skladu uvedeny v hello aplikací ve vaší AAD.  

5.  Vyberte **přístup k ovládacím prvkům**, vyberte **Grant**a potom zkontrolujte zásady hello chcete tooapply. V tomto příkladu jsme vyberte **vyžadovat vícefaktorové ověřování**.  
  ![Vyberte udělení přístupu](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Souhrn  
Hello vybrané aplikace (Azure SQL Database) umožňuje tooconnect tooAzure SQL DB nebo datového skladu pomocí Azure AD Premium, teď vynucuje hello vybrané zásady podmíněného přístupu, **požadované služby Multi-Factor authentication.**  
Otázky týkající se Azure SQL Database a datového skladu týkající se služby Multi-Factor authentication, vám poskytne MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Další kroky  

Podívejte se kurz [zabezpečení vaší databázi SQL Azure](sql-database-security-tutorial.md).
