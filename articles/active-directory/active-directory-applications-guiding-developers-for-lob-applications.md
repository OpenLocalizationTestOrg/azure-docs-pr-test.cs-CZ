---
title: "aaaDevelop aplikací pro Azure AD | Microsoft Docs"
description: "Napsané pro IT specialisty hello, tento článek obsahuje pokyny k integraci aplikací Azure se službou Active Directory."
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
ms.openlocfilehash: d2924be752af0be2843b1d9b74d9ec446d3fe1ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a><span data-ttu-id="e358e-103">Vyvíjet-obchodní aplikace pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e358e-103">Develop line-of-business apps for Azure Active Directory</span></span>
<span data-ttu-id="e358e-104">Tato příručka poskytuje přehled vývoje-obchodní aplikace (LoB) pro Azure Active Directory (AD) .hello určené cílové skupiny je globální správci Active Directory nebo Office 365.</span><span class="sxs-lookup"><span data-stu-id="e358e-104">This guide provides an overview of developing line-of-business (LoB) applications for Azure Active Directory (AD).hello intended audience is Active Directory/Office 365 global administrators.</span></span>

## <a name="overview"></a><span data-ttu-id="e358e-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="e358e-105">Overview</span></span>
<span data-ttu-id="e358e-106">Vytváření aplikací, které jsou integrované s Azure AD poskytuje uživatelům ve vaší organizaci jednotné přihlašování s Office 365.</span><span class="sxs-lookup"><span data-stu-id="e358e-106">Building applications integrated with Azure AD gives users in your organization single sign-on with Office 365.</span></span> <span data-ttu-id="e358e-107">S hello aplikace ve službě Azure AD umožňuje spravovat přes hello zásady ověřování pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="e358e-107">Having hello application in Azure AD gives you control over hello authentication policy for hello application.</span></span> <span data-ttu-id="e358e-108">Další informace o podmíněného přístupu a jak zjistit, tooprotect aplikací pomocí služby Multi-Factor authentication (MFA) toolearn [pravidla přístupu k konfigurace](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="e358e-108">toolearn more about conditional access and how tooprotect apps with multi-factor authentication (MFA) see [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

<span data-ttu-id="e358e-109">Zaregistrujte toouse vaše aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e358e-109">Register your application toouse Azure Active Directory.</span></span> <span data-ttu-id="e358e-110">Registrace aplikace hello znamená, že vaše vývojáři mohou použít tooauthenticate uživatele Azure AD a požádat o přístup k prostředkům toouser například e-mailu, kalendáři a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="e358e-110">Registering hello application means that your developers can use Azure AD tooauthenticate users and request access toouser resources such as email, calendar, and documents.</span></span>

<span data-ttu-id="e358e-111">Každý člen adresáři (ne Hosté) můžete zaregistrovat aplikaci, v opačném případě se označuje jako *vytvoření objektu aplikace*.</span><span class="sxs-lookup"><span data-stu-id="e358e-111">Any member of your directory (not guests) can register an application, otherwise known as *creating an application object*.</span></span>

<span data-ttu-id="e358e-112">Registrace aplikace umožňuje všechny uživatele toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="e358e-112">Registering an application allows any user toodo hello following:</span></span>

* <span data-ttu-id="e358e-113">Získat identity pro svou aplikaci, který rozpoznává Azure AD</span><span class="sxs-lookup"><span data-stu-id="e358e-113">Get an identity for their application that Azure AD recognizes</span></span>
* <span data-ttu-id="e358e-114">Získat jeden nebo více tajné klíče nebo klíče, které hello aplikace můžete použít tooauthenticate sám sebe tooAD</span><span class="sxs-lookup"><span data-stu-id="e358e-114">Get one or more secrets/keys that hello application can use tooauthenticate itself tooAD</span></span>
* <span data-ttu-id="e358e-115">Aplikace hello Brand hello portál Azure s vlastní název, logem, atd.</span><span class="sxs-lookup"><span data-stu-id="e358e-115">Brand hello application in hello Azure portal with a custom name, logo, etc.</span></span>
* <span data-ttu-id="e358e-116">Použít aplikaci tootheir autorizace funkce Azure AD, včetně:</span><span class="sxs-lookup"><span data-stu-id="e358e-116">Apply Azure AD authorization features tootheir app, including:</span></span>

  * <span data-ttu-id="e358e-117">Řízení přístupu na základě role (RBAC)</span><span class="sxs-lookup"><span data-stu-id="e358e-117">Role-Based Access Control (RBAC)</span></span>
  * <span data-ttu-id="e358e-118">Azure Active Directory jako server autorizace oAuth (zabezpečení rozhraní API vystavené aplikace hello)</span><span class="sxs-lookup"><span data-stu-id="e358e-118">Azure Active Directory as oAuth authorization server (secure an API exposed by hello application)</span></span>
* <span data-ttu-id="e358e-119">Deklarujte požadovaná oprávnění potřebná pro toofunction aplikace hello podle očekávání, včetně:</span><span class="sxs-lookup"><span data-stu-id="e358e-119">Declare required permissions necessary for hello application toofunction as expected, including:</span></span>

      - <span data-ttu-id="e358e-120">Oprávnění aplikací (jenom globální správci).</span><span class="sxs-lookup"><span data-stu-id="e358e-120">App permissions (global administrators only).</span></span> <span data-ttu-id="e358e-121">Příklad: členství Role v jiné službě Azure AD aplikace nebo role členství relativní tooan prostředků Azure, skupinu prostředků nebo předplatného</span><span class="sxs-lookup"><span data-stu-id="e358e-121">For example: Role membership in another Azure AD application or role membership relative tooan Azure Resource, Resource Group, or Subscription</span></span>
      - <span data-ttu-id="e358e-122">Přidělená oprávnění (všechny uživatele).</span><span class="sxs-lookup"><span data-stu-id="e358e-122">Delegated permissions (any user).</span></span> <span data-ttu-id="e358e-123">Příklad: Azure AD, přihlášení a čtení profilu</span><span class="sxs-lookup"><span data-stu-id="e358e-123">For example: Azure AD, Sign-in, and Read Profile</span></span>

> [!NOTE]
> <span data-ttu-id="e358e-124">Ve výchozím nastavení může každý člen zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e358e-124">By default, any member can register an application.</span></span> <span data-ttu-id="e358e-125">toolearn jak zjistit, toorestrict oprávnění pro registraci aplikace toospecific členy, [jak se aplikace přidávají tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span><span class="sxs-lookup"><span data-stu-id="e358e-125">toolearn how toorestrict permissions for registering applications toospecific members, see [How applications are added tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span></span>
>
>

<span data-ttu-id="e358e-126">Tady je co jste globální správce hello, třeba toodo toohelp vývojář podá jejich aplikace připravené pro produkci:</span><span class="sxs-lookup"><span data-stu-id="e358e-126">Here’s what you, hello global administrator, need toodo toohelp developers make their application ready for production:</span></span>

* <span data-ttu-id="e358e-127">Konfigurace pravidel přístupu (zásady přístupu vícefaktorového ověřování)</span><span class="sxs-lookup"><span data-stu-id="e358e-127">Configure access rules (access policy/MFA)</span></span>
* <span data-ttu-id="e358e-128">Konfigurace přiřazení uživatelských toorequire aplikace hello a přiřadit uživatele</span><span class="sxs-lookup"><span data-stu-id="e358e-128">Configure hello app toorequire user assignment and assign users</span></span>
* <span data-ttu-id="e358e-129">Potlačit hello výchozí uživatelské souhlasu prostředí</span><span class="sxs-lookup"><span data-stu-id="e358e-129">Suppress hello default user consent experience</span></span>

## <a name="configure-access-rules"></a><span data-ttu-id="e358e-130">Nakonfigurovat pravidla přístupu</span><span class="sxs-lookup"><span data-stu-id="e358e-130">Configure access rules</span></span>
<span data-ttu-id="e358e-131">Konfigurace aplikace SaaS tooyour pravidla přístupu pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e358e-131">Configure per-application access rules tooyour SaaS apps.</span></span> <span data-ttu-id="e358e-132">Můžete například vyžadovat vícefaktorové ověřování nebo povolit toousers přístup jenom v důvěryhodných sítích.</span><span class="sxs-lookup"><span data-stu-id="e358e-132">For example, you can require MFA or only allow access toousers on trusted networks.</span></span> <span data-ttu-id="e358e-133">Podrobnosti Hello jsou k dispozici v dokumentu hello [pravidla přístupu k konfigurace](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="e358e-133">hello details for this are available in hello document [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

## <a name="configure-hello-app-toorequire-user-assignment-and-assign-users"></a><span data-ttu-id="e358e-134">Konfigurace přiřazení uživatelských toorequire aplikace hello a přiřadit uživatele</span><span class="sxs-lookup"><span data-stu-id="e358e-134">Configure hello app toorequire user assignment and assign users</span></span>
<span data-ttu-id="e358e-135">Ve výchozím nastavení uživatelé mohou využívat aplikace bez přiřazení.</span><span class="sxs-lookup"><span data-stu-id="e358e-135">By default, users can access applications without being assigned.</span></span> <span data-ttu-id="e358e-136">Ale pokud hello aplikace zpřístupní role nebo pokud chcete tooappear aplikace hello na panel přístupu uživatele, byste měli vyžadovat přiřazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e358e-136">However, if hello application exposes roles or if you want hello application tooappear on a user’s access panel, you should require user assignment.</span></span>

[<span data-ttu-id="e358e-137">Vyžádání přiřazení uživatele</span><span class="sxs-lookup"><span data-stu-id="e358e-137">Requiring user assignment</span></span>](active-directory-applications-guiding-developers-requiring-user-assignment.md)

<span data-ttu-id="e358e-138">Pokud jste Azure AD Premium nebo Enterprise Mobility Suite (EMS) odběratele, důrazně doporučujeme použití skupin.</span><span class="sxs-lookup"><span data-stu-id="e358e-138">If you’re an Azure AD Premium or Enterprise Mobility Suite (EMS) subscriber, we strongly recommend using groups.</span></span> <span data-ttu-id="e358e-139">Přiřazení skupiny toohello aplikace umožňuje toodelegate probíhající přístup správu toohello vlastníkem skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="e358e-139">Assigning groups toohello application allows you toodelegate ongoing access management toohello owner of hello group.</span></span> <span data-ttu-id="e358e-140">Můžete vytvořit skupiny hello nebo požádejte hello zodpovědná strany ve vaší organizaci toocreate hello skupině pomocí vaše skupiny správy zařízení.</span><span class="sxs-lookup"><span data-stu-id="e358e-140">You can create hello group or ask hello responsible party in your organization toocreate hello group using your group management facility.</span></span>

[<span data-ttu-id="e358e-141">Přiřazení uživatelů tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="e358e-141">Assigning users tooan application</span></span>](active-directory-applications-guiding-developers-assigning-users.md)  
[<span data-ttu-id="e358e-142">Přiřazení skupiny tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="e358e-142">Assigning groups tooan application</span></span>](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a><span data-ttu-id="e358e-143">Potlačit souhlas uživatele</span><span class="sxs-lookup"><span data-stu-id="e358e-143">Suppress user consent</span></span>
<span data-ttu-id="e358e-144">Ve výchozím nastavení každý uživatel prochází toosign prostředí souhlasu v.</span><span class="sxs-lookup"><span data-stu-id="e358e-144">By default, each user goes through a consent experience toosign in.</span></span> <span data-ttu-id="e358e-145">prostředí pro souhlasu Hello žádostí oprávnění uživatelé toogrant tooan aplikace, může být zneklidňovat pro uživatele, kteří jsou obeznámeni s rozhodování.</span><span class="sxs-lookup"><span data-stu-id="e358e-145">hello consent experience, asking users toogrant permissions tooan application, can be disconcerting for users who are unfamiliar with making such decisions.</span></span>

<span data-ttu-id="e358e-146">Pro aplikace, které důvěřujete můžete zjednodušit hello uživatelské prostředí aplikace toohello souhlas jménem vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="e358e-146">For applications that you trust, you can simplify hello user experience by consenting toohello application on behalf of your organization.</span></span>

<span data-ttu-id="e358e-147">Další informace o souhlas uživatele a hello souhlasu uživatele v Azure, najdete v části [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="e358e-147">For more information about user consent and hello consent experience in Azure, see [Integrating Applications with Azure Active Directory](active-directory-integrating-applications.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="e358e-148">Související články</span><span class="sxs-lookup"><span data-stu-id="e358e-148">Related Articles</span></span>
* [<span data-ttu-id="e358e-149">Povolit zabezpečený vzdálený přístup tooon místní aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e358e-149">Enable secure remote access tooon-premises applications with Azure AD Application Proxy</span></span>](active-directory-application-proxy-get-started.md)
* [<span data-ttu-id="e358e-150">Verze Preview Azure podmíněného přístupu pro aplikace SaaS</span><span class="sxs-lookup"><span data-stu-id="e358e-150">Azure Conditional Access Preview for SaaS Apps</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="e358e-151">Správa přístupu tooapps s Azure AD</span><span class="sxs-lookup"><span data-stu-id="e358e-151">Managing access tooapps with Azure AD</span></span>](active-directory-managing-access-to-apps.md)
* [<span data-ttu-id="e358e-152">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e358e-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
