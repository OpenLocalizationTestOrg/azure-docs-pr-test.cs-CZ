---
title: "Ochrana osobních údajů s ovládacími prvky Azure identit a přístupu | Microsoft Docs"
description: "Pomocí Azure identit a přístupu k ochraně vašich osobních údajů ovládací prvky"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: b43754efd207679dbe08710f44f56454a5fd20ab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a><span data-ttu-id="81a25-103">Azure Active Directory a služby Multi-Factor Authentication: ochrana osobních dat s ovládacími prvky identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="81a25-103">Azure Active Directory and Multi-Factor Authentication: Protect personal data with identity and access controls</span></span>

<span data-ttu-id="81a25-104">Tento článek obsahuje informace a postupy, které můžete použít k ochraně osobní data pomocí Azure Active Directory a službou Multi-Factor authentication zabezpečení funkcích a službách.</span><span class="sxs-lookup"><span data-stu-id="81a25-104">This article provides information and procedures you can use to protect personal data using Azure Active Directory and Multi-factor authentication security features and services.</span></span>

## <a name="scenario"></a><span data-ttu-id="81a25-105">Scénář</span><span class="sxs-lookup"><span data-stu-id="81a25-105">Scenario</span></span>

<span data-ttu-id="81a25-106">Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří, Jaderského a baltský moři, jakož i Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="81a25-106">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="81a25-107">Pro podporu těchto úsilí, získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojeném království</span><span class="sxs-lookup"><span data-stu-id="81a25-107">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span> 

<span data-ttu-id="81a25-108">Společnost používá Microsoft Azure k ukládání firemních dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="81a25-108">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="81a25-109">To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka.</span><span class="sxs-lookup"><span data-stu-id="81a25-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="81a25-110">Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako jsou adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti.</span><span class="sxs-lookup"><span data-stu-id="81a25-110">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="81a25-111">Na řádku výletních také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje ke sledování vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="81a25-111">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

<span data-ttu-id="81a25-112">Zaměstnanci společnosti přístup k síti ze vzdálených pobočkách společnosti a agenty cesta umístěné po celém světě mají přístup k některým prostředkům společnosti.</span><span class="sxs-lookup"><span data-stu-id="81a25-112">Corporate employees access the network from the company’s remote offices and travel agents located around the world have access to some company resources.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="81a25-113">Popis problému</span><span class="sxs-lookup"><span data-stu-id="81a25-113">Problem statement</span></span>

<span data-ttu-id="81a25-114">Společnosti musí ochrany osobních údajů zaměstnanců a zákazníků osobních údajů z útočníci snaží používat ohroženými identity k získání přístupu.</span><span class="sxs-lookup"><span data-stu-id="81a25-114">The company must protect the privacy of customers’ and employees’ personal data from attackers seeking to use compromised identities to gain access.</span></span> <span data-ttu-id="81a25-115">Se taky musí zajistěte, aby byl přístup k osobním údajům oprávněným uživatelům s omezeným přístupem jenom na ty, kteří potřebují ho pro svou práci.</span><span class="sxs-lookup"><span data-stu-id="81a25-115">They also must ensure that access to personal data by legitimate users is restricted to only those who need it to do their jobs.</span></span>

## <a name="company-goal"></a><span data-ttu-id="81a25-116">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="81a25-116">Company goal</span></span>

<span data-ttu-id="81a25-117">Cílem společnosti je potřeba zajistit přísnou kontrolu přístupu k osobním datům.</span><span class="sxs-lookup"><span data-stu-id="81a25-117">The company’s goal is to ensure that access to personal data is strictly controlled.</span></span> <span data-ttu-id="81a25-118">Je nezbytné, aby identit uživatelů s přístupem k osobním údajům chráněn silné ověřování.</span><span class="sxs-lookup"><span data-stu-id="81a25-118">It is essential that identities of users with access to personal data be protected by strong authentication.</span></span> <span data-ttu-id="81a25-119">Zásady [nejnižší oprávnění] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) musí být vynucená tak, aby oprávněných uživatelů pouze úroveň přístupu, které potřebují a žádné další.</span><span class="sxs-lookup"><span data-stu-id="81a25-119">A policy of [least privilege] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) must be enforced so that legitimate users have only the level of  access they need, and no more.</span></span>

## <a name="solutions"></a><span data-ttu-id="81a25-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="81a25-120">Solutions</span></span>

<span data-ttu-id="81a25-121">Microsoft Azure poskytuje nástroje pro správu identit a přístupu ke společnosti určovat, kdo má přístup k prostředkům, které obsahují osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="81a25-121">Microsoft Azure provides identity and access management tools to help companies control who has access to resources that contain personal data.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="81a25-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="81a25-122">Azure Active Directory</span></span>

<span data-ttu-id="81a25-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) spravuje identit a řídí přístup k Azure stejně jako další místní a dalších prostředků cloudu, datům a aplikacím.</span><span class="sxs-lookup"><span data-stu-id="81a25-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) manages identities and controls access to Azure as well as other on-premises and other cloud resources, data, and applications.</span></span> <span data-ttu-id="81a25-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) pomáhá správcům Azure, chcete-li minimalizovat počet lidí, kteří mají přístup k určité informace, jako je například osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="81a25-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) helps Azure administrators to minimize the number of people who have access to certain information such as personal data.</span></span> <span data-ttu-id="81a25-125">Umožňuje jejich k zjištění, omezte a monitorujte privilegované identity a jejich přístup k prostředkům a k přiřazení dočasné, JIT (JIT) práva správce na oprávněné uživatele.</span><span class="sxs-lookup"><span data-stu-id="81a25-125">It enables them to discover, restrict, and monitor privileged identities and their access to resources, and to assign temporary, Just-In-Time (JIT) administrative rights to eligible users.</span></span> <span data-ttu-id="81a25-126">Také poskytuje přehled o uživatelů, kteří mají oprávnění správce AAD.</span><span class="sxs-lookup"><span data-stu-id="81a25-126">It also provides insight into those who have AAD administrative privileges.</span></span>

<span data-ttu-id="81a25-127">Činnosti spojené s použitím AAD PIM, patří:</span><span class="sxs-lookup"><span data-stu-id="81a25-127">The activities involved in using AAD PIM include:</span></span>

- <span data-ttu-id="81a25-128">Povolení Privileged Identity Management pro svůj adresář</span><span class="sxs-lookup"><span data-stu-id="81a25-128">Enabling Privileged Identity Management for your directory</span></span>

- <span data-ttu-id="81a25-129">Pomocí řídicího panelu Správce Privileged Identity Management důležité informace na první pohled</span><span class="sxs-lookup"><span data-stu-id="81a25-129">Using Privileged Identity Management admin dashboard to see important information at a glance</span></span>

- <span data-ttu-id="81a25-130">Správa privilegovaných identit (administrators) přidáním nebo odebráním trvalé nebo oprávněné správce ke každé roli</span><span class="sxs-lookup"><span data-stu-id="81a25-130">Managing the privileged identities (administrators) by adding or removing permanent or eligible administrators to each role</span></span>

- <span data-ttu-id="81a25-131">Konfigurace nastavení aktivace role</span><span class="sxs-lookup"><span data-stu-id="81a25-131">Configuring the role activation settings</span></span>

- <span data-ttu-id="81a25-132">Aktivace role</span><span class="sxs-lookup"><span data-stu-id="81a25-132">Activating roles</span></span>

- <span data-ttu-id="81a25-133">Kontrola role aktivity</span><span class="sxs-lookup"><span data-stu-id="81a25-133">Reviewing role activity</span></span>

#### <a name="how-do-i-enable-aad-pim"></a><span data-ttu-id="81a25-134">Povolení AAD PIM</span><span class="sxs-lookup"><span data-stu-id="81a25-134">How do I enable AAD PIM?</span></span>

<span data-ttu-id="81a25-135">Chcete-li začít používat PIM pro váš adresář, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="81a25-135">To start using PIM for your directory, do the following:</span></span>

1. <span data-ttu-id="81a25-136">Přihlaste se k portálu Azure jako globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="81a25-136">Sign in to the Azure portal as a global administrator of your directory.</span></span>

2. <span data-ttu-id="81a25-137">Pokud má vaše organizace víc než jeden adresář, vyberte své uživatelské jméno v pravém horním rohu webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="81a25-137">If your organization has more than one directory, select your username in the upper right-hand corner of the Azure portal.</span></span> <span data-ttu-id="81a25-138">Vyberte adresář, kde budete používat Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="81a25-138">Select the directory where you will use Azure AD Privileged Identity Management.</span></span>

3. <span data-ttu-id="81a25-139">Vyberte **další služby** a použít **filtru** textové pole pro vyhledávání Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="81a25-139">Select **More services** and use the **Filter** textbox to search for Azure AD Privileged Identity Management.</span></span>

4. <span data-ttu-id="81a25-140">Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81a25-140">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="81a25-141">Aplikace Privileged Identity Management se otevře.</span><span class="sxs-lookup"><span data-stu-id="81a25-141">The Privileged Identity Management application opens.</span></span>

<span data-ttu-id="81a25-142">Po nastavení Azure AD Privileged Identity Management se vždy, když aplikaci otevřete, zobrazí navigační okno.</span><span class="sxs-lookup"><span data-stu-id="81a25-142">Once Azure AD Privileged Identity Management is set up, you see the navigation blade whenever you open the application.</span></span>

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

<span data-ttu-id="81a25-143">Další informace a pokyny, Začínáme se službou AAD PIM najdete v tématu [spustit pomocí Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span><span class="sxs-lookup"><span data-stu-id="81a25-143">For more information and instructions on getting started with AAD PIM, see [Start Using Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span></span>

### <a name="azure-role-based-access-control"></a><span data-ttu-id="81a25-144">Řízení přístupu Azure na základě rolí</span><span class="sxs-lookup"><span data-stu-id="81a25-144">Azure Role-based Access Control</span></span>

<span data-ttu-id="81a25-145">[Řízení přístupu Azure na základě rolí](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) pomáhá Azure správcům spravovat přístup k prostředkům Azure povolením udělení přístupu na základě role přiřazené uživatele.</span><span class="sxs-lookup"><span data-stu-id="81a25-145">[Azure Role-Based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) helps Azure administrators manage access to Azure resources by enabling the granting of access based on the user’s assigned role.</span></span> <span data-ttu-id="81a25-146">Můžete povinnosti v rámci týmu oddělit a poskytnout pouze takovou úroveň přístupu pro uživatele, skupiny a aplikace, které potřebují k provádění svých úloh.</span><span class="sxs-lookup"><span data-stu-id="81a25-146">You can segregate duties within a team and grant only the amount of access to users, groups and applications that they need to perform their jobs.</span></span>

<span data-ttu-id="81a25-147">Přístup na základě role můžete udělit uživatelům, kteří používají portál Azure, nástroje příkazového řádku Azure nebo rozhraní API pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="81a25-147">Role-based access can be granted to users using the Azure portal, Azure Command-Line tools or Azure Management APIs.</span></span>

<span data-ttu-id="81a25-148">Další informace o základní informace o Azure RBAC najdete v tématu [Začínáme s řízením přístupu na základě rolí na portálu Azure.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span><span class="sxs-lookup"><span data-stu-id="81a25-148">For more information about Azure RBAC basics, see [Get started with Role-Based Access Control in the Azure Portal.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span></span>

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a><span data-ttu-id="81a25-149">Jak lze spravovat Azure RBAC pomocí prostředí PowerShell?</span><span class="sxs-lookup"><span data-stu-id="81a25-149">How do I manage Azure RBAC with PowerShell?</span></span>

<span data-ttu-id="81a25-150">Rutiny prostředí PowerShell můžete použít ke správě Azure RBAC, včetně těchto úloh správy:</span><span class="sxs-lookup"><span data-stu-id="81a25-150">You can use PowerShell cmdlets to manage Azure RBAC, including the following management tasks:</span></span>

- <span data-ttu-id="81a25-151">Seznam rolí</span><span class="sxs-lookup"><span data-stu-id="81a25-151">List roles</span></span>

- <span data-ttu-id="81a25-152">Zjistit, kdo má přístup</span><span class="sxs-lookup"><span data-stu-id="81a25-152">See who has access</span></span>

- <span data-ttu-id="81a25-153">Udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="81a25-153">Grant access</span></span>

- <span data-ttu-id="81a25-154">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="81a25-154">Remove access</span></span>

- <span data-ttu-id="81a25-155">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="81a25-155">Create a custom role</span></span>

- <span data-ttu-id="81a25-156">Získat akce pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="81a25-156">Get Actions for a Resource Provider</span></span>

- <span data-ttu-id="81a25-157">Upravit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="81a25-157">Modify a custom role</span></span>

- <span data-ttu-id="81a25-158">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="81a25-158">Delete a custom role</span></span>

- <span data-ttu-id="81a25-159">Vlastní role seznamu</span><span class="sxs-lookup"><span data-stu-id="81a25-159">List custom roles</span></span>

<span data-ttu-id="81a25-160">Pokyny o tom, jak spravovat Azure RBAC pomocí prostředí PowerShell najdete v tématu [přístupu na základě Role spravovat pomocí prostředí Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span><span class="sxs-lookup"><span data-stu-id="81a25-160">For instructions on how to manage Azure RBAC with PowerShell, see [Manage Role-based Access with Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span></span>

### <a name="azure-multi-factor-authentication"></a><span data-ttu-id="81a25-161">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="81a25-161">Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="81a25-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) je řešení dvoustupňové ověření, která pomáhá zabezpečit přístup k datům a aplikacím, při splnění požadavků uživatelů pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="81a25-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) is a two-step verification solution that helps safeguard access to data and applications, while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="81a25-163">Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="81a25-163">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

<span data-ttu-id="81a25-164">Pokud chcete nasadit MFA v cloudu Azure, musíte nejprve povolte a potom zapnout dvoustupňové ověřování pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="81a25-164">To deploy MFA in the Azure cloud, you need to first enable it and then turn on two-step verification for users.</span></span>

#### <a name="how-do-i-enable-azure-to-use-mfa"></a><span data-ttu-id="81a25-165">Povolení Azure a použít vícefaktorové ověřování?</span><span class="sxs-lookup"><span data-stu-id="81a25-165">How do I enable Azure to use MFA?</span></span>

<span data-ttu-id="81a25-166">Pokud mají vaši uživatelé licencí, které zahrnují Azure Multi-Factor Authentication, není nic, které potřebujete udělat, aby zapnout Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="81a25-166">If your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need to do to turn on Azure MFA.</span></span> <span data-ttu-id="81a25-167">Pokud ne, musíte vytvořit poskytovatele vícefaktorového ověřování ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="81a25-167">If not, you need to create a Multi-Factor Auth provider in your directory.</span></span> <span data-ttu-id="81a25-168">Chcete-li to provést, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="81a25-168">To do this, follow these steps:</span></span>

1. <span data-ttu-id="81a25-169">Vyberte **služby Active Directory** na portálu Azure classic (přihlášeni jako správce).</span><span class="sxs-lookup"><span data-stu-id="81a25-169">Select **Active Directory** in the Azure classic portal (logged on as an administrator).</span></span>

2. <span data-ttu-id="81a25-170">Vyberte **poskytovatelé služby Multi-Factor Authentication.**</span><span class="sxs-lookup"><span data-stu-id="81a25-170">Select **Multi-Factor Authentication Providers.**</span></span>

3. <span data-ttu-id="81a25-171">Vyberte **nový,** a potom v části **aplikační služby,** vyberte **zprostředkovatel vícefaktorového ověřování.**</span><span class="sxs-lookup"><span data-stu-id="81a25-171">Select **New,** and then under **App Services,** select **Multi-Factor Auth Provider.**</span></span>

4. <span data-ttu-id="81a25-172">Vyberte **rychle vytvořit.**</span><span class="sxs-lookup"><span data-stu-id="81a25-172">Select **Quick Create.**</span></span>

5. <span data-ttu-id="81a25-173">Vyplňte pole název a vyberte modelu využití (za ověření nebo za povoleného uživatele).</span><span class="sxs-lookup"><span data-stu-id="81a25-173">Fill in the name field and select a usage model (per authentication or per enabled user).</span></span>

6. <span data-ttu-id="81a25-174">Určíte adresář, ke kterému je přiřazeno zprostředkovatele vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="81a25-174">Designate a directory with which the MFA Provider is associated.</span></span>

7. <span data-ttu-id="81a25-175">Klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81a25-175">Click the **Create** button.</span></span>

![](media/protect-personal-data-identity-access-controls/quick-create.png)

<span data-ttu-id="81a25-176">Další pokyny o tom, jak spravovat vaše zprostředkovatel vícefaktorového ověřování najdete v tématu [Začínáme s poskytovatele Azure Multi-Factor Auth.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span><span class="sxs-lookup"><span data-stu-id="81a25-176">For more instructions on how to manage your Multi-Factor Auth Provider, see [Getting Started with an Azure Multi-Factor Auth Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span></span>

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a><span data-ttu-id="81a25-177">Jak lze zapnout dvoustupňové ověřování pro uživatele?</span><span class="sxs-lookup"><span data-stu-id="81a25-177">How do I turn on two-step verification for users?</span></span>

<span data-ttu-id="81a25-178">Můžete vynutit dvoustupňové ověřování pro všechny přihlášení, nebo můžete vytvořit zásady podmíněného přístupu tak, aby vyžadovala dvoustupňové ověřování jenom v případě, že určité podmínky.</span><span class="sxs-lookup"><span data-stu-id="81a25-178">You can enforce two-step verification for all sign-ins, or you can create conditional access policies to require two-step verification only when specific conditions apply.</span></span>

<span data-ttu-id="81a25-179">Povolení Azure MFA změnou stavů uživatele je tradiční přístup pro vyžádání dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="81a25-179">Enabling Azure MFA by changing user states is the traditional approach for requiring two-step verification.</span></span> <span data-ttu-id="81a25-180">Všechny uživatele, kteří povolíte, bude mít stejný požadavek k provedení dvoustupňové ověřování při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="81a25-180">All the users that you enable will have the same requirement to perform two-step verification every time they sign in.</span></span> <span data-ttu-id="81a25-181">Povolení uživatele potlačí všechny zásady podmíněného přístupu, které může mít vliv na tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="81a25-181">Enabling a user overrides any conditional access policies that may affect that user.</span></span>

<span data-ttu-id="81a25-182">Povolení Azure MFA se zásadami podmíněného přístupu je flexibilnější přístup pro vyžádání dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="81a25-182">Enabling Azure MFA with a conditional access policy is a more flexible approach for requiring two-step verification.</span></span> <span data-ttu-id="81a25-183">Můžete vytvořit zásady podmíněného přístupu, které platí pro skupiny, jakož i jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="81a25-183">You can create conditional access policies that apply to groups as well as individual users.</span></span> <span data-ttu-id="81a25-184">S vysokým rizikem skupiny je možné přidělit další omezení než nízkým rizikem skupiny nebo dvoustupňové ověřování můžete vyžaduje se jenom pro vysoce rizikové cloudové aplikace a pro ty, které jsou nízkým rizikem přeskočeno.</span><span class="sxs-lookup"><span data-stu-id="81a25-184">High-risk groups can be given more restrictions than low-risk groups, or two-step verification can be required only for high-risk cloud apps and skipped for low-risk ones.</span></span> <span data-ttu-id="81a25-185">Podmíněný přístup je však placené funkce služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="81a25-185">However, conditional access is a paid feature of Azure Active Directory.</span></span>

<span data-ttu-id="81a25-186">Pokud chcete povolit MFA změnou stavu uživatele, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="81a25-186">To enable MFA by changing user state, do the following:</span></span>

1. <span data-ttu-id="81a25-187">Přihlaste se k portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="81a25-187">Sign in to the Azure portal as an administrator.</span></span>
2. <span data-ttu-id="81a25-188">Přejděte na **Azure Active Directory \> uživatelů a skupin \> všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="81a25-188">Go to **Azure Active Directory \> Users and groups \> All users**.</span></span>
3. <span data-ttu-id="81a25-189">Vyberte **služby Multi-Factor Authentication**.</span><span class="sxs-lookup"><span data-stu-id="81a25-189">Select **Multi-Factor Authentication**.</span></span>
4. <span data-ttu-id="81a25-190">Vyhledejte uživatele, který chcete povolit pro Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="81a25-190">Find the user that you want to enable for Azure MFA.</span></span> <span data-ttu-id="81a25-191">Možná bude třeba změnit zobrazení v horní části.</span><span class="sxs-lookup"><span data-stu-id="81a25-191">You may need to change the view at the top.</span></span>
5. <span data-ttu-id="81a25-192">Zaškrtněte políčko vedle jména uživatele.</span><span class="sxs-lookup"><span data-stu-id="81a25-192">Check the box next to the user’s name.</span></span>
6. <span data-ttu-id="81a25-193">Na pravé straně v části Rychlé kroky, zvolte **povolit**.</span><span class="sxs-lookup"><span data-stu-id="81a25-193">On the right, under quick steps, choose **Enable**.</span></span>

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. <span data-ttu-id="81a25-194">Potvrďte výběr v místním okně, které se otevře.</span><span class="sxs-lookup"><span data-stu-id="81a25-194">Confirm your selection in the pop-up window that opens.</span></span>  <span data-ttu-id="81a25-195">Uživatelé, pro které bylo povoleno vícefaktorové ověřování vyzve k registraci při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="81a25-195">Users for whom MFA has been enabled will be asked to register the next time they sign in.</span></span>

<span data-ttu-id="81a25-196">Pokud chcete povolit Azure MFA se zásadami podmíněného přístupu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="81a25-196">To enable Azure MFA with a conditional access policy, do the following:</span></span>

1. <span data-ttu-id="81a25-197">Přihlaste se k portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="81a25-197">Sign in to the Azure portal as an administrator.</span></span>

2. <span data-ttu-id="81a25-198">Přejděte na **Azure Active Directory \> podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="81a25-198">Go to **Azure Active Directory \> Conditional access**.</span></span>

3. <span data-ttu-id="81a25-199">Vyberte **nové zásady**.</span><span class="sxs-lookup"><span data-stu-id="81a25-199">Select **New policy**.</span></span>

4. <span data-ttu-id="81a25-200">V části **přiřazení**, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="81a25-200">Under **Assignments**, select **Users and groups**.</span></span> <span data-ttu-id="81a25-201">Použití **zahrnout** a **vyloučit** karty se můžete určit, kteří uživatelé a skupiny bude spravovat zásady.</span><span class="sxs-lookup"><span data-stu-id="81a25-201">Use the **Include** and     **Exclude** tabs to specify which users and groups will be managed by the policy.</span></span>

5. <span data-ttu-id="81a25-202">V části **přiřazení**, vyberte **cloudových aplikací.**</span><span class="sxs-lookup"><span data-stu-id="81a25-202">Under **Assignments**, select **Cloud apps.**</span></span> <span data-ttu-id="81a25-203">Zvolit **zahrnují všechny cloudové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="81a25-203">Choose to **include All cloud apps**.</span></span>
6.  <span data-ttu-id="81a25-204">V části **přístup k ovládacím prvkům**, vyberte **Grant**.</span><span class="sxs-lookup"><span data-stu-id="81a25-204">Under **Access controls**, select **Grant**.</span></span> <span data-ttu-id="81a25-205">Zvolte **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="81a25-205">Choose **Require multi-factor authentication**.</span></span>
7.  <span data-ttu-id="81a25-206">Zapnout **povolit zásady** k **na** a pak vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="81a25-206">Turn **Enable policy** to **On** and then select **Save**.</span></span>

<span data-ttu-id="81a25-207">Informace o tom, jak nakonfigurovat nastavení Azure MFA k nastavení upozornění na podvod, vytvořit jednorázové přihlášení, použijte vlastní hlasové zprávy, konfigurovat ukládání do mezipaměti, zadejte důvěryhodné IP adresy, vytvoření hesel aplikací, povolte zapamatování vícefaktorového ověřování pro zařízení tématu uživatelé důvěryhodnosti a vyberte možnost ověření metody [konfigurace nastavení Azure Multi-Factor Authentication.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span><span class="sxs-lookup"><span data-stu-id="81a25-207">For information on how to configure Azure MFA settings to set up fraud alerts, create a one-time bypass, use custom voice messages, configure caching, specify trusted IPs, create app passwords, enable remembering MFA for devices that users trust, and select verification methods, see [Configure Azure Multi-Factor Authentication Settings.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span></span>

## <a name="next-steps"></a><span data-ttu-id="81a25-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81a25-208">Next steps</span></span>

- [<span data-ttu-id="81a25-209">Zabezpečení privilegovaného přístupu ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="81a25-209">Securing privileged access in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [<span data-ttu-id="81a25-210">Nejčastější dotazy ohledně služby Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="81a25-210">Frequently asked questions about Azure Multi-Factor Authentication</span></span>](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [<span data-ttu-id="81a25-211">Na základě rolí řešení potíží s řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="81a25-211">Role-based Access Control troubleshooting</span></span>](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [<span data-ttu-id="81a25-212">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="81a25-212">Azure Active Directory Identity Protection</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
