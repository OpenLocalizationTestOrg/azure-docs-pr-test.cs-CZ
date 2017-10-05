---
title: "Azure AD Connect: Další kroky a postupy pro správu Azure AD Connect | Microsoft Docs"
description: "Zjistěte, jak rozšířit výchozí konfigurace a provozní úlohy pro Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: beace24fa00c85a5038a3c39ae8f76af5fd12111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a><span data-ttu-id="f84be-103">Další kroky a postupy pro správu Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="f84be-103">Next steps and how to manage Azure AD Connect</span></span>
<span data-ttu-id="f84be-104">Chcete-li přizpůsobit Azure Active Directory (Azure AD) připojit ke splnění potřebám a požadavkům vaší organizace pomocí provozních postupů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f84be-104">Use the operational procedures in this article to customize Azure Active Directory (Azure AD) Connect to meet your organization's needs and requirements.</span></span>  

## <a name="add-additional-sync-admins"></a><span data-ttu-id="f84be-105">Přidat další synchronizaci admins</span><span class="sxs-lookup"><span data-stu-id="f84be-105">Add additional sync admins</span></span>
<span data-ttu-id="f84be-106">Ve výchozím nastavení je moct spravovat nainstalované synchronizační modul pouze uživatele, který se instalace a místní správci.</span><span class="sxs-lookup"><span data-stu-id="f84be-106">By default, only the user who did the installation and local admins are able to manage the installed sync engine.</span></span> <span data-ttu-id="f84be-107">Pro další osoby mohly k otvírat a spravovat synchronizační modul vyhledejte skupinu s názvem ADSyncAdmins na místním serveru a přidat je do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="f84be-107">For additional people to be able to access and manage the sync engine, locate the group named ADSyncAdmins on the local server and add them to this group.</span></span>

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a><span data-ttu-id="f84be-108">Přiřazení licencí uživatelům Azure AD Premium a Enterprise Mobility Suite</span><span class="sxs-lookup"><span data-stu-id="f84be-108">Assign licenses to Azure AD Premium and Enterprise Mobility Suite users</span></span>
<span data-ttu-id="f84be-109">Teď, když uživatelé byly synchronizované do cloudu, budete muset přiřadit licenci, můžete začít s cloudové aplikace, jako je například Office 365.</span><span class="sxs-lookup"><span data-stu-id="f84be-109">Now that your users have been synchronized to the cloud, you need to assign them a license so they can get going with cloud apps such as Office 365.</span></span>

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a><span data-ttu-id="f84be-110">Chcete-li přiřadit Azure AD Premium nebo Enterprise Mobility Suite licencí</span><span class="sxs-lookup"><span data-stu-id="f84be-110">To assign an Azure AD Premium or Enterprise Mobility Suite License</span></span>

1. <span data-ttu-id="f84be-111">Přihlaste se k portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="f84be-111">Sign in to the Azure portal as an admin.</span></span>
2. <span data-ttu-id="f84be-112">Vlevo vyberte možnost **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f84be-112">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="f84be-113">Na **služby Active Directory** stránky, dvakrát klikněte na adresář, který obsahuje uživatele, kterou chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="f84be-113">On the **Active Directory** page, double-click the directory that has the users you want to set up.</span></span>
4. <span data-ttu-id="f84be-114">V horní části stránky adresáře vyberte možnost **Licence**.</span><span class="sxs-lookup"><span data-stu-id="f84be-114">At the top of the directory page, select **Licenses**.</span></span>
5. <span data-ttu-id="f84be-115">Na **licence** vyberte **Active Directory Premium** nebo **Enterprise Mobility Suite**a potom klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="f84be-115">On the **Licenses** page, select **Active Directory Premium** or **Enterprise Mobility Suite**, and then click **Assign**.</span></span>
6. <span data-ttu-id="f84be-116">V dialogovém okně vyberte uživatele, kterým chcete přiřadit licence, a potom změny uložte kliknutím na ikonu zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="f84be-116">In the dialog box, select the users you want to assign licenses to, and then click the check mark icon to save the changes.</span></span>

## <a name="verify-the-scheduled-synchronization-task"></a><span data-ttu-id="f84be-117">Ověření úlohy plánované synchronizace</span><span class="sxs-lookup"><span data-stu-id="f84be-117">Verify the scheduled synchronization task</span></span>
<span data-ttu-id="f84be-118">Použití portálu Azure a zkontrolujte stav synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f84be-118">Use the Azure portal to check the status of a synchronization.</span></span>

### <a name="to-verify-the-scheduled-synchronization-task"></a><span data-ttu-id="f84be-119">Ověření úlohy plánované synchronizace</span><span class="sxs-lookup"><span data-stu-id="f84be-119">To verify the scheduled synchronization task</span></span>
1. <span data-ttu-id="f84be-120">Přihlaste se k portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="f84be-120">Sign in to the Azure portal as an admin.</span></span>
2. <span data-ttu-id="f84be-121">Vlevo vyberte možnost **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f84be-121">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="f84be-122">Na **služby Active Directory** stránky, dvakrát klikněte na adresář, který obsahuje uživatele, kterou chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="f84be-122">On the **Active Directory** page, double-click the directory that has the users you want to set up.</span></span>
4. <span data-ttu-id="f84be-123">V horní části stránky adresáře vyberte **integrace adresáře**.</span><span class="sxs-lookup"><span data-stu-id="f84be-123">At the top of the directory page, select **Directory Integration**.</span></span>
5. <span data-ttu-id="f84be-124">V části **integraci s místní službou active directory**, Všimněte si, čas poslední synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f84be-124">Under **integration with local active directory**, note the last sync time.</span></span>

<span data-ttu-id="f84be-125"><center>![Čas synchronizace adresáře](./media/active-directory-aadconnect-whats-next/verify.png)</center></span><span class="sxs-lookup"><span data-stu-id="f84be-125"><center>![Directory sync time](./media/active-directory-aadconnect-whats-next/verify.png)</center></span></span>

## <a name="start-a-scheduled-synchronization-task"></a><span data-ttu-id="f84be-126">Spustit úlohu plánované synchronizace</span><span class="sxs-lookup"><span data-stu-id="f84be-126">Start a scheduled synchronization task</span></span>
<span data-ttu-id="f84be-127">Pokud potřebujete spustit úlohu synchronizace, musíte znovu spuštěním prostřednictvím Průvodce službou Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f84be-127">If you need to run a synchronization task, you can do this by running through the Azure AD Connect wizard again.</span></span>  <span data-ttu-id="f84be-128">Je třeba zadat přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f84be-128">You need to provide your Azure AD credentials.</span></span>  <span data-ttu-id="f84be-129">V průvodci vyberte **přizpůsobit možnosti synchronizace** úloh a klikněte na tlačítko **Další** přesunout pomocí průvodce.</span><span class="sxs-lookup"><span data-stu-id="f84be-129">In the wizard, select the **Customize synchronization options** task, and click **Next** to move through the wizard.</span></span> <span data-ttu-id="f84be-130">Na konci, ujistěte se, že **zahájit proces synchronizace ihned po dokončení prvotní konfigurace** políčka.</span><span class="sxs-lookup"><span data-stu-id="f84be-130">At the end, ensure that the **Start the synchronization process as soon as the initial configuration completes** box is selected.</span></span>

<span data-ttu-id="f84be-131"><center>![Spustit synchronizaci.](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span><span class="sxs-lookup"><span data-stu-id="f84be-131"><center>![Start synchronization](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span></span>

<span data-ttu-id="f84be-132">Další informace o Azure AD Connect sync Scheduler najdete v tématu [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="f84be-132">For more information on the Azure AD Connect sync Scheduler, see [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span>

## <a name="additional-tasks-available-in-azure-ad-connect"></a><span data-ttu-id="f84be-133">Další úlohy, které jsou k dispozici ve službě Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="f84be-133">Additional tasks available in Azure AD Connect</span></span>
<span data-ttu-id="f84be-134">Po počáteční instalace systému služby Azure AD Connect můžete vždy spustíte průvodce znovu z úvodní stránky nebo plochy zástupce Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f84be-134">After your initial installation of Azure AD Connect, you can always start the wizard again from the Azure AD Connect start page or desktop shortcut.</span></span>  <span data-ttu-id="f84be-135">Si všimnete, že znovu projít průvodce nabízí některé nové možnosti ve formě další úkoly.</span><span class="sxs-lookup"><span data-stu-id="f84be-135">You will notice that going through the wizard again provides some new options in the form of additional tasks.</span></span>  

<span data-ttu-id="f84be-136">Následující tabulka obsahuje souhrn tyto úlohy a stručný popis jednotlivých úloh.</span><span class="sxs-lookup"><span data-stu-id="f84be-136">The following table provides a summary of these tasks and a brief description of each task.</span></span>

![Seznam dalších úloh](./media/active-directory-aadconnect-whats-next/addtasks.png)

| <span data-ttu-id="f84be-138">Další úlohy</span><span class="sxs-lookup"><span data-stu-id="f84be-138">Additional task</span></span> | <span data-ttu-id="f84be-139">Popis</span><span class="sxs-lookup"><span data-stu-id="f84be-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f84be-140">**Zobrazení vybraným scénářem**</span><span class="sxs-lookup"><span data-stu-id="f84be-140">**View the selected scenario**</span></span> |<span data-ttu-id="f84be-141">Zobrazte vaše aktuální řešení Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f84be-141">View your current Azure AD Connect solution.</span></span>  <span data-ttu-id="f84be-142">To zahrnuje obecná nastavení, synchronizovat adresáře a nastavení synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f84be-142">This includes general settings, synchronized directories, and sync settings.</span></span> |
| <span data-ttu-id="f84be-143">**Přizpůsobit možnosti synchronizace**</span><span class="sxs-lookup"><span data-stu-id="f84be-143">**Customize synchronization options**</span></span> |<span data-ttu-id="f84be-144">Změňte aktuální konfiguraci jako přidání další doménové struktury služby Active Directory ke konfiguraci nebo povolení možnosti synchronizace, jako je například uživatele, skupiny, zařízení nebo zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="f84be-144">Change the current configuration like adding additional Active Directory forests to the configuration, or enabling sync options such as user, group, device, or password write-back.</span></span> |
| <span data-ttu-id="f84be-145">**Zapnout pracovní režim**</span><span class="sxs-lookup"><span data-stu-id="f84be-145">**Enable Staging Mode**</span></span> |<span data-ttu-id="f84be-146">Fáze informace, které není synchronizována okamžitě a není exportovány do služby Azure AD nebo místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f84be-146">Stage information that is not immediately synchronized and is not exported to Azure AD or on-premises Active Directory.</span></span>  <span data-ttu-id="f84be-147">Pomocí této funkce můžete zobrazit náhled synchronizace předtím, než k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="f84be-147">With this feature, you can preview the synchronizations before they occur.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f84be-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f84be-148">Next steps</span></span>
<span data-ttu-id="f84be-149">Další informace o [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="f84be-149">Learn more about [integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
