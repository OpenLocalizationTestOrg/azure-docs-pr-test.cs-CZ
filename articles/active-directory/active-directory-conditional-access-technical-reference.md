---
title: "Technické informace o Azure Active Directory podmíněného přístupu | Microsoft Docs"
description: "Azure Active Directory pomocí podmíněného řízení přístupu, zkontroluje konkrétní podmínky, kterou vyberete při ověřování uživatele a před povolením přístupu k aplikaci. Po splnění těchto podmínek je uživatel ověřený a přistupovat k aplikaci."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ca16a5399f94fd1ab267e0798cade3fd83f75b13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a><span data-ttu-id="09b63-104">Technické informace o Azure Active Directory podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="09b63-104">Azure Active Directory Conditional Access technical reference</span></span>

## <a name="services-enabled-with-conditional-access"></a><span data-ttu-id="09b63-105">Služba povolena s podmíněným přístupem</span><span class="sxs-lookup"><span data-stu-id="09b63-105">Services enabled with conditional access</span></span>

<span data-ttu-id="09b63-106">Pravidla podmíněného přístupu jsou podporovány v rámci různých typů aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="09b63-106">Conditional Access rules are supported across various Azure AD application types.</span></span> <span data-ttu-id="09b63-107">Tento seznam obsahuje:</span><span class="sxs-lookup"><span data-stu-id="09b63-107">This list includes:</span></span>


* <span data-ttu-id="09b63-108">Aplikace zaregistrované pomocí Proxy aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="09b63-108">Applications registered with the Azure Application Proxy</span></span>
* <span data-ttu-id="09b63-109">Vzdálené aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="09b63-109">Azure Remote App</span></span>
* <span data-ttu-id="09b63-110">Vyvinuté řádek obchodní a víceklientské aplikace zaregistrované s Azure AD</span><span class="sxs-lookup"><span data-stu-id="09b63-110">Developed line of business and multi-tenant applications registered with Azure AD</span></span>
* <span data-ttu-id="09b63-111">Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="09b63-111">Dynamics CRM</span></span>
* <span data-ttu-id="09b63-112">Federované aplikace v galerii aplikací Azure AD</span><span class="sxs-lookup"><span data-stu-id="09b63-112">Federated applications from the Azure AD application gallery</span></span>
* <span data-ttu-id="09b63-113">Aplikace Microsoft Office 365 Yammer</span><span class="sxs-lookup"><span data-stu-id="09b63-113">Microsoft Office 365 Yammer</span></span>
* <span data-ttu-id="09b63-114">Aplikace Microsoft Office 365 Exchange Online</span><span class="sxs-lookup"><span data-stu-id="09b63-114">Microsoft Office 365 Exchange Online</span></span>
* <span data-ttu-id="09b63-115">Aplikace Microsoft Office 365 SharePoint Online (zahrnuje Onedrivu pro firmy)</span><span class="sxs-lookup"><span data-stu-id="09b63-115">Microsoft Office 365 SharePoint Online (includes OneDrive for Business)</span></span>
* <span data-ttu-id="09b63-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="09b63-116">Microsoft Power BI</span></span> 
* <span data-ttu-id="09b63-117">Heslo aplikace jednotného přihlašování z galerii aplikací Azure AD</span><span class="sxs-lookup"><span data-stu-id="09b63-117">Password SSO applications from the Azure AD application gallery</span></span>
* <span data-ttu-id="09b63-118">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="09b63-118">Visual Studio Team Services</span></span>
* <span data-ttu-id="09b63-119">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="09b63-119">Microsoft Teams</span></span>









## <a name="enable-access-rules"></a><span data-ttu-id="09b63-120">Povolení pravidla přístupu</span><span class="sxs-lookup"><span data-stu-id="09b63-120">Enable access rules</span></span>
<span data-ttu-id="09b63-121">Každé pravidlo můžete povolit nebo zakázat v na základů aplikace.</span><span class="sxs-lookup"><span data-stu-id="09b63-121">Each rule can be enabled or disabled on a per application bases.</span></span> <span data-ttu-id="09b63-122">Pokud jsou pravidla **ON** bude možné povolit a vynutit pro uživatele, kteří používají aplikaci.</span><span class="sxs-lookup"><span data-stu-id="09b63-122">When rules are **ON** they will be enabled and enforced for users accessing the application.</span></span> <span data-ttu-id="09b63-123">Když jsou **OFF** se nebude používat a nebude mít vliv na přihlašování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="09b63-123">When they are **OFF** they will not be used and will not impact the users sign in experience.</span></span>

## <a name="applying-rules-to-specific-users"></a><span data-ttu-id="09b63-124">Použití pravidel pro konkrétní uživatele</span><span class="sxs-lookup"><span data-stu-id="09b63-124">Applying rules to specific users</span></span>
<span data-ttu-id="09b63-125">Pravidla lze použít pro konkrétní skupiny uživatelů podle skupiny zabezpečení nastavením **použít na**.</span><span class="sxs-lookup"><span data-stu-id="09b63-125">Rules can be applied to specific sets of users based on security group by setting **Apply To**.</span></span> <span data-ttu-id="09b63-126">**Použít na** může být nastaven na **všichni uživatelé** nebo **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="09b63-126">**Apply To** can be set to **All Users** or **Groups**.</span></span> <span data-ttu-id="09b63-127">Pokud nastavíte hodnotu **všichni uživatelé** pravidla budou platit pro všechny uživatele s přístupem k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="09b63-127">When set to **All Users** the rules will apply to any user with access to the application.</span></span> <span data-ttu-id="09b63-128">**Skupiny** možnost umožňuje konkrétní zabezpečení a distribučních skupin být vybrána, pravidla se vynucují jenom pro tyto skupiny.</span><span class="sxs-lookup"><span data-stu-id="09b63-128">The **Groups** option allows specific security and distribution groups to be selected, rules will only be enforced for these groups.</span></span>

<span data-ttu-id="09b63-129">Pokud nasazujete pravidlo, je běžné nejprve provést omezené uživatelů, které jsou členy skupin pilotní.</span><span class="sxs-lookup"><span data-stu-id="09b63-129">When deploying a rule,  it is common to first apply it a limited set of users, that are members of a piloting groups.</span></span> <span data-ttu-id="09b63-130">Po dokončení pravidlo můžete použít k **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="09b63-130">Once complete the rule can be applied to **All Users**.</span></span> <span data-ttu-id="09b63-131">To způsobí, že je pravidlo, která budou vynucena pro všechny uživatele v organizaci.</span><span class="sxs-lookup"><span data-stu-id="09b63-131">This will cause the rule to be enforced for all users in the organization.</span></span>

<span data-ttu-id="09b63-132">Vyberte skupiny může být také vyloučené z pomocí zásad **s výjimkou** možnost.</span><span class="sxs-lookup"><span data-stu-id="09b63-132">Select groups may also be exempted from policy using the **Except** option.</span></span> <span data-ttu-id="09b63-133">Všechny členy těchto skupin bude vyloučené i v případě, že se objeví ve skupině součástí.</span><span class="sxs-lookup"><span data-stu-id="09b63-133">Any members of these groups will be exempted even if they appear in an included group.</span></span>

## <a name="at-work-networks"></a><span data-ttu-id="09b63-134">Sítě "v práci"</span><span class="sxs-lookup"><span data-stu-id="09b63-134">“At work” networks</span></span>
<span data-ttu-id="09b63-135">Pravidla podmíněného přístupu, které používají síť "v práci" spoléhat na důvěryhodný rozsahy IP adres, které byly konfigurovány ve službě Azure AD, nebo použijte deklarace "uvnitř corpnet" ze služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="09b63-135">Conditional access rules that use an “At work” network, rely on trusted IP address ranges that have been configured in Azure AD, or use of the "inside corpnet" claim from AD FS.</span></span> <span data-ttu-id="09b63-136">Tato pravidla zahrnují:</span><span class="sxs-lookup"><span data-stu-id="09b63-136">These rules include:</span></span>

* <span data-ttu-id="09b63-137">Vyžadovat vícefaktorové ověřování, když není v práci</span><span class="sxs-lookup"><span data-stu-id="09b63-137">Require multi-factor authentication when not at work</span></span>
* <span data-ttu-id="09b63-138">Blokování přístupu, když není v práci</span><span class="sxs-lookup"><span data-stu-id="09b63-138">Block access when not at work</span></span>

<span data-ttu-id="09b63-139">Možnosti pro zadání sítě "v práci"</span><span class="sxs-lookup"><span data-stu-id="09b63-139">Options for specifiying “at work” networks</span></span>

1. <span data-ttu-id="09b63-140">Konfigurovat důvěryhodné rozsahy IP adres ve [stránku konfigurace služby Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="09b63-140">Configure trusted IP address ranges in the [multi-factor authentication configuration page](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span></span> <span data-ttu-id="09b63-141">Zásadu podmíněného přístupu použije k vyhodnocení pravidel nastavených rozsahů na každé žádosti o ověření a vystavování tokenů.</span><span class="sxs-lookup"><span data-stu-id="09b63-141">Conditional Access policy will use the configured ranges on each authentication request and token issuance to evaluate rules.</span></span> 
2. <span data-ttu-id="09b63-142">Nakonfigurovat použití uvnitř corpnet deklarace identity, tuto možnost lze použít s federované adresáře pomocí služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="09b63-142">Configure use of the inside corpnet claim, this option can be used with federated directories, using AD FS.</span></span> <span data-ttu-id="09b63-143">Další informace o uvnitř corpnet deklarace identity, najdete v části [IP adresy Tusted](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="09b63-143">To learn more about the inside corpnet claims, see [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="rules-based-on-application-sensitivity"></a><span data-ttu-id="09b63-144">Pravidla založená na aplikace velkých a malých písmen</span><span class="sxs-lookup"><span data-stu-id="09b63-144">Rules based on application sensitivity</span></span>
<span data-ttu-id="09b63-145">Pravidla se konfigurují na aplikaci povolení služby vysoké hodnoty zabezpečený bez dopadu na přístup k jiným službám.</span><span class="sxs-lookup"><span data-stu-id="09b63-145">Rules are configured per application allowing the high value services to be secured without impacting access to other services.</span></span> <span data-ttu-id="09b63-146">Pravidla podmíněného přístupu můžete nakonfigurovat na **konfigurace** aplikace.</span><span class="sxs-lookup"><span data-stu-id="09b63-146">Conditional access rules can be configured on the  **Configure** tab of the application.</span></span> 

<span data-ttu-id="09b63-147">Pravidla aktuálně nabízí:</span><span class="sxs-lookup"><span data-stu-id="09b63-147">Rules currently offered:</span></span>

* <span data-ttu-id="09b63-148">**Vyžadovat vícefaktorové ověřování**</span><span class="sxs-lookup"><span data-stu-id="09b63-148">**Require multi-factor authentication**</span></span>
  
  * <span data-ttu-id="09b63-149">Všichni uživatelé, které se aplikují tyto zásady budou muset ověřit pomocí služby Multi-Factor authentication alespoň jednou.</span><span class="sxs-lookup"><span data-stu-id="09b63-149">All users that this policy is applied to will be required to authenticate via multi-factor authentication at least once.</span></span>
* <span data-ttu-id="09b63-150">**Vyžadovat vícefaktorové ověřování, když není v práci**</span><span class="sxs-lookup"><span data-stu-id="09b63-150">**Require multi-factor authentication when not at work**</span></span>
  
  * <span data-ttu-id="09b63-151">Pokud je tato zásada použitá, všichni uživatelé budou muset mít provést služby Multi-Factor authentication alespoň jednou, pokud jejich přístup ke službě ze vzdáleného umístění není funkční.</span><span class="sxs-lookup"><span data-stu-id="09b63-151">If this policy is applied, all users will be required to have performed multi-factor authentication at least once if they access the service from a non-work remote location.</span></span> <span data-ttu-id="09b63-152">Pokud se přesouvají z pracovní do vzdáleného umístění, budou muset provést vícefaktorového ověřování při přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="09b63-152">If they move from a work to remote location, they will be required to perform multifactor authentication when accessing the service.</span></span>
* <span data-ttu-id="09b63-153">**Blokování přístupu, když není v práci**</span><span class="sxs-lookup"><span data-stu-id="09b63-153">**Block access when not at work**</span></span> 
  
  * <span data-ttu-id="09b63-154">Když se uživatelé přesunou do vzdáleného umístění v práci, že budou Blokovaní, pokud k nim jsou použité zásady "Zablokovat přístup, když není v práci".</span><span class="sxs-lookup"><span data-stu-id="09b63-154">When users move from work to a remote location, they will be blocked if the "Block access when not at work" policy is applied to them.</span></span>  <span data-ttu-id="09b63-155">Je znovu dovoleno přístup v pracovního umístění.</span><span class="sxs-lookup"><span data-stu-id="09b63-155">They will be re-allowed access when at a work location.</span></span>

## <a name="related-topics"></a><span data-ttu-id="09b63-156">Související témata</span><span class="sxs-lookup"><span data-stu-id="09b63-156">Related topics</span></span>
* [<span data-ttu-id="09b63-157">Zabezpečení přístupu k Office 365 a jiným aplikacím připojeným ke službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="09b63-157">Securing access to Office 365 and other apps connected to Azure Active Directory</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="09b63-158">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="09b63-158">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

