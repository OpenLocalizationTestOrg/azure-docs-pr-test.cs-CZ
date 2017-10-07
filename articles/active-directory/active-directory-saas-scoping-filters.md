---
title: "aaaProvisioning aplikace s filtry oborů | Microsoft Docs"
description: "Zjistěte, jak toouse rozsahu filtry tooprevent objektů v aplikace, které podporují zřizování automatizované uživatelů z ve skutečnosti se zřídí Pokud objekt nemá splňují vaše podnikové požadavky."
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
ms.openlocfilehash: f0299390dc3fdb70aa9d271e835069a08827d635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a><span data-ttu-id="dc7f1-103">Zřizování aplikace na základě atributů s filtry oborů</span><span class="sxs-lookup"><span data-stu-id="dc7f1-103">Attribute-based application provisioning with scoping filters</span></span>
<span data-ttu-id="dc7f1-104">Hello cílem této části je, že tooexplain jak toouse rozsahu filtry na základě atributů pravidla toodefine, které určují, kteří uživatelé mají zřízený toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-104">hello objective of this section is tooexplain how toouse scoping filters toodefine attribute-based rules that determine which users are provisioned toohello application.</span></span>

## <a name="clauses-and-scope-groups"></a><span data-ttu-id="dc7f1-105">Klauzule a obor skupiny</span><span class="sxs-lookup"><span data-stu-id="dc7f1-105">Clauses and Scope Groups</span></span>
![Filtr vytváření oboru][1] 

<span data-ttu-id="dc7f1-107">Oboru filtry jsou definované za jeden nebo více **obor skupiny**, každý z které obsahovat jednu nebo více **klauzule**.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-107">Scoping filters are defined by one or more **scope groups**, each of which hold one or more **clauses**.</span></span> <span data-ttu-id="dc7f1-108">klauzule hello toosee pro konkrétní obor skupiny, rozbalte ho kliknutím hello šipku toohello nalevo od názvu skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-108">toosee hello clauses for a particular scope group, expand it by clicking hello arrow toohello left of hello group name.</span></span>

<span data-ttu-id="dc7f1-109">A **klauzule** Určuje, kteří uživatelé mohou toopass prostřednictvím hello filtr vytváření oboru vyhodnocením atributy každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-109">A **clause** determines which users are allowed toopass through hello scoping filter by evaluating each user’s attributes.</span></span> <span data-ttu-id="dc7f1-110">Například můžete mít jednu klauzuli, která vyžaduje, aby uživatele 'stav' atribut rovna New Yorku, takže jsou pouze uživatelé v New Yorku zřízené do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-110">For example, you might have one clause that requires that a user’s ‘state’ attribute equal New York, so only New York users are provisioned into hello application.</span></span>

![Název oboru skupiny][2] 

<span data-ttu-id="dc7f1-112">Každý **oboru skupiny** začíná jeden povinné **klauzule**, jak je znázorněno v výše uvedený snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-112">Each **scope group** starts with one mandatory **clause**, as shown in hello screenshot above.</span></span> <span data-ttu-id="dc7f1-113">Tuto klauzuli jednoduše stavy, že tento uživatel hello musí být přiřazena toohello aplikace předtím, než je vyhodnocena podle oboru filtry.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-113">This clause simply states that hello user must first be assigned toohello application before it’s evaluated by your scoping filters.</span></span> <span data-ttu-id="dc7f1-114">Tuto klauzuli nemůže být odstraněna nebo upravena.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-114">This clause cannot be deleted or modified.</span></span>

<span data-ttu-id="dc7f1-115">Stisknutím tlačítka odpovídající hello můžete přidat nové klauzule nebo nové oboru skupiny.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-115">You can add new clauses or new scope groups by pressing hello appropriate button.</span></span> <span data-ttu-id="dc7f1-116">Můžete udělit každou skupinu oboru název tak, že upravíte jeho **název oboru skupiny** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-116">You can give each scope group a name by editing its **Scope Group Name** property.</span></span>

## <a name="how-scoping-filters-are-evaluated"></a><span data-ttu-id="dc7f1-117">Jak se vyhodnocují filtry oborů</span><span class="sxs-lookup"><span data-stu-id="dc7f1-117">How Scoping Filters are Evaluated</span></span>
<span data-ttu-id="dc7f1-118">Při zřizování, jsme vyzkoušejte každých přiřazený uživatel proti vaší oboru toodetermine filtry, pokud je tento uživatel si zaslouží přístup toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-118">During provisioning, we test every assigned user against your scoping filters toodetermine if that user deserves access toohello application.</span></span> <span data-ttu-id="dc7f1-119">Si můžete představit jako test, který musí být předán tooavoid uživatele hello získávání odfiltrovat, aby každý klauzule.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-119">You can think of each clause as being a test that must be passed in order for hello user tooavoid getting filtered out.</span></span> 

<span data-ttu-id="dc7f1-120">Pokud máte více skupin oboru definován, každý uživatel musí projít alespoň jeden z nich tooaccess aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-120">If you have multiple scope groups defined, each user must pass at least one of them tooaccess hello application.</span></span> <span data-ttu-id="dc7f1-121">V rámci jednotlivých skupin oboru však hello uživatel musí projít každých toopass klauzule této konkrétní obor skupiny.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-121">Within each scope group, however, hello user must pass every clause toopass that specific scope group.</span></span> 

<span data-ttu-id="dc7f1-122">Jinými slovy, si můžete představit oboru skupiny, že by společně nebo si můžete představit hello klauzule v nich jako a společně by.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-122">In other words, you can think of scope groups as being OR’d together, and you can think of hello clauses within them as being AND’d together.</span></span> <span data-ttu-id="dc7f1-123">Představte si třeba hello obor filtru níže:</span><span class="sxs-lookup"><span data-stu-id="dc7f1-123">For example, consider hello scoping filter below:</span></span>

![Název oboru skupiny][3]  

<span data-ttu-id="dc7f1-125">Podle toothis filtr vytváření oboru, uživatelé musí splňovat následující hello kritérií, toobe zřízené:</span><span class="sxs-lookup"><span data-stu-id="dc7f1-125">According toothis scoping filter, users must satisfy hello following criteria, toobe provisioned:</span></span>

1. <span data-ttu-id="dc7f1-126">Musí být přiřazena toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-126">They must be assigned toohello application.</span></span>
2. <span data-ttu-id="dc7f1-127">Musí pracují v oddělení inženýrství hello</span><span class="sxs-lookup"><span data-stu-id="dc7f1-127">They must work in hello Engineering department</span></span>
3. <span data-ttu-id="dc7f1-128">Musí být práce v síti San Franciscu nebo Kanadě.</span><span class="sxs-lookup"><span data-stu-id="dc7f1-128">They must be work in either San Francisco or Canada.</span></span>

## <a name="related-articles"></a><span data-ttu-id="dc7f1-129">Související články</span><span class="sxs-lookup"><span data-stu-id="dc7f1-129">Related Articles</span></span>
* [<span data-ttu-id="dc7f1-130">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc7f1-130">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="dc7f1-131">Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="dc7f1-131">Automate User Provisioning and Deprovisioning tooSaaS Applications</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="dc7f1-132">Přizpůsobení mapování atributů pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="dc7f1-132">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="dc7f1-133">Zapisují se výrazy pro mapování atributů</span><span class="sxs-lookup"><span data-stu-id="dc7f1-133">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="dc7f1-134">Účet zřizování oznámení</span><span class="sxs-lookup"><span data-stu-id="dc7f1-134">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="dc7f1-135">Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="dc7f1-135">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="dc7f1-136">Seznam kurzů tooIntegrate aplikace SaaS</span><span class="sxs-lookup"><span data-stu-id="dc7f1-136">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
