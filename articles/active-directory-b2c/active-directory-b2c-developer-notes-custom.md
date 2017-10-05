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
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a><span data-ttu-id="522f2-103">Poznámky k verzi pro verzi public preview služby Azure Active Directory B2C vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="522f2-103">Release notes for Azure Active Directory B2C custom policy public preview</span></span>
<span data-ttu-id="522f2-104">Sada funkcí vlastních zásad je nyní k dispozici pro vyhodnocení v rámci verze public preview pro všechny Azure Active Directory B2C zákazníků (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="522f2-104">The custom policy feature set is now available for evaluation under public preview for all Azure Active Directory B2C (Azure AD B2C) customers.</span></span> <span data-ttu-id="522f2-105">Tato sada funkcí je cílena na vývojáře pokročilé identity vytváření nejvíce komplexní řešení identity.</span><span class="sxs-lookup"><span data-stu-id="522f2-105">This feature set is targeted at advanced identity developers building the most complex identity solutions.</span></span>  

<span data-ttu-id="522f2-106">Tato sada funkcí v současné době vyžadují vývojářům konfigurovat rozhraní Identity prostředí přímo prostřednictvím úpravy souborů XML.</span><span class="sxs-lookup"><span data-stu-id="522f2-106">Today, this feature set requires developers to configure the Identity Experience Framework directly via XML file editing.</span></span> <span data-ttu-id="522f2-107">Tato metoda konfigurace je výkonná a komplexní.</span><span class="sxs-lookup"><span data-stu-id="522f2-107">This method of configuration is powerful and complex.</span></span> <span data-ttu-id="522f2-108">Pokročilé vývojáře, kteří používají rozhraní Identity prostředí měli naplánovat investovat chvíli dokončení návodů a čtení dokumenty referenční identity.</span><span class="sxs-lookup"><span data-stu-id="522f2-108">Advanced identity developers using the Identity Experience Framework should plan to invest some time completing walk-throughs and reading reference documents.</span></span> 

## <a name="features-included-in-this-public-preview"></a><span data-ttu-id="522f2-109">Funkce obsažené v této verzi public preview</span><span class="sxs-lookup"><span data-stu-id="522f2-109">Features included in this public preview</span></span>
<span data-ttu-id="522f2-110">Nové funkce, zavedená ve verzi public preview vývojáři můžete provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="522f2-110">With the new features introduced in the public preview, developers can perform the following tasks:</span></span><br>

* <span data-ttu-id="522f2-111">Autor a nahrání vlastní ověřování uživatele cesty pomocí vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="522f2-111">Author and upload custom authentication user journeys by using custom policies.</span></span> 
   * <span data-ttu-id="522f2-112">Popis uživatelské cesty podrobné jako výměny mezi zprostředkovatelem deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="522f2-112">Describe user journeys step-by-step as exchanges between claims providers.</span></span> 
   * <span data-ttu-id="522f2-113">Definujte podmíněného větvení v cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="522f2-113">Define conditional branching in user journeys.</span></span> 
* <span data-ttu-id="522f2-114">Integrate povoleno rozhraní REST API služby ve vaší vlastní ověřování uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="522f2-114">Integrate REST API-enabled services in your custom authentication user journeys.</span></span>  
* <span data-ttu-id="522f2-115">Přidejte federaci se zprostředkovatelů identity, které jsou kompatibilní s standardní OpenIDConnect.</span><span class="sxs-lookup"><span data-stu-id="522f2-115">Add federation with identity providers that are compliant with the OpenIDConnect standard.</span></span> <br>
* <span data-ttu-id="522f2-116">Přidejte federaci se zprostředkovatelů identity, které dodržovat protokolu SAML verze 2.0.</span><span class="sxs-lookup"><span data-stu-id="522f2-116">Add federation with identity providers that adhere to the SAML 2.0 protocol.</span></span> 

## <a name="terms-of-the-public-preview"></a><span data-ttu-id="522f2-117">Podmínky verze public Preview</span><span class="sxs-lookup"><span data-stu-id="522f2-117">Terms of the public preview</span></span>

* <span data-ttu-id="522f2-118">Doporučujeme vám používat nové funkce jenom pro účely hodnocení.</span><span class="sxs-lookup"><span data-stu-id="522f2-118">We encourage you to use the new features for evaluation purposes only.</span></span><br>
* <span data-ttu-id="522f2-119">Nové funkce nejsou určeny pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="522f2-119">The new features are not intended for use in a production environment.</span></span><br>
* <span data-ttu-id="522f2-120">Smlouvy o úrovni služeb (SLA) se nevztahují na nové funkce.</span><span class="sxs-lookup"><span data-stu-id="522f2-120">Service level agreements (SLAs) do not apply to the new features.</span></span> <br>
* <span data-ttu-id="522f2-121">Žádosti o podporu můžete podává prostřednictvím regulární podpora kanálů.</span><span class="sxs-lookup"><span data-stu-id="522f2-121">Support requests can be filed through regular support channels.</span></span> <br>
* <span data-ttu-id="522f2-122">Neexistuje žádné přislíbeném datum pro obecnou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="522f2-122">There is no promised date for general availability.</span></span><br>
* <span data-ttu-id="522f2-123">Naše uvážení a z jakéhokoli důvodu můžete Microsoft příznak a odmítnout nebo omezit scénáře a cesty uživatele, které překračují oboru listinou produktu Azure AD B2C sloužit jako platforma správy (CIAM) přístupu a identit zákazníka.</span><span class="sxs-lookup"><span data-stu-id="522f2-123">At our discretion, and for any reason, Microsoft can flag and reject or restrict scenarios and user journeys that exceed the scope of the Azure AD B2C product charter to serve as a customer identity and access management (CIAM) platform.</span></span>

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a><span data-ttu-id="522f2-124">Odpovědnosti vlastních zásad pro sadu funkcí vývojářů</span><span class="sxs-lookup"><span data-stu-id="522f2-124">Responsibilities of custom policy feature-set developers</span></span>
<span data-ttu-id="522f2-125">Konfigurace zásad ruční uděluje nižší úrovně přístupu pro základní platformu Azure AD B2C a má za následek vytvoření rozhraní jedinečný plně přizpůsobitelná vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="522f2-125">Manual policy configuration grants lower-level access to the underlying platform of Azure AD B2C and results in the creation of a unique, fully customizable trust framework.</span></span> <span data-ttu-id="522f2-126">Počet možných kombinací zprostředkovatelů vlastní identity, vztahy důvěryhodnosti, integrace s externích služeb a podrobné pracovních umístit větší nároky na pokročilé vývojáře využívají je.</span><span class="sxs-lookup"><span data-stu-id="522f2-126">The possible permutations of custom identity providers, trust relationships, integrations with external services, and step-by-step workflows place greater demands on the advanced developers consuming them.</span></span>

<span data-ttu-id="522f2-127">Chcete-li plně využívat výhod verzi public preview, doporučujeme, aby vývojáři využívání sadu funkcí vlastní zásady dodržovat následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="522f2-127">To fully benefit from the public preview, we suggest that developers consuming the custom policy feature set adhere to the following guidelines:</span></span>
* <span data-ttu-id="522f2-128">Seznamte se s jazykem konfigurace modulu prostředí Identity a řízení tajné klíče nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="522f2-128">Become familiar with the configuration language of the Identity Experience Engine and key/secrets management.</span></span>
* <span data-ttu-id="522f2-129">Převzít vlastnictví scénáře a vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="522f2-129">Take ownership of scenarios and custom integrations.</span></span>
* <span data-ttu-id="522f2-130">Proveďte testování metodický scénář.</span><span class="sxs-lookup"><span data-stu-id="522f2-130">Perform methodical scenario testing.</span></span>
* <span data-ttu-id="522f2-131">Postupujte podle vývoj softwaru a pracovní osvědčené postupy s minimálně jeden vývoj a testování prostředí a jeden produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="522f2-131">Follow software development and staging best practices with a minimum of one development and testing environment and one production environment.</span></span>
* <span data-ttu-id="522f2-132">Udržení informovanosti o novém vývoji z poskytovatelů identit a službách, které můžete integrovat.</span><span class="sxs-lookup"><span data-stu-id="522f2-132">Stay informed about new developments from the identity providers and services you integrate with.</span></span> <span data-ttu-id="522f2-133">Například sledovat určité změny v tajných klíčů a plánovaných a neplánovaných změn ke službě.</span><span class="sxs-lookup"><span data-stu-id="522f2-133">For example, keep track of changes in secrets and of scheduled and unscheduled changes to the service.</span></span>
* <span data-ttu-id="522f2-134">Nastavte aktivní monitorování a sledování odezvy provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="522f2-134">Set up active monitoring, and monitor the responsiveness of production environments.</span></span>
* <span data-ttu-id="522f2-135">Zachovat aktuální kontaktní e-mailové adresy a zůstat reaguje na e-mailů team Web live společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="522f2-135">Keep contact email addresses current, and stay responsive to the Microsoft live-site team emails.</span></span>
* <span data-ttu-id="522f2-136">Trvat včas akce při nedoporučuje Uděláte to tak tým Microsoft live-site.</span><span class="sxs-lookup"><span data-stu-id="522f2-136">Take timely action when advised to do so by the Microsoft live-site team.</span></span> 


>[!NOTE]
><span data-ttu-id="522f2-137">Tyto funkce možná časem zahrneme v integrovaných zásad služby Azure AD, přitom přístupnější pro všechny vývojáře.</span><span class="sxs-lookup"><span data-stu-id="522f2-137">These features might eventually be included in Azure AD built-in policies, making them more accessible to all developers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="522f2-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="522f2-138">Next steps</span></span>
<span data-ttu-id="522f2-139">[Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="522f2-139">[Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>
