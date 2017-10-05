---
title: "Podmíněný přístup – Azure SQL Database a Data Warehouse | Microsoft dokumentů"
description: "Postup konfigurace podmíněného přístupu pro Azure SQL Database a datového skladu."
services: sql-database
author: BYHAM
manager: jhubbard
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: rickbyh
ms.openlocfilehash: 0dcec61c03a84197e2c351761c743683caa98a06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a><span data-ttu-id="87cb3-103">Podmíněný přístup (MFA) s Azure SQL Database a datového skladu</span><span class="sxs-lookup"><span data-stu-id="87cb3-103">Conditional Access (MFA) with Azure SQL Database and Data Warehouse</span></span>  

<span data-ttu-id="87cb3-104">SQL Database a SQL Data Warehouse podporují podmíněný přístup společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="87cb3-104">Both SQL Database and SQL Data Warehouse support Microsoft Conditional Access.</span></span> <span data-ttu-id="87cb3-105">Následující kroky ukazují, jak nakonfigurovat databázi SQL k vynucení zásad podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="87cb3-105">The following steps show how to configure SQL Database to enforce a Conditional Access policy.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="87cb3-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87cb3-106">Prerequisites</span></span>  
- <span data-ttu-id="87cb3-107">Je nutné nakonfigurovat SQL Database nebo SQL Data Warehouse pro podporu ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="87cb3-107">You must configure your SQL Database or SQL Data Warehouse to support Azure Active Directory authentication.</span></span> <span data-ttu-id="87cb3-108">Konkrétní pokyny najdete v tématu [konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](sql-database-aad-authentication-configure.md).</span><span class="sxs-lookup"><span data-stu-id="87cb3-108">For specific steps, see [Configure and manage Azure Active Directory authentication with SQL Database or SQL Data Warehouse](sql-database-aad-authentication-configure.md).</span></span>  
- <span data-ttu-id="87cb3-109">Pokud je povoleno ověřování Multi-Factor authentication, je nutné připojit s v podporovaných nástroji, jako nejnovější aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="87cb3-109">When multi-factor authentication is enabled, you must connect with at supported tool, such as the latest SSMS.</span></span> <span data-ttu-id="87cb3-110">Další informace najdete v tématu [nakonfigurovat databázi SQL Azure Multi-Factor authentication pro SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).</span><span class="sxs-lookup"><span data-stu-id="87cb3-110">For more information, see [Configure Azure SQL Database multi-factor authentication for SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).</span></span>  

## <a name="configure-ca-for-azure-sql-dbdw"></a><span data-ttu-id="87cb3-111">Konfigurace certifikační Autority pro Azure SQL DB nebo datového skladu</span><span class="sxs-lookup"><span data-stu-id="87cb3-111">Configure CA for Azure SQL DB/DW</span></span>  
1.  <span data-ttu-id="87cb3-112">Přihlaste se k portálu, vyberte **Azure Active Directory**a potom vyberte **podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="87cb3-112">Sign in to the Portal, select **Azure Active Directory**, and then select **Conditional access**.</span></span> <span data-ttu-id="87cb3-113">Další informace najdete v tématu [technické informace o Azure Active Directory podmíněného přístupu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference).</span><span class="sxs-lookup"><span data-stu-id="87cb3-113">For more information, see [Azure Active Directory Conditional Access technical reference](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference).</span></span>  
  <span data-ttu-id="87cb3-114">![okno podmíněného přístupu](./media/sql-database-conditional-access/conditional-access-blade.png)</span><span class="sxs-lookup"><span data-stu-id="87cb3-114">![conditional access blade](./media/sql-database-conditional-access/conditional-access-blade.png)</span></span> 
     
2.  <span data-ttu-id="87cb3-115">V **zásady podmíněného přístupu** okně klikněte na tlačítko **nové zásady**, zadejte název a potom klikněte na **konfigurovat pravidla**.</span><span class="sxs-lookup"><span data-stu-id="87cb3-115">In the **Conditional Access-Policies** blade, click **New policy**, provide a name, and then click **Configure rules**.</span></span>  
3.  <span data-ttu-id="87cb3-116">V části **přiřazení**, vyberte **uživatelů a skupin**, zkontrolujte **vyberte uživatele a skupiny**a poté vyberte uživatele nebo skupinu pro podmíněný přístup.</span><span class="sxs-lookup"><span data-stu-id="87cb3-116">Under **Assignments**, select **Users and groups**, check **Select users and groups**, and then select the user or group for conditional access.</span></span> <span data-ttu-id="87cb3-117">Klikněte na tlačítko **vyberte**a potom klikněte na **provádí** tak, aby přijímal váš výběr.</span><span class="sxs-lookup"><span data-stu-id="87cb3-117">Click **Select**, and then click **Done** to accept your selection.</span></span>  
  <span data-ttu-id="87cb3-118">![Vyberte uživatele a skupiny](./media/sql-database-conditional-access/select-users-and-groups.png)</span><span class="sxs-lookup"><span data-stu-id="87cb3-118">![select users and groups](./media/sql-database-conditional-access/select-users-and-groups.png)</span></span>  

4.  <span data-ttu-id="87cb3-119">Vyberte **cloudových aplikací**, klikněte na tlačítko **vybrat aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87cb3-119">Select **Cloud apps**, click **Select apps**.</span></span> <span data-ttu-id="87cb3-120">Zobrazí všechny aplikace dostupné pro podmíněný přístup.</span><span class="sxs-lookup"><span data-stu-id="87cb3-120">You see all apps available for conditional access.</span></span> <span data-ttu-id="87cb3-121">Vyberte **Azure SQL Database**, v dolní části klikněte na tlačítko **vyberte**a potom klikněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="87cb3-121">Select **Azure SQL Database**, at the bottom click **Select**, and then click **Done**.</span></span>  
  <span data-ttu-id="87cb3-122">![Vyberte databázi SQL](./media/sql-database-conditional-access/select-sql-database.png)</span><span class="sxs-lookup"><span data-stu-id="87cb3-122">![select SQL Database](./media/sql-database-conditional-access/select-sql-database.png)</span></span>  
  <span data-ttu-id="87cb3-123">Pokud nemůžete najít **Azure SQL Database** uvedené v následujícím snímku obrazovky třetí, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87cb3-123">If you can’t find **Azure SQL Database** listed in the following third screen shot, complete the following steps:</span></span>   
  - <span data-ttu-id="87cb3-124">Přihlaste se k vaší instanci Azure SQL DB nebo DW pomocí aplikace SSMS pomocí účtu správce AAD.</span><span class="sxs-lookup"><span data-stu-id="87cb3-124">Sign in to your Azure SQL DB/DW instance using SSMS with an AAD admin account.</span></span>  
  - <span data-ttu-id="87cb3-125">Spustit `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.</span><span class="sxs-lookup"><span data-stu-id="87cb3-125">Execute `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.</span></span>  
  - <span data-ttu-id="87cb3-126">Přihlaste se k AAD a ověřte, že Azure SQL Database a datový sklad jsou uvedeny v aplikacích v vaší AAD.</span><span class="sxs-lookup"><span data-stu-id="87cb3-126">Sign in to AAD and verify that Azure SQL Database and Data Warehouse are listed in the applications in your AAD.</span></span>  

5.  <span data-ttu-id="87cb3-127">Vyberte **přístup k ovládacím prvkům**, vyberte **Grant**a potom zkontrolujte zásady, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="87cb3-127">Select **Access controls**, select **Grant**, and then check the policy you want to apply.</span></span> <span data-ttu-id="87cb3-128">V tomto příkladu jsme vyberte **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="87cb3-128">For this example, we select **Require multi-factor authentication**.</span></span>  
  <span data-ttu-id="87cb3-129">![Vyberte udělení přístupu](./media/sql-database-conditional-access/grant-access.png)</span><span class="sxs-lookup"><span data-stu-id="87cb3-129">![select grant access](./media/sql-database-conditional-access/grant-access.png)</span></span>  

## <a name="summary"></a><span data-ttu-id="87cb3-130">Souhrn</span><span class="sxs-lookup"><span data-stu-id="87cb3-130">Summary</span></span>  
<span data-ttu-id="87cb3-131">Vybrané aplikace (Azure SQL Database) povolení pro připojení k Azure SQL DB nebo datového skladu pomocí Azure AD Premium, teď vynucuje vybrané zásady podmíněného přístupu, **požadované služby Multi-Factor authentication.**</span><span class="sxs-lookup"><span data-stu-id="87cb3-131">The selected application (Azure SQL Database) allowing to connect to Azure SQL DB/DW using Azure AD Premium, now enforces the selected Conditional Access policy, **Required multi-factor authentication.**</span></span>  
<span data-ttu-id="87cb3-132">Otázky týkající se Azure SQL Database a datového skladu týkající se služby Multi-Factor authentication, vám poskytne MFAforSQLDB@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="87cb3-132">For questions about Azure SQL Database and Data Warehouse regarding multi-factor authentication, contact MFAforSQLDB@microsoft.com.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="87cb3-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87cb3-133">Next steps</span></span>  

<span data-ttu-id="87cb3-134">Podívejte se kurz [zabezpečení vaší databázi SQL Azure](sql-database-security-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="87cb3-134">For a tutorial, see [Secure your Azure SQL Database](sql-database-security-tutorial.md).</span></span>