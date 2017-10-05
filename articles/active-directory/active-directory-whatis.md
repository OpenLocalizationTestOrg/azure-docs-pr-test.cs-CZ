---
title: "Představení služby Azure Active Directory"
description: "Pomocí Azure Active Directory rozšířit vaše stávající místních identit do cloudu, nebo vyvíjet aplikace, integraci služby Azure AD."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.custom: it-pro
ms.openlocfilehash: b6746afd508832afbd54153851b6f2ae404af147
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-azure-active-directory"></a><span data-ttu-id="ff189-103">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff189-103">What is Azure Active Directory?</span></span>
<span data-ttu-id="ff189-104">Azure Active Directory (Azure AD) je Microsoft je více klientů, cloudu na základě adresáře a identity management service.</span><span class="sxs-lookup"><span data-stu-id="ff189-104">Azure Active Directory (Azure AD) is Microsoft’s multi-tenant, cloud based directory and identity management service.</span></span> <span data-ttu-id="ff189-105">Azure AD kombinuje základní adresářové služby, pokročilé identity zásad správného řízení a správa přístupu aplikací.</span><span class="sxs-lookup"><span data-stu-id="ff189-105">Azure AD combines core directory services, advanced identity governance, and application access management.</span></span> <span data-ttu-id="ff189-106">Azure AD také nabízí bohatou a založených na standardech platformu, která umožňuje vývojářům k poskytování řízení přístupu k jejich aplikacím na základě centralizovaných zásad a pravidla.</span><span class="sxs-lookup"><span data-stu-id="ff189-106">Azure AD also offers a rich, standards-based platform that enables developers to deliver access control to their applications, based on centralized policy and rules.</span></span> 

<span data-ttu-id="ff189-107">Pro správce IT, Azure AD poskytuje cenově dostupné a snadno použitelný řešení poskytnout zaměstnancům a obchodními partnery jednotné přihlašování (SSO) přístup [tisíce cloudové aplikace SaaS](active-directory-saas-tutorial-list.md) , jako třeba Office 365, Salesforce.com, DropBox a Concur.</span><span class="sxs-lookup"><span data-stu-id="ff189-107">For IT Admins, Azure AD provides an affordable, easy to use solution to give employees and business partners single sign-on (SSO) access to [thousands of cloud SaaS Applications](active-directory-saas-tutorial-list.md) like Office365, Salesforce.com, DropBox, and Concur.</span></span>

<span data-ttu-id="ff189-108">Pro vývojáře aplikací Azure AD vám umožní soustředit na budování aplikaci tím, že rychlé a jednoduché integrovat řešení správy identity – třída world používané miliony organizací po celém světě.</span><span class="sxs-lookup"><span data-stu-id="ff189-108">For application developers, Azure AD lets you focus on building your application by making it fast and simple to integrate with a world class identity management solution used by millions of organizations around the world.</span></span>

<span data-ttu-id="ff189-109">Azure AD také zahrnuje úplná sada funkcí správy identit, včetně vícefaktorového ověřování, registrace zařízení, správu hesla pomocí samoobslužné služby, Samoobslužná správa skupin, správu privilegovaného účtu, řízení přístupu, sledování využití aplikací, bohaté auditování a zabezpečení monitorování a výstrahy na základě role.</span><span class="sxs-lookup"><span data-stu-id="ff189-109">Azure AD also includes a full suite of identity management capabilities including multi-factor authentication, device registration, self-service password management, self-service group management, privileged account management, role based access control, application usage monitoring, rich auditing and security monitoring and alerting.</span></span> <span data-ttu-id="ff189-110">Tyto možnosti můžete pomůže zabezpečené cloudové aplikace, zjednodušit procesy IT, snížit náklady a ujistěte se, zda jsou splněny dodržování podnikových předpisů cíle.</span><span class="sxs-lookup"><span data-stu-id="ff189-110">These capabilities can help secure cloud based applications, streamline IT processes, cut costs and help ensure that corporate compliance goals are met.</span></span>

<span data-ttu-id="ff189-111">Kromě toho se právě [čtyři kliknutí](./connect/active-directory-aadconnect-get-started-express.md), Azure AD se dá integrovat s existující Windows Server Active Directory, což umožní využít svých stávajících místních organizace investice identity můžete spravovat přístup do cloudu na základě aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ff189-111">Additionally, with just [four clicks](./connect/active-directory-aadconnect-get-started-express.md), Azure AD can be integrated with an existing Windows Server Active Directory, giving organizations the ability to leverage their existing on-premises identity investments to manage access to cloud based SaaS applications.</span></span>

<span data-ttu-id="ff189-112">Pokud jste zákazník Office 365, Azure nebo Dynamics CRM Online, nemusí Uvědomte si, že už používáte Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff189-112">If you are an Office 365, Azure or Dynamics CRM Online customer, you might not realize that you are already using Azure AD.</span></span> <span data-ttu-id="ff189-113">Každému klientovi Office 365, Azure a Dynamics CRM je ve skutečnosti již klient služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff189-113">Every Office 365, Azure and Dynamics CRM tenant is actually already an Azure AD tenant.</span></span> <span data-ttu-id="ff189-114">Vždy, když chcete, že můžete začít používat ke správě tohoto klienta přístup k tisícům jiných cloudových aplikací Azure AD se integruje se službou!</span><span class="sxs-lookup"><span data-stu-id="ff189-114">Whenever you want you can start using that tenant to manage access to thousands of other cloud applications Azure AD integrates with!</span></span>

![Sada komponent Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="how-reliable-is-azure-ad"></a><span data-ttu-id="ff189-116">Jak spolehlivé je Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ff189-116">How reliable is Azure AD?</span></span>
<span data-ttu-id="ff189-117">Návrh víceklientské, zeměpisné polohy, vysokou dostupnost služby Azure AD znamená, že můžete spoléhat na pro vaše nejdůležitější obchodní potřeby.</span><span class="sxs-lookup"><span data-stu-id="ff189-117">The multi-tenant, geo-distributed, high availability design of Azure AD means that you can rely on it for your most critical business needs.</span></span> <span data-ttu-id="ff189-118">Systém mimo 28 datových centrech po celém světě s automatické převzetí služeb při selhání, budete mít pohodlí zároveň budete vědět, že Azure AD je vysoce spolehlivé a to i v případě, že datové centrum ocitne mimo provoz kopie adresáře data jsou v za provozu alespoň dva další regionální rozptýlená v datových centrech a k dispozici pro okamžitý přístup.</span><span class="sxs-lookup"><span data-stu-id="ff189-118">Running out of 28 data centers around the world with automated failover, you’ll have the comfort of knowing that Azure AD is highly reliable and that even if a data center goes down, copies of your directory data are live in at least two more regionally dispersed data centers and available for instant access.</span></span>

<span data-ttu-id="ff189-119">Další podrobnosti najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="ff189-119">For more details, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="choose-an-edition"></a><span data-ttu-id="ff189-120">Zvolte edice</span><span class="sxs-lookup"><span data-stu-id="ff189-120">Choose an edition</span></span>
<span data-ttu-id="ff189-121">Všechny firemní služby Microsoft Online spoléhají na Azure Active Directory (Azure AD) pro přihlášení a je třeba další identity.</span><span class="sxs-lookup"><span data-stu-id="ff189-121">All Microsoft Online business services rely on Azure Active Directory (Azure AD) for sign-in and other identity needs.</span></span> <span data-ttu-id="ff189-122">Pokud se přihlásíte k některému z firemní služby Microsoft Online (například Office 365 nebo Microsoft Azure), bude docházet k Azure AD s přístupem ke všem funkcím volné.</span><span class="sxs-lookup"><span data-stu-id="ff189-122">If you subscribe to any of Microsoft Online business services (for example, Office 365 or Microsoft Azure), you get Azure AD with access to all of the Free features.</span></span> <span data-ttu-id="ff189-123">S edice Azure Active Directory Free můžete spravovat uživatele a skupiny, proveďte synchronizaci s místních adresářů, získat jednotné přihlašování v Azure, Office 365 a tisícům oblíbených aplikací SaaS jako Salesforce, Workday, Concur, DocuSign, Google Apps, pole, ServiceNow, Dropbox a další.</span><span class="sxs-lookup"><span data-stu-id="ff189-123">With the Azure Active Directory Free edition, you can manage users and groups, synchronize with on-premises directories, get single sign-on across Azure, Office 365, and thousands of popular SaaS applications like Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox, and more.</span></span> 

<span data-ttu-id="ff189-124">K vylepšení služby Azure Active Directory, můžete přidat placené možnosti pomocí edice Azure Active Directory Basic, Premium P1 a Premium P2.</span><span class="sxs-lookup"><span data-stu-id="ff189-124">To enhance your Azure Active Directory, you can add paid capabilities using the Azure Active Directory Basic, Premium P1, and Premium P2 editions.</span></span> <span data-ttu-id="ff189-125">Azure Active Directory placené edice jsou postavená na adresáře existující volné poskytování podnikové třídy pokrývání uzlů samoobslužné služby, rozšířené monitorování, vytváření sestav zabezpečení, služba Multi-Factor Authentication (MFA) a zabezpečený přístup pro mobilních pracovníků.</span><span class="sxs-lookup"><span data-stu-id="ff189-125">Azure Active Directory paid editions are built on top of your existing free directory, providing enterprise class capabilities spanning self-service, enhanced monitoring, security reporting, Multi-Factor Authentication (MFA), and secure access for your mobile workforce.</span></span>

> [!NOTE]
> <span data-ttu-id="ff189-126">Cenová možnosti tyto edice najdete v tématu [Azure Active Directory ceny](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="ff189-126">For the pricing options of these editions, see [Azure Active Directory Pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span> <span data-ttu-id="ff189-127">Azure Active Directory Premium P1, Premium P2 a Azure Active Directory Basic v Číně aktuálně nepodporují.</span><span class="sxs-lookup"><span data-stu-id="ff189-127">Azure Active Directory Premium P1, Premium P2, and Azure Active Directory Basic are not currently supported in China.</span></span> <span data-ttu-id="ff189-128">Kontaktujte nás na fóru Azure Active Directory pro další informace.</span><span class="sxs-lookup"><span data-stu-id="ff189-128">Please contact us at the Azure Active Directory Forum for more information.</span></span>
>

* <span data-ttu-id="ff189-129">**Azure Active Directory Basic** -navržený pro úkolové pracovníky s potřebami cloudu první, tato edice poskytuje cloudové řešení správy identity zaměřená na aplikace pro přístup a samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="ff189-129">**Azure Active Directory Basic** - Designed for task workers with cloud-first needs, this edition provides cloud centric application access and self-service identity management solutions.</span></span> <span data-ttu-id="ff189-130">Azure Active Directory ve verzi Basic vám poskytne funkce pro zvýšení produktivity a snížení nákladů, jako je správa přístupu na základě skupin, samoobslužné obnovení hesel pro cloudové aplikace a aplikační proxy Azure Active Directory (aby se pomocí Azure Active Directory daly publikovat místní webové aplikace), a to všechno se smlouvou SLA na podnikové úrovni s 99,9procentní dostupností.</span><span class="sxs-lookup"><span data-stu-id="ff189-130">With the Basic edition of Azure Active Directory, you get productivity enhancing and cost reducing features like group-based access management, self-service password reset for cloud applications, and Azure Active Directory Application Proxy (to publish on-premises web applications using Azure Active Directory), all backed by an enterprise-level SLA of 99.9 percent uptime.</span></span>
* <span data-ttu-id="ff189-131">**Azure Active Directory Premium P1** – navržené pro organizace s náročnější umožnit správu identit a přístupu musí, Azure Active Directory Premium edition přidá funkce pro správu identit podnikové úrovni bohaté funkce a umožňuje uživatelům hybridní bezproblémově přistupovat k místnímu a funkce v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ff189-131">**Azure Active Directory Premium P1** - Designed to empower organizations with more demanding identity and access management needs, Azure Active Directory Premium edition adds feature-rich enterprise-level identity management capabilities and enables hybrid users to seamlessly access on-premises and cloud capabilities.</span></span> <span data-ttu-id="ff189-132">Tato verze obsahuje všechno, co informační pracovníci a správci identit potřebují v hybridním prostředí k zajištění přístupu k aplikacím, samoobslužné správě identit a přístupu (IAM), ochraně identit a zabezpečení v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ff189-132">This edition includes everything you need for information worker and identity administrators in hybrid environments across application access, self-service identity and access management (IAM), identity protection and security in the cloud.</span></span> <span data-ttu-id="ff189-133">Podporuje rozšířené správy a delegování prostředky jako dynamických skupin a samoobslužnou správu skupin.</span><span class="sxs-lookup"><span data-stu-id="ff189-133">It supports advanced administration and delegation resources like dynamic groups and self-service group management.</span></span> <span data-ttu-id="ff189-134">Zahrnuje Microsoft Identity Manager (místní přístupu a identit a správy sady) a poskytuje cloudu zpětný zápis funkce povolení řešení, jako jsou samoobslužné resetování hesla pro místní uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff189-134">It includes Microsoft Identity Manager (an on-premises identity and access management suite) and provides cloud write-back capabilities enabling solutions like self-service password reset for your on-premises users.</span></span>
* <span data-ttu-id="ff189-135">**Azure Active Directory Premium P2** -navržený s rozšířenou ochranu pro všechny uživatele a správce, tato nová nabídka zahrnuje všechny funkce v Azure AD Premium P1 a také naše nové Identity Protection a Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="ff189-135">**Azure Active Directory Premium P2** - Designed with advanced protection for all your users and administrators, this new offering includes all the capabilities in Azure AD Premium P1 as well as our new Identity Protection and Privileged Identity Management.</span></span> <span data-ttu-id="ff189-136">Azure Active Directory Identity Protection využívá až miliardy signály, které poskytují podmíněného přístupu na základě riziko k vašim aplikacím a důležitá firemní data.</span><span class="sxs-lookup"><span data-stu-id="ff189-136">Azure Active Directory Identity Protection leverages billions of signals to provide risk-based conditional access to your applications and critical company data.</span></span> <span data-ttu-id="ff189-137">Nám také pomáhají spravovat a chránit privilegovaných účtů s Azure Active Directory Privileged Identity Management, můžete zjistit, omezit a sledovat správci a jejich přístup k prostředkům a poskytují přístup za běhu v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ff189-137">We also help you manage and protect privileged accounts with Azure Active Directory Privileged Identity Management so you can discover, restrict and monitor administrators and their access to resources and provide just-in-time access when needed.</span></span>  

> [!NOTE]
> <span data-ttu-id="ff189-138">Počet možností Azure Active Directory jsou k dispozici prostřednictvím "platím průběžně" edice:</span><span class="sxs-lookup"><span data-stu-id="ff189-138">A number of Azure Active Directory capabilities are available through "pay as you go" editions:</span></span>
>
> * <span data-ttu-id="ff189-139">Active Directory B2C je řešení správy identit a přístupu pro aplikace určených.</span><span class="sxs-lookup"><span data-stu-id="ff189-139">Active Directory B2C is the identity and access management solution for your consumer-facing applications.</span></span> <span data-ttu-id="ff189-140">Další podrobnosti najdete v tématu [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)</span><span class="sxs-lookup"><span data-stu-id="ff189-140">For more details, see [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)</span></span>
> * <span data-ttu-id="ff189-141">Azure Multi-Factor Authentication umožňuje použít pro uživatele, nebo v zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff189-141">Azure Multi-Factor Authentication can be used through per user or per authentication providers.</span></span> <span data-ttu-id="ff189-142">Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="ff189-142">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
>

## <a name="how-can-i-get-started"></a><span data-ttu-id="ff189-143">Jak můžu začít?</span><span class="sxs-lookup"><span data-stu-id="ff189-143">How can I get started?</span></span>

<span data-ttu-id="ff189-144">**Pokud jste správce IT:**</span><span class="sxs-lookup"><span data-stu-id="ff189-144">**If you are an IT admin:**</span></span>

* [<span data-ttu-id="ff189-145">Vyzkoušejte si to!</span><span class="sxs-lookup"><span data-stu-id="ff189-145">Try it out!</span></span>](https://azure.microsoft.com/trial/get-started-active-directory/) <span data-ttu-id="ff189-146">-můžete zaregistrujte se ke bezplatné 30denní zkušební verze dnes a nasadit řešení první cloudu pomocí tohoto odkazu v části 5 minut.</span><span class="sxs-lookup"><span data-stu-id="ff189-146">- you can sign up for a free 30 day trial today and deploy your first cloud solution in under 5 minutes using this link</span></span>

* <span data-ttu-id="ff189-147">Čtení [Začínáme s Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) pro klienta spuštěná rychlé tipy a triky na získávání Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff189-147">Read [Getting started with Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) for tips and tricks on getting an Azure AD tenant up and running fast</span></span>

<span data-ttu-id="ff189-148">**Pokud jste vývojář:**</span><span class="sxs-lookup"><span data-stu-id="ff189-148">**If you are a developer:**</span></span>
 
* <span data-ttu-id="ff189-149">Podívejte se na naše [Příručka pro vývojáře](active-directory-developers-guide.md) do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff189-149">Check out our [Developers Guide](active-directory-developers-guide.md) to Azure Active Directory</span></span>

* <span data-ttu-id="ff189-150">[Spuštění zkušební verze](https://azure.microsoft.com/trial/get-started-active-directory/) – zaregistrujte se ke bezplatné 30denní zkušební verze dnes a spusťte integrace aplikací s Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff189-150">[Start a trial](https://azure.microsoft.com/trial/get-started-active-directory/) – sign up for a free 30 day trial today and  start integrating your apps with Azure AD</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff189-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff189-151">Next steps</span></span>
[<span data-ttu-id="ff189-152">Další informace o základní informace o Azure správu identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="ff189-152">Learn more about the fundamentals of Azure identity and access management</span></span>](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)