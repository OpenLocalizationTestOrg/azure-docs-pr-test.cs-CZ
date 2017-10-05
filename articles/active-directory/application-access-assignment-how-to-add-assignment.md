---
title: "Přiřazení uživatelů a skupin k aplikaci | Microsoft Docs"
description: "Přiřazení uživatelů k aplikaci udělit přístup"
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
ms.openlocfilehash: 61536612e0dd5102b8f5e911c350826846f5ed77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-assign-users-and-groups-to-an-application"></a><span data-ttu-id="0b07b-103">Přiřazení uživatelů a skupin k aplikaci</span><span class="sxs-lookup"><span data-stu-id="0b07b-103">How to assign users and groups to an application</span></span>

<span data-ttu-id="0b07b-104">Předtím, než mohou uživatelé provádět některé z níže pro konkrétní aplikace, budete muset první **je přiřadit k aplikaci** a udělit mu přístup:</span><span class="sxs-lookup"><span data-stu-id="0b07b-104">Before your users can do any of the below for a specific application, you need to first **assign them to the application** to grant them access:</span></span>

-   <span data-ttu-id="0b07b-105">Přístup k aplikaci pomocí **přejdete na adresu URL aplikace přímo** (také označované jako spouštěná SP přihlášení).</span><span class="sxs-lookup"><span data-stu-id="0b07b-105">Access an application by **navigating to the application’s URL directly** (also known as SP-initiated sign-on).</span></span>

-   <span data-ttu-id="0b07b-106">Přístup k aplikaci pomocí **adresu URL pro přístup uživatelů** na aplikace **vlastnosti** stránky (také označované jako spouštěná IDP přihlašování).</span><span class="sxs-lookup"><span data-stu-id="0b07b-106">Access an application by using the **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).</span></span>

-   <span data-ttu-id="0b07b-107">V tématu aplikace se zobrazují v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="0b07b-107">See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.</span></span>

-   <span data-ttu-id="0b07b-108">V tématu aplikace se zobrazují v jejich [Spouštěč aplikace Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span><span class="sxs-lookup"><span data-stu-id="0b07b-108">See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span></span>

## <a name="methods-to-assign-applications-with-azure-active-directory"></a><span data-ttu-id="0b07b-109">Metody přiřadit aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b07b-109">Methods to assign applications with Azure Active Directory</span></span> 

<span data-ttu-id="0b07b-110">Aplikace s Azure Active Directory můžete přiřadit 3 způsoby:</span><span class="sxs-lookup"><span data-stu-id="0b07b-110">There are 3 ways you can assign applications with Azure Active Directory:</span></span>

-   [<span data-ttu-id="0b07b-111">Přiřazení uživatele přímo k aplikaci jako správce</span><span class="sxs-lookup"><span data-stu-id="0b07b-111">Assign a user directly to an application as an administrator</span></span>](#assign-a-user-directly-as-an-administrator)

-   [<span data-ttu-id="0b07b-112">Přiřazení skupiny přímo na aplikace jako správce</span><span class="sxs-lookup"><span data-stu-id="0b07b-112">Assign a group directly to an application as an administrator</span></span>](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [<span data-ttu-id="0b07b-113">Povolit přístup k aplikaci Samoobslužné služby umožnit uživatelům najít vlastní aplikace</span><span class="sxs-lookup"><span data-stu-id="0b07b-113">Enable self-service application access to allow users to find their own applications</span></span>](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="0b07b-114">Přiřazení uživatele přímo jako správce</span><span class="sxs-lookup"><span data-stu-id="0b07b-114">Assign a user directly as an administrator</span></span>

<span data-ttu-id="0b07b-115">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="0b07b-115">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="0b07b-116">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-116">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0b07b-117">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="0b07b-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0b07b-118">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="0b07b-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0b07b-119">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0b07b-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0b07b-120">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="0b07b-120">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="0b07b-121">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-121">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="0b07b-122">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-122">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="0b07b-123">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b07b-123">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0b07b-124">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="0b07b-124">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="0b07b-125">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="0b07b-125">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="0b07b-126">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="0b07b-126">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="0b07b-127">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="0b07b-127">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="0b07b-128">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-128">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="0b07b-129">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-129">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="0b07b-130">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b07b-130">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="0b07b-131">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="0b07b-131">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="0b07b-132">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b07b-132">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="0b07b-133">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace pomocí metody popsané v části popis řešení.</span><span class="sxs-lookup"><span data-stu-id="0b07b-133">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a><span data-ttu-id="0b07b-134">Přiřazení skupiny přímo na aplikace jako správce</span><span class="sxs-lookup"><span data-stu-id="0b07b-134">Assign a group directly to an application as an administrator</span></span>

<span data-ttu-id="0b07b-135">Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="0b07b-135">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="0b07b-136">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-136">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0b07b-137">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="0b07b-137">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0b07b-138">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="0b07b-138">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0b07b-139">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0b07b-139">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0b07b-140">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="0b07b-140">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="0b07b-141">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-141">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="0b07b-142">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-142">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="0b07b-143">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b07b-143">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0b07b-144">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="0b07b-144">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="0b07b-145">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="0b07b-145">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="0b07b-146">Zadejte **název úplné skupiny** vás zajímá přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="0b07b-146">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="0b07b-147">Najeďte myší **skupiny** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="0b07b-147">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="0b07b-148">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-148">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="0b07b-149">**Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-149">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="0b07b-150">Po dokončení výběru skupiny klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b07b-150">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="0b07b-151">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit ke skupinám, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="0b07b-151">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="0b07b-152">Klikněte **přiřadit** tlačítko přiřazení aplikace k vybraným skupinám.</span><span class="sxs-lookup"><span data-stu-id="0b07b-152">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="0b07b-153">Po krátké době uživatelů v rámci skupiny, které jste vybrali moci spustit tyto aplikace pomocí metody popsané v části popis řešení.</span><span class="sxs-lookup"><span data-stu-id="0b07b-153">After a short period of time, the users within the groups you have selected be able to launch these applications using the methods described in the solution description section.</span></span> <span data-ttu-id="0b07b-154">Pokud jsou dynamické skupiny, může být zpoždění některé další zpracování v těchto přiřazení zobrazování pro uživatele v rámci těchto přiřazeny skupiny.</span><span class="sxs-lookup"><span data-stu-id="0b07b-154">If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.</span></span>

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a><span data-ttu-id="0b07b-155">Povolit přístup k aplikaci Samoobslužné služby umožnit uživatelům najít vlastní aplikace</span><span class="sxs-lookup"><span data-stu-id="0b07b-155">Enable self-service application access to allow users to find their own applications</span></span>

<span data-ttu-id="0b07b-156">Přístup k aplikaci Samoobslužné služby je skvělým způsobem, jak povolit uživatelům samoobslužné zjišťování aplikací, můžete povolit obchodní skupiny můžete schválit přístup pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b07b-156">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="0b07b-157">Můžete povolit obchodní skupině pro správu přiřazené pro tyto uživatele pro heslo jednotné přihlašování v aplikacích vpravo z jejich přístup panelů přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0b07b-157">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="0b07b-158">Pokud chcete povolit samoobslužné služby aplikaci přístup k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="0b07b-158">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="0b07b-159">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0b07b-160">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="0b07b-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0b07b-161">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="0b07b-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0b07b-162">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0b07b-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0b07b-163">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="0b07b-163">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="0b07b-164">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="0b07b-165">Vyberte aplikaci, které chcete povolit samoobslužné přístup ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-165">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="0b07b-166">Po načtení aplikace, klikněte na **samoobslužné služby** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b07b-166">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0b07b-167">Pokud chcete povolit přístup k aplikaci Samoobslužné služby pro tuto aplikaci, zapněte **povolit uživatelům žádat o přístup k této aplikaci?** přepnutím **Ano.**</span><span class="sxs-lookup"><span data-stu-id="0b07b-167">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="0b07b-168">V dalším kroku vyberte skupiny, které uživatelům, kteří požadují by se měl přístup k této aplikaci přidat, klikněte na tlačítko modulu pro výběr vedle popisek **skupinu, pro kterou má přiřazené byli přidáni uživatelé?** a vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="0b07b-168">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="0b07b-169">**Volitelné:** nastaví, pokud chcete vyžadovat schválení obchodní před uživatelé mají povolen přístup **vyžadovat schválení před udělením přístupu k této aplikaci?** přepnutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0b07b-169">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="0b07b-170">**Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete povolit tyto firmy schvalovatelů k zadání hesla, které se odesílají na tuto žádost o schválení uživatelé, nastavte **povolit schvalovatelů k nastavení hesla uživatele pro tuto aplikaci?** přepnutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0b07b-170">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="0b07b-171">**Volitelné:** pro zadání schvalovatelů firmy, kteří mají povoleno schválit přístup k této aplikaci, klikněte na výběr vedle popisek **kdo může schválit přístup k této aplikaci?** můžete vybrat až 10 jednotlivé obchodní schvalovatele.</span><span class="sxs-lookup"><span data-stu-id="0b07b-171">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

  >[!NOTE]
  ><span data-ttu-id="0b07b-172">Skupiny nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="0b07b-172">Groups are not supported.</span></span>
  >
  >

13. <span data-ttu-id="0b07b-173">**Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete přiřadit roli schválené uživatelé samoobslužné služby, klikněte na modulu pro výběr vedle **do role, které by měl být přiřazena uživatelům v této aplikaci?** vyberte roli, ke kterému by se měla přiřadit těmto uživatelům.</span><span class="sxs-lookup"><span data-stu-id="0b07b-173">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="0b07b-174">Klikněte **Uložit** tlačítka v horní části okna dokončit.</span><span class="sxs-lookup"><span data-stu-id="0b07b-174">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="0b07b-175">Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít na jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko **+ přidat** tlačítko k vyhledání aplikace, na které jste povolili samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="0b07b-175">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="0b07b-176">Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="0b07b-176">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="0b07b-177">Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup k aplikaci, která vyžaduje schválení.</span><span class="sxs-lookup"><span data-stu-id="0b07b-177">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="0b07b-178">Tato schválení podporovat pouze jeden schválení pracovních, což znamená, že pokud zadáte několik schvalovatelů, může jeden schvalovatel schvalovatel přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b07b-178">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b07b-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b07b-179">Next steps</span></span>
[<span data-ttu-id="0b07b-180">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="0b07b-180">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
