---
title: "Co je Samoobslužná registrace pro Azure? | Dokumentace Microsoftu"
description: "Přehled samoobslužné registrace pro Azure, jak spravovat proces registrace a jak převzít kontrolu nad název domény DNS."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: de8b55516ae7e19f2b42466fa4dc387b82495a5b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-self-service-signup-for-azure"></a><span data-ttu-id="5b92e-104">Co je Samoobslužná registrace pro Azure?</span><span class="sxs-lookup"><span data-stu-id="5b92e-104">What is Self-Service Signup for Azure?</span></span>
<span data-ttu-id="5b92e-105">Toto téma vysvětluje proces samoobslužné registrace a postup převzít kontrolu nad název domény DNS.</span><span class="sxs-lookup"><span data-stu-id="5b92e-105">This topic explains the self-service signup process and how to take over a DNS domain name.</span></span>  

## <a name="why-use-self-service-signup"></a><span data-ttu-id="5b92e-106">Proč používat samoobslužné registrace?</span><span class="sxs-lookup"><span data-stu-id="5b92e-106">Why use self-service signup?</span></span>
* <span data-ttu-id="5b92e-107">Získejte zákazníků ke službám, které mají být rychlejší.</span><span class="sxs-lookup"><span data-stu-id="5b92e-107">Get customers to services they want faster.</span></span>
* <span data-ttu-id="5b92e-108">Vytvořte e mailové nabídky pro službu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-108">Create email-based offers for a service.</span></span>
* <span data-ttu-id="5b92e-109">Vytvořte e mailové registrace toky, které rychle povolit uživatelům vytvářet identit pomocí jejich aliasy snadno zapamatovat pracovní e-mailu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-109">Create email-based signup flows which quickly allow users to create identities using their easy-to-remember work email aliases.</span></span>
* <span data-ttu-id="5b92e-110">Nespravované Azure adresáře mohou být provedeny později do spravovaných adresáře a znovu použít pro jiné služby.</span><span class="sxs-lookup"><span data-stu-id="5b92e-110">Unmanaged Azure directories can be made into managed directories later and be reused for other services.</span></span>

## <a name="terms-and-definitions"></a><span data-ttu-id="5b92e-111">Termíny a definice</span><span class="sxs-lookup"><span data-stu-id="5b92e-111">Terms and Definitions</span></span>
* <span data-ttu-id="5b92e-112">**Samoobslužná registrace do**: Toto je metoda, podle kterého uživatel zaregistruje cloudovou službu a má identitu automaticky vytvoří pro ně v Azure Active Directory (Azure AD), na základě jejich e-mailovou doménu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-112">**Self-service sign up**: This is the method by which a user signs up for a cloud service and has an identity automatically created for them in Azure Active Directory (Azure AD) based on their email domain.</span></span>
* <span data-ttu-id="5b92e-113">**Nespravované Azure directory**: Toto je adresář, kde se má vytvořit danou identitu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-113">**Unmanaged Azure directory**: This is the directory where that identity is created.</span></span> <span data-ttu-id="5b92e-114">Nespravované adresáře se adresář, který má žádný globální správce.</span><span class="sxs-lookup"><span data-stu-id="5b92e-114">An unmanaged directory is a directory that has no global administrator.</span></span>
* <span data-ttu-id="5b92e-115">**Ověřit e-mailu uživatel**: Jedná se o typ uživatelského účtu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5b92e-115">**Email-verified user**: This is a type of user account in Azure AD.</span></span> <span data-ttu-id="5b92e-116">Uživatel, který má identity vytvoří automaticky po registraci na nabídku samoobslužné služby se označuje jako uživatel s ověřenou e-mailu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-116">A user who has an identity created automatically after signing up for a self-service offer is known as an email-verified user.</span></span> <span data-ttu-id="5b92e-117">Ověření e-mailu uživatel je členem regulární adresáře označené creationmethod = EmailVerified.</span><span class="sxs-lookup"><span data-stu-id="5b92e-117">An email-verified user is a regular member of a directory tagged with creationmethod=EmailVerified.</span></span>

## <a name="user-experience"></a><span data-ttu-id="5b92e-118">Činnost koncového uživatele</span><span class="sxs-lookup"><span data-stu-id="5b92e-118">User experience</span></span>
<span data-ttu-id="5b92e-119">Předpokládejme například, že uživatele, jejichž e-mailu je Dan@BellowsCollege.com obdrží citlivé soubory e-mailem.</span><span class="sxs-lookup"><span data-stu-id="5b92e-119">For example, let's say a user whose email is Dan@BellowsCollege.com receives sensitive files via email.</span></span> <span data-ttu-id="5b92e-120">Soubory chráněné pomocí Azure Rights Management (Azure RMS).</span><span class="sxs-lookup"><span data-stu-id="5b92e-120">The files have been protected by Azure Rights Management (Azure RMS).</span></span> <span data-ttu-id="5b92e-121">Ale Dana pro organizaci, univerzity, kterou vlnovce, nebyl zaregistrovali do služby Azure RMS, ani nasadil Active Directory RMS.</span><span class="sxs-lookup"><span data-stu-id="5b92e-121">But Dan's organization, Bellows College, has not signed up for Azure RMS, nor has it deployed Active Directory RMS.</span></span> <span data-ttu-id="5b92e-122">V takovém případě Dana můžete si zaregistrovat bezplatné předplatné RMS pro jednotlivce aby bylo možné číst chráněné soubory.</span><span class="sxs-lookup"><span data-stu-id="5b92e-122">In this case, Dan can sign up for a free subscription to RMS for individuals in order to read the protected files.</span></span>

<span data-ttu-id="5b92e-123">Pokud Dana první uživatele s e-mailovou adresu z BellowsCollege.com zaregistrovat tento samoobslužný nabídky, pak nespravované adresář bude vytvořen pro BellowsCollege.com ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5b92e-123">If Dan is the first user with an email address from BellowsCollege.com to sign up for this self-service offering, then an unmanaged directory will be created for BellowsCollege.com in Azure AD.</span></span> <span data-ttu-id="5b92e-124">Pokud jiných uživatelů z domény BellowsCollege.com registraci této nabídky nebo nabídku podobné samoobslužné služby, budou mít i e-mailu ověřit uživatelské účty vytvořené ve stejném adresáři nespravované v Azure.</span><span class="sxs-lookup"><span data-stu-id="5b92e-124">If other users from the BellowsCollege.com domain sign up for this offering or a similar self-service offering, they will also have email-verified user accounts created in the same unmanaged directory in Azure.</span></span>

## <a name="admin-experience"></a><span data-ttu-id="5b92e-125">Z pohledu správce</span><span class="sxs-lookup"><span data-stu-id="5b92e-125">Admin experience</span></span>
<span data-ttu-id="5b92e-126">Správce, který vlastní název domény DNS adresář nespravované Azure můžete převzít kontrolu nad nebo sloučení adresáři po prokázání vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="5b92e-126">An admin who owns the DNS domain name of an unmanaged Azure directory can take over or merge the directory after proving ownership.</span></span> <span data-ttu-id="5b92e-127">Další části popisují pohledu správce podrobněji, ale tady je Shrnutí:</span><span class="sxs-lookup"><span data-stu-id="5b92e-127">The next sections explain the admin experience in more detail, but here's a summary:</span></span>

* <span data-ttu-id="5b92e-128">Pokud převezmete adresář nespravované Azure, jednoduše stát globální správce adresáře, nespravované.</span><span class="sxs-lookup"><span data-stu-id="5b92e-128">When you take over an unmanaged Azure directory, you simply become the global administrator of the unmanaged directory.</span></span> <span data-ttu-id="5b92e-129">Někdy se označuje jako interní převzetí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-129">This is sometimes called an internal takeover.</span></span>
* <span data-ttu-id="5b92e-130">Pokud sloučení adresář Azure nespravované, přidejte název domény DNS nespravované adresáře do adresáře spravované Azure a vytvoření mapování uživatelů prostředků proto uživatelé nadále pro přístup ke službám bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="5b92e-130">When you merge an unmanaged Azure directory, you add the DNS domain name of the unmanaged directory to your managed Azure directory and a mapping of users-to-resources is created so users can continue to access services without interruption.</span></span> <span data-ttu-id="5b92e-131">Někdy se označuje jako externí převzetí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-131">This is sometimes called an external takeover.</span></span>

## <a name="what-gets-created-in-azure-active-directory"></a><span data-ttu-id="5b92e-132">Co se vytvoří v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5b92e-132">What gets created in Azure Active Directory?</span></span>
#### <a name="directory"></a><span data-ttu-id="5b92e-133">Adresář</span><span class="sxs-lookup"><span data-stu-id="5b92e-133">directory</span></span>
* <span data-ttu-id="5b92e-134">Adresář služby Azure Active Directory pro doménu je vytvořen, jeden adresář v každé doméně.</span><span class="sxs-lookup"><span data-stu-id="5b92e-134">An Azure Active Directory directory for the domain is created, one directory per domain.</span></span>
* <span data-ttu-id="5b92e-135">Adresář Azure AD má žádné globální správce.</span><span class="sxs-lookup"><span data-stu-id="5b92e-135">The Azure AD directory has no global admin.</span></span>

#### <a name="users"></a><span data-ttu-id="5b92e-136">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="5b92e-136">Users</span></span>
* <span data-ttu-id="5b92e-137">Pro každého uživatele, která se zaregistruje je vytvořen objekt uživatele v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5b92e-137">For each user who signs up, a user object is created in the Azure AD directory.</span></span>
* <span data-ttu-id="5b92e-138">Každý objekt uživatele je označena jako externí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-138">Each user object is marked as external.</span></span>
* <span data-ttu-id="5b92e-139">Každý uživatel přístup ke službě, která jejich registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b92e-139">Each user is given access to the service that they signed up for.</span></span>

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a><span data-ttu-id="5b92e-140">Jak deklarace samoobslužné služby Azure AD adresář pro doménu I vlastní?</span><span class="sxs-lookup"><span data-stu-id="5b92e-140">How do I claim a self-service Azure AD directory for a domain I own?</span></span>
<span data-ttu-id="5b92e-141">Samoobslužné služby Azure AD můžete deklarace adresář tak, že provedete ověření domény.</span><span class="sxs-lookup"><span data-stu-id="5b92e-141">You can claim a self-service Azure AD directory by performing domain validation.</span></span> <span data-ttu-id="5b92e-142">Ověření domény prokáže, že jste vlastníkem domény tak, že vytvoříte záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="5b92e-142">Domain validation proves you own the domain by creating DNS records.</span></span>

<span data-ttu-id="5b92e-143">Existují dva způsoby, jak provést převzetí DNS z adresáře služby Azure AD:</span><span class="sxs-lookup"><span data-stu-id="5b92e-143">There are two ways to do a DNS takeover of an Azure AD directory:</span></span>

* <span data-ttu-id="5b92e-144">interní převzetí (Správce zjistí adresář nespravované Azure a chce zapnout do spravovaných adresáře)</span><span class="sxs-lookup"><span data-stu-id="5b92e-144">internal takeover (Admin discovers an unmanaged Azure directory, and wants to turn into a managed directory)</span></span>
* <span data-ttu-id="5b92e-145">externí převzetí (správce pokusí přidat novou doménu do jejich spravované Azure directory)</span><span class="sxs-lookup"><span data-stu-id="5b92e-145">external takeover (Admin tries to add a new domain to their managed Azure directory)</span></span>

<span data-ttu-id="5b92e-146">Může zajímat ověření vlastní domény, protože adresář nespravované jsou převzetí po uživatel provedl samoobslužné registrace, nebo může být přidání nové domény do existujícího spravovaného adresáře.</span><span class="sxs-lookup"><span data-stu-id="5b92e-146">You might be interested in validating that you own a domain because you are taking over an unmanaged directory after a user performed self-service signup, or you might be adding a new domain to an existing managed directory.</span></span> <span data-ttu-id="5b92e-147">Například máte doménu s názvem contoso.com a chcete přidat novou doménu s názvem contoso.co.uk nebo contoso.uk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-147">For example, you have a domain named contoso.com and you want to add a new domain named contoso.co.uk or contoso.uk.</span></span>

## <a name="what-is-domain-takeover"></a><span data-ttu-id="5b92e-148">Co je převzetí domény?</span><span class="sxs-lookup"><span data-stu-id="5b92e-148">What is domain takeover?</span></span>
<span data-ttu-id="5b92e-149">Tato část obsahuje informace o ověření, že jste vlastníkem domény</span><span class="sxs-lookup"><span data-stu-id="5b92e-149">This section covers how to validate that you own a domain</span></span>

### <a name="what-is-domain-validation-and-why-is-it-used"></a><span data-ttu-id="5b92e-150">Co je ověření domény a proč slouží?</span><span class="sxs-lookup"><span data-stu-id="5b92e-150">What is domain validation and why is it used?</span></span>
<span data-ttu-id="5b92e-151">Abyste mohli provádět operace v adresáři, Azure AD vyžaduje, abyste ověřili vlastnictví domény DNS.</span><span class="sxs-lookup"><span data-stu-id="5b92e-151">In order to perform operations on a directory, Azure AD requires that you validate ownership of the DNS domain.</span></span>  <span data-ttu-id="5b92e-152">Ověření domény umožňuje deklarace identity adresář a buď zvýšit úroveň adresář samoobslužné služby, který má spravovaný adresář nebo sloučení adresáři samoobslužných služeb do existujícího spravovaného adresáře.</span><span class="sxs-lookup"><span data-stu-id="5b92e-152">Validation of the domain allows you to claim the directory and either promote the self-service directory to a managed directory, or merge the self-service directory into an existing managed directory.</span></span>

## <a name="examples-of-domain-validation"></a><span data-ttu-id="5b92e-153">Příklady ověření domény</span><span class="sxs-lookup"><span data-stu-id="5b92e-153">Examples of domain validation</span></span>
<span data-ttu-id="5b92e-154">Existují dva způsoby, jak provést převzetí DNS adresáře:</span><span class="sxs-lookup"><span data-stu-id="5b92e-154">There are two ways to do a DNS takeover of a directory:</span></span>

* <span data-ttu-id="5b92e-155">interní převzetí (například správce zjistí adresář samoobslužné služby, nespravované a chce zapnout do spravovaných adresáře)</span><span class="sxs-lookup"><span data-stu-id="5b92e-155">internal takeover  (For example, an admin discovers a self-service, unmanaged directory, and wants to turn into managed directory)</span></span>
* <span data-ttu-id="5b92e-156">externí převzetí (například k oprávnění správce se pokusí přidat novou doménu do spravovaných adresáře)</span><span class="sxs-lookup"><span data-stu-id="5b92e-156">external takeover (For example, a admin tries to add a new domain to a managed directory)</span></span>

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a><span data-ttu-id="5b92e-157">Interní převzetí - podporovat samoobslužné služby, nespravované adresáři, který bude spravovaný adresář</span><span class="sxs-lookup"><span data-stu-id="5b92e-157">Internal Takeover - promote a self-service, unmanaged directory to be a managed directory</span></span>
<span data-ttu-id="5b92e-158">Když provedete interní převzetí, získá adresáři převést z adresář nespravované na spravované adresáře.</span><span class="sxs-lookup"><span data-stu-id="5b92e-158">When you do internal takeover, the directory gets converted from an unmanaged directory to a managed directory.</span></span> <span data-ttu-id="5b92e-159">Je potřeba provést ověření názvu domény DNS, kde v zóně DNS vytvořit záznam MX nebo záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="5b92e-159">You need to complete DNS domain name validation, where you create an MX record or a TXT record in the DNS zone.</span></span> <span data-ttu-id="5b92e-160">Tato akce:</span><span class="sxs-lookup"><span data-stu-id="5b92e-160">That action:</span></span>

* <span data-ttu-id="5b92e-161">Ověří, že jste vlastníkem domény</span><span class="sxs-lookup"><span data-stu-id="5b92e-161">Validates that you own the domain</span></span>
* <span data-ttu-id="5b92e-162">Umožňuje spravovat adresář</span><span class="sxs-lookup"><span data-stu-id="5b92e-162">Makes the directory managed</span></span>
* <span data-ttu-id="5b92e-163">Umožňuje vám globální správce adresáře</span><span class="sxs-lookup"><span data-stu-id="5b92e-163">Makes you the global admin of the directory</span></span>

<span data-ttu-id="5b92e-164">Řekněme, že správce IT z vlnovce univerzity, kterou zjistí, že uživatelé z školy zaregistrovali pro samoobslužné služby nabídky.</span><span class="sxs-lookup"><span data-stu-id="5b92e-164">Let's say an IT administrator from Bellows College discovers that users from the school have signed up for self-service offerings.</span></span> <span data-ttu-id="5b92e-165">Jako registrovaný vlastník DNS název BellowsCollege.com, správce IT může ověřit vlastnictví název DNS v Azure a pak proveďte přes adresáři nespravované.</span><span class="sxs-lookup"><span data-stu-id="5b92e-165">As the registered owner of the DNS name BellowsCollege.com, the IT administrator can validate ownership of the DNS name in Azure and then take over the unmanaged directory.</span></span> <span data-ttu-id="5b92e-166">Adresář se pak stane spravovaný adresář a správce IT má přiřazenou roli globálního správce pro adresář BellowsCollege.com.</span><span class="sxs-lookup"><span data-stu-id="5b92e-166">The directory then becomes a managed directory, and the IT administrator is assigned the global administrator role for the BellowsCollege.com directory.</span></span>

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a><span data-ttu-id="5b92e-167">Externí převzetí - sloučit adresář samoobslužné služby do existující spravované adresář</span><span class="sxs-lookup"><span data-stu-id="5b92e-167">External Takeover - merge a self-service directory into an existing managed directory</span></span>
<span data-ttu-id="5b92e-168">V externí převzetí již máte spravovaný adresář a chcete, aby všichni uživatelé a skupiny z nespravovaných adresáře pro připojení k tomuto adresáři spravované místo vlastní dva samostatné adresáře.</span><span class="sxs-lookup"><span data-stu-id="5b92e-168">In an external takeover, you already have a managed directory and you want all users and groups from an unmanaged directory to join that managed directory, rather than own two separate directories.</span></span>

<span data-ttu-id="5b92e-169">Jako správce adresáře spravované přidání domény a domény se stane, tak, aby měl adresář nespravované s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="5b92e-169">As an admin of a managed directory, you add a domain, and that domain happens to have an unmanaged directory associated with it.</span></span>

<span data-ttu-id="5b92e-170">Řekněme například, jsou správce IT a už máte spravovaný adresář pro doménu Contoso.com, název domény, které je registrované pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="5b92e-170">For example, let's say you are an IT administrator and you already have a managed directory for Contoso.com, a domain name that is registered to your organization.</span></span> <span data-ttu-id="5b92e-171">Zjistíte, že uživatelé z vaší organizace provedli Samoobslužná registrace až pro nabídky pomocí doménového názvu e-mailu user@contoso.co.uk, což je jiný název domény, který vaše organizace vlastní.</span><span class="sxs-lookup"><span data-stu-id="5b92e-171">You discover that users from your organization have performed self-service sign up for an offering by using email domain name user@contoso.co.uk, which is another domain name that your organization owns.</span></span> <span data-ttu-id="5b92e-172">Tito uživatelé nyní mají účty v nespravované adresář pro contoso.co.uk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-172">Those users currently have accounts in an unmanaged directory for contoso.co.uk.</span></span>

<span data-ttu-id="5b92e-173">Nechcete spravovat dva samostatné adresáře, aby sloučit nespravované adresář pro contoso.co.uk do existujícího adresáře spravované IT pro doménu contoso.com.</span><span class="sxs-lookup"><span data-stu-id="5b92e-173">You don't want to manage two separate directories, so you merge the unmanaged directory for contoso.co.uk into your existing IT managed directory for contoso.com.</span></span>

<span data-ttu-id="5b92e-174">Externí převzetí postupuje stejně DNS ověření jako interní převzetí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-174">External takeover follows the same DNS validation process as internal takeover.</span></span>  <span data-ttu-id="5b92e-175">Přičemž rozdíl: uživatelů a služeb jsou mapovány na IT spravovat adresář.</span><span class="sxs-lookup"><span data-stu-id="5b92e-175">Difference being: users and services are remapped to the IT managed directory.</span></span>

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a><span data-ttu-id="5b92e-176">Co je dopad provedení externí převzetí?</span><span class="sxs-lookup"><span data-stu-id="5b92e-176">What's the impact of performing an external takeover?</span></span>
<span data-ttu-id="5b92e-177">S externí převzetí se vytvoří mapování uživatelů prostředků, uživatelé mohou i nadále přístup ke službám bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="5b92e-177">With an external takeover, a mapping of users-to-resources is created so users can continue to access services without interruption.</span></span> <span data-ttu-id="5b92e-178">Mnoho aplikací, včetně služby RMS pro jednotlivce, zpracování a mapování uživatelů prostředků a uživatelé mohou i nadále přístup k těchto služeb bez změn.</span><span class="sxs-lookup"><span data-stu-id="5b92e-178">Many applications, including RMS for individuals, handle the mapping of users-to-resources well, and users can continue to access those services without change.</span></span> <span data-ttu-id="5b92e-179">Pokud aplikace efektivně nezpracovává mapování uživatelů na prostředky, může být externí převzetí explicitně blokována tak, aby uživatelé z nízký prostředí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-179">If an application does not handle the mapping of users-to-resources effectively, external takeover may be explicitly blocked to prevent users from a poor experience.</span></span>

#### <a name="directory-takeover-support-by-service"></a><span data-ttu-id="5b92e-180">Podpora převzetí Directory službou</span><span class="sxs-lookup"><span data-stu-id="5b92e-180">directory takeover support by service</span></span>
<span data-ttu-id="5b92e-181">Následující služby v současné době podporují převzetí:</span><span class="sxs-lookup"><span data-stu-id="5b92e-181">Currently the following services support takeover:</span></span>

* <span data-ttu-id="5b92e-182">RMS</span><span class="sxs-lookup"><span data-stu-id="5b92e-182">RMS</span></span>

<span data-ttu-id="5b92e-183">Následující služby bude brzy k dispozici podpora převzetí:</span><span class="sxs-lookup"><span data-stu-id="5b92e-183">The following services will soon be supporting takeover:</span></span>

* <span data-ttu-id="5b92e-184">PowerBI</span><span class="sxs-lookup"><span data-stu-id="5b92e-184">PowerBI</span></span>

<span data-ttu-id="5b92e-185">Následující nepodporují a vyžadovat další správce akce k migraci dat uživatele po externí převzetí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-185">The following do not and require additional admin action to migrate user data after an external takeover.</span></span>

* <span data-ttu-id="5b92e-186">SharePoint nebo OneDrive</span><span class="sxs-lookup"><span data-stu-id="5b92e-186">SharePoint/OneDrive</span></span>

## <a name="how-to-perform-a-dns-domain-name-takeover"></a><span data-ttu-id="5b92e-187">Postup převzetí název domény DNS</span><span class="sxs-lookup"><span data-stu-id="5b92e-187">How to perform a DNS domain name takeover</span></span>
<span data-ttu-id="5b92e-188">Máte několik možností k provedení ověření domény (a pokud chcete provést převzetí):</span><span class="sxs-lookup"><span data-stu-id="5b92e-188">You have a few options for how to perform a domain validation (and do a takeover if you wish):</span></span>

1. <span data-ttu-id="5b92e-189">Portál pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="5b92e-189">Azure Management Portal</span></span>

   <span data-ttu-id="5b92e-190">Pomocí tohoto postupu přidání domény převzetím aktivaci.</span><span class="sxs-lookup"><span data-stu-id="5b92e-190">A takeover is triggered by doing a domain addition.</span></span>  <span data-ttu-id="5b92e-191">Pokud adresář již existuje pro doménu, budete mít možnost provádět externí převzetí.</span><span class="sxs-lookup"><span data-stu-id="5b92e-191">If a directory already exists for the domain, you'll have the option to perform an external takeover.</span></span>

   <span data-ttu-id="5b92e-192">Přihlaste se k portálu Azure pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5b92e-192">Sign in to the Azure portal using your credentials.</span></span>  <span data-ttu-id="5b92e-193">Přejděte na existující adresář a potom na **přidáním domény**.</span><span class="sxs-lookup"><span data-stu-id="5b92e-193">Navigate to your existing directory and then to **Add domain**.</span></span>
2. <span data-ttu-id="5b92e-194">Office 365</span><span class="sxs-lookup"><span data-stu-id="5b92e-194">Office 365</span></span>

   <span data-ttu-id="5b92e-195">Použijete možnosti na [spravovat domény](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) stránky v Office 365 pro práci s doménách a záznamech DNS.</span><span class="sxs-lookup"><span data-stu-id="5b92e-195">You can use the options on the [Manage domains](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) page in Office 365 to work with your domains and DNS records.</span></span> <span data-ttu-id="5b92e-196">V tématu [ověřte svoji doménu v Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).</span><span class="sxs-lookup"><span data-stu-id="5b92e-196">See [Verify your domain in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).</span></span>
3. <span data-ttu-id="5b92e-197">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="5b92e-197">Windows PowerShell</span></span>

   <span data-ttu-id="5b92e-198">Následující kroky jsou potřebná k provedení ověřování pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5b92e-198">The following steps are required to perform a validation using Windows PowerShell.</span></span>

   | <span data-ttu-id="5b92e-199">Krok</span><span class="sxs-lookup"><span data-stu-id="5b92e-199">Step</span></span> | <span data-ttu-id="5b92e-200">Pomocí rutiny</span><span class="sxs-lookup"><span data-stu-id="5b92e-200">Cmdlet to use</span></span> |
   | --- | --- |
   | <span data-ttu-id="5b92e-201">Vytvořit objekt přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="5b92e-201">Create a credential object</span></span> |<span data-ttu-id="5b92e-202">Get-Credential</span><span class="sxs-lookup"><span data-stu-id="5b92e-202">Get-Credential</span></span> |
   | <span data-ttu-id="5b92e-203">Připojení ke službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="5b92e-203">Connect to Azure AD</span></span> |<span data-ttu-id="5b92e-204">Connect-MsolService</span><span class="sxs-lookup"><span data-stu-id="5b92e-204">Connect-MsolService</span></span> |
   | <span data-ttu-id="5b92e-205">získat seznam domén</span><span class="sxs-lookup"><span data-stu-id="5b92e-205">get a list of domains</span></span> |<span data-ttu-id="5b92e-206">Get-MsolDomain</span><span class="sxs-lookup"><span data-stu-id="5b92e-206">Get-MsolDomain</span></span> |
   | <span data-ttu-id="5b92e-207">Vytvoření výzvu</span><span class="sxs-lookup"><span data-stu-id="5b92e-207">Create a challenge</span></span> |<span data-ttu-id="5b92e-208">Get-MsolDomainVerificationDns</span><span class="sxs-lookup"><span data-stu-id="5b92e-208">Get-MsolDomainVerificationDns</span></span> |
   | <span data-ttu-id="5b92e-209">Vytvořit záznam DNS</span><span class="sxs-lookup"><span data-stu-id="5b92e-209">Create DNS record</span></span> |<span data-ttu-id="5b92e-210">To udělat na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="5b92e-210">Do this on your DNS server</span></span> |
   | <span data-ttu-id="5b92e-211">Ověření výzvy</span><span class="sxs-lookup"><span data-stu-id="5b92e-211">Verify the challenge</span></span> |<span data-ttu-id="5b92e-212">Confirm-MsolEmailVerifiedDomain</span><span class="sxs-lookup"><span data-stu-id="5b92e-212">Confirm-MsolEmailVerifiedDomain</span></span> |

<span data-ttu-id="5b92e-213">Například:</span><span class="sxs-lookup"><span data-stu-id="5b92e-213">For example:</span></span>

1. <span data-ttu-id="5b92e-214">Připojení k Azure AD pomocí přihlašovacích údajů, které jste použili k reagovat na nabídku samoobslužné služby:</span><span class="sxs-lookup"><span data-stu-id="5b92e-214">Connect to Azure AD using the credentials that were used to respond to the self-service offering:</span></span>

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. <span data-ttu-id="5b92e-215">Získáte seznam domén:</span><span class="sxs-lookup"><span data-stu-id="5b92e-215">Get a list of domains:</span></span>

    <span data-ttu-id="5b92e-216">Get-MsolDomain</span><span class="sxs-lookup"><span data-stu-id="5b92e-216">Get-MsolDomain</span></span>
3. <span data-ttu-id="5b92e-217">Poté spusťte rutinu Get-MsolDomainVerificationDns vytvořit výzvu:</span><span class="sxs-lookup"><span data-stu-id="5b92e-217">Then run the Get-MsolDomainVerificationDns cmdlet to create a challenge:</span></span>

    <span data-ttu-id="5b92e-218">Get-MsolDomainVerificationDns – DomainName *your_domain_name* – DnsTxtRecord režimu</span><span class="sxs-lookup"><span data-stu-id="5b92e-218">Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord</span></span>

    <span data-ttu-id="5b92e-219">Například:</span><span class="sxs-lookup"><span data-stu-id="5b92e-219">For example:</span></span>

    <span data-ttu-id="5b92e-220">Get-MsolDomainVerificationDns – DomainName contoso.com – DnsTxtRecord režimu</span><span class="sxs-lookup"><span data-stu-id="5b92e-220">Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord</span></span>
4. <span data-ttu-id="5b92e-221">Zkopírujte hodnotu (výzva), která je vrácena z tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-221">Copy the value (the challenge) that is returned from this command.</span></span>

    <span data-ttu-id="5b92e-222">Například:</span><span class="sxs-lookup"><span data-stu-id="5b92e-222">For example:</span></span>

    <span data-ttu-id="5b92e-223">MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9</span><span class="sxs-lookup"><span data-stu-id="5b92e-223">MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9</span></span>
5. <span data-ttu-id="5b92e-224">V oboru názvů veřejné DNS vytvořte záznam DNS txt, který obsahuje hodnotu, kterou jste zkopírovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="5b92e-224">In your public DNS namespace, create a DNS txt record that contains the value that you copied in the previous step.</span></span>

    <span data-ttu-id="5b92e-225">Název pro tento záznam je název nadřazené domény, tak pokud vytvoříte tento záznam o prostředku pomocí role DNS ze systému Windows Server, ponechte název prázdné, jenom vložení záznamu hodnotu do textového pole</span><span class="sxs-lookup"><span data-stu-id="5b92e-225">The name for this record is the name of the parent domain, so if you create this resource record by using the DNS role from Windows Server, leave the Record name blank and just paste the value into the Text box</span></span>
6. <span data-ttu-id="5b92e-226">Spusťte rutinu potvrdit MsolDomain ověření výzvy:</span><span class="sxs-lookup"><span data-stu-id="5b92e-226">Run the Confirm-MsolDomain cmdlet to verify the challenge:</span></span>

    <span data-ttu-id="5b92e-227">Potvrďte MsolEmailVerifiedDomain - DomainName *your_domain_name*</span><span class="sxs-lookup"><span data-stu-id="5b92e-227">Confirm-MsolEmailVerifiedDomain -DomainName *your_domain_name*</span></span>

    <span data-ttu-id="5b92e-228">Například:</span><span class="sxs-lookup"><span data-stu-id="5b92e-228">for example:</span></span>

    <span data-ttu-id="5b92e-229">Potvrďte MsolEmailVerifiedDomain - DomainName contoso.com</span><span class="sxs-lookup"><span data-stu-id="5b92e-229">Confirm-MsolEmailVerifiedDomain -DomainName contoso.com</span></span>

<span data-ttu-id="5b92e-230">Úspěšné výzvy se vrátíte do příkazového řádku bez chyby.</span><span class="sxs-lookup"><span data-stu-id="5b92e-230">A successful challenge returns you to the prompt without an error.</span></span>

## <a name="how-do-i-control-self-service-settings"></a><span data-ttu-id="5b92e-231">Jak řídit nastavení samoobslužné služby?</span><span class="sxs-lookup"><span data-stu-id="5b92e-231">How do I control self-service settings?</span></span>
<span data-ttu-id="5b92e-232">Správci mají dvou ovládacích prvků samoobslužné služby ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="5b92e-232">Admins have two self-service controls today.</span></span> <span data-ttu-id="5b92e-233">Kontroly nad:</span><span class="sxs-lookup"><span data-stu-id="5b92e-233">They can control:</span></span>

* <span data-ttu-id="5b92e-234">Zda uživatelé mohou připojit adresáři e-mailem.</span><span class="sxs-lookup"><span data-stu-id="5b92e-234">Whether or not users can join the directory via email.</span></span>
* <span data-ttu-id="5b92e-235">Zda uživatelé mohou licence sami pro aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="5b92e-235">Whether or not users can license themselves for applications and services.</span></span>

### <a name="how-can-i-control-these-capabilities"></a><span data-ttu-id="5b92e-236">Jak můžete řídit tyto funkce?</span><span class="sxs-lookup"><span data-stu-id="5b92e-236">How can I control these capabilities?</span></span>
<span data-ttu-id="5b92e-237">Správce můžete nakonfigurovat tyto možnosti, pomocí těchto parametrů rutiny Set-MsolCompanySettings Azure AD:</span><span class="sxs-lookup"><span data-stu-id="5b92e-237">An admin can configure these capabilities using these Azure AD cmdlet Set-MsolCompanySettings parameters:</span></span>

* <span data-ttu-id="5b92e-238">**AllowEmailVerifiedUsers** Určuje, jestli uživatel může vytvořit nebo připojit k nespravované adresáři.</span><span class="sxs-lookup"><span data-stu-id="5b92e-238">**AllowEmailVerifiedUsers** controls whether a user can create or join an unmanaged directory.</span></span> <span data-ttu-id="5b92e-239">Pokud tento parametr nastavte na $false, žádní uživatelé ověřit e-mailu se můžete zapojit do adresáře.</span><span class="sxs-lookup"><span data-stu-id="5b92e-239">If you set that parameter to $false, no email-verified users can join the directory.</span></span>
* <span data-ttu-id="5b92e-240">**AllowAdHocSubscriptions** řídí schopnost uživatelům provádět Samoobslužná registrace nahoru.</span><span class="sxs-lookup"><span data-stu-id="5b92e-240">**AllowAdHocSubscriptions** controls the ability for users to perform self-service sign up.</span></span> <span data-ttu-id="5b92e-241">Pokud tento parametr nastavte na $false, žádní uživatelé provést samoobslužné registrace.</span><span class="sxs-lookup"><span data-stu-id="5b92e-241">If you set that parameter to $false, no users can perform self-service signup.</span></span>

### <a name="how-do-the-controls-work-together"></a><span data-ttu-id="5b92e-242">Jak ovládací prvky spolupracují?</span><span class="sxs-lookup"><span data-stu-id="5b92e-242">How do the controls work together?</span></span>
<span data-ttu-id="5b92e-243">Tyto dva parametry můžete použít ve spojení definovat přesnější kontrolu nad Samoobslužná registrace až.</span><span class="sxs-lookup"><span data-stu-id="5b92e-243">These two parameters can be used in conjunction to define more precise control over self-service sign up.</span></span> <span data-ttu-id="5b92e-244">Například následující příkaz umožní uživatelům proveďte Samoobslužná registrace, ale pouze v případě, že tito uživatelé již máte účet ve službě Azure AD (jinými slovy, uživatele, kteří se potřebují účet ověřit e-mailu, který chcete vytvořit nelze provést Samoobslužná registrace nahoru):</span><span class="sxs-lookup"><span data-stu-id="5b92e-244">For example, the following command will allow users to perform self-service sign up, but only if those users already have an account in Azure AD (in other words, users who would need an email-verified account to be created cannot perform self-service sign up):</span></span>

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

<span data-ttu-id="5b92e-245">Následující vývojový diagram vysvětluje všechny různé kombinace pro tyto parametry a výsledný podmínky k adresáři a Samoobslužná registrace nahoru.</span><span class="sxs-lookup"><span data-stu-id="5b92e-245">The following flowchart explains all the different combinations for these parameters and the resulting conditions for the directory and self-service sign up.</span></span>

![][1]

<span data-ttu-id="5b92e-246">Další informace a příklady použití těchto parametrů najdete v tématu [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span><span class="sxs-lookup"><span data-stu-id="5b92e-246">For more information and examples of how to use these parameters, see [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span></span>

## <a name="see-also"></a><span data-ttu-id="5b92e-247">Viz také</span><span class="sxs-lookup"><span data-stu-id="5b92e-247">See Also</span></span>
* [<span data-ttu-id="5b92e-248">Jak nainstalovat a nakonfigurovat Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5b92e-248">How to install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="5b92e-249">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5b92e-249">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="5b92e-250">Referenční informace k rutinám Azure</span><span class="sxs-lookup"><span data-stu-id="5b92e-250">Azure Cmdlet Reference</span></span>](/powershell/azure/get-started-azureps)
* [<span data-ttu-id="5b92e-251">Set-MsolCompanySettings</span><span class="sxs-lookup"><span data-stu-id="5b92e-251">Set-MsolCompanySettings</span></span>](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
