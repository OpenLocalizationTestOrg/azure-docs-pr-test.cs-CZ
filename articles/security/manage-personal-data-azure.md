---
title: "aaaManage osobních údajů v Microsoft Azure | Microsoft Docs"
description: "informace o tom, jak toocorrect, aktualizovat, odstranit a exportujte osobních údajů v Azure Active Directory a Azure SQL Database"
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
ms.openlocfilehash: 032f70d32377cb9395cb2c35c27dad05001537c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a><span data-ttu-id="87c01-103">Správa osobních údajů v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="87c01-103">Manage personal data in Microsoft Azure</span></span>

<span data-ttu-id="87c01-104">Tento článek obsahuje pokyny k jak toocorrect, aktualizovat, odstranit a exportujte osobních údajů v Azure Active Directory a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="87c01-104">This article provides guidance on how toocorrect, update, delete, and export personal data in Azure Active Directory and Azure SQL Database.</span></span>

## <a name="scenario"></a><span data-ttu-id="87c01-105">Scénář</span><span class="sxs-lookup"><span data-stu-id="87c01-105">Scenario</span></span>

<span data-ttu-id="87c01-106">Na základě Dublin společnost poskytuje centrální místo pro výkonných cílové svatby v Irsku a kolem hello, world pro obě na základní místní i mezinárodní zákazníka.</span><span class="sxs-lookup"><span data-stu-id="87c01-106">A Dublin-based company provides one-stop shopping for high end destination weddings in Ireland and around hello world for both a local and international customer base.</span></span> <span data-ttu-id="87c01-107">Mají pobočky, zákazníky, zaměstnanci a dodavatelé nachází kolem hello world toofully služby hello místa, které nabízejí.</span><span class="sxs-lookup"><span data-stu-id="87c01-107">They have offices, customers, employees, and vendors located around hello world toofully service hello venues they offer.</span></span>

<span data-ttu-id="87c01-108">Mezi mnoha dalším položkám uchovává informace společnosti hello o RSVPs, které zahrnují alergií a podávání předvolby.</span><span class="sxs-lookup"><span data-stu-id="87c01-108">Among many other items, hello company keeps track of RSVPs that include food allergies and dietary preferences.</span></span> <span data-ttu-id="87c01-109">Hosté svatební můžete zaregistrovat pro různé aktivity, například na koni JÍZDA, procházení webu, lodních lyžovačku atd. a dokonce vzájemné interakce na webové stránce Centrální během hello měsíců až toohello událostí.</span><span class="sxs-lookup"><span data-stu-id="87c01-109">Wedding guests can register for various activities such as horseback riding, surfing, boat rides, etc., and even interact with one another on a central web page during hello months leading up toohello event.</span></span> <span data-ttu-id="87c01-110">Hello společnosti shromažďuje z zaměstnanců, dodavatelů, zákazníků a svatební hosté osobní informace.</span><span class="sxs-lookup"><span data-stu-id="87c01-110">hello company collects personal information from employees, vendors, customers, and wedding guests.</span></span> <span data-ttu-id="87c01-111">Z důvodu hello mezinárodní povaha hello firmy hello společnosti musí být v souladu s více úrovněmi nařízení.</span><span class="sxs-lookup"><span data-stu-id="87c01-111">Because of hello international nature of hello business hello company must comply with multiple levels of regulation.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="87c01-112">Popis problému</span><span class="sxs-lookup"><span data-stu-id="87c01-112">Problem statement</span></span>

- <span data-ttu-id="87c01-113">Data správců musí být schopný toocorrect nesprávné osobní informace a aktualizace neúplný nebo změna osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="87c01-113">Data admins must be able toocorrect inaccurate personal information and update incomplete or changing personal information.</span></span>

- <span data-ttu-id="87c01-114">Třeba admins data musí být schopný toodelete osobní údaje na vyžádání hello subjektu data.</span><span class="sxs-lookup"><span data-stu-id="87c01-114">Data admins need must be able toodelete personal information upon hello request of a data subject.</span></span>

- <span data-ttu-id="87c01-115">Data admins potřebovat tooexport dat a poskytnout ho subjektu tooa data ve formátu běžné, strukturovaných při jeho žádost.</span><span class="sxs-lookup"><span data-stu-id="87c01-115">Data admins need tooexport data and provide it tooa data subject in a common, structured format upon his or her request.</span></span>

## <a name="company-goals"></a><span data-ttu-id="87c01-116">Cíle společnosti</span><span class="sxs-lookup"><span data-stu-id="87c01-116">Company goals</span></span>

- <span data-ttu-id="87c01-117">Nesprávné nebo neúplné zákazníka, svatební hosta, zaměstnance a dodavatele osobní údaje, musí být opraveny nebo aktualizovány v Azure Active Directory a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="87c01-117">Inaccurate or incomplete customer, wedding guest, employee, and vendor personal information must be corrected or updated in Azure Active Directory and Azure SQL Database.</span></span>

- <span data-ttu-id="87c01-118">Osobní údaje, třeba jej odstranit v Azure Active Directory a Azure SQL Database na vyžádání hello subjektu data.</span><span class="sxs-lookup"><span data-stu-id="87c01-118">Personal information must be deleted in Azure Active Directory and Azure SQL Database upon hello request of a data subject.</span></span>

- <span data-ttu-id="87c01-119">Osobní data musí být exportován ve formátu běžné, strukturovaných na vyžádání hello subjektu data.</span><span class="sxs-lookup"><span data-stu-id="87c01-119">Personal data must be exported in a common, structured format upon hello request of a data subject.</span></span>

## <a name="solutions"></a><span data-ttu-id="87c01-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="87c01-120">Solutions</span></span>

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a><span data-ttu-id="87c01-121">Azure Active Directory: Tato napravila nebo opravte nesprávné nebo neúplné osobní údaje a vymazání a odstranění dat nebo uživatelských profilů</span><span class="sxs-lookup"><span data-stu-id="87c01-121">Azure Active Directory: rectify/correct inaccurate or incomplete personal data and erase/delete personal data/user profiles</span></span>

<span data-ttu-id="87c01-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) je společnosti Microsoft založená na cloudu, víceklientské adresáře a identity management service.</span><span class="sxs-lookup"><span data-stu-id="87c01-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) is Microsoft’s cloud-based, multi-tenant directory and identity management service.</span></span>
<span data-ttu-id="87c01-123">Opravte, aktualizovat nebo odstranit zákazníků a profily uživatelů a pracovní informace o uživateli, které obsahují osobní údaje, jako je například název, název pracovní, adresa nebo telefonní číslo uživatele v vaše [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) prostředí pomocí hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="87c01-123">You can correct, update, or delete customer and employee user profiles and user work information that contain personal data, such as a user’s name, work title, address, or phone number, in your [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) environment by using hello [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="87c01-124">Přihlaste se pod účtem, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="87c01-124">You must sign in with an account that’s a global admin for hello directory.</span></span>

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a><span data-ttu-id="87c01-125">Jak opravit nebo aktualizovat uživatelský profil a pracovní informace v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="87c01-125">How do I correct or update user profile and work information in Azure Active Directory?</span></span>

1. <span data-ttu-id="87c01-126">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="87c01-126">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>

2. <span data-ttu-id="87c01-127">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="87c01-127">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

    ![média nebo image1.png](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="87c01-129">Na hello **uživatelů a skupin** vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="87c01-129">On hello **Users and groups** blade, select **Users**.</span></span>

    ![média nebo image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="87c01-131">Na hello **uživatelé a skupiny - Uživatelé** okně vyberte uživatele ze seznamu hello a poté v okně hello hello vybraného uživatele, vyberte možnost **profil** tooview hello informace o profilu uživatele vyžadující toobe opravě nebo aktualizované.</span><span class="sxs-lookup"><span data-stu-id="87c01-131">On hello **Users and groups - Users** blade, select a user from hello list, and then, on hello blade for hello selected user, select **Profile** tooview hello user profile information that needs toobe corrected or updated.</span></span>

    ![média nebo image3.png](media/manage-personal-data-azure/image005.png)

5. <span data-ttu-id="87c01-133">Opravte nebo aktualizovat informace o hello a pak v řádku nabídek hello vyberte **uložit.**</span><span class="sxs-lookup"><span data-stu-id="87c01-133">Correct or update hello information, and then, in hello command bar, select **Save.**</span></span>

6.  <span data-ttu-id="87c01-134">V okně hello hello vybraného uživatele, vyberte **fungovat informace** tooview pracovní informace o uživateli musí toobe opraveny nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="87c01-134">On hello blade for hello selected user, select **Work Info** tooview user work information that needs toobe corrected or updated.</span></span>

    ![média nebo image4.png](media/manage-personal-data-azure/image007.png)

7. <span data-ttu-id="87c01-136">Opravte nebo aktualizovat informace o práci hello uživatele a pak v řádku nabídek hello vyberte **uložit.**</span><span class="sxs-lookup"><span data-stu-id="87c01-136">Correct or update hello user work information, and then, in hello command bar, select **Save.**</span></span>

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a><span data-ttu-id="87c01-137">Jak lze odstranit profil uživatele v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="87c01-137">How do I delete a user profile in Azure Active Directory?</span></span>

1. <span data-ttu-id="87c01-138">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="87c01-138">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>

2. <span data-ttu-id="87c01-139">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="87c01-139">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

    ![](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="87c01-140">Na hello **uživatelů a skupin** vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="87c01-140">On hello **Users and groups** blade, select **Users**.</span></span>

    ![média nebo image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="87c01-142">Na hello **uživatelé a skupiny - Uživatelé** okně vyberte uživatele ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="87c01-142">On hello **Users and groups - Users** blade, select a user from hello list.</span></span>

    ![média nebo image3.png](media/manage-personal-data-azure/image007.png)

5. <span data-ttu-id="87c01-144">V okně hello hello vybraného uživatele, vyberte **přehled**a pak v řádku nabídek hello vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="87c01-144">On hello blade for hello selected user, select **Overview**, and then in hello command bar, select **Delete**.</span></span>

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a><span data-ttu-id="87c01-145">SQL Database: nápravě nebo opravte nesprávné nebo neúplné osobní údaje; smazání nebo odstranění osobních údajů; Exportovat osobní údaje</span><span class="sxs-lookup"><span data-stu-id="87c01-145">SQL Database: rectify/correct inaccurate or incomplete personal data; erase/delete personal data; export personal data</span></span> 

<span data-ttu-id="87c01-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) je databáze cloudu, která pomáhá vývojářům vytvářet a udržovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="87c01-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) is a cloud database that helps developers build and maintain applications.</span></span>

<span data-ttu-id="87c01-147">Osobní data můžete aktualizovat v [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) pomocí standardní dotazy SQL a můžete také odstranit.</span><span class="sxs-lookup"><span data-stu-id="87c01-147">Personal data can be updated in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) using standard SQL queries, and it can also be deleted.</span></span> <span data-ttu-id="87c01-148">Kromě toho je možné exportovat osobní data z databáze SQL pomocí různých metod, včetně hello Azure SQL Server import a export průvodce a v různých formátech, včetně souboru BACPAC souboru.</span><span class="sxs-lookup"><span data-stu-id="87c01-148">Additionally, personal data can be exported from SQL Database using a variety of methods, including hello Azure SQL Server import and export wizard, and in a variety of formats, including a BACPAC file.</span></span>

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a><span data-ttu-id="87c01-149">Jak opravit, aktualizovat a vymazat osobní data v databázi SQL?</span><span class="sxs-lookup"><span data-stu-id="87c01-149">How do I correct, update, or erase personal data in SQL Database?</span></span>

<span data-ttu-id="87c01-150">toolearn jak toocorrect nebo aktualizace osobní data v databázi SQL, navštivte hello [aktualizace (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [aktualizovat Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [aktualizace pomocí výrazu běžné tabulky](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), nebo [ Aktualizovat zápis textu](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="87c01-150">toolearn how toocorrect or update personal data in SQL Database, visit hello [Update (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [Update Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [Update with Common Table Expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), or [Update Write Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) documentation.</span></span>

<span data-ttu-id="87c01-151">toolearn jak toodelete osobní data v databázi SQL, navštivte hello [odstranit (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="87c01-151">toolearn how toodelete personal data in SQL Database, visit hello [Delete (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) documentation.</span></span>

#### <a name="how-do-i-export-personal-data-tooa-bacpac-file-in-sql-database"></a><span data-ttu-id="87c01-152">Jak exportovat soubor souboru BACPAC tooa osobní data v databázi SQL?</span><span class="sxs-lookup"><span data-stu-id="87c01-152">How do I export personal data tooa BACPAC file in SQL Database?</span></span>

<span data-ttu-id="87c01-153">Soubor souboru BACPAC zahrnuje hello databáze SQL data a metadata a je soubor zip s příponou souboru BACPAC.</span><span class="sxs-lookup"><span data-stu-id="87c01-153">A BACPAC file includes hello SQL Database data and metadata and is a zip file with a BACPAC extension.</span></span> <span data-ttu-id="87c01-154">To lze provést pomocí hello [portál Azure](https://portal.azure.com/), hello SQLPackage nástroj příkazového řádku, SQL Server Management Studio (SSMS) nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87c01-154">This can be done using hello [Azure portal](https://portal.azure.com/), hello SQLPackage command-line utility, SQL Server Management Studio (SSMS), or PowerShell.</span></span>

<span data-ttu-id="87c01-155">toolearn jak tooexport data tooa souboru BACPAC souboru, navštivte hello [exportujte soubor Azure SQL database tooa souboru BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-export) stránku, která obsahuje podrobné pokyny pro jednotlivé metody uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="87c01-155">toolearn how tooexport data tooa BACPAC file, visit hello [Export an Azure SQL database tooa BACPAC file](https://docs.microsoft.com/azure/sql-database/sql-database-export) page, which includes detailed instructions for each method listed above.</span></span>

<span data-ttu-id="87c01-156">Jak exportovat osobní data z databáze SQL s hello Microsoft SQL Server importovat a exportovat Průvodce?</span><span class="sxs-lookup"><span data-stu-id="87c01-156">How do I export personal data from SQL Database with hello SQL Server Import and Export Wizard?</span></span>

<span data-ttu-id="87c01-157">Tento průvodce vám pomůže zkopírovat data z zdroj tooa cíl.</span><span class="sxs-lookup"><span data-stu-id="87c01-157">This wizard helps you copy data from a source tooa destination.</span></span> <span data-ttu-id="87c01-158">Průvodce Úvod toohello, včetně jak tooget ji, informace o oprávnění a jak v hello nástroj, pomoci tooget navštívit hello [Import a Export dat s hello Import SQL serveru a Průvodce exportem](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) webové stránky.</span><span class="sxs-lookup"><span data-stu-id="87c01-158">For an introduction toohello wizard, including how tooget it, permissions information, and how tooget help with hello tool, visit hello [Import and Export Data with hello SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) web page.</span></span>

<span data-ttu-id="87c01-159">Přehled kroků průvodce hello najdete na adrese hello [kroky v hello Microsoft SQL Server importovat a exportovat průvodce](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) webové stránky.</span><span class="sxs-lookup"><span data-stu-id="87c01-159">For an overview of steps for hello wizard, visit hello [Steps in hello SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) web page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87c01-160">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="87c01-160">Next Steps:</span></span>

[<span data-ttu-id="87c01-161">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="87c01-161">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[<span data-ttu-id="87c01-162">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="87c01-162">Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

