---
title: "Potíže s přihlášením k webu přístup panely | Microsoft Docs"
description: "Pokyny k odstraňování problémů, které můžete narazit při pokusu o přihlášení k použití na přístupovém panelu"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviwer: japere
ms.openlocfilehash: 28d91237adf745e591b02322de7881c8122827ac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-signing-in-to-the-access-panel-website"></a><span data-ttu-id="9c53f-103">Potíže s přihlášením k webu přístup k panelu</span><span class="sxs-lookup"><span data-stu-id="9c53f-103">Problem signing in to the access panel website</span></span>

<span data-ttu-id="9c53f-104">Přístupový Panel je webový portál, který uživatel, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) umožňuje zobrazovat a spouštět cloudové aplikace, které je správce Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="9c53f-104">The Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) to view and launch cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="9c53f-105">Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="9c53f-105">A user who has Azure AD editions can also use self-service group and app management capabilities through the Access Panel.</span></span> <span data-ttu-id="9c53f-106">Přístupový Panel je nezávislý na portálu Azure a nevyžaduje, aby uživatelé měli předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9c53f-106">The Access Panel is separate from the Azure portal and does not require users to have an Azure subscription.</span></span>

<span data-ttu-id="9c53f-107">Uživatele můžete přihlásit k přístupovému panelu případě, že mají pracovní nebo školní účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c53f-107">Users can sign in to the Access Panel if they have a work or school account in Azure AD.</span></span>

-   <span data-ttu-id="9c53f-108">Uživatelé se můžou ověřovat službou Azure AD přímo.</span><span class="sxs-lookup"><span data-stu-id="9c53f-108">Users can be authenticated by Azure AD directly.</span></span>

-   <span data-ttu-id="9c53f-109">Uživatelé se můžou ověřovat pomocí služby Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="9c53f-109">Users can be authenticated by using Active Directory Federation Services (AD FS).</span></span>

-   <span data-ttu-id="9c53f-110">Uživatelé se můžou ověřovat pomocí služby Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9c53f-110">Users can be authenticated by Windows Server Active Directory.</span></span>

<span data-ttu-id="9c53f-111">Pokud je uživatel má předplatné Azure nebo Office 365 a používá portál Azure nebo aplikace Office 365, budou moci bez problémů používat na přístupovém panelu bez nutnosti se znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="9c53f-111">If a user has a subscription for Azure or Office 365 and has been using the Azure portal or an Office 365 application, they'll be able to use the Access Panel seamlessly without needing to sign in again.</span></span> <span data-ttu-id="9c53f-112">Uživatelé, kteří nejsou ověřené vyzváni k přihlášení pomocí uživatelského jména a hesla pro svůj účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c53f-112">Users who are not authenticated be prompted to sign in by using the username and password for their account in Azure AD.</span></span> <span data-ttu-id="9c53f-113">Pokud organizace nemá nakonfigurovali federaci, zadejte uživatelské jméno je dostačující.</span><span class="sxs-lookup"><span data-stu-id="9c53f-113">If the organization has configured federation, typing the username is sufficient.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="9c53f-114">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="9c53f-114">General issues to check first</span></span> 

-   <span data-ttu-id="9c53f-115">Ujistěte se, že uživatel je přihlášení k **opravte adresu URL**: <https://myapps.microsoft.com></span><span class="sxs-lookup"><span data-stu-id="9c53f-115">Make sure the user is signing in to the **correct URL**: <https://myapps.microsoft.com></span></span>

-   <span data-ttu-id="9c53f-116">Zajistěte, aby prohlížeče uživatele přidala adresu URL na jeho **Důvěryhodné servery**</span><span class="sxs-lookup"><span data-stu-id="9c53f-116">Make sure the user’s browser has added the URL to its **trusted sites**</span></span>

-   <span data-ttu-id="9c53f-117">Zkontrolujte, zda je účet uživatele **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9c53f-117">Make sure the user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="9c53f-118">Zkontrolujte, zda je účet uživatele **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-118">Make sure the user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="9c53f-119">Zkontrolujte, že uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-119">Make sure the user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="9c53f-120">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c53f-120">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="9c53f-121">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c53f-121">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="9c53f-122">Ujistěte se, že uživatele **ověřování kontaktní údaje** je aktuální povolit Multi-Factor Authentication nebo podmíněného přístupu zásad, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="9c53f-122">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span>

-   <span data-ttu-id="9c53f-123">Zkontrolujte, zda také zkuste vymazání souborů cookie v prohlížeči a zkusit se přihlásit znovu.</span><span class="sxs-lookup"><span data-stu-id="9c53f-123">Make sure to also try clearing your browser’s cookies and trying to sign in again.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="9c53f-124">Splňuje požadavky na prohlížeč pro přístup k panelu</span><span class="sxs-lookup"><span data-stu-id="9c53f-124">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="9c53f-125">Přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="9c53f-125">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="9c53f-126">Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c53f-126">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="9c53f-127">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="9c53f-127">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="9c53f-128">Pro jednotné přihlašování založené na heslech může být prohlížeče koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="9c53f-128">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="9c53f-129">Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9c53f-129">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="9c53f-130">Hraniční Windows 10 Anniversary Edition nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9c53f-130">Edge on Windows 10 Anniversary Edition or later</span></span> 

-   <span data-ttu-id="9c53f-131">Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="9c53f-131">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="9c53f-132">Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9c53f-132">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>


## <a name="problems-with-the-users-account"></a><span data-ttu-id="9c53f-133">Problémy s uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="9c53f-133">Problems with the user’s account</span></span>

<span data-ttu-id="9c53f-134">Přístup k přístupovému panelu můžete blokovaný z důvodu problému s účtem uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c53f-134">Access to the Access Panel can be blocked due to a problem with the user’s account.</span></span> <span data-ttu-id="9c53f-135">Tady jsou některé způsoby řešení problémů a řešení problémů s uživateli a jejich nastavení účtu:</span><span class="sxs-lookup"><span data-stu-id="9c53f-135">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="9c53f-136">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c53f-136">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="9c53f-137">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-137">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="9c53f-138">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-138">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="9c53f-139">Povolení samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="9c53f-139">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="9c53f-140">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-140">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="9c53f-141">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-141">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="9c53f-142">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="9c53f-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="9c53f-143">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="9c53f-143">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="9c53f-144">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-144">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="9c53f-145">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c53f-145">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="9c53f-146">Pokud chcete zkontrolovat, zda účet uživatele je přítomen, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-146">To check if a user’s account is present, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-147">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-147">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-148">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-148">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-149">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-149">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-150">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-150">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-151">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-151">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-152">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-152">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-153">Zkontrolujte vlastnosti objektu uživatele a ujistěte se, že zobrazují se podle očekávání a nebyla nalezena žádná data.</span><span class="sxs-lookup"><span data-stu-id="9c53f-153">Check the properties of the user object to be sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="9c53f-154">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-154">Check a user’s account status</span></span>

<span data-ttu-id="9c53f-155">Pokud chcete zkontrolovat stav účtu uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-155">To check a user’s account status, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-156">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-156">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-157">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-157">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-158">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-158">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-159">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-159">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-160">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-160">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-161">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-161">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-162">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-162">click **Profile**.</span></span>

8.  <span data-ttu-id="9c53f-163">V části **nastavení** Ujistěte se, že **bloku přihlásit** je nastaven na **ne**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-163">Under **Settings** ensure that **Block sign in** is set to **No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="9c53f-164">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-164">Reset a user’s password</span></span>

<span data-ttu-id="9c53f-165">Pokud chcete resetovat heslo uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-165">To reset a user’s password, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-166">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-166">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-167">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-167">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-168">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-168">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-169">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-169">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-170">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-170">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-171">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-171">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-172">klikněte **resetovat heslo** tlačítka v horní části okna uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c53f-172">click the **Reset password** button at the top of the user blade.</span></span>

8.  <span data-ttu-id="9c53f-173">klikněte **resetovat heslo** na tlačítko **resetovat heslo** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="9c53f-173">click the **Reset password** button on the **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="9c53f-174">Kopírování **dočasné heslo** nebo **zadat nové heslo** pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c53f-174">Copy the **temporary password** or **enter a new password** for the user.</span></span>

10. <span data-ttu-id="9c53f-175">Komunikaci tohoto nového hesla pro uživatele, budou muset změnit toto heslo při příštím přihlášení v do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9c53f-175">Communicate this new password to the user, they be required to change this password during their next sign in to Azure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="9c53f-176">Povolit samoobslužné resetování hesla</span><span class="sxs-lookup"><span data-stu-id="9c53f-176">Enable self-service password reset</span></span>

<span data-ttu-id="9c53f-177">Pokud chcete povolit samoobslužné resetování hesla, postupujte podle následujících kroků nasazení:</span><span class="sxs-lookup"><span data-stu-id="9c53f-177">To enable self-service password reset, follow the deployment steps below:</span></span>

-   [<span data-ttu-id="9c53f-178">Umožnění resetování hesel služby Azure Active Directory uživatelům</span><span class="sxs-lookup"><span data-stu-id="9c53f-178">Enable users to reset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="9c53f-179">Povolit uživatelům resetovat nebo změnit své heslo místní služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c53f-179">Enable users to reset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="9c53f-180">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-180">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="9c53f-181">Pokud chcete zkontrolovat stav uživatele služby Multi-Factor authentication, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-181">To check a user’s multi-factor authentication status, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-182">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-182">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-183">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-183">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-184">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-184">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-185">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-185">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-186">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-186">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-187">klikněte **Multi-Factor Authentication** tlačítka v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="9c53f-187">click the **Multi-Factor Authentication** button at the top of the blade.</span></span>

7.  <span data-ttu-id="9c53f-188">Jednou **portálu pro správu služby Multi-Factor Authentication** zatížení, ujistěte se, jsou na **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="9c53f-188">Once the **Multi-Factor Authentication Administration Portal** loads, ensure you are on the **Users** tab.</span></span>

8.  <span data-ttu-id="9c53f-189">Vyhledejte uživatele pomocí vyhledávání, filtrování a řazení v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c53f-189">Find the user in the list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="9c53f-190">Vyberte uživatele ze seznamu uživatelů a **povolit**, **zakázat**, nebo **vynutit** služby Multi-Factor authentication podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9c53f-190">Select the user from the list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

   >[!NOTE]
   ><span data-ttu-id="9c53f-191">Pokud je uživatel v **vynucené** stavu, může je nastaven **zakázané** dočasně a dát jim zpět do svého účtu.</span><span class="sxs-lookup"><span data-stu-id="9c53f-191">If a user is in an **Enforced** state, you may set them to **Disabled** temporarily to let them back into their account.</span></span> <span data-ttu-id="9c53f-192">Jakmile se zpět, můžete změnit svůj stav na **povoleno** znovu, aby je znovu zaregistrovat své kontaktní informace při příštím přihlášení v vyžadují.</span><span class="sxs-lookup"><span data-stu-id="9c53f-192">Once they are back in, you can then change their state to **Enabled** again to require them to re-register their contact information during their next sign in.</span></span> <span data-ttu-id="9c53f-193">Alternativně můžete podle kroků v [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info) ověření nebo pro ně nastavit tato data.</span><span class="sxs-lookup"><span data-stu-id="9c53f-193">Alternatively, you can follow the steps in the [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) to verify or set this data for them.</span></span>
   >
   >

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="9c53f-194">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-194">Check a user’s authentication contact info</span></span>

<span data-ttu-id="9c53f-195">Pokud chcete zkontrolovat uživatele ověřování kontaktní údaje pro službu Multi-Factor authentication, podmíněného přístupu, ochrany identit a resetování hesla, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-195">To check a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-196">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-196">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-197">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-197">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-198">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-198">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-199">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-199">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-200">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-200">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-201">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-201">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-202">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-202">click **Profile**.</span></span>

8.  <span data-ttu-id="9c53f-203">Přejděte dolů k položce **ověřování kontaktní údaje**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-203">Scroll down to **Authentication contact info**.</span></span>

9.  <span data-ttu-id="9c53f-204">**Zkontrolujte** data zaregistrovaný pro uživatele a aktualizovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9c53f-204">**Review** the data registered for the user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="9c53f-205">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="9c53f-205">Check a user’s group memberships</span></span>

<span data-ttu-id="9c53f-206">Pokud chcete zkontrolovat členství uživatele ve skupinách, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-206">To check a user’s group memberships, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-207">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-207">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-208">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-208">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-209">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-209">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-210">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-210">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-211">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-211">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-212">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-212">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-213">Klikněte na tlačítko **skupiny** zobrazíte, které skupiny uživatel je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="9c53f-213">click **Groups** to see which groups the user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="9c53f-214">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="9c53f-214">Check a user’s assigned licenses</span></span>

<span data-ttu-id="9c53f-215">Pokud chcete zkontrolovat uživatele přiřazené licence, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9c53f-215">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-216">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-216">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-217">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-217">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-218">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-218">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-219">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-219">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-220">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-220">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-221">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-221">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-222">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9c53f-222">click **Licenses** to see which licenses the user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="9c53f-223">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="9c53f-223">Assign a user a license</span></span> 

<span data-ttu-id="9c53f-224">Chcete-li přiřadit licenci pro uživatele, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9c53f-224">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="9c53f-225">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="9c53f-225">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9c53f-226">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="9c53f-226">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9c53f-227">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="9c53f-227">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9c53f-228">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="9c53f-228">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="9c53f-229">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c53f-229">click **All users**.</span></span>

6.  <span data-ttu-id="9c53f-230">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="9c53f-230">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="9c53f-231">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="9c53f-231">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="9c53f-232">klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c53f-232">click the **Assign** button.</span></span>

9.  <span data-ttu-id="9c53f-233">Vyberte **jeden nebo více produktů** ze seznamu dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="9c53f-233">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="9c53f-234">**Volitelné** klikněte na tlačítko **přiřazení možností** položky granularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="9c53f-234">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="9c53f-235">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="9c53f-235">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="9c53f-236">Klikněte **přiřadit** tlačítko tyto licence přiřadit tomuto uživateli.</span><span class="sxs-lookup"><span data-stu-id="9c53f-236">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a><span data-ttu-id="9c53f-237">Pokud tyto kroky řešení potíží problém nevyřeší</span><span class="sxs-lookup"><span data-stu-id="9c53f-237">If these troubleshooting steps do not resolve the issue</span></span>

<span data-ttu-id="9c53f-238">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="9c53f-238">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="9c53f-239">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="9c53f-239">Correlation error ID</span></span>

-   <span data-ttu-id="9c53f-240">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="9c53f-240">UPN (user email address)</span></span>

-   <span data-ttu-id="9c53f-241">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="9c53f-241">Tenant ID</span></span>

-   <span data-ttu-id="9c53f-242">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="9c53f-242">Browser type</span></span>

-   <span data-ttu-id="9c53f-243">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="9c53f-243">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="9c53f-244">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="9c53f-244">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c53f-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c53f-245">Next steps</span></span>
[<span data-ttu-id="9c53f-246">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="9c53f-246">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
