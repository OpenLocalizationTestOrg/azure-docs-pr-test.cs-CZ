---
title: "Správa skupin ve službě Azure Active Directory | Dokumentace Microsoftu"
description: "Postupy při vytváření a správě skupin pro správu uživatelů Azure pomocí služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 2cc2b63312b331a19c61cd7b59a4cac78edf32e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="managing-groups-in-azure-active-directory"></a><span data-ttu-id="5d9fa-103">Správa skupin ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d9fa-103">Managing groups in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5d9fa-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5d9fa-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="5d9fa-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="5d9fa-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="5d9fa-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d9fa-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="5d9fa-107">Jedna z funkcí správy uživatelů služby Azure Active Directory (Azure AD) je možnost vytváření skupin uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-107">One of the features of Azure Active Directory (Azure AD) user management is the ability to create groups of users.</span></span> <span data-ttu-id="5d9fa-108">Pomocí skupiny můžete provádět úlohy správy, jako je přiřazení licencí nebo oprávnění víc uživatelům současně.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-108">You use a group to perform management tasks such as assigning licenses or permissions to a number of users at once.</span></span> <span data-ttu-id="5d9fa-109">Skupiny můžete použít také k přiřazení přístupových oprávnění pro:</span><span class="sxs-lookup"><span data-stu-id="5d9fa-109">You can also use groups to assign access permission to</span></span>

* <span data-ttu-id="5d9fa-110">prostředky, například objekty v adresáři</span><span class="sxs-lookup"><span data-stu-id="5d9fa-110">Resources such as objects in the directory</span></span>
* <span data-ttu-id="5d9fa-111">Prostředky, které jsou pro adresář externí, třeba aplikace SaaS, služby Azure, weby SharePoint nebo místní prostředky</span><span class="sxs-lookup"><span data-stu-id="5d9fa-111">Resources external to the directory such as SaaS applications, Azure services, SharePoint sites, or on-premises resources</span></span>

<span data-ttu-id="5d9fa-112">Vlastník prostředku může navíc přístup k prostředku přiřadit i skupině služby Azure AD, která patří někomu jinému.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-112">In addition, a resource owner can also assign access to a resource to an Azure AD group owned by someone else.</span></span> <span data-ttu-id="5d9fa-113">Tím získají členové této skupiny přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-113">This assignment grants the members of that group access to the resource.</span></span> <span data-ttu-id="5d9fa-114">Vlastník skupiny potom spravuje členství ve skupině.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-114">Then, the owner of the group manages membership in the group.</span></span> <span data-ttu-id="5d9fa-115">Vlastník prostředku deleguje vlastníkovi skupiny oprávnění přiřazovat uživatele k jeho prostředku.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-115">Effectively, the resource owner delegates to the owner of the group the permission to assign users to their resource.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d9fa-116">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-116">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="5d9fa-117">Informace o správě skupin v centru pro správu Azure AD najdete v tématu [Vytvoření skupiny a přidání členů v Azure Active Directory](active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5d9fa-117">For how to manage groups in the Azure AD admin center, see [Create a group and add members in Azure Active Directory](active-directory-groups-create-azure-portal.md).</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="5d9fa-118">Vytvoření skupiny</span><span class="sxs-lookup"><span data-stu-id="5d9fa-118">How do I create a group?</span></span>
<span data-ttu-id="5d9fa-119">V závislosti na tom, jaké služby má vaše organizace předplacené, můžete vytvořit skupinu pomocí některé z těchto služeb:</span><span class="sxs-lookup"><span data-stu-id="5d9fa-119">Depending on the services to which your organization has subscribed, you can create a group using one of the following:</span></span>

* <span data-ttu-id="5d9fa-120">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="5d9fa-120">the Azure classic portal</span></span>
* <span data-ttu-id="5d9fa-121">Portál účtu služeb Office 365</span><span class="sxs-lookup"><span data-stu-id="5d9fa-121">the Office 365 account portal</span></span>
* <span data-ttu-id="5d9fa-122">Portál účtu Windows Intune</span><span class="sxs-lookup"><span data-stu-id="5d9fa-122">the Windows Intune account portal</span></span>

<span data-ttu-id="5d9fa-123">Budeme popisovat úlohy prováděné na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-123">We'll describe tasks as performed in the Azure classic portal.</span></span> <span data-ttu-id="5d9fa-124">Další informace o používání portálů mimo Azure ke správě adresáře služby Azure AD najdete v článku [Správa adresáře služby Azure AD](active-directory-administer.md).</span><span class="sxs-lookup"><span data-stu-id="5d9fa-124">For more information about using non-Azure portals to manage your Azure AD directory, see [Administering your Azure AD directory](active-directory-administer.md).</span></span>

1. <span data-ttu-id="5d9fa-125">Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-125">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="5d9fa-126">Vyberte kartu **Skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-126">Select the **Groups** tab.</span></span>
3. <span data-ttu-id="5d9fa-127">Vyberte **Přidat skupinu**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-127">Select **Add Group**.</span></span>
4. <span data-ttu-id="5d9fa-128">V okně **Přidat skupinu** zadejte název a popis skupiny.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-128">In the **Add Group** window, specify the name and the description of a group.</span></span>

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a><span data-ttu-id="5d9fa-129">Přidání nebo odebrání jednotlivých uživatelů ve skupině zabezpečení</span><span class="sxs-lookup"><span data-stu-id="5d9fa-129">How do I add or remove individual users in a security group?</span></span>
<span data-ttu-id="5d9fa-130">**Přidání jednotlivého uživatele do skupiny**</span><span class="sxs-lookup"><span data-stu-id="5d9fa-130">**To add an individual user to a group**</span></span>

1. <span data-ttu-id="5d9fa-131">Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-131">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="5d9fa-132">Vyberte kartu **Skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-132">Select the **Groups** tab.</span></span>
3. <span data-ttu-id="5d9fa-133">Otevřete skupinu, do které chcete přidat členy.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-133">Open the group to which you want to add members.</span></span> <span data-ttu-id="5d9fa-134">Pro vybranou skupinu otevřete kartu **Členové**, pokud ještě není zobrazená.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-134">Open the **Members** tab of the selected group if it not already displaying.</span></span>
4. <span data-ttu-id="5d9fa-135">Vyberte **Přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-135">Select **Add Members**.</span></span>
5. <span data-ttu-id="5d9fa-136">Na stránce **Přidat členy** vyberte jméno uživatele nebo název skupiny, které chcete přidat jako člena této skupiny.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-136">On the **Add Members** page, select the name of the user or a group that you want to add as a member of this group.</span></span> <span data-ttu-id="5d9fa-137">Nezapomeňte toto jméno nebo název přidat i do podokna **Vybraní**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-137">Make sure that this name is added to the **Selected** pane.</span></span>

<span data-ttu-id="5d9fa-138">**Odebrání jednotlivého uživatele ze skupiny**</span><span class="sxs-lookup"><span data-stu-id="5d9fa-138">**To remove an individual user from a group**</span></span>

1. <span data-ttu-id="5d9fa-139">Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-139">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="5d9fa-140">Vyberte kartu **Skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-140">Select the **Groups** tab.</span></span>
3. <span data-ttu-id="5d9fa-141">Otevřete skupinu, ze které chcete odebrat členy.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-141">Open the group from which you want to remove members.</span></span>
4. <span data-ttu-id="5d9fa-142">Vyberte kartu **Členové**, vyberte jméno člena, kterého chcete z této skupiny odebrat, a potom klikněte na **Odebrat**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-142">Select the **Members** tab, select the name of the member that you want to remove from this group, and then click **Remove**.</span></span>
5. <span data-ttu-id="5d9fa-143">V zobrazené výzvě potvrďte, že chcete tohoto člena ze skupiny odebrat.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-143">Confirm at the prompt that you want to remove this member from the group.</span></span>

## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a><span data-ttu-id="5d9fa-144">Dynamická správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="5d9fa-144">How can I manage the membership of a group dynamically?</span></span>
<span data-ttu-id="5d9fa-145">Ve službě Azure AD můžete velmi snadno nastavit jednoduché pravidlo a určit tak uživatele, kteří se mají stát členy skupiny.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-145">In Azure AD, you can very easily set up a simple rule to determine which users are to be members of the group.</span></span> <span data-ttu-id="5d9fa-146">Jednoduché pravidlo je takové pravidlo, které provádí jenom jedno porovnání.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-146">A simple rule is one that makes only a single comparison.</span></span> <span data-ttu-id="5d9fa-147">Pokud je třeba některá skupina přiřazená aplikaci SaaS, můžete vytvořit pravidlo, které zajistí přidání uživatelů s názvem pracovní pozice „Obchodní zástupce“.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-147">For example, if a group is assigned to a SaaS application, you can set up a rule to add users who have a job title of "Sales Rep."</span></span> <span data-ttu-id="5d9fa-148">Toto pravidlo potom udělí přístup do této aplikace SaaS všem uživatelům s touto pracovní pozicí ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-148">This rule then grants access to this SaaS application to all users with that job title in your directory.</span></span>

<span data-ttu-id="5d9fa-149">Když se změní libovolné atributy uživatele, systém vyhodnotí všechna dynamická pravidla skupin v adresáři a zjistí, zda změna uživatele aktivuje nějaké přidání do skupiny nebo odebrání ze skupiny.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-149">When any attributes of a user change, the system evaluates all dynamic group rules in a directory to see if the attribute change of the user would trigger any group adds or removes.</span></span> <span data-ttu-id="5d9fa-150">Pokud uživatel splňuje pravidlo pro skupinu, je do této skupiny přidán jako člen.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-150">If a user satisfies a rule on a group, they are added as a member to that group.</span></span> <span data-ttu-id="5d9fa-151">Pokud již uživatel pravidlo pro skupinu, ve které je členem, nesplňuje, je jeho členství ve skupině odebráno.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-151">If they no longer satisfy the rule of a group they are a member of, they are removed as a members from that group.</span></span>

> [!NOTE]
> <span data-ttu-id="5d9fa-152">Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-152">You can set up a rule for dynamic membership on security groups or Office 365 groups.</span></span> <span data-ttu-id="5d9fa-153">Vnořené členství ve skupinách se v současné době nepodporuje v případě přiřazování k aplikacím podle skupiny.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-153">Nested group memberships aren't currently supported for group-based assignment to applications.</span></span>
>
> <span data-ttu-id="5d9fa-154">Dynamické členství ve skupinách vyžaduje přiřazení licence služby Azure AD Premium následujícím uživatelům:</span><span class="sxs-lookup"><span data-stu-id="5d9fa-154">Dynamic memberships for groups require an Azure AD Premium license to be assigned to</span></span>
>
> * <span data-ttu-id="5d9fa-155">správce, který spravuje pravidlo pro skupinu</span><span class="sxs-lookup"><span data-stu-id="5d9fa-155">The administrator who manages the rule on a group</span></span>
> * <span data-ttu-id="5d9fa-156">Všichni členové skupiny</span><span class="sxs-lookup"><span data-stu-id="5d9fa-156">All members of the group</span></span>
>
>

<span data-ttu-id="5d9fa-157">**Povolení dynamického členství ve skupině**</span><span class="sxs-lookup"><span data-stu-id="5d9fa-157">**To enable dynamic membership for a group**</span></span>

1. <span data-ttu-id="5d9fa-158">Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-158">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="5d9fa-159">Vyberte kartu **Skupiny** a otevřete skupinu, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-159">Select the **Groups** tab, and open the group you want to edit.</span></span>
3. <span data-ttu-id="5d9fa-160">Vyberte kartu **Konfigurace** a potom nastavte možnost **Povolit dynamické členství** na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-160">Select the **Configure** tab, and then set **Enable Dynamic Memberships** to **Yes**.</span></span>
4. <span data-ttu-id="5d9fa-161">Nastavte jedno jednoduché pravidlo pro skupinu, které bude řídit dynamické členství pro funkce v této skupině.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-161">Set up a simple single rule for the group to control how dynamic membership for this group functions.</span></span> <span data-ttu-id="5d9fa-162">Zkontrolujte, jestli jste vybrali možnost **Přidat uživatele, kde**, a potom vyberte vlastnost uživatele ze seznamu (například oddělení, pracovní funkce atd.),</span><span class="sxs-lookup"><span data-stu-id="5d9fa-162">Make sure the **Add users where** option is selected, and then select a user property from the list (for example, department, jobTitle, etc.),</span></span>
5. <span data-ttu-id="5d9fa-163">V dalším kroku vyberte podmínku (nerovná se, rovná se, nezačíná, začíná, neobsahuje, obsahuje, neodpovídá, odpovídá).</span><span class="sxs-lookup"><span data-stu-id="5d9fa-163">Next, select a condition (Not Equals, Equals, Not Starts With, Starts With, Not Contains, Contains, Not Match, Match).</span></span>
6. <span data-ttu-id="5d9fa-164">Zadejte hodnotu vybrané vlastnosti uživatele.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-164">Specify a comparison value for the selected user property.</span></span>

<span data-ttu-id="5d9fa-165">Další informace o vytváření *rozšířených* pravidel (která můžou obsahovat několik porovnání) pro dynamické členství ve skupině najdete v článku o [používání atributů k vytvoření rozšířených pravidel](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="5d9fa-165">To learn about how to create *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

## <a name="additional-information"></a><span data-ttu-id="5d9fa-166">Další informace</span><span class="sxs-lookup"><span data-stu-id="5d9fa-166">Additional information</span></span>
<span data-ttu-id="5d9fa-167">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d9fa-167">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="5d9fa-168">Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d9fa-168">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="5d9fa-169">Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="5d9fa-169">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="5d9fa-170">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d9fa-170">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="5d9fa-171">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d9fa-171">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="5d9fa-172">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d9fa-172">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
