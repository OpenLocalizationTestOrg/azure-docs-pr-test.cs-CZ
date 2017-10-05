---
title: 'Azure AD Connect: Vyberte typ instalace | Microsoft Docs'
description: "Toto téma vás provede jak vybrat typ instalace, který má být použit pro Azure AD Connect"
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
ms.openlocfilehash: a5697686bd1f41d581554b27ce78897963e38c74
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="select-which-installation-type-to-use-for-azure-ad-connect"></a><span data-ttu-id="7135a-103">Vyberte typ instalace, která se má použít pro Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="7135a-103">Select which installation type to use for Azure AD Connect</span></span>
<span data-ttu-id="7135a-104">Azure AD Connect má dva typy instalace pro novou instalaci: Express a přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="7135a-104">Azure AD Connect has two installation types for new installation: Express and customized.</span></span> <span data-ttu-id="7135a-105">V tomto tématu můžete rozhodnout, které můžete použít během instalace.</span><span class="sxs-lookup"><span data-stu-id="7135a-105">This topic helps you to decide which option to use during installation.</span></span>

## <a name="express"></a><span data-ttu-id="7135a-106">Express</span><span class="sxs-lookup"><span data-stu-id="7135a-106">Express</span></span>
<span data-ttu-id="7135a-107">Express je nejběžnější možnost a používá přibližně 90 % všechny nové instalace.</span><span class="sxs-lookup"><span data-stu-id="7135a-107">Express is the most common option and is used by about 90% of all new installations.</span></span> <span data-ttu-id="7135a-108">Byla navržená tak, aby zadat konfigurace, která funguje pro nejběžnějších scénářů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="7135a-108">It was designed to provide a configuration that works for the most common customer scenarios.</span></span>

<span data-ttu-id="7135a-109">Předpokládá:</span><span class="sxs-lookup"><span data-stu-id="7135a-109">It assumes:</span></span>

- <span data-ttu-id="7135a-110">Máte jeden Active Directory doménové struktury na místě.</span><span class="sxs-lookup"><span data-stu-id="7135a-110">You have a single Active Directory forest on-premises.</span></span>
- <span data-ttu-id="7135a-111">Máte účet správce podnikové sítě, které můžete použít pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="7135a-111">You have an enterprise administrator account you can use for the installation.</span></span>
- <span data-ttu-id="7135a-112">Máte méně než 100 000 objektů ve vaší místní službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7135a-112">You have less than 100,000 objects in your on-premises Active Directory.</span></span>

<span data-ttu-id="7135a-113">Můžete získat:</span><span class="sxs-lookup"><span data-stu-id="7135a-113">You get:</span></span>

- <span data-ttu-id="7135a-114">[Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) z místním nasazením a Azure AD pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7135a-114">[Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) from on-premises to Azure AD for single sign-on.</span></span>
- <span data-ttu-id="7135a-115">Konfigurace, který se synchronizuje [uživatelé, skupiny, kontakty a počítače s Windows 10](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="7135a-115">A configuration that synchronizes [users, groups, contacts, and Windows 10 computers](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
- <span data-ttu-id="7135a-116">Synchronizace všech oprávněných objektů ve všech doménách a ve všech organizačních.</span><span class="sxs-lookup"><span data-stu-id="7135a-116">Synchronization of all eligible objects in all domains and all OUs.</span></span>
- <span data-ttu-id="7135a-117">[Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) je povolena a zajistěte, aby vždy použít nejnovější dostupnou verzi.</span><span class="sxs-lookup"><span data-stu-id="7135a-117">[Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is enabled to make sure you always use the latest available version.</span></span>

<span data-ttu-id="7135a-118">Možnosti, kde můžete dál používat Express:</span><span class="sxs-lookup"><span data-stu-id="7135a-118">Options where you can still use Express:</span></span>

- <span data-ttu-id="7135a-119">Pokud nechcete synchronizovat všechny organizační jednotky, můžete stále používají systém Express a na poslední stránce, zrušte výběr **spustit proces synchronizace...** *.</span><span class="sxs-lookup"><span data-stu-id="7135a-119">If you do not want to synchronize all OUs, you can still use Express and on the last page, unselect **Start the synchronization process...***.</span></span> <span data-ttu-id="7135a-120">Pak znovu spusťte Průvodce instalací a změnit organizačních jednotek ve [možnosti konfigurace](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) a povolte plánované synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="7135a-120">Then run the installation wizard again and change the OUs in [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) and enable scheduled sync.</span></span>
- <span data-ttu-id="7135a-121">Chcete povolit jedna z funkcí v Azure AD Premium, jako je například zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="7135a-121">You want to enable one of the features in Azure AD Premium, such as Password writeback.</span></span> <span data-ttu-id="7135a-122">Nejdřív projít express získat počáteční instalaci dokončit.</span><span class="sxs-lookup"><span data-stu-id="7135a-122">First go through express to get the initial installation completed.</span></span> <span data-ttu-id="7135a-123">Pak znovu spusťte Průvodce instalací a změnit [možnosti konfigurace](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span><span class="sxs-lookup"><span data-stu-id="7135a-123">Then run the installation wizard again and change the [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span></span>

## <a name="custom"></a><span data-ttu-id="7135a-124">Vlastní</span><span class="sxs-lookup"><span data-stu-id="7135a-124">Custom</span></span>
<span data-ttu-id="7135a-125">Cesta k přizpůsobenému umožňuje mnohem víc možností než express.</span><span class="sxs-lookup"><span data-stu-id="7135a-125">The customized path allows many more options than express.</span></span> <span data-ttu-id="7135a-126">By být používána ve všech případech, kdy není konfigurace popsané v předchozí části pro expresní reprezentativní pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="7135a-126">It should be used in all cases where the configuration described in previous section for express is not representative for your organization.</span></span>

<span data-ttu-id="7135a-127">Použijte, když:</span><span class="sxs-lookup"><span data-stu-id="7135a-127">Use when:</span></span>

- <span data-ttu-id="7135a-128">Nemáte přístup k účet správce podnikové sítě ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7135a-128">You do not have access to an enterprise admin account in Active Directory.</span></span>
- <span data-ttu-id="7135a-129">Máte více než jedné doménové struktuře nebo budete chtít v budoucnu synchronizovat více než jedné doménové struktuře.</span><span class="sxs-lookup"><span data-stu-id="7135a-129">You have more than one forest or you plan to synchronize more than one forest in the future.</span></span>
- <span data-ttu-id="7135a-130">Máte domén v doménové struktuře není dostupný ze serveru připojit.</span><span class="sxs-lookup"><span data-stu-id="7135a-130">You have domains in your forest not reachable from the Connect server.</span></span>
- <span data-ttu-id="7135a-131">Máte v plánu používat federaci nebo předávací ověřování pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="7135a-131">You plan to use federation or pass-through authentication for user sign-in.</span></span>
- <span data-ttu-id="7135a-132">Máte více než 100 000 objektů a potřebují používat plnou instalaci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7135a-132">You have more than 100,000 objects and need to use a full SQL Server.</span></span>
- <span data-ttu-id="7135a-133">Máte v plánu pomocí filtrování podle skupiny a ne jen domény nebo filtrování založené na organizační jednotku.</span><span class="sxs-lookup"><span data-stu-id="7135a-133">You plan to use group-based filtering and not only domain or OU-based filtering.</span></span>

## <a name="upgrade-from-dirsync"></a><span data-ttu-id="7135a-134">Upgrade z nástroje DirSync</span><span class="sxs-lookup"><span data-stu-id="7135a-134">Upgrade from DirSync</span></span>
<span data-ttu-id="7135a-135">Pokud aktuálně používáte DirSync, postupujte podle pokynů v [upgradu z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) k upgradu vaší stávající konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7135a-135">If you are currently using DirSync, then follow the steps in [Upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) to upgrade your existing configuration.</span></span> <span data-ttu-id="7135a-136">Existují dva různé možnosti upgradu:</span><span class="sxs-lookup"><span data-stu-id="7135a-136">There are two different upgrade options:</span></span>

- <span data-ttu-id="7135a-137">Místní upgrade na instalaci připojit na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="7135a-137">In-place upgrade to install Connect on the same server.</span></span>
- <span data-ttu-id="7135a-138">Paralelní nasazení k instalaci Connect na nový server, zatímco existující server DirSync je stále v provozu.</span><span class="sxs-lookup"><span data-stu-id="7135a-138">Parallel deployment to install Connect on a new server while the existing DirSync server is still operational.</span></span>

## <a name="upgrade-from-azure-ad-sync"></a><span data-ttu-id="7135a-139">Upgrade z Azure AD Sync</span><span class="sxs-lookup"><span data-stu-id="7135a-139">Upgrade from Azure AD Sync</span></span>
<span data-ttu-id="7135a-140">Pokud používáte Azure AD Sync, pak můžete provést [stejný postup](active-directory-aadconnect-upgrade-previous-version.md) jako při upgradu z jedné verze připojit na novější.</span><span class="sxs-lookup"><span data-stu-id="7135a-140">If you are currently using Azure AD Sync, then you can follow the [same steps](active-directory-aadconnect-upgrade-previous-version.md) as when you upgrade from one Connect version to a newer.</span></span> <span data-ttu-id="7135a-141">Existují dva různé možnosti upgradu:</span><span class="sxs-lookup"><span data-stu-id="7135a-141">There are two different upgrade options:</span></span>

- <span data-ttu-id="7135a-142">Místní upgrade na instalaci připojit na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="7135a-142">In-place upgrade to install Connect on the same server.</span></span>
- <span data-ttu-id="7135a-143">Migrace dráha instalace Connect na nový server při existující server Azure AD Sync je stále v provozu.</span><span class="sxs-lookup"><span data-stu-id="7135a-143">Swing-migration to install Connect on a new server while the existing Azure AD Sync server is still operational.</span></span>

## <a name="migrate-from-fim2010-or-mim2016"></a><span data-ttu-id="7135a-144">Migrace z FIM2010 nebo MIM2016</span><span class="sxs-lookup"><span data-stu-id="7135a-144">Migrate from FIM2010 or MIM2016</span></span>
<span data-ttu-id="7135a-145">Pokud aktuálně používáte produktu Forefront Identity Manager 2010 nebo Microsoft Identity Manager 2016 se konektor služby Azure AD, je jedinou možností migrace.</span><span class="sxs-lookup"><span data-stu-id="7135a-145">If you are currently using Forefront Identity Manager 2010 or Microsoft Identity Manager 2016 with the Azure AD Connector, then your only option is a migration.</span></span> <span data-ttu-id="7135a-146">Postupujte podle kroků popsaných v [dráha migrace](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="7135a-146">Follow the steps described in [swing-migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span> <span data-ttu-id="7135a-147">V krocích nahraďte FIM2010/MIM2016 uvedeny žádné informace o Azure AD Sync.</span><span class="sxs-lookup"><span data-stu-id="7135a-147">In the steps, replace any mention of Azure AD Sync with FIM2010/MIM2016.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7135a-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7135a-148">Next steps</span></span>
<span data-ttu-id="7135a-149">V závislosti na možnosti, kterou jste se rozhodli použít použijte v tabulce obsahu na levé straně najít váš článek s podrobné kroky.</span><span class="sxs-lookup"><span data-stu-id="7135a-149">Depending on the option you have selected to use, use the table of content to the left to find your article with the detailed steps.</span></span>
