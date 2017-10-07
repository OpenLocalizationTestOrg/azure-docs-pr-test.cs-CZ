---
title: "aaaProblem přihlášení toohello přístup k panelu webu | Microsoft Docs"
description: "Pokyny tootroubleshoot problémy, ke kterým může dojít při pokusu o toosign v toouse hello přístupového panelu"
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
ms.reviwer: japere
ms.openlocfilehash: 1037f7c5fbaa9425760ad5739b383c716d5fc3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-signing-in-toohello-access-panel-website"></a>Potíže s přihlášením toohello přístup k panelu webu

Hello přístupový Panel je, že je webový portál, který umožňuje uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spouštět cloudové aplikace tento správce hello Azure AD udělil přístup. Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu. Hello přístupový Panel je oddělená od hello portál Azure a nevyžaduje, aby uživatelé toohave předplatné Azure.

Uživatelé můžou přihlásit toohello přístupového panelu v případě, že mají pracovní nebo školní účet ve službě Azure AD.

-   Uživatelé se můžou ověřovat službou Azure AD přímo.

-   Uživatelé se můžou ověřovat pomocí služby Active Directory Federation Services (AD FS).

-   Uživatelé se můžou ověřovat pomocí služby Windows Server Active Directory.

Pokud je uživatel má předplatné Azure nebo Office 365 a používá hello portál Azure nebo Office 365 aplikaci, bude se mít toouse hello přístupový Panel bezproblémově bez nutnosti toosign v akci. Uživatelé, kteří nejsou ověřené být výzvami toosign v pomocí hello uživatelské jméno a heslo pro svůj účet ve službě Azure AD. Pokud organizace hello nakonfiguroval federace, zadejte uživatelské jméno hello je dostačující.

## <a name="general-issues-toocheck-first"></a>Obecné problémy toocheck nejprve 

-   Zajistěte, aby uživatel hello přihlašuje toohello **opravte adresu URL**: <https://myapps.microsoft.com>

-   Zajistěte, aby prohlížeče hello uživatele přidala hello URL tooits **Důvěryhodné servery**

-   Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení.

-   Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**

-   Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.**

-   Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.

-   Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.

-   Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené.

-   Zkontrolujte že tooalso zkuste vymazání souborů cookie v prohlížeči a toosign v zkusit to znovu.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Splňuje požadavky na prohlížeč pro hello přístupového panelu

Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:

-   Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější

-   Hraniční Windows 10 Anniversary Edition nebo novější. 

-   Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější

-   Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější


## <a name="problems-with-hello-users-account"></a>Problémy s hello uživatelský účet

Přístup toohello přístupového panelu můžete blokovat z důvodu tooa problém s hello uživatelský účet. Tady jsou některé způsoby řešení problémů a řešení problémů s uživateli a jejich nastavení účtu:

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

   >[!NOTE]
   >Pokud je uživatel v **vynucené** stavu, je pravděpodobně příliš nastavit**zakázané** dočasně toolet je zpět do svého účtu. Jakmile se zpět, můžete změnit jejich stavu příliš**povoleno** znovu toorequire je toore registrace své kontaktní informace při příštím přihlášení. Alternativně můžete provést kroky hello v hello [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info) tooverify nebo pro ně nastavit tato data.
   >
   >

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

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Pokud tyto kroky řešení potíží Nepokoušejte se vyřešit problém hello

Otevřete lístek podpory se hello, pokud je k dispozici následující informace:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   ID tenanta

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
