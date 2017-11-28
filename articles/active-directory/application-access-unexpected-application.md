---
title: "Neočekávané aplikace v seznamu aplikací | Microsoft Docs"
description: "Jak zobrazit všechny aplikace ve vašem klientovi a pochopit, jak aplikace se objeví v seznamu všechny aplikace v rámci podnikové aplikace"
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
ms.openlocfilehash: 0f8136cb066fa6e3e4a8dd06e34b9f866e3916f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-application-in-my-applications-list"></a><span data-ttu-id="319e3-103">Neočekávané aplikace v seznamu Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="319e3-103">Unexpected application in my applications list</span></span>

<span data-ttu-id="319e3-104">Tento článek vám pomůže lépe porozumět jak aplikace se objeví v vaše **všechny aplikace** v rámci **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="319e3-104">This article help you to understand how applications appear in your **All Applications** list under **Enterprise Applications**.</span></span> 

## <a name="how-to-see-all-applications-in-your-tenant"></a><span data-ttu-id="319e3-105">Jak zjistit, všechny aplikace ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="319e3-105">How to see all applications in your tenant</span></span>

<span data-ttu-id="319e3-106">Pokud chcete zobrazit všechny aplikace ve vašem klientovi, budete muset použít **filtru** řízení zobrazíte **všechny aplikace** v části **všechny aplikace** seznamu.</span><span class="sxs-lookup"><span data-stu-id="319e3-106">To see all the applications in your tenant, you need to use the **Filter** control to show **All Applications** under the **All Applications** list.</span></span> <span data-ttu-id="319e3-107">Chcete-li to provést, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="319e3-107">To do this, follow the steps below:</span></span>

1.  <span data-ttu-id="319e3-108">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="319e3-108">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="319e3-109">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="319e3-109">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="319e3-110">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="319e3-110">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="319e3-111">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="319e3-111">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="319e3-112">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="319e3-112">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="319e3-113">Klikněte na tlačítko použít **filtru** ovládací prvek v horní části **seznam všech aplikací**.</span><span class="sxs-lookup"><span data-stu-id="319e3-113">click the use the **Filter** control at the top of the **All Applications List**.</span></span>

7.  <span data-ttu-id="319e3-114">Na **filtru** okně nastavit **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="319e3-114">On the **Filter** blade, set the **Show** option to **All Applications.**</span></span>

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a><span data-ttu-id="319e3-115">Proč konkrétní aplikace se zobrazuje v seznamu všechny aplikace?</span><span class="sxs-lookup"><span data-stu-id="319e3-115">Why does a specific application appear in my all applications list?</span></span>

<span data-ttu-id="319e3-116">Když filtruje tak, aby **všechny aplikace**, **všechny aplikace** **seznamu** ukazuje každý objekt zabezpečení služby ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="319e3-116">When filtered to **All Applications**, the **All Applications** **List** shows every Service Principal object in your tenant.</span></span> <span data-ttu-id="319e3-117">Hlavní objekty služeb můžete zobrazit v tomto seznamu různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="319e3-117">Service Principal objects can appear in this list in a various ways:</span></span>

1.  <span data-ttu-id="319e3-118">Když přidáte všechny aplikace v galerii aplikací, včetně:</span><span class="sxs-lookup"><span data-stu-id="319e3-118">When you add any application from the application gallery, including:</span></span>

   1. <span data-ttu-id="319e3-119">**Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="319e3-119">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

   2. <span data-ttu-id="319e3-120">**Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které byste chtěli poskytnout zabezpečený jednotné přihlašování k externě.</span><span class="sxs-lookup"><span data-stu-id="319e3-120">**Application Proxy Applications** – An application running in your on-premises environment that you want to provide secure single-sign on to externally.</span></span>

   3. <span data-ttu-id="319e3-121">**Aplikace vyvinuté vlastní** – aplikaci, kterou vaše organizace chce vývoji na platformu vývoj aplikací Azure AD, ale který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="319e3-121">**Custom-developed Applications** – An application that your organization wishes to develop on the Azure AD Application Development Platform, but that may not exist yet.</span></span>

   4. <span data-ttu-id="319e3-122">**Aplikace bez Galerie** – přineste si vlastní aplikace!</span><span class="sxs-lookup"><span data-stu-id="319e3-122">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="319e3-123">Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete integrovat pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="319e3-123">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish to integrate for single sign-on with Azure AD.</span></span>

2.  <span data-ttu-id="319e3-124">Když registrujete nebo přihlášení k a 3<sup>VP</sup> aplikaci strany integrované s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="319e3-124">When signing up for, or signing in to, a 3<sup>rd</sup> party application integrated with Azure Active Directory.</span></span> <span data-ttu-id="319e3-125">Příkladem toho je [Smartsheet](https://app.smartsheet.com/b/home) nebo [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span><span class="sxs-lookup"><span data-stu-id="319e3-125">One example of this is [Smartsheet](https://app.smartsheet.com/b/home) or [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span></span>

3.  <span data-ttu-id="319e3-126">Pokud registrujete nebo přidání licenci na uživatele nebo skupiny k první aplikaci strany, jako například [Microsoft Office 365](http://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="319e3-126">When signing up for, or adding a license to a user or a group to a first party application, like [Microsoft Office 365](http://products.office.com/).</span></span>

4.  <span data-ttu-id="319e3-127">Když přidáte novou registraci aplikace tak, že vytvoříte vlastní vyvinuté aplikace pomocí [aplikace registru](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span><span class="sxs-lookup"><span data-stu-id="319e3-127">When you add a new application registration by creating a custom-developed application using the [Application Registry](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span></span>

5.  <span data-ttu-id="319e3-128">Když přidáte novou registraci aplikace tak, že vytvoříte vlastní vyvinuté aplikace pomocí [portálu pro registraci aplikace V2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span><span class="sxs-lookup"><span data-stu-id="319e3-128">When you add a new application registration by creating a custom-developed application using the [V2.0 Application Registration Portal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span></span>

6.  <span data-ttu-id="319e3-129">Když přidáváte aplikaci provádíte vývoj pomocí sady Visual Studio [metody ověřování ASP.net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) nebo [připojené služby](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span><span class="sxs-lookup"><span data-stu-id="319e3-129">When you add an application you’re developing using Visual Studio’s [ASP.net Authentication Methods](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) or [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span></span>

7.  <span data-ttu-id="319e3-130">Když vytvoříte službu objektu zabezpečení pomocí [modulu Azure AD PowerShell](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="319e3-130">When you create a service principal object using the [Azure AD PowerShell Module](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

8.  <span data-ttu-id="319e3-131">Pokud jste [souhlas aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) jako správce a používat data ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="319e3-131">When you [consent to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) as an administrator to use data in your tenant.</span></span>

9.  <span data-ttu-id="319e3-132">Když [uživatel souhlasí k aplikaci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) používat data ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="319e3-132">When a [user consents to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) to use data in your tenant.</span></span>

10. <span data-ttu-id="319e3-133">Když povolíte určité služby, které ukládají data ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="319e3-133">When you enable certain services that store data in your tenant.</span></span> <span data-ttu-id="319e3-134">Příkladem toho je resetování hesla, která je modelovaná jako objekt služby k uložení vaše heslo resetovat zásady bezpečně.</span><span class="sxs-lookup"><span data-stu-id="319e3-134">One example of this is Password Reset, which is modeled as a service principal to store your password reset policy securely.</span></span>

<span data-ttu-id="319e3-135">Chcete-li získat další informace o tom, jak aplikace jsou přidány do vašeho adresáře, přečtěte si [jak a proč se aplikace přidávají do služby Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span><span class="sxs-lookup"><span data-stu-id="319e3-135">To get more details on how apps are added to your directory, read [How and why applications are added to Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span></span>

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a><span data-ttu-id="319e3-136">Chcete odstranit přiřazení konkrétního uživatele nebo skupiny do aplikace</span><span class="sxs-lookup"><span data-stu-id="319e3-136">I want to remove a specific user’s or group’s assignment to an application</span></span>

<span data-ttu-id="319e3-137">Chcete-li odebrat uživatele nebo skupiny přiřazení k aplikaci, postupujte podle kroků uvedených v [odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) článku.</span><span class="sxs-lookup"><span data-stu-id="319e3-137">To remove a user or group assignment to an application, follow the steps listed in the [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a><span data-ttu-id="319e3-138">Chcete zakázat veškerý přístup k aplikaci pro každého uživatele</span><span class="sxs-lookup"><span data-stu-id="319e3-138">I want to disable all access to an application for every user</span></span>

<span data-ttu-id="319e3-139">Pokud chcete zakázat všechna uživatelská přihlášení k aplikaci, postupujte podle kroků uvedených v [zakázat uživatelská přihlášení pro podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) článku.</span><span class="sxs-lookup"><span data-stu-id="319e3-139">To disable all user sign-ins to an application, follow the steps listed in the [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-to-delete-an-application-entirely"></a><span data-ttu-id="319e3-140">Chcete odstranit aplikaci zcela</span><span class="sxs-lookup"><span data-stu-id="319e3-140">I want to delete an application entirely</span></span>

<span data-ttu-id="319e3-141">K **odstranit aplikaci**, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="319e3-141">To **delete an application**, follow the instructions below:</span></span>

1.  <span data-ttu-id="319e3-142">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="319e3-142">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="319e3-143">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="319e3-143">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="319e3-144">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="319e3-144">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="319e3-145">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="319e3-145">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="319e3-146">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="319e3-146">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="319e3-147">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="319e3-147">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="319e3-148">Vyberte aplikaci, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="319e3-148">Select the application you want to delete.</span></span>

7.  <span data-ttu-id="319e3-149">Po načtení aplikace, klikněte na **odstranit** ikonu z hlavní aplikace **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="319e3-149">Once the application loads, click **Delete** icon from the top application’s **Overview** blade.</span></span>

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a><span data-ttu-id="319e3-150">Chcete zakázat všechny operace souhlasu budoucí uživatele do jakékoli aplikace</span><span class="sxs-lookup"><span data-stu-id="319e3-150">I want to disable all future user consent operations to any application</span></span>

<span data-ttu-id="319e3-151">Zakázání souhlas uživatele pro souhlas pro žádnou aplikaci koncovým uživatelům zabránit celý adresář.</span><span class="sxs-lookup"><span data-stu-id="319e3-151">Disabling user consent for your entire directory prevent end users from consenting to any application.</span></span> <span data-ttu-id="319e3-152">Správci mohou stále souhlas na behalves uživatele.</span><span class="sxs-lookup"><span data-stu-id="319e3-152">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="319e3-153">Další informace o aplikaci souhlas, a proto může nebo nemusí Chcete to provést, číst [Principy uživatelů a správce souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="319e3-153">To learn more about application consent, and why you may or may not wish to do this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="319e3-154">K **zakažte všechny operace souhlasu budoucí uživatele v adresáři celý**, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="319e3-154">To **disable all future user consent operations in your entire directory**, follow the instructions below:</span></span>

1.  <span data-ttu-id="319e3-155">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="319e3-155">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="319e3-156">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="319e3-156">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="319e3-157">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="319e3-157">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="319e3-158">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="319e3-158">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="319e3-159">Klikněte na tlačítko **uživatelská nastavení**.</span><span class="sxs-lookup"><span data-stu-id="319e3-159">click **User settings**.</span></span>

6.  <span data-ttu-id="319e3-160">Zakažte všechny budoucí uživatele souhlasu operace nastavením **uživatelé můžou aplikace přistupovat ke svým datům** přepnutím **ne** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="319e3-160">Disable all future user consent operations by setting the **Users can allow apps to access their data** toggle to **No** and click the **Save** button.</span></span>

## <a name="next-steps"></a><span data-ttu-id="319e3-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="319e3-161">Next steps</span></span>
[<span data-ttu-id="319e3-162">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="319e3-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
