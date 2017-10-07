---
title: aaaManaging skupin v Azure Active Directory | Microsoft Docs
description: "Jak toocreate a spravovat skupiny toomanage Azure uživatelů pomocí služby Azure Active Directory."
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
ms.openlocfilehash: 9bee224655639740b3dd99983892b30c3c537aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-groups-in-azure-active-directory"></a><span data-ttu-id="4816f-103">Správa skupin ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4816f-103">Managing groups in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4816f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4816f-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="4816f-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="4816f-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="4816f-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4816f-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="4816f-107">Jedna z funkcí hello správy uživatele Azure Active Directory (Azure AD) je hello možnost toocreate skupiny uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4816f-107">One of hello features of Azure Active Directory (Azure AD) user management is hello ability toocreate groups of users.</span></span> <span data-ttu-id="4816f-108">Používáte úlohy správy skupiny tooperform, jako je například přiřazení licence nebo oprávnění tooa počet uživatelů najednou.</span><span class="sxs-lookup"><span data-stu-id="4816f-108">You use a group tooperform management tasks such as assigning licenses or permissions tooa number of users at once.</span></span> <span data-ttu-id="4816f-109">Můžete také skupiny tooassign přístupová oprávnění k</span><span class="sxs-lookup"><span data-stu-id="4816f-109">You can also use groups tooassign access permission to</span></span>

* <span data-ttu-id="4816f-110">Prostředky, například objekty v adresáři hello</span><span class="sxs-lookup"><span data-stu-id="4816f-110">Resources such as objects in hello directory</span></span>
* <span data-ttu-id="4816f-111">Adresář externí toohello prostředky jako aplikace SaaS, služby Azure, weby služby SharePoint nebo místní prostředky</span><span class="sxs-lookup"><span data-stu-id="4816f-111">Resources external toohello directory such as SaaS applications, Azure services, SharePoint sites, or on-premises resources</span></span>

<span data-ttu-id="4816f-112">Vlastník prostředku kromě toho můžete přiřadit přístup tooa tooan Azure AD skupinu prostředků vlastníkem někdo jiný.</span><span class="sxs-lookup"><span data-stu-id="4816f-112">In addition, a resource owner can also assign access tooa resource tooan Azure AD group owned by someone else.</span></span> <span data-ttu-id="4816f-113">Toto přiřazení uděluje hello členové skupiny přístup toohello prostředku.</span><span class="sxs-lookup"><span data-stu-id="4816f-113">This assignment grants hello members of that group access toohello resource.</span></span> <span data-ttu-id="4816f-114">Vlastník hello hello skupiny potom spravuje členství ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="4816f-114">Then, hello owner of hello group manages membership in hello group.</span></span> <span data-ttu-id="4816f-115">Efektivně hello vlastníka delegáti toohello vlastník prostředku hello skupiny hello oprávnění tooassign uživatelé tootheir prostředku.</span><span class="sxs-lookup"><span data-stu-id="4816f-115">Effectively, hello resource owner delegates toohello owner of hello group hello permission tooassign users tootheir resource.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4816f-116">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4816f-116">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="4816f-117">Jak toomanage skupin v Centru pro správu hello Azure AD, najdete v části [vytvořte skupinu a přidejte členy v Azure Active Directory](active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4816f-117">For how toomanage groups in hello Azure AD admin center, see [Create a group and add members in Azure Active Directory](active-directory-groups-create-azure-portal.md).</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="4816f-118">Vytvoření skupiny</span><span class="sxs-lookup"><span data-stu-id="4816f-118">How do I create a group?</span></span>
<span data-ttu-id="4816f-119">V závislosti na toowhich hello služby, které má vaše organizace předplatné můžete vytvořit skupinu pomocí jedné z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4816f-119">Depending on hello services toowhich your organization has subscribed, you can create a group using one of hello following:</span></span>

* <span data-ttu-id="4816f-120">Hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="4816f-120">hello Azure classic portal</span></span>
* <span data-ttu-id="4816f-121">portál účtů Hello Office 365</span><span class="sxs-lookup"><span data-stu-id="4816f-121">hello Office 365 account portal</span></span>
* <span data-ttu-id="4816f-122">portál účtů Windows Intune Hello</span><span class="sxs-lookup"><span data-stu-id="4816f-122">hello Windows Intune account portal</span></span>

<span data-ttu-id="4816f-123">Úlohy popíšeme, jak provést v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="4816f-123">We'll describe tasks as performed in hello Azure classic portal.</span></span> <span data-ttu-id="4816f-124">Další informace o používání portálů mimo platformu Azure toomanage adresáře Azure AD najdete v tématu [Správa adresáře služby Azure AD](active-directory-administer.md).</span><span class="sxs-lookup"><span data-stu-id="4816f-124">For more information about using non-Azure portals toomanage your Azure AD directory, see [Administering your Azure AD directory](active-directory-administer.md).</span></span>

1. <span data-ttu-id="4816f-125">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="4816f-125">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="4816f-126">Vyberte hello **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="4816f-126">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="4816f-127">Vyberte **Přidat skupinu**.</span><span class="sxs-lookup"><span data-stu-id="4816f-127">Select **Add Group**.</span></span>
4. <span data-ttu-id="4816f-128">V hello **přidat skupinu** okno, zadejte hello název a popis skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="4816f-128">In hello **Add Group** window, specify hello name and hello description of a group.</span></span>

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a><span data-ttu-id="4816f-129">Přidání nebo odebrání jednotlivých uživatelů ve skupině zabezpečení</span><span class="sxs-lookup"><span data-stu-id="4816f-129">How do I add or remove individual users in a security group?</span></span>
<span data-ttu-id="4816f-130">**tooadd skupinu tooa jednotlivé uživatele**</span><span class="sxs-lookup"><span data-stu-id="4816f-130">**tooadd an individual user tooa group**</span></span>

1. <span data-ttu-id="4816f-131">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="4816f-131">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="4816f-132">Vyberte hello **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="4816f-132">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="4816f-133">Otevřete toowhich skupiny hello chcete tooadd členy.</span><span class="sxs-lookup"><span data-stu-id="4816f-133">Open hello group toowhich you want tooadd members.</span></span> <span data-ttu-id="4816f-134">Otevřete hello **členy** kartě hello vybrané skupiny, pokud ho ještě není zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4816f-134">Open hello **Members** tab of hello selected group if it not already displaying.</span></span>
4. <span data-ttu-id="4816f-135">Vyberte **Přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="4816f-135">Select **Add Members**.</span></span>
5. <span data-ttu-id="4816f-136">Na hello **přidat členy** stránky, vyberte hello název hello uživatele nebo skupinu, že chcete tooadd jako člena této skupiny.</span><span class="sxs-lookup"><span data-stu-id="4816f-136">On hello **Add Members** page, select hello name of hello user or a group that you want tooadd as a member of this group.</span></span> <span data-ttu-id="4816f-137">Ujistěte se, zda tento název je přidána toohello **vybrané** podokně.</span><span class="sxs-lookup"><span data-stu-id="4816f-137">Make sure that this name is added toohello **Selected** pane.</span></span>

<span data-ttu-id="4816f-138">**tooremove jednotlivého uživatele ze skupiny**</span><span class="sxs-lookup"><span data-stu-id="4816f-138">**tooremove an individual user from a group**</span></span>

1. <span data-ttu-id="4816f-139">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="4816f-139">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="4816f-140">Vyberte hello **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="4816f-140">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="4816f-141">Otevřete skupinu hello, ze kterého mají být členy tooremove.</span><span class="sxs-lookup"><span data-stu-id="4816f-141">Open hello group from which you want tooremove members.</span></span>
4. <span data-ttu-id="4816f-142">Vyberte hello **členy** karty, vyberte hello název člena hello má tooremove z této skupiny a pak klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="4816f-142">Select hello **Members** tab, select hello name of hello member that you want tooremove from this group, and then click **Remove**.</span></span>
5. <span data-ttu-id="4816f-143">Příkazového řádku hello potvrďte, že chcete tooremove tohoto člena ze skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="4816f-143">Confirm at hello prompt that you want tooremove this member from hello group.</span></span>

## <a name="how-can-i-manage-hello-membership-of-a-group-dynamically"></a><span data-ttu-id="4816f-144">Jak lze spravovat hello členství ve skupině dynamicky?</span><span class="sxs-lookup"><span data-stu-id="4816f-144">How can I manage hello membership of a group dynamically?</span></span>
<span data-ttu-id="4816f-145">Ve službě Azure AD můžete velmi snadno nastavit jednoduché pravidlo toodetermine, uživatelů, kteří jsou členy toobe skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="4816f-145">In Azure AD, you can very easily set up a simple rule toodetermine which users are toobe members of hello group.</span></span> <span data-ttu-id="4816f-146">Jednoduché pravidlo je takové pravidlo, které provádí jenom jedno porovnání.</span><span class="sxs-lookup"><span data-stu-id="4816f-146">A simple rule is one that makes only a single comparison.</span></span> <span data-ttu-id="4816f-147">Například pokud je skupina přiřazena tooa aplikace SaaS, můžete nastavit uživatele tooadd pravidlo, kteří mají na pozici "Obchodního zástupce".</span><span class="sxs-lookup"><span data-stu-id="4816f-147">For example, if a group is assigned tooa SaaS application, you can set up a rule tooadd users who have a job title of "Sales Rep."</span></span> <span data-ttu-id="4816f-148">Toto pravidlo pak udělují přístup toothis SaaS aplikace tooall uživatelů s touto funkcí ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="4816f-148">This rule then grants access toothis SaaS application tooall users with that job title in your directory.</span></span>

<span data-ttu-id="4816f-149">Pokud se vyhodnotí jako hello systému atributy změnu uživatele, všechna pravidla dynamické skupiny v adresáři toosee Pokud změnu atributu hello hello uživatele by spustit žádnou skupinu přidá nebo odebere.</span><span class="sxs-lookup"><span data-stu-id="4816f-149">When any attributes of a user change, hello system evaluates all dynamic group rules in a directory toosee if hello attribute change of hello user would trigger any group adds or removes.</span></span> <span data-ttu-id="4816f-150">Pokud uživatel splňuje pravidlo ve skupině, přidají se jako člen skupiny toothat.</span><span class="sxs-lookup"><span data-stu-id="4816f-150">If a user satisfies a rule on a group, they are added as a member toothat group.</span></span> <span data-ttu-id="4816f-151">Pokud už vyhovují hello pravidlo, které jsou členy skupiny, budou odstraněny jako členy z této skupiny.</span><span class="sxs-lookup"><span data-stu-id="4816f-151">If they no longer satisfy hello rule of a group they are a member of, they are removed as a members from that group.</span></span>

> [!NOTE]
> <span data-ttu-id="4816f-152">Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365.</span><span class="sxs-lookup"><span data-stu-id="4816f-152">You can set up a rule for dynamic membership on security groups or Office 365 groups.</span></span> <span data-ttu-id="4816f-153">Vnořené členství ve skupinách nejsou aktuálně podporovány pro přiřazení na základě skupiny tooapplications.</span><span class="sxs-lookup"><span data-stu-id="4816f-153">Nested group memberships aren't currently supported for group-based assignment tooapplications.</span></span>
>
> <span data-ttu-id="4816f-154">Dynamické členství ve skupinách vyžaduje toobe licence Azure AD Premium přiřazené</span><span class="sxs-lookup"><span data-stu-id="4816f-154">Dynamic memberships for groups require an Azure AD Premium license toobe assigned to</span></span>
>
> * <span data-ttu-id="4816f-155">Hello správce, který spravuje hello pravidlo pro skupinu</span><span class="sxs-lookup"><span data-stu-id="4816f-155">hello administrator who manages hello rule on a group</span></span>
> * <span data-ttu-id="4816f-156">Všichni členové skupiny hello</span><span class="sxs-lookup"><span data-stu-id="4816f-156">All members of hello group</span></span>
>
>

<span data-ttu-id="4816f-157">**tooenable dynamické členství ve skupině**</span><span class="sxs-lookup"><span data-stu-id="4816f-157">**tooenable dynamic membership for a group**</span></span>

1. <span data-ttu-id="4816f-158">V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak vyberte název hello hello adresáře pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="4816f-158">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="4816f-159">Vyberte hello **skupiny** kartě a chcete tooedit skupiny otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="4816f-159">Select hello **Groups** tab, and open hello group you want tooedit.</span></span>
3. <span data-ttu-id="4816f-160">Vyberte hello **konfigurace** kartě a poté nastavte **povolit dynamické členství** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="4816f-160">Select hello **Configure** tab, and then set **Enable Dynamic Memberships** too**Yes**.</span></span>
4. <span data-ttu-id="4816f-161">Nastavte jedno jednoduché pravidlo pro skupinu toocontrol hello dynamické členství pro funkce v této skupině.</span><span class="sxs-lookup"><span data-stu-id="4816f-161">Set up a simple single rule for hello group toocontrol how dynamic membership for this group functions.</span></span> <span data-ttu-id="4816f-162">Ujistěte se, zda text hello **přidat uživatele kde** možnost je vybrána a potom vyberte vlastnost uživatele ze seznamu hello (například. oddělení, pracovní funkce atd.),</span><span class="sxs-lookup"><span data-stu-id="4816f-162">Make sure hello **Add users where** option is selected, and then select a user property from hello list (for example, department, jobTitle, etc.),</span></span>
5. <span data-ttu-id="4816f-163">V dalším kroku vyberte podmínku (nerovná se, rovná se, nezačíná, začíná, neobsahuje, obsahuje, neodpovídá, odpovídá).</span><span class="sxs-lookup"><span data-stu-id="4816f-163">Next, select a condition (Not Equals, Equals, Not Starts With, Starts With, Not Contains, Contains, Not Match, Match).</span></span>
6. <span data-ttu-id="4816f-164">Zadejte hodnotu porovnání pro vlastnost hello vybrané uživatele.</span><span class="sxs-lookup"><span data-stu-id="4816f-164">Specify a comparison value for hello selected user property.</span></span>

<span data-ttu-id="4816f-165">toolearn o tom, toocreate *rozšířené* pravidel (která může obsahovat několik porovnání) pro dynamické členství ve skupině, najdete v části [pravidel pomocí atributů toocreate rozšířené](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="4816f-165">toolearn about how toocreate *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes toocreate advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

## <a name="additional-information"></a><span data-ttu-id="4816f-166">Další informace</span><span class="sxs-lookup"><span data-stu-id="4816f-166">Additional information</span></span>
<span data-ttu-id="4816f-167">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4816f-167">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="4816f-168">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4816f-168">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="4816f-169">Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="4816f-169">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="4816f-170">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4816f-170">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="4816f-171">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4816f-171">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="4816f-172">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4816f-172">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
