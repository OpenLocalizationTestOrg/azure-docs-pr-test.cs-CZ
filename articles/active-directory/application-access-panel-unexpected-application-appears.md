---
title: "aaaHow aplikace se objeví na panel přístupu hello | Microsoft Docs"
description: "Poradce při potížích se aplikace se zobrazuje v hello přístupového panelu"
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
ms.reviewr: japere
ms.openlocfilehash: 14ee732c4ed5260cba878e949cf9d90877aee67e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-applications-appear-on-hello-access-panel"></a>Jak aplikace se objeví na panel přístupu hello

Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k. Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello. Dobrý den, správce můžete zřídit hello aplikace toohello uživatel přímo nebo tooa skupinu, kterou uživatel je součástí výsledkem aplikace hello zobrazovaných na Panel přístupu hello uživatele.

## <a name="general-issues-toocheck-first"></a>Obecné problémy toocheck nejprve

-   Pokud aplikace právě odebral od uživatele nebo skupiny hello uživatel je členem skupiny, opakujte toosign a odhlašování do přístupového panelu hello uživatele po několika minutách toosee Pokud odebrání aplikace hello.

-   Pokud licence byla právě odebrána ze uživatele nebo skupiny hello uživatel je že členem skupiny to může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny pro toobe změny provedené. Povolit další dobu před přihlášením hello přístupového panelu.

## <a name="problems-related-tooassigning-applications-toousers"></a>Problémy související tooassigning aplikace toousers

Uživatel může být zobrazuje aplikace na jejich přístupového panelu jejich měl byly dřív přiřazené tooit. Tady jsou některé toocheck způsoby:

-   [Zkontrolujte, pokud uživatel je přiřazená toohello aplikace](#check-if-a-user-is-assigned-to-the-application)

-   [Zkontrolujte, jestli je uživatel v rámci licence související toohello aplikace](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-toohello-application"></a>Zkontrolujte, pokud uživatel je přiřazená toohello aplikace

toocheck Pokud uživatel přiřazený toohello aplikace, postupujte podle kroků hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

6.  **Hledání** pro název hello aplikace hello nejistá.

7.  Klikněte na tlačítko **uživatelů a skupin**.

8.  Toosee zkontrolujte, zda váš uživatel toohello aplikace.

  * Pokud chcete, aby uživatel hello tooremove z aplikace hello **klikněte na řádek hello** hello uživatele a vyberte **odstranit**.

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a>Zkontrolujte, jestli je uživatel v rámci licence související toohello aplikace

toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.

   * Pokud uživatel hello přiřazené tooan Office licence tato povolí tooappear aplikace první strany Office na Panel přístupu hello uživatele.

## <a name="problems-related-tooassigning-applications-toogroups"></a>Problémy související tooassigning aplikace toogroups

Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace hello. Tady jsou některé toocheck způsoby:

-   [Zkontrolujte členství uživatele ve skupinách](#check-a-users-group-memberships)

-   [Zkontrolujte, zda uživatel je členem skupiny přiřazené licence tooa](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Zkontrolujte členství uživatele ve skupinách

toocheck členství ve skupině, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **skupiny.**

8.  Zkontrolujte toosee, pokud uživatel je součástí aplikace toohello skupiny přiřazené.

   * Pokud chcete, aby tooremove hello uživatele ze skupiny hello **klikněte na řádek hello** hello skupiny a vyberte možnost odstranit.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-tooa-license"></a>Zkontrolujte, zda uživatel je členem skupiny přiřazené licence tooa

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **skupiny.**

8.  Klikněte na řádek hello určité skupiny.

9.  Klikněte na tlačítko **licence** toosee, které skupiny licencí hello přiřazenému tooit.

  * Pokud skupina hello přiřazené tooan Office licence to může povolit určité aplikace tooappear první strany Office na Panel přístupu hello uživatele.


## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Pokud tento postup řešení není hello hello problém vyřešte

Otevřete lístek podpory se hello, pokud je k dispozici následující informace:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   ID tenanta

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
