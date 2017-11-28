---
title: "Vývoj aplikací pro Azure AD | Microsoft Docs"
description: "Napsané pro IT specialisty, tento článek obsahuje pokyny k integraci aplikací Azure se službou Active Directory."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b119be9c06d8c1ccc8e747168429e6c2d2e7a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a><span data-ttu-id="34b00-103">Vyvíjet-obchodní aplikace pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="34b00-103">Develop line-of-business apps for Azure Active Directory</span></span>
<span data-ttu-id="34b00-104">Tato příručka poskytuje přehled vývoje-obchodní aplikace (LoB) pro Azure Active Directory (AD). Předpokládanou cílovou skupinou je globální správci Active Directory nebo Office 365.</span><span class="sxs-lookup"><span data-stu-id="34b00-104">This guide provides an overview of developing line-of-business (LoB) applications for Azure Active Directory (AD).The intended audience is Active Directory/Office 365 global administrators.</span></span>

## <a name="overview"></a><span data-ttu-id="34b00-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="34b00-105">Overview</span></span>
<span data-ttu-id="34b00-106">Vytváření aplikací, které jsou integrované s Azure AD poskytuje uživatelům ve vaší organizaci jednotné přihlašování s Office 365.</span><span class="sxs-lookup"><span data-stu-id="34b00-106">Building applications integrated with Azure AD gives users in your organization single sign-on with Office 365.</span></span> <span data-ttu-id="34b00-107">S aplikace ve službě Azure AD umožňuje spravovat přes zásady ověřování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34b00-107">Having the application in Azure AD gives you control over the authentication policy for the application.</span></span> <span data-ttu-id="34b00-108">Další informace o podmíněný přístup a ochrana aplikací pomocí služby Multi-Factor authentication (MFA) najdete v tématu [pravidla přístupu k konfigurace](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="34b00-108">To learn more about conditional access and how to protect apps with multi-factor authentication (MFA) see [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

<span data-ttu-id="34b00-109">Registrace aplikace pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="34b00-109">Register your application to use Azure Active Directory.</span></span> <span data-ttu-id="34b00-110">Registrace aplikace znamená, že vaše vývojáři použít Azure AD ověřuje uživatele a požádat o přístup k prostředkům uživatele, například e-mailu, kalendáři a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="34b00-110">Registering the application means that your developers can use Azure AD to authenticate users and request access to user resources such as email, calendar, and documents.</span></span>

<span data-ttu-id="34b00-111">Každý člen adresáři (ne Hosté) můžete zaregistrovat aplikaci, v opačném případě se označuje jako *vytvoření objektu aplikace*.</span><span class="sxs-lookup"><span data-stu-id="34b00-111">Any member of your directory (not guests) can register an application, otherwise known as *creating an application object*.</span></span>

<span data-ttu-id="34b00-112">Registrace aplikace umožňuje všem uživatelům postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="34b00-112">Registering an application allows any user to do the following:</span></span>

* <span data-ttu-id="34b00-113">Získat identity pro svou aplikaci, který rozpoznává Azure AD</span><span class="sxs-lookup"><span data-stu-id="34b00-113">Get an identity for their application that Azure AD recognizes</span></span>
* <span data-ttu-id="34b00-114">Získat jeden nebo více tajné klíče nebo klíče, které aplikace můžete použít k vlastnímu ověření AD</span><span class="sxs-lookup"><span data-stu-id="34b00-114">Get one or more secrets/keys that the application can use to authenticate itself to AD</span></span>
* <span data-ttu-id="34b00-115">Značky aplikace na portálu Azure s vlastní název, logem, atd.</span><span class="sxs-lookup"><span data-stu-id="34b00-115">Brand the application in the Azure portal with a custom name, logo, etc.</span></span>
* <span data-ttu-id="34b00-116">Použití funkce autorizace Azure AD do své aplikace, včetně:</span><span class="sxs-lookup"><span data-stu-id="34b00-116">Apply Azure AD authorization features to their app, including:</span></span>

  * <span data-ttu-id="34b00-117">Řízení přístupu na základě role (RBAC)</span><span class="sxs-lookup"><span data-stu-id="34b00-117">Role-Based Access Control (RBAC)</span></span>
  * <span data-ttu-id="34b00-118">Azure Active Directory jako server autorizace oAuth (zabezpečení rozhraní API zveřejněný prostřednictvím aplikace)</span><span class="sxs-lookup"><span data-stu-id="34b00-118">Azure Active Directory as oAuth authorization server (secure an API exposed by the application)</span></span>
* <span data-ttu-id="34b00-119">Deklarujte požadovaná oprávnění nutné pro funkce aplikace podle očekávání, včetně:</span><span class="sxs-lookup"><span data-stu-id="34b00-119">Declare required permissions necessary for the application to function as expected, including:</span></span>

      - <span data-ttu-id="34b00-120">Oprávnění aplikací (jenom globální správci).</span><span class="sxs-lookup"><span data-stu-id="34b00-120">App permissions (global administrators only).</span></span> <span data-ttu-id="34b00-121">Příklad: Role členství v jiné službě Azure AD aplikace nebo role členství relativně k prostředku Azure, skupiny prostředků nebo předplatného</span><span class="sxs-lookup"><span data-stu-id="34b00-121">For example: Role membership in another Azure AD application or role membership relative to an Azure Resource, Resource Group, or Subscription</span></span>
      - <span data-ttu-id="34b00-122">Přidělená oprávnění (všechny uživatele).</span><span class="sxs-lookup"><span data-stu-id="34b00-122">Delegated permissions (any user).</span></span> <span data-ttu-id="34b00-123">Příklad: Azure AD, přihlášení a čtení profilu</span><span class="sxs-lookup"><span data-stu-id="34b00-123">For example: Azure AD, Sign-in, and Read Profile</span></span>

> [!NOTE]
> <span data-ttu-id="34b00-124">Ve výchozím nastavení může každý člen zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34b00-124">By default, any member can register an application.</span></span> <span data-ttu-id="34b00-125">Zjistěte, jak omezit oprávnění pro registraci aplikace do konkrétních členů, najdete v tématu [jak se aplikace přidávají do služby Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span><span class="sxs-lookup"><span data-stu-id="34b00-125">To learn how to restrict permissions for registering applications to specific members, see [How applications are added to Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span></span>
>
>

<span data-ttu-id="34b00-126">Tady je co, globální správce, musíte udělat, což vývojářům Zkontrolujte své aplikace připravené pro produkci:</span><span class="sxs-lookup"><span data-stu-id="34b00-126">Here’s what you, the global administrator, need to do to help developers make their application ready for production:</span></span>

* <span data-ttu-id="34b00-127">Konfigurace pravidel přístupu (zásady přístupu vícefaktorového ověřování)</span><span class="sxs-lookup"><span data-stu-id="34b00-127">Configure access rules (access policy/MFA)</span></span>
* <span data-ttu-id="34b00-128">Nakonfiguruje aplikaci, aby vyžadují přiřazení uživatelů a přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="34b00-128">Configure the app to require user assignment and assign users</span></span>
* <span data-ttu-id="34b00-129">Potlačit výchozí uživatelské souhlasu prostředí</span><span class="sxs-lookup"><span data-stu-id="34b00-129">Suppress the default user consent experience</span></span>

## <a name="configure-access-rules"></a><span data-ttu-id="34b00-130">Nakonfigurovat pravidla přístupu</span><span class="sxs-lookup"><span data-stu-id="34b00-130">Configure access rules</span></span>
<span data-ttu-id="34b00-131">Nakonfigurujte každou aplikaci přístup pravidla pro aplikace SaaS.</span><span class="sxs-lookup"><span data-stu-id="34b00-131">Configure per-application access rules to your SaaS apps.</span></span> <span data-ttu-id="34b00-132">Můžete například vyžadovat vícefaktorové ověřování nebo přístup k uživatelům povolit jenom v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="34b00-132">For example, you can require MFA or only allow access to users on trusted networks.</span></span> <span data-ttu-id="34b00-133">Podrobnosti o to jsou k dispozici v dokumentu [pravidla přístupu k konfigurace](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="34b00-133">The details for this are available in the document [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a><span data-ttu-id="34b00-134">Nakonfiguruje aplikaci, aby vyžadují přiřazení uživatelů a přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="34b00-134">Configure the app to require user assignment and assign users</span></span>
<span data-ttu-id="34b00-135">Ve výchozím nastavení uživatelé mohou využívat aplikace bez přiřazení.</span><span class="sxs-lookup"><span data-stu-id="34b00-135">By default, users can access applications without being assigned.</span></span> <span data-ttu-id="34b00-136">Ale pokud aplikace zpřístupní role nebo pokud chcete aplikace se objeví na panel přístupu uživatele, byste měli vyžadovat přiřazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="34b00-136">However, if the application exposes roles or if you want the application to appear on a user’s access panel, you should require user assignment.</span></span>

[<span data-ttu-id="34b00-137">Vyžádání přiřazení uživatele</span><span class="sxs-lookup"><span data-stu-id="34b00-137">Requiring user assignment</span></span>](active-directory-applications-guiding-developers-requiring-user-assignment.md)

<span data-ttu-id="34b00-138">Pokud jste Azure AD Premium nebo Enterprise Mobility Suite (EMS) odběratele, důrazně doporučujeme použití skupin.</span><span class="sxs-lookup"><span data-stu-id="34b00-138">If you’re an Azure AD Premium or Enterprise Mobility Suite (EMS) subscriber, we strongly recommend using groups.</span></span> <span data-ttu-id="34b00-139">Přiřazení skupiny k aplikaci, můžete delegovat správu probíhající přístupu vlastníkovi skupiny.</span><span class="sxs-lookup"><span data-stu-id="34b00-139">Assigning groups to the application allows you to delegate ongoing access management to the owner of the group.</span></span> <span data-ttu-id="34b00-140">Můžete vytvořit skupinu, nebo požádejte zodpovědná strany ve vaší organizaci vytvoření skupiny pomocí vaše skupiny správy zařízení.</span><span class="sxs-lookup"><span data-stu-id="34b00-140">You can create the group or ask the responsible party in your organization to create the group using your group management facility.</span></span>

[<span data-ttu-id="34b00-141">Přiřazování uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="34b00-141">Assigning users to an application</span></span>](active-directory-applications-guiding-developers-assigning-users.md)  
[<span data-ttu-id="34b00-142">Přiřazování skupin k aplikaci</span><span class="sxs-lookup"><span data-stu-id="34b00-142">Assigning groups to an application</span></span>](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a><span data-ttu-id="34b00-143">Potlačit souhlas uživatele</span><span class="sxs-lookup"><span data-stu-id="34b00-143">Suppress user consent</span></span>
<span data-ttu-id="34b00-144">Ve výchozím nastavení každý uživatel prochází souhlasu prostředí pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="34b00-144">By default, each user goes through a consent experience to sign in.</span></span> <span data-ttu-id="34b00-145">Prostředí souhlasu, požádat uživatele udělit oprávnění k aplikaci, může být zneklidňovat pro uživatele, kteří jsou obeznámeni s rozhodování.</span><span class="sxs-lookup"><span data-stu-id="34b00-145">The consent experience, asking users to grant permissions to an application, can be disconcerting for users who are unfamiliar with making such decisions.</span></span>

<span data-ttu-id="34b00-146">Pro aplikace, které důvěřujete můžete zjednodušit uživatelské prostředí tím souhlas k aplikaci jménem vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="34b00-146">For applications that you trust, you can simplify the user experience by consenting to the application on behalf of your organization.</span></span>

<span data-ttu-id="34b00-147">Další informace o souhlas uživatele a souhlasu prostředí v Azure, najdete v části [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="34b00-147">For more information about user consent and the consent experience in Azure, see [Integrating Applications with Azure Active Directory](active-directory-integrating-applications.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="34b00-148">Související články</span><span class="sxs-lookup"><span data-stu-id="34b00-148">Related Articles</span></span>
* [<span data-ttu-id="34b00-149">Povolit zabezpečený vzdálený přístup k místním aplikacím s proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="34b00-149">Enable secure remote access to on-premises applications with Azure AD Application Proxy</span></span>](active-directory-application-proxy-get-started.md)
* [<span data-ttu-id="34b00-150">Verze Preview Azure podmíněného přístupu pro aplikace SaaS</span><span class="sxs-lookup"><span data-stu-id="34b00-150">Azure Conditional Access Preview for SaaS Apps</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="34b00-151">Správa přístupu k aplikacím s Azure AD</span><span class="sxs-lookup"><span data-stu-id="34b00-151">Managing access to apps with Azure AD</span></span>](active-directory-managing-access-to-apps.md)
* [<span data-ttu-id="34b00-152">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="34b00-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
