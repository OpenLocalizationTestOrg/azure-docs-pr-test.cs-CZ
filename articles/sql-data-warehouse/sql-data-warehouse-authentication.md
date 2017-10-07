---
title: aaaAuthentication tooAzure SQL Data Warehouse | Microsoft Docs
description: "Azure Active Directory (AAD) a SQL Server ověřování tooAzure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
tags: 
ms.assetid: fefaaa75-2d0c-4e5d-aadb-410342d1ad73
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.custom: security
ms.date: 03/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 66673ce1d4761243755254c8f64de1078a0ea82e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-tooazure-sql-data-warehouse"></a>TooAzure ověřování SQL datového skladu
> [!div class="op_single_selector"]
> * [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
> * [Ověřování](sql-data-warehouse-authentication.md)
> * [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
> * [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

tooconnect tooSQL datového skladu, musíte zadat v zabezpečovací přihlašovací údaje pro účely ověření. Při navazování připojení, jsou určitá nastavení připojení nakonfigurovaný jako součást navazování relace dotazu.  

Další informace o zabezpečení a jak tooenable připojení tooyour datového skladu, najdete v části [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>Ověřování pomocí SQL
tooconnect tooSQL datového skladu, je nutné zadat hello následující informace:

* Plně kvalifikovaný servername
* Zadejte ověřování SQL.
* Uživatelské jméno
* Heslo
* Výchozí databáze (volitelné)

Ve výchozím nastavení se připojení připojí toohello *hlavní* databáze a nejsou své databázi uživatelů. tooconnect tooyour uživatele databáze, můžete zvolit toodo jednu ze dvou akcí:

* Zadejte výchozí databázi hello při registraci serveru na hello Průzkumník objektů SQL serveru v sadě SSDT, aplikace SSMS, nebo v připojovacím řetězci vaší aplikace. Příklady hello vlastnosti InitialCatalog parametr pro připojení k rozhraní ODBC.
* Před vytvořením relace v sadě SSDT zvýrazněte hello uživatelské databáze.

> [!NOTE]
> Hello příkazu Transact-SQL **použití databáze;** není podporována pro změnu hello databáze pro připojení. Pokyny k připojení tooSQL datového skladu s rozšířením SSDT, najdete v tématu toohello [dotazu pomocí sady Visual Studio] [ Query with Visual Studio] článku.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Ověřování Azure Active Directory (AAD)
[Azure Active Directory] [ What is Azure Active Directory] ověřování je mechanismus připojující tooMicrosoft Azure SQL Data Warehouse pomocí identit v Azure Active Directory (Azure AD). Pomocí ověřování Azure Active Directory můžete centrálně spravovat hello identity uživatelů, databáze a další služby Microsoftu v jednom centrálním místě. Centrální správa ID umožňuje uživatelům toomanage SQL Data Warehouse na jednom místě a zjednodušuje správu oprávnění. 

### <a name="benefits"></a>Výhody
Azure Active Directory výhody patří:

* Poskytuje alternativní tooSQL ověření serveru.
* Pomáhá zastavit šíření hello identit uživatelů serverů databáze.
* Umožňuje otočení heslo na jednom místě
* Správa oprávnění databáze pomocí externí skupiny (AAD).
* Eliminuje ukládání hesel povolením integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure Active Directory.
* Používá obsažené databáze uživatelé tooauthenticate identity na úrovni databáze hello.
* Podporuje ověřování na základě tokenu pro aplikace připojení tooSQL datového skladu.
* Podporuje Vícefaktorové ověřování prostřednictvím Universal ověřování služby Active Directory pro SQL Server Management Studio. Popis služby Multi-Factor Authentication najdete v tématu [SSMS podpora pro Azure AD MFA s SQL Database a SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Azure Active Directory je pořád relativně nové a má určitá omezení. tooensure, že Azure Active Directory je vhodné pro vaše prostředí, najdete v části [funkce Azure AD a omezení][Azure AD features and limitations], konkrétně hello další aspekty.
> 
> 

### <a name="configuration-steps"></a>Kroky konfigurace
Postupujte podle těchto kroků tooconfigure Azure Active Directory ověřování.

1. Vytvořit a naplnit Azure Active Directory
2. Volitelné: Přidružení nebo změňte hello služby active directory, který je aktuálně přidružena předplatného Azure
3. Vytvořte správce Azure Active Directory pro Azure SQL Data Warehouse.
4. Konfigurace klientských počítačů
5. Vytvoření uživatele databáze s omezením ve vaší databázi namapované tooAzure identit AD
6. Připojit tooyour datového skladu pomocí identit Azure AD

Aktuálně nejsou Azure Active Directory uživatelům zobrazí v Průzkumníku objektů rozšíření SSDT. Jako alternativní řešení, zobrazení hello uživatelů v [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-hello-details"></a>Najde podrobnosti o hello
* Hello kroky tooconfigure a používání ověřování Azure Active Directory jsou téměř stejné pro databázi SQL Azure a Azure SQL Data Warehouse. Postupujte podle kroků v tématu hello podrobné hello [tooSQL připojení databáze nebo SQL Data Warehouse pomocí pomocí Azure ověřování Active Directory](../sql-database/sql-database-aad-authentication.md).
* Vytvořte vlastní databázové role a přidejte toohello role uživatele. Potom udělte oprávnění na podrobné úrovni toohello role. Další informace najdete v tématu [Začínáme s oprávněními modul databáze](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Další kroky
toostart dotazování datového skladu pomocí sady Visual Studio a dalších aplikací, najdete v části [dotazu pomocí sady Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
