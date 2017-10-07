---
title: "Opětovným spuštěním Průvodce instalací hello Azure AD Connect | Microsoft Docs"
description: "Vysvětluje, jak Průvodce instalací hello funguje hello druhý čas spuštění."
keywords: "Průvodce instalací Hello Azure AD Connect vám umožní nakonfigurovat nastavení hello údržby druhý čas spuštění"
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
ms.openlocfilehash: 83cc74aca471ef9b4f65f7f3582e3e48d3d81cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-running-hello-installation-wizard-a-second-time"></a><span data-ttu-id="dc000-104">Synchronizace Azure AD Connect: spuštění Průvodce instalací hello podruhé</span><span class="sxs-lookup"><span data-stu-id="dc000-104">Azure AD Connect sync: Running hello installation wizard a second time</span></span>
<span data-ttu-id="dc000-105">Hello poprvé spustíte Průvodce instalací hello Azure AD Connect, provede vás procesem tooconfigure instalace.</span><span class="sxs-lookup"><span data-stu-id="dc000-105">hello first time you run hello Azure AD Connect installation wizard, it walks you through how tooconfigure your installation.</span></span> <span data-ttu-id="dc000-106">Pokud znovu spustíte Průvodce instalací hello, nabízí možnosti pro údržbu.</span><span class="sxs-lookup"><span data-stu-id="dc000-106">If you run hello installation wizard again, it offers options for maintenance.</span></span>

<span data-ttu-id="dc000-107">Průvodce instalací hello můžete najít v nabídce start hello s názvem **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="dc000-107">You can find hello installation wizard in hello start menu named **Azure AD Connect**.</span></span>

![Nabídka Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

<span data-ttu-id="dc000-109">Když spustíte Průvodce instalací hello, zobrazí se stránka pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="dc000-109">When you start hello installation wizard, you see a page with these options:</span></span>

![Stránka s seznam dalších úloh](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

<span data-ttu-id="dc000-111">Pokud jste nainstalovali služby AD FS službou Azure AD Connect, máte i další možnosti.</span><span class="sxs-lookup"><span data-stu-id="dc000-111">If you have installed ADFS with Azure AD Connect, you have even more options.</span></span> <span data-ttu-id="dc000-112">Další možnosti, které máte služby AD FS jsou dokumentovány v článku Hello [správu služby AD FS](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="dc000-112">hello additional options you have for ADFS are documented in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span></span>

<span data-ttu-id="dc000-113">Vyberte jednu z úloh hello a klikněte na tlačítko **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="dc000-113">Select one of hello tasks and click **Next** toocontinue.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc000-114">Při otevření Průvodce instalací hello, jsou všechny operace v hello synchronizační modul pozastavit.</span><span class="sxs-lookup"><span data-stu-id="dc000-114">While you have hello installation wizard open, all operations in hello sync engine are suspended.</span></span> <span data-ttu-id="dc000-115">Ujistěte se, že zavřete průvodce instalací hello Jakmile dokončíte změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dc000-115">Make sure you close hello installation wizard as soon as you have completed your configuration changes.</span></span>
>
>

## <a name="view-current-configuration"></a><span data-ttu-id="dc000-116">Aktuální konfigurace zobrazení</span><span class="sxs-lookup"><span data-stu-id="dc000-116">View current configuration</span></span>
<span data-ttu-id="dc000-117">Tato možnost poskytuje rychlý přehled o aktuálně nakonfigurované možnosti.</span><span class="sxs-lookup"><span data-stu-id="dc000-117">This option gives you a quick view of your currently configured options.</span></span>

![Stránka se seznam všech možností a jejich stavu](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

<span data-ttu-id="dc000-119">Klikněte na tlačítko **předchozí** toogo zpět.</span><span class="sxs-lookup"><span data-stu-id="dc000-119">Click **Previous** toogo back.</span></span> <span data-ttu-id="dc000-120">Pokud vyberete **ukončení**, zavřete průvodce instalací hello.</span><span class="sxs-lookup"><span data-stu-id="dc000-120">If you select **Exit**, you close hello installation wizard.</span></span>

## <a name="customize-synchronization-options"></a><span data-ttu-id="dc000-121">Přizpůsobit možnosti synchronizace</span><span class="sxs-lookup"><span data-stu-id="dc000-121">Customize synchronization options</span></span>
<span data-ttu-id="dc000-122">Tato možnost je použít toomake změny toohello synchronizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dc000-122">This option is used toomake changes toohello sync configuration.</span></span> <span data-ttu-id="dc000-123">Zobrazí podmnožinu možnosti z cesty instalace hello vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dc000-123">You see a subset of options from hello custom configuration installation path.</span></span> <span data-ttu-id="dc000-124">Tato možnost zobrazí i v případě začátku použít rychlé instalace.</span><span class="sxs-lookup"><span data-stu-id="dc000-124">You see this option even if you used express installation initially.</span></span>

* <span data-ttu-id="dc000-125">[Další adresáře přidat](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span><span class="sxs-lookup"><span data-stu-id="dc000-125">[Add more directories](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span></span> <span data-ttu-id="dc000-126">Odebírání adresáře, najdete v části [odstranit konektor](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span><span class="sxs-lookup"><span data-stu-id="dc000-126">For removing a directory, see [Delete a Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span></span>
* <span data-ttu-id="dc000-127">[Změnit doménu a organizační jednotku filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span><span class="sxs-lookup"><span data-stu-id="dc000-127">[Change Domain and OU filtering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span></span>
* <span data-ttu-id="dc000-128">Odstranit skupinu filtrování.</span><span class="sxs-lookup"><span data-stu-id="dc000-128">Remove Group filtering.</span></span>
* <span data-ttu-id="dc000-129">[Změnit volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features).</span><span class="sxs-lookup"><span data-stu-id="dc000-129">[Change optional features](active-directory-aadconnect-get-started-custom.md#optional-features).</span></span>

<span data-ttu-id="dc000-130">Hello další možnosti z hello počáteční instalaci nelze změnit a nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="dc000-130">hello other options from hello initial installation cannot be changed and are not available.</span></span> <span data-ttu-id="dc000-131">Tyto možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="dc000-131">These options are:</span></span>

* <span data-ttu-id="dc000-132">Změnit hello toouse atribut userPrincipalName a sourceAnchor.</span><span class="sxs-lookup"><span data-stu-id="dc000-132">Change hello attribute toouse for userPrincipalName and sourceAnchor.</span></span>
* <span data-ttu-id="dc000-133">Změnit hello propojení metody pro objekty z jiné doménové struktuře.</span><span class="sxs-lookup"><span data-stu-id="dc000-133">Change hello joining method for objects from different forest.</span></span>
* <span data-ttu-id="dc000-134">Povolte filtrování podle skupiny.</span><span class="sxs-lookup"><span data-stu-id="dc000-134">Enable group-based filtering.</span></span>

## <a name="refresh-directory-schema"></a><span data-ttu-id="dc000-135">Aktualizace schématu adresáře</span><span class="sxs-lookup"><span data-stu-id="dc000-135">Refresh directory schema</span></span>
<span data-ttu-id="dc000-136">Tato možnost se používá, pokud jste změnili hello schéma v jednom z místními doménovými strukturami služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="dc000-136">This option is used if you have changed hello schema in one of your on-premises AD DS forests.</span></span> <span data-ttu-id="dc000-137">Například by mohl mít nainstalován Exchange nebo upgradovat schématu tooa systému Windows Server 2012 s objekty zařízení.</span><span class="sxs-lookup"><span data-stu-id="dc000-137">For example, you might have installed Exchange or upgraded tooa Windows Server 2012 schema with device objects.</span></span> <span data-ttu-id="dc000-138">V takovém případě budete potřebovat tooinstruct Azure AD Connect tooread hello schéma znovu ze služby AD DS a aktualizovat své mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc000-138">In this case, you need tooinstruct Azure AD Connect tooread hello schema again from AD DS and update its cache.</span></span> <span data-ttu-id="dc000-139">Tato akce také regeneruje hello synchronizačního pravidla.</span><span class="sxs-lookup"><span data-stu-id="dc000-139">This action also regenerates hello Sync Rules.</span></span> <span data-ttu-id="dc000-140">Pokud přidáte hello Exchange schématu, jako příklad, přidána hello synchronizační pravidla pro Exchange toohello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dc000-140">If you add hello Exchange schema, as an example, hello Sync Rules for Exchange are added toohello configuration.</span></span>

<span data-ttu-id="dc000-141">Když vyberete tuto možnost, jsou uvedeny všechny adresáře hello ve vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="dc000-141">When you select this option, all hello directories in your configuration are listed.</span></span> <span data-ttu-id="dc000-142">Můžete ponechat výchozí nastavení hello a aktualizovat všechny doménové struktury nebo zrušit výběr některé z nich.</span><span class="sxs-lookup"><span data-stu-id="dc000-142">You can keep hello default setting and refresh all forests or unselect some of them.</span></span>

![Stránka s seznamu všech adresářů v prostředí hello](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a><span data-ttu-id="dc000-144">Konfigurovat pracovní režim</span><span class="sxs-lookup"><span data-stu-id="dc000-144">Configure staging mode</span></span>
<span data-ttu-id="dc000-145">Tato možnost umožňuje tooenable a vypněte pracovní režim na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="dc000-145">This option allows you tooenable and disable staging mode on hello server.</span></span> <span data-ttu-id="dc000-146">Další informace o pracovním režimu a způsobu jejich použití naleznete v [operace](active-directory-aadconnectsync-operations.md#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="dc000-146">More information about staging mode and how it is used can be found in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span></span>

<span data-ttu-id="dc000-147">Hello možnost zobrazí, pokud pracovní aktuálně povolená nebo zakázaná:</span><span class="sxs-lookup"><span data-stu-id="dc000-147">hello option shows if staging is currently enabled or disabled:</span></span>  
![Možnost, která se také zobrazuje aktuální stav hello pracovní režim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

<span data-ttu-id="dc000-149">toochange hello stavu, vyberte tuto možnost a vyberte nebo zrušit výběr hello zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="dc000-149">toochange hello state, select this option and select or unselect hello checkbox.</span></span>  
![Možnost, která se také zobrazuje aktuální stav hello pracovní režim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a><span data-ttu-id="dc000-151">Změnit přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="dc000-151">Change user sign-in</span></span>
<span data-ttu-id="dc000-152">Tato možnost umožňuje toochange z toofederation synchronizace hesel nebo hello opačným způsobem.</span><span class="sxs-lookup"><span data-stu-id="dc000-152">This option allows you toochange from password sync toofederation or hello other way around.</span></span> <span data-ttu-id="dc000-153">Nelze změnit, příliš**nekonfigurujte**.</span><span class="sxs-lookup"><span data-stu-id="dc000-153">You cannot change too**do not configure**.</span></span>

<span data-ttu-id="dc000-154">Další informace o této možnosti najdete v tématu [přihlášení uživatele](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span><span class="sxs-lookup"><span data-stu-id="dc000-154">For more information on this option, see [user sign-in](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc000-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc000-155">Next steps</span></span>
* <span data-ttu-id="dc000-156">Další informace o hello konfigurační model používá synchronizaci Azure AD Connect v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="dc000-156">Learn more about hello configuration model used by Azure AD Connect sync in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>

<span data-ttu-id="dc000-157">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="dc000-157">**Overview topics**</span></span>

* [<span data-ttu-id="dc000-158">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="dc000-158">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="dc000-159">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc000-159">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
