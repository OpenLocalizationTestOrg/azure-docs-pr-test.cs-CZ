---
title: "nastavení zásad a správu mobilních zařízení aaaGroup | Microsoft Docs"
description: "Poskytuje informace o zásadách skupiny a mobilních zařízení nastavení správy (MDM), které se mají použít na zařízeních vlastněných společností. Tyto zásady jsou použité toohello uživatele zařízení."
services: active-directory
keywords: "Co jsou zásady skupiny a nastavení MDM Enterprise State Roaming, Enterprise State Roaming, cloudu systému windows"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 762419b47014b1fb4d92ac528785e20078afe689
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="group-policy-and-mdm-settings"></a><span data-ttu-id="5ca7d-105">Nastavení zásad skupiny a správu mobilních zařízení</span><span class="sxs-lookup"><span data-stu-id="5ca7d-105">Group Policy and MDM settings</span></span>
<span data-ttu-id="5ca7d-106">Používejte tyto zásady skupiny a nastavení správy mobilních zařízení jenom na zařízení ve vlastnictví firmy, protože tyto zásady jsou použité toohello uživatele zařízení.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-106">Use these group policy and mobile device management (MDM) settings only on corporate-owned devices because these policies are applied toohello user’s entire device.</span></span> <span data-ttu-id="5ca7d-107">MDM zásad toodisable nastavení synchronizace pro osobní použití, zařízení ve vlastnictví uživatele bude mít negativní vliv na použití hello těchto zařízení.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-107">Applying an MDM policy toodisable settings sync for a personal, user-owned device will negatively impact hello use of that device.</span></span> <span data-ttu-id="5ca7d-108">Kromě toho by jiné uživatelské účty na hello zařízení nebude mít vliv také zásadami hello.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-108">Additionally, other user accounts on hello device will also be affected by hello policy.</span></span>

<span data-ttu-id="5ca7d-109">Podnikům, které chcete, můžete použít toomanage roamingu pro osobní zařízení (nespravovaný) hello Azure portálu tooenable nebo zakázat roaming, nikoli pomocí zásad skupiny nebo správy mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-109">Enterprises that want toomanage roaming for personal (unmanaged) devices can use hello Azure portal tooenable or disable roaming, rather than using Group Policy or MDM.</span></span>
<span data-ttu-id="5ca7d-110">Hello následující tabulky popisují hello nastavení zásad, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-110">hello following tables describe hello policy settings available.</span></span>

## <a name="mdm-settings"></a><span data-ttu-id="5ca7d-111">Nastavení MDM</span><span class="sxs-lookup"><span data-stu-id="5ca7d-111">MDM settings</span></span>
<span data-ttu-id="5ca7d-112">nastavení zásad MDM Hello týká tooboth Windows 10 a Windows 10 Mobile.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-112">hello MDM policy settings apply tooboth Windows 10 and Windows 10 Mobile.</span></span>  <span data-ttu-id="5ca7d-113">Windows 10 Mobile podpora je dostupná pouze pro účet Microsoft na základě roaming prostřednictvím účtu OneDrive.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-113">Windows 10 Mobile support exists only for Microsoft account based roaming via user’s OneDrive account.</span></span>  <span data-ttu-id="5ca7d-114">Podrobnosti najdete příliš "Koncové body a zařízení" části Podrobnosti o jaká zařízení jsou podporovány pro Azure AD na základě, synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-114">Please refer too“Devices and endpoints” section for details on what devices are supported for Azure AD based syncing.</span></span>

| <span data-ttu-id="5ca7d-115">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5ca7d-115">Name</span></span> | <span data-ttu-id="5ca7d-116">Popis</span><span class="sxs-lookup"><span data-stu-id="5ca7d-116">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5ca7d-117">Povolit účet Microsoft připojení</span><span class="sxs-lookup"><span data-stu-id="5ca7d-117">Allow Microsoft Account Connection</span></span> |<span data-ttu-id="5ca7d-118">Umožňuje uživatelům tooauthenticate používání účtu Microsoft na zařízení hello</span><span class="sxs-lookup"><span data-stu-id="5ca7d-118">Allows users tooauthenticate using a Microsoft account on hello device</span></span> |
| <span data-ttu-id="5ca7d-119">Povolit synchronizaci nastavení</span><span class="sxs-lookup"><span data-stu-id="5ca7d-119">Allow Sync My Settings</span></span> |<span data-ttu-id="5ca7d-120">Umožňuje uživatelům tooroam Windows nastavení a dat aplikací; Zakázání této zásady zakážete synchronizaci, stejně jako zálohy na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="5ca7d-120">Allows users tooroam Windows settings and app data; Disabling this policy will disable sync as well as backups on mobile devices</span></span> |

## <a name="group-policy-settings"></a><span data-ttu-id="5ca7d-121">Nastavení zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="5ca7d-121">Group Policy settings</span></span>
<span data-ttu-id="5ca7d-122">nastavení zásad skupiny Hello použít tooWindows 10 zařízení, které jsou připojené k tooan domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-122">hello Group Policy settings apply tooWindows 10 devices that are joined tooan Active Directory domain.</span></span> <span data-ttu-id="5ca7d-123">Tabulka Hello také obsahuje starší verze nastavení, se zobrazí toomanage nastavení synchronizace, ale které nefungují pro podnikové stavu roamingu pro Windows 10, které jsou uvedené s, nepoužívejte, v popisu hello.</span><span class="sxs-lookup"><span data-stu-id="5ca7d-123">hello table also includes legacy settings that would appear toomanage sync settings, but that do not work for Enterprise State Roaming for Windows 10, which are noted with ‘Do not use’ in hello description.</span></span>

| <span data-ttu-id="5ca7d-124">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5ca7d-124">Name</span></span> | <span data-ttu-id="5ca7d-125">Popis</span><span class="sxs-lookup"><span data-stu-id="5ca7d-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5ca7d-126">Účty: Blokovat účty Microsoft</span><span class="sxs-lookup"><span data-stu-id="5ca7d-126">Accounts: Block Microsoft Accounts</span></span> |<span data-ttu-id="5ca7d-127">Nastavení této zásady zabrání uživatelům v přidávání nových účtů Microsoft na tomto počítači</span><span class="sxs-lookup"><span data-stu-id="5ca7d-127">This policy setting prevents users from adding new Microsoft accounts on this computer</span></span> |
| <span data-ttu-id="5ca7d-128">Není synchronizována</span><span class="sxs-lookup"><span data-stu-id="5ca7d-128">Do not sync</span></span> |<span data-ttu-id="5ca7d-129">Zabraňuje uživatelům tooroam Windows nastavení a dat aplikací</span><span class="sxs-lookup"><span data-stu-id="5ca7d-129">Prevents users tooroam Windows settings and app data</span></span> |
| <span data-ttu-id="5ca7d-130">Není synchronizována přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="5ca7d-130">Do not sync personalize</span></span> |<span data-ttu-id="5ca7d-131">Zakáže synchronizuje hello motivy skupiny</span><span class="sxs-lookup"><span data-stu-id="5ca7d-131">Disables syncing of hello Themes group</span></span> |
| <span data-ttu-id="5ca7d-132">Není synchronizována nastavení prohlížeče</span><span class="sxs-lookup"><span data-stu-id="5ca7d-132">Do not sync browser settings</span></span> |<span data-ttu-id="5ca7d-133">Zakáže synchronizuje skupiny hello Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="5ca7d-133">Disables syncing of hello Internet Explorer group</span></span> |
| <span data-ttu-id="5ca7d-134">Ne synchronizaci hesel</span><span class="sxs-lookup"><span data-stu-id="5ca7d-134">Do not sync passwords</span></span> |<span data-ttu-id="5ca7d-135">Zakáže synchronizaci hesel skupiny</span><span class="sxs-lookup"><span data-stu-id="5ca7d-135">Disables syncing of Passwords group</span></span> |
| <span data-ttu-id="5ca7d-136">Není synchronizována další nastavení Windows</span><span class="sxs-lookup"><span data-stu-id="5ca7d-136">Do not sync other Windows settings</span></span> |<span data-ttu-id="5ca7d-137">Zakáže synchronizuje ostatní okna nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="5ca7d-137">Disables syncing of Other Windows settings group</span></span> |
| <span data-ttu-id="5ca7d-138">Přizpůsobení plochy není synchronizována</span><span class="sxs-lookup"><span data-stu-id="5ca7d-138">Do not sync desktop personalization</span></span> |<span data-ttu-id="5ca7d-139">Nepoužívejte; nemá žádný vliv</span><span class="sxs-lookup"><span data-stu-id="5ca7d-139">Do not use; has no effect</span></span> |
| <span data-ttu-id="5ca7d-140">Není synchronizována u měřených připojení k</span><span class="sxs-lookup"><span data-stu-id="5ca7d-140">Do not sync on metered connections</span></span> |<span data-ttu-id="5ca7d-141">Zakáže roaming v měřená připojení, jako je například mobilní 3 G</span><span class="sxs-lookup"><span data-stu-id="5ca7d-141">Disables roaming on metered connections, such as cellular 3G</span></span> |
| <span data-ttu-id="5ca7d-142">Není synchronizována aplikace</span><span class="sxs-lookup"><span data-stu-id="5ca7d-142">Do not sync apps</span></span> |<span data-ttu-id="5ca7d-143">Nepoužívejte; nemá žádný vliv</span><span class="sxs-lookup"><span data-stu-id="5ca7d-143">Do not use; has no effect</span></span> |
| <span data-ttu-id="5ca7d-144">Není synchronizována nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="5ca7d-144">Do not sync app settings</span></span> |<span data-ttu-id="5ca7d-145">Zakáže roaming dat aplikací</span><span class="sxs-lookup"><span data-stu-id="5ca7d-145">Disables roaming of app data</span></span> |
| <span data-ttu-id="5ca7d-146">Není synchronizována nastavení spuštění</span><span class="sxs-lookup"><span data-stu-id="5ca7d-146">Do not sync start settings</span></span> |<span data-ttu-id="5ca7d-147">Nepoužívejte; nemá žádný vliv</span><span class="sxs-lookup"><span data-stu-id="5ca7d-147">Do not use; has no effect</span></span> |

## <a name="related-topics"></a><span data-ttu-id="5ca7d-148">Související témata</span><span class="sxs-lookup"><span data-stu-id="5ca7d-148">Related topics</span></span>
* [<span data-ttu-id="5ca7d-149">Přehled Enterprise State Roaming</span><span class="sxs-lookup"><span data-stu-id="5ca7d-149">Enterprise State Roaming overview</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)
* [<span data-ttu-id="5ca7d-150">Povolit stav enterprise roaming v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5ca7d-150">Enable enterprise state roaming in Azure Active Directory</span></span>](active-directory-windows-enterprise-state-roaming-enable.md)
* [<span data-ttu-id="5ca7d-151">Nastavení a datový roaming – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5ca7d-151">Settings and data roaming FAQ</span></span>](active-directory-windows-enterprise-state-roaming-faqs.md)
* [<span data-ttu-id="5ca7d-152">Odkaz nastavení roamingu Windows 10</span><span class="sxs-lookup"><span data-stu-id="5ca7d-152">Windows 10 roaming settings reference</span></span>](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [<span data-ttu-id="5ca7d-153">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5ca7d-153">Troubleshooting</span></span>](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

