---
title: "aaaCustomizing mapování atributů Azure AD | Microsoft Docs"
description: "Zjistěte, co mapování atributů pro aplikace SaaS ve službě Azure Active Directory se, jak můžete je upravit tooaddress váš podnik potřebuje."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14db5303f06fc8df3b07a0a8b75713312e71bbfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a><span data-ttu-id="0da7e-103">Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů</span><span class="sxs-lookup"><span data-stu-id="0da7e-103">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>
<span data-ttu-id="0da7e-104">Microsoft Azure AD poskytuje podporu pro zřizování aplikace SaaS toothird výrobců jako Salesforce, Google Apps a dalších uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0da7e-104">Microsoft Azure AD provides support for user provisioning toothird-party SaaS applications such as Salesforce, Google Apps and others.</span></span> <span data-ttu-id="0da7e-105">Pokud máte uživatele zřizování pro aplikace SaaS třetích stran povoleno, hello portálu pro správu Azure prvky jeho hodnot atributů v podobě konfigurace s názvem "mapování atributů."</span><span class="sxs-lookup"><span data-stu-id="0da7e-105">If you have user provisioning for a third-party SaaS application enabled, hello Azure Management Portal controls its attribute values in form of a configuration called “attribute mapping.”</span></span>

<span data-ttu-id="0da7e-106">Není předkonfigurované sadu atributů mapování mezi objekty uživatele Azure AD a každá aplikace SaaS uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="0da7e-106">There is a preconfigured set of attribute mappings between Azure AD user objects and each SaaS app’s user objects.</span></span> <span data-ttu-id="0da7e-107">Některé aplikace spravovat jiné typy objektů, jako jsou skupiny nebo kontakty.</span><span class="sxs-lookup"><span data-stu-id="0da7e-107">Some apps manage other types of objects, such as Groups or Contacts.</span></span> <br> 
 <span data-ttu-id="0da7e-108">Můžete upravit mapování atributů výchozí hello podle tooyour obchodním potřebám.</span><span class="sxs-lookup"><span data-stu-id="0da7e-108">You can customize hello default attribute mappings according tooyour business needs.</span></span> <span data-ttu-id="0da7e-109">To znamená, můžete změnit nebo odstranit existující mapování atributů nebo vytvořte nové mapování atributů.</span><span class="sxs-lookup"><span data-stu-id="0da7e-109">This means, you can change or delete existing attribute mappings or create new attribute mappings.</span></span>

<span data-ttu-id="0da7e-110">Portálu hello Azure AD, dostanete tuto funkci kliknutím **mapování** konfigurace v **zřizování** v hello **spravovat** části  **Podniková aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0da7e-110">In hello Azure AD portal, you can access this feature by clicking a **Mappings** configuration under **Provisioning** in hello **Manage** section of an **Enterprise application**.</span></span>


![Salesforce][5] 

<span data-ttu-id="0da7e-112">Kliknutím **mapování** otevření hello související konfigurace, **mapování atributů** okno.</span><span class="sxs-lookup"><span data-stu-id="0da7e-112">Clicking a **Mappings** configuration, opens hello related **Attribute Mapping** blade.</span></span>  
<span data-ttu-id="0da7e-113">Existují mapování atributů, které jsou vyžadované toofunction aplikace SaaS správně.</span><span class="sxs-lookup"><span data-stu-id="0da7e-113">There are attribute mappings that are required by a SaaS application toofunction correctly.</span></span> <span data-ttu-id="0da7e-114">Povinné atributy hello **odstranit** funkce není dostupná.</span><span class="sxs-lookup"><span data-stu-id="0da7e-114">For required attributes, hello **Delete** feature is unavailable.</span></span>


![Salesforce][6]  

<span data-ttu-id="0da7e-116">V předchozím příkladu hello, uvidíte, že hello **uživatelské jméno** atribut spravovaného objektu v Salesforce se zobrazí v hello **userPrincipalName** hodnotu hello propojené objekt Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0da7e-116">In hello example above, you can see that hello **Username** attribute of a managed object in Salesforce is populated with hello **userPrincipalName** value of hello linked Azure Active Directory Object.</span></span>

<span data-ttu-id="0da7e-117">Můžete upravit existující **mapování atributů** kliknutím mapování.</span><span class="sxs-lookup"><span data-stu-id="0da7e-117">You can customize existing **Attribute Mappings** by clicking a mapping.</span></span> <span data-ttu-id="0da7e-118">Tím se otevře hello **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="0da7e-118">This opens hello **Edit Attribute** blade.</span></span>

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a><span data-ttu-id="0da7e-120">Principy typů mapování atributů</span><span class="sxs-lookup"><span data-stu-id="0da7e-120">Understanding attribute mapping types</span></span>
<span data-ttu-id="0da7e-121">S mapování atributů řídit, jak jsou naplněny atributy v aplikaci SaaS jiných výrobců.</span><span class="sxs-lookup"><span data-stu-id="0da7e-121">With attribute mappings, you control how attributes are populated in a third-party SaaS application.</span></span> <span data-ttu-id="0da7e-122">Existují čtyři typy různých mapování podporované:</span><span class="sxs-lookup"><span data-stu-id="0da7e-122">There are four different mapping types supported:</span></span>

* <span data-ttu-id="0da7e-123">**Přímé** – atribut target hello naplněný hello hodnotu atributu hello připojeného objektu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0da7e-123">**Direct** – hello target attribute is populated with hello value of an attribute of hello linked object in Azure AD.</span></span>
* <span data-ttu-id="0da7e-124">**Konstantní** – atribut target hello se zobrazí v konkrétní řetězec, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="0da7e-124">**Constant** – hello target attribute is populated with a specific string you have specified.</span></span>
* <span data-ttu-id="0da7e-125">**Výraz** -atribut target hello se naplní na základě výsledku hello skriptu jako výrazu.</span><span class="sxs-lookup"><span data-stu-id="0da7e-125">**Expression** - hello target attribute is populated based on hello result of a script-like expression.</span></span> 
  <span data-ttu-id="0da7e-126">Další informace najdete v tématu [zápis výrazy pro mapování atributů v Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="0da7e-126">For more information, see [Writing Expressions for Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>
* <span data-ttu-id="0da7e-127">**Žádný** -atribut target hello je ponechán beze změny.</span><span class="sxs-lookup"><span data-stu-id="0da7e-127">**None** - hello target attribute is left unmodified.</span></span> <span data-ttu-id="0da7e-128">Ale pokud atribut target hello je někdy prázdná, je naplněna s hello výchozí hodnotu, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="0da7e-128">However, if hello target attribute is ever empty, it is populated with hello Default value that you specify.</span></span>

<span data-ttu-id="0da7e-129">Přidání toothese čtyři základní atribut mapování typů, mapování vlastních atributů podporují hello koncept volitelný **výchozí** přiřazení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0da7e-129">In addition toothese four basic attribute mapping types, custom attribute mappings support hello concept of an optional **default** value assignment.</span></span> <span data-ttu-id="0da7e-130">Přiřazení hodnoty výchozí Hello zajistí, že atribut target je vyplněný s hodnotou, když není ani jedna hodnota ve službě Azure AD ani na hello cílový objekt.</span><span class="sxs-lookup"><span data-stu-id="0da7e-130">hello default value assignment ensures that a target attribute is populated with a value if there is neither a value in Azure AD nor on hello target object.</span></span> <span data-ttu-id="0da7e-131">Nejběžnější konfigurace Hello je tooleave tomto prázdné.</span><span class="sxs-lookup"><span data-stu-id="0da7e-131">hello most common configuration is tooleave this blank.</span></span>


## <a name="understanding-attribute-mapping-properties"></a><span data-ttu-id="0da7e-132">Principy vlastnosti mapování atributů</span><span class="sxs-lookup"><span data-stu-id="0da7e-132">Understanding attribute mapping properties</span></span>

<span data-ttu-id="0da7e-133">V předchozí části hello již jste vlastnost Typ mapování přináší toohello atributu.</span><span class="sxs-lookup"><span data-stu-id="0da7e-133">In hello previous section, you have already been introduced toohello attribute mapping type property.</span></span>
<span data-ttu-id="0da7e-134">V přidání vlastnosti toothis mapování atributů také podporují hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="0da7e-134">In addition toothis property, attribute mappings do also support hello following attributes:</span></span>

- <span data-ttu-id="0da7e-135">**Zdrojový atribut** -hello atribut uživatele ze zdrojového systému hello (např: Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="0da7e-135">**Source attribute** - hello user attribute from hello source system (e.g.: Azure Active Directory).</span></span>
- <span data-ttu-id="0da7e-136">**Atribut target** – atribut hello uživatele v cílovém systému hello (např: ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="0da7e-136">**Target attribute** – hello user attribute in hello target system (e.g.: ServiceNow).</span></span>
- <span data-ttu-id="0da7e-137">**Objekty používající tento atribut odpovídat** – zda by měl použít toto mapování toouniquely identifikaci uživatelů mezi zdrojovými a cílovými systémy hello.</span><span class="sxs-lookup"><span data-stu-id="0da7e-137">**Match objects using this attribute** – Whether or not this mapping should be used toouniquely identify users between hello source and target systems.</span></span> <span data-ttu-id="0da7e-138">To se obvykle nastavuje na hello userPrincipalName nebo atribut mail v Azure AD, což je většinou mapována tooa pole pro uživatelské jméno v cílové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0da7e-138">This is typically set on hello userPrincipalName or mail attribute in Azure AD, which is typically mapped tooa username field in a target application.</span></span>
- <span data-ttu-id="0da7e-139">**Odpovídající prioritu** – více odpovídající atributy lze nastavit.</span><span class="sxs-lookup"><span data-stu-id="0da7e-139">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="0da7e-140">Pokud existuje více vyhodnocují se v pořadí hello definované v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="0da7e-140">When there are multiple, they are evaluated in hello order defined by this field.</span></span> <span data-ttu-id="0da7e-141">Jakmile je nalezena shoda, žádné další odpovídající atributy vyhodnocují.</span><span class="sxs-lookup"><span data-stu-id="0da7e-141">As soon as a match is found, no further matching attributes are evaluated.</span></span>
- <span data-ttu-id="0da7e-142">**Použít toto mapování**</span><span class="sxs-lookup"><span data-stu-id="0da7e-142">**Apply this mapping**</span></span>
    - <span data-ttu-id="0da7e-143">**Vždy** – použít toto mapování na obou vytvoření uživatele a aktualizovat akce</span><span class="sxs-lookup"><span data-stu-id="0da7e-143">**Always** – Apply this mapping on both user creation and update actions</span></span>
    - <span data-ttu-id="0da7e-144">**Pouze během vytváření** -použít toto mapování pouze na akcí vytvoření uživatele</span><span class="sxs-lookup"><span data-stu-id="0da7e-144">**Only during creation** - Apply this mapping only on user creation actions</span></span>


## <a name="what-you-should-know"></a><span data-ttu-id="0da7e-145">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="0da7e-145">What you should know</span></span>

<span data-ttu-id="0da7e-146">Microsoft Azure AD poskytuje efektivní implementaci procesu synchronizace.</span><span class="sxs-lookup"><span data-stu-id="0da7e-146">Microsoft Azure AD provides an efficient implementation of a synchronization process.</span></span> <span data-ttu-id="0da7e-147">V prostředí inicializovaného jsou zpracovány pouze objekty, které vyžadují aktualizace při synchronizační cyklus.</span><span class="sxs-lookup"><span data-stu-id="0da7e-147">In an initialized environment, only objects requiring updates are processed during a synchronization cycle.</span></span> <span data-ttu-id="0da7e-148">Aktualizace mapování atributů má vliv na výkon hello synchronizační cyklus.</span><span class="sxs-lookup"><span data-stu-id="0da7e-148">Updating attribute mappings has an impact on hello performance of a synchronization cycle.</span></span> <span data-ttu-id="0da7e-149">Aktualizaci toohello atributů mapování vyžaduje všechny spravované objekty toobe již znovu.</span><span class="sxs-lookup"><span data-stu-id="0da7e-149">An update toohello attribute mapping configuration requires all managed objects toobe reevaluated.</span></span> <span data-ttu-id="0da7e-150">Je doporučené nejlepší postup tookeep hello několika po sobě jdoucích změny mapování atributů tooyour minimálně.</span><span class="sxs-lookup"><span data-stu-id="0da7e-150">It is a recommended best practice tookeep hello number of consecutive changes tooyour attribute mappings at a minimum.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0da7e-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0da7e-151">Next steps</span></span>

* [<span data-ttu-id="0da7e-152">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0da7e-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="0da7e-153">Automatizace zřizování uživatelů nebo jeho rušení tooSaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="0da7e-153">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="0da7e-154">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="0da7e-154">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="0da7e-155">Filtry pro zřizování uživatelů oborů</span><span class="sxs-lookup"><span data-stu-id="0da7e-155">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="0da7e-156">Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="0da7e-156">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="0da7e-157">Účet zřizování oznámení</span><span class="sxs-lookup"><span data-stu-id="0da7e-157">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="0da7e-158">Seznam kurzů tooIntegrate aplikace SaaS</span><span class="sxs-lookup"><span data-stu-id="0da7e-158">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

