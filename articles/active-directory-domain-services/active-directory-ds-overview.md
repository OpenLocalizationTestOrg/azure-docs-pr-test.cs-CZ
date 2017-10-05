---
title: "Přehled služby Azure Active Directory Domain Services | Microsoft Docs"
description: "Přehled služby Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: b7bbbd29d9e39357e1b3d6346119e08c88f026fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-domain-services"></a><span data-ttu-id="9f1fc-103">Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="9f1fc-103">Azure AD Domain Services</span></span>
## <a name="overview"></a><span data-ttu-id="9f1fc-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9f1fc-104">Overview</span></span>
<span data-ttu-id="9f1fc-105">Služby infrastruktury Azure umožňují nasadit širokou škálu řešení výpočetní agilní způsobem.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-105">Azure Infrastructure Services enable you to deploy a wide range of computing solutions in an agile manner.</span></span> <span data-ttu-id="9f1fc-106">Pomocí Azure Virtual Machines, můžete nasadit téměř okamžitě a platíte jenom podle počtu minut.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-106">With Azure Virtual Machines, you can deploy nearly instantaneously and you pay only by the minute.</span></span> <span data-ttu-id="9f1fc-107">Využitím podpory pro Windows, Linux, SQL Server, Oracle, SAP, IBM a BizTalk, můžete nasadit jakoukoli úlohu, žádný jazyk na téměř jakémkoliv operačním systému.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-107">Using support for Windows, Linux, SQL Server, Oracle, IBM, SAP, and BizTalk, you can deploy any workload, any language, on nearly any operating system.</span></span> <span data-ttu-id="9f1fc-108">Tyto výhody umožňují migrovat starší verze aplikace nasazené na místě do Azure, uložte na provozní náklady.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-108">These benefits enable you to migrate legacy applications deployed on-premises to Azure, to save on operational expenses.</span></span>

<span data-ttu-id="9f1fc-109">Klíčovým prvkem migrace místní aplikace do Azure je zpracování identity potřebám tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-109">A key aspect of migrating on-premises applications to Azure is handling the identity needs of these applications.</span></span> <span data-ttu-id="9f1fc-110">Aplikace využívající technologii adresáře může spoléhají na LDAP pro čtení nebo zápis do podnikového adresáře nebo spoléhají na integrované ověřování systému Windows (ověřování protokolu Kerberos nebo NTLM) k ověření koncových uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-110">Directory-aware applications may rely on LDAP for read or write access to the corporate directory or rely on Windows Integrated Authentication (Kerberos or NTLM authentication) to authenticate end users.</span></span> <span data-ttu-id="9f1fc-111">Obchodní (LOB) aplikace spuštěné na Windows serveru se obvykle nasazují na počítačích připojených k doméně, aby se dala spravovat, bezpečně pomocí zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-111">Line-of-business (LOB) applications running on Windows Server are typically deployed on domain joined machines, so they can be managed securely using Group Policy.</span></span> <span data-ttu-id="9f1fc-112">Tyto závislosti na infrastruktuře podnikové identitě 'navýšení a shift' v místě aplikací do cloudu, musí být rozpoznány.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-112">To 'lift-and-shift' on-premises applications to the cloud, these dependencies on the corporate identity infrastructure need to be resolved.</span></span>

<span data-ttu-id="9f1fc-113">Správci často zapnout na jednu z následujících řešení vyhovět potřebám identity jejich aplikace nasazené v Azure:</span><span class="sxs-lookup"><span data-stu-id="9f1fc-113">Administrators often turn to one of the following solutions to satisfy the identity needs of their applications deployed in Azure:</span></span>

* <span data-ttu-id="9f1fc-114">Nasazení připojení site-to-site VPN mezi úlohami spuštěnými v služby infrastruktury Azure a firemní adresář místní.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-114">Deploy a site-to-site VPN connection between workloads running in Azure Infrastructure Services and the corporate directory on-premises.</span></span>
* <span data-ttu-id="9f1fc-115">Rozšiřte podnikové infrastruktury domény nebo doménové struktuře AD nastavení řadičů domény repliky pomocí virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-115">Extend the corporate AD domain/forest infrastructure by setting up replica domain controllers using Azure virtual machines.</span></span>
* <span data-ttu-id="9f1fc-116">Nasazení samostatné doméně v Azure pomocí řadiče domény nasazené jako virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-116">Deploy a stand-alone domain in Azure using domain controllers deployed as Azure virtual machines.</span></span>

<span data-ttu-id="9f1fc-117">Všechny tyto přístupy trpí vysoké náklady a režijní náklady na správu.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-117">All these approaches suffer from high cost and administrative overhead.</span></span> <span data-ttu-id="9f1fc-118">Správci jsou nutné k nasazení řadičů domény pomocí virtuálních počítačů v Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-118">Administrators are required to deploy domain controllers using virtual machines in Azure.</span></span> <span data-ttu-id="9f1fc-119">Kromě toho potřebují spravovat, zabezpečení, opravy, monitorování, zálohování a řešení těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-119">Additionally, they need to manage, secure, patch, monitor, backup, and troubleshoot these virtual machines.</span></span> <span data-ttu-id="9f1fc-120">Závislá na připojení k síti VPN do místního adresáře způsobí, že úlohy nasazené v Azure a být zranitelné v důsledku chyb přechodný síťový nebo výpadků.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-120">The reliance on VPN connections to the on-premises directory causes workloads deployed in Azure to be vulnerable to transient network glitches or outages.</span></span> <span data-ttu-id="9f1fc-121">Tyto výpadky sítě zase mít za následek nižší provozu a sníženou spolehlivost pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-121">These network outages in turn result in lower uptime and reduced reliability for these applications.</span></span>

<span data-ttu-id="9f1fc-122">Jsme chtěli Azure AD Domain Services zajistit alternativu jednodušší.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-122">We designed Azure AD Domain Services to provide an easier alternative.</span></span>

## <a name="introducing-azure-ad-domain-services"></a><span data-ttu-id="9f1fc-123">Představení služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="9f1fc-123">Introducing Azure AD Domain Services</span></span>
<span data-ttu-id="9f1fc-124">Služba Azure AD Domain Services poskytuje spravované doméně služby, jako je například připojení k doméně, ověřování protokolu Kerberos nebo NTLM zásad LDAP, skupiny, které jsou plně kompatibilní s Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-124">Azure AD Domain Services provides managed domain services such as domain join, group policy, LDAP, Kerberos/NTLM authentication that are fully compatible with Windows Server Active Directory.</span></span> <span data-ttu-id="9f1fc-125">Můžete využívat tyto domény služby bez nutnosti nasazení, správě a oprava řadiče domény v cloudu.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-125">You can consume these domain services without the need for you to deploy, manage, and patch domain controllers in the cloud.</span></span> <span data-ttu-id="9f1fc-126">Služba Azure AD Domain Services se integruje s existující klientovi Azure AD, a tím umožnit uživatelům přihlásit se pomocí svých podnikových přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-126">Azure AD Domain Services integrates with your existing Azure AD tenant, thus making it possible for users to log in using their corporate credentials.</span></span> <span data-ttu-id="9f1fc-127">Kromě toho můžete použít existující skupiny a uživatelské účty zabezpečený přístup k prostředkům, čímž zajišťuje hladší 'navýšení a shift' z místních prostředků do služby infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-127">Additionally, you can use existing groups and user accounts to secure access to resources, thus ensuring a smoother 'lift-and-shift' of on-premises resources to Azure Infrastructure Services.</span></span>

<span data-ttu-id="9f1fc-128">Azure AD Domain Services funkce funguje bezproblémově bez ohledu na to, jestli klientovi Azure AD je jenom pro cloud nebo synchronizované s vaší místní Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-128">Azure AD Domain Services functionality works seamlessly regardless of whether your Azure AD tenant is cloud-only or synced with your on-premises Active Directory.</span></span>

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a><span data-ttu-id="9f1fc-129">Azure AD Domain Services pro organizace, jenom pro cloud</span><span class="sxs-lookup"><span data-stu-id="9f1fc-129">Azure AD Domain Services for cloud-only organizations</span></span>
<span data-ttu-id="9f1fc-130">Výhradně cloudový klienta Azure AD (často označované jako "spravované klientů.) nemá žádné místní identity nároky.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-130">A cloud-only Azure AD tenant (often referred to as 'managed tenants') does not have any on-premises identity footprint.</span></span> <span data-ttu-id="9f1fc-131">Jinými slovy uživatelské účty, jejich hesla a členství ve skupinách jsou všechny nativní pro cloud – to znamená, vytvořit a spravovat ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-131">In other words, user accounts, their passwords, and group memberships are all native to the cloud - that is, created and managed in Azure AD.</span></span> <span data-ttu-id="9f1fc-132">Vezměte v úvahu na chvíli že Contoso je čistě cloudové klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-132">Consider for a moment that Contoso is a cloud-only Azure AD tenant.</span></span> <span data-ttu-id="9f1fc-133">Jak je znázorněno na následujícím obrázku, správce společnosti Contoso nakonfiguroval virtuální sítě ve službách infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-133">As shown in the following illustration, Contoso's administrator has configured a virtual network in Azure Infrastructure Services.</span></span> <span data-ttu-id="9f1fc-134">V této virtuální sítě ve virtuálních počítačích Azure nasazených aplikací a zatížení serveru.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-134">Applications and server workloads are deployed in this virtual network in Azure virtual machines.</span></span> <span data-ttu-id="9f1fc-135">Vzhledem k tomu, že Contoso je jenom pro cloud klienta, všechny identity uživatelů, jejich přihlašovacích údajů a členství ve skupinách jsou vytvořit a spravovat ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-135">Since Contoso is a cloud-only tenant, all user identities, their credentials, and group memberships are created and managed in Azure AD.</span></span>

![Přehled služby Azure AD Domain Services](./media/active-directory-domain-services-overview/aadds-overview.png)

<span data-ttu-id="9f1fc-137">Společnosti Contoso správce IT může povolení služby Azure AD Domain Services pro svého tenanta Azure AD a zvolte chcete zpřístupnit služby domény v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-137">Contoso's IT administrator can enable Azure AD Domain Services for their Azure AD tenant and choose to make domain services available in this virtual network.</span></span> <span data-ttu-id="9f1fc-138">Po tomto datu Azure AD Domain Services zřizuje spravované domény a zpřístupní ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-138">Thereafter, Azure AD Domain Services provisions a managed domain and makes it available in the virtual network.</span></span> <span data-ttu-id="9f1fc-139">Všechny uživatelské účty, členství ve skupinách a k dispozici v klientovi Azure AD společnosti Contoso přihlašovací údaje uživatele jsou taky k dispozici v této doméně nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-139">All user accounts, group memberships, and user credentials available in Contoso's Azure AD tenant are also available in this newly created domain.</span></span> <span data-ttu-id="9f1fc-140">Tato funkce umožňuje uživatelům v organizaci se přihlásit k doméně pomocí podnikových přihlašovacích – například při vzdáleném připojení k doméně počítače přes vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-140">This feature enables users in the organization to sign in to the domain using their corporate credentials - for example, when connecting remotely to domain-joined machines via Remote Desktop.</span></span> <span data-ttu-id="9f1fc-141">Správci můžou zřizovat přístup k prostředkům v doméně pomocí existující členství ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-141">Administrators can provision access to resources in the domain using existing group memberships.</span></span> <span data-ttu-id="9f1fc-142">Aplikace nasazené ve virtuálních počítačích ve virtuální síti můžete použít funkce, například připojení k doméně, LDAP pro čtení, LDAP bind, ověřování NTLM a Kerberos a zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-142">Applications deployed in virtual machines on the virtual network can use features like domain join, LDAP read, LDAP bind, NTLM and Kerberos authentication, and Group Policy.</span></span>

<span data-ttu-id="9f1fc-143">Několik nejdůležitějšími aspekty tohoto spravované domény, který je zajištěný nástrojem Azure AD Domain Services jsou následující:</span><span class="sxs-lookup"><span data-stu-id="9f1fc-143">A few salient aspects of the managed domain that is provisioned by Azure AD Domain Services are as follows:</span></span>

* <span data-ttu-id="9f1fc-144">Společnosti Contoso správce IT není potřeba spravovat, oprava nebo sledovat tuto doménu nebo všechny řadiče domény pro toto spravované domény.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-144">Contoso's IT administrator does not need to manage, patch, or monitor this domain or any domain controllers for this managed domain.</span></span>
* <span data-ttu-id="9f1fc-145">Není nutné ke správě replikace služby AD pro tuto doménu.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-145">There is no need to manage AD replication for this domain.</span></span> <span data-ttu-id="9f1fc-146">Uživatelské účty, členství ve skupinách a přihlašovacích údajů z klienta Azure AD společnosti Contoso jsou automaticky dostupné v rámci této spravované domény.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-146">User accounts, group memberships, and credentials from Contoso's Azure AD tenant are automatically available within this managed domain.</span></span>
* <span data-ttu-id="9f1fc-147">Vzhledem k tomu, že doménu je spravované službou Azure AD Domain Services, Contoso na správce IT nemá oprávnění správce domény nebo správce podnikové sítě v této doméně.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-147">Since the domain is managed by Azure AD Domain Services, Contoso's IT administrator does not have Domain Administrator or Enterprise Administrator privileges on this domain.</span></span>

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a><span data-ttu-id="9f1fc-148">Azure AD Domain Services pro hybridní organizace</span><span class="sxs-lookup"><span data-stu-id="9f1fc-148">Azure AD Domain Services for hybrid organizations</span></span>
<span data-ttu-id="9f1fc-149">Organizace s hybridní infrastrukturu IT využívat kombinaci prostředků cloudu a místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-149">Organizations with a hybrid IT infrastructure consume a mix of cloud resources and on-premises resources.</span></span> <span data-ttu-id="9f1fc-150">Tyto organizace synchronizovat informace o identitě ze svého místního adresáře do jejich klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-150">Such organizations synchronize identity information from their on-premises directory to their Azure AD tenant.</span></span> <span data-ttu-id="9f1fc-151">Jako hybridní organizace podívejte se na migraci více ze své místní aplikace do cloudu, zejména starší verze adresáře aplikací, může být užitečné k nim Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-151">As hybrid organizations look to migrate more of their on-premises applications to the cloud, especially legacy directory-aware applications, Azure AD Domain Services can be useful to them.</span></span>

<span data-ttu-id="9f1fc-152">Litware Corporation nasadil [Azure AD Connect](../active-directory/active-directory-aadconnect.md), k synchronizaci informací o identitě ze svého místního adresáře do jejich klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-152">Litware Corporation has deployed [Azure AD Connect](../active-directory/active-directory-aadconnect.md), to synchronize identity information from their on-premises directory to their Azure AD tenant.</span></span> <span data-ttu-id="9f1fc-153">Informace o identitě, který je synchronizován se obsahuje uživatelské účty, jejich hodnoty hash přihlašovacích údajů pro ověřování (heslo sync) a členství ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-153">The identity information that is synchronized includes user accounts, their credential hashes for authentication (password sync) and group memberships.</span></span>

> [!NOTE]
> <span data-ttu-id="9f1fc-154">**Synchronizace hesla je povinné pro hybridní organizace používat Azure AD Domain Services**.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-154">**Password synchronization is mandatory for hybrid organizations to use Azure AD Domain Services**.</span></span> <span data-ttu-id="9f1fc-155">Tento požadavek je, protože jsou vyžadované přihlašovací údaje uživatelů ve spravované doméně poskytované Azure AD Domain Services, k ověřování těchto uživatelů prostřednictvím metody ověřování protokolem NTLM nebo Kerberos.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-155">This requirement is because users' credentials are needed in the managed domain provided by Azure AD Domain Services, to authenticate these users via NTLM or Kerberos authentication methods.</span></span>
>
>

![Služba Azure AD Domain Services pro Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

<span data-ttu-id="9f1fc-157">Předchozí obrázek ukazuje, jak organizace s hybridní infrastrukturu IT, jako je například Litware Corporation, můžete použít Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-157">The preceding illustration shows how organizations with a hybrid IT infrastructure, such as Litware Corporation, can use Azure AD Domain Services.</span></span> <span data-ttu-id="9f1fc-158">Aplikace a úlohy serveru, které vyžadují služby domény litware's se nasadí ve virtuální síti ve službách infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-158">Litware's applications and server workloads that require domain services are deployed in a virtual network in Azure Infrastructure Services.</span></span> <span data-ttu-id="9f1fc-159">Litware's správce IT může povolení služby Azure AD Domain Services pro svého tenanta Azure AD a zvolte chcete zpřístupnit spravované domény v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-159">Litware's IT administrator can enable Azure AD Domain Services for their Azure AD tenant and choose to make a managed domain available in this virtual network.</span></span> <span data-ttu-id="9f1fc-160">Protože Litware organizaci s hybridní infrastrukturu IT, uživatelské účty, skupiny a přihlašovací údaje jsou synchronizovány do jejich klienta Azure AD ze svého místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-160">Since Litware is an organization with a hybrid IT infrastructure, user accounts, groups, and credentials are synchronized to their Azure AD tenant from their on-premises directory.</span></span> <span data-ttu-id="9f1fc-161">Tato funkce umožňuje uživatelům přihlášení k doméně pomocí podnikových přihlašovacích – například při vzdáleném připojení pro počítače připojené k doméně přes vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-161">This feature enables users to sign in to the domain using their corporate credentials - for example, when connecting remotely to machines joined to the domain via Remote Desktop.</span></span> <span data-ttu-id="9f1fc-162">Správci můžou zřizovat přístup k prostředkům v doméně pomocí existující členství ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-162">Administrators can provision access to resources in the domain using existing group memberships.</span></span> <span data-ttu-id="9f1fc-163">Aplikace nasazené ve virtuálních počítačích ve virtuální síti můžete použít funkce, například připojení k doméně, LDAP pro čtení, LDAP bind, ověřování NTLM a Kerberos a zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-163">Applications deployed in virtual machines on the virtual network can use features like domain join, LDAP read, LDAP bind, NTLM and Kerberos authentication, and Group Policy.</span></span>

<span data-ttu-id="9f1fc-164">Několik nejdůležitějšími aspekty tohoto spravované domény, který je zajištěný nástrojem Azure AD Domain Services jsou následující:</span><span class="sxs-lookup"><span data-stu-id="9f1fc-164">A few salient aspects of the managed domain that is provisioned by Azure AD Domain Services are as follows:</span></span>

* <span data-ttu-id="9f1fc-165">Spravované doméně je samostatné domény.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-165">The managed domain is a stand-alone domain.</span></span> <span data-ttu-id="9f1fc-166">Není rozšíření Litware's místní domény.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-166">It is not an extension of Litware's on-premises domain.</span></span>
* <span data-ttu-id="9f1fc-167">Litware's správce IT není potřeba spravovat, opravy, nebo monitorování řadičů domény pro toto spravované domény.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-167">Litware's IT administrator does not need to manage, patch, or monitor domain controllers for this managed domain.</span></span>
* <span data-ttu-id="9f1fc-168">Není nutné ke správě replikace AD k této doméně.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-168">There is no need to manage AD replication to this domain.</span></span> <span data-ttu-id="9f1fc-169">Uživatelské účty, členství ve skupinách a přihlašovacích údajů z místního adresáře Litware's jsou synchronizovány do Azure AD pomocí Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-169">User accounts, group memberships, and credentials from Litware's on-premises directory are synchronized to Azure AD via Azure AD Connect.</span></span> <span data-ttu-id="9f1fc-170">Tyto uživatelské účty, členství ve skupinách a přihlašovacích údajů jsou automaticky dostupné v rámci spravované domény.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-170">These user accounts, group memberships, and credentials are automatically available within the managed domain.</span></span>
* <span data-ttu-id="9f1fc-171">Vzhledem k tomu, že doménu je spravované službou Azure AD Domain Services, Litware na správce IT nemá oprávnění správce domény nebo správce podnikové sítě v této doméně.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-171">Since the domain is managed by Azure AD Domain Services, Litware's IT administrator does not have Domain Administrator or Enterprise Administrator privileges on this domain.</span></span>

## <a name="benefits"></a><span data-ttu-id="9f1fc-172">Výhody</span><span class="sxs-lookup"><span data-stu-id="9f1fc-172">Benefits</span></span>
<span data-ttu-id="9f1fc-173">S Azure AD Domain Services můžete využívat následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9f1fc-173">With Azure AD Domain Services, you can enjoy the following benefits:</span></span>

* <span data-ttu-id="9f1fc-174">**Jednoduché** – můžete vyhovět potřebám identity virtuální počítače nasazené služby infrastruktury Azure s jenom několik jednoduchých kliknutí.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-174">**Simple** – You can satisfy the identity needs of virtual machines deployed to Azure Infrastructure services with a few simple clicks.</span></span> <span data-ttu-id="9f1fc-175">Není nutné k nasazení a správě infrastruktury identit v Azure nebo instalační program připojit zpět k vaší místní infrastruktury identity.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-175">You do not need to deploy and manage identity infrastructure in Azure or setup connectivity back to your on-premises identity infrastructure.</span></span>
* <span data-ttu-id="9f1fc-176">**Integrované** – Azure AD Domain Services se úzce integruje se klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-176">**Integrated** – Azure AD Domain Services is deeply integrated with your Azure AD tenant.</span></span> <span data-ttu-id="9f1fc-177">Teď můžete použít Azure AD integrované podnikové cloudové adresáře, který je určen pro potřeby moderních aplikací a tradiční aplikace využívající technologii directory.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-177">You can now use Azure AD as an integrated cloud-based enterprise directory that caters to the needs of both your modern applications and traditional directory-aware applications.</span></span>
* <span data-ttu-id="9f1fc-178">**Kompatibilní** – Azure AD Domain Services je založený na úrovni infrastruktury Principy enterprise systému Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-178">**Compatible** – Azure AD Domain Services is built on the proven enterprise grade infrastructure of Windows Server Active Directory.</span></span> <span data-ttu-id="9f1fc-179">Aplikace můžete tedy spoléhat na vyšší úroveň kompatibility s funkcemi, Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-179">Therefore, your applications can rely on a greater degree of compatibility with Windows Server Active Directory features.</span></span> <span data-ttu-id="9f1fc-180">Ne všechny funkce, které jsou k dispozici v systému Windows Server AD jsou aktuálně dostupné v Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-180">Not all features available in Windows Server AD are currently available in Azure AD Domain Services.</span></span> <span data-ttu-id="9f1fc-181">K dispozici funkce jsou však kompatibilní s odpovídající funkce systému Windows Server AD, které byste tedy spoléhat na ve vaší místní infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-181">However, available features are compatible with the corresponding Windows Server AD features you rely on in your on-premises infrastructure.</span></span> <span data-ttu-id="9f1fc-182">Možnosti připojení LDAP, protokolu Kerberos, NTLM, zásad skupiny a domény tvoří vyspělá nabídky, které byly testovány a vylepšení v různých verzích systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-182">The LDAP, Kerberos, NTLM, Group Policy, and domain join capabilities constitute a mature offering that has been tested and refined over various Windows Server releases.</span></span>
* <span data-ttu-id="9f1fc-183">**Nákladově efektivní** – službou Azure AD Domain Services, se můžete vyhnout spojené se správou infrastruktury identity pro podporu tradiční aplikace využívající technologii directory zatížení infrastruktury a správu.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-183">**Cost-effective** – With Azure AD Domain Services, you can avoid the infrastructure and management burden that is associated with managing identity infrastructure to support traditional directory-aware applications.</span></span> <span data-ttu-id="9f1fc-184">Můžete přesunout tyto aplikace službám infrastruktury Azure a využívat větší úsporu na provozní náklady.</span><span class="sxs-lookup"><span data-stu-id="9f1fc-184">You can move these applications to Azure Infrastructure Services and benefit from greater savings on operational expenses.</span></span>