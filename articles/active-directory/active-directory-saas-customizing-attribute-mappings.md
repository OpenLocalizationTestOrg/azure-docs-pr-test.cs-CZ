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
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů
Microsoft Azure AD poskytuje podporu pro zřizování aplikace SaaS toothird výrobců jako Salesforce, Google Apps a dalších uživatelů. Pokud máte uživatele zřizování pro aplikace SaaS třetích stran povoleno, hello portálu pro správu Azure prvky jeho hodnot atributů v podobě konfigurace s názvem "mapování atributů."

Není předkonfigurované sadu atributů mapování mezi objekty uživatele Azure AD a každá aplikace SaaS uživatelské objekty. Některé aplikace spravovat jiné typy objektů, jako jsou skupiny nebo kontakty. <br> 
 Můžete upravit mapování atributů výchozí hello podle tooyour obchodním potřebám. To znamená, můžete změnit nebo odstranit existující mapování atributů nebo vytvořte nové mapování atributů.

Portálu hello Azure AD, dostanete tuto funkci kliknutím **mapování** konfigurace v **zřizování** v hello **spravovat** části  **Podniková aplikace**.


![Salesforce][5] 

Kliknutím **mapování** otevření hello související konfigurace, **mapování atributů** okno.  
Existují mapování atributů, které jsou vyžadované toofunction aplikace SaaS správně. Povinné atributy hello **odstranit** funkce není dostupná.


![Salesforce][6]  

V předchozím příkladu hello, uvidíte, že hello **uživatelské jméno** atribut spravovaného objektu v Salesforce se zobrazí v hello **userPrincipalName** hodnotu hello propojené objekt Azure Active Directory.

Můžete upravit existující **mapování atributů** kliknutím mapování. Tím se otevře hello **Upravit atribut** okno.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Principy typů mapování atributů
S mapování atributů řídit, jak jsou naplněny atributy v aplikaci SaaS jiných výrobců. Existují čtyři typy různých mapování podporované:

* **Přímé** – atribut target hello naplněný hello hodnotu atributu hello připojeného objektu ve službě Azure AD.
* **Konstantní** – atribut target hello se zobrazí v konkrétní řetězec, který jste zadali.
* **Výraz** -atribut target hello se naplní na základě výsledku hello skriptu jako výrazu. 
  Další informace najdete v tématu [zápis výrazy pro mapování atributů v Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Žádný** -atribut target hello je ponechán beze změny. Ale pokud atribut target hello je někdy prázdná, je naplněna s hello výchozí hodnotu, která zadáte.

Přidání toothese čtyři základní atribut mapování typů, mapování vlastních atributů podporují hello koncept volitelný **výchozí** přiřazení hodnoty. Přiřazení hodnoty výchozí Hello zajistí, že atribut target je vyplněný s hodnotou, když není ani jedna hodnota ve službě Azure AD ani na hello cílový objekt. Nejběžnější konfigurace Hello je tooleave tomto prázdné.


## <a name="understanding-attribute-mapping-properties"></a>Principy vlastnosti mapování atributů

V předchozí části hello již jste vlastnost Typ mapování přináší toohello atributu.
V přidání vlastnosti toothis mapování atributů také podporují hello následující atributy:

- **Zdrojový atribut** -hello atribut uživatele ze zdrojového systému hello (např: Azure Active Directory).
- **Atribut target** – atribut hello uživatele v cílovém systému hello (např: ServiceNow).
- **Objekty používající tento atribut odpovídat** – zda by měl použít toto mapování toouniquely identifikaci uživatelů mezi zdrojovými a cílovými systémy hello. To se obvykle nastavuje na hello userPrincipalName nebo atribut mail v Azure AD, což je většinou mapována tooa pole pro uživatelské jméno v cílové aplikace.
- **Odpovídající prioritu** – více odpovídající atributy lze nastavit. Pokud existuje více vyhodnocují se v pořadí hello definované v tomto poli. Jakmile je nalezena shoda, žádné další odpovídající atributy vyhodnocují.
- **Použít toto mapování**
    - **Vždy** – použít toto mapování na obou vytvoření uživatele a aktualizovat akce
    - **Pouze během vytváření** -použít toto mapování pouze na akcí vytvoření uživatele


## <a name="what-you-should-know"></a>Důležité informace

Microsoft Azure AD poskytuje efektivní implementaci procesu synchronizace. V prostředí inicializovaného jsou zpracovány pouze objekty, které vyžadují aktualizace při synchronizační cyklus. Aktualizace mapování atributů má vliv na výkon hello synchronizační cyklus. Aktualizaci toohello atributů mapování vyžaduje všechny spravované objekty toobe již znovu. Je doporučené nejlepší postup tookeep hello několika po sobě jdoucích změny mapování atributů tooyour minimálně.

## <a name="next-steps"></a>Další kroky

* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizace zřizování uživatelů nebo jeho rušení tooSaaS aplikace](active-directory-saas-app-provisioning.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů tooIntegrate aplikace SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

