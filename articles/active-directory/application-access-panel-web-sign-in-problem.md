---
title: "aaaProblem přihlášení toohello přístup k panelu webu | Microsoft Docs"
description: "Pokyny tootroubleshoot problémy, ke kterým může dojít při pokusu o toosign v toouse hello přístupového panelu"
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
ms.openlocfilehash: 1037f7c5fbaa9425760ad5739b383c716d5fc3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-signing-in-toohello-access-panel-website"></a><span data-ttu-id="5d76b-103">Potíže s přihlášením toohello přístup k panelu webu</span><span class="sxs-lookup"><span data-stu-id="5d76b-103">Problem signing in toohello access panel website</span></span>

<span data-ttu-id="5d76b-104">Hello přístupový Panel je, že je webový portál, který umožňuje uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spouštět cloudové aplikace tento správce hello Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="5d76b-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="5d76b-105">Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="5d76b-106">Hello přístupový Panel je oddělená od hello portál Azure a nevyžaduje, aby uživatelé toohave předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5d76b-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="5d76b-107">Uživatelé můžou přihlásit toohello přístupového panelu v případě, že mají pracovní nebo školní účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5d76b-107">Users can sign in toohello Access Panel if they have a work or school account in Azure AD.</span></span>

-   <span data-ttu-id="5d76b-108">Uživatelé se můžou ověřovat službou Azure AD přímo.</span><span class="sxs-lookup"><span data-stu-id="5d76b-108">Users can be authenticated by Azure AD directly.</span></span>

-   <span data-ttu-id="5d76b-109">Uživatelé se můžou ověřovat pomocí služby Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="5d76b-109">Users can be authenticated by using Active Directory Federation Services (AD FS).</span></span>

-   <span data-ttu-id="5d76b-110">Uživatelé se můžou ověřovat pomocí služby Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d76b-110">Users can be authenticated by Windows Server Active Directory.</span></span>

<span data-ttu-id="5d76b-111">Pokud je uživatel má předplatné Azure nebo Office 365 a používá hello portál Azure nebo Office 365 aplikaci, bude se mít toouse hello přístupový Panel bezproblémově bez nutnosti toosign v akci.</span><span class="sxs-lookup"><span data-stu-id="5d76b-111">If a user has a subscription for Azure or Office 365 and has been using hello Azure portal or an Office 365 application, they'll be able toouse hello Access Panel seamlessly without needing toosign in again.</span></span> <span data-ttu-id="5d76b-112">Uživatelé, kteří nejsou ověřené být výzvami toosign v pomocí hello uživatelské jméno a heslo pro svůj účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5d76b-112">Users who are not authenticated be prompted toosign in by using hello username and password for their account in Azure AD.</span></span> <span data-ttu-id="5d76b-113">Pokud organizace hello nakonfiguroval federace, zadejte uživatelské jméno hello je dostačující.</span><span class="sxs-lookup"><span data-stu-id="5d76b-113">If hello organization has configured federation, typing hello username is sufficient.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="5d76b-114">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="5d76b-114">General issues toocheck first</span></span> 

-   <span data-ttu-id="5d76b-115">Zajistěte, aby uživatel hello přihlašuje toohello **opravte adresu URL**: <https://myapps.microsoft.com></span><span class="sxs-lookup"><span data-stu-id="5d76b-115">Make sure hello user is signing in toohello **correct URL**: <https://myapps.microsoft.com></span></span>

-   <span data-ttu-id="5d76b-116">Zajistěte, aby prohlížeče hello uživatele přidala hello URL tooits **Důvěryhodné servery**</span><span class="sxs-lookup"><span data-stu-id="5d76b-116">Make sure hello user’s browser has added hello URL tooits **trusted sites**</span></span>

-   <span data-ttu-id="5d76b-117">Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5d76b-117">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="5d76b-118">Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-118">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="5d76b-119">Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-119">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="5d76b-120">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5d76b-120">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="5d76b-121">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5d76b-121">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="5d76b-122">Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené.</span><span class="sxs-lookup"><span data-stu-id="5d76b-122">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="5d76b-123">Zkontrolujte že tooalso zkuste vymazání souborů cookie v prohlížeči a toosign v zkusit to znovu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-123">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="5d76b-124">Splňuje požadavky na prohlížeč pro hello přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="5d76b-124">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="5d76b-125">Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="5d76b-125">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="5d76b-126">toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5d76b-126">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="5d76b-127">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="5d76b-127">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="5d76b-128">Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="5d76b-128">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="5d76b-129">Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5d76b-129">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="5d76b-130">Hraniční Windows 10 Anniversary Edition nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5d76b-130">Edge on Windows 10 Anniversary Edition or later</span></span> 

-   <span data-ttu-id="5d76b-131">Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="5d76b-131">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="5d76b-132">Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5d76b-132">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>


## <a name="problems-with-hello-users-account"></a><span data-ttu-id="5d76b-133">Problémy s hello uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="5d76b-133">Problems with hello user’s account</span></span>

<span data-ttu-id="5d76b-134">Přístup toohello přístupového panelu můžete blokovat z důvodu tooa problém s hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="5d76b-134">Access toohello Access Panel can be blocked due tooa problem with hello user’s account.</span></span> <span data-ttu-id="5d76b-135">Tady jsou některé způsoby řešení problémů a řešení problémů s uživateli a jejich nastavení účtu:</span><span class="sxs-lookup"><span data-stu-id="5d76b-135">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="5d76b-136">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d76b-136">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="5d76b-137">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-137">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="5d76b-138">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-138">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="5d76b-139">Povolení samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="5d76b-139">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="5d76b-140">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-140">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="5d76b-141">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-141">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="5d76b-142">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="5d76b-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="5d76b-143">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="5d76b-143">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="5d76b-144">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-144">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="5d76b-145">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d76b-145">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="5d76b-146">toocheck, pokud se nachází uživatelský účet, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5d76b-146">toocheck if a user’s account is present, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-147">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-147">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-148">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-148">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-149">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-149">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-150">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-150">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-151">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-151">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-152">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-152">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-153">Zkontrolujte vlastnosti hello hello uživatele objektu toobe se, že zobrazují se podle očekávání a nebyla nalezena žádná data.</span><span class="sxs-lookup"><span data-stu-id="5d76b-153">Check hello properties of hello user object toobe sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="5d76b-154">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-154">Check a user’s account status</span></span>

<span data-ttu-id="5d76b-155">toocheck uživatele pro účet stavu, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5d76b-155">toocheck a user’s account status, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-156">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-156">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-157">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-157">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-158">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-158">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-159">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-159">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-160">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-160">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-161">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-161">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-162">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-162">click **Profile**.</span></span>

8.  <span data-ttu-id="5d76b-163">V části **nastavení** Ujistěte se, že **bloku přihlásit** je nastaven příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-163">Under **Settings** ensure that **Block sign in** is set too**No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="5d76b-164">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-164">Reset a user’s password</span></span>

<span data-ttu-id="5d76b-165">tooreset heslo uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5d76b-165">tooreset a user’s password, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-166">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-166">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-167">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-167">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-168">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-168">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-169">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-169">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-170">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-170">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-171">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-171">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-172">Klikněte na tlačítko hello **resetovat heslo** tlačítko hello horní části okna hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5d76b-172">click hello **Reset password** button at hello top of hello user blade.</span></span>

8.  <span data-ttu-id="5d76b-173">Klikněte na tlačítko hello **resetovat heslo** na hello tlačítko **resetovat heslo** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="5d76b-173">click hello **Reset password** button on hello **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="5d76b-174">Kopírování hello **dočasné heslo** nebo **zadat nové heslo** pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-174">Copy hello **temporary password** or **enter a new password** for hello user.</span></span>

10. <span data-ttu-id="5d76b-175">Komunikaci tohoto nového uživatele toohello heslo, ale měly požadované toochange toto heslo při příštím přihlášení tooAzure služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d76b-175">Communicate this new password toohello user, they be required toochange this password during their next sign in tooAzure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="5d76b-176">Povolit samoobslužné resetování hesla</span><span class="sxs-lookup"><span data-stu-id="5d76b-176">Enable self-service password reset</span></span>

<span data-ttu-id="5d76b-177">tooenable samoobslužné služby heslo resetovat, postupujte podle následujících pokynů hello nasazení:</span><span class="sxs-lookup"><span data-stu-id="5d76b-177">tooenable self-service password reset, follow hello deployment steps below:</span></span>

-   [<span data-ttu-id="5d76b-178">Povolit uživatelům tooreset hesel služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d76b-178">Enable users tooreset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="5d76b-179">Povolit uživatelům tooreset nebo změnit své heslo místní služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d76b-179">Enable users tooreset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="5d76b-180">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-180">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="5d76b-181">toocheck uživatele je vícefaktorového ověřování stavu, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5d76b-181">toocheck a user’s multi-factor authentication status, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-182">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-182">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-183">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-183">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-184">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-184">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-185">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-185">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-186">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-186">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-187">Klikněte na tlačítko hello **Multi-Factor Authentication** tlačítko hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-187">click hello **Multi-Factor Authentication** button at hello top of hello blade.</span></span>

7.  <span data-ttu-id="5d76b-188">Jednou hello **portálu pro správu služby Multi-Factor Authentication** zatížení, ujistěte se, jsou na hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="5d76b-188">Once hello **Multi-Factor Authentication Administration Portal** loads, ensure you are on hello **Users** tab.</span></span>

8.  <span data-ttu-id="5d76b-189">Najít hello uživatele v seznamu hello uživatelů ve vyhledávání, filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="5d76b-189">Find hello user in hello list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="5d76b-190">Vyberte hello uživatele z hello seznam uživatelů a **povolit**, **zakázat**, nebo **vynutit** služby Multi-Factor authentication podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5d76b-190">Select hello user from hello list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

   >[!NOTE]
   ><span data-ttu-id="5d76b-191">Pokud je uživatel v **vynucené** stavu, je pravděpodobně příliš nastavit**zakázané** dočasně toolet je zpět do svého účtu.</span><span class="sxs-lookup"><span data-stu-id="5d76b-191">If a user is in an **Enforced** state, you may set them too**Disabled** temporarily toolet them back into their account.</span></span> <span data-ttu-id="5d76b-192">Jakmile se zpět, můžete změnit jejich stavu příliš**povoleno** znovu toorequire je toore registrace své kontaktní informace při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5d76b-192">Once they are back in, you can then change their state too**Enabled** again toorequire them toore-register their contact information during their next sign in.</span></span> <span data-ttu-id="5d76b-193">Alternativně můžete provést kroky hello v hello [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info) tooverify nebo pro ně nastavit tato data.</span><span class="sxs-lookup"><span data-stu-id="5d76b-193">Alternatively, you can follow hello steps in hello [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) tooverify or set this data for them.</span></span>
   >
   >

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="5d76b-194">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-194">Check a user’s authentication contact info</span></span>

<span data-ttu-id="5d76b-195">toocheck ověřování uživatele kontaktní informace pro vícefaktorové ověřování, podmíněného přístupu, ochrany identit a resetování hesla, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="5d76b-195">toocheck a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-196">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-196">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-197">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-197">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-198">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-198">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-199">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-199">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-200">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-200">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-201">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-201">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-202">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-202">click **Profile**.</span></span>

8.  <span data-ttu-id="5d76b-203">Posuňte se dolů příliš**ověřování kontaktní údaje**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-203">Scroll down too**Authentication contact info**.</span></span>

9.  <span data-ttu-id="5d76b-204">**Zkontrolujte** hello dat registrované pro uživatele hello a aktualizace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5d76b-204">**Review** hello data registered for hello user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="5d76b-205">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="5d76b-205">Check a user’s group memberships</span></span>

<span data-ttu-id="5d76b-206">toocheck uživatele na členství ve skupinách, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5d76b-206">toocheck a user’s group memberships, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-207">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-207">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-208">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-208">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-209">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-209">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-210">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-210">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-211">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-211">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-212">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-212">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-213">Klikněte na tlačítko **skupiny** toosee, které skupiny hello uživatel je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="5d76b-213">click **Groups** toosee which groups hello user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="5d76b-214">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="5d76b-214">Check a user’s assigned licenses</span></span>

<span data-ttu-id="5d76b-215">toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5d76b-215">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-216">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-216">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-217">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-217">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-218">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-218">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-219">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-219">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-220">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-220">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-221">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-221">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-222">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5d76b-222">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="5d76b-223">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="5d76b-223">Assign a user a license</span></span> 

<span data-ttu-id="5d76b-224">tooassign licence tooa uživatele, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="5d76b-224">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="5d76b-225">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5d76b-225">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d76b-226">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-226">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d76b-227">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5d76b-227">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d76b-228">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5d76b-228">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5d76b-229">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5d76b-229">click **All users**.</span></span>

6.  <span data-ttu-id="5d76b-230">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5d76b-230">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5d76b-231">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5d76b-231">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="5d76b-232">Klikněte na tlačítko hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5d76b-232">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="5d76b-233">Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="5d76b-233">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="5d76b-234">**Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="5d76b-234">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="5d76b-235">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="5d76b-235">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="5d76b-236">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign tyto licence toothis uživatele.</span><span class="sxs-lookup"><span data-stu-id="5d76b-236">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="5d76b-237">Pokud tyto kroky řešení potíží Nepokoušejte se vyřešit problém hello</span><span class="sxs-lookup"><span data-stu-id="5d76b-237">If these troubleshooting steps do not resolve hello issue</span></span>

<span data-ttu-id="5d76b-238">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="5d76b-238">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="5d76b-239">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="5d76b-239">Correlation error ID</span></span>

-   <span data-ttu-id="5d76b-240">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="5d76b-240">UPN (user email address)</span></span>

-   <span data-ttu-id="5d76b-241">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="5d76b-241">Tenant ID</span></span>

-   <span data-ttu-id="5d76b-242">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="5d76b-242">Browser type</span></span>

-   <span data-ttu-id="5d76b-243">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="5d76b-243">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="5d76b-244">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="5d76b-244">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d76b-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d76b-245">Next steps</span></span>
[<span data-ttu-id="5d76b-246">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="5d76b-246">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
