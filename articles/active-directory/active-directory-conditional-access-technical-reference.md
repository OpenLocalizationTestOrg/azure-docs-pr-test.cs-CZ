---
title: "aaaAzure technické informace o službě Active Directory podmíněného přístupu | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétní podmínky, kterou vyberete při ověřování uživatele hello a před povolením přístupu toohello aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace."
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
ms.openlocfilehash: ee201405d1d17f130607a95bf455b60cd222dd0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a><span data-ttu-id="b26b8-104">Technické informace o Azure Active Directory podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="b26b8-104">Azure Active Directory Conditional Access technical reference</span></span>

## <a name="services-enabled-with-conditional-access"></a><span data-ttu-id="b26b8-105">Služba povolena s podmíněným přístupem</span><span class="sxs-lookup"><span data-stu-id="b26b8-105">Services enabled with conditional access</span></span>

<span data-ttu-id="b26b8-106">Pravidla podmíněného přístupu jsou podporovány v rámci různých typů aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b26b8-106">Conditional Access rules are supported across various Azure AD application types.</span></span> <span data-ttu-id="b26b8-107">Tento seznam obsahuje:</span><span class="sxs-lookup"><span data-stu-id="b26b8-107">This list includes:</span></span>


* <span data-ttu-id="b26b8-108">Aplikace, které jsou registrovány hello Proxy aplikace služby Azure</span><span class="sxs-lookup"><span data-stu-id="b26b8-108">Applications registered with hello Azure Application Proxy</span></span>
* <span data-ttu-id="b26b8-109">Vzdálené aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="b26b8-109">Azure Remote App</span></span>
* <span data-ttu-id="b26b8-110">Vyvinuté řádek obchodní a víceklientské aplikace zaregistrované s Azure AD</span><span class="sxs-lookup"><span data-stu-id="b26b8-110">Developed line of business and multi-tenant applications registered with Azure AD</span></span>
* <span data-ttu-id="b26b8-111">Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="b26b8-111">Dynamics CRM</span></span>
* <span data-ttu-id="b26b8-112">Federované aplikace hello galerii aplikací Azure AD</span><span class="sxs-lookup"><span data-stu-id="b26b8-112">Federated applications from hello Azure AD application gallery</span></span>
* <span data-ttu-id="b26b8-113">Aplikace Microsoft Office 365 Yammer</span><span class="sxs-lookup"><span data-stu-id="b26b8-113">Microsoft Office 365 Yammer</span></span>
* <span data-ttu-id="b26b8-114">Aplikace Microsoft Office 365 Exchange Online</span><span class="sxs-lookup"><span data-stu-id="b26b8-114">Microsoft Office 365 Exchange Online</span></span>
* <span data-ttu-id="b26b8-115">Aplikace Microsoft Office 365 SharePoint Online (zahrnuje Onedrivu pro firmy)</span><span class="sxs-lookup"><span data-stu-id="b26b8-115">Microsoft Office 365 SharePoint Online (includes OneDrive for Business)</span></span>
* <span data-ttu-id="b26b8-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="b26b8-116">Microsoft Power BI</span></span> 
* <span data-ttu-id="b26b8-117">Heslo aplikace jednotného přihlašování z Galerie aplikace hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b26b8-117">Password SSO applications from hello Azure AD application gallery</span></span>
* <span data-ttu-id="b26b8-118">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="b26b8-118">Visual Studio Team Services</span></span>
* <span data-ttu-id="b26b8-119">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="b26b8-119">Microsoft Teams</span></span>









## <a name="enable-access-rules"></a><span data-ttu-id="b26b8-120">Povolení pravidla přístupu</span><span class="sxs-lookup"><span data-stu-id="b26b8-120">Enable access rules</span></span>
<span data-ttu-id="b26b8-121">Každé pravidlo můžete povolit nebo zakázat v na základů aplikace.</span><span class="sxs-lookup"><span data-stu-id="b26b8-121">Each rule can be enabled or disabled on a per application bases.</span></span> <span data-ttu-id="b26b8-122">Pokud jsou pravidla **ON** bude možné povolit a vynutit pro uživatele, kteří používají aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b26b8-122">When rules are **ON** they will be enabled and enforced for users accessing hello application.</span></span> <span data-ttu-id="b26b8-123">Když jsou **OFF** nebudou použity a není dopad hello přihlášení prostředí.</span><span class="sxs-lookup"><span data-stu-id="b26b8-123">When they are **OFF** they will not be used and will not impact hello users sign in experience.</span></span>

## <a name="applying-rules-toospecific-users"></a><span data-ttu-id="b26b8-124">Použití pravidla toospecific uživatelů</span><span class="sxs-lookup"><span data-stu-id="b26b8-124">Applying rules toospecific users</span></span>
<span data-ttu-id="b26b8-125">Pravidla mohou být použité toospecific skupiny uživatelů podle skupiny zabezpečení nastavením **použít na**.</span><span class="sxs-lookup"><span data-stu-id="b26b8-125">Rules can be applied toospecific sets of users based on security group by setting **Apply To**.</span></span> <span data-ttu-id="b26b8-126">**Použít na** lze nastavit příliš**všichni uživatelé** nebo **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="b26b8-126">**Apply To** can be set too**All Users** or **Groups**.</span></span> <span data-ttu-id="b26b8-127">Pokud nastavíte příliš**všichni uživatelé** hello pravidla budou platit tooany uživatele s aplikací toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="b26b8-127">When set too**All Users** hello rules will apply tooany user with access toohello application.</span></span> <span data-ttu-id="b26b8-128">Hello **skupiny** možnost umožňuje konkrétní zabezpečení a distribučních skupin toobe vybrali, pravidla se vynucují jenom pro tyto skupiny.</span><span class="sxs-lookup"><span data-stu-id="b26b8-128">hello **Groups** option allows specific security and distribution groups toobe selected, rules will only be enforced for these groups.</span></span>

<span data-ttu-id="b26b8-129">Pokud nasazujete pravidlo, je běžné toofirst použít omezenou sadu uživatelů, které jsou členy skupin pilotní.</span><span class="sxs-lookup"><span data-stu-id="b26b8-129">When deploying a rule,  it is common toofirst apply it a limited set of users, that are members of a piloting groups.</span></span> <span data-ttu-id="b26b8-130">Po dokončení hello pravidlo může být příliš**všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b26b8-130">Once complete hello rule can be applied too**All Users**.</span></span> <span data-ttu-id="b26b8-131">To způsobí, že pravidlo hello toobe vynucuje pro všechny uživatele v organizaci hello.</span><span class="sxs-lookup"><span data-stu-id="b26b8-131">This will cause hello rule toobe enforced for all users in hello organization.</span></span>

<span data-ttu-id="b26b8-132">Vyberte skupiny může být také vyloučené ze zásad pomocí hello **s výjimkou** možnost.</span><span class="sxs-lookup"><span data-stu-id="b26b8-132">Select groups may also be exempted from policy using hello **Except** option.</span></span> <span data-ttu-id="b26b8-133">Všechny členy těchto skupin bude vyloučené i v případě, že se objeví ve skupině součástí.</span><span class="sxs-lookup"><span data-stu-id="b26b8-133">Any members of these groups will be exempted even if they appear in an included group.</span></span>

## <a name="at-work-networks"></a><span data-ttu-id="b26b8-134">Sítě "v práci"</span><span class="sxs-lookup"><span data-stu-id="b26b8-134">“At work” networks</span></span>
<span data-ttu-id="b26b8-135">Pravidla podmíněného přístupu, které používají síť "v práci" spoléhat na důvěryhodný rozsahy IP adres, které byly konfigurovány ve službě Azure AD, nebo použijte hello "uvnitř corpnet" deklarace identity ze služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="b26b8-135">Conditional access rules that use an “At work” network, rely on trusted IP address ranges that have been configured in Azure AD, or use of hello "inside corpnet" claim from AD FS.</span></span> <span data-ttu-id="b26b8-136">Tato pravidla zahrnují:</span><span class="sxs-lookup"><span data-stu-id="b26b8-136">These rules include:</span></span>

* <span data-ttu-id="b26b8-137">Vyžadovat vícefaktorové ověřování, když není v práci</span><span class="sxs-lookup"><span data-stu-id="b26b8-137">Require multi-factor authentication when not at work</span></span>
* <span data-ttu-id="b26b8-138">Blokování přístupu, když není v práci</span><span class="sxs-lookup"><span data-stu-id="b26b8-138">Block access when not at work</span></span>

<span data-ttu-id="b26b8-139">Možnosti pro zadání sítě "v práci"</span><span class="sxs-lookup"><span data-stu-id="b26b8-139">Options for specifiying “at work” networks</span></span>

1. <span data-ttu-id="b26b8-140">Konfigurovat důvěryhodné rozsahy IP adres v hello [stránku konfigurace služby Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="b26b8-140">Configure trusted IP address ranges in hello [multi-factor authentication configuration page](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span></span> <span data-ttu-id="b26b8-141">Zásadu podmíněného přístupu použije rozsahy hello nakonfigurované pro každý pravidla ověřování žádosti a token vystavování tooevaluate.</span><span class="sxs-lookup"><span data-stu-id="b26b8-141">Conditional Access policy will use hello configured ranges on each authentication request and token issuance tooevaluate rules.</span></span> 
2. <span data-ttu-id="b26b8-142">Nakonfigurovat použití hello uvnitř corpnet deklarace identity, tuto možnost lze použít s federované adresáře pomocí služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="b26b8-142">Configure use of hello inside corpnet claim, this option can be used with federated directories, using AD FS.</span></span> <span data-ttu-id="b26b8-143">toolearn Další informace o hello uvnitř corpnet deklarace identity, najdete v části [IP adresy Tusted](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="b26b8-143">toolearn more about hello inside corpnet claims, see [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="rules-based-on-application-sensitivity"></a><span data-ttu-id="b26b8-144">Pravidla založená na aplikace velkých a malých písmen</span><span class="sxs-lookup"><span data-stu-id="b26b8-144">Rules based on application sensitivity</span></span>
<span data-ttu-id="b26b8-145">Pravidla jsou konfigurována na povolení hello vysoké hodnoty služby toobe zabezpečené bez dopadu na služby tooother přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b26b8-145">Rules are configured per application allowing hello high value services toobe secured without impacting access tooother services.</span></span> <span data-ttu-id="b26b8-146">Pravidla podmíněného přístupu se dá konfigurovat na hello **konfigurace** karta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b26b8-146">Conditional access rules can be configured on hello  **Configure** tab of hello application.</span></span> 

<span data-ttu-id="b26b8-147">Pravidla aktuálně nabízí:</span><span class="sxs-lookup"><span data-stu-id="b26b8-147">Rules currently offered:</span></span>

* <span data-ttu-id="b26b8-148">**Vyžadovat vícefaktorové ověřování**</span><span class="sxs-lookup"><span data-stu-id="b26b8-148">**Require multi-factor authentication**</span></span>
  
  * <span data-ttu-id="b26b8-149">Všichni uživatelé, je tato zásada použitá toowill být požadované tooauthenticate prostřednictvím služby Multi-Factor authentication alespoň jednou.</span><span class="sxs-lookup"><span data-stu-id="b26b8-149">All users that this policy is applied toowill be required tooauthenticate via multi-factor authentication at least once.</span></span>
* <span data-ttu-id="b26b8-150">**Vyžadovat vícefaktorové ověřování, když není v práci**</span><span class="sxs-lookup"><span data-stu-id="b26b8-150">**Require multi-factor authentication when not at work**</span></span>
  
  * <span data-ttu-id="b26b8-151">Pokud je tato zásada použitá, všichni uživatelé se budou požadované toohave provést službu Multi-Factor authentication alespoň jednou, pokud budou přistupovat k hello služby ze vzdáleného umístění mimo pracovní.</span><span class="sxs-lookup"><span data-stu-id="b26b8-151">If this policy is applied, all users will be required toohave performed multi-factor authentication at least once if they access hello service from a non-work remote location.</span></span> <span data-ttu-id="b26b8-152">Pokud se přesouvají z pracovního tooremote umístění, budou požadované tooperform vícefaktorového ověřování při přístupu k hello služby.</span><span class="sxs-lookup"><span data-stu-id="b26b8-152">If they move from a work tooremote location, they will be required tooperform multifactor authentication when accessing hello service.</span></span>
* <span data-ttu-id="b26b8-153">**Blokování přístupu, když není v práci**</span><span class="sxs-lookup"><span data-stu-id="b26b8-153">**Block access when not at work**</span></span> 
  
  * <span data-ttu-id="b26b8-154">Když se uživatelé přesunou z pracovní tooa vzdáleného umístění, pokud hello "Zablokovat přístup, když není v práci" zásady použité toothem že budou Blokovaní.</span><span class="sxs-lookup"><span data-stu-id="b26b8-154">When users move from work tooa remote location, they will be blocked if hello "Block access when not at work" policy is applied toothem.</span></span>  <span data-ttu-id="b26b8-155">Je znovu dovoleno přístup v pracovního umístění.</span><span class="sxs-lookup"><span data-stu-id="b26b8-155">They will be re-allowed access when at a work location.</span></span>

## <a name="related-topics"></a><span data-ttu-id="b26b8-156">Související témata</span><span class="sxs-lookup"><span data-stu-id="b26b8-156">Related topics</span></span>
* [<span data-ttu-id="b26b8-157">Zabezpečení přístupu tooOffice 365 a další aplikace připojeným tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="b26b8-157">Securing access tooOffice 365 and other apps connected tooAzure Active Directory</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="b26b8-158">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b26b8-158">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

