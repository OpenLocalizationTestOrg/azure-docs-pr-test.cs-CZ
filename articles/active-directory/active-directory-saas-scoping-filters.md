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
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Zřizování aplikace na základě atributů s filtry oborů
Hello cílem této části je, že tooexplain jak toouse rozsahu filtry na základě atributů pravidla toodefine, které určují, kteří uživatelé mají zřízený toohello aplikace.

## <a name="clauses-and-scope-groups"></a>Klauzule a obor skupiny
![Filtr vytváření oboru][1] 

Oboru filtry jsou definované za jeden nebo více **obor skupiny**, každý z které obsahovat jednu nebo více **klauzule**. klauzule hello toosee pro konkrétní obor skupiny, rozbalte ho kliknutím hello šipku toohello nalevo od názvu skupiny hello.

A **klauzule** Určuje, kteří uživatelé mohou toopass prostřednictvím hello filtr vytváření oboru vyhodnocením atributy každého uživatele. Například můžete mít jednu klauzuli, která vyžaduje, aby uživatele 'stav' atribut rovna New Yorku, takže jsou pouze uživatelé v New Yorku zřízené do aplikace hello.

![Název oboru skupiny][2] 

Každý **oboru skupiny** začíná jeden povinné **klauzule**, jak je znázorněno v výše uvedený snímek obrazovky hello. Tuto klauzuli jednoduše stavy, že tento uživatel hello musí být přiřazena toohello aplikace předtím, než je vyhodnocena podle oboru filtry. Tuto klauzuli nemůže být odstraněna nebo upravena.

Stisknutím tlačítka odpovídající hello můžete přidat nové klauzule nebo nové oboru skupiny. Můžete udělit každou skupinu oboru název tak, že upravíte jeho **název oboru skupiny** vlastnost.

## <a name="how-scoping-filters-are-evaluated"></a>Jak se vyhodnocují filtry oborů
Při zřizování, jsme vyzkoušejte každých přiřazený uživatel proti vaší oboru toodetermine filtry, pokud je tento uživatel si zaslouží přístup toohello aplikace. Si můžete představit jako test, který musí být předán tooavoid uživatele hello získávání odfiltrovat, aby každý klauzule. 

Pokud máte více skupin oboru definován, každý uživatel musí projít alespoň jeden z nich tooaccess aplikace hello. V rámci jednotlivých skupin oboru však hello uživatel musí projít každých toopass klauzule této konkrétní obor skupiny. 

Jinými slovy, si můžete představit oboru skupiny, že by společně nebo si můžete představit hello klauzule v nich jako a společně by. Představte si třeba hello obor filtru níže:

![Název oboru skupiny][3]  

Podle toothis filtr vytváření oboru, uživatelé musí splňovat následující hello kritérií, toobe zřízené:

1. Musí být přiřazena toohello aplikace.
2. Musí pracují v oddělení inženýrství hello
3. Musí být práce v síti San Franciscu nebo Kanadě.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikace](active-directory-saas-app-provisioning.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Seznam kurzů tooIntegrate aplikace SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
