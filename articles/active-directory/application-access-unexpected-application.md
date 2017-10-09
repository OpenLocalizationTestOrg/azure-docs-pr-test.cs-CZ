---
title: "aaaUnexpected aplikace v seznamu aplikací | Microsoft Docs"
description: "Jak toosee všechny aplikace ve vašem klientovi a pochopit, jak aplikace se objeví v seznamu všechny aplikace v rámci podnikové aplikace"
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
ms.openlocfilehash: 2f974046b655bc36a05f002c56511a8a988cd01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-application-in-my-applications-list"></a><span data-ttu-id="62f7b-103">Neočekávané aplikace v seznamu Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="62f7b-103">Unexpected application in my applications list</span></span>

<span data-ttu-id="62f7b-104">Tento článek vám pomůže toounderstand jak aplikace se objeví v vaše **všechny aplikace** v rámci **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="62f7b-104">This article help you toounderstand how applications appear in your **All Applications** list under **Enterprise Applications**.</span></span> 

## <a name="how-toosee-all-applications-in-your-tenant"></a><span data-ttu-id="62f7b-105">Jak toosee všechny aplikace ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="62f7b-105">How toosee all applications in your tenant</span></span>

<span data-ttu-id="62f7b-106">toosee všechny hello aplikací ve vašem klientovi, musíte toouse hello **filtru** řízení tooshow **všechny aplikace** pod hello **všechny aplikace** seznamu.</span><span class="sxs-lookup"><span data-stu-id="62f7b-106">toosee all hello applications in your tenant, you need toouse hello **Filter** control tooshow **All Applications** under hello **All Applications** list.</span></span> <span data-ttu-id="62f7b-107">toodo tento, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="62f7b-107">toodo this, follow hello steps below:</span></span>

1.  <span data-ttu-id="62f7b-108">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="62f7b-108">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="62f7b-109">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="62f7b-109">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="62f7b-110">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="62f7b-110">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="62f7b-111">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="62f7b-111">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="62f7b-112">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="62f7b-112">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="62f7b-113">Klikněte na tlačítko hello použití hello **filtru** řízení hello horní části hello **seznam všech aplikací**.</span><span class="sxs-lookup"><span data-stu-id="62f7b-113">click hello use hello **Filter** control at hello top of hello **All Applications List**.</span></span>

7.  <span data-ttu-id="62f7b-114">Na hello **filtru** okno, sada hello **zobrazit** možnost příliš**všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="62f7b-114">On hello **Filter** blade, set hello **Show** option too**All Applications.**</span></span>

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a><span data-ttu-id="62f7b-115">Proč konkrétní aplikace se zobrazuje v seznamu všechny aplikace?</span><span class="sxs-lookup"><span data-stu-id="62f7b-115">Why does a specific application appear in my all applications list?</span></span>

<span data-ttu-id="62f7b-116">Při filtrovat příliš**všechny aplikace**, hello **všechny aplikace** **seznamu** ukazuje každý objekt zabezpečení služby ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="62f7b-116">When filtered too**All Applications**, hello **All Applications** **List** shows every Service Principal object in your tenant.</span></span> <span data-ttu-id="62f7b-117">Hlavní objekty služeb můžete zobrazit v tomto seznamu různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="62f7b-117">Service Principal objects can appear in this list in a various ways:</span></span>

1.  <span data-ttu-id="62f7b-118">Když přidáte z Galerie aplikace hello jakékoli aplikace, včetně:</span><span class="sxs-lookup"><span data-stu-id="62f7b-118">When you add any application from hello application gallery, including:</span></span>

   1. <span data-ttu-id="62f7b-119">**Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62f7b-119">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

   2. <span data-ttu-id="62f7b-120">**Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které mají tooprovide zabezpečené jednotného přihlašování tooexternally.</span><span class="sxs-lookup"><span data-stu-id="62f7b-120">**Application Proxy Applications** – An application running in your on-premises environment that you want tooprovide secure single-sign on tooexternally.</span></span>

   3. <span data-ttu-id="62f7b-121">**Aplikace vyvinuté vlastní** – aplikace, které vaše organizace si přeje toodevelop na hello platformy vývoj aplikací Azure AD, ale který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="62f7b-121">**Custom-developed Applications** – An application that your organization wishes toodevelop on hello Azure AD Application Development Platform, but that may not exist yet.</span></span>

   4. <span data-ttu-id="62f7b-122">**Aplikace bez Galerie** – přineste si vlastní aplikace!</span><span class="sxs-lookup"><span data-stu-id="62f7b-122">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="62f7b-123">Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete toointegrate pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62f7b-123">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish toointegrate for single sign-on with Azure AD.</span></span>

2.  <span data-ttu-id="62f7b-124">Když registrujete nebo přihlášení k a 3<sup>VP</sup> aplikaci strany integrované s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62f7b-124">When signing up for, or signing in to, a 3<sup>rd</sup> party application integrated with Azure Active Directory.</span></span> <span data-ttu-id="62f7b-125">Příkladem toho je [Smartsheet](https://app.smartsheet.com/b/home) nebo [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span><span class="sxs-lookup"><span data-stu-id="62f7b-125">One example of this is [Smartsheet](https://app.smartsheet.com/b/home) or [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span></span>

3.  <span data-ttu-id="62f7b-126">Pokud registrujete nebo přidávání licencí tooa uživatele nebo skupiny tooa první strany aplikaci, jako například [Microsoft Office 365](http://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="62f7b-126">When signing up for, or adding a license tooa user or a group tooa first party application, like [Microsoft Office 365](http://products.office.com/).</span></span>

4.  <span data-ttu-id="62f7b-127">Když přidáte novou registraci aplikace tak, že vytvoříte vlastní vyvinuté aplikace, pomocí hello [aplikace registru](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span><span class="sxs-lookup"><span data-stu-id="62f7b-127">When you add a new application registration by creating a custom-developed application using hello [Application Registry](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span></span>

5.  <span data-ttu-id="62f7b-128">Když přidáte novou registraci aplikace tak, že vytvoříte vlastní vyvinuté aplikace, pomocí hello [portálu pro registraci aplikace V2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span><span class="sxs-lookup"><span data-stu-id="62f7b-128">When you add a new application registration by creating a custom-developed application using hello [V2.0 Application Registration Portal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span></span>

6.  <span data-ttu-id="62f7b-129">Když přidáváte aplikaci provádíte vývoj pomocí sady Visual Studio [metody ověřování ASP.net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) nebo [připojené služby](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span><span class="sxs-lookup"><span data-stu-id="62f7b-129">When you add an application you’re developing using Visual Studio’s [ASP.net Authentication Methods](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) or [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span></span>

7.  <span data-ttu-id="62f7b-130">Při vytváření objektu zabezpečení služby pomocí hello [modulu Azure AD PowerShell](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="62f7b-130">When you create a service principal object using hello [Azure AD PowerShell Module](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

8.  <span data-ttu-id="62f7b-131">Pokud jste [souhlas tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) za správce toouse data ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="62f7b-131">When you [consent tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) as an administrator toouse data in your tenant.</span></span>

9.  <span data-ttu-id="62f7b-132">Když [uživatel souhlasí tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="62f7b-132">When a [user consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data in your tenant.</span></span>

10. <span data-ttu-id="62f7b-133">Když povolíte určité služby, které ukládají data ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="62f7b-133">When you enable certain services that store data in your tenant.</span></span> <span data-ttu-id="62f7b-134">Příkladem toho je resetování hesla, která je modelovaná jako hlavní toostore služby heslo resetovat zásady bezpečně.</span><span class="sxs-lookup"><span data-stu-id="62f7b-134">One example of this is Password Reset, which is modeled as a service principal toostore your password reset policy securely.</span></span>

<span data-ttu-id="62f7b-135">Přečtěte si další informace o aplikací, jak jsou přidat tooyour directory tooget [jak a proč se aplikace přidávají tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span><span class="sxs-lookup"><span data-stu-id="62f7b-135">tooget more details on how apps are added tooyour directory, read [How and why applications are added tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span></span>

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a><span data-ttu-id="62f7b-136">Chci tooremove aplikace tooan přiřazení konkrétního uživatele nebo skupiny</span><span class="sxs-lookup"><span data-stu-id="62f7b-136">I want tooremove a specific user’s or group’s assignment tooan application</span></span>

<span data-ttu-id="62f7b-137">tooremove uživatele nebo skupinu přiřazení tooan aplikace, postupujte podle kroků hello uvedené v hello [odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) článku.</span><span class="sxs-lookup"><span data-stu-id="62f7b-137">tooremove a user or group assignment tooan application, follow hello steps listed in hello [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

## <a name="i-want-toodisable-all-access-tooan-application-for-every-user"></a><span data-ttu-id="62f7b-138">Chci toodisable všechny aplikace tooan přístupu pro každého uživatele</span><span class="sxs-lookup"><span data-stu-id="62f7b-138">I want toodisable all access tooan application for every user</span></span>

<span data-ttu-id="62f7b-139">toodisable všechny tooan přihlášení uživatele-aplikací, postupujte podle kroků hello uvedených v hello [zakázat uživatelská přihlášení pro podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) článku.</span><span class="sxs-lookup"><span data-stu-id="62f7b-139">toodisable all user sign-ins tooan application, follow hello steps listed in hello [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-toodelete-an-application-entirely"></a><span data-ttu-id="62f7b-140">Chci, aby toodelete aplikaci zcela</span><span class="sxs-lookup"><span data-stu-id="62f7b-140">I want toodelete an application entirely</span></span>

<span data-ttu-id="62f7b-141">příliš**odstranit aplikaci**, postupujte podle pokynů hello níže:</span><span class="sxs-lookup"><span data-stu-id="62f7b-141">too**delete an application**, follow hello instructions below:</span></span>

1.  <span data-ttu-id="62f7b-142">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="62f7b-142">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="62f7b-143">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="62f7b-143">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="62f7b-144">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="62f7b-144">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="62f7b-145">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="62f7b-145">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="62f7b-146">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="62f7b-146">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="62f7b-147">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="62f7b-147">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="62f7b-148">Vyberte aplikaci hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="62f7b-148">Select hello application you want toodelete.</span></span>

7.  <span data-ttu-id="62f7b-149">Po načtení hello aplikace, klikněte na **odstranit** ikonu z hello hlavní aplikace **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="62f7b-149">Once hello application loads, click **Delete** icon from hello top application’s **Overview** blade.</span></span>

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a><span data-ttu-id="62f7b-150">Chci toodisable všechny budoucí uživatelská souhlasu operations tooany aplikace</span><span class="sxs-lookup"><span data-stu-id="62f7b-150">I want toodisable all future user consent operations tooany application</span></span>

<span data-ttu-id="62f7b-151">Zakázání souhlas uživatele pro celý adresář zabránit koncovým uživatelům z souhlas tooany aplikace.</span><span class="sxs-lookup"><span data-stu-id="62f7b-151">Disabling user consent for your entire directory prevent end users from consenting tooany application.</span></span> <span data-ttu-id="62f7b-152">Správci mohou stále souhlas na behalves uživatele.</span><span class="sxs-lookup"><span data-stu-id="62f7b-152">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="62f7b-153">souhlas toolearn Další informace o aplikaci a proč může nebo nemusí nepřejete toodo, články [Principy uživatelů a správce souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="62f7b-153">toolearn more about application consent, and why you may or may not wish toodo this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="62f7b-154">příliš**zakažte všechny operace souhlasu budoucí uživatele v adresáři celý**, postupujte podle pokynů hello níže:</span><span class="sxs-lookup"><span data-stu-id="62f7b-154">too**disable all future user consent operations in your entire directory**, follow hello instructions below:</span></span>

1.  <span data-ttu-id="62f7b-155">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="62f7b-155">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="62f7b-156">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="62f7b-156">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="62f7b-157">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="62f7b-157">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="62f7b-158">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="62f7b-158">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="62f7b-159">Klikněte na tlačítko **uživatelská nastavení**.</span><span class="sxs-lookup"><span data-stu-id="62f7b-159">click **User settings**.</span></span>

6.  <span data-ttu-id="62f7b-160">Zakažte všechny operace souhlasu budoucí uživatele tak, že nastavení hello **uživatelé můžou aplikace tooaccess svá data** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="62f7b-160">Disable all future user consent operations by setting hello **Users can allow apps tooaccess their data** toggle too**No** and click hello **Save** button.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62f7b-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62f7b-161">Next steps</span></span>
[<span data-ttu-id="62f7b-162">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62f7b-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
