---
title: "Nesprávný sadu uživatelů jsou se zřídí k aplikaci Galerie Azure AD | Microsoft Docs"
description: "Zjistěte, jak zjistit, proč jsou se zřídí jinou sadu uživatelů do aplikace než ty, které jste očekávali"
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
ms.openlocfilehash: 85b533584c8ec15a23be32e20db7de583fced6a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a><span data-ttu-id="7544c-103">Nesprávný sadu uživatelů jsou se zřídí k aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="7544c-103">Wrong set of users are being provisioned to an Azure AD Gallery application</span></span>

<span data-ttu-id="7544c-104">Uživatele, kteří jsou zřízené v aplikaci je primárně vycházejí z kteří uživatelé a skupiny byly **přiřazené** do aplikace.</span><span class="sxs-lookup"><span data-stu-id="7544c-104">Which users are provisioned to the app is primarily driven by which users and groups have been **assigned** to the application.</span></span>

<span data-ttu-id="7544c-105">Pomocí níže uvedených zdrojů se dozvíte, jak zkontrolovat, kteří uživatelé a skupiny mají přiřazený k aplikaci v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7544c-105">Use the resources below to learn how to check which users and groups have been assigned to an application within Azure Active Directory.</span></span>

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="7544c-106">Přiřazení uživatele přímo jako správce</span><span class="sxs-lookup"><span data-stu-id="7544c-106">Assign a user directly as an administrator</span></span>

<span data-ttu-id="7544c-107">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="7544c-107">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="7544c-108">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="7544c-108">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="7544c-109">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="7544c-109">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="7544c-110">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="7544c-110">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="7544c-111">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7544c-111">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="7544c-112">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="7544c-112">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="7544c-113">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="7544c-113">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="7544c-114">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="7544c-114">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="7544c-115">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="7544c-115">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="7544c-116">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="7544c-116">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="7544c-117">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="7544c-117">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="7544c-118">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="7544c-118">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="7544c-119">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="7544c-119">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="7544c-120">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="7544c-120">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="7544c-121">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="7544c-121">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="7544c-122">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7544c-122">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="7544c-123">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="7544c-123">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="7544c-124">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7544c-124">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="7544c-125">Pokud je nakonfigurován zřizování a pro aplikace je již spuštěn, noví uživatelé musí být zřízená aplikace v přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="7544c-125">If provisioning is configured and already running for an app, new users should be provisioned to an application in approximately 10 minutes.</span></span> <span data-ttu-id="7544c-126">Zkontrolujte **protokoly auditu** podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7544c-126">Check the **Audit Logs** for details.</span></span>

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a><span data-ttu-id="7544c-127">Přiřazení skupiny přímo na aplikace jako správce</span><span class="sxs-lookup"><span data-stu-id="7544c-127">Assign a group directly to an application as an administrator</span></span>

<span data-ttu-id="7544c-128">Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="7544c-128">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="7544c-129">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="7544c-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="7544c-130">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="7544c-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="7544c-131">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="7544c-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="7544c-132">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7544c-132">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="7544c-133">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="7544c-133">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="7544c-134">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="7544c-134">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="7544c-135">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="7544c-135">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="7544c-136">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="7544c-136">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="7544c-137">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="7544c-137">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="7544c-138">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="7544c-138">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="7544c-139">Zadejte **název úplné skupiny** vás zajímá přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="7544c-139">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="7544c-140">Najeďte myší **skupiny** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="7544c-140">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="7544c-141">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="7544c-141">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="7544c-142">**Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="7544c-142">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="7544c-143">Po dokončení výběru skupiny klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7544c-143">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="7544c-144">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit ke skupinám, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="7544c-144">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="7544c-145">Klikněte **přiřadit** tlačítko přiřazení aplikace k vybraným skupinám.</span><span class="sxs-lookup"><span data-stu-id="7544c-145">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="7544c-146">Pokud je nakonfigurován zřizování a pro aplikace je již spuštěn, noví uživatelé, obsažené v rámci skupiny by měl být zřízená aplikace v přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="7544c-146">If provisioning is configured and already running for an app, new users contained within the group should be provisioned to an application in approximately 10 minutes.</span></span> <span data-ttu-id="7544c-147">Zkontrolujte **protokoly auditu** podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7544c-147">Check the **Audit Logs** for details.</span></span>

>[!IMPORTANT]
><span data-ttu-id="7544c-148">Zřizování název skupiny a údaje skupiny, kromě členy, pokud pro některé aplikace podporován.</span><span class="sxs-lookup"><span data-stu-id="7544c-148">Provisioning of the group name and group details, in addition to the members, if supported for some applications.</span></span> <span data-ttu-id="7544c-149">Můžete povolit nebo zakázat tuto funkci povolením nebo zakázáním **mapování** pro objekty skupiny ukazuje **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="7544c-149">You can enable or disable this functionality by enabling or disabling the **Mapping** for group objects shown in the **Provisioning** tab.</span></span> 
>
>

<span data-ttu-id="7544c-150">Pokud je povoleno zřizování skupiny, nezapomeňte si projít mapování atributů k zajištění, že na odpovídající pole je používána pro "Odpovídající ID".</span><span class="sxs-lookup"><span data-stu-id="7544c-150">If provisioning groups is enabled, be sure to review the attribute mappings to ensure an appropriate field is being used for the “matching ID”.</span></span> <span data-ttu-id="7544c-151">To může být zobrazovaný název nebo e-mailu alias, jako skupiny a její členy nelze zřídit Pokud odpovídající vlastnost není prázdný, nebo není vyplněný pro skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7544c-151">This can be the display name or email alias, as the group and its members not be provisioned if the matching property is empty or not populated for a group in Azure AD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7544c-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7544c-152">Next steps</span></span>
[<span data-ttu-id="7544c-153">Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7544c-153">Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)
