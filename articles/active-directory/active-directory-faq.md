---
title: "aaaAzure Active Directory – nejčastější dotazy | Microsoft Docs"
description: "Azure Active Directory nejčastější dotazy k odpovědi na otázky o přístupu tooaccess Azure a Azure Active Directory, Správa hesel služby a aplikace."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/16/2017
ms.author: markvi
ms.openlocfilehash: 63c30c4aeda4551bf02c6b968f98cded5a3b2c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-faq"></a><span data-ttu-id="7bbe5-103">Nejčastější dotazy ke službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7bbe5-103">Azure Active Directory FAQ</span></span>
<span data-ttu-id="7bbe5-104">Azure Active Directory (Azure AD) je komplexní řešení Identity jako služby (IDaaS), které pokrývá všechny prvky identity, řízení přístupu a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-104">Azure Active Directory (Azure AD) is a comprehensive identity as a service (IDaaS) solution that spans all aspects of identity, access management, and security.</span></span>

<span data-ttu-id="7bbe5-105">Další informace najdete v tématu [Co je Azure Active Directory?](active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-105">For more information, see [What is Azure Active Directory?](active-directory-whatis.md).</span></span>


## <a name="access-azure-and-azure-active-directory"></a><span data-ttu-id="7bbe5-106">Přístup ke službě Azure a Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7bbe5-106">Access Azure and Azure Active Directory</span></span>
<span data-ttu-id="7bbe5-107">**Otázka: Proč se zobrazila "žádné předplatné nenalezeno" při tooaccess Azure AD v hello portál Azure classic?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-107">**Q: Why do I get “No subscriptions found” when I try tooaccess Azure AD in hello Azure classic portal?**</span></span>

<span data-ttu-id="7bbe5-108">**Odpověď:** tooaccess hello portál Azure classic, každý uživatel potřebuje oprávnění má předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-108">**A:** tooaccess hello Azure classic portal, each user needs permissions with an Azure subscription.</span></span> <span data-ttu-id="7bbe5-109">Pokud máte placené předplatné služeb Office 365 nebo Azure AD, přejděte příliš[http://aka.ms/accessAAD](http://aka.ms/accessAAD) pro krok jednorázové aktivaci.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-109">If you have a paid Office 365 or Azure AD subscription, go too[http://aka.ms/accessAAD](http://aka.ms/accessAAD) for a one-time activation step.</span></span> <span data-ttu-id="7bbe5-110">Jinak, budete potřebovat bezplatný tooactivate [účet Azure](https://azure.microsoft.com/pricing/free-trial/) nebo placené předplatné.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-110">Otherwise, you will need tooactivate a free [Azure account](https://azure.microsoft.com/pricing/free-trial/) or a paid subscription.</span></span>

<span data-ttu-id="7bbe5-111">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7bbe5-111">For more information, see:</span></span>

* [<span data-ttu-id="7bbe5-112">Jak je předplatné Azure propojeno se službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7bbe5-112">How Azure subscriptions are associated with Azure Active Directory</span></span>](active-directory-how-subscriptions-associated-directory.md)
* [<span data-ttu-id="7bbe5-113">Spravovat hello adresáře pro předplatné služeb Office 365 ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="7bbe5-113">Manage hello directory for your Office 365 subscription in Azure</span></span>](active-directory-manage-o365-subscription.md)

- - -
<span data-ttu-id="7bbe5-114">**Otázka: co je hello vztah mezi službou Azure AD, Office 365 a Azure?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-114">**Q: What’s hello relationship between Azure AD, Office 365, and Azure?**</span></span>

<span data-ttu-id="7bbe5-115">**Odpověď:** Azure AD poskytuje běžné funkce identity a přístupu tooall webové služby.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-115">**A:** Azure AD provides you with common identity and access capabilities tooall web services.</span></span> <span data-ttu-id="7bbe5-116">Ať používáte Office 365, Microsoft Azure, Intune, nebo jiné, můžete se už používá Azure AD toohelp zapnout správu přihlašování a přístupu pro všechny tyto služby.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-116">Whether you are using Office 365, Microsoft Azure, Intune, or others, you're already using Azure AD toohelp turn on sign-on and access management for all these services.</span></span>

<span data-ttu-id="7bbe5-117">Všichni uživatelé, kteří jsou nastaveni tak toouse webových služeb jsou definovány jako uživatelské účty v jedné nebo více instancí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-117">All users who are set up toouse web services are defined as user accounts in one or more Azure AD instances.</span></span> <span data-ttu-id="7bbe5-118">Těmto účtům můžete nastavit přístup k bezplatným funkcím služby Azure AD, například ke cloudovým aplikacím.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-118">You can set up these accounts for free Azure AD capabilities like cloud application access.</span></span>

<span data-ttu-id="7bbe5-119">Placené služby AD Azure, jako je Enterprise Mobility + Security, doplňují ostatní webové služby, např. Office 365 nebo Microsoft Azure o komplexní řešení správy a zabezpečení celého podniku.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-119">Azure AD paid services like Enterprise Mobility + Security complement other web services like Office 365 and Microsoft Azure with comprehensive enterprise-scale management and security solutions.</span></span>
- - -
<span data-ttu-id="7bbe5-120">**Otázka: Proč může přihlášení toohello portál Azure ale není hello portál Azure classic?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-120">**Q:  Why can I sign in toohello Azure portal but not hello Azure classic portal?**</span></span>

<span data-ttu-id="7bbe5-121">**Odpověď:** hello portál Azure nevyžaduje platné předplatné, a portálu classic hello vyžadují platné předplatné.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-121">**A:**  hello Azure portal does not require a valid subscription, and hello classic portal does require a valid subscription.</span></span>  <span data-ttu-id="7bbe5-122">Pokud nemáte předplatné, nemůžete se přihlásit toohello portálu classic.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-122">If you do not have a subscription, you can't sign in toohello classic portal.</span></span>
- - -
<span data-ttu-id="7bbe5-123">**Otázka: co jsou hello rozdíly mezi správce předplatného a správce adresáře?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-123">**Q:  What are hello differences between Subscription Administrator and Directory Administrator?**</span></span>

<span data-ttu-id="7bbe5-124">**Odpověď:** ve výchozím nastavení, můžete se přiřazuje role správce předplatného hello při registraci v Azure.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-124">**A:** By default, you are assigned hello Subscription Administrator role when you sign up for Azure.</span></span> <span data-ttu-id="7bbe5-125">Správce předplatného můžete použít účet Microsoft nebo pracovní nebo školní účet hello adresář, který hello předplatné Azure je přidružen.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-125">A subscription admin can use either a Microsoft account or a work or school account from hello directory that hello Azure subscription is associated with.</span></span>  <span data-ttu-id="7bbe5-126">Tato role je autorizovaný toomanage služeb v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-126">This role is authorized toomanage services in hello Azure portal.</span></span>

<span data-ttu-id="7bbe5-127">Pokud ostatní potřebují toosign v a získat přístup ke službám pomocí pomocí hello stejného předplatného, můžete je přidat jako pomocní správci.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-127">If others need toosign in and access services by using hello same subscription, you can add them as co-admins.</span></span> <span data-ttu-id="7bbe5-128">Tato role má hello stejný přístup oprávnění jako dobrý den, Správce služby, ale nelze změnit přidružení hello odběry tooAzure adresářů.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-128">This role has hello same access privileges as hello service admin, but can’t change hello association of subscriptions tooAzure directories.</span></span>  <span data-ttu-id="7bbe5-129">Další informace o Správci předplatného najdete v tématu [jak tooadd nebo změna role Správce služby Azure](../billing-add-change-azure-subscription-administrator.md) a [asociování předplatných Azure se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-129">For additional information on subscription admins, see [How tooadd or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) and [How Azure subscriptions are associated with Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span></span>


<span data-ttu-id="7bbe5-130">Azure AD má jinou sadu správce rolí toomanage hello adresáře a funkce související s identity.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-130">Azure AD has a different set of admin roles toomanage hello directory and identity-related features.</span></span>  <span data-ttu-id="7bbe5-131">Tyto správce bude mít přístup k funkcím toovarious v hello portál Azure nebo hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-131">These admins will have access toovarious features in hello Azure portal or hello Azure classic portal.</span></span> <span data-ttu-id="7bbe5-132">role správce Hello Určuje, co mohou provádět, jako je vytvoření nebo úprava uživatelů, přiřazení role správců tooothers, resetovat uživatelská hesla, spravovat uživatelské licence nebo spravovat domény.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-132">hello admin's role determines what they can do, like create or edit users, assign administrative roles tooothers, reset user passwords, manage user licenses, or manage domains.</span></span>  <span data-ttu-id="7bbe5-133">Další informace o správcích adresáře služby Azure AD a jejich rolích najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-133">For additional information on Azure AD directory admins and their roles, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="7bbe5-134">Kromě toho placené služby AD Azure, jako je Enterprise Mobility + Security, doplňují ostatní webové služby, např. Office 365 nebo Microsoft Azure o komplexní řešení správy a zabezpečení celého podniku.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-134">Additionally, Azure AD paid services like Enterprise Mobility + Security complement other web services, such as Office 365 and Microsoft Azure, with comprehensive enterprise-scale management and security solutions.</span></span>

- - -
<span data-ttu-id="7bbe5-135">**Otázka: Existuje sestava, která ukazuje, kdy vyprší platnost mé uživatelské licence Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-135">**Q: Is there a report that shows when my Azure AD user licenses will expire?**</span></span>

<span data-ttu-id="7bbe5-136">**Odpověď:** Ne.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-136">**A:** No.</span></span>  <span data-ttu-id="7bbe5-137">Aktuálně není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-137">This is not currently available.</span></span>

- - -

## <a name="get-started-with-hybrid-azure-ad"></a><span data-ttu-id="7bbe5-138">Začínáme s hybridní službou Azure AD</span><span class="sxs-lookup"><span data-stu-id="7bbe5-138">Get started with Hybrid Azure AD</span></span>


<span data-ttu-id="7bbe5-139">**Otázka: Jak opustím tenanta, když jsem přidán jako spolupracovník?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-139">**Q: How do I leave a tenant when I am added as a collaborator?**</span></span>

<span data-ttu-id="7bbe5-140">**Odpověď:** při tooanother organizace klienta jsou přidány jako spolupracovníka, můžete použít hello "klienta přepínači" v horním pravém tooswitch hello mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-140">**A:** When you are added tooanother organization's tenant as a collaborator, you can use hello "tenant switcher" in hello upper right tooswitch between tenants.</span></span>  <span data-ttu-id="7bbe5-141">V současné době neexistuje žádný způsob, jak tooleave hello pozvání organizace a společnost Microsoft pracuje na poskytnutí tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-141">Currently, there is no way tooleave hello inviting organization, and Microsoft is working on providing this functionality.</span></span>  <span data-ttu-id="7bbe5-142">Dokud nebude tato funkce je dostupná, můžete požádat hello pozvání tooremove organizace vám v jejich klienta.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-142">Until this feature is available, you can ask hello inviting organization tooremove you from their tenant.</span></span>
- - -
<span data-ttu-id="7bbe5-143">**Otázka: jak můžete připojit Moje místní adresář tooAzure AD?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-143">**Q: How can I connect my on-premises directory tooAzure AD?**</span></span>

<span data-ttu-id="7bbe5-144">**Odpověď:** připojíte tooAzure directory vaše místní AD pomocí Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-144">**A:** You can connect your on-premises directory tooAzure AD by using Azure AD Connect.</span></span>

<span data-ttu-id="7bbe5-145">Další informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-145">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="7bbe5-146">**Otázka: Jak mám nastavit jednotné přihlašování mezi místním adresářem a cloudovými aplikacemi?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-146">**Q: How do I set up SSO between my on-premises directory and my cloud applications?**</span></span>

<span data-ttu-id="7bbe5-147">**Odpověď:** stačí tooset si jednotné přihlašování (SSO) mezi místním adresářem a službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-147">**A:** You only need tooset up single sign-on (SSO) between your on-premises directory and Azure AD.</span></span> <span data-ttu-id="7bbe5-148">Tak dlouho, dokud cloudové aplikace přistupujete přes Azure AD, hello služba automaticky povede uživatele toocorrectly ověřování pomocí jejich místních přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-148">As long as you access your cloud applications through Azure AD, hello service automatically drives your users toocorrectly authenticate with their on-premises credentials.</span></span>

<span data-ttu-id="7bbe5-149">Implementaci jednotného přihlašování z místního prostředí lze snadno nastavit pomocí federačních řešení, jako je například služba Active Directory Federation Services (AD FS), nebo konfigurací synchronizace hodnot hash hesel. Obě možnosti můžete snadno nasadit pomocí Průvodce konfigurací hello Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-149">Implementing SSO from on-premises can be easily achieved with federation solutions such as Active Directory Federation Services (AD FS), or by configuring password hash sync. You can easily deploy both options by using hello Azure AD Connect configuration wizard.</span></span>

<span data-ttu-id="7bbe5-150">Další informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-150">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="7bbe5-151">**Otázka: Poskytuje Azure AD samoobslužný portál pro uživatele v naší organizaci?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-151">**Q: Does Azure AD provide a self-service portal for users in my organization?**</span></span>

<span data-ttu-id="7bbe5-152">**Odpověď:** Ano, Azure AD poskytuje hello [přístupový Panel Azure AD](http://myapps.microsoft.com) pro uživatelskou samoobsluhu a přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-152">**A:** Yes, Azure AD provides you with hello [Azure AD Access Panel](http://myapps.microsoft.com) for user self-service and application access.</span></span> <span data-ttu-id="7bbe5-153">Pokud jste zákazníkem služby Office 365, najdete mnoho hello stejné funkce portálu hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-153">If you are an Office 365 customer, you can find many of hello same capabilities in hello Office 365 portal.</span></span>

<span data-ttu-id="7bbe5-154">Další informace najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-154">For more information, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

- - -
<span data-ttu-id="7bbe5-155">**Otázka: Pomůže mi Azure AD spravovat místní infrastrukturu?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-155">**Q: Does Azure AD help me manage my on-premises infrastructure?**</span></span>

<span data-ttu-id="7bbe5-156">**Odpověď:** Ano.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-156">**A:** Yes.</span></span> <span data-ttu-id="7bbe5-157">edice Azure AD Premium Hello vám poskytne Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-157">hello Azure AD Premium edition provides you with Azure AD Connect Health.</span></span> <span data-ttu-id="7bbe5-158">Azure AD Connect Health pomáhá monitorovat a proniknout do vaší identity místní infrastruktury a hello synchronizační služby.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-158">Azure AD Connect Health helps you monitor and gain insight into your on-premises identity infrastructure and hello synchronization services.</span></span>  

<span data-ttu-id="7bbe5-159">Další informace najdete v tématu [monitorování vaší místní infrastruktury identit a synchronizace služeb v cloudu hello](active-directory-aadconnect-health.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-159">For more information, see [Monitor your on-premises identity infrastructure and synchronization services in hello cloud](active-directory-aadconnect-health.md).</span></span>  

- - -
## <a name="password-management"></a><span data-ttu-id="7bbe5-160">Správa hesel</span><span class="sxs-lookup"><span data-stu-id="7bbe5-160">Password management</span></span>
<span data-ttu-id="7bbe5-161">**Otázka: Je možné použít zpětný zápis hesla služby Azure AD bez synchronizace hesla? (V tomto scénáři je možné toouse Azure AD samoobslužné resetování hesla (SSPR) s heslo zpětný zápis a ne úložiště hesel v cloudu hello?)**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-161">**Q: Can I use Azure AD password write-back without password sync? (In this scenario, is it possible toouse Azure AD self-service password reset (SSPR) with password write-back and not store passwords in hello cloud?)**</span></span>

<span data-ttu-id="7bbe5-162">**Odpověď:** není nutné toosynchronize vaší služby Active Directory hesla tooAzure AD tooenable zpětný zápis.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-162">**A:** You do not need toosynchronize your Active Directory passwords tooAzure AD tooenable write-back.</span></span> <span data-ttu-id="7bbe5-163">Ve federovaném prostředí Azure AD jednotné přihlašování (SSO) spoléhá na hello místní adresář tooauthenticate hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-163">In a federated environment, Azure AD single sign-on (SSO) relies on hello on-premises directory tooauthenticate hello user.</span></span> <span data-ttu-id="7bbe5-164">Tento scénář nevyžaduje hello místní heslo toobe sledovat ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-164">This scenario does not require hello on-premises password toobe tracked in Azure AD.</span></span>

- - -
<span data-ttu-id="7bbe5-165">**Otázka: jak dlouho trvá heslo toobe, zapisovat zpátky tooActive na místní adresáře?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-165">**Q: How long does it take for a password toobe written back tooActive Directory on-premises?**</span></span>

<span data-ttu-id="7bbe5-166">**Odpověď:** Zpětný zápis hesla funguje v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-166">**A:** Password write-back operates in real time.</span></span>

<span data-ttu-id="7bbe5-167">Další informace najdete v tématu [Začínáme se správou hesel](active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-167">For more information, see [Getting started with password management](active-directory-passwords-getting-started.md).</span></span>

- - -
<span data-ttu-id="7bbe5-168">**Otázka: Je možné použít zpětný zápis hesel, která spravuje správce?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-168">**Q: Can I use password write-back with passwords that are managed by an admin?**</span></span>

<span data-ttu-id="7bbe5-169">**Odpověď:** Ano, pokud máte zpětný zápis hesla povolena, hello heslo provádí správce zapíšou se operace zpět tooyour v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-169">**A:** Yes, if you have password write-back enabled, hello password operations performed by an admin are written back tooyour on-premises environment.</span></span>  

<span data-ttu-id="7bbe5-170">Další odpovědi najdete informace o toopassword [nejčastější dotazy se správou hesel](active-directory-passwords-faq.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-170">For more answers toopassword-related questions, see [Password management frequently asked questions](active-directory-passwords-faq.md).</span></span>
- - -
<span data-ttu-id="7bbe5-171">**Otázka: Co mám dělat, když nepamatuji si stávající heslo Office 365 nebo Azure AD při pokusu o toochange hesla?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-171">**Q:  What can I do if I can't remember my existing Office 365/Azure AD password while trying toochange my password?**</span></span>

<span data-ttu-id="7bbe5-172">**Odpověď:** Pro takové situace existuje řada možností.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-172">**A:** For this type of situation, there are a couple of options.</span></span>  <span data-ttu-id="7bbe5-173">Pokud je k dispozici, použijte samoobslužné resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-173">Use self-service password reset (SSPR) if it's available.</span></span>  <span data-ttu-id="7bbe5-174">Fungování samoobslužného resetování hesla závisí na jeho konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-174">Whether SSPR works depends on how it's configured.</span></span>  <span data-ttu-id="7bbe5-175">Další informace najdete v tématu [jak hello heslo resetovat portálu pracovní](active-directory-passwords-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-175">For more information, see [How does hello password reset portal work](active-directory-passwords-best-practices.md).</span></span>

<span data-ttu-id="7bbe5-176">Pro uživatele služeb Office 365, může správce resetovat heslo hello pomocí hello kroků uvedených v [resetovat hesla uživatelů](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-176">For Office 365 users, your admin can reset hello password by using hello steps outlined in [Reset user passwords](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span></span>

<span data-ttu-id="7bbe5-177">Pro účty Azure AD můžete správci resetování hesla pomocí jedné z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="7bbe5-177">For Azure AD accounts, admins can reset passwords by using one of hello following:</span></span>

- [<span data-ttu-id="7bbe5-178">Resetování účtů v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7bbe5-178">Reset accounts in hello Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
- [<span data-ttu-id="7bbe5-179">Resetování účtů portálu classic hello</span><span class="sxs-lookup"><span data-stu-id="7bbe5-179">Reset accounts in hello classic portal</span></span>](active-directory-create-users-reset-password.md)
- [<span data-ttu-id="7bbe5-180">Pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="7bbe5-180">Using PowerShell</span></span>](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a><span data-ttu-id="7bbe5-181">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7bbe5-181">Security</span></span>
<span data-ttu-id="7bbe5-182">**Otázka: Uzamknou se účty po určitém počtu neúspěšných pokusů o přihlášení, nebo se používá složitější strategie?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-182">**Q: Are accounts locked after a specific number of failed attempts or is there a more sophisticated strategy used?**</span></span></br>
<span data-ttu-id="7bbe5-183">Používáme sofistikovanější účty toolock strategie.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-183">We use a more sophisticated strategy toolock accounts.</span></span>  <span data-ttu-id="7bbe5-184">To je založené na hello IP hello požadavku a zadaná hesla hello.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-184">This is based on hello IP of hello request and hello passwords entered.</span></span> <span data-ttu-id="7bbe5-185">Doba trvání Hello hello uzamčení se taky zvýší podle hello pravděpodobnost, že se jedná o útoku.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-185">hello duration of hello lockout also increases based on hello likelihood that it is an attack.</span></span>  

<span data-ttu-id="7bbe5-186">**Otázka: určité (běžné) hesla získat odmítne kvůli hello zprávy 'Toto heslo bylo použité toomany časy', to získáte toopasswords použít v aktuální službě active directory hello?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-186">**Q:  Certain (common) passwords get rejected with hello messages ‘this password has been used toomany times’, does this refer toopasswords used in hello current active directory?**</span></span></br>
<span data-ttu-id="7bbe5-187">Vztahuje se toopasswords, které jsou globálně společné, například všechny varianty "Password" a "123456".</span><span class="sxs-lookup"><span data-stu-id="7bbe5-187">This refers toopasswords that are globally common, such as any variants of “Password” and “123456”.</span></span>

<span data-ttu-id="7bbe5-188">**Otázka: Budou všechny žádosti o přihlášení z podezřelých zdrojů (botnety, koncový bod tor) blokované v případě tenanta B2C, nebo to vyžaduje tenanta edice Basic nebo Premium?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-188">**Q: Will a sign-in request from dubious sources (botnets, tor endpoint) be blocked in a B2C tenant or does this require a Basic or Premium edition tenant?**</span></span></br>
<span data-ttu-id="7bbe5-189">Máme bránu, která filtruje požadavky a nabízí určitou ochranu před botnety a která se používá pro všechny tenanty B2C.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-189">We do have a gateway that filters requests and provides some protection from botnets, and is applied for all B2C tenants.</span></span>

## <a name="application-access"></a><span data-ttu-id="7bbe5-190">Přístup k aplikaci</span><span class="sxs-lookup"><span data-stu-id="7bbe5-190">Application access</span></span>
<span data-ttu-id="7bbe5-191">**Otázka: Kde najdu seznam aplikací, které jsou předem integrovány se službou Azure AD a jejími funkcemi?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-191">**Q: Where can I find a list of applications that are pre-integrated with Azure AD and their capabilities?**</span></span>

<span data-ttu-id="7bbe5-192">**Odpověď:** Azure AD má více než 2 600 předem integrovaných aplikací od společnosti Microsoft, poskytovatelů služeb aplikací a partnerů.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-192">**A:** Azure AD has more than 2,600 pre-integrated applications from Microsoft, application service providers, and partners.</span></span> <span data-ttu-id="7bbe5-193">Všechny předem integrované aplikace podporují jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-193">All pre-integrated applications support single sign-on (SSO).</span></span> <span data-ttu-id="7bbe5-194">Jednotné přihlašování umožňuje využívat tooaccess vaše firemní přihlašovací údaje vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-194">SSO lets you use your organizational credentials tooaccess your apps.</span></span> <span data-ttu-id="7bbe5-195">Některé aplikace hello také podporují automatické zřizování a jeho rušení.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-195">Some of hello applications also support automated provisioning and de-provisioning.</span></span>

<span data-ttu-id="7bbe5-196">Úplný seznam předem integrovaných aplikací hello najdete v tématu hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-196">For a complete list of hello pre-integrated applications, see hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span></span>

- - -
<span data-ttu-id="7bbe5-197">**Otázka: Co když hello aplikace, které je potřeba není hello Azure AD Marketplace?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-197">**Q: What if hello application I need is not in hello Azure AD marketplace?**</span></span>

<span data-ttu-id="7bbe5-198">**Odpověď:** Se službou Azure AD Premium můžete přidávat a konfigurovat libovolné aplikace.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-198">**A:** With Azure AD Premium, you can add and configure any application that you want.</span></span> <span data-ttu-id="7bbe5-199">V závislosti na funkcích aplikace a předvolbách můžete nakonfigurovat jednotné přihlašování a automatické zřizování.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-199">Depending on your application’s capabilities and your preferences, you can configure SSO and automated provisioning.</span></span>  

<span data-ttu-id="7bbe5-200">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7bbe5-200">For more information, see:</span></span>

* [<span data-ttu-id="7bbe5-201">Konfigurace jednoho přihlášení tooapplications které nejsou v galerii aplikací Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="7bbe5-201">Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery</span></span>](active-directory-saas-custom-apps.md)
* [<span data-ttu-id="7bbe5-202">Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="7bbe5-202">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)

- - -
<span data-ttu-id="7bbe5-203">**Otázka: jak uživatelé přihlásit tooapplications pomocí Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-203">**Q: How do users sign in tooapplications by using Azure AD?**</span></span>

<span data-ttu-id="7bbe5-204">**Odpověď:** Azure AD poskytuje několik způsobů pro uživatele tooview a přístupu k aplikacím, jako například:</span><span class="sxs-lookup"><span data-stu-id="7bbe5-204">**A:** Azure AD provides several ways for users tooview and access their applications, such as:</span></span>

* <span data-ttu-id="7bbe5-205">panel přístupu Hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7bbe5-205">hello Azure AD access panel</span></span>
* <span data-ttu-id="7bbe5-206">Spouštěč aplikace Hello Office 365</span><span class="sxs-lookup"><span data-stu-id="7bbe5-206">hello Office 365 application launcher</span></span>
* <span data-ttu-id="7bbe5-207">Přímé přihlášení toofederated aplikace</span><span class="sxs-lookup"><span data-stu-id="7bbe5-207">Direct sign-in toofederated apps</span></span>
* <span data-ttu-id="7bbe5-208">Přímé odkazy toofederated, založené na heslech, nebo existující aplikace</span><span class="sxs-lookup"><span data-stu-id="7bbe5-208">Deep links toofederated, password-based, or existing apps</span></span>

<span data-ttu-id="7bbe5-209">Další informace najdete v tématu [nasazení služby Azure AD integrovaných aplikací toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-209">For more information, see [Deploying Azure AD integrated applications toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>

- - -
<span data-ttu-id="7bbe5-210">**Otázka: co je hello různými způsoby Azure AD umožňuje ověřování a tooapplications přihlášení?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-210">**Q: What are hello different ways Azure AD enables authentication and single sign-on tooapplications?**</span></span>

<span data-ttu-id="7bbe5-211">**Odpověď:** Azure AD podporuje mnoho standardizovaných protokolů pro ověřování a autorizaci, například SAML 2.0, OpenID Connect, OAuth 2.0 a WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-211">**A:** Azure AD supports many standardized protocols for authentication and authorization, such as SAML 2.0, OpenID Connect, OAuth 2.0, and WS-Federation.</span></span> <span data-ttu-id="7bbe5-212">Azure AD podporuje funkce ukládání hesel do trezoru a automatického přihlašování pro aplikace, které podporují pouze ověření na základě formuláře. </span><span class="sxs-lookup"><span data-stu-id="7bbe5-212">Azure AD also supports password vaulting and automated sign-in capabilities for apps that only support forms-based authentication.</span></span>  

<span data-ttu-id="7bbe5-213">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7bbe5-213">For more information, see:</span></span>

* [<span data-ttu-id="7bbe5-214">Scénáře ověřování pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="7bbe5-214">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)
* [<span data-ttu-id="7bbe5-215">Protokoly ověřování pro Active Directory</span><span class="sxs-lookup"><span data-stu-id="7bbe5-215">Active Directory authentication protocols</span></span>](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [<span data-ttu-id="7bbe5-216">Jak funguje jednotné přihlašování pomocí služby Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7bbe5-216">How does single sign-on with Azure Active Directory work?</span></span>](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
<span data-ttu-id="7bbe5-217">**Otázka: Je možné přidat místní aplikace?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-217">**Q: Can I add applications I’m running on-premises?**</span></span>

<span data-ttu-id="7bbe5-218">**Odpověď:** proxy aplikace služby Azure AD poskytují snadný a bezpečný přístup tooon místní webové aplikace, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-218">**A:** Azure AD Application Proxy provides you with easy and secure access tooon-premises web applications that you choose.</span></span> <span data-ttu-id="7bbe5-219">Aplikace v hello můžete používat stejným způsobem jako přístup váš software jako služba (SaaS) aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-219">You can access these applications in hello same way that you access your software as a service (SaaS) apps in Azure AD.</span></span> <span data-ttu-id="7bbe5-220">Není nutné pro sítě VPN nebo toochange infrastruktura vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-220">There is no need for a VPN or toochange your network infrastructure.</span></span>  

<span data-ttu-id="7bbe5-221">Další informace najdete v tématu [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-221">For more information, see [How tooprovide secure remote access tooon-premises applications](active-directory-application-proxy-get-started.md).</span></span>

- - -
<span data-ttu-id="7bbe5-222">**Otázka: Jak můžu vyžadovat vícefaktorové ověřování pro uživatele, kteří používají určitou aplikaci?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-222">**Q: How do I require multi-factor authentication for users who access a particular application?**</span></span>

<span data-ttu-id="7bbe5-223">**Odpověď:** Díky podmíněnému přístupu ke službě Azure AD můžete ke každé aplikaci přiřadit jedinečné zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-223">**A:** With Azure AD conditional access, you can assign a unique access policy for each application.</span></span> <span data-ttu-id="7bbe5-224">V zásadách můžete požadovat použití vícefaktorového ověřování vždy nebo pokud uživatelé nejsou připojené toohello místní sítě.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-224">In your policy, you can require multi-factor authentication always, or when users are not connected toohello local network.</span></span>  

<span data-ttu-id="7bbe5-225">Další informace najdete v tématu [zabezpečení přístupu tooOffice 365 a další aplikace připojeným tooAzure služby Active Directory](active-directory-conditional-access.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-225">For more information, see [Securing access tooOffice 365 and other apps connected tooAzure Active Directory](active-directory-conditional-access.md).</span></span>

- - -
<span data-ttu-id="7bbe5-226">**Otázka: Co je automatické zřizování uživatelů pro aplikace SaaS?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-226">**Q: What is automated user provisioning for SaaS apps?**</span></span>

<span data-ttu-id="7bbe5-227">**Odpověď:** tooautomate hello vytváření, údržbu a odebírání uživatelských identit v mnoha oblíbených cloudových aplikací SaaS používání Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-227">**A:** Use Azure AD tooautomate hello creation, maintenance, and removal of user identities in many popular cloud SaaS apps.</span></span>

<span data-ttu-id="7bbe5-228">Další informace najdete v tématu [automatizace zřizování uživatelů a rušení zajištění tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="7bbe5-228">For more information, see [Automate user provisioning and deprovisioning tooSaaS applications with Azure Active Directory](active-directory-saas-app-provisioning.md).</span></span>

- - -
<span data-ttu-id="7bbe5-229">**Otázka: Je možné vytvořit zabezpečené připojení LDAP se službou Azure Active Directory?**</span><span class="sxs-lookup"><span data-stu-id="7bbe5-229">**Q:  Can I set up a secure LDAP connection with Azure AD?**</span></span>

<span data-ttu-id="7bbe5-230">**Odpověď:** Ne.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-230">**A:**  No.</span></span> <span data-ttu-id="7bbe5-231">Azure AD nepodporuje protokol LDAP hello.</span><span class="sxs-lookup"><span data-stu-id="7bbe5-231">Azure AD does not support hello LDAP protocol.</span></span>
