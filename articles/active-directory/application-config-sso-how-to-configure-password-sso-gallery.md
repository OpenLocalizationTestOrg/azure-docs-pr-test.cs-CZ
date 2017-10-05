---
title: "Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Postup konfigurace aplikace pro zabezpečení založené na heslech jednotné přihlašování, když je již uveden v galerii aplikací Azure AD"
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
ms.openlocfilehash: d4dc110eb25c3e550ac4663d28e626a696b58f62
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="f1446-103">Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1446-103">How to configure password single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="f1446-104">Když přidáte aplikace [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), máte možnost volby způsob uživatelům se přihlásit k dané aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1446-104">When you add an application from the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), you have the choice of how you want your users to sign in to that application.</span></span> <span data-ttu-id="f1446-105">Tato volba kdykoli můžete nakonfigurovat tak, že vyberete **jednotné přihlašování** navigační položka na podniková aplikace v [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f1446-105">You can configure this choice at any time by selecting the **Single Sign-on** navigation item on an Enterprise Application in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="f1446-106">Jeden z jedné přihlašování metod k dispozici je [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) možnost.</span><span class="sxs-lookup"><span data-stu-id="f1446-106">One of the single sign-on methods available to you is the [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="f1446-107">To je skvělý způsob, jak začít rychle integrace aplikací do Azure AD a umožňuje:</span><span class="sxs-lookup"><span data-stu-id="f1446-107">This is a great way to get started integrating applications into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="f1446-108">Povolit **jednotné přihlašování pro vaše uživatele** bezpečně ukládání a přehrání uživatelských jmen a hesel pro aplikaci jste integrované s Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1446-108">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for the application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="f1446-109">**Podpora aplikací, které vyžadují více polí přihlášení** pro aplikace, které vyžadují více než jen uživatelské jméno a heslo pole pro přihlášení</span><span class="sxs-lookup"><span data-stu-id="f1446-109">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields to sign in</span></span>

-   <span data-ttu-id="f1446-110">**Přizpůsobení popisků** vstupní pole uživatelské jméno a heslo se uživatelům zobrazí na [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) při jejich zadat své přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="f1446-110">**Customize the labels** of the username and password input fields your users see on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="f1446-111">Povolit vaše **uživatelé** zajistit vlastní uživatelská jména a hesla pro všechny existující účty aplikace zadávání v ručně na [aplikace přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="f1446-111">Allow your **users** to provide their own usernames and passwords for any existing application accounts they are typing in manually on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="f1446-112">Povolit **členem obchodní skupině** a zadat uživatelská jména a hesla přiřazen k uživateli pomocí [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funkce</span><span class="sxs-lookup"><span data-stu-id="f1446-112">Allow a **member of the business group** to specify the usernames and passwords assigned to a user by using the [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="f1446-113">Povolit **správce** k určení uživatelských jmen a hesel přiřazen k uživateli pomocí přihlašovacích údajů aktualizace funkcí při [přiřazení uživatele k aplikaci](#assign-a-user-to-an-application-directly)</span><span class="sxs-lookup"><span data-stu-id="f1446-113">Allow an **administrator** to specify the usernames and passwords assigned to a user by using the Update Credentials feature when [assigning a user to an application](#assign-a-user-to-an-application-directly)</span></span>

-   <span data-ttu-id="f1446-114">Povolit **správce** k určení sdílené uživatelské jméno nebo heslo použité pro skupinu uživatelů pomocí přihlašovacích údajů aktualizace funkcí při [přiřazení skupiny k aplikaci](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="f1446-114">Allow an **administrator** to specify the shared username or password used by a group of people by using the Update Credentials feature when [assigning a group to an application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="f1446-115">Níže popisuje, jak můžete povolit [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) k aplikaci, která se již [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="f1446-115">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) to an application that is already in the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="f1446-116">Přehled kroků potřebných</span><span class="sxs-lookup"><span data-stu-id="f1446-116">Overview of steps required</span></span>
<span data-ttu-id="f1446-117">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="f1446-117">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="f1446-118">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1446-118">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="f1446-119">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1446-119">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="f1446-120">Přiřazení aplikace k uživatele nebo skupinu</span><span class="sxs-lookup"><span data-stu-id="f1446-120">Assign the application to a user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="f1446-121">Přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="f1446-121">Assign a user to an application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="f1446-122">Přiřadit přímo aplikaci pro skupinu</span><span class="sxs-lookup"><span data-stu-id="f1446-122">Assign an application to a group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="f1446-123">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1446-123">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="f1446-124">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f1446-124">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="f1446-125">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="f1446-125">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="f1446-126">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="f1446-126">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f1446-127">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f1446-127">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f1446-128">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f1446-128">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f1446-129">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="f1446-129">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="f1446-130">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace</span><span class="sxs-lookup"><span data-stu-id="f1446-130">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application</span></span>

7.  <span data-ttu-id="f1446-131">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1446-131">Select the application you want to configure for single sign-on</span></span>

8.  <span data-ttu-id="f1446-132">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f1446-132">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="f1446-133">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1446-133">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="f1446-134">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1446-134">After a short period, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="f1446-135">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1446-135">Configure the application for password single sign-on</span></span>

<span data-ttu-id="f1446-136">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f1446-136">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="f1446-137">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="f1446-137">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="f1446-138">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="f1446-138">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f1446-139">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f1446-139">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f1446-140">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f1446-140">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f1446-141">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f1446-141">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="f1446-142">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f1446-142">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f1446-143">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1446-143">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="f1446-144">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1446-144">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f1446-145">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="f1446-145">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="f1446-146">[Přiřazení uživatelů k aplikaci](#assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="f1446-146">[Assign users to the application](#assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="f1446-147">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1446-147">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="f1446-148">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="f1446-148">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="assign-a-user-to-an-application-directly"></a><span data-ttu-id="f1446-149">Přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="f1446-149">Assign a user to an application directly</span></span>

<span data-ttu-id="f1446-150">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f1446-150">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="f1446-151">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="f1446-151">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f1446-152">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="f1446-152">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f1446-153">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f1446-153">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f1446-154">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f1446-154">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f1446-155">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f1446-155">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="f1446-156">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f1446-156">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f1446-157">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1446-157">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="f1446-158">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1446-158">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f1446-159">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f1446-159">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f1446-160">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f1446-160">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f1446-161">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f1446-161">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f1446-162">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="f1446-162">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="f1446-163">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1446-163">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="f1446-164">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1446-164">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="f1446-165">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1446-165">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="f1446-166">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f1446-166">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="f1446-167">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1446-167">Click the **Assign** button to assign the application to the selected users.</span></span>

## <a name="assign-an-application-to-a-group-directly"></a><span data-ttu-id="f1446-168">Přiřadit přímo aplikaci pro skupinu</span><span class="sxs-lookup"><span data-stu-id="f1446-168">Assign an application to a group directly</span></span>

<span data-ttu-id="f1446-169">Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f1446-169">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="f1446-170">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="f1446-170">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f1446-171">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="f1446-171">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f1446-172">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f1446-172">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f1446-173">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f1446-173">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f1446-174">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f1446-174">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="f1446-175">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f1446-175">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f1446-176">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1446-176">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="f1446-177">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1446-177">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="f1446-178">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f1446-178">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="f1446-179">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="f1446-179">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="f1446-180">Zadejte **název úplné skupiny** vás zajímá přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="f1446-180">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="f1446-181">Najeďte myší **skupiny** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="f1446-181">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="f1446-182">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1446-182">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="f1446-183">**Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1446-183">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="f1446-184">Po dokončení výběru skupiny klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1446-184">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="f1446-185">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit ke skupinám, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f1446-185">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="f1446-186">Klikněte **přiřadit** tlačítko přiřazení aplikace k vybraným skupinám.</span><span class="sxs-lookup"><span data-stu-id="f1446-186">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="f1446-187">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="f1446-187">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1446-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1446-188">Next steps</span></span>
[<span data-ttu-id="f1446-189">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f1446-189">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
