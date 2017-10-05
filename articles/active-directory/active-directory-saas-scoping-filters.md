---
title: "Zřizování aplikace s filtry oborů | Microsoft Docs"
description: "Další informace o použití oboru filtrů objekty v aplikacích, které podporují zřizování automatizované uživatelů z ve skutečnosti se zřídí Pokud objekt nemá splňují vaše podnikové požadavky."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 109635052e2ded33831b050eb12d50745944091b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a><span data-ttu-id="238bb-103">Zřizování aplikace na základě atributů s filtry oborů</span><span class="sxs-lookup"><span data-stu-id="238bb-103">Attribute-based application provisioning with scoping filters</span></span>
<span data-ttu-id="238bb-104">Cílem této části se popisují, jak se oboru filtrů umožňují definovat pravidla na základě atributů, které určují, kteří uživatelé jsou zřízené do aplikace.</span><span class="sxs-lookup"><span data-stu-id="238bb-104">The objective of this section is to explain how to use scoping filters to define attribute-based rules that determine which users are provisioned to the application.</span></span>

## <a name="clauses-and-scope-groups"></a><span data-ttu-id="238bb-105">Klauzule a obor skupiny</span><span class="sxs-lookup"><span data-stu-id="238bb-105">Clauses and Scope Groups</span></span>
![Filtr vytváření oboru][1] 

<span data-ttu-id="238bb-107">Oboru filtry jsou definované za jeden nebo více **obor skupiny**, každý z které obsahovat jednu nebo více **klauzule**.</span><span class="sxs-lookup"><span data-stu-id="238bb-107">Scoping filters are defined by one or more **scope groups**, each of which hold one or more **clauses**.</span></span> <span data-ttu-id="238bb-108">Zobrazíte klauzule pro konkrétní obor skupiny, rozbalte ho kliknutím na šipku nalevo od názvu skupiny.</span><span class="sxs-lookup"><span data-stu-id="238bb-108">To see the clauses for a particular scope group, expand it by clicking the arrow to the left of the group name.</span></span>

<span data-ttu-id="238bb-109">A **klauzule** Určuje, kteří uživatelé mohou předávat oboru filtru vyhodnocením atributy každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="238bb-109">A **clause** determines which users are allowed to pass through the scoping filter by evaluating each user’s attributes.</span></span> <span data-ttu-id="238bb-110">Například můžete mít jednu klauzuli, která vyžaduje, aby atribut 'stav' uživatele rovnat New Yorku, tak jenom uživatelé v New Yorku jsou zřízené do aplikace.</span><span class="sxs-lookup"><span data-stu-id="238bb-110">For example, you might have one clause that requires that a user’s ‘state’ attribute equal New York, so only New York users are provisioned into the application.</span></span>

![Název oboru skupiny][2] 

<span data-ttu-id="238bb-112">Každý **oboru skupiny** začíná jeden povinné **klauzule**, jak je znázorněno v výše uvedený snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="238bb-112">Each **scope group** starts with one mandatory **clause**, as shown in the screenshot above.</span></span> <span data-ttu-id="238bb-113">Tuto klauzuli jednoduše stavy, že uživatel musí nejprve přiřadit k aplikaci předtím, než je vyhodnocena podle oboru filtry.</span><span class="sxs-lookup"><span data-stu-id="238bb-113">This clause simply states that the user must first be assigned to the application before it’s evaluated by your scoping filters.</span></span> <span data-ttu-id="238bb-114">Tuto klauzuli nemůže být odstraněna nebo upravena.</span><span class="sxs-lookup"><span data-stu-id="238bb-114">This clause cannot be deleted or modified.</span></span>

<span data-ttu-id="238bb-115">Stisknutím kombinace kláves na příslušné tlačítko můžete přidat nové klauzule nebo nové oboru skupiny.</span><span class="sxs-lookup"><span data-stu-id="238bb-115">You can add new clauses or new scope groups by pressing the appropriate button.</span></span> <span data-ttu-id="238bb-116">Můžete udělit každou skupinu oboru název tak, že upravíte jeho **název oboru skupiny** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="238bb-116">You can give each scope group a name by editing its **Scope Group Name** property.</span></span>

## <a name="how-scoping-filters-are-evaluated"></a><span data-ttu-id="238bb-117">Jak se vyhodnocují filtry oborů</span><span class="sxs-lookup"><span data-stu-id="238bb-117">How Scoping Filters are Evaluated</span></span>
<span data-ttu-id="238bb-118">Při zřizování, abychom otestovat všechny přiřazené uživatele vůči oboru filtry k určení, pokud se tento uživatel si zaslouží přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="238bb-118">During provisioning, we test every assigned user against your scoping filters to determine if that user deserves access to the application.</span></span> <span data-ttu-id="238bb-119">Si můžete představit jako test, který musí být předán v pořadí pro uživatele, aby se zabránilo získávání odfiltrovat každý klauzule.</span><span class="sxs-lookup"><span data-stu-id="238bb-119">You can think of each clause as being a test that must be passed in order for the user to avoid getting filtered out.</span></span> 

<span data-ttu-id="238bb-120">Pokud máte více skupin oboru definován, každý uživatel musí projít alespoň jeden z nich pro přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="238bb-120">If you have multiple scope groups defined, each user must pass at least one of them to access the application.</span></span> <span data-ttu-id="238bb-121">V rámci jednotlivých skupin oboru ale uživatel musí projít každých klauzule předat této konkrétní obor skupiny.</span><span class="sxs-lookup"><span data-stu-id="238bb-121">Within each scope group, however, the user must pass every clause to pass that specific scope group.</span></span> 

<span data-ttu-id="238bb-122">Jinými slovy, si můžete představit oboru skupiny, že by společně nebo si můžete představit jako klauzulích v nich a společně by.</span><span class="sxs-lookup"><span data-stu-id="238bb-122">In other words, you can think of scope groups as being OR’d together, and you can think of the clauses within them as being AND’d together.</span></span> <span data-ttu-id="238bb-123">Představte si třeba oboru filtru níže:</span><span class="sxs-lookup"><span data-stu-id="238bb-123">For example, consider the scoping filter below:</span></span>

![Název oboru skupiny][3]  

<span data-ttu-id="238bb-125">Podle tohoto oboru filtru uživatelé musí splňovat následující kritéria, které se má zřídit:</span><span class="sxs-lookup"><span data-stu-id="238bb-125">According to this scoping filter, users must satisfy the following criteria, to be provisioned:</span></span>

1. <span data-ttu-id="238bb-126">Je třeba je přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="238bb-126">They must be assigned to the application.</span></span>
2. <span data-ttu-id="238bb-127">Musí pracují v oddělení inženýrství</span><span class="sxs-lookup"><span data-stu-id="238bb-127">They must work in the Engineering department</span></span>
3. <span data-ttu-id="238bb-128">Musí být práce v síti San Franciscu nebo Kanadě.</span><span class="sxs-lookup"><span data-stu-id="238bb-128">They must be work in either San Francisco or Canada.</span></span>

## <a name="related-articles"></a><span data-ttu-id="238bb-129">Související články</span><span class="sxs-lookup"><span data-stu-id="238bb-129">Related Articles</span></span>
* [<span data-ttu-id="238bb-130">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="238bb-130">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="238bb-131">Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS</span><span class="sxs-lookup"><span data-stu-id="238bb-131">Automate User Provisioning and Deprovisioning to SaaS Applications</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="238bb-132">Přizpůsobení mapování atributů pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="238bb-132">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="238bb-133">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="238bb-133">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="238bb-134">Účet zřizování oznámení</span><span class="sxs-lookup"><span data-stu-id="238bb-134">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="238bb-135">Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM</span><span class="sxs-lookup"><span data-stu-id="238bb-135">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="238bb-136">Seznam kurzů k integraci aplikací SaaS</span><span class="sxs-lookup"><span data-stu-id="238bb-136">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
