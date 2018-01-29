---
title: "Zřizování správy firemních aplikací ve službě Azure Active Directory uživatelů | Microsoft Docs"
description: "Naučte se spravovat uživatele zřizování účtu pro podnikové aplikace pomocí služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser
manager: mtillman
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 122dbdb838377a36020f9ec692b38544004e676c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Správa uživatelský účet zřizování pro podnikové aplikace na portálu Azure
Tento článek popisuje postup použití [portál Azure](https://portal.azure.com) ke správě automatické uživatel účet zřizování a jeho rušení pro aplikace, které to podporují, zejména ta, která byla přidána z "doporučenou" kategorii [ Galerii aplikací Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Další informace o zřizování účtu automatické uživatele a jak to funguje, najdete v části [automatizace zřizování uživatelů a jeho rušení pro aplikace SaaS ve službě Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Vyhledání aplikace v portálu
Všechny aplikace, které jsou konfigurovány pro jednotné přihlašování v adresáři, pomocí Správce adresáře pomocí [galerii aplikací Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), můžete zobrazit a spravovat [portál Azure](https://portal.azure.com). Aplikace lze nalézt v **více služeb** &gt; **podnikové aplikace, které** části portálu. Podnikové aplikace jsou aplikace, které jsou nasazené a použít v rámci vaší organizace.

![Okno podnikové aplikace][0]

Výběr **všechny aplikace** odkaz na levé straně zobrazuje seznam všech aplikací, které byly nakonfigurovány, včetně aplikací, přidané z galerie. Výběr aplikace načte okna prostředků pro tuto aplikaci, kde je možné zobrazit sestavy pro tuto aplikaci a lze je spravovat celou řadu nastavení.

Uživatelský účet zřizování nastavení lze spravovat výběrem **zřizování** na levé straně.

![Okna prostředků aplikace][1]

## <a name="provisioning-modes"></a>Zřizování režimy
**Zřizování** okno začíná **režimu** nabídce, která ukazuje, jaké zřizování režimy jsou podporovány pro podniková aplikace a umožňuje její konfiguraci. Mezi dostupné možnosti patří:

* **Automatické** – tato možnost se zobrazí, pokud Azure AD podporuje automatické zřizování na základě rozhraní API a jeho rušení uživatelských účtů do této aplikace. Výběr tento režim zobrazí rozhraní, které vede správce konfigurace Azure AD pro připojení k rozhraní API pro správu aplikace uživatelů, vytvoření účtu mapování a pracovní postupy, které definují, jak by měla toku dat účet uživatele mezi službou Azure AD a aplikace a správa zřizování služby Azure AD.
* **Ruční** – tato možnost se zobrazí, pokud Azure AD nepodporuje automatické vytváření uživatelských účtů do této aplikace. Tato možnost znamená, že účet záznamů uživatele uložené v aplikaci se musí spravovat pomocí externího procesu, podle uživatele správy a zřizování funkce poskytované verzí této aplikace (včetně zřizování JIT SAML).

## <a name="configuring-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu
Výběr **automatické** možnost se zobrazí obrazovka, která je rozdělena do čtyř částí:

### <a name="admin-credentials"></a>Přihlašovací údaje správce
Toto je, kde se přihlašovací údaje potřebné pro Azure AD pro připojení k rozhraní API se zadávají Správa uživatelů aplikace. Vstup, vyžaduje se liší v závislosti na aplikaci. Další informace o požadavky pro konkrétní aplikace a typy přihlašovacích údajů, najdete v článku [kurz konfigurace pro konkrétní aplikace](active-directory-saas-app-provisioning.md).

Výběr **otestovat připojení** tlačítko umožňuje otestovat přihlašovací údaje tak, že Azure AD pokus o připojení k aplikaci je zřizování aplikace pomocí zadané přihlašovací údaje.

### <a name="mappings"></a>Mapování
Toto je, kde můžou správci zobrazit a upravit jaké toku atributů uživatele mezi službou Azure AD a cílová aplikace, když jsou uživatelské účty zřízen nebo aktualizován.

Není předkonfigurované sada mapování mezi objekty uživatele Azure AD a každá aplikace SaaS uživatelské objekty. Některé aplikace spravovat jiné typy objektů, jako jsou skupiny nebo kontakty. Výběrem jedné z těchto mapování v v tabulce jsou uvedeny editor mapování vpravo, kde mohou být zobrazit a upravit.

![Okna prostředků aplikace][2]

Podporované vlastnímu nastavení patří:

* Povolení a zákaz mapování pro konkrétní objekty, například objekt uživatele Azure AD k objektu uživatele aplikace SaaS.
* Úpravy, které atributy toku z objektu uživatele Azure AD k objektu uživatele aplikace. Další informace o mapování atributů najdete v tématu [seznámení s typy mapování atributů](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Filtrujte zřizování akce, které Azure AD provádí cílové aplikace. Místo nutnosti plně synchronizovat objekty služby Azure AD, můžete omezit akce provést. Například výběrem pouze **aktualizace**, Azure AD pouze aktualizace stávajících uživatelských účtů v aplikaci a nevytvoří nové. Pouze výběrem **vytvořit**, Azure pouze vytvoří nové uživatelské účty, ale neaktualizuje existující. Tato funkce umožňuje správcům vytvořit různé mapování pro vytváření účtů a aktualizovat pracovní postupy.

### <a name="settings"></a>Nastavení
Tato část umožňuje správcům spuštění a zastavení služby Azure AD zřizování služby pro vybranou aplikaci, a také volitelně vymazat mezipaměť zřizování a restartujte službu.

Pokud zřizování je povolený pro aplikaci poprvé, zapnout službu změnou **Stav zřizování** k **na**. To způsobí, že Azure AD zřizování služby k provedení počáteční synchronizaci, kde přečte v přiřazených uživateli **uživatelů a skupin** části dotazuje cílová aplikace pro ně a potom provede zřizování akce definované ve službě Azure AD **mapování** části. Během tohoto procesu zřizování služby uloží data uložená v mezipaměti, o jaké uživatelské účty, které spravuje, aby jiné spravované účty v cílové aplikace, které nebyly nikdy v oboru pro přiřazení nemá vliv jeho rušení operace. Po počáteční synchronizaci zřizování služby automaticky synchronizuje uživatele a skupiny objektů v intervalu deset minut.

Změna **Stav zřizování** k **vypnout** jednoduše pozastaví zřizování služby. V tomto stavu Azure není vytvářet, aktualizovat nebo odebrat všechny uživatele nebo skupiny objektů v aplikaci. Změna stavu zpět do na způsobí, že službu a pokračovat tam, kde bylo přerušeno.

Výběr **vymaže aktuální stav a restartovat synchronizaci** zaškrtávací políčko a ukládání zastaví službu zřizování výpisů paměti je pod správou, restartuje službu a provede počáteční data uložená v mezipaměti, o jaké účty Azure AD synchronizaci znovu. Tato možnost umožňuje správcům spustit proces zřizování nasazení přes znovu.

### <a name="synchronization-details"></a>Podrobnosti o synchronizaci
Tato část obsahuje další podrobnosti o operaci zřizování služby, včetně první a poslední doby, ke které došlo zřizování služby pro aplikace a kolik uživatelů a objektů skupin, je spravován.

Jsou uvedeny odkazy na **zřizování sestava aktivit**, který poskytuje protokolu všichni uživatelé a skupiny vytvořený, aktualizovat a odebrané mezi Azure AD a cílová aplikace a **zřizování chybách** který nabízí podrobnější chybové zprávy pro uživatele a objektů skupin, které se nepodařilo přečíst, vytvořit, aktualizace nebo odstranění. 

##<a name="feedback"></a>Váš názor

Věříme, že je jako prostředí Azure AD. Prosím udržovat zpětnou vazbu, než dorazí! POST vaše názory a návrhy pro zlepšení **portál pro správu** části našich [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Jsme se vzrušení o vytváření nástrojů nové vlastní položky každý den a použijte vaše pokyny na obrazec a definovat, co se máme zaměřit příště.


[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
