---
title: "Azure Active Directory hybridní identity aspekty návrhu - určení požadavků na synchronizaci adresáře | Microsoft Docs"
description: "Určit, jaké požadavky jsou nezbytné k synchronizaci všech uživatelů mezi na = místní a cloudové pro podnik."
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
ms.openlocfilehash: 5ef87e606f055359ca325befd6048353ce57ca2b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="determine-directory-synchronization-requirements"></a><span data-ttu-id="f488b-103">Určení požadavků na synchronizaci adresáře</span><span class="sxs-lookup"><span data-stu-id="f488b-103">Determine directory synchronization requirements</span></span>
<span data-ttu-id="f488b-104">Synchronizace slouží k poskytování identity v cloudu na základě jejich identity místní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="f488b-104">Synchronization is all about providing users an identity in the cloud based on their on-premises identity.</span></span> <span data-ttu-id="f488b-105">Zda se bude používat synchronizované účet pro ověřování nebo federovaného ověřování, uživatelé budou stále musí být identity v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f488b-105">Whether or not they will use synchronized account for authentication or federated authentication, the users will still need to have an identity in the cloud.</span></span>  <span data-ttu-id="f488b-106">Tato identita muset udržuje a pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="f488b-106">This identity will need to be maintained and updated periodically.</span></span>  <span data-ttu-id="f488b-107">Aktualizace můžete mít mnoho forem, od title změny změny hesla.</span><span class="sxs-lookup"><span data-stu-id="f488b-107">The updates can take many forms, from title changes to password changes.</span></span>  

<span data-ttu-id="f488b-108">Spusťte vyhodnocením organizace místní identity řešení a uživatelské požadavky.</span><span class="sxs-lookup"><span data-stu-id="f488b-108">Start by evaluating the organizations on-premises identity solution and user requirements.</span></span> <span data-ttu-id="f488b-109">Toto testování je důležité určit technické požadavky na tom, jak bude identit uživatelů vytvářené a udržované v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f488b-109">This evaluation is important to define the technical requirements for how user identities will be created and maintained in the cloud.</span></span>  <span data-ttu-id="f488b-110">Pro většinu organizací je místní služba Active Directory a místní adresář, který uživatelé budou podle synchronizovány z, ale v některých případech to nebude tak.</span><span class="sxs-lookup"><span data-stu-id="f488b-110">For a majority of organizations, Active Directory is on-premises and will be the on-premises directory that users will by synchronized from, however in some cases this will not be the case.</span></span>  

<span data-ttu-id="f488b-111">Ujistěte se, že odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="f488b-111">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="f488b-112">Máte jednu doménovou strukturu AD, několik nebo žádné?</span><span class="sxs-lookup"><span data-stu-id="f488b-112">Do you have one AD forest, multiple, or none?</span></span>
  
  * <span data-ttu-id="f488b-113">Kolik adresáře Azure AD bude můžete být synchronizaci?</span><span class="sxs-lookup"><span data-stu-id="f488b-113">How many Azure AD directories will you be synchronizing to?</span></span>
    
    1. <span data-ttu-id="f488b-114">Používáte filtrování?</span><span class="sxs-lookup"><span data-stu-id="f488b-114">Are you using filtering?</span></span>
    2. <span data-ttu-id="f488b-115">Máte více serverů Azure AD Connect plánované?</span><span class="sxs-lookup"><span data-stu-id="f488b-115">Do you have multiple Azure AD Connect servers planned?</span></span>
* <span data-ttu-id="f488b-116">Aktuálně máte synchronizace nástroje místně?</span><span class="sxs-lookup"><span data-stu-id="f488b-116">Do you currently have a synchronization tool on-premises?</span></span>
  
  * <span data-ttu-id="f488b-117">Pokud ano, podporuje vaši uživatelé, pokud uživatelé používají virtuální adresář/integrace identit?</span><span class="sxs-lookup"><span data-stu-id="f488b-117">If yes, does your users if users have a virtual directory/integration of identities?</span></span>
* <span data-ttu-id="f488b-118">Máte k dispozici žádné jiné na místní adresáře, který chcete synchronizovat (například adresář LDAP, databáze HR atd)?</span><span class="sxs-lookup"><span data-stu-id="f488b-118">Do you have any other directory on-premises that you want to synchronize (e.g. LDAP Directory, HR database, etc)?</span></span>
  * <span data-ttu-id="f488b-119">Chystáte se to žádné GALSync?</span><span class="sxs-lookup"><span data-stu-id="f488b-119">Are you going to be doing any GALSync?</span></span>
  * <span data-ttu-id="f488b-120">Co je aktuální stav UPN ve vaší organizaci?</span><span class="sxs-lookup"><span data-stu-id="f488b-120">What is the current state of UPNs in your organization?</span></span> 
  * <span data-ttu-id="f488b-121">Máte na jiný adresář, který uživatelé ověřování proti?</span><span class="sxs-lookup"><span data-stu-id="f488b-121">Do you have a different directory that users authenticate against?</span></span>
  * <span data-ttu-id="f488b-122">Používá vaše společnost Microsoft Exchange?</span><span class="sxs-lookup"><span data-stu-id="f488b-122">Does your company use Microsoft Exchange?</span></span>
    * <span data-ttu-id="f488b-123">Jejich plánování toho, že hybridní nasazení systému exchange?</span><span class="sxs-lookup"><span data-stu-id="f488b-123">Do they plan of having a hybrid exchange deployment?</span></span>

<span data-ttu-id="f488b-124">Nyní když máte představu o synchronizačních požadavcích, musíte určit, který nástroj je správná pro splnění těchto požadavků.</span><span class="sxs-lookup"><span data-stu-id="f488b-124">Now that you have an idea about your synchronization requirements, you need to determine which tool is the correct one to meet these requirements.</span></span>  <span data-ttu-id="f488b-125">Společnost Microsoft poskytuje několik nástrojů k provádění integrace adresáře a synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f488b-125">Microsoft provides several tools to accomplish directory integration and synchronization.</span></span>  <span data-ttu-id="f488b-126">Najdete v článku [hybridní identita directory integrace nástrojů srovnávací tabulka](active-directory-hybrid-identity-design-considerations-tools-comparison.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f488b-126">See the [Hybrid Identity directory integration tools comparison table](active-directory-hybrid-identity-design-considerations-tools-comparison.md) for more information.</span></span> 

<span data-ttu-id="f488b-127">Nyní když máte synchronizačních požadavcích a nástroj, který bude dosáhnout vaší společnosti, budete muset vyhodnocování aplikací, které používají tyto služby directory.</span><span class="sxs-lookup"><span data-stu-id="f488b-127">Now that you have your synchronization requirements and the tool that will accomplish this for your company, you need to evaluate the applications that use these directory services.</span></span> <span data-ttu-id="f488b-128">Toto testování je důležité určit požadavky na technické integrovat tyto aplikace do cloudu.</span><span class="sxs-lookup"><span data-stu-id="f488b-128">This evaluation is important to define the technical requirements to integrate these applications to the cloud.</span></span> <span data-ttu-id="f488b-129">Ujistěte se, že odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="f488b-129">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="f488b-130">Tyto aplikace se přesune do cloudu a používat adresář?</span><span class="sxs-lookup"><span data-stu-id="f488b-130">Will these applications be moved to the cloud and use the directory?</span></span>
* <span data-ttu-id="f488b-131">Jsou nějaké speciální atributy, které je nutné synchronizovat do cloudu, můžete tyto aplikace použít úspěšně?</span><span class="sxs-lookup"><span data-stu-id="f488b-131">Are there special attributes that need to be synchronized to the cloud so these applications can use them successfully?</span></span>
* <span data-ttu-id="f488b-132">Bude potřeba tyto aplikace znovu zapsání využívat cloudové ověřování?</span><span class="sxs-lookup"><span data-stu-id="f488b-132">Will these applications need to be re-written to take advantage of cloud auth?</span></span>
* <span data-ttu-id="f488b-133">Bude pokračovat za provozu místně, zatímco uživatelé přistupovat k nim pomocí cloudové identitě těchto aplikací?</span><span class="sxs-lookup"><span data-stu-id="f488b-133">Will these applications continue to live on-premises while users access them using the cloud identity?</span></span>

<span data-ttu-id="f488b-134">Také budete muset určit synchronizace adresářů požadavky a omezení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f488b-134">You also need to determine the security requirements and constraints directory synchronization.</span></span> <span data-ttu-id="f488b-135">Toto testování je důležité získat seznam požadavků, které bude nutné k vytváření a údržbu identity uživatele v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f488b-135">This evaluation is important to get a list of the requirements that will be needed in order to create and maintain user’s identities in the cloud.</span></span> <span data-ttu-id="f488b-136">Ujistěte se, že odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="f488b-136">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="f488b-137">Kde budou synchronizačním serverem umístěné?</span><span class="sxs-lookup"><span data-stu-id="f488b-137">Where will the synchronization server be located?</span></span>
* <span data-ttu-id="f488b-138">Bude se připojený k doméně?</span><span class="sxs-lookup"><span data-stu-id="f488b-138">Will it be domain joined?</span></span>
* <span data-ttu-id="f488b-139">Bude server nacházet v omezené síti za bránou firewall, jako je například DMZ?</span><span class="sxs-lookup"><span data-stu-id="f488b-139">Will the server be located on a restricted network behind a firewall, such as a DMZ?</span></span>
  * <span data-ttu-id="f488b-140">Se nebudete moct otevřít porty brány firewall požadovaná kvůli podpoře synchronizace?</span><span class="sxs-lookup"><span data-stu-id="f488b-140">Will you be able to open the required firewall ports to support synchronization?</span></span>
* <span data-ttu-id="f488b-141">Máte v plánu zotavení po havárii pro server synchronizace?</span><span class="sxs-lookup"><span data-stu-id="f488b-141">Do you have a disaster recovery plan for the synchronization server?</span></span>
* <span data-ttu-id="f488b-142">Máte účet s správná oprávnění pro všechny doménové struktury, že chcete, aby byla synchronizována s?</span><span class="sxs-lookup"><span data-stu-id="f488b-142">Do you have an account with the correct permissions for all forests you want to synch with?</span></span>
  * <span data-ttu-id="f488b-143">Pokud vaše společnost nebude vědět, odpověď pro tuto otázku, projděte si část "Oprávnění pro synchronizaci hesel" v článku [instalace služby Azure Active Directory Sync](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) a zjistit, pokud již máte účet Pomocí těchto oprávnění nebo pokud potřebujete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f488b-143">If your company doesn’t know the answer for this question, review the section “Permissions for password synchronization” in the article [Install the Azure Active Directory Sync Service](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) and determine if you already have an account with these permissions or if you need to create one.</span></span>
* <span data-ttu-id="f488b-144">Pokud máte doménovou strukturou třeba synchronizace je synchronizační server nemůže získat pro každou doménovou strukturu?</span><span class="sxs-lookup"><span data-stu-id="f488b-144">If you have mutli-forest sync is the sync server able to get to each forest?</span></span>

> [!NOTE]
> <span data-ttu-id="f488b-145">Zajistěte, aby poznamenejte každou odpověď a pochopit důvody odpověď.</span><span class="sxs-lookup"><span data-stu-id="f488b-145">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="f488b-146">[Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) budou přenášeny po dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="f488b-146">[Determine incident response requirements](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) will go over the options available.</span></span> <span data-ttu-id="f488b-147">Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.</span><span class="sxs-lookup"><span data-stu-id="f488b-147">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f488b-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f488b-148">Next steps</span></span>
[<span data-ttu-id="f488b-149">Stanovení požadavků na službu Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="f488b-149">Determine multi-factor authentication requirements</span></span>](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a><span data-ttu-id="f488b-150">Viz také</span><span class="sxs-lookup"><span data-stu-id="f488b-150">See also</span></span>
[<span data-ttu-id="f488b-151">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="f488b-151">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

