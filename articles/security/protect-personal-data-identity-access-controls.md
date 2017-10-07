---
title: "aaaProtect osobních dat s ovládacími prvky Azure přístupu a identit | Microsoft Docs"
description: "Pomocí Azure přístupu a identit a ovládací prvky toohelp ochrana vašich osobních údajů"
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
ms.openlocfilehash: 3132c2af25f86662668e5b555eab1d81de7f2e6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a><span data-ttu-id="72d0f-103">Azure Active Directory a služby Multi-Factor Authentication: ochrana osobních dat s ovládacími prvky identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="72d0f-103">Azure Active Directory and Multi-Factor Authentication: Protect personal data with identity and access controls</span></span>

<span data-ttu-id="72d0f-104">Tento článek obsahuje informace a postupy tooprotect osobní data pomocí Azure Active Directory a službou Multi-Factor authentication zabezpečení funkcích a službách, můžete použít.</span><span class="sxs-lookup"><span data-stu-id="72d0f-104">This article provides information and procedures you can use tooprotect personal data using Azure Active Directory and Multi-factor authentication security features and services.</span></span>

## <a name="scenario"></a><span data-ttu-id="72d0f-105">Scénář</span><span class="sxs-lookup"><span data-stu-id="72d0f-105">Scenario</span></span>

<span data-ttu-id="72d0f-106">Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="72d0f-106">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="72d0f-107">toosupport těchto úsilí získala menší výletních Víceřádkový na základě v Itálii Německo, Dánsko a hello Spojené království</span><span class="sxs-lookup"><span data-stu-id="72d0f-107">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span> 

<span data-ttu-id="72d0f-108">Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="72d0f-108">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="72d0f-109">To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka.</span><span class="sxs-lookup"><span data-stu-id="72d0f-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="72d0f-110">Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako jsou adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti.</span><span class="sxs-lookup"><span data-stu-id="72d0f-110">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="72d0f-111">Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="72d0f-111">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="72d0f-112">Zaměstnanci společnosti přístupu hello síti ze vzdálených pobočkách a agenty cesta hello společnosti nachází kolem hello, world mít přístup k prostředkům společnosti toosome.</span><span class="sxs-lookup"><span data-stu-id="72d0f-112">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="72d0f-113">Popis problému</span><span class="sxs-lookup"><span data-stu-id="72d0f-113">Problem statement</span></span>

<span data-ttu-id="72d0f-114">Hello společnosti musí ochrany osobních údajů hello zaměstnanců a zákazníků osobních údajů z útočníci, kteří přístup toogain identity toouse ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="72d0f-114">hello company must protect hello privacy of customers’ and employees’ personal data from attackers seeking toouse compromised identities toogain access.</span></span> <span data-ttu-id="72d0f-115">Také musí zajistit tento přístup toopersonal, které se data podle oprávněných uživatelů je omezena pouze na ty, kteří ho potřebují toodo svou práci.</span><span class="sxs-lookup"><span data-stu-id="72d0f-115">They also must ensure that access toopersonal data by legitimate users is restricted to only those who need it toodo their jobs.</span></span>

## <a name="company-goal"></a><span data-ttu-id="72d0f-116">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="72d0f-116">Company goal</span></span>

<span data-ttu-id="72d0f-117">Cílem společnosti Hello je, že tooensure, který přístup k datům toopersonal přísnou kontrolu.</span><span class="sxs-lookup"><span data-stu-id="72d0f-117">hello company’s goal is tooensure that access toopersonal data is strictly controlled.</span></span> <span data-ttu-id="72d0f-118">Je nezbytné, aby identit uživatelů s přístup k datům toopersonal chráněn silné ověřování.</span><span class="sxs-lookup"><span data-stu-id="72d0f-118">It is essential that identities of users with access toopersonal data be protected by strong authentication.</span></span> <span data-ttu-id="72d0f-119">Zásady [nejnižší oprávnění] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) musí být vynucená tak, aby oprávněných uživatelů pouze hello úroveň přístupu, které potřebují a žádné další.</span><span class="sxs-lookup"><span data-stu-id="72d0f-119">A policy of [least privilege] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) must be enforced so that legitimate users have only hello level of  access they need, and no more.</span></span>

## <a name="solutions"></a><span data-ttu-id="72d0f-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="72d0f-120">Solutions</span></span>

<span data-ttu-id="72d0f-121">Microsoft Azure poskytuje nástroje pro správu identit a přístupu toohelp společností určovat, kdo má přístup tooresources, které obsahují osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="72d0f-121">Microsoft Azure provides identity and access management tools toohelp companies control who has access tooresources that contain personal data.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="72d0f-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="72d0f-122">Azure Active Directory</span></span>

<span data-ttu-id="72d0f-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) spravuje identit a řídí přístup tooAzure stejně jako další místní a dalších prostředků cloudu, datům a aplikacím.</span><span class="sxs-lookup"><span data-stu-id="72d0f-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) manages identities and controls access tooAzure as well as other on-premises and other cloud resources, data, and applications.</span></span> <span data-ttu-id="72d0f-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) pomáhá Azure správci toominimize hello počet lidí, kteří mají přístup toocertain informace, jako je osobní data.</span><span class="sxs-lookup"><span data-stu-id="72d0f-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) helps Azure administrators toominimize hello number of people who have access toocertain information such as personal data.</span></span> <span data-ttu-id="72d0f-125">Umožňuje jim toodiscover, omezte a monitorujte privilegované identity a jejich přístup tooresources a tooassign dočasné, JIT (JIT) právy tooeligible uživatelů.</span><span class="sxs-lookup"><span data-stu-id="72d0f-125">It enables them toodiscover, restrict, and monitor privileged identities and their access tooresources, and tooassign temporary, Just-In-Time (JIT) administrative rights tooeligible users.</span></span> <span data-ttu-id="72d0f-126">Také poskytuje přehled o uživatelů, kteří mají oprávnění správce AAD.</span><span class="sxs-lookup"><span data-stu-id="72d0f-126">It also provides insight into those who have AAD administrative privileges.</span></span>

<span data-ttu-id="72d0f-127">Hello činnosti spojené s použitím AAD PIM, patří:</span><span class="sxs-lookup"><span data-stu-id="72d0f-127">hello activities involved in using AAD PIM include:</span></span>

- <span data-ttu-id="72d0f-128">Povolení Privileged Identity Management pro svůj adresář</span><span class="sxs-lookup"><span data-stu-id="72d0f-128">Enabling Privileged Identity Management for your directory</span></span>

- <span data-ttu-id="72d0f-129">Použití Privileged Identity Management správce řídicího panelu toosee důležité informace na první pohled</span><span class="sxs-lookup"><span data-stu-id="72d0f-129">Using Privileged Identity Management admin dashboard toosee important information at a glance</span></span>

- <span data-ttu-id="72d0f-130">Správa identit hello privilegovaný (administrators) přidáním nebo odebráním rolí tooeach trvalé nebo oprávněné správce</span><span class="sxs-lookup"><span data-stu-id="72d0f-130">Managing hello privileged identities (administrators) by adding or removing permanent or eligible administrators tooeach role</span></span>

- <span data-ttu-id="72d0f-131">Konfigurace nastavení aktivace role hello</span><span class="sxs-lookup"><span data-stu-id="72d0f-131">Configuring hello role activation settings</span></span>

- <span data-ttu-id="72d0f-132">Aktivace role</span><span class="sxs-lookup"><span data-stu-id="72d0f-132">Activating roles</span></span>

- <span data-ttu-id="72d0f-133">Kontrola role aktivity</span><span class="sxs-lookup"><span data-stu-id="72d0f-133">Reviewing role activity</span></span>

#### <a name="how-do-i-enable-aad-pim"></a><span data-ttu-id="72d0f-134">Povolení AAD PIM</span><span class="sxs-lookup"><span data-stu-id="72d0f-134">How do I enable AAD PIM?</span></span>

<span data-ttu-id="72d0f-135">toostart pomocí PIM pro váš adresář hello následující:</span><span class="sxs-lookup"><span data-stu-id="72d0f-135">toostart using PIM for your directory, do hello following:</span></span>

1. <span data-ttu-id="72d0f-136">Přihlaste se toohello portálu Azure jako globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="72d0f-136">Sign in toohello Azure portal as a global administrator of your directory.</span></span>

2. <span data-ttu-id="72d0f-137">Pokud má vaše organizace více než jeden adresář, vyberte své uživatelské jméno v hello pravém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="72d0f-137">If your organization has more than one directory, select your username in hello upper right-hand corner of hello Azure portal.</span></span> <span data-ttu-id="72d0f-138">Vyberte adresář hello, kde budete používat Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="72d0f-138">Select hello directory where you will use Azure AD Privileged Identity Management.</span></span>

3. <span data-ttu-id="72d0f-139">Vyberte **další služby** a používat hello **filtru** toosearch textové pole pro Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="72d0f-139">Select **More services** and use hello **Filter** textbox toosearch for Azure AD Privileged Identity Management.</span></span>

4. <span data-ttu-id="72d0f-140">Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-140">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="72d0f-141">Otevře se Hello aplikace Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="72d0f-141">hello Privileged Identity Management application opens.</span></span>

<span data-ttu-id="72d0f-142">Jakmile Azure AD Privileged Identity Management nastavená, zobrazí se při každém otevření aplikace hello hello navigačním okně.</span><span class="sxs-lookup"><span data-stu-id="72d0f-142">Once Azure AD Privileged Identity Management is set up, you see hello navigation blade whenever you open hello application.</span></span>

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

<span data-ttu-id="72d0f-143">Další informace a pokyny, Začínáme se službou AAD PIM najdete v tématu [spustit pomocí Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span><span class="sxs-lookup"><span data-stu-id="72d0f-143">For more information and instructions on getting started with AAD PIM, see [Start Using Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span></span>

### <a name="azure-role-based-access-control"></a><span data-ttu-id="72d0f-144">Řízení přístupu Azure na základě rolí</span><span class="sxs-lookup"><span data-stu-id="72d0f-144">Azure Role-based Access Control</span></span>

<span data-ttu-id="72d0f-145">[Řízení přístupu Azure na základě rolí](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) pomáhá Azure správcům spravovat přístup k prostředkům tooAzure povolením hello udělení přístupu na základě role přiřazené hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="72d0f-145">[Azure Role-Based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) helps Azure administrators manage access tooAzure resources by enabling hello granting of access based on hello user’s assigned role.</span></span> <span data-ttu-id="72d0f-146">Můžete povinnosti v rámci týmu oddělit a udělit pouze hello množství toousers přístup, skupiny nebo aplikace, které potřebují tooperform svou práci.</span><span class="sxs-lookup"><span data-stu-id="72d0f-146">You can segregate duties within a team and grant only hello amount of access toousers, groups and applications that they need tooperform their jobs.</span></span>

<span data-ttu-id="72d0f-147">Toousers pomocí hello portálu Azure, nástroje příkazového řádku Azure nebo rozhraní API pro správu Azure můžete udělit přístup na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="72d0f-147">Role-based access can be granted toousers using hello Azure portal, Azure Command-Line tools or Azure Management APIs.</span></span>

<span data-ttu-id="72d0f-148">Další informace o základní informace o Azure RBAC najdete v tématu [Začínáme s řízením přístupu na základě rolí v hello portálu Azure.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span><span class="sxs-lookup"><span data-stu-id="72d0f-148">For more information about Azure RBAC basics, see [Get started with Role-Based Access Control in hello Azure Portal.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span></span>

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a><span data-ttu-id="72d0f-149">Jak lze spravovat Azure RBAC pomocí prostředí PowerShell?</span><span class="sxs-lookup"><span data-stu-id="72d0f-149">How do I manage Azure RBAC with PowerShell?</span></span>

<span data-ttu-id="72d0f-150">Můžete použít toomanage rutiny prostředí PowerShell Azure RBAC, včetně hello následující úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="72d0f-150">You can use PowerShell cmdlets toomanage Azure RBAC, including hello following management tasks:</span></span>

- <span data-ttu-id="72d0f-151">Seznam rolí</span><span class="sxs-lookup"><span data-stu-id="72d0f-151">List roles</span></span>

- <span data-ttu-id="72d0f-152">Zjistit, kdo má přístup</span><span class="sxs-lookup"><span data-stu-id="72d0f-152">See who has access</span></span>

- <span data-ttu-id="72d0f-153">Udělení přístupu</span><span class="sxs-lookup"><span data-stu-id="72d0f-153">Grant access</span></span>

- <span data-ttu-id="72d0f-154">Odebrání přístupu</span><span class="sxs-lookup"><span data-stu-id="72d0f-154">Remove access</span></span>

- <span data-ttu-id="72d0f-155">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="72d0f-155">Create a custom role</span></span>

- <span data-ttu-id="72d0f-156">Získat akce pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="72d0f-156">Get Actions for a Resource Provider</span></span>

- <span data-ttu-id="72d0f-157">Upravit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="72d0f-157">Modify a custom role</span></span>

- <span data-ttu-id="72d0f-158">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="72d0f-158">Delete a custom role</span></span>

- <span data-ttu-id="72d0f-159">Vlastní role seznamu</span><span class="sxs-lookup"><span data-stu-id="72d0f-159">List custom roles</span></span>

<span data-ttu-id="72d0f-160">Pokyny, jak toomanage Azure RBAC v prostředí PowerShell najdete v části [přístupu na základě Role spravovat pomocí prostředí Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span><span class="sxs-lookup"><span data-stu-id="72d0f-160">For instructions on how toomanage Azure RBAC with PowerShell, see [Manage Role-based Access with Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span></span>

### <a name="azure-multi-factor-authentication"></a><span data-ttu-id="72d0f-161">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="72d0f-161">Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="72d0f-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) je řešení dvoustupňové ověření, které pomáhá chránit přístup toodata a aplikace, při splnění požadavků uživatelů pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="72d0f-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) is a two-step verification solution that helps safeguard access toodata and applications, while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="72d0f-163">Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="72d0f-163">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

<span data-ttu-id="72d0f-164">toodeploy MFA v cloudu Azure hello, budete potřebovat toofirst ji povolit a potom zapnout dvoustupňové ověřování pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="72d0f-164">toodeploy MFA in hello Azure cloud, you need toofirst enable it and then turn on two-step verification for users.</span></span>

#### <a name="how-do-i-enable-azure-toouse-mfa"></a><span data-ttu-id="72d0f-165">Povolení Azure toouse MFA?</span><span class="sxs-lookup"><span data-stu-id="72d0f-165">How do I enable Azure toouse MFA?</span></span>

<span data-ttu-id="72d0f-166">Pokud mají vaši uživatelé licencí, které zahrnují Azure Multi-Factor Authentication, není nic, je nutné, aby toodo tooturn na Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="72d0f-166">If your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need toodo tooturn on Azure MFA.</span></span> <span data-ttu-id="72d0f-167">Pokud ne, je nutné toocreate poskytovatele vícefaktorového ověřování ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="72d0f-167">If not, you need toocreate a Multi-Factor Auth provider in your directory.</span></span> <span data-ttu-id="72d0f-168">toodo, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="72d0f-168">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="72d0f-169">Vyberte **služby Active Directory** v hello portál Azure classic (přihlášeni jako správce).</span><span class="sxs-lookup"><span data-stu-id="72d0f-169">Select **Active Directory** in hello Azure classic portal (logged on as an administrator).</span></span>

2. <span data-ttu-id="72d0f-170">Vyberte **poskytovatelé služby Multi-Factor Authentication.**</span><span class="sxs-lookup"><span data-stu-id="72d0f-170">Select **Multi-Factor Authentication Providers.**</span></span>

3. <span data-ttu-id="72d0f-171">Vyberte **nový,** a potom v části **aplikační služby,** vyberte **zprostředkovatel vícefaktorového ověřování.**</span><span class="sxs-lookup"><span data-stu-id="72d0f-171">Select **New,** and then under **App Services,** select **Multi-Factor Auth Provider.**</span></span>

4. <span data-ttu-id="72d0f-172">Vyberte **rychle vytvořit.**</span><span class="sxs-lookup"><span data-stu-id="72d0f-172">Select **Quick Create.**</span></span>

5. <span data-ttu-id="72d0f-173">Vyplňte pole s názvem hello a vyberte modelu využití (za ověření nebo za povoleného uživatele).</span><span class="sxs-lookup"><span data-stu-id="72d0f-173">Fill in hello name field and select a usage model (per authentication or per enabled user).</span></span>

6. <span data-ttu-id="72d0f-174">Určíte adresář, ke které hello je přiřazeno zprostředkovatele vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="72d0f-174">Designate a directory with which hello MFA Provider is associated.</span></span>

7. <span data-ttu-id="72d0f-175">Klikněte na tlačítko hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="72d0f-175">Click hello **Create** button.</span></span>

![](media/protect-personal-data-identity-access-controls/quick-create.png)

<span data-ttu-id="72d0f-176">Další pokyny toomanage vaše zprostředkovatel vícefaktorového ověřování najdete v části [Začínáme s poskytovatele Azure Multi-Factor Auth.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span><span class="sxs-lookup"><span data-stu-id="72d0f-176">For more instructions on how toomanage your Multi-Factor Auth Provider, see [Getting Started with an Azure Multi-Factor Auth Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span></span>

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a><span data-ttu-id="72d0f-177">Jak lze zapnout dvoustupňové ověřování pro uživatele?</span><span class="sxs-lookup"><span data-stu-id="72d0f-177">How do I turn on two-step verification for users?</span></span>

<span data-ttu-id="72d0f-178">Můžete vynutit dvoustupňové ověřování pro všechny přihlášení nebo podmíněného přístupu zásady toorequire dvoustupňové ověření můžete vytvořit pouze v případě, že určité podmínky.</span><span class="sxs-lookup"><span data-stu-id="72d0f-178">You can enforce two-step verification for all sign-ins, or you can create conditional access policies toorequire two-step verification only when specific conditions apply.</span></span>

<span data-ttu-id="72d0f-179">Povolení Azure MFA změnou stavů uživatele je hello tradiční přístup pro vyžádání dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="72d0f-179">Enabling Azure MFA by changing user states is hello traditional approach for requiring two-step verification.</span></span> <span data-ttu-id="72d0f-180">Budou mít všichni uživatelé hello, které povolíte hello stejné požadavek tooperform dvoustupňové ověřování při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="72d0f-180">All hello users that you enable will have hello same requirement tooperform two-step verification every time they sign in.</span></span> <span data-ttu-id="72d0f-181">Povolení uživatele potlačí všechny zásady podmíněného přístupu, které může mít vliv na tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="72d0f-181">Enabling a user overrides any conditional access policies that may affect that user.</span></span>

<span data-ttu-id="72d0f-182">Povolení Azure MFA se zásadami podmíněného přístupu je flexibilnější přístup pro vyžádání dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="72d0f-182">Enabling Azure MFA with a conditional access policy is a more flexible approach for requiring two-step verification.</span></span> <span data-ttu-id="72d0f-183">Můžete vytvořit zásady podmíněného přístupu, které se vztahují toogroups, jakož i jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="72d0f-183">You can create conditional access policies that apply toogroups as well as individual users.</span></span> <span data-ttu-id="72d0f-184">S vysokým rizikem skupiny je možné přidělit další omezení než nízkým rizikem skupiny nebo dvoustupňové ověřování můžete vyžaduje se jenom pro vysoce rizikové cloudové aplikace a pro ty, které jsou nízkým rizikem přeskočeno.</span><span class="sxs-lookup"><span data-stu-id="72d0f-184">High-risk groups can be given more restrictions than low-risk groups, or two-step verification can be required only for high-risk cloud apps and skipped for low-risk ones.</span></span> <span data-ttu-id="72d0f-185">Podmíněný přístup je však placené funkce služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="72d0f-185">However, conditional access is a paid feature of Azure Active Directory.</span></span>

<span data-ttu-id="72d0f-186">tooenable MFA změnou stavu uživatele, hello následující:</span><span class="sxs-lookup"><span data-stu-id="72d0f-186">tooenable MFA by changing user state, do hello following:</span></span>

1. <span data-ttu-id="72d0f-187">Přihlaste se toohello portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="72d0f-187">Sign in toohello Azure portal as an administrator.</span></span>
2. <span data-ttu-id="72d0f-188">Přejděte příliš**Azure Active Directory \> uživatelů a skupin \> všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-188">Go too**Azure Active Directory \> Users and groups \> All users**.</span></span>
3. <span data-ttu-id="72d0f-189">Vyberte **služby Multi-Factor Authentication**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-189">Select **Multi-Factor Authentication**.</span></span>
4. <span data-ttu-id="72d0f-190">Najít hello uživatele, že chcete tooenable pro Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="72d0f-190">Find hello user that you want tooenable for Azure MFA.</span></span> <span data-ttu-id="72d0f-191">Může být nutné toochange hello zobrazení v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="72d0f-191">You may need toochange hello view at hello top.</span></span>
5. <span data-ttu-id="72d0f-192">Zkontrolujte název hello pole Další toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="72d0f-192">Check hello box next toohello user’s name.</span></span>
6. <span data-ttu-id="72d0f-193">Na pravé straně v části Rychlé kroky hello, zvolte **povolit**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-193">On hello right, under quick steps, choose **Enable**.</span></span>

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. <span data-ttu-id="72d0f-194">Potvrďte výběr v hello místní okno, které se otevře.</span><span class="sxs-lookup"><span data-stu-id="72d0f-194">Confirm your selection in hello pop-up window that opens.</span></span>  <span data-ttu-id="72d0f-195">Uživatelé, pro které bylo povoleno vícefaktorové ověřování vyzve tooregister hello při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="72d0f-195">Users for whom MFA has been enabled will be asked tooregister hello next time they sign in.</span></span>

<span data-ttu-id="72d0f-196">tooenable Azure MFA se zásadami podmíněného přístupu, hello následující:</span><span class="sxs-lookup"><span data-stu-id="72d0f-196">tooenable Azure MFA with a conditional access policy, do hello following:</span></span>

1. <span data-ttu-id="72d0f-197">Přihlaste se toohello portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="72d0f-197">Sign in toohello Azure portal as an administrator.</span></span>

2. <span data-ttu-id="72d0f-198">Přejděte příliš**Azure Active Directory \> podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-198">Go too**Azure Active Directory \> Conditional access**.</span></span>

3. <span data-ttu-id="72d0f-199">Vyberte **nové zásady**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-199">Select **New policy**.</span></span>

4. <span data-ttu-id="72d0f-200">V části **přiřazení**, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-200">Under **Assignments**, select **Users and groups**.</span></span> <span data-ttu-id="72d0f-201">Použití hello **zahrnout** a **vyloučit** karty toospecify, kteří uživatelé a skupiny bude spravovat zásady hello.</span><span class="sxs-lookup"><span data-stu-id="72d0f-201">Use hello **Include** and     **Exclude** tabs toospecify which users and groups will be managed by hello policy.</span></span>

5. <span data-ttu-id="72d0f-202">V části **přiřazení**, vyberte **cloudových aplikací.**</span><span class="sxs-lookup"><span data-stu-id="72d0f-202">Under **Assignments**, select **Cloud apps.**</span></span> <span data-ttu-id="72d0f-203">Zvolte příliš**zahrnují všechny cloudové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-203">Choose too**include All cloud apps**.</span></span>
6.  <span data-ttu-id="72d0f-204">V části **přístup k ovládacím prvkům**, vyberte **Grant**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-204">Under **Access controls**, select **Grant**.</span></span> <span data-ttu-id="72d0f-205">Zvolte **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-205">Choose **Require multi-factor authentication**.</span></span>
7.  <span data-ttu-id="72d0f-206">Zapnout **povolit zásady** příliš**na** a pak vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="72d0f-206">Turn **Enable policy** too**On** and then select **Save**.</span></span>

<span data-ttu-id="72d0f-207">Informace o způsobu vytvoření tooset nastavení Azure MFA tooconfigure si upozornění na podvod, jednorázové přihlášení, použít vlastní hlasové zprávy, konfigurovat ukládání do mezipaměti, zadejte důvěryhodné IP adresy, vytvoření hesel aplikací, povolte zapamatování vícefaktorového ověřování pro zařízení, které důvěřují uživatele, vyberte metody ověřování, najdete v části [konfigurace nastavení Azure Multi-Factor Authentication.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span><span class="sxs-lookup"><span data-stu-id="72d0f-207">For information on how tooconfigure Azure MFA settings tooset up fraud alerts, create a one-time bypass, use custom voice messages, configure caching, specify trusted IPs, create app passwords, enable remembering MFA for devices that users trust, and select verification methods, see [Configure Azure Multi-Factor Authentication Settings.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span></span>

## <a name="next-steps"></a><span data-ttu-id="72d0f-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72d0f-208">Next steps</span></span>

- [<span data-ttu-id="72d0f-209">Zabezpečení privilegovaného přístupu ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="72d0f-209">Securing privileged access in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [<span data-ttu-id="72d0f-210">Nejčastější dotazy ohledně služby Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="72d0f-210">Frequently asked questions about Azure Multi-Factor Authentication</span></span>](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [<span data-ttu-id="72d0f-211">Na základě rolí řešení potíží s řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="72d0f-211">Role-based Access Control troubleshooting</span></span>](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [<span data-ttu-id="72d0f-212">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="72d0f-212">Azure Active Directory Identity Protection</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
