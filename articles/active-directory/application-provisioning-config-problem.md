---
title: "aaaProblem konfigurace uživatele zřizování aplikace Galerie tooan Azure AD | Microsoft Docs"
description: "Jak potýkají tootroubleshoot běžné problémy při konfiguraci aplikace tooan již uveden v galerii aplikací Azure AD hello zřizování uživatelů"
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
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 19b12be019b05faecef74c6f92a9de03b52a5ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-user-provisioning-tooan-azure-ad-gallery-application"></a>Problém konfigurace tooan Azure AD application Gallery zřizování uživatelů

Konfigurace [zřizování automatické uživatelů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) pro aplikaci (Pokud je podporuje), vyžaduje, aby konkrétní pokyny aplikace hello nakonec tooprepare pro automatické zřizování. Pak můžete použít hello Azure portálu tooconfigure hello zřizování aplikace toohello služby toosynchronize uživatelské účty.

Vždy byste měli začít hledání hello instalace kurz konkrétní toosetting až zřizování pro vaši aplikaci. Potom postupujte podle těchto kroků tooconfigure obě aplikace hello a Azure AD toocreate hello zřizování připojení. Seznam kurzů aplikace najdete na [seznamu kurzy o tooIntegrate SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-toosee-if-provisioning-is-working"></a>Jak funguje toosee Pokud zřizování 

Po nakonfigurování služby hello lze většinu přehledy hello operaci služby hello rozlišovat ze dvou míst:

-   **Protokoly auditu** – hello zřizování záznam protokoly auditu všechny operace hello provádí hello zřizování služby, včetně dotazování Azure AD pro přiřazené uživatele, kteří jsou v oboru pro zřizování. Dotaz aplikace hello cíl pro hello existenci uživatelům porovnávání hello uživatelských objektů mezi hello systému. Potom přidání, aktualizace nebo zakázat hello uživatelský účet v cílovém systému hello na základě porovnání hello. Hello zřizování protokoly auditu je přístupná v hello portál Azure, v hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; protokolech auditování**kartě. Filtr hello přihlásí hello **zřizování účtu** kategorie tooonly najdete v části hello zřizování události pro tuto aplikaci.

-   **Stav – zřízení** souhrn hello poslední zřizování spusťte pro danou aplikaci si můžete prohlédnout ve hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; Zřizování** části v hello dolní části obrazovky hello v části Nastavení služby hello. Tento oddíl shrnuje, kolik uživatelů (nebo skupin) aktuálně dochází k synchronizaci mezi hello dvěma systémy, a pokud nejsou žádné chyby. Podrobnosti o chybě se v protokolech auditu hello. Všimněte si, že hello Stav zřizování nebyla vyplněný, dokud se nedokončí jeden úplné počáteční synchronizaci mezi službou Azure AD a aplikace hello.

## <a name="general-problem-areas-with-provisioning-tooconsider"></a>Obecné problémových oblastí se zřizováním tooconsider

Níže uvádíme seznam hello obecné problémových oblastí, které můžete rozbalit Pokud máte představu o kde toostart.

* [Zřizování služby nezobrazí toostart](#provisioning-service-does-not-appear-to-start)
* [Nelze uložit konfiguraci z důvodu tooapp pověření nepracuje](#can’t-save-configuration-due-to-app-credentials-not-working)
* [Protokoly auditu, že uživatelé jsou "přeskočen" a není zajišťováno, i když jsou přiřazeny](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-toostart"></a>Zřizování služby nezobrazí toostart

Pokud nastavíte hello **Stav zřizování** toobe **na** v hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt;Zřizování** části hello portálu Azure. Žádné další podrobnosti o stavu se ale zobrazují na této stránce po následné znovu načte. Je pravděpodobné, že hello služba běží, ale nebyl dokončen ještě počáteční synchronizaci. Zkontrolujte hello **protokoly auditu** popsané výše toodetermine jaké operace hello služba pracuje, a pokud nejsou žádné chyby.

>[!NOTE]
>Počáteční synchronizace může trvat hodiny tooseveral 20 minut, v závislosti na velikosti hello hello adresář Azure AD a hello počet uživatelů v oboru pro zřizování. Následné synchronizace po počáteční synchronizaci hello být rychlejší jako hello zřizování služby ukládá vodoznaky, které představují hello stav obou systémů po počáteční synchronizaci hello, zvýšení výkonu následné synchronizace.
>
>

## <a name="cant-save-configuration-due-tooapp-credentials-not-working"></a>Nelze uložit konfiguraci z důvodu tooapp pověření nepracuje

Aby zřizování toowork Azure AD vyžaduje platné přihlašovací údaje, které ji povolit správu uživatelů tooa tooconnect rozhraní API poskytované tuto aplikaci. Pokud tyto přihlašovací údaje nefungují, nebo neznáte wat, které se nacházejí, přečtěte si kurz hello pro nastavení této aplikace a popsané.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Protokoly auditu, že uživatelé jsou přeskočeny a není zajišťováno i když jsou přiřazeny

Když uživatel se zobrazí jako "přeskočen" v protokolech auditu hello, je velmi důležité tooread hello Rozšířené podrobnosti v hello protokolu zpráv toodetermine hello důvod. Níže jsou uvedeny běžné příčiny a řešení:

-   **Byl nakonfigurován oboru filtru** **, je filtrování hello uživatele podle hodnota atributu**. Další informace o filtry oborů najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **uživatel Hello nárok "není efektivně".** Pokud se zobrazí tato konkrétní chybová zpráva, je to, protože došlo k potížím s hello uživatele přiřazení záznam uložené ve službě Azure AD. toofix-li tento problém, zrušte přiřazení z aplikace hello hello uživatele (nebo skupiny) a znovu přiřadit. Další informace o přiřazení najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Požadovaný atribut nebyl nalezen nebo není vyplněná pro uživatele.** Tooconsider důležité při nastavování zřizování být tooreview a konfigurace mapování atributů hello a pracovní postupy, které definují vlastnosti toku z Azure AD toohello aplikace které uživatele (nebo skupiny). To zahrnuje nastavení hello "odpovídající vlastnost", který se použité toouniquely identifikovat a odpovídají uživatele nebo skupiny mezi hello dvěma systémy. Další informace o tomto procesu důležité najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

   * **Mapování pro skupiny atributů:** zřizování hello skupiny názvů a skupinových podrobnosti v přidání toohello členy, pokud pro některé aplikace podporován. Můžete povolit nebo zakázat tuto funkci povolením nebo zakázáním hello **mapování** pro objekty skupiny uvedené v hello **zřizování** kartě. Pokud zřizování skupiny je povolena, ujistěte se, že tooreview hello atribut mapování tooensure na odpovídající pole se používá pro hello "odpovídající ID". Může to být hello zobrazovaný název nebo e-mailu alias), jak hello skupiny a její členy nelze zřídit Pokud hello odpovídající vlastnost je prázdný nebo není vyplněná skupiny ve službě Azure AD.

#<a name="next-steps"></a>Další kroky
[Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md)
