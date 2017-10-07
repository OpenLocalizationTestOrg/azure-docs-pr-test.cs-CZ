---
title: "aaaProblems přihlášení tooa aplikace Microsoft | Microsoft Docs"
description: "Řešení běžných potíží potýkají při přihlášení výrobců toofirst Microsoft Applications pomocí služby Azure AD (např. Office 365)"
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
ms.openlocfilehash: 35849ca8dbaa909d17b6d0da572f5c11041a8559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="problems-signing-in-tooa-microsoft-application"></a>Potíže s přihlášením tooa aplikace Microsoft

Microsoft Applications (např. Office 365 Exchange, SharePoint, Yammer atd.) jsou přiřazeny a trochu jinak než 3. aplikace SaaS strany nebo jiné aplikace, které integrují s Azure AD pro jednotné přihlašování na spravovat.

Existují tři hlavní způsoby, které uživatel může získat přístup k tooa Microsoft publikované aplikace.

-   Pro aplikace v hello Office 365 nebo jiné placené sady, mají uživatelé udělen přístup prostřednictvím **přiřazení licence** buď přímo tootheir uživatelský účet, nebo prostřednictvím skupiny pomocí funkce přiřazení našeho na základě skupin licencí.

-   Pro aplikace společnosti Microsoft nebo třetích stran publikování, a to volně pro každý, kdo toouse, může být uživatelé udělen přístup prostřednictvím **souhlas uživatele**. This0 znamená, že se přihlaste toohello aplikace s jejich Azure AD pracovního nebo školního účtu a povolit toohave přístup toosome omezenou sadu dat na svůj účet.

-   Pro aplikace, že společnost Microsoft nebo 3rd strany publikuje volně pro každý, kdo toouse, uživatelé také udělit přístup přes **souhlas správce**. To znamená, že správce zjistí, že aplikace hello může být používán všichni uživatelé v organizaci hello, takže přihlásit toohello aplikaci pomocí účtu globálního správce a udělit přístup tooeveryone v organizaci hello.

tootroubleshoot problém, začněte s hello [obecné problémových oblastí s přístup k aplikaci tooconsider](#general-problem-areas-with-application-access-to-consider) a potom si přečtěte hello [návod: kroky tootroubleshoot Microsoft Application přístup](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget do hello podrobnosti.

## <a name="general-problem-areas-with-application-access-tooconsider"></a>Obecné problémových oblastí s tooconsider přístup k aplikaci

Níže uvádíme seznam hello obecné problémových oblastí, které můžete rozbalit Pokud máte představu o kterém se toostart, ale nemůžeme doporučujeme, číst tooget hello návodu budete rychle: [návod: kroky tootroubleshoot Microsoft Application přístup](#walkthrough-steps-to-troubleshoot-microsoft-application-access).

-   [Problémy s hello uživatelský účet](#problems-with-the-users-account)

-   [Problémy se skupinami](#problems-with-groups)

-   [Problémy se zásadami podmíněného přístupu](#problems-with-conditional-access-policies)

-   [Problémy s aplikací souhlasu](#problems-with-application-consent)

## <a name="steps-tootroubleshoot-microsoft-application-access"></a>Kroky tootroubleshoot Microsoft Application přístup

Níže jsou některé běžné problémy zaměstnance spustit do při jejich uživatelé nemůžete se přihlásit tooa aplikace společnost Microsoft.

-   Obecné problémy toocheck nejprve

  * Zajistěte, aby uživatel hello přihlašuje toohello **opravte adresu URL** a ne místní aplikace adresy URL.

  * Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**

  * Ujistěte se, zda text hello **uživatelský účet existuje** v Azure Active Directory. [Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory](#problems-with-the-users-account)

  * Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení. [Zkontrolujte stav účtu uživatele](#problems-with-the-users-account)

  * Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.** [Resetovat heslo uživatele](#reset-a-users-password) nebo [povolit samoobslužné resetování hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

   * Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů. [Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status) nebo [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)

   * Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů. [Zkontrolujte zásady podmíněného přístupu konkrétním](#problems-with-conditional-access-policies) nebo [zkontrolujte zásady podmíněného přístupu pro konkrétní aplikaci](#check-a-specific-applications-conditional-access-policy) nebo [zakázat zásadu podmíněného přístupu konkrétním](#disable-a-specific-conditional-access-policy)

   * Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené. [Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status) nebo [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)

-   Pro **Microsoft** **aplikace, které vyžadují licenci** (např. Office 365), tady jsou některé konkrétní problémy toocheck po nevyloučí hello obecné problémy výše:

   * Ujistěte se, hello uživatele nebo má **přiřazenou licenci.** [Zkontrolujte uživatele přiřazené licence](#check-a-users-assigned-licenses) nebo [Zkontrolujte skupiny přiřazené licence](#check-a-groups-assigned-licenses)

   * Pokud je licence hello **přiřazené tooa** **statická skupina**, ujistěte se, že hello **uživatel je členem** této skupiny. [Zkontrolujte členství uživatele ve skupinách](#check-a-users-group-memberships)

   * Pokud je licence hello **přiřazené tooa** **dynamická skupina**, ujistěte se, že hello **je dynamická skupina pravidlo správně nastavené**. [Zkontrolujte kritéria členství dynamické skupiny](#check-a-dynamic-groups-membership-criteria)

   * Pokud je licence hello **přiřazené tooa** **dynamická skupina**, ujistěte se, že hello dynamická skupina má **bylo dokončeno zpracování** jeho členství a že hello **uživatel člen** (to může trvat nějakou dobu). [Zkontrolujte členství uživatele ve skupinách](#check-a-users-group-memberships)

   *  Jakmile budete mít jistotu, hello licence se nepřiřadí, zkontrolujte, zda je licence hello **nevypršela**.

   *  Zkontrolujte, zda je licence hello **aplikace hello** uživatelé přistupují.

-   Pro **Microsoft** **aplikace, které nevyžadují licenci**, zde jsou některé další toocheck věcí:

   * Pokud aplikace hello požaduje **oprávnění na úrovni uživatele** (například "přístup k poštovní schránka tohoto uživatele"), zajistěte, aby tento uživatel hello se přihlásil toohello aplikace a byla provedena **individuální souhlasu operace**  toolet hello aplikace přístup k jeho data.

   * Pokud aplikace hello požaduje **oprávnění na úrovni správce** (například "přístup k poštovním schránkám všechny uživatelské"), ujistěte se, že byla provedena globálního správce **operace správce úroveň souhlasu s jménem všech uživatelů** v organizaci hello.

## <a name="problems-with-hello-users-account"></a>Problémy s hello uživatelský účet

Z důvodu tooa problém s uživatelem, který je přiřazen toohello aplikace můžete blokovat přístup k aplikaci. Tady jsou některé způsoby řešení problémů a řešení problémů s uživateli a jejich nastavení účtu:

-   [Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Zkontrolujte stav účtu uživatele](#check-a-users-account-status)

-   [Resetovat heslo uživatele](#reset-a-users-password)

-   [Povolení samoobslužného resetování hesla](#enable-self-service-password-reset)

-   [Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status)

-   [Zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)

-   [Zkontrolujte členství uživatele ve skupinách](#check-a-users-group-memberships)

-   [Zkontrolujte uživatele přiřazené licence](#check-a-users-assigned-licenses)

-   [Přiřazení licence uživatele](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory

toocheck, pokud se nachází uživatelský účet, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Zkontrolujte vlastnosti hello hello uživatele objektu toobe se, že zobrazují se podle očekávání a nebyla nalezena žádná data.

### <a name="check-a-users-account-status"></a>Zkontrolujte stav účtu uživatele

toocheck uživatele pro účet stavu, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **profil**.

8.  V části **nastavení** Ujistěte se, že **bloku přihlásit** je nastaven příliš**ne**.

### <a name="reset-a-users-password"></a>Resetovat heslo uživatele

tooreset heslo uživatele, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko hello **resetovat heslo** tlačítko hello horní části okna hello uživatele.

8.  Klikněte na tlačítko hello **resetovat heslo** na hello tlačítko **resetovat heslo** okno, které se zobrazí.

9.  Kopírování hello **dočasné heslo** nebo **zadat nové heslo** pro uživatele hello.

10. Komunikaci tohoto nového uživatele toohello heslo, ale měly požadované toochange toto heslo při příštím přihlášení tooAzure služby Active Directory.

### <a name="enable-self-service-password-reset"></a>Povolit samoobslužné resetování hesla

tooenable samoobslužné služby heslo resetovat, postupujte podle následujících pokynů hello nasazení:

-   [Povolit uživatelům tooreset hesel služby Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Povolit uživatelům tooreset nebo změnit své heslo místní služby Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>Zkontrolujte stav služby Multi-Factor authentication uživatele

toocheck uživatele je vícefaktorového ověřování stavu, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  Klikněte na tlačítko hello **Multi-Factor Authentication** tlačítko hello horní části okna hello.

7.  Jednou hello **portálu pro správu služby Multi-Factor Authentication** zatížení, ujistěte se, jsou na hello **uživatelé** kartě.

8.  Najít hello uživatele v seznamu hello uživatelů ve vyhledávání, filtrování a řazení.

9.  Vyberte hello uživatele z hello seznam uživatelů a **povolit**, **zakázat**, nebo **vynutit** služby Multi-Factor authentication podle potřeby.

  * **Poznámka:**: Pokud je uživatel v **vynucené** stavu, je pravděpodobně příliš nastavit**zakázané** dočasně toolet je zpět do svého účtu. Jakmile se zpět, můžete změnit jejich stavu příliš**povoleno** znovu toorequire je toore registrace své kontaktní informace při příštím přihlášení. Alternativně můžete provést kroky hello v hello [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info) tooverify nebo pro ně nastavit tato data.

### <a name="check-a-users-authentication-contact-info"></a>Zkontrolovat kontaktní informace o ověřování uživatele

toocheck ověřování uživatele kontaktní informace pro vícefaktorové ověřování, podmíněného přístupu, ochrany identit a resetování hesla, postupujte podle kroků hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **profil**.

8.  Posuňte se dolů příliš**ověřování kontaktní údaje**.

9.  **Zkontrolujte** hello dat registrované pro uživatele hello a aktualizace podle potřeby.

### <a name="check-a-users-group-memberships"></a>Zkontrolujte členství uživatele ve skupinách

toocheck uživatele na členství ve skupinách, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **skupiny** toosee, které skupiny hello uživatel je členem skupiny.

### <a name="check-a-users-assigned-licenses"></a>Zkontrolujte uživatele přiřazené licence

toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.

### <a name="assign-a-user-a-license"></a>Přiřazení licence uživatele 

tooassign licence tooa uživatele, postupujte podle kroků hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.

8.  Klikněte na tlačítko hello **přiřadit** tlačítko.

9.  Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.

10. **Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty. Klikněte na tlačítko **Ok** po dokončení.

11. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign tyto licence toothis uživatele.

## <a name="problems-with-groups"></a>Problémy se skupinami

Z důvodu tooa problém s skupinu, která je přiřazena toohello aplikace můžete blokovat přístup k aplikaci. Tady jsou některé způsoby řešení problémů a řešení problémů s skupin a členství ve skupinách:

-   [Zkontrolovat členství ve skupině](#check-a-groups-membership)

-   [Zkontrolujte kritéria členství dynamické skupiny](#check-a-dynamic-groups-membership-criteria)

-   [Zkontrolujte skupiny přiřazené licence](#check-a-groups-assigned-licenses)

-   [Znovu zpracovat skupině licencí](#reprocess-a-groups-licenses)

-   [Přiřazení skupiny licencí](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Zkontrolovat členství ve skupině

toocheck členství ve skupině, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny skupiny**.

6.  **Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **členy** toothis skupiny přiřazeny tooreview hello seznam uživatelů.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Zkontrolujte kritéria členství dynamické skupiny 

toocheck kritéria členství dynamické skupiny, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny skupiny**.

6.  **Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **pravidla dynamické členství.**

8.  Zkontrolujte hello **jednoduché** nebo **rozšířené** pravidlo definované pro tuto skupinu a ujistěte se, že hello uživatele chcete toobe členem této skupiny splňuje tato kritéria.

### <a name="check-a-groups-assigned-licenses"></a>Zkontrolujte skupiny přiřazené licence

toocheck skupinu na přiřadit licence, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny skupiny**.

6.  **Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.

### <a name="reprocess-a-groups-licenses"></a>Znovu zpracovat skupině licencí

tooreprocess skupinu na přiřadit licence, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny skupiny**.

6.  **Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.

8.  Klikněte na tlačítko hello **znovu zpracovat** tooensure tlačítko, která hello členy skupiny toothis přiřazené licence jsou aktuální. To může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny.

   >[!NOTE]
   >toodo to rychlejší, vezměte v úvahu dočasně přiřazení licence toohello uživatele přímo. [Přiřazení licence uživatele](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Přiřazení skupiny licencí

tooassign tooa skupiny licencí, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny skupiny**.

6.  **Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.

8.  Klikněte na tlačítko hello **přiřadit** tlačítko.

9.  Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.

10. **Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty. Klikněte na tlačítko **Ok** po dokončení.

11. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign těchto toothis skupin licencí. To může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny.

   >[!NOTE]
   >toodo to rychlejší, vezměte v úvahu dočasně přiřazení licence toohello uživatele přímo. [Přiřazení licence uživatele](#problems-with-application-consent).
   > 
   >

## <a name="problems-with-conditional-access-policies"></a>Problémy se zásadami podmíněného přístupu

### <a name="check-a-specific-conditional-access-policy"></a>Zkontrolujte zásady podmíněného přístupu konkrétním

toocheck, případně k ověření zásad jednoho podmíněného přístupu:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce hello.

5.  Klikněte na tlačítko hello **podmíněného přístupu** navigační položka.

6.  Klikněte na tlačítko hello zásady, které vás zajímají kontroly.

7.  Zkontrolujte, že neexistují žádné konkrétní podmínky, přiřazení nebo jiná nastavení, které blokuje přístup uživatelů.

   >[!NOTE]
   >Může přát zakázat tootemporarily této zásady tooensure ji nemají vliv na sign in toodo se sada hello **povolit zásady** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko .
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Zkontrolujte zásady podmíněného přístupu pro konkrétní aplikace

toocheck, případně k ověření zásad jednu aplikaci aktuálně nakonfigurovaný podmíněný přístup:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny aplikace**.

6.  Vyhledávání pro aplikace hello zájem o nebo hello uživatel se pokouší toosign v aplikaci tooby zobrazení id, nebo název aplikace.

     >[!NOTE]
     >Pokud nevidíte hello aplikace, které hledáte, klikněte na tlačítko hello **filtru** tlačítko a rozbalte hello oboru hello seznamu příliš**všechny aplikace**. Pokud chcete toosee více sloupců, klikněte na tlačítko hello **sloupce** tlačítko tooadd další podrobnosti pro vaše aplikace.
     >
     >

7.  Klikněte na tlačítko hello **podmíněného přístupu** navigační položka.

8.  Klikněte na tlačítko hello zásady, které vás zajímají kontroly.

9.  Zkontrolujte, že neexistují žádné konkrétní podmínky, přiřazení nebo jiná nastavení, které blokuje přístup uživatelů.

     >[!NOTE]
     >Může přát zakázat tootemporarily této zásady tooensure ji nemají vliv na sign in toodo se sada hello **povolit zásady** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko .
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Zakázat zásadu podmíněného přístupu konkrétním

toocheck, případně k ověření zásad jednoho podmíněného přístupu:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce hello.

5.  Klikněte na tlačítko hello **podmíněného přístupu** navigační položka.

6.  Klikněte na tlačítko hello zásady, které vás zajímají kontroly.

7.  Zakažte zásadu hello nastavení hello **povolit zásady** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko.

## <a name="problems-with-application-consent"></a>Problémy s aplikací souhlasu

Přístup k aplikaci můžete blokovat, protože nedošlo k operaci souhlasu hello příslušná oprávnění. Tady jsou některé způsoby řešení problémů a vyřešit problémy s aplikací souhlasu:

-   [Provedení operace individuální souhlasu](#perform-a-user-level-consent-operation)

-   [Provedení operace správce úroveň souhlasu pro žádnou aplikaci.](#perform-administrator-level-consent-operation-for-any-application)

-   [Provést na úrovni správce souhlasu pro jednoho klienta aplikace](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Provést na úrovni správce souhlasu pro víceklientské aplikace](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Provedení operace individuální souhlasu

-   Pro každou Open ID Connect-povolit aplikaci, která vyžaduje oprávnění navigace přihlašovací obrazovka, toohello aplikace provede uživatelská aplikace toohello úrovně souhlasu pro hello přihlášeného uživatele.

-   Pokud chcete toodo to prostřednictvím kódu programu, najdete v části [vyžaduje souhlas jednotlivé uživatele](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Provedení operace správce úroveň souhlasu pro žádnou aplikaci.

-   Pro **jenom aplikace vyvinuté pomocí modelu aplikace hello V1**, můžete vynutit tento správce úrovně souhlasu toooccur přidáním "**? řádku = správce\_souhlas**" toohello konec Přihlašovací adresa URL aplikace.

-   Pro **všechny aplikace vyvinuté pomocí modelu aplikace hello V2**, můžete vynutit tento správce úroveň souhlasu toooccur podle pokynů hello pod hello **žádostí o oprávnění hello z adresáře správce** části [používá koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Provést na úrovni správce souhlasu pro jednoho klienta aplikace

-   Pro **jednoho klienta aplikace** , žádostí o oprávnění (jako jsou ty, které jste vývoji nebo vlastní ve vaší organizaci), můžete provést **správu úroveň souhlasu** operace jménem všechny uživatelé přihlášení jako globální správce a kliknete na hello **udělit oprávnění** tlačítko hello horní části hello **aplikace registru -&gt; všechny aplikace -&gt; vyberte aplikaci - &gt; Požadovaných oprávnění** okno.

-   Pro **všechny aplikace vyvinuté pomocí hello V1 nebo V2 aplikačního modelu**, můžete vynutit tento správce úroveň souhlasu toooccur podle pokynů hello pod hello **žádostí o oprávnění hello z Správce Directory** části [používá koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Provést na úrovni správce souhlasu pro víceklientské aplikace

-   Pro **víceklientským aplikacím** této žádosti o oprávnění (jako jsou aplikace třetích stran nebo sama vyvinula společnost Microsoft,), můžete provést **správu úroveň souhlasu** operaci. Přihlaste se jako globální správce a kliknete na hello **udělit oprávnění** tlačítko hello **podnikové aplikace –&gt; všechny aplikace -&gt; vyberte aplikaci -&gt; Oprávnění** okno (k dispozici brzy).

-   Tento správce úroveň souhlasu toooccur může také vynucovat podle pokynů hello pod hello **hello oprávnění požádat správce directory** části [pomocí koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Další kroky
[Pomocí koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

