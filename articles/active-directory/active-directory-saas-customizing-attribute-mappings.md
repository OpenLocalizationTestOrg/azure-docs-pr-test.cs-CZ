---
title: "Přizpůsobení mapování atributů Azure AD | Microsoft Docs"
description: "Zjistěte, co mapování atributů pro aplikace SaaS ve službě Azure Active Directory se, jak je vyřešit obchodních potřeb můžete upravit."
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
ms.openlocfilehash: 6ca2fdc9c68ea0030d938eeaebd57aafa0e2790f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a><span data-ttu-id="7ba1e-103">Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů</span><span class="sxs-lookup"><span data-stu-id="7ba1e-103">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>
<span data-ttu-id="7ba1e-104">Microsoft Azure AD poskytuje podporu pro zřizování uživatelů pro aplikace SaaS jiných výrobců jako Salesforce, Google Apps a dalších.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-104">Microsoft Azure AD provides support for user provisioning to third-party SaaS applications such as Salesforce, Google Apps and others.</span></span> <span data-ttu-id="7ba1e-105">Pokud máte uživatele zřizování pro aplikace SaaS třetích stran povoleno, portálu pro správu Azure prvky jeho hodnot atributů v podobě konfigurace s názvem "mapování atributů."</span><span class="sxs-lookup"><span data-stu-id="7ba1e-105">If you have user provisioning for a third-party SaaS application enabled, the Azure Management Portal controls its attribute values in form of a configuration called “attribute mapping.”</span></span>

<span data-ttu-id="7ba1e-106">Není předkonfigurované sadu atributů mapování mezi objekty uživatele Azure AD a každá aplikace SaaS uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-106">There is a preconfigured set of attribute mappings between Azure AD user objects and each SaaS app’s user objects.</span></span> <span data-ttu-id="7ba1e-107">Některé aplikace spravovat jiné typy objektů, jako jsou skupiny nebo kontakty.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-107">Some apps manage other types of objects, such as Groups or Contacts.</span></span> <br> 
 <span data-ttu-id="7ba1e-108">Mapování atributů výchozí můžete přizpůsobit podle obchodních potřeb.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-108">You can customize the default attribute mappings according to your business needs.</span></span> <span data-ttu-id="7ba1e-109">To znamená, můžete změnit nebo odstranit existující mapování atributů nebo vytvořte nové mapování atributů.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-109">This means, you can change or delete existing attribute mappings or create new attribute mappings.</span></span>

<span data-ttu-id="7ba1e-110">Na portálu Azure AD, dostanete tuto funkci kliknutím **mapování** konfigurace v **zřizování** v **spravovat** části  **Podniková aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-110">In the Azure AD portal, you can access this feature by clicking a **Mappings** configuration under **Provisioning** in the **Manage** section of an **Enterprise application**.</span></span>


![Salesforce][5] 

<span data-ttu-id="7ba1e-112">Kliknutím **mapování** otevření související konfigurace, **mapování atributů** okno.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-112">Clicking a **Mappings** configuration, opens the related **Attribute Mapping** blade.</span></span>  
<span data-ttu-id="7ba1e-113">Existují mapování atributů vyžadované aplikací SaaS fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-113">There are attribute mappings that are required by a SaaS application to function correctly.</span></span> <span data-ttu-id="7ba1e-114">Pro povinné atributy **odstranit** funkce není dostupná.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-114">For required attributes, the **Delete** feature is unavailable.</span></span>


![Salesforce][6]  

<span data-ttu-id="7ba1e-116">V předchozím příkladu můžete uvidíte, že **uživatelské jméno** se zobrazí v atributu spravovaného objektu v Salesforce **userPrincipalName** hodnota propojené objekt služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-116">In the example above, you can see that the **Username** attribute of a managed object in Salesforce is populated with the **userPrincipalName** value of the linked Azure Active Directory Object.</span></span>

<span data-ttu-id="7ba1e-117">Můžete upravit existující **mapování atributů** kliknutím mapování.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-117">You can customize existing **Attribute Mappings** by clicking a mapping.</span></span> <span data-ttu-id="7ba1e-118">Tím se otevře **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-118">This opens the **Edit Attribute** blade.</span></span>

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a><span data-ttu-id="7ba1e-120">Principy typů mapování atributů</span><span class="sxs-lookup"><span data-stu-id="7ba1e-120">Understanding attribute mapping types</span></span>
<span data-ttu-id="7ba1e-121">S mapování atributů řídit, jak jsou naplněny atributy v aplikaci SaaS jiných výrobců.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-121">With attribute mappings, you control how attributes are populated in a third-party SaaS application.</span></span> <span data-ttu-id="7ba1e-122">Existují čtyři typy různých mapování podporované:</span><span class="sxs-lookup"><span data-stu-id="7ba1e-122">There are four different mapping types supported:</span></span>

* <span data-ttu-id="7ba1e-123">**Přímé** – atribut target naplněný hodnotu atributu propojeného objektu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-123">**Direct** – the target attribute is populated with the value of an attribute of the linked object in Azure AD.</span></span>
* <span data-ttu-id="7ba1e-124">**Konstantní** – cílový atribut je naplněna konkrétní řetězec, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-124">**Constant** – the target attribute is populated with a specific string you have specified.</span></span>
* <span data-ttu-id="7ba1e-125">**Výraz** -cílový atribut je vyplněný, na základě výsledku skriptu jako výraz.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-125">**Expression** - the target attribute is populated based on the result of a script-like expression.</span></span> 
  <span data-ttu-id="7ba1e-126">Další informace najdete v tématu [zápis výrazy pro mapování atributů v Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="7ba1e-126">For more information, see [Writing Expressions for Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>
* <span data-ttu-id="7ba1e-127">**Žádný** -cílový atribut je ponechán beze změny.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-127">**None** - the target attribute is left unmodified.</span></span> <span data-ttu-id="7ba1e-128">Ale pokud cílový atribut je někdy prázdná, je naplněna s výchozí hodnotou, který určíte.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-128">However, if the target attribute is ever empty, it is populated with the Default value that you specify.</span></span>

<span data-ttu-id="7ba1e-129">Kromě těchto čtyř typů mapování základní atribut mapování vlastních atributů podporují koncept volitelný **výchozí** přiřazení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-129">In addition to these four basic attribute mapping types, custom attribute mappings support the concept of an optional **default** value assignment.</span></span> <span data-ttu-id="7ba1e-130">Přiřazení hodnoty výchozí zajistí, že atribut target je vyplněný s hodnotou, když není ani jedna hodnota ve službě Azure AD ani v cílovém objektu.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-130">The default value assignment ensures that a target attribute is populated with a value if there is neither a value in Azure AD nor on the target object.</span></span> <span data-ttu-id="7ba1e-131">Nejběžnější konfigurace je nechte pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-131">The most common configuration is to leave this blank.</span></span>


## <a name="understanding-attribute-mapping-properties"></a><span data-ttu-id="7ba1e-132">Principy vlastnosti mapování atributů</span><span class="sxs-lookup"><span data-stu-id="7ba1e-132">Understanding attribute mapping properties</span></span>

<span data-ttu-id="7ba1e-133">V předchozí části je již byly zavedeny na vlastnost typu mapování atributů.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-133">In the previous section, you have already been introduced to the attribute mapping type property.</span></span>
<span data-ttu-id="7ba1e-134">Kromě tato vlastnost mapování atributů také podporují následující atributy:</span><span class="sxs-lookup"><span data-stu-id="7ba1e-134">In addition to this property, attribute mappings do also support the following attributes:</span></span>

- <span data-ttu-id="7ba1e-135">**Zdrojový atribut** -atribut uživatele ze zdrojového systému (například: Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="7ba1e-135">**Source attribute** - The user attribute from the source system (e.g.: Azure Active Directory).</span></span>
- <span data-ttu-id="7ba1e-136">**Atribut target** – atribut uživatele v cílovém systému (například: ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="7ba1e-136">**Target attribute** – The user attribute in the target system (e.g.: ServiceNow).</span></span>
- <span data-ttu-id="7ba1e-137">**Objekty používající tento atribut odpovídat** – jestli toto mapování se má použít k jednoznačné identifikaci uživatele mezi zdrojovým a cílovým systémem.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-137">**Match objects using this attribute** – Whether or not this mapping should be used to uniquely identify users between the source and target systems.</span></span> <span data-ttu-id="7ba1e-138">To se obvykle nastavuje na atribut userPrincipalName nebo e-mailu ve službě Azure AD, která se obvykle mapuje na pole uživatelské jméno v cílové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-138">This is typically set on the userPrincipalName or mail attribute in Azure AD, which is typically mapped to a username field in a target application.</span></span>
- <span data-ttu-id="7ba1e-139">**Odpovídající prioritu** – více odpovídající atributy lze nastavit.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-139">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="7ba1e-140">Pokud existuje více vyhodnocují se v pořadí podle tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-140">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="7ba1e-141">Jakmile je nalezena shoda, žádné další odpovídající atributy vyhodnocují.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-141">As soon as a match is found, no further matching attributes are evaluated.</span></span>
- <span data-ttu-id="7ba1e-142">**Použít toto mapování**</span><span class="sxs-lookup"><span data-stu-id="7ba1e-142">**Apply this mapping**</span></span>
    - <span data-ttu-id="7ba1e-143">**Vždy** – použít toto mapování na obou vytvoření uživatele a aktualizovat akce</span><span class="sxs-lookup"><span data-stu-id="7ba1e-143">**Always** – Apply this mapping on both user creation and update actions</span></span>
    - <span data-ttu-id="7ba1e-144">**Pouze během vytváření** -použít toto mapování pouze na akcí vytvoření uživatele</span><span class="sxs-lookup"><span data-stu-id="7ba1e-144">**Only during creation** - Apply this mapping only on user creation actions</span></span>


## <a name="what-you-should-know"></a><span data-ttu-id="7ba1e-145">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="7ba1e-145">What you should know</span></span>

<span data-ttu-id="7ba1e-146">Microsoft Azure AD poskytuje efektivní implementaci procesu synchronizace.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-146">Microsoft Azure AD provides an efficient implementation of a synchronization process.</span></span> <span data-ttu-id="7ba1e-147">V prostředí inicializovaného jsou zpracovány pouze objekty, které vyžadují aktualizace při synchronizační cyklus.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-147">In an initialized environment, only objects requiring updates are processed during a synchronization cycle.</span></span> <span data-ttu-id="7ba1e-148">Aktualizace mapování atributů má vliv na výkon synchronizační cyklus.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-148">Updating attribute mappings has an impact on the performance of a synchronization cycle.</span></span> <span data-ttu-id="7ba1e-149">Aktualizace mapování atributů vyžaduje všechny spravované objekty do znovu vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-149">An update to the attribute mapping configuration requires all managed objects to be reevaluated.</span></span> <span data-ttu-id="7ba1e-150">Je součástí osvědčeného postupu udrželi počet po sobě jdoucích změn na vaše mapování atributů minimálně.</span><span class="sxs-lookup"><span data-stu-id="7ba1e-150">It is a recommended best practice to keep the number of consecutive changes to your attribute mappings at a minimum.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ba1e-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ba1e-151">Next steps</span></span>

* [<span data-ttu-id="7ba1e-152">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ba1e-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="7ba1e-153">Automatizovat uživatele zřízení nebo zrušení zřízení k aplikacím SaaS</span><span class="sxs-lookup"><span data-stu-id="7ba1e-153">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="7ba1e-154">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="7ba1e-154">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="7ba1e-155">Filtry pro zřizování uživatelů oborů</span><span class="sxs-lookup"><span data-stu-id="7ba1e-155">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="7ba1e-156">Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM</span><span class="sxs-lookup"><span data-stu-id="7ba1e-156">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="7ba1e-157">Účet zřizování oznámení</span><span class="sxs-lookup"><span data-stu-id="7ba1e-157">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="7ba1e-158">Seznam kurzů k integraci aplikací SaaS</span><span class="sxs-lookup"><span data-stu-id="7ba1e-158">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

