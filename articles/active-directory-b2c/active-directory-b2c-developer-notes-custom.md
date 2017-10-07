---
title: "Azure Active Directory B2C: Poznámky pro vývojáře na pomocí vlastních zásad | Microsoft Docs"
description: "Poznámky pro vývojáře o konfiguraci a údržbu Azure AD B2C pomocí vlastních zásad"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: 979b8a264eb819ee4a208b9171a53a5ffbf062c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a><span data-ttu-id="048e5-103">Poznámky k verzi pro verzi public preview služby Azure Active Directory B2C vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="048e5-103">Release notes for Azure Active Directory B2C custom policy public preview</span></span>
<span data-ttu-id="048e5-104">Hello sada funkcí vlastních zásad je nyní k dispozici pro vyhodnocení v rámci verze public preview pro všechny Azure Active Directory B2C zákazníků (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="048e5-104">hello custom policy feature set is now available for evaluation under public preview for all Azure Active Directory B2C (Azure AD B2C) customers.</span></span> <span data-ttu-id="048e5-105">Tato sada funkcí je cílena na vývojáře pokročilé identity vytváření hello nejvíce komplexní řešení identity.</span><span class="sxs-lookup"><span data-stu-id="048e5-105">This feature set is targeted at advanced identity developers building hello most complex identity solutions.</span></span>  

<span data-ttu-id="048e5-106">Tato sada funkcí v současné době vyžadují vývojáři tooconfigure hello Identity rozhraní Framework přímo prostřednictvím úpravy souborů XML.</span><span class="sxs-lookup"><span data-stu-id="048e5-106">Today, this feature set requires developers tooconfigure hello Identity Experience Framework directly via XML file editing.</span></span> <span data-ttu-id="048e5-107">Tato metoda konfigurace je výkonná a komplexní.</span><span class="sxs-lookup"><span data-stu-id="048e5-107">This method of configuration is powerful and complex.</span></span> <span data-ttu-id="048e5-108">Pokročilé vývojáře identity, kteří používají hello Identity Framework prostředí měli naplánovat tooinvest chvíli dokončení návodů a čtení odkaz dokumenty.</span><span class="sxs-lookup"><span data-stu-id="048e5-108">Advanced identity developers using hello Identity Experience Framework should plan tooinvest some time completing walk-throughs and reading reference documents.</span></span> 

## <a name="features-included-in-this-public-preview"></a><span data-ttu-id="048e5-109">Funkce obsažené v této verzi public preview</span><span class="sxs-lookup"><span data-stu-id="048e5-109">Features included in this public preview</span></span>
<span data-ttu-id="048e5-110">Hello nové funkce zavedená ve verzi public preview hello vývojáři mohou provádět hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="048e5-110">With hello new features introduced in hello public preview, developers can perform hello following tasks:</span></span><br>

* <span data-ttu-id="048e5-111">Autor a nahrání vlastní ověřování uživatele cesty pomocí vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="048e5-111">Author and upload custom authentication user journeys by using custom policies.</span></span> 
   * <span data-ttu-id="048e5-112">Popis uživatelské cesty podrobné jako výměny mezi zprostředkovatelem deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="048e5-112">Describe user journeys step-by-step as exchanges between claims providers.</span></span> 
   * <span data-ttu-id="048e5-113">Definujte podmíněného větvení v cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="048e5-113">Define conditional branching in user journeys.</span></span> 
* <span data-ttu-id="048e5-114">Integrate povoleno rozhraní REST API služby ve vaší vlastní ověřování uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="048e5-114">Integrate REST API-enabled services in your custom authentication user journeys.</span></span>  
* <span data-ttu-id="048e5-115">Přidejte federaci se zprostředkovatelů identity, které jsou kompatibilní s hello standardní OpenIDConnect.</span><span class="sxs-lookup"><span data-stu-id="048e5-115">Add federation with identity providers that are compliant with hello OpenIDConnect standard.</span></span> <br>
* <span data-ttu-id="048e5-116">Přidejte federaci se zprostředkovatelů identity, které splňovat protokol toohello SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="048e5-116">Add federation with identity providers that adhere toohello SAML 2.0 protocol.</span></span> 

## <a name="terms-of-hello-public-preview"></a><span data-ttu-id="048e5-117">Podmínky verze public Preview hello</span><span class="sxs-lookup"><span data-stu-id="048e5-117">Terms of hello public preview</span></span>

* <span data-ttu-id="048e5-118">Doporučujeme vám toouse hello nové funkce jenom pro účely hodnocení.</span><span class="sxs-lookup"><span data-stu-id="048e5-118">We encourage you toouse hello new features for evaluation purposes only.</span></span><br>
* <span data-ttu-id="048e5-119">Nové funkce nejsou určeny pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="048e5-119">The new features are not intended for use in a production environment.</span></span><br>
* <span data-ttu-id="048e5-120">Smlouvy o úrovni služeb (SLA) se nevztahují toohello nové funkce.</span><span class="sxs-lookup"><span data-stu-id="048e5-120">Service level agreements (SLAs) do not apply toohello new features.</span></span> <br>
* <span data-ttu-id="048e5-121">Žádosti o podporu můžete podává prostřednictvím regulární podpora kanálů.</span><span class="sxs-lookup"><span data-stu-id="048e5-121">Support requests can be filed through regular support channels.</span></span> <br>
* <span data-ttu-id="048e5-122">Neexistuje žádné přislíbeném datum pro obecnou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="048e5-122">There is no promised date for general availability.</span></span><br>
* <span data-ttu-id="048e5-123">Naše uvážení a z jakéhokoli důvodu můžete Microsoft příznak a odmítnout nebo omezit scénáře a cesty uživatele, které překračují hello oboru tooserve listinou produktu hello Azure AD B2C jako platforma správy (CIAM) přístupu a identit zákazníka.</span><span class="sxs-lookup"><span data-stu-id="048e5-123">At our discretion, and for any reason, Microsoft can flag and reject or restrict scenarios and user journeys that exceed hello scope of hello Azure AD B2C product charter tooserve as a customer identity and access management (CIAM) platform.</span></span>

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a><span data-ttu-id="048e5-124">Odpovědnosti vlastních zásad pro sadu funkcí vývojářů</span><span class="sxs-lookup"><span data-stu-id="048e5-124">Responsibilities of custom policy feature-set developers</span></span>
<span data-ttu-id="048e5-125">Konfigurace zásad ruční uděluje přístup na nižší úrovni toohello základní platformu Azure AD B2C a výsledkem hello vytvoření rozhraní jedinečný plně přizpůsobitelná vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="048e5-125">Manual policy configuration grants lower-level access toohello underlying platform of Azure AD B2C and results in hello creation of a unique, fully customizable trust framework.</span></span> <span data-ttu-id="048e5-126">Počet možných kombinací zprostředkovatelů vlastní identity, vztahy důvěryhodnosti, integrace s externích služeb a podrobné pracovních umístit větší nároky na hello pokročilé vývojáře využívají je.</span><span class="sxs-lookup"><span data-stu-id="048e5-126">The possible permutations of custom identity providers, trust relationships, integrations with external services, and step-by-step workflows place greater demands on hello advanced developers consuming them.</span></span>

<span data-ttu-id="048e5-127">výhody toofully z hello veřejné verze preview, doporučujeme, aby vývojáři využívání sada funkcí vlastní zásady hello splňovat toohello následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="048e5-127">toofully benefit from hello public preview, we suggest that developers consuming hello custom policy feature set adhere toohello following guidelines:</span></span>
* <span data-ttu-id="048e5-128">Seznámení s hello konfigurace jazyk hello modul prostředí Identity a správu tajné klíče nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="048e5-128">Become familiar with hello configuration language of hello Identity Experience Engine and key/secrets management.</span></span>
* <span data-ttu-id="048e5-129">Převzít vlastnictví scénáře a vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="048e5-129">Take ownership of scenarios and custom integrations.</span></span>
* <span data-ttu-id="048e5-130">Proveďte testování metodický scénář.</span><span class="sxs-lookup"><span data-stu-id="048e5-130">Perform methodical scenario testing.</span></span>
* <span data-ttu-id="048e5-131">Postupujte podle vývoj softwaru a pracovní osvědčené postupy s minimálně jeden vývoj a testování prostředí a jeden produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="048e5-131">Follow software development and staging best practices with a minimum of one development and testing environment and one production environment.</span></span>
* <span data-ttu-id="048e5-132">Udržení informovanosti o novém vývoji z hello poskytovatelů identit a službách, které můžete integrovat.</span><span class="sxs-lookup"><span data-stu-id="048e5-132">Stay informed about new developments from hello identity providers and services you integrate with.</span></span> <span data-ttu-id="048e5-133">Například sledovat určité změny v tajných klíčů a plánovaných a neplánovaných změny toohello služby.</span><span class="sxs-lookup"><span data-stu-id="048e5-133">For example, keep track of changes in secrets and of scheduled and unscheduled changes toohello service.</span></span>
* <span data-ttu-id="048e5-134">Nastavte aktivní monitorování a monitorovat rychlost reakce hello produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="048e5-134">Set up active monitoring, and monitor hello responsiveness of production environments.</span></span>
* <span data-ttu-id="048e5-135">Zachovat aktuální kontaktní e-mailové adresy a zůstat přizpůsobivý toohello Microsoft team Web live e-mailů.</span><span class="sxs-lookup"><span data-stu-id="048e5-135">Keep contact email addresses current, and stay responsive toohello Microsoft live-site team emails.</span></span>
* <span data-ttu-id="048e5-136">Akce včas při doporučené toodo tak, že hello týmu Microsoft live-site.</span><span class="sxs-lookup"><span data-stu-id="048e5-136">Take timely action when advised toodo so by hello Microsoft live-site team.</span></span> 


>[!NOTE]
><span data-ttu-id="048e5-137">Tyto funkce možná časem zahrneme v integrovaných zásad služby Azure AD, přitom přístupnější tooall vývojáři.</span><span class="sxs-lookup"><span data-stu-id="048e5-137">These features might eventually be included in Azure AD built-in policies, making them more accessible tooall developers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="048e5-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="048e5-138">Next steps</span></span>
<span data-ttu-id="048e5-139">[Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="048e5-139">[Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>
