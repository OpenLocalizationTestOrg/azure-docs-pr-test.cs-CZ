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
# <a name="authentication-tooazure-sql-data-warehouse"></a><span data-ttu-id="1294f-103">TooAzure ověřování SQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="1294f-103">Authentication tooAzure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1294f-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="1294f-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="1294f-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="1294f-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="1294f-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="1294f-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="1294f-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="1294f-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="1294f-108">tooconnect tooSQL datového skladu, musíte zadat v zabezpečovací přihlašovací údaje pro účely ověření.</span><span class="sxs-lookup"><span data-stu-id="1294f-108">tooconnect tooSQL Data Warehouse, you must pass in security credentials for authentication purposes.</span></span> <span data-ttu-id="1294f-109">Při navazování připojení, jsou určitá nastavení připojení nakonfigurovaný jako součást navazování relace dotazu.</span><span class="sxs-lookup"><span data-stu-id="1294f-109">Upon establishing a connection, certain connection settings are configured as part of establishing your query session.</span></span>  

<span data-ttu-id="1294f-110">Další informace o zabezpečení a jak tooenable připojení tooyour datového skladu, najdete v části [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="1294f-110">For more information on security and how tooenable connections tooyour data warehouse, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>

## <a name="sql-authentication"></a><span data-ttu-id="1294f-111">Ověřování pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="1294f-111">SQL authentication</span></span>
<span data-ttu-id="1294f-112">tooconnect tooSQL datového skladu, je nutné zadat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="1294f-112">tooconnect tooSQL Data Warehouse, you must provide hello following information:</span></span>

* <span data-ttu-id="1294f-113">Plně kvalifikovaný servername</span><span class="sxs-lookup"><span data-stu-id="1294f-113">Fully qualified servername</span></span>
* <span data-ttu-id="1294f-114">Zadejte ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="1294f-114">Specify SQL authentication</span></span>
* <span data-ttu-id="1294f-115">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="1294f-115">Username</span></span>
* <span data-ttu-id="1294f-116">Heslo</span><span class="sxs-lookup"><span data-stu-id="1294f-116">Password</span></span>
* <span data-ttu-id="1294f-117">Výchozí databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="1294f-117">Default database (optional)</span></span>

<span data-ttu-id="1294f-118">Ve výchozím nastavení se připojení připojí toohello *hlavní* databáze a nejsou své databázi uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1294f-118">By default your connection connects toohello *master* database and not your user database.</span></span> <span data-ttu-id="1294f-119">tooconnect tooyour uživatele databáze, můžete zvolit toodo jednu ze dvou akcí:</span><span class="sxs-lookup"><span data-stu-id="1294f-119">tooconnect tooyour user database, you can choose toodo one of two things:</span></span>

* <span data-ttu-id="1294f-120">Zadejte výchozí databázi hello při registraci serveru na hello Průzkumník objektů SQL serveru v sadě SSDT, aplikace SSMS, nebo v připojovacím řetězci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1294f-120">Specify hello default database when registering your server with hello SQL Server Object Explorer in SSDT, SSMS, or in your application connection string.</span></span> <span data-ttu-id="1294f-121">Příklady hello vlastnosti InitialCatalog parametr pro připojení k rozhraní ODBC.</span><span class="sxs-lookup"><span data-stu-id="1294f-121">For example, include hello InitialCatalog parameter for an ODBC connection.</span></span>
* <span data-ttu-id="1294f-122">Před vytvořením relace v sadě SSDT zvýrazněte hello uživatelské databáze.</span><span class="sxs-lookup"><span data-stu-id="1294f-122">Highlight hello user database before creating a session in SSDT.</span></span>

> [!NOTE]
> <span data-ttu-id="1294f-123">Hello příkazu Transact-SQL **použití databáze;** není podporována pro změnu hello databáze pro připojení.</span><span class="sxs-lookup"><span data-stu-id="1294f-123">hello Transact-SQL statement **USE MyDatabase;** is not supported for changing hello database for a connection.</span></span> <span data-ttu-id="1294f-124">Pokyny k připojení tooSQL datového skladu s rozšířením SSDT, najdete v tématu toohello [dotazu pomocí sady Visual Studio] [ Query with Visual Studio] článku.</span><span class="sxs-lookup"><span data-stu-id="1294f-124">For guidance connecting tooSQL Data Warehouse with SSDT, refer toohello [Query with Visual Studio][Query with Visual Studio] article.</span></span>
> 
> 

## <a name="azure-active-directory-aad-authentication"></a><span data-ttu-id="1294f-125">Ověřování Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="1294f-125">Azure Active Directory (AAD) authentication</span></span>
<span data-ttu-id="1294f-126">[Azure Active Directory] [ What is Azure Active Directory] ověřování je mechanismus připojující tooMicrosoft Azure SQL Data Warehouse pomocí identit v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1294f-126">[Azure Active Directory][What is Azure Active Directory] authentication is a mechanism of connecting tooMicrosoft Azure SQL Data Warehouse by using identities in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="1294f-127">Pomocí ověřování Azure Active Directory můžete centrálně spravovat hello identity uživatelů, databáze a další služby Microsoftu v jednom centrálním místě.</span><span class="sxs-lookup"><span data-stu-id="1294f-127">With Azure Active Directory authentication, you can centrally manage hello identities of database users and other Microsoft services in one central location.</span></span> <span data-ttu-id="1294f-128">Centrální správa ID umožňuje uživatelům toomanage SQL Data Warehouse na jednom místě a zjednodušuje správu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1294f-128">Central ID management provides a single place toomanage SQL Data Warehouse users and simplifies permission management.</span></span> 

### <a name="benefits"></a><span data-ttu-id="1294f-129">Výhody</span><span class="sxs-lookup"><span data-stu-id="1294f-129">Benefits</span></span>
<span data-ttu-id="1294f-130">Azure Active Directory výhody patří:</span><span class="sxs-lookup"><span data-stu-id="1294f-130">Azure Active Directory benefits include:</span></span>

* <span data-ttu-id="1294f-131">Poskytuje alternativní tooSQL ověření serveru.</span><span class="sxs-lookup"><span data-stu-id="1294f-131">Provides an alternative tooSQL Server authentication.</span></span>
* <span data-ttu-id="1294f-132">Pomáhá zastavit šíření hello identit uživatelů serverů databáze.</span><span class="sxs-lookup"><span data-stu-id="1294f-132">Helps stop hello proliferation of user identities across database servers.</span></span>
* <span data-ttu-id="1294f-133">Umožňuje otočení heslo na jednom místě</span><span class="sxs-lookup"><span data-stu-id="1294f-133">Allows password rotation in a single place</span></span>
* <span data-ttu-id="1294f-134">Správa oprávnění databáze pomocí externí skupiny (AAD).</span><span class="sxs-lookup"><span data-stu-id="1294f-134">Manage database permissions using external (AAD) groups.</span></span>
* <span data-ttu-id="1294f-135">Eliminuje ukládání hesel povolením integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1294f-135">Eliminates storing passwords by enabling integrated Windows authentication and other forms of authentication supported by Azure Active Directory.</span></span>
* <span data-ttu-id="1294f-136">Používá obsažené databáze uživatelé tooauthenticate identity na úrovni databáze hello.</span><span class="sxs-lookup"><span data-stu-id="1294f-136">Uses contained database users tooauthenticate identities at hello database level.</span></span>
* <span data-ttu-id="1294f-137">Podporuje ověřování na základě tokenu pro aplikace připojení tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="1294f-137">Supports token-based authentication for applications connecting tooSQL Data Warehouse.</span></span>
* <span data-ttu-id="1294f-138">Podporuje Vícefaktorové ověřování prostřednictvím Universal ověřování služby Active Directory pro SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="1294f-138">Supports Multi-Factor authentication through Active Directory Universal Authentication for SQL Server Management Studio.</span></span> <span data-ttu-id="1294f-139">Popis služby Multi-Factor Authentication najdete v tématu [SSMS podpora pro Azure AD MFA s SQL Database a SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1294f-139">For a description of Multi-Factor Authentication, see [SSMS support for Azure AD MFA with SQL Database and SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1294f-140">Azure Active Directory je pořád relativně nové a má určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="1294f-140">Azure Active Directory is still relatively new and has some limitations.</span></span> <span data-ttu-id="1294f-141">tooensure, že Azure Active Directory je vhodné pro vaše prostředí, najdete v části [funkce Azure AD a omezení][Azure AD features and limitations], konkrétně hello další aspekty.</span><span class="sxs-lookup"><span data-stu-id="1294f-141">tooensure that Azure Active Directory is a good fit for your environment, see [Azure AD features and limitations][Azure AD features and limitations], specifically hello Additional considerations.</span></span>
> 
> 

### <a name="configuration-steps"></a><span data-ttu-id="1294f-142">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="1294f-142">Configuration steps</span></span>
<span data-ttu-id="1294f-143">Postupujte podle těchto kroků tooconfigure Azure Active Directory ověřování.</span><span class="sxs-lookup"><span data-stu-id="1294f-143">Follow these steps tooconfigure Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="1294f-144">Vytvořit a naplnit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1294f-144">Create and populate an Azure Active Directory</span></span>
2. <span data-ttu-id="1294f-145">Volitelné: Přidružení nebo změňte hello služby active directory, který je aktuálně přidružena předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="1294f-145">Optional: Associate or change hello active directory that is currently associated with your Azure Subscription</span></span>
3. <span data-ttu-id="1294f-146">Vytvořte správce Azure Active Directory pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="1294f-146">Create an Azure Active Directory administrator for Azure SQL Data Warehouse.</span></span>
4. <span data-ttu-id="1294f-147">Konfigurace klientských počítačů</span><span class="sxs-lookup"><span data-stu-id="1294f-147">Configure your client computers</span></span>
5. <span data-ttu-id="1294f-148">Vytvoření uživatele databáze s omezením ve vaší databázi namapované tooAzure identit AD</span><span class="sxs-lookup"><span data-stu-id="1294f-148">Create contained database users in your database mapped tooAzure AD identities</span></span>
6. <span data-ttu-id="1294f-149">Připojit tooyour datového skladu pomocí identit Azure AD</span><span class="sxs-lookup"><span data-stu-id="1294f-149">Connect tooyour data warehouse by using Azure AD identities</span></span>

<span data-ttu-id="1294f-150">Aktuálně nejsou Azure Active Directory uživatelům zobrazí v Průzkumníku objektů rozšíření SSDT.</span><span class="sxs-lookup"><span data-stu-id="1294f-150">Currently Azure Active Directory users are not shown in SSDT Object Explorer.</span></span> <span data-ttu-id="1294f-151">Jako alternativní řešení, zobrazení hello uživatelů v [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span><span class="sxs-lookup"><span data-stu-id="1294f-151">As a workaround, view hello users in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span></span>

### <a name="find-hello-details"></a><span data-ttu-id="1294f-152">Najde podrobnosti o hello</span><span class="sxs-lookup"><span data-stu-id="1294f-152">Find hello details</span></span>
* <span data-ttu-id="1294f-153">Hello kroky tooconfigure a používání ověřování Azure Active Directory jsou téměř stejné pro databázi SQL Azure a Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="1294f-153">hello steps tooconfigure and use Azure Active Directory authentication are nearly identical for Azure SQL Database and Azure SQL Data Warehouse.</span></span> <span data-ttu-id="1294f-154">Postupujte podle kroků v tématu hello podrobné hello [tooSQL připojení databáze nebo SQL Data Warehouse pomocí pomocí Azure ověřování Active Directory](../sql-database/sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1294f-154">Follow hello detailed steps in hello topic [Connecting tooSQL Database or SQL Data Warehouse By Using Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md).</span></span>
* <span data-ttu-id="1294f-155">Vytvořte vlastní databázové role a přidejte toohello role uživatele.</span><span class="sxs-lookup"><span data-stu-id="1294f-155">Create custom database roles and add users toohello roles.</span></span> <span data-ttu-id="1294f-156">Potom udělte oprávnění na podrobné úrovni toohello role.</span><span class="sxs-lookup"><span data-stu-id="1294f-156">Then grant granular permissions toohello roles.</span></span> <span data-ttu-id="1294f-157">Další informace najdete v tématu [Začínáme s oprávněními modul databáze](https://msdn.microsoft.com/library/mt667986.aspx).</span><span class="sxs-lookup"><span data-stu-id="1294f-157">For more information, see [Getting Started with Database Engine Permissions](https://msdn.microsoft.com/library/mt667986.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1294f-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1294f-158">Next steps</span></span>
<span data-ttu-id="1294f-159">toostart dotazování datového skladu pomocí sady Visual Studio a dalších aplikací, najdete v části [dotazu pomocí sady Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="1294f-159">toostart querying your data warehouse with Visual Studio and other applications, see [Query with Visual Studio][Query with Visual Studio].</span></span>

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
