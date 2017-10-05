---
title: "Postup konfigurace hesel jednotné přihlašování pro jiný Galerie applicationn | Microsoft Docs"
description: "Jak nakonfigurovat vlastní aplikace bez Galerie pro zabezpečení založené na heslech jednotné přihlašování, když není uveden v galerii aplikací Azure AD"
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
ms.openlocfilehash: f629ec99824199306ebf825901beaa99d83d434d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="4f981-103">Postup konfigurace hesel jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="4f981-103">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="4f981-104">Kromě volby najít v galerii aplikací Azure AD, máte také možnost Přidat **aplikace bez Galerie** při aplikaci, kterou má není v seznamu uvedeno.</span><span class="sxs-lookup"><span data-stu-id="4f981-104">In addition to the choices found within the Azure AD Application Gallery, you also have the option to add a **non-gallery application** when the application you want is not listed there.</span></span> <span data-ttu-id="4f981-105">Díky této funkci můžete přidat libovolné aplikace, která již existuje ve vaší organizaci, nebo jakékoli aplikaci jiného výrobce, který můžete použít od dodavatele, který není již součástí [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="4f981-105">Using this capability, you can add any application that already exists in your organization, or any third-party application that you might use from a vendor who is not already part of the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

<span data-ttu-id="4f981-106">Jakmile přidáte aplikace bez galerie, potom můžete nakonfigurovat jednu metodu přihlašování tato aplikace používá výběrem **jednotné přihlašování** navigační položka na podniková aplikace v [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4f981-106">Once you add a non-gallery application, you can then configure the Single sign-on method this application uses by selecting the **Single Sign-on** navigation item on an Enterprise Application in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="4f981-107">Jeden z jednotného přihlašování metod k dispozici je [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) možnost.</span><span class="sxs-lookup"><span data-stu-id="4f981-107">One of the Single Sign-on methods available to you is the [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="4f981-108">Pomocí **přidání aplikace bez Galerie** prostředí, můžete integrovat jakékoli aplikace, která vykreslí uživatelské jméno ve formátu HTML a heslo vstupní pole, i když není v našem sadu předem integrovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="4f981-108">With the **add a non-gallery application** experience, you can integrate any application that renders an HTML-based username and password input field, even if it is not in our set of pre-integrated applications.</span></span>

<span data-ttu-id="4f981-109">Na stránce oškrabávání technologie, která je součástí přístupového panelu linku, která umožňuje automaticky rozpoznat uživatelské jméno a heslo vstupní pole, bezpečně uložit pro konkrétní aplikaci instanci, je způsob, jakým tento postup funguje.</span><span class="sxs-lookup"><span data-stu-id="4f981-109">The way this works is by a page scraping technology that is part of the Access Panel extension that allows us to auto-detect username and password input fields, store them securely for your specific application instance.</span></span> <span data-ttu-id="4f981-110">Bezpečně přehráním uživatelských jmen a hesel do těchto polí, když uživatel přejde k dané aplikaci na panel přístupu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f981-110">Then securely replay usernames and passwords to those fields when a user navigates to that application on the application access panel.</span></span>

<span data-ttu-id="4f981-111">To je skvělý způsob, jak začít rychle integraci jakékoliv aplikace do Azure AD a umožňuje:</span><span class="sxs-lookup"><span data-stu-id="4f981-111">This is a great way to get started integrating any kind of application into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="4f981-112">Integrovat **všechny aplikace na světě** s klientovi Azure AD, takže pokud vykreslí uživatelské jméno a heslo vstupní pole HTML</span><span class="sxs-lookup"><span data-stu-id="4f981-112">Integrate **any application in the world** with your Azure AD tenant, so long as it renders an HTML username and password input field</span></span>

-   <span data-ttu-id="4f981-113">Povolit **jednotné přihlašování pro vaše uživatele** bezpečně ukládání a přehrání uživatelských jmen a hesel pro aplikaci jste integrované s Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f981-113">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for the application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="4f981-114">**Automaticky rozpoznat vstup** polí pro každou aplikaci, a umožňují ručně zjistit těchto polí pomocí rozšíření prohlížeče Panel přístupu, v případě, že je nenajde automatického zjištění</span><span class="sxs-lookup"><span data-stu-id="4f981-114">**Auto-detect input** fields for any application and allow you to manually detect those fields using the Access Panel Browser Extension, in case auto-detection does not find them</span></span>

-   <span data-ttu-id="4f981-115">**Podpora aplikací, které vyžadují více polí přihlášení** pro aplikace, které vyžadují více než jen uživatelské jméno a heslo pole pro přihlášení</span><span class="sxs-lookup"><span data-stu-id="4f981-115">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields to sign in</span></span>

-   <span data-ttu-id="4f981-116">**Přizpůsobení popisků** vstupní pole uživatelské jméno a heslo se uživatelům zobrazí na [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) při jejich zadat své přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="4f981-116">**Customize the labels** of the username and password input fields your users see on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="4f981-117">Povolit vaše **uživatelé** zajistit vlastní uživatelská jména a hesla pro všechny existující účty aplikace zadávání v ručně na [aplikace přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="4f981-117">Allow your **users** to provide their own usernames and passwords for any existing application accounts they are typing in manually on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="4f981-118">Povolit **členem obchodní skupině** a zadat uživatelská jména a hesla přiřazen k uživateli pomocí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funkce</span><span class="sxs-lookup"><span data-stu-id="4f981-118">Allow a **member of the business group** to specify the usernames and passwords assigned to a user by using the [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="4f981-119">Povolit **správce** k určení uživatelských jmen a hesel přiřazen k uživateli pomocí přihlašovacích údajů aktualizace funkcí při [přiřazení uživatele k aplikaci](#_How_to_configure_1)</span><span class="sxs-lookup"><span data-stu-id="4f981-119">Allow an **administrator** to specify the usernames and passwords assigned to a user by using the Update Credentials feature when [assigning a user to an application](#_How_to_configure_1)</span></span>

-   <span data-ttu-id="4f981-120">Povolit **správce** k určení sdílené uživatelské jméno nebo heslo použité pro skupinu uživatelů pomocí přihlašovacích údajů aktualizace funkcí při [přiřazení skupiny k aplikaci](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="4f981-120">Allow an **administrator** to specify the shared username or password used by a group of people by using the Update Credentials feature when [assigning a group to an application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="4f981-121">Níže popisuje, jak můžete povolit [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) do jakékoli aplikace, která přidáte pomocí **přidání aplikace bez Galerie** prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f981-121">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) to any application that you add using the **add a non-gallery application** experience.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="4f981-122">Přehled kroků potřebných</span><span class="sxs-lookup"><span data-stu-id="4f981-122">Overview of steps required</span></span>

<span data-ttu-id="4f981-123">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="4f981-123">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="4f981-124">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="4f981-124">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="4f981-125">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4f981-125">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="4f981-126">Přiřazení aplikace k uživatele nebo skupinu</span><span class="sxs-lookup"><span data-stu-id="4f981-126">Assign the application to a user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="4f981-127">Přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="4f981-127">Assign a user to an application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="4f981-128">Přiřadit přímo aplikaci pro skupinu</span><span class="sxs-lookup"><span data-stu-id="4f981-128">Assign an application to a group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a><span data-ttu-id="4f981-129">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="4f981-129">Add a non-gallery application</span></span>

<span data-ttu-id="4f981-130">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4f981-130">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="4f981-131">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="4f981-131">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="4f981-132">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4f981-132">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4f981-133">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4f981-133">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4f981-134">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f981-134">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4f981-135">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="4f981-135">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="4f981-136">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4f981-136">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="4f981-137">Zadejte název vaší aplikace v **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="4f981-137">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="4f981-138">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="4f981-138">Select **Add.**</span></span>

<span data-ttu-id="4f981-139">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f981-139">After a short period, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="4f981-140">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4f981-140">Configure the application for password single sign-on</span></span>

<span data-ttu-id="4f981-141">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4f981-141">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="4f981-142">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="4f981-142">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4f981-143">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4f981-143">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4f981-144">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4f981-144">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4f981-145">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f981-145">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4f981-146">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="4f981-146">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4f981-147">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4f981-147">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4f981-148">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4f981-148">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="4f981-149">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f981-149">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4f981-150">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="4f981-150">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="4f981-151">Zadejte **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="4f981-151">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="4f981-152">Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f981-152">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="4f981-153">Zkontrolujte, zda že přihlašovací pole jsou viditelné na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="4f981-153">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="4f981-154">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f981-154">Assign users to the application.</span></span>

11. <span data-ttu-id="4f981-155">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f981-155">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="4f981-156">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="4f981-156">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="assign-a-user-to-an-application-directly"></a><span data-ttu-id="4f981-157">Přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="4f981-157">Assign a user to an application directly</span></span>

<span data-ttu-id="4f981-158">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4f981-158">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4f981-159">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4f981-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4f981-160">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4f981-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4f981-161">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4f981-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4f981-162">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f981-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4f981-163">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="4f981-163">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4f981-164">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4f981-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4f981-165">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f981-165">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4f981-166">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f981-166">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4f981-167">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="4f981-167">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4f981-168">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="4f981-168">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4f981-169">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4f981-169">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4f981-170">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="4f981-170">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4f981-171">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f981-171">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4f981-172">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f981-172">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="4f981-173">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f981-173">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4f981-174">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="4f981-174">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="4f981-175">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f981-175">Click the **Assign** button to assign the application to the selected users.</span></span>

## <a name="assign-an-application-to-a-group-directly"></a><span data-ttu-id="4f981-176">Přiřadit přímo aplikaci pro skupinu</span><span class="sxs-lookup"><span data-stu-id="4f981-176">Assign an application to a group directly</span></span>

<span data-ttu-id="4f981-177">Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4f981-177">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4f981-178">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4f981-178">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4f981-179">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4f981-179">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4f981-180">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4f981-180">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4f981-181">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f981-181">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4f981-182">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="4f981-182">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4f981-183">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4f981-183">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4f981-184">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f981-184">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4f981-185">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f981-185">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4f981-186">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="4f981-186">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4f981-187">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="4f981-187">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4f981-188">Zadejte **název úplné skupiny** vás zajímá přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4f981-188">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4f981-189">Najeďte myší **skupiny** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="4f981-189">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4f981-190">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f981-190">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4f981-191">**Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f981-191">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="4f981-192">Po dokončení výběru skupiny klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f981-192">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4f981-193">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit ke skupinám, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="4f981-193">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="4f981-194">Klikněte **přiřadit** tlačítko přiřazení aplikace k vybraným skupinám.</span><span class="sxs-lookup"><span data-stu-id="4f981-194">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="4f981-195">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="4f981-195">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f981-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f981-196">Next steps</span></span>
[<span data-ttu-id="4f981-197">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="4f981-197">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
