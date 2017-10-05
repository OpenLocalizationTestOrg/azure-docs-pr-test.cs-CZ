---
title: "Náhled vypršení platnosti skupiny Office 365 ve službě Azure Active Directory | Microsoft Docs"
description: "Jak nastavit vypršení platnosti pro skupiny Office 365 ve službě Azure Active Directory (preview)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 8a43df84fd050d7b4bd8d937b8c55e744cb805d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a><span data-ttu-id="4df8c-103">Nakonfigurovat vypršení platnosti skupiny Office 365 (preview)</span><span class="sxs-lookup"><span data-stu-id="4df8c-103">Configure Office 365 groups expiration (preview)</span></span>

<span data-ttu-id="4df8c-104">Teď můžete spravovat životní cyklus skupiny Office 365 nastavením vypršení platnosti pro všechny skupiny Office 365, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="4df8c-104">You can now manage the lifecycle of Office 365 groups by setting expiration for any Office 365 groups that you select.</span></span> <span data-ttu-id="4df8c-105">Po této vypršení platnosti je nastaveno, vlastníků těchto skupin vyzváni k obnovení jejich skupiny, pokud je stále nutné jejich skupiny.</span><span class="sxs-lookup"><span data-stu-id="4df8c-105">Once this expiration is set, owners of those groups are asked to renew their groups if they still need the groups.</span></span> <span data-ttu-id="4df8c-106">Odstraní se všechny skupiny Office 365, který není obnoven.</span><span class="sxs-lookup"><span data-stu-id="4df8c-106">Any Office 365 group that is not renewed will be deleted.</span></span> <span data-ttu-id="4df8c-107">Všechny skupiny Office 365, který byl odstraněn lze obnovit do 30 dní vlastníků skupiny nebo správce.</span><span class="sxs-lookup"><span data-stu-id="4df8c-107">Any Office 365 group that was deleted can be restored within 30 days by the group owners or the administrator.</span></span>  


> [!NOTE]
> <span data-ttu-id="4df8c-108">Můžete nastavit dobu platnosti pro pouze skupiny Office 365.</span><span class="sxs-lookup"><span data-stu-id="4df8c-108">You can set expiration for only Office 365 groups.</span></span>
>
> <span data-ttu-id="4df8c-109">Nastavení vypršení platnosti pro skupiny O365 vyžaduje, aby se k přiřazenou licenci Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="4df8c-109">Setting expiration for O365 groups requires that an Azure AD Premium license is assigned to</span></span>
>   - <span data-ttu-id="4df8c-110">Správce, který konfiguruje nastavení vypršení platnosti pro klienta</span><span class="sxs-lookup"><span data-stu-id="4df8c-110">The administrator who configures the expiration settings for the tenant</span></span>
>   - <span data-ttu-id="4df8c-111">Všichni členové skupiny vybrané pro toto nastavení</span><span class="sxs-lookup"><span data-stu-id="4df8c-111">All members of the groups selected for this setting</span></span>

## <a name="set-office-365-groups-expiration"></a><span data-ttu-id="4df8c-112">Nastavit dobu platnosti skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="4df8c-112">Set Office 365 groups expiration</span></span>

1. <span data-ttu-id="4df8c-113">Otevřete [centra pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4df8c-113">Open the [Azure AD admin center](https://aad.portal.azure.com) with an account that is a global administrator in your Azure AD tenant.</span></span>

2. <span data-ttu-id="4df8c-114">Otevřete Azure AD, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4df8c-114">Open Azure AD, select **Users and groups**.</span></span>

3. <span data-ttu-id="4df8c-115">Vyberte **nastavení skupiny** a pak vyberte **vypršení platnosti** otevře se nastavení vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="4df8c-115">Select **Group settings** and then select **Expiration** to open the expiration settings.</span></span>
  
  ![Okno vypršení platnosti](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. <span data-ttu-id="4df8c-117">Na **vypršení platnosti** okno, můžete:</span><span class="sxs-lookup"><span data-stu-id="4df8c-117">On the **Expiration** blade, you can:</span></span>

  * <span data-ttu-id="4df8c-118">Nastavení doby platnosti skupiny ve dnech.</span><span class="sxs-lookup"><span data-stu-id="4df8c-118">Set the group lifetime in days.</span></span> <span data-ttu-id="4df8c-119">Můžete třeba vybrat jednu z přednastavení hodnoty, nebo vlastní hodnota (by měl být 31 dnů nebo déle).</span><span class="sxs-lookup"><span data-stu-id="4df8c-119">You could select one of the preset values, or a custom value (should be 31 days or more).</span></span> 
  * <span data-ttu-id="4df8c-120">Zadejte e-mailovou adresu, kde odeslat oznámení vypršení platnosti a obnovení v případě, že skupina má bez vlastníka.</span><span class="sxs-lookup"><span data-stu-id="4df8c-120">Specify an email address where the renewal and expiration notifications should be sent when a group has no owner.</span></span> 
  * <span data-ttu-id="4df8c-121">Vyberte skupiny Office 365, které vyprší.</span><span class="sxs-lookup"><span data-stu-id="4df8c-121">Select which Office 365 groups expire.</span></span> <span data-ttu-id="4df8c-122">Můžete povolit vypršení platnosti pro **všechny** skupiny Office 365, můžete vybrat z skupiny Office 365, nebo můžete vybrat **žádné** zakázat vypršení platnosti pro všechny skupiny.</span><span class="sxs-lookup"><span data-stu-id="4df8c-122">You can enable expiration for **All** Office 365 groups, you can select from among the Office 365 groups, or you select **None** to disable expiration for all groups.</span></span>
  * <span data-ttu-id="4df8c-123">Uložit nastavení, když jste hotovi výběrem **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4df8c-123">Save your settings when you're done by selecting **Save**.</span></span>

<span data-ttu-id="4df8c-124">Pokyny o tom, jak stáhnout a nainstalovat modul Microsoft PowerShell nakonfigurovat vypršení platnosti pro skupiny Office 365 pomocí prostředí PowerShell najdete v tématu [Azure modulu Active Directory V2 PowerShell - veřejné verze Preview 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span><span class="sxs-lookup"><span data-stu-id="4df8c-124">For instructions on how to download and install the Microsoft PowerShell module to configure expiration for Office 365 groups via PowerShell, see [Azure Active Directory V2 PowerShell Module - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span></span>

<span data-ttu-id="4df8c-125">E-mailová oznámení, jako je tato jsou odesílány vlastníků skupiny Office 365 30 dní, 15 dní a 1 den před vypršením platnosti skupiny.</span><span class="sxs-lookup"><span data-stu-id="4df8c-125">Email notifications such as this one are sent to the Office 365 group owners 30 days, 15 days, and 1 day prior to expiration of the group.</span></span>

![Vypršení platnosti e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

<span data-ttu-id="4df8c-127">Vlastník skupiny pak můžete vybrat **obnovit skupinu** obnovit Office 365 skupinu, nebo můžete vybrat **přejděte do skupiny** zobrazíte členů a další podrobnosti o této skupině.</span><span class="sxs-lookup"><span data-stu-id="4df8c-127">The group owner can then select **Renew group** to renew the Office 365 group, or can select **Go to group** to see the members and other details about the group.</span></span>

<span data-ttu-id="4df8c-128">Když vyprší platnost skupinu, je skupinu odstranit jeden den po datu vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="4df8c-128">When a group expires, the group is deleted one day after the expiration date.</span></span> <span data-ttu-id="4df8c-129">E-mailové oznámení, jako je tato posílá informace o vypršení platnosti a následné mazání jejich skupiny Office 365 vlastníci skupiny Office 365.</span><span class="sxs-lookup"><span data-stu-id="4df8c-129">An email notification such as this one is sent to the Office 365 group owners informing them about the expiration and subsequent deletion of their Office 365 group.</span></span>

![Skupiny odstranění e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

<span data-ttu-id="4df8c-131">Skupinu lze obnovit tak, že vyberete **obnovení skupiny** nebo pomocí rutin prostředí PowerShell, jak je popsáno v [obnovení odstraněné Office 365 skupiny ve službě Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="4df8c-131">The group can be restored by selecting **Restore group** or by using PowerShell cmdlets, as described in [Restore a deleted Office 365 group in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).</span></span>
    
<span data-ttu-id="4df8c-132">Pokud na skupinu, kterou jste obnovení obsahuje dokumenty, weby Sharepointu nebo jiné trvalé objekty, může trvat až 24 hodin plně obnovit skupiny a její obsah.</span><span class="sxs-lookup"><span data-stu-id="4df8c-132">If the group you're restoring contains documents, SharePoint sites, or other persistent objects, it might take up to 24 hours to fully restore the group and its contents.</span></span>

> [!NOTE]
> * <span data-ttu-id="4df8c-133">Pokud nasazujete nastavení vypršení platnosti, může být některé skupiny, které jsou starší než období platnosti.</span><span class="sxs-lookup"><span data-stu-id="4df8c-133">When deploying the expiration settings, there might be some groups that are older than the expiration window.</span></span> <span data-ttu-id="4df8c-134">Tyto skupiny nebudou odstraněny okamžitě, ale jsou nastaveny na 30 dní do vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="4df8c-134">These groups are not be immediately deleted, but are set to 30 days until expiration.</span></span> <span data-ttu-id="4df8c-135">První e-mail s oznámením obnovení odeslání během dne.</span><span class="sxs-lookup"><span data-stu-id="4df8c-135">The first renewal notification email is sent out within a day.</span></span> <span data-ttu-id="4df8c-136">Například skupiny A byl vytvořen před 400 dny a interval vypršení platnosti je nastaven na 180 dní.</span><span class="sxs-lookup"><span data-stu-id="4df8c-136">For example, Group A was created 400 days ago, and the expiration interval is set to 180 days.</span></span> <span data-ttu-id="4df8c-137">Při aplikování nastavení vypršení platnosti, skupiny A má 30 dní, než je odstraní, pokud vlastník obnovuje ho.</span><span class="sxs-lookup"><span data-stu-id="4df8c-137">When you apply expiration settings, Group A has 30 days before it is deleted, unless the owner renews it.</span></span>
> * <span data-ttu-id="4df8c-138">Když dynamická skupina je odstranit a obnovit, je považovat za novou skupinu a znovu vyplněna podle pravidla.</span><span class="sxs-lookup"><span data-stu-id="4df8c-138">When a dynamic group is deleted and restored, it is seen as a new group and re-populated according to the rule.</span></span> <span data-ttu-id="4df8c-139">Tento proces může trvat až 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="4df8c-139">This process might take up to 24 hours.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4df8c-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4df8c-140">Next steps</span></span>
<span data-ttu-id="4df8c-141">Tyto články poskytují další informace o skupinách Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4df8c-141">These articles provide additional information on Azure AD groups.</span></span>

* [<span data-ttu-id="4df8c-142">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="4df8c-142">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="4df8c-143">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="4df8c-143">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="4df8c-144">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="4df8c-144">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="4df8c-145">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="4df8c-145">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="4df8c-146">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="4df8c-146">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
