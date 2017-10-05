---
title: "Správa osobních údajů v Microsoft Azure | Microsoft Docs"
description: "Pokyny o tom, jak opravit, aktualizovat, odstranit a exportujte osobních údajů v Azure Active Directory a Azure SQL Database"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: b04c745feb710d3d1d8a1fce23807168d6e6fa3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a><span data-ttu-id="a7747-103">Správa osobních údajů v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a7747-103">Manage personal data in Microsoft Azure</span></span>

<span data-ttu-id="a7747-104">Tento článek obsahuje pokyny o tom, jak opravit, aktualizovat, odstranit a exportujte osobních údajů v Azure Active Directory a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a7747-104">This article provides guidance on how to correct, update, delete, and export personal data in Azure Active Directory and Azure SQL Database.</span></span>

## <a name="scenario"></a><span data-ttu-id="a7747-105">Scénář</span><span class="sxs-lookup"><span data-stu-id="a7747-105">Scenario</span></span>

<span data-ttu-id="a7747-106">Na základě Dublin společnost poskytuje centrální místo pro výkonných cílové svatby v Irsku a po celém světě pro obě místní i mezinárodní zákazníka základní.</span><span class="sxs-lookup"><span data-stu-id="a7747-106">A Dublin-based company provides one-stop shopping for high end destination weddings in Ireland and around the world for both a local and international customer base.</span></span> <span data-ttu-id="a7747-107">Mají pobočky, zákazníky, zaměstnanci a dodavatelé umístěné po celém světě plně služby místa, které nabízejí.</span><span class="sxs-lookup"><span data-stu-id="a7747-107">They have offices, customers, employees, and vendors located around the world to fully service the venues they offer.</span></span>

<span data-ttu-id="a7747-108">Mezi mnoha dalším položkám uchovává informace společnosti o RSVPs, které zahrnují alergií a podávání předvolby.</span><span class="sxs-lookup"><span data-stu-id="a7747-108">Among many other items, the company keeps track of RSVPs that include food allergies and dietary preferences.</span></span> <span data-ttu-id="a7747-109">Hosté svatební můžete zaregistrovat pro různé aktivity, například na koni JÍZDA, procházení webu, lodních lyžovačku atd. a dokonce vzájemné interakce na webové stránce Centrální během měsíců vedoucích k události.</span><span class="sxs-lookup"><span data-stu-id="a7747-109">Wedding guests can register for various activities such as horseback riding, surfing, boat rides, etc., and even interact with one another on a central web page during the months leading up to the event.</span></span> <span data-ttu-id="a7747-110">Společnost shromažďuje ze zaměstnanců, dodavatelů, zákazníků a svatební hosté osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="a7747-110">The company collects personal information from employees, vendors, customers, and wedding guests.</span></span> <span data-ttu-id="a7747-111">Z důvodu mezinárodní povahu podnikání společnosti musí být v souladu s více úrovněmi nařízení.</span><span class="sxs-lookup"><span data-stu-id="a7747-111">Because of the international nature of the business the company must comply with multiple levels of regulation.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="a7747-112">Popis problému</span><span class="sxs-lookup"><span data-stu-id="a7747-112">Problem statement</span></span>

- <span data-ttu-id="a7747-113">Data správců musí být schopen správné nesprávné informace a aktualizace neúplný nebo změna osobní informace.</span><span class="sxs-lookup"><span data-stu-id="a7747-113">Data admins must be able to correct inaccurate personal information and update incomplete or changing personal information.</span></span>

- <span data-ttu-id="a7747-114">Třeba admins data musí být schopen odstranit osobní informace na žádost subjektu data.</span><span class="sxs-lookup"><span data-stu-id="a7747-114">Data admins need must be able to delete personal information upon the request of a data subject.</span></span>

- <span data-ttu-id="a7747-115">Data správců je nutné exportovat data a zadejte předmět data ve formátu běžné, strukturovaných při jeho žádost.</span><span class="sxs-lookup"><span data-stu-id="a7747-115">Data admins need to export data and provide it to a data subject in a common, structured format upon his or her request.</span></span>

## <a name="company-goals"></a><span data-ttu-id="a7747-116">Cíle společnosti</span><span class="sxs-lookup"><span data-stu-id="a7747-116">Company goals</span></span>

- <span data-ttu-id="a7747-117">Nesprávné nebo neúplné zákazníka, svatební hosta, zaměstnance a dodavatele osobní údaje, musí být opraveny nebo aktualizovány v Azure Active Directory a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a7747-117">Inaccurate or incomplete customer, wedding guest, employee, and vendor personal information must be corrected or updated in Azure Active Directory and Azure SQL Database.</span></span>

- <span data-ttu-id="a7747-118">Osobní údaje třeba jej odstranit v Azure Active Directory a Azure SQL Database na žádost subjektu data.</span><span class="sxs-lookup"><span data-stu-id="a7747-118">Personal information must be deleted in Azure Active Directory and Azure SQL Database upon the request of a data subject.</span></span>

- <span data-ttu-id="a7747-119">Osobní data musí být exportován ve formátu běžné, strukturovaných při žádosti o subjektu data.</span><span class="sxs-lookup"><span data-stu-id="a7747-119">Personal data must be exported in a common, structured format upon the request of a data subject.</span></span>

## <a name="solutions"></a><span data-ttu-id="a7747-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="a7747-120">Solutions</span></span>

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a><span data-ttu-id="a7747-121">Azure Active Directory: Tato napravila nebo opravte nesprávné nebo neúplné osobní údaje a vymazání a odstranění dat nebo uživatelských profilů</span><span class="sxs-lookup"><span data-stu-id="a7747-121">Azure Active Directory: rectify/correct inaccurate or incomplete personal data and erase/delete personal data/user profiles</span></span>

<span data-ttu-id="a7747-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) je společnosti Microsoft založená na cloudu, víceklientské adresáře a identity management service.</span><span class="sxs-lookup"><span data-stu-id="a7747-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) is Microsoft’s cloud-based, multi-tenant directory and identity management service.</span></span>
<span data-ttu-id="a7747-123">Opravte, aktualizovat nebo odstranit zákazníků a profily uživatelů a pracovní informace o uživateli, které obsahují osobní údaje, jako je například název, název pracovní, adresa nebo telefonní číslo uživatele v vaše [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) prostředí pomocí [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a7747-123">You can correct, update, or delete customer and employee user profiles and user work information that contain personal data, such as a user’s name, work title, address, or phone number, in your [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) environment by using the [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="a7747-124">Přihlaste se pod účtem, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="a7747-124">You must sign in with an account that’s a global admin for the directory.</span></span>

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a><span data-ttu-id="a7747-125">Jak opravit nebo aktualizovat uživatelský profil a pracovní informace v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a7747-125">How do I correct or update user profile and work information in Azure Active Directory?</span></span>

1. <span data-ttu-id="a7747-126">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="a7747-126">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>

2. <span data-ttu-id="a7747-127">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7747-127">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

    ![média nebo image1.png](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="a7747-129">Na **uživatelů a skupin** vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a7747-129">On the **Users and groups** blade, select **Users**.</span></span>

    ![média nebo image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="a7747-131">Na **uživatelé a skupiny - Uživatelé** okně vyberte uživatele ze seznamu a poté v okně pro vybraného uživatele, vyberte možnost **profil** zobrazíte informace o profilu uživatele, které musí být opraveny nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a7747-131">On the **Users and groups - Users** blade, select a user from the list, and then, on the blade for the selected user, select **Profile** to view the user profile information that needs to be corrected or updated.</span></span>

    ![média nebo image3.png](media/manage-personal-data-azure/image005.png)

5. <span data-ttu-id="a7747-133">Opravte nebo aktualizujte informace a pak na panelu příkazů vyberte **uložit.**</span><span class="sxs-lookup"><span data-stu-id="a7747-133">Correct or update the information, and then, in the command bar, select **Save.**</span></span>

6.  <span data-ttu-id="a7747-134">V okně pro vybraného uživatele, vyberte **informace o pracovních** zobrazíte informace o práci uživatele vyžadující opravu či aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a7747-134">On the blade for the selected user, select **Work Info** to view user work information that needs to be corrected or updated.</span></span>

    ![média nebo image4.png](media/manage-personal-data-azure/image007.png)

7. <span data-ttu-id="a7747-136">Opravte nebo aktualizovat informace o práci uživatele a pak na panelu příkazů vyberte **uložit.**</span><span class="sxs-lookup"><span data-stu-id="a7747-136">Correct or update the user work information, and then, in the command bar, select **Save.**</span></span>

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a><span data-ttu-id="a7747-137">Jak lze odstranit profil uživatele v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a7747-137">How do I delete a user profile in Azure Active Directory?</span></span>

1. <span data-ttu-id="a7747-138">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="a7747-138">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>

2. <span data-ttu-id="a7747-139">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7747-139">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

    ![](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="a7747-140">Na **uživatelů a skupin** vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a7747-140">On the **Users and groups** blade, select **Users**.</span></span>

    ![média nebo image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="a7747-142">V okně **Uživatelé a skupiny – Uživatelé** vyberte uživatele ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="a7747-142">On the **Users and groups - Users** blade, select a user from the list.</span></span>

    ![média nebo image3.png](media/manage-personal-data-azure/image007.png)

5. <span data-ttu-id="a7747-144">V okně pro vybraného uživatele, vyberte **přehled**a potom na panelu příkazů vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a7747-144">On the blade for the selected user, select **Overview**, and then in the command bar, select **Delete**.</span></span>

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a><span data-ttu-id="a7747-145">SQL Database: nápravě nebo opravte nesprávné nebo neúplné osobní údaje; smazání nebo odstranění osobních údajů; Exportovat osobní údaje</span><span class="sxs-lookup"><span data-stu-id="a7747-145">SQL Database: rectify/correct inaccurate or incomplete personal data; erase/delete personal data; export personal data</span></span> 

<span data-ttu-id="a7747-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) je databáze cloudu, která pomáhá vývojářům vytvářet a udržovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7747-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) is a cloud database that helps developers build and maintain applications.</span></span>

<span data-ttu-id="a7747-147">Osobní data můžete aktualizovat v [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) pomocí standardní dotazy SQL a můžete také odstranit.</span><span class="sxs-lookup"><span data-stu-id="a7747-147">Personal data can be updated in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) using standard SQL queries, and it can also be deleted.</span></span> <span data-ttu-id="a7747-148">Kromě toho je možné exportovat osobní data z databáze SQL pomocí různých metod, včetně serveru SQL Azure import a export průvodce a v různých formátech, včetně souboru BACPAC souboru.</span><span class="sxs-lookup"><span data-stu-id="a7747-148">Additionally, personal data can be exported from SQL Database using a variety of methods, including the Azure SQL Server import and export wizard, and in a variety of formats, including a BACPAC file.</span></span>

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a><span data-ttu-id="a7747-149">Jak opravit, aktualizovat a vymazat osobní data v databázi SQL?</span><span class="sxs-lookup"><span data-stu-id="a7747-149">How do I correct, update, or erase personal data in SQL Database?</span></span>

<span data-ttu-id="a7747-150">Zjistěte, jak odstranit nebo aktualizovat osobní data v databázi SQL, najdete [aktualizace (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [aktualizovat Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [aktualizace pomocí výrazu běžné tabulky](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), nebo [Aktualizovat zápisu Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a7747-150">To learn how to correct or update personal data in SQL Database, visit the [Update (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [Update Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [Update with Common Table Expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), or [Update Write Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) documentation.</span></span>

<span data-ttu-id="a7747-151">Zjistěte, jak odstranit osobní data v databázi SQL, najdete [odstranit (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a7747-151">To learn how to delete personal data in SQL Database, visit the [Delete (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) documentation.</span></span>

#### <a name="how-do-i-export-personal-data-to-a-bacpac-file-in-sql-database"></a><span data-ttu-id="a7747-152">Jak exportovat osobní data do souboru BACPAC souboru v databázi SQL?</span><span class="sxs-lookup"><span data-stu-id="a7747-152">How do I export personal data to a BACPAC file in SQL Database?</span></span>

<span data-ttu-id="a7747-153">Soubor souboru BACPAC obsahuje data databáze SQL a metadata a je soubor zip s příponou souboru BACPAC.</span><span class="sxs-lookup"><span data-stu-id="a7747-153">A BACPAC file includes the SQL Database data and metadata and is a zip file with a BACPAC extension.</span></span> <span data-ttu-id="a7747-154">To lze provést pomocí [portál Azure](https://portal.azure.com/), SQLPackage nástroj příkazového řádku, SQL Server Management Studio (SSMS) nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a7747-154">This can be done using the [Azure portal](https://portal.azure.com/), the SQLPackage command-line utility, SQL Server Management Studio (SSMS), or PowerShell.</span></span>

<span data-ttu-id="a7747-155">Zjistěte, jak exportovat data do souboru BACPAC souboru, najdete [exportovat do souboru BACPAC souboru Azure SQL database](https://docs.microsoft.com/azure/sql-database/sql-database-export) stránku, která obsahuje podrobné pokyny pro jednotlivé metody uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="a7747-155">To learn how to export data to a BACPAC file, visit the [Export an Azure SQL database to a BACPAC file](https://docs.microsoft.com/azure/sql-database/sql-database-export) page, which includes detailed instructions for each method listed above.</span></span>

<span data-ttu-id="a7747-156">Jak exportovat osobní data z SQL Database přes SQL Server importovat a exportovat Průvodce?</span><span class="sxs-lookup"><span data-stu-id="a7747-156">How do I export personal data from SQL Database with the SQL Server Import and Export Wizard?</span></span>

<span data-ttu-id="a7747-157">Tento průvodce umožňuje kopírování dat ze zdrojového do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="a7747-157">This wizard helps you copy data from a source to a destination.</span></span> <span data-ttu-id="a7747-158">Úvod do průvodce, včetně toho, jak se dá stáhnout, informace o oprávnění a jak získat nápovědu k nástroji, přejděte [Import a Export dat s Microsoft SQL Server importovat a exportovat průvodce](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) webové stránky.</span><span class="sxs-lookup"><span data-stu-id="a7747-158">For an introduction to the wizard, including how to get it, permissions information, and how to get help with the tool, visit the [Import and Export Data with the SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) web page.</span></span>

<span data-ttu-id="a7747-159">Přehled kroků průvodce, přejděte [kroky v Průvodci Export a Import serveru SQL](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) webové stránky.</span><span class="sxs-lookup"><span data-stu-id="a7747-159">For an overview of steps for the wizard, visit the [Steps in the SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) web page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7747-160">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="a7747-160">Next Steps:</span></span>

[<span data-ttu-id="a7747-161">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a7747-161">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[<span data-ttu-id="a7747-162">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a7747-162">Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

