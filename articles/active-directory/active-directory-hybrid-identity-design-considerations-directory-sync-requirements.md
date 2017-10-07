---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity – určení požadavků na synchronizaci adresáře | Microsoft Docs"
description: "Určit, jaké požadavky jsou nezbytné k synchronizaci všech uživatelů hello mezi na = místní a cloud pro hello enterprise."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6646e3792c65f37c3d62eecdb6c6f3bd257f04f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-directory-synchronization-requirements"></a><span data-ttu-id="6fc2b-103">Určení požadavků na synchronizaci adresáře</span><span class="sxs-lookup"><span data-stu-id="6fc2b-103">Determine directory synchronization requirements</span></span>
<span data-ttu-id="6fc2b-104">Synchronizace slouží k poskytování identity v cloudu hello na základě jejich identity místní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-104">Synchronization is all about providing users an identity in hello cloud based on their on-premises identity.</span></span> <span data-ttu-id="6fc2b-105">Zda se bude používat synchronizované účet pro ověřování nebo federovaného ověřování, hello uživatelé budou potřebovat stále toohave identity v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-105">Whether or not they will use synchronized account for authentication or federated authentication, hello users will still need toohave an identity in hello cloud.</span></span>  <span data-ttu-id="6fc2b-106">Tato identita potřebovat toobe udržuje a pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-106">This identity will need toobe maintained and updated periodically.</span></span>  <span data-ttu-id="6fc2b-107">aktualizace Hello mohou mít mnoho forem, od title změny toopassword změny.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-107">hello updates can take many forms, from title changes toopassword changes.</span></span>  

<span data-ttu-id="6fc2b-108">Spusťte vyhodnocením hello organizace místní řešení a uživatelské požadavky identity.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-108">Start by evaluating hello organizations on-premises identity solution and user requirements.</span></span> <span data-ttu-id="6fc2b-109">Toto testování je důležité toodefine hello technické požadavky na tom, jak bude vytvářené a udržované v cloudu hello identity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-109">This evaluation is important toodefine hello technical requirements for how user identities will be created and maintained in hello cloud.</span></span>  <span data-ttu-id="6fc2b-110">Pro většinu organizací je místní služba Active Directory a hello místní adresář, který uživatelé budou podle synchronizovány z, ale v některých případech to nebudou hello případ.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-110">For a majority of organizations, Active Directory is on-premises and will be hello on-premises directory that users will by synchronized from, however in some cases this will not be hello case.</span></span>  

<span data-ttu-id="6fc2b-111">Ujistěte se, zda text hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="6fc2b-111">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="6fc2b-112">Máte jednu doménovou strukturu AD, několik nebo žádné?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-112">Do you have one AD forest, multiple, or none?</span></span>
  
  * <span data-ttu-id="6fc2b-113">Kolik adresáře Azure AD bude můžete být synchronizaci?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-113">How many Azure AD directories will you be synchronizing to?</span></span>
    
    1. <span data-ttu-id="6fc2b-114">Používáte filtrování?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-114">Are you using filtering?</span></span>
    2. <span data-ttu-id="6fc2b-115">Máte více serverů Azure AD Connect plánované?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-115">Do you have multiple Azure AD Connect servers planned?</span></span>
* <span data-ttu-id="6fc2b-116">Aktuálně máte synchronizace nástroje místně?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-116">Do you currently have a synchronization tool on-premises?</span></span>
  
  * <span data-ttu-id="6fc2b-117">Pokud ano, podporuje vaši uživatelé, pokud uživatelé používají virtuální adresář/integrace identit?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-117">If yes, does your users if users have a virtual directory/integration of identities?</span></span>
* <span data-ttu-id="6fc2b-118">Máte k dispozici žádné jiné na místní adresáře, které chcete toosynchronize (například adresář LDAP, databáze HR atd)?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-118">Do you have any other directory on-premises that you want toosynchronize (e.g. LDAP Directory, HR database, etc)?</span></span>
  * <span data-ttu-id="6fc2b-119">Budete to žádné GALSync toobe?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-119">Are you going toobe doing any GALSync?</span></span>
  * <span data-ttu-id="6fc2b-120">Co je hello aktuální stav UPN ve vaší organizaci?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-120">What is hello current state of UPNs in your organization?</span></span> 
  * <span data-ttu-id="6fc2b-121">Máte na jiný adresář, který uživatelé ověřování proti?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-121">Do you have a different directory that users authenticate against?</span></span>
  * <span data-ttu-id="6fc2b-122">Používá vaše společnost Microsoft Exchange?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-122">Does your company use Microsoft Exchange?</span></span>
    * <span data-ttu-id="6fc2b-123">Jejich plánování toho, že hybridní nasazení systému exchange?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-123">Do they plan of having a hybrid exchange deployment?</span></span>

<span data-ttu-id="6fc2b-124">Teď, když máte představu o synchronizačních požadavcích, musíte toodetermine, který nástroj se správnou jeden toomeet hello tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-124">Now that you have an idea about your synchronization requirements, you need toodetermine which tool is hello correct one toomeet these requirements.</span></span>  <span data-ttu-id="6fc2b-125">Společnost Microsoft poskytuje několik nástrojů pro integraci adresáře tooaccomplish a synchronizace.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-125">Microsoft provides several tools tooaccomplish directory integration and synchronization.</span></span>  <span data-ttu-id="6fc2b-126">V tématu hello [hybridní identita directory integrace nástrojů srovnávací tabulka](active-directory-hybrid-identity-design-considerations-tools-comparison.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-126">See hello [Hybrid Identity directory integration tools comparison table](active-directory-hybrid-identity-design-considerations-tools-comparison.md) for more information.</span></span> 

<span data-ttu-id="6fc2b-127">Nyní když máte požadavků na synchronizaci a hello nástroj, který bude dosáhnout vaší společnosti, musíte tooevaluate hello aplikace, které používají tyto služby adresáře.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-127">Now that you have your synchronization requirements and hello tool that will accomplish this for your company, you need tooevaluate hello applications that use these directory services.</span></span> <span data-ttu-id="6fc2b-128">Toto testování je důležité toodefine hello toointegrate technické požadavky těchto toohello cloudové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-128">This evaluation is important toodefine hello technical requirements toointegrate these applications toohello cloud.</span></span> <span data-ttu-id="6fc2b-129">Ujistěte se, zda text hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="6fc2b-129">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="6fc2b-130">Budou tyto aplikace přesunutý toohello cloudu a použít hello adresář?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-130">Will these applications be moved toohello cloud and use hello directory?</span></span>
* <span data-ttu-id="6fc2b-131">Jsou nějaké speciální atributy, které potřebují toobe synchronizované toohello cloudu, aby se tyto aplikace můžete úspěšně používat?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-131">Are there special attributes that need toobe synchronized toohello cloud so these applications can use them successfully?</span></span>
* <span data-ttu-id="6fc2b-132">Tyto aplikace potřebovat toobe znovu zapsat tootake výhod cloudové ověřování?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-132">Will these applications need toobe re-written tootake advantage of cloud auth?</span></span>
* <span data-ttu-id="6fc2b-133">Tyto aplikace budou toolive místní při uživatelé přistupovat k nim pomocí hello cloudové identity?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-133">Will these applications continue toolive on-premises while users access them using hello cloud identity?</span></span>

<span data-ttu-id="6fc2b-134">Budete také potřebovat toodetermine hello zabezpečení požadavky a omezení synchronizace adresářů.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-134">You also need toodetermine hello security requirements and constraints directory synchronization.</span></span> <span data-ttu-id="6fc2b-135">Toto testování je důležité tooget seznam hello požadavků, které bude potřeba v pořadí toocreate a udržovat identity uživatele v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-135">This evaluation is important tooget a list of hello requirements that will be needed in order toocreate and maintain user’s identities in hello cloud.</span></span> <span data-ttu-id="6fc2b-136">Ujistěte se, zda text hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="6fc2b-136">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="6fc2b-137">Kde budou serveru pro synchronizaci hello umístěné?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-137">Where will hello synchronization server be located?</span></span>
* <span data-ttu-id="6fc2b-138">Bude se připojený k doméně?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-138">Will it be domain joined?</span></span>
* <span data-ttu-id="6fc2b-139">Bude hello server nacházet v omezené síti za bránou firewall, jako je například DMZ?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-139">Will hello server be located on a restricted network behind a firewall, such as a DMZ?</span></span>
  * <span data-ttu-id="6fc2b-140">Bude moct tooopen hello vyžaduje brány firewall porty toosupport synchronizace?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-140">Will you be able tooopen hello required firewall ports toosupport synchronization?</span></span>
* <span data-ttu-id="6fc2b-141">Máte v plánu zotavení po havárii pro server synchronizace hello?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-141">Do you have a disaster recovery plan for hello synchronization server?</span></span>
* <span data-ttu-id="6fc2b-142">Máte účet s hello správná oprávnění pro všechny doménové struktury, které chcete toosynch s?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-142">Do you have an account with hello correct permissions for all forests you want toosynch with?</span></span>
  * <span data-ttu-id="6fc2b-143">Pokud vaše společnost neví hello odpověď pro tuto otázku, projděte si část hello "Oprávnění pro synchronizaci hesel" v článku hello [hello instalace služby Azure Active Directory Sync](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) a zjistit, pokud již máte účet se tato oprávnění nebo pokud potřebujete toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-143">If your company doesn’t know hello answer for this question, review hello section “Permissions for password synchronization” in hello article [Install hello Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) and determine if you already have an account with these permissions or if you need toocreate one.</span></span>
* <span data-ttu-id="6fc2b-144">Pokud máte doménovou strukturou třeba synchronizace je doménové struktury hello synchronizační server dokáže tooget tooeach?</span><span class="sxs-lookup"><span data-stu-id="6fc2b-144">If you have mutli-forest sync is hello sync server able tooget tooeach forest?</span></span>

> [!NOTE]
> <span data-ttu-id="6fc2b-145">Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-145">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="6fc2b-146">[Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) budou přenášeny po hello možnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-146">[Determine incident response requirements](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) will go over hello options available.</span></span> <span data-ttu-id="6fc2b-147">Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.</span><span class="sxs-lookup"><span data-stu-id="6fc2b-147">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6fc2b-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fc2b-148">Next steps</span></span>
[<span data-ttu-id="6fc2b-149">Stanovení požadavků na službu Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="6fc2b-149">Determine multi-factor authentication requirements</span></span>](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a><span data-ttu-id="6fc2b-150">Viz také</span><span class="sxs-lookup"><span data-stu-id="6fc2b-150">See also</span></span>
[<span data-ttu-id="6fc2b-151">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="6fc2b-151">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

