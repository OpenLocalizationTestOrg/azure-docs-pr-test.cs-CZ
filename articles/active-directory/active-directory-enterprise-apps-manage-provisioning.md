---
title: "aaaUser zřizování správy pro podnikové aplikace v Azure Active Directory hello | Microsoft Docs"
description: "Zjistěte, jak toomanage uživatelský účet zřizování pro podnikové aplikace pomocí hello Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
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
ms.openlocfilehash: a613f844c8f51e04b92e62b488313a78ab85f7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-hello-azure-portal"></a>Správa uživatelský účet zřizování pro podnikové aplikace v hello portálu Azure
Tento článek popisuje, jak toouse hello [portál Azure](https://portal.azure.com) toomanage automatické uživatelský účet zřizování a jeho rušení pro aplikace, které to podporují, zejména ta, která byla přidána z hello "vybrané" kategorii Hello [galerii aplikací Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). toolearn informace o zřizování účtu automatické uživatele a jak to funguje, najdete v části [automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-hello-portal"></a>Vyhledání aplikace hello portálu
Všechny aplikace, které jsou konfigurovány pro jednotné přihlašování v adresáři, pomocí Správce adresáře pomocí hello [galerii aplikací Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), můžete zobrazit a spravovat hello [portál Azure](https://portal.azure.com). aplikace Hello lze nalézt v hello **více služeb** &gt; **podnikové aplikace, které** části portálu hello. Podnikové aplikace jsou aplikace, které jsou nasazené a použít v rámci vaší organizace.

![Okno podnikové aplikace][0]

Výběr hello **všechny aplikace** odkaz na levé straně hello zobrazuje seznam všech aplikací, které byly nakonfigurovány, včetně aplikací, přidané z Galerie hello. Výběr aplikace načte hello okna prostředků pro tuto aplikaci, kde je možné zobrazit sestavy pro tuto aplikaci a lze je spravovat celou řadu nastavení.

Uživatelský účet zřizování nastavení lze spravovat výběrem **zřizování** na levé straně hello.

![Okna prostředků aplikace][1]

## <a name="provisioning-modes"></a>Zřizování režimy
Hello **zřizování** okno začíná **režimu** nabídky, které se zobrazuje, jaké zřizování režimy jsou podporovány pro podniková aplikace a umožňuje jim toobe nakonfigurované. Hello dostupné možnosti patří:

* **Automatické** – tato možnost se zobrazí, pokud Azure AD podporuje automatické zřizování na základě rozhraní API a jeho rušení uživatelské účty toothis aplikace. Výběr tento režim zobrazí rozhraní, které vede správce konfigurace uživatele Azure AD tooconnect toohello aplikace rozhraní API pro správu, vytvoření účtu mapování a pracovní postupy, které definují, jak by měla toku dat účet uživatele mezi službou Azure AD a Hello aplikace a správu hello zřizování služby Azure AD.
* **Ruční** – tato možnost se zobrazí, pokud Azure AD nepodporuje automatické zřizování aplikace toothis uživatelské účty. Tato možnost znamená, že záznamy účet uživatele uložené v aplikaci hello se musí spravovat pomocí externího procesu, podle hello uživatele správy a zřizování dostupné možnosti danou aplikací (které mohou obsahovat zřizování JIT SAML).

## <a name="configuring-automatic-user-account-provisioning"></a>Konfigurace automatického uživatele zřizování účtu
Výběr hello **automatické** možnost se zobrazí obrazovka, která je rozdělena do čtyř částí:

### <a name="admin-credentials"></a>Přihlašovací údaje správce
Toto je, kde jsou zadány hello pověření potřebná pro uživatele Azure AD tooconnect toohello aplikace rozhraní API pro správu. Hello vstup požadované se liší v závislosti na aplikaci hello. toolearn o typy hello přihlašovacích údajů a požadavcích pro konkrétní aplikace, najdete v části hello [kurz konfigurace pro konkrétní aplikace](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Výběr hello **Test připojení** tlačítko umožňuje vám tootest hello pověření tak, že pokus tooconnect toohello aplikace Azure AD je zřizování aplikace pomocí přihlašovacích údajů hello zadaný.

### <a name="mappings"></a>Mapování
Toto je, kde můžou správci zobrazit a upravit jaké toku atributů uživatele mezi službou Azure AD a hello cílová aplikace, když jsou uživatelské účty zřízen nebo aktualizován.

Není předkonfigurované sada mapování mezi objekty uživatele Azure AD a každá aplikace SaaS uživatelské objekty. Některé aplikace spravovat jiné typy objektů, jako jsou skupiny nebo kontakty. Výběrem jedné z těchto mapování v tabulce hello zobrazuje editor mapování hello toohello vpravo, kde mohou být zobrazit a upravit.

![Okna prostředků aplikace][2]

Podporované vlastnímu nastavení patří:

* Povolení a zákaz mapování pro konkrétní objekty, například objekt uživatele hello Azure AD uživatele objektu toohello SaaS aplikace.
* Atributy, které toku z objektu uživatele hello Azure AD uživatele objektu toohello aplikace při provádění úprav. Další informace o mapování atributů najdete v tématu [seznámení s typy mapování atributů](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Filtr hello zřizování akce, které Azure AD provádí hello cílové aplikace. Místo nutnosti plně synchronizovat objekty služby Azure AD, můžete omezit hello akce provést. Například výběrem pouze **aktualizace**, Azure AD pouze aktualizace stávajících uživatelských účtů v aplikaci a nevytvoří nové. Pouze výběrem **vytvořit**, Azure pouze vytvoří nové uživatelské účty, ale neaktualizuje existující. Tato funkce umožňuje správci toocreate jiné mapování pro účet, vytvoření a aktualizace pracovních postupů.

### <a name="settings"></a>Nastavení
Tato část umožňuje správci toostart a zastavení hello zřizování služby Azure AD pro hello vybrané aplikace, jakož i volitelně zrušte hello zřizování mezipaměti a restartujte službu hello.

Pokud zřizování je povolený pro hello poprvé pro aplikaci, zapnout službu hello změnou hello **Stav zřizování** příliš**na**. To způsobí, že hello Azure AD zřizování služby tooperform počáteční synchronizaci, kde ho načte hello uživatelé přiřazení v hello **uživatelů a skupin** části dotazuje hello cílová aplikace pro ně a potom provede hello zřizování akce definované v hello Azure AD **mapování** části. Během tohoto procesu hello zřizování služby uloží data uložená v mezipaměti, o jaké uživatelské účty, které spravuje, aby jiné spravované účty v hello cílové aplikace, které nebyly nikdy v oboru pro přiřazení nemá vliv jeho rušení operace. Po hello počáteční synchronizaci, hello zřizování služby automaticky synchronizuje objekty uživatelů a skupin v intervalu deset minut.

Změna hello **Stav zřizování** příliš**vypnout** jednoduše pozastaví hello zřizování služby. V tomto stavu Azure není vytvářet, aktualizovat nebo odebrat všechny uživatele nebo skupiny objektů v aplikaci hello. Změna hello stavu zpět tooon způsobí, že hello služby toopick tam, kde ji skončil.

Výběr hello **vymaže aktuální stav a restartovat synchronizaci** zaškrtávací políčko a ukládání zastaví hello zřizování služby, výpisy hello mezipaměti data o jaké účty Azure AD je správa, restartuje hello služby a provede hello počáteční synchronizaci znovu. Tato možnost umožňuje správci toostart hello zřizování procesu nasazení prostřednictvím znovu.

### <a name="synchronization-details"></a>Podrobnosti o synchronizaci
Tato část poskytuje další podrobnosti o hello operace hello zřizování služby, včetně hello první a poslední časy hello zřizování služby spustili před hello aplikace a kolik uživatelů a skupin objektů jsou spravován.

Odkazy jsou k dispozici toohello **zřizování sestava aktivit**, který poskytuje protokolu všichni uživatelé a skupiny vytvořený, aktualizovat a odebrané mezi Azure AD a hello cílová aplikace a toohello **zřizování chyby Sestava** který nabízí podrobnější chybové zprávy pro uživatele a skupiny objekty tohoto selhání toobe přečíst, vytvořit, aktualizovat nebo odebrat. 

##<a name="feedback"></a>Váš názor

Věříme, že je jako prostředí Azure AD. Uchovávejte hello zpětnou vazbu, než dorazí! Zveřejněte vaše názory a návrhy pro zlepšení ve hello **portál pro správu** části našich [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Jsme se vzrušení o vytváření nástrojů nové vlastní položky každý den a používat vaše tooshape pokyny a definovat, co se máme zaměřit příště.


[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
