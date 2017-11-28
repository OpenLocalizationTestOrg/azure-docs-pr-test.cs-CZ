---
title: 'Azure AD Connect: Vyberte typ instalace | Microsoft Docs'
description: "Toto téma vás provede procesem instalace hello tooselect jak zadejte toouse pro Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a700e59eb05947ee1dbd9993141200c9340b2f1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="select-which-installation-type-toouse-for-azure-ad-connect"></a><span data-ttu-id="e21ac-103">Vyberte typ toouse které instalace Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="e21ac-103">Select which installation type toouse for Azure AD Connect</span></span>
<span data-ttu-id="e21ac-104">Azure AD Connect má dva typy instalace pro novou instalaci: Express a přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="e21ac-104">Azure AD Connect has two installation types for new installation: Express and customized.</span></span> <span data-ttu-id="e21ac-105">Toto téma vám pomůže toodecide, která možnost toouse během instalace.</span><span class="sxs-lookup"><span data-stu-id="e21ac-105">This topic helps you toodecide which option toouse during installation.</span></span>

## <a name="express"></a><span data-ttu-id="e21ac-106">Express</span><span class="sxs-lookup"><span data-stu-id="e21ac-106">Express</span></span>
<span data-ttu-id="e21ac-107">Express je hello nejběžnější možnost a používá přibližně 90 % všechny nové instalace.</span><span class="sxs-lookup"><span data-stu-id="e21ac-107">Express is hello most common option and is used by about 90% of all new installations.</span></span> <span data-ttu-id="e21ac-108">Bylo navrženou tooprovide konfigurace, který je použitelný pro hello nejběžnějších scénářů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e21ac-108">It was designed tooprovide a configuration that works for hello most common customer scenarios.</span></span>

<span data-ttu-id="e21ac-109">Předpokládá:</span><span class="sxs-lookup"><span data-stu-id="e21ac-109">It assumes:</span></span>

- <span data-ttu-id="e21ac-110">Máte jeden Active Directory doménové struktury na místě.</span><span class="sxs-lookup"><span data-stu-id="e21ac-110">You have a single Active Directory forest on-premises.</span></span>
- <span data-ttu-id="e21ac-111">Máte účet správce podnikové sítě, které můžete použít pro instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="e21ac-111">You have an enterprise administrator account you can use for hello installation.</span></span>
- <span data-ttu-id="e21ac-112">Máte méně než 100 000 objektů ve vaší místní službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e21ac-112">You have less than 100,000 objects in your on-premises Active Directory.</span></span>

<span data-ttu-id="e21ac-113">Můžete získat:</span><span class="sxs-lookup"><span data-stu-id="e21ac-113">You get:</span></span>

- <span data-ttu-id="e21ac-114">[Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) z tooAzure místní AD pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e21ac-114">[Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) from on-premises tooAzure AD for single sign-on.</span></span>
- <span data-ttu-id="e21ac-115">Konfigurace, který se synchronizuje [uživatelé, skupiny, kontakty a počítače s Windows 10](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="e21ac-115">A configuration that synchronizes [users, groups, contacts, and Windows 10 computers](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
- <span data-ttu-id="e21ac-116">Synchronizace všech oprávněných objektů ve všech doménách a ve všech organizačních.</span><span class="sxs-lookup"><span data-stu-id="e21ac-116">Synchronization of all eligible objects in all domains and all OUs.</span></span>
- <span data-ttu-id="e21ac-117">[Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) je povoleno toomake se vždy používáte nejnovější dostupnou verzi hello.</span><span class="sxs-lookup"><span data-stu-id="e21ac-117">[Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is enabled toomake sure you always use hello latest available version.</span></span>

<span data-ttu-id="e21ac-118">Možnosti, kde můžete dál používat Express:</span><span class="sxs-lookup"><span data-stu-id="e21ac-118">Options where you can still use Express:</span></span>

- <span data-ttu-id="e21ac-119">Pokud nechcete, aby toosynchronize všechny organizační jednotky, můžete stále používají systém Express a na poslední stránku hello, zrušte výběr **spustit proces synchronizace hello...** *.</span><span class="sxs-lookup"><span data-stu-id="e21ac-119">If you do not want toosynchronize all OUs, you can still use Express and on hello last page, unselect **Start hello synchronization process...***.</span></span> <span data-ttu-id="e21ac-120">Pak znovu spusťte Průvodce instalací hello a změnit hello organizačních jednotek ve [možnosti konfigurace](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) a povolte plánované synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="e21ac-120">Then run hello installation wizard again and change hello OUs in [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) and enable scheduled sync.</span></span>
- <span data-ttu-id="e21ac-121">Chcete tooenable jedna z funkcí hello v Azure AD Premium, jako je například zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="e21ac-121">You want tooenable one of hello features in Azure AD Premium, such as Password writeback.</span></span> <span data-ttu-id="e21ac-122">Nejdřív projít express tooget hello počáteční instalace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="e21ac-122">First go through express tooget hello initial installation completed.</span></span> <span data-ttu-id="e21ac-123">Pak znovu spusťte Průvodce instalací hello a změnit hello [možnosti konfigurace](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span><span class="sxs-lookup"><span data-stu-id="e21ac-123">Then run hello installation wizard again and change hello [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span></span>

## <a name="custom"></a><span data-ttu-id="e21ac-124">Vlastní</span><span class="sxs-lookup"><span data-stu-id="e21ac-124">Custom</span></span>
<span data-ttu-id="e21ac-125">Vlastní cesta Hello umožňuje mnohem víc možností než express.</span><span class="sxs-lookup"><span data-stu-id="e21ac-125">hello customized path allows many more options than express.</span></span> <span data-ttu-id="e21ac-126">By být používána ve všech případech, kdy není hello konfigurace popsané v předchozí části pro expresní reprezentativní pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="e21ac-126">It should be used in all cases where hello configuration described in previous section for express is not representative for your organization.</span></span>

<span data-ttu-id="e21ac-127">Použijte, když:</span><span class="sxs-lookup"><span data-stu-id="e21ac-127">Use when:</span></span>

- <span data-ttu-id="e21ac-128">Nemáte účet správce podnikové tooan přístupu ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e21ac-128">You do not have access tooan enterprise admin account in Active Directory.</span></span>
- <span data-ttu-id="e21ac-129">Máte více než jedné doménové struktuře nebo plánujete toosynchronize více než jednu doménovou strukturu v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="e21ac-129">You have more than one forest or you plan toosynchronize more than one forest in hello future.</span></span>
- <span data-ttu-id="e21ac-130">Máte domén v doménové struktuře není dosažitelný z hello Connect server.</span><span class="sxs-lookup"><span data-stu-id="e21ac-130">You have domains in your forest not reachable from hello Connect server.</span></span>
- <span data-ttu-id="e21ac-131">Máte v plánu toouse federaci nebo předávací ověřování pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="e21ac-131">You plan toouse federation or pass-through authentication for user sign-in.</span></span>
- <span data-ttu-id="e21ac-132">Máte více než 100 000 objektů a potřebujete toouse plnou instalaci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e21ac-132">You have more than 100,000 objects and need toouse a full SQL Server.</span></span>
- <span data-ttu-id="e21ac-133">Máte v plánu toouse na základě skupiny filtrování a nejen domény nebo filtrování založené na organizační jednotku.</span><span class="sxs-lookup"><span data-stu-id="e21ac-133">You plan toouse group-based filtering and not only domain or OU-based filtering.</span></span>

## <a name="upgrade-from-dirsync"></a><span data-ttu-id="e21ac-134">Upgrade z nástroje DirSync</span><span class="sxs-lookup"><span data-stu-id="e21ac-134">Upgrade from DirSync</span></span>
<span data-ttu-id="e21ac-135">Pokud aktuálně používáte DirSync, pak postupujte podle kroků hello v [upgradu z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade vaší stávající konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e21ac-135">If you are currently using DirSync, then follow hello steps in [Upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade your existing configuration.</span></span> <span data-ttu-id="e21ac-136">Existují dva různé možnosti upgradu:</span><span class="sxs-lookup"><span data-stu-id="e21ac-136">There are two different upgrade options:</span></span>

- <span data-ttu-id="e21ac-137">Místní upgrade tooinstall připojit na hello stejný server.</span><span class="sxs-lookup"><span data-stu-id="e21ac-137">In-place upgrade tooinstall Connect on hello same server.</span></span>
- <span data-ttu-id="e21ac-138">Paralelní nasazení tooinstall Connect na nový server, zatímco existující server DirSync hello je stále v provozu.</span><span class="sxs-lookup"><span data-stu-id="e21ac-138">Parallel deployment tooinstall Connect on a new server while hello existing DirSync server is still operational.</span></span>

## <a name="upgrade-from-azure-ad-sync"></a><span data-ttu-id="e21ac-139">Upgrade z Azure AD Sync</span><span class="sxs-lookup"><span data-stu-id="e21ac-139">Upgrade from Azure AD Sync</span></span>
<span data-ttu-id="e21ac-140">Pokud používáte Azure AD Sync, pak můžete postupovat podle hello [stejný postup](active-directory-aadconnect-upgrade-previous-version.md) jako při upgradu z jedné verze Connect tooa novější.</span><span class="sxs-lookup"><span data-stu-id="e21ac-140">If you are currently using Azure AD Sync, then you can follow hello [same steps](active-directory-aadconnect-upgrade-previous-version.md) as when you upgrade from one Connect version tooa newer.</span></span> <span data-ttu-id="e21ac-141">Existují dva různé možnosti upgradu:</span><span class="sxs-lookup"><span data-stu-id="e21ac-141">There are two different upgrade options:</span></span>

- <span data-ttu-id="e21ac-142">Místní upgrade tooinstall připojit na hello stejný server.</span><span class="sxs-lookup"><span data-stu-id="e21ac-142">In-place upgrade tooinstall Connect on hello same server.</span></span>
- <span data-ttu-id="e21ac-143">Migrace dráha tooinstall Connect na nový server při hello existující server Azure AD Sync je stále v provozu.</span><span class="sxs-lookup"><span data-stu-id="e21ac-143">Swing-migration tooinstall Connect on a new server while hello existing Azure AD Sync server is still operational.</span></span>

## <a name="migrate-from-fim2010-or-mim2016"></a><span data-ttu-id="e21ac-144">Migrace z FIM2010 nebo MIM2016</span><span class="sxs-lookup"><span data-stu-id="e21ac-144">Migrate from FIM2010 or MIM2016</span></span>
<span data-ttu-id="e21ac-145">Pokud aktuálně používáte produktu Forefront Identity Manager 2010 nebo Microsoft Identity Manager 2016 s hello konektoru služby Azure AD, je jedinou možností migrace.</span><span class="sxs-lookup"><span data-stu-id="e21ac-145">If you are currently using Forefront Identity Manager 2010 or Microsoft Identity Manager 2016 with hello Azure AD Connector, then your only option is a migration.</span></span> <span data-ttu-id="e21ac-146">Postupujte podle kroků hello popsaných v [dráha migrace](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="e21ac-146">Follow hello steps described in [swing-migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span> <span data-ttu-id="e21ac-147">V krocích hello nahraďte FIM2010/MIM2016 uvedeny žádné informace o Azure AD Sync.</span><span class="sxs-lookup"><span data-stu-id="e21ac-147">In hello steps, replace any mention of Azure AD Sync with FIM2010/MIM2016.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e21ac-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e21ac-148">Next steps</span></span>
<span data-ttu-id="e21ac-149">V závislosti na hello možnost jste vybrali toouse, použijte hello tabulku z obsahu toohello levém toofind váš článek s hello podrobné kroky.</span><span class="sxs-lookup"><span data-stu-id="e21ac-149">Depending on hello option you have selected toouse, use hello table of content toohello left toofind your article with hello detailed steps.</span></span>
