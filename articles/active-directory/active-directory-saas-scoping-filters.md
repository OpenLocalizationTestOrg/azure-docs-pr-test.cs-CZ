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
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Zřizování aplikace na základě atributů s filtry oborů
Cílem této části se popisují, jak se oboru filtrů umožňují definovat pravidla na základě atributů, které určují, kteří uživatelé jsou zřízené do aplikace.

## <a name="clauses-and-scope-groups"></a>Klauzule a obor skupiny
![Filtr vytváření oboru][1] 

Oboru filtry jsou definované za jeden nebo více **obor skupiny**, každý z které obsahovat jednu nebo více **klauzule**. Zobrazíte klauzule pro konkrétní obor skupiny, rozbalte ho kliknutím na šipku nalevo od názvu skupiny.

A **klauzule** Určuje, kteří uživatelé mohou předávat oboru filtru vyhodnocením atributy každého uživatele. Například můžete mít jednu klauzuli, která vyžaduje, aby atribut 'stav' uživatele rovnat New Yorku, tak jenom uživatelé v New Yorku jsou zřízené do aplikace.

![Název oboru skupiny][2] 

Každý **oboru skupiny** začíná jeden povinné **klauzule**, jak je znázorněno v výše uvedený snímek obrazovky. Tuto klauzuli jednoduše stavy, že uživatel musí nejprve přiřadit k aplikaci předtím, než je vyhodnocena podle oboru filtry. Tuto klauzuli nemůže být odstraněna nebo upravena.

Stisknutím kombinace kláves na příslušné tlačítko můžete přidat nové klauzule nebo nové oboru skupiny. Můžete udělit každou skupinu oboru název tak, že upravíte jeho **název oboru skupiny** vlastnost.

## <a name="how-scoping-filters-are-evaluated"></a>Jak se vyhodnocují filtry oborů
Při zřizování, abychom otestovat všechny přiřazené uživatele vůči oboru filtry k určení, pokud se tento uživatel si zaslouží přístup k aplikaci. Si můžete představit jako test, který musí být předán v pořadí pro uživatele, aby se zabránilo získávání odfiltrovat každý klauzule. 

Pokud máte více skupin oboru definován, každý uživatel musí projít alespoň jeden z nich pro přístup k aplikaci. V rámci jednotlivých skupin oboru ale uživatel musí projít každých klauzule předat této konkrétní obor skupiny. 

Jinými slovy, si můžete představit oboru skupiny, že by společně nebo si můžete představit jako klauzulích v nich a společně by. Představte si třeba oboru filtru níže:

![Název oboru skupiny][3]  

Podle tohoto oboru filtru uživatelé musí splňovat následující kritéria, které se má zřídit:

1. Je třeba je přiřadit k aplikaci.
2. Musí pracují v oddělení inženýrství
3. Musí být práce v síti San Franciscu nebo Kanadě.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS](active-directory-saas-app-provisioning.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](active-directory-scim-provisioning.md)
* [Seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
