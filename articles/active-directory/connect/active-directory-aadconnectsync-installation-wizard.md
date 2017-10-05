---
title: "Znovu spustit Azure AD Connect Průvodce instalací | Microsoft Docs"
description: "Vysvětluje, jak funguje Průvodce instalací druhá, když jej spustíte."
keywords: "Průvodce instalací služby Azure AD Connect vám umožní nakonfigurovat nastavení údržby druhý čas spuštění"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 42855b785c0ab334e33a622c8db912ce2438c627
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a><span data-ttu-id="47802-104">Synchronizace Azure AD Connect: opětovným spuštěním Průvodce instalací</span><span class="sxs-lookup"><span data-stu-id="47802-104">Azure AD Connect sync: Running the installation wizard a second time</span></span>
<span data-ttu-id="47802-105">Při prvním spuštění Průvodce instalací služby Azure AD Connect, se vás provede procesem konfigurace instalace.</span><span class="sxs-lookup"><span data-stu-id="47802-105">The first time you run the Azure AD Connect installation wizard, it walks you through how to configure your installation.</span></span> <span data-ttu-id="47802-106">Pokud znovu spustíte Průvodce instalací, nabízí možnosti pro údržbu.</span><span class="sxs-lookup"><span data-stu-id="47802-106">If you run the installation wizard again, it offers options for maintenance.</span></span>

<span data-ttu-id="47802-107">Průvodce instalací můžete najít v nabídce start s názvem **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="47802-107">You can find the installation wizard in the start menu named **Azure AD Connect**.</span></span>

![Nabídka Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

<span data-ttu-id="47802-109">Když spustíte Průvodce instalací, zobrazí se stránka pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="47802-109">When you start the installation wizard, you see a page with these options:</span></span>

![Stránka s seznam dalších úloh](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

<span data-ttu-id="47802-111">Pokud jste nainstalovali služby AD FS službou Azure AD Connect, máte i další možnosti.</span><span class="sxs-lookup"><span data-stu-id="47802-111">If you have installed ADFS with Azure AD Connect, you have even more options.</span></span> <span data-ttu-id="47802-112">Další možnosti máte služby AD FS jsou dokumentovány v článku [správu služby AD FS](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="47802-112">The additional options you have for ADFS are documented in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span></span>

<span data-ttu-id="47802-113">Vyberte jednu z úloh a klikněte na **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="47802-113">Select one of the tasks and click **Next** to continue.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="47802-114">Při otevření Průvodce instalací, jsou všechny operace v synchronizační modul pozastavit.</span><span class="sxs-lookup"><span data-stu-id="47802-114">While you have the installation wizard open, all operations in the sync engine are suspended.</span></span> <span data-ttu-id="47802-115">Ujistěte se, že jakmile dokončíte změny konfigurace se zavřete průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="47802-115">Make sure you close the installation wizard as soon as you have completed your configuration changes.</span></span>
>
>

## <a name="view-current-configuration"></a><span data-ttu-id="47802-116">Aktuální konfigurace zobrazení</span><span class="sxs-lookup"><span data-stu-id="47802-116">View current configuration</span></span>
<span data-ttu-id="47802-117">Tato možnost poskytuje rychlý přehled o aktuálně nakonfigurované možnosti.</span><span class="sxs-lookup"><span data-stu-id="47802-117">This option gives you a quick view of your currently configured options.</span></span>

![Stránka se seznam všech možností a jejich stavu](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

<span data-ttu-id="47802-119">Klikněte na tlačítko **předchozí** a vraťte se.</span><span class="sxs-lookup"><span data-stu-id="47802-119">Click **Previous** to go back.</span></span> <span data-ttu-id="47802-120">Pokud vyberete **ukončení**, zavřete průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="47802-120">If you select **Exit**, you close the installation wizard.</span></span>

## <a name="customize-synchronization-options"></a><span data-ttu-id="47802-121">Přizpůsobit možnosti synchronizace</span><span class="sxs-lookup"><span data-stu-id="47802-121">Customize synchronization options</span></span>
<span data-ttu-id="47802-122">Tato možnost slouží k provádět změny konfigurace synchronizace.</span><span class="sxs-lookup"><span data-stu-id="47802-122">This option is used to make changes to the sync configuration.</span></span> <span data-ttu-id="47802-123">Zobrazí podmnožinu možnosti z cesty instalace vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="47802-123">You see a subset of options from the custom configuration installation path.</span></span> <span data-ttu-id="47802-124">Tato možnost zobrazí i v případě začátku použít rychlé instalace.</span><span class="sxs-lookup"><span data-stu-id="47802-124">You see this option even if you used express installation initially.</span></span>

* <span data-ttu-id="47802-125">[Další adresáře přidat](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span><span class="sxs-lookup"><span data-stu-id="47802-125">[Add more directories](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span></span> <span data-ttu-id="47802-126">Odebírání adresáře, najdete v části [odstranit konektor](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span><span class="sxs-lookup"><span data-stu-id="47802-126">For removing a directory, see [Delete a Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span></span>
* <span data-ttu-id="47802-127">[Změnit doménu a organizační jednotku filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span><span class="sxs-lookup"><span data-stu-id="47802-127">[Change Domain and OU filtering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span></span>
* <span data-ttu-id="47802-128">Odstranit skupinu filtrování.</span><span class="sxs-lookup"><span data-stu-id="47802-128">Remove Group filtering.</span></span>
* <span data-ttu-id="47802-129">[Změnit volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features).</span><span class="sxs-lookup"><span data-stu-id="47802-129">[Change optional features](active-directory-aadconnect-get-started-custom.md#optional-features).</span></span>

<span data-ttu-id="47802-130">Další možnosti z počáteční instalaci nelze změnit a nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="47802-130">The other options from the initial installation cannot be changed and are not available.</span></span> <span data-ttu-id="47802-131">Tyto možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="47802-131">These options are:</span></span>

* <span data-ttu-id="47802-132">Změňte atribut, který chcete použít pro userPrincipalName a sourceAnchor.</span><span class="sxs-lookup"><span data-stu-id="47802-132">Change the attribute to use for userPrincipalName and sourceAnchor.</span></span>
* <span data-ttu-id="47802-133">Změňte metodu spojující pro objekty z jiné doménové struktuře.</span><span class="sxs-lookup"><span data-stu-id="47802-133">Change the joining method for objects from different forest.</span></span>
* <span data-ttu-id="47802-134">Povolte filtrování podle skupiny.</span><span class="sxs-lookup"><span data-stu-id="47802-134">Enable group-based filtering.</span></span>

## <a name="refresh-directory-schema"></a><span data-ttu-id="47802-135">Aktualizace schématu adresáře</span><span class="sxs-lookup"><span data-stu-id="47802-135">Refresh directory schema</span></span>
<span data-ttu-id="47802-136">Tato možnost se používá, pokud jste změnili schéma v jednom z místními doménovými strukturami služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="47802-136">This option is used if you have changed the schema in one of your on-premises AD DS forests.</span></span> <span data-ttu-id="47802-137">Například je může Exchange nainstalovali nebo upgradovali schématu systému Windows Server 2012 s objekty zařízení.</span><span class="sxs-lookup"><span data-stu-id="47802-137">For example, you might have installed Exchange or upgraded to a Windows Server 2012 schema with device objects.</span></span> <span data-ttu-id="47802-138">V takovém případě budete muset pokyn Azure AD Connect číst schéma znovu ze služby AD DS a aktualizovat své mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="47802-138">In this case, you need to instruct Azure AD Connect to read the schema again from AD DS and update its cache.</span></span> <span data-ttu-id="47802-139">Tato akce také regeneruje pravidla synchronizace.</span><span class="sxs-lookup"><span data-stu-id="47802-139">This action also regenerates the Sync Rules.</span></span> <span data-ttu-id="47802-140">Pokud přidáte schéma Exchange jako příklad, pravidla synchronizace pro Exchange se přidají do konfigurace.</span><span class="sxs-lookup"><span data-stu-id="47802-140">If you add the Exchange schema, as an example, the Sync Rules for Exchange are added to the configuration.</span></span>

<span data-ttu-id="47802-141">Když vyberete tuto možnost, jsou uvedeny všechny adresáře ve vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="47802-141">When you select this option, all the directories in your configuration are listed.</span></span> <span data-ttu-id="47802-142">Můžete ponechat výchozí nastavení a aktualizujte všechny doménové struktury nebo zrušení výběru některé z nich.</span><span class="sxs-lookup"><span data-stu-id="47802-142">You can keep the default setting and refresh all forests or unselect some of them.</span></span>

![Stránka s seznamu všech adresářů v prostředí](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a><span data-ttu-id="47802-144">Konfigurovat pracovní režim</span><span class="sxs-lookup"><span data-stu-id="47802-144">Configure staging mode</span></span>
<span data-ttu-id="47802-145">Tato možnost umožňuje povolit nebo zakázat pracovní režim na serveru.</span><span class="sxs-lookup"><span data-stu-id="47802-145">This option allows you to enable and disable staging mode on the server.</span></span> <span data-ttu-id="47802-146">Další informace o pracovním režimu a způsobu jejich použití naleznete v [operace](active-directory-aadconnectsync-operations.md#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="47802-146">More information about staging mode and how it is used can be found in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span></span>

<span data-ttu-id="47802-147">Možnost ukazuje, zda je aktuálně povolena nebo zakázána pracovní:</span><span class="sxs-lookup"><span data-stu-id="47802-147">The option shows if staging is currently enabled or disabled:</span></span>  
![Možnost, která se také zobrazuje aktuální stav pracovní režim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

<span data-ttu-id="47802-149">Na změnu stavu, vyberte tuto možnost a vyberte nebo zrušte výběr zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="47802-149">To change the state, select this option and select or unselect the checkbox.</span></span>  
![Možnost, která se také zobrazuje aktuální stav pracovní režim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a><span data-ttu-id="47802-151">Změnit přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="47802-151">Change user sign-in</span></span>
<span data-ttu-id="47802-152">Tato možnost umožňuje změnit z synchronizaci hesla k federaci nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="47802-152">This option allows you to change from password sync to federation or the other way around.</span></span> <span data-ttu-id="47802-153">Nelze změnit na **nekonfigurujte**.</span><span class="sxs-lookup"><span data-stu-id="47802-153">You cannot change to **do not configure**.</span></span>

<span data-ttu-id="47802-154">Další informace o této možnosti najdete v tématu [přihlášení uživatele](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span><span class="sxs-lookup"><span data-stu-id="47802-154">For more information on this option, see [user sign-in](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47802-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47802-155">Next steps</span></span>
* <span data-ttu-id="47802-156">Další informace o konfiguraci modelu používá synchronizaci Azure AD Connect v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="47802-156">Learn more about the configuration model used by Azure AD Connect sync in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>

<span data-ttu-id="47802-157">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="47802-157">**Overview topics**</span></span>

* [<span data-ttu-id="47802-158">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="47802-158">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="47802-159">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="47802-159">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
