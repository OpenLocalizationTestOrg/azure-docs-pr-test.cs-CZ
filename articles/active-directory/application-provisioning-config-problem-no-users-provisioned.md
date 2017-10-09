---
title: "Uživatelé aaaNo probíhá aplikace Galerie zřízené tooan Azure AD | Microsoft Docs"
description: "Jak potýkají tootroubleshoot běžné problémy při nevidíte uživatelé zobrazovaných v na Azure AD Application Gallery jste nakonfigurovali pro zřizování uživatelů s Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: asteen
ms.openlocfilehash: 4d9693a202ed657e1de5571b50e5d499bef1bb3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="no-users-are-being-provisioned-tooan-azure-ad-gallery-application"></a>Žádní uživatelé probíhá aplikace Galerie zřízené tooan Azure AD

Jednou automatické zřizování byl nakonfigurován pro aplikace (včetně ověřování, zda text hello aplikace přihlašovací údaje tooAzure AD tooconnect toohello aplikace jsou platné). Potom uživatelů nebo skupin jsou zřízené toohello aplikace a je dáno hello následující věci:

-   Které uživatelé a skupiny byly **přiřazené** toohello aplikace. Další informace o přiřazení najdete v tématu [přiřadit aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Zda **mapování atributů** jsou povolené a nakonfigurované toosync platné atributy z toohello aplikace Azure AD. Další informace o mapování atributů najdete v tématu [přizpůsobení zřizování atribut mapování uživatelů pro aplikace SaaS ve službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Zda je **oboru filtru** přítomen, který je filtrování uživatelů na základě konkrétní atribut hodnot. Další informace o filtry oborů najdete v tématu [zřizování aplikace na základě atributů s filtry oborů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Při sledování, zda uživatelé nejsou se zřídí, projděte si protokoly auditu hello ve službě Azure AD a vyhledejte položky protokolu pro konkrétního uživatele.

Hello zřizování protokoly auditu je přístupná v hello portál Azure, v hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; protokolech auditování**kartě. Filtr hello přihlásí hello **zřizování účtu** kategorie tooonly najdete v části hello zřizování události pro tuto aplikaci. Můžete vyhledat uživatele podle hello "odpovídající ID" nakonfigurovaný pro ně v mapování atributů hello. Například, pokud jste nakonfigurovali hello "hlavní název uživatele" nebo "e-mailovou adresu" jako hello odpovídající atribut na straně hello Azure AD, a tím zřizování uživatelů hello má hodnotu "audrey@contoso.com". Pak vyhledejte protokoly auditu hello "audrey@contoso.com" a kontrola a vrácena žádná položka.

Hello zřizování auditu protokoly záznam všech hello operace provedené hello zřizování služby, včetně dotazování Azure AD pro přiřazené uživatele, které jsou v oboru pro zřizování, dotazování hello cílové aplikace hello existence tyto uživatele, porovnání hello uživatelské objekty mezi hello systému. Potom přidání, aktualizace nebo zakázat hello uživatelský účet v cílovém systému hello na základě porovnání hello.

## <a name="general-problem-areas-with-provisioning-tooconsider"></a>Obecné problémových oblastí se zřizováním tooconsider

Níže uvádíme seznam hello obecné problémových oblastí, které můžete rozbalit Pokud máte představu o kde toostart.

* [Zřizování služby nezobrazí toostart](#provisioning-service-does-not-appear-to-start)
* [Protokoly auditu, že uživatelé jsou přeskočeny a není zajišťováno, i když jsou přiřazeny](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-toostart"></a>Zřizování služby nezobrazí toostart

Pokud nastavíte hello **Stav zřizování** toobe **na** v hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt;Zřizování** části hello portálu Azure. Ale žádné další podrobnosti o stavu jsou zobrazena na této stránce po následné znovu načte, je pravděpodobné, že hello služba běží, ale nebyl dokončen ještě počáteční synchronizaci. Zkontrolujte hello **protokoly auditu** popsané výše toodetermine jaké operace hello služba pracuje, a pokud nejsou žádné chyby.

>[!NOTE]
>Počáteční synchronizace může trvat hodiny tooseveral 20 minut, v závislosti na velikosti hello hello adresář Azure AD a hello počet uživatelů v oboru pro zřizování. Následné synchronizace po počáteční synchronizaci hello je rychlejší, jako hello zřizování služby ukládá vodoznaky, které představují hello stav obou systémů po počáteční synchronizaci hello. To zvyšuje výkon následné synchronizace.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Protokoly auditu, že uživatelé jsou přeskočeny a není zajišťováno i když jsou přiřazeny

Když uživatel se zobrazí jako "přeskočen" v protokolech auditu hello, je velmi důležité tooread hello Rozšířené podrobnosti v hello protokolu zpráv toodetermine hello důvod. Níže jsou uvedeny běžné příčiny a řešení:

-   **Byl nakonfigurován oboru filtru** **, je filtrování hello uživatele podle hodnota atributu**. Další informace o filtry oborů najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **uživatel Hello nárok "není efektivně".** Pokud se zobrazí tato konkrétní chybová zpráva, je to, protože došlo k potížím s hello uživatele přiřazení záznam uložené ve službě Azure AD. toofix-li tento problém, zrušte přiřazení z aplikace hello hello uživatele (nebo skupiny) a znovu přiřadit. Další informace o přiřazení najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Požadovaný atribut nebyl nalezen nebo není vyplněná pro uživatele.** Tooconsider důležité při nastavování zřizování být tooreview a konfigurace mapování atributů hello a pracovní postupy, které definují vlastnosti toku z Azure AD toohello aplikace které uživatele (nebo skupiny). To zahrnuje nastavení hello "odpovídající vlastnost", který se použité toouniquely identifikovat a odpovídají uživatele nebo skupiny mezi hello dvěma systémy. Další informace o tomto procesu důležité najdete v tématu [přizpůsobení zřizování atribut mapování uživatelů pro aplikace SaaS ve službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Mapování pro skupiny atributů:** zřizování hello skupiny názvů a skupinových podrobnosti v přidání toohello členy, pokud pro některé aplikace podporován. Můžete povolit nebo zakázat tuto funkci povolením nebo zakázáním hello **mapování** pro objekty skupiny uvedené v hello **zřizování** kartě. Pokud zřizování skupiny je povolena, ujistěte se, že tooreview hello atribut mapování tooensure na odpovídající pole se používá pro hello "odpovídající ID". Může to být hello zobrazovaný název nebo e-mailu alias), jak hello skupiny a její členy nelze zřídit Pokud hello odpovídající vlastnost je prázdný nebo není vyplněná skupiny ve službě Azure AD.

## <a name="next-steps"></a>Další kroky
[Synchronizace Azure AD Connect: Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

