---
title: "Ověřování do Azure SQL Data Warehouse | Microsoft Docs"
description: "Azure Active Directory (AAD) a SQL Server ověřování pro Azure SQL Data Warehouse."
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
ms.openlocfilehash: 92f48027051bc4aff4d6b8d66fdd6de81bba3657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authentication-to-azure-sql-data-warehouse"></a><span data-ttu-id="5fc5f-103">Ověřování do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5fc5f-103">Authentication to Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5fc5f-104">Přehled zabezpečení</span><span class="sxs-lookup"><span data-stu-id="5fc5f-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="5fc5f-105">Ověřování</span><span class="sxs-lookup"><span data-stu-id="5fc5f-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="5fc5f-106">Šifrování (portál)</span><span class="sxs-lookup"><span data-stu-id="5fc5f-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="5fc5f-107">Šifrování (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="5fc5f-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="5fc5f-108">Pro připojení k SQL Data Warehouse, musí projít v zabezpečovací přihlašovací údaje pro účely ověření.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-108">To connect to SQL Data Warehouse, you must pass in security credentials for authentication purposes.</span></span> <span data-ttu-id="5fc5f-109">Při navazování připojení, jsou určitá nastavení připojení nakonfigurovaný jako součást navazování relace dotazu.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-109">Upon establishing a connection, certain connection settings are configured as part of establishing your query session.</span></span>  

<span data-ttu-id="5fc5f-110">Další informace o zabezpečení a povolení připojení k vašemu datovému skladu naleznete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5fc5f-110">For more information on security and how to enable connections to your data warehouse, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>

## <a name="sql-authentication"></a><span data-ttu-id="5fc5f-111">Ověřování pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="5fc5f-111">SQL authentication</span></span>
<span data-ttu-id="5fc5f-112">Pro připojení k SQL Data Warehouse, je nutné zadat následující informace:</span><span class="sxs-lookup"><span data-stu-id="5fc5f-112">To connect to SQL Data Warehouse, you must provide the following information:</span></span>

* <span data-ttu-id="5fc5f-113">Plně kvalifikovaný servername</span><span class="sxs-lookup"><span data-stu-id="5fc5f-113">Fully qualified servername</span></span>
* <span data-ttu-id="5fc5f-114">Zadejte ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-114">Specify SQL authentication</span></span>
* <span data-ttu-id="5fc5f-115">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5fc5f-115">Username</span></span>
* <span data-ttu-id="5fc5f-116">Heslo</span><span class="sxs-lookup"><span data-stu-id="5fc5f-116">Password</span></span>
* <span data-ttu-id="5fc5f-117">Výchozí databáze (volitelné)</span><span class="sxs-lookup"><span data-stu-id="5fc5f-117">Default database (optional)</span></span>

<span data-ttu-id="5fc5f-118">Ve výchozím nastavení se připojení připojí k *hlavní* databáze a nejsou své databázi uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-118">By default your connection connects to the *master* database and not your user database.</span></span> <span data-ttu-id="5fc5f-119">Pokud chcete připojit ke své databázi uživatelů, můžete provést jednu ze dvou akcí:</span><span class="sxs-lookup"><span data-stu-id="5fc5f-119">To connect to your user database, you can choose to do one of two things:</span></span>

* <span data-ttu-id="5fc5f-120">Zadejte výchozí databáze při registraci serveru pomocí Průzkumníka objektů SQL Server v rozšíření SSDT, aplikace SSMS, nebo v aplikaci připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-120">Specify the default database when registering your server with the SQL Server Object Explorer in SSDT, SSMS, or in your application connection string.</span></span> <span data-ttu-id="5fc5f-121">Příklady parametr vlastnosti InitialCatalog pro připojení k rozhraní ODBC.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-121">For example, include the InitialCatalog parameter for an ODBC connection.</span></span>
* <span data-ttu-id="5fc5f-122">Před vytvořením relace v sadě SSDT zvýrazněte uživatele databáze.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-122">Highlight the user database before creating a session in SSDT.</span></span>

> [!NOTE]
> <span data-ttu-id="5fc5f-123">Příkaz Transact-SQL **použití databáze;** není podporována pro změnu databáze pro připojení.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-123">The Transact-SQL statement **USE MyDatabase;** is not supported for changing the database for a connection.</span></span> <span data-ttu-id="5fc5f-124">Pokyny k připojení k SQL Data Warehouse s rozšířením SSDT, najdete v tématu [dotazu pomocí sady Visual Studio] [ Query with Visual Studio] článku.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-124">For guidance connecting to SQL Data Warehouse with SSDT, refer to the [Query with Visual Studio][Query with Visual Studio] article.</span></span>
> 
> 

## <a name="azure-active-directory-aad-authentication"></a><span data-ttu-id="5fc5f-125">Ověřování Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="5fc5f-125">Azure Active Directory (AAD) authentication</span></span>
<span data-ttu-id="5fc5f-126">[Azure Active Directory] [ What is Azure Active Directory] ověřování je mechanismus připojit k Microsoft Azure SQL Data Warehouse pomocí identit v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5fc5f-126">[Azure Active Directory][What is Azure Active Directory] authentication is a mechanism of connecting to Microsoft Azure SQL Data Warehouse by using identities in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="5fc5f-127">Pomocí ověřování Azure Active Directory můžete centrálně spravovat identity uživatelů, databáze a další služby Microsoftu v jednom centrálním místě.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-127">With Azure Active Directory authentication, you can centrally manage the identities of database users and other Microsoft services in one central location.</span></span> <span data-ttu-id="5fc5f-128">Centrální správa ID poskytuje jednotné místo pro správu uživatelů SQL Data Warehouse a zjednodušuje správu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-128">Central ID management provides a single place to manage SQL Data Warehouse users and simplifies permission management.</span></span> 

### <a name="benefits"></a><span data-ttu-id="5fc5f-129">Výhody</span><span class="sxs-lookup"><span data-stu-id="5fc5f-129">Benefits</span></span>
<span data-ttu-id="5fc5f-130">Azure Active Directory výhody patří:</span><span class="sxs-lookup"><span data-stu-id="5fc5f-130">Azure Active Directory benefits include:</span></span>

* <span data-ttu-id="5fc5f-131">Poskytuje alternativu k ověřování serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-131">Provides an alternative to SQL Server authentication.</span></span>
* <span data-ttu-id="5fc5f-132">Pomáhá zastavit šíření identit uživatelů mezi servery databáze.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-132">Helps stop the proliferation of user identities across database servers.</span></span>
* <span data-ttu-id="5fc5f-133">Umožňuje otočení heslo na jednom místě</span><span class="sxs-lookup"><span data-stu-id="5fc5f-133">Allows password rotation in a single place</span></span>
* <span data-ttu-id="5fc5f-134">Správa oprávnění databáze pomocí externí skupiny (AAD).</span><span class="sxs-lookup"><span data-stu-id="5fc5f-134">Manage database permissions using external (AAD) groups.</span></span>
* <span data-ttu-id="5fc5f-135">Eliminuje ukládání hesel povolením integrované ověřování systému Windows a jiných forem ověřování podporovaných službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-135">Eliminates storing passwords by enabling integrated Windows authentication and other forms of authentication supported by Azure Active Directory.</span></span>
* <span data-ttu-id="5fc5f-136">Uživatelé databáze obsažené používá k ověření identity na úrovni databáze.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-136">Uses contained database users to authenticate identities at the database level.</span></span>
* <span data-ttu-id="5fc5f-137">Podporuje ověřování na základě tokenu pro připojení k SQL Data Warehouse aplikace.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-137">Supports token-based authentication for applications connecting to SQL Data Warehouse.</span></span>
* <span data-ttu-id="5fc5f-138">Podporuje Vícefaktorové ověřování prostřednictvím Universal ověřování služby Active Directory pro SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-138">Supports Multi-Factor authentication through Active Directory Universal Authentication for SQL Server Management Studio.</span></span> <span data-ttu-id="5fc5f-139">Popis služby Multi-Factor Authentication najdete v tématu [SSMS podpora pro Azure AD MFA s SQL Database a SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="5fc5f-139">For a description of Multi-Factor Authentication, see [SSMS support for Azure AD MFA with SQL Database and SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5fc5f-140">Azure Active Directory je pořád relativně nové a má určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-140">Azure Active Directory is still relatively new and has some limitations.</span></span> <span data-ttu-id="5fc5f-141">Pro zajištění, že Azure Active Directory je vhodné pro vaše prostředí, naleznete v [funkce Azure AD a omezení][Azure AD features and limitations], konkrétně další aspekty.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-141">To ensure that Azure Active Directory is a good fit for your environment, see [Azure AD features and limitations][Azure AD features and limitations], specifically the Additional considerations.</span></span>
> 
> 

### <a name="configuration-steps"></a><span data-ttu-id="5fc5f-142">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="5fc5f-142">Configuration steps</span></span>
<span data-ttu-id="5fc5f-143">Postupujte podle těchto kroků nakonfigurujete ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-143">Follow these steps to configure Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="5fc5f-144">Vytvořit a naplnit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5fc5f-144">Create and populate an Azure Active Directory</span></span>
2. <span data-ttu-id="5fc5f-145">Volitelné: Přidružení nebo změňte služby active directory, který je aktuálně přidružena předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="5fc5f-145">Optional: Associate or change the active directory that is currently associated with your Azure Subscription</span></span>
3. <span data-ttu-id="5fc5f-146">Vytvořte správce Azure Active Directory pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-146">Create an Azure Active Directory administrator for Azure SQL Data Warehouse.</span></span>
4. <span data-ttu-id="5fc5f-147">Konfigurace klientských počítačů</span><span class="sxs-lookup"><span data-stu-id="5fc5f-147">Configure your client computers</span></span>
5. <span data-ttu-id="5fc5f-148">Vytvoření databáze s omezením uživatelů ve vaší databázi namapované na identit Azure AD</span><span class="sxs-lookup"><span data-stu-id="5fc5f-148">Create contained database users in your database mapped to Azure AD identities</span></span>
6. <span data-ttu-id="5fc5f-149">Připojení k vašemu datovému skladu pomocí identit Azure AD</span><span class="sxs-lookup"><span data-stu-id="5fc5f-149">Connect to your data warehouse by using Azure AD identities</span></span>

<span data-ttu-id="5fc5f-150">Aktuálně nejsou Azure Active Directory uživatelům zobrazí v Průzkumníku objektů rozšíření SSDT.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-150">Currently Azure Active Directory users are not shown in SSDT Object Explorer.</span></span> <span data-ttu-id="5fc5f-151">Jako alternativní řešení, zobrazit uživateli v [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fc5f-151">As a workaround, view the users in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span></span>

### <a name="find-the-details"></a><span data-ttu-id="5fc5f-152">Najít podrobnosti</span><span class="sxs-lookup"><span data-stu-id="5fc5f-152">Find the details</span></span>
* <span data-ttu-id="5fc5f-153">Postup pro konfiguraci a použití ověřování Azure Active Directory jsou téměř stejné pro databázi SQL Azure a Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-153">The steps to configure and use Azure Active Directory authentication are nearly identical for Azure SQL Database and Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5fc5f-154">Podrobné pokyny v tématu [připojení k SQL Database nebo SQL Data Warehouse pomocí pomocí Azure ověřování služby Active Directory](../sql-database/sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="5fc5f-154">Follow the detailed steps in the topic [Connecting to SQL Database or SQL Data Warehouse By Using Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md).</span></span>
* <span data-ttu-id="5fc5f-155">Vytvořte vlastní databázové role a přidání uživatelů do rolí.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-155">Create custom database roles and add users to the roles.</span></span> <span data-ttu-id="5fc5f-156">Potom udělte oprávnění na podrobné úrovni do rolí.</span><span class="sxs-lookup"><span data-stu-id="5fc5f-156">Then grant granular permissions to the roles.</span></span> <span data-ttu-id="5fc5f-157">Další informace najdete v tématu [Začínáme s oprávněními modul databáze](https://msdn.microsoft.com/library/mt667986.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fc5f-157">For more information, see [Getting Started with Database Engine Permissions](https://msdn.microsoft.com/library/mt667986.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5fc5f-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5fc5f-158">Next steps</span></span>
<span data-ttu-id="5fc5f-159">Pokud chcete spustit dotaz na vaše data warehouse pomocí sady Visual Studio a dalších aplikací, najdete v části [dotazu pomocí sady Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="5fc5f-159">To start querying your data warehouse with Visual Studio and other applications, see [Query with Visual Studio][Query with Visual Studio].</span></span>

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
