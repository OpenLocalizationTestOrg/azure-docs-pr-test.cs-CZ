---
title: "vypršení platnosti v Azure Active Directory skupiny aaaPreview Office 365 | Microsoft Docs"
description: "Jak tooset do vypršení platnosti pro Office 365 skupin v Azure Active Directory (preview)"
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
ms.openlocfilehash: a8c99961cff3aad3f4d8b0cc1b2eb8e8a4c9ba95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a><span data-ttu-id="5cf12-103">Nakonfigurovat vypršení platnosti skupiny Office 365 (preview)</span><span class="sxs-lookup"><span data-stu-id="5cf12-103">Configure Office 365 groups expiration (preview)</span></span>

<span data-ttu-id="5cf12-104">Teď můžete spravovat životní cyklus hello skupiny Office 365 nastavením vypršení platnosti pro všechny skupiny Office 365, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="5cf12-104">You can now manage hello lifecycle of Office 365 groups by setting expiration for any Office 365 groups that you select.</span></span> <span data-ttu-id="5cf12-105">Po této vypršení platnosti je nastaveno, vlastníků těchto skupin jsou vyzváni toorenew jejich skupiny, pokud je stále nutné hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="5cf12-105">Once this expiration is set, owners of those groups are asked toorenew their groups if they still need hello groups.</span></span> <span data-ttu-id="5cf12-106">Odstraní se všechny skupiny Office 365, který není obnoven.</span><span class="sxs-lookup"><span data-stu-id="5cf12-106">Any Office 365 group that is not renewed will be deleted.</span></span> <span data-ttu-id="5cf12-107">Všechny skupiny Office 365, který byl odstraněn lze obnovit do 30 dní hello skupiny Vlastníci nebo hello správce.</span><span class="sxs-lookup"><span data-stu-id="5cf12-107">Any Office 365 group that was deleted can be restored within 30 days by hello group owners or hello administrator.</span></span>  


> [!NOTE]
> <span data-ttu-id="5cf12-108">Můžete nastavit dobu platnosti pro pouze skupiny Office 365.</span><span class="sxs-lookup"><span data-stu-id="5cf12-108">You can set expiration for only Office 365 groups.</span></span>
>
> <span data-ttu-id="5cf12-109">Nastavení vypršení platnosti pro skupiny O365 vyžaduje, aby se k přiřazenou licenci Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="5cf12-109">Setting expiration for O365 groups requires that an Azure AD Premium license is assigned to</span></span>
>   - <span data-ttu-id="5cf12-110">Hello správce, který konfiguruje nastavení vypršení platnosti hello hello klienta</span><span class="sxs-lookup"><span data-stu-id="5cf12-110">hello administrator who configures hello expiration settings for hello tenant</span></span>
>   - <span data-ttu-id="5cf12-111">Všichni členové skupiny hello vybrané pro toto nastavení</span><span class="sxs-lookup"><span data-stu-id="5cf12-111">All members of hello groups selected for this setting</span></span>

## <a name="set-office-365-groups-expiration"></a><span data-ttu-id="5cf12-112">Nastavit dobu platnosti skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="5cf12-112">Set Office 365 groups expiration</span></span>

1. <span data-ttu-id="5cf12-113">Otevřete hello [centra pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cf12-113">Open hello [Azure AD admin center](https://aad.portal.azure.com) with an account that is a global administrator in your Azure AD tenant.</span></span>

2. <span data-ttu-id="5cf12-114">Otevřete Azure AD, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5cf12-114">Open Azure AD, select **Users and groups**.</span></span>

3. <span data-ttu-id="5cf12-115">Vyberte **nastavení skupiny** a pak vyberte **vypršení platnosti** nastavení vypršení platnosti tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="5cf12-115">Select **Group settings** and then select **Expiration** tooopen hello expiration settings.</span></span>
  
  ![Okno vypršení platnosti](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. <span data-ttu-id="5cf12-117">Na hello **vypršení platnosti** okno, můžete:</span><span class="sxs-lookup"><span data-stu-id="5cf12-117">On hello **Expiration** blade, you can:</span></span>

  * <span data-ttu-id="5cf12-118">Nastavení doby platnosti hello skupiny ve dnech.</span><span class="sxs-lookup"><span data-stu-id="5cf12-118">Set hello group lifetime in days.</span></span> <span data-ttu-id="5cf12-119">Můžete třeba vybrat jednu z hello přednastavení hodnoty, nebo vlastní hodnota (by měl být 31 dnů nebo déle).</span><span class="sxs-lookup"><span data-stu-id="5cf12-119">You could select one of hello preset values, or a custom value (should be 31 days or more).</span></span> 
  * <span data-ttu-id="5cf12-120">Zadejte e-mailovou adresu, kde mají být odeslána oznámení o vypršení platnosti a obnovení hello když vlastník skupiny.</span><span class="sxs-lookup"><span data-stu-id="5cf12-120">Specify an email address where hello renewal and expiration notifications should be sent when a group has no owner.</span></span> 
  * <span data-ttu-id="5cf12-121">Vyberte skupiny Office 365, které vyprší.</span><span class="sxs-lookup"><span data-stu-id="5cf12-121">Select which Office 365 groups expire.</span></span> <span data-ttu-id="5cf12-122">Můžete povolit vypršení platnosti pro **všechny** skupiny Office 365, můžete vybrat některé z těchto skupin hello Office 365, nebo můžete vybrat **žádné** zakázat vypršení platnosti pro všechny skupiny.</span><span class="sxs-lookup"><span data-stu-id="5cf12-122">You can enable expiration for **All** Office 365 groups, you can select from among hello Office 365 groups, or you select **None** to disable expiration for all groups.</span></span>
  * <span data-ttu-id="5cf12-123">Uložit nastavení, když jste hotovi výběrem **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5cf12-123">Save your settings when you're done by selecting **Save**.</span></span>

<span data-ttu-id="5cf12-124">Pokyny jak toodownload a nainstalujte hello Microsoft PowerShell modulu tooconfigure vypršení platnosti pro skupiny Office 365 pomocí prostředí PowerShell, najdete v části [Azure modulu Active Directory V2 PowerShell - veřejné verze Preview 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span><span class="sxs-lookup"><span data-stu-id="5cf12-124">For instructions on how toodownload and install hello Microsoft PowerShell module tooconfigure expiration for Office 365 groups via PowerShell, see [Azure Active Directory V2 PowerShell Module - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span></span>

<span data-ttu-id="5cf12-125">E-mailová oznámení, jako je tato odešlou toohello Office 365 skupiny Vlastníci 30 dní, 15 dní a 1 den předchozí tooexpiration hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="5cf12-125">Email notifications such as this one are sent toohello Office 365 group owners 30 days, 15 days, and 1 day prior tooexpiration of hello group.</span></span>

![Vypršení platnosti e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

<span data-ttu-id="5cf12-127">Vlastník skupiny Hello pak můžete vybrat **obnovit skupinu** toorenew hello skupiny Office 365, nebo můžete vybrat **přejděte toogroup** toosee hello členy a další podrobnosti o hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="5cf12-127">hello group owner can then select **Renew group** toorenew hello Office 365 group, or can select **Go toogroup** toosee hello members and other details about hello group.</span></span>

<span data-ttu-id="5cf12-128">Když vyprší platnost skupinu, odstranění skupiny hello jeden den po vypršení platnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5cf12-128">When a group expires, hello group is deleted one day after hello expiration date.</span></span> <span data-ttu-id="5cf12-129">E-mailové oznámení, jako je tato je odeslaných vlastníků skupiny toohello Office 365, informace o vypršení platnosti hello a následné mazání jejich skupiny Office 365.</span><span class="sxs-lookup"><span data-stu-id="5cf12-129">An email notification such as this one is sent toohello Office 365 group owners informing them about hello expiration and subsequent deletion of their Office 365 group.</span></span>

![Skupiny odstranění e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

<span data-ttu-id="5cf12-131">Hello skupiny může být obnovena výběrem **obnovení skupiny** nebo pomocí rutin prostředí PowerShell, jak je popsáno v [obnovení odstraněné Office 365 skupiny ve službě Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-Restore-Azure-Portal).</span><span class="sxs-lookup"><span data-stu-id="5cf12-131">hello group can be restored by selecting **Restore group** or by using PowerShell cmdlets, as described in [Restore a deleted Office 365 group in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).</span></span>
    
<span data-ttu-id="5cf12-132">Pokud hello skupinu, kterou jste obnovení obsahuje dokumenty, weby Sharepointu nebo jiné trvalé objekty, může to trvat až too24 hodin toofully obnovení hello skupiny a její obsah.</span><span class="sxs-lookup"><span data-stu-id="5cf12-132">If hello group you're restoring contains documents, SharePoint sites, or other persistent objects, it might take up too24 hours toofully restore hello group and its contents.</span></span>

> [!NOTE]
> * <span data-ttu-id="5cf12-133">Pokud nasazujete hello nastavení vypršení platnosti, může být některé skupiny, které jsou starší než období platnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5cf12-133">When deploying hello expiration settings, there might be some groups that are older than hello expiration window.</span></span> <span data-ttu-id="5cf12-134">Tyto skupiny nebudou odstraněny okamžitě, ale jsou nastavit too30 počet dnů do vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="5cf12-134">These groups are not be immediately deleted, but are set too30 days until expiration.</span></span> <span data-ttu-id="5cf12-135">Hello první obnovení e-mailové oznámení odeslání během dne.</span><span class="sxs-lookup"><span data-stu-id="5cf12-135">hello first renewal notification email is sent out within a day.</span></span> <span data-ttu-id="5cf12-136">Například skupiny A byl vytvořen před 400 dny a interval vypršení platnosti hello nastaven too180 dnů.</span><span class="sxs-lookup"><span data-stu-id="5cf12-136">For example, Group A was created 400 days ago, and hello expiration interval is set too180 days.</span></span> <span data-ttu-id="5cf12-137">Při aplikování nastavení vypršení platnosti, skupiny A má 30 dní, než je odstraní, pokud hello vlastníka obnovuje se.</span><span class="sxs-lookup"><span data-stu-id="5cf12-137">When you apply expiration settings, Group A has 30 days before it is deleted, unless hello owner renews it.</span></span>
> * <span data-ttu-id="5cf12-138">Když dynamická skupina je odstranit a obnovit, považuje se za nové skupiny a znovu vyplněná podle toohello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="5cf12-138">When a dynamic group is deleted and restored, it is seen as a new group and re-populated according toohello rule.</span></span> <span data-ttu-id="5cf12-139">Tento proces může trvat až too24 hodin.</span><span class="sxs-lookup"><span data-stu-id="5cf12-139">This process might take up too24 hours.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5cf12-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5cf12-140">Next steps</span></span>
<span data-ttu-id="5cf12-141">Tyto články poskytují další informace o skupinách Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cf12-141">These articles provide additional information on Azure AD groups.</span></span>

* [<span data-ttu-id="5cf12-142">Zobrazení existujících skupin</span><span class="sxs-lookup"><span data-stu-id="5cf12-142">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="5cf12-143">Spravovat nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="5cf12-143">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="5cf12-144">Správa členů skupiny</span><span class="sxs-lookup"><span data-stu-id="5cf12-144">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="5cf12-145">Správa členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="5cf12-145">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="5cf12-146">Dynamická pravidla pro uživatele ve skupině pro správu</span><span class="sxs-lookup"><span data-stu-id="5cf12-146">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
