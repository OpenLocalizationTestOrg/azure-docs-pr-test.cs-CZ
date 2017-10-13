---
title: "Jak aplikace se objeví na panel přístupu | Microsoft Docs"
description: "Poradce při potížích se aplikace se zobrazuje na přístupovém panelu"
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
ms.openlocfilehash: f8ccf2cf66b49940bc7f2b9f4764020efc04838e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a>Jak aplikace se objeví na panel přístupu

Přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) k zobrazení a spustit cloudové aplikace, aby správce Azure AD má je přístup povolen. Tyto aplikace jsou konfigurovány jménem uživatele na portálu Azure AD. Správce můžete zřídit aplikaci na tohoto uživatele přímo nebo do skupiny uživatel je součástí výsledkem aplikace, které jsou uvedeny na Panel přístupu uživatele.

## <a name="general-issues-to-check-first"></a>Běžné problémy a proveďte nejprve kontrolu

-   Pokud aplikace byla právě odebrána ze uživatele nebo skupiny, kterých je uživatel členem, zkuste pro přihlášení a odhlášení znovu do přístupového panelu uživatele po několika minutách zda je aplikace odebrat.

-   Pokud licence byla právě odebrána ze uživatele nebo skupinu je uživatel že členem skupiny to může trvat dlouhou dobu, v závislosti na velikost a složitost skupiny změny má být provedeno. Povolit další dobu před přihlášení k přístupovému panelu.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problémy související s přiřazení aplikací pro uživatele

Uživatel může být zobrazuje aplikace na jejich přístupový Panel měl byly dřív přiřazené k němu. Tady jsou některé způsoby, jak zkontrolovat:

-   [Zkontrolujte, pokud uživatel je přiřazená k aplikaci](#check-if-a-user-is-assigned-to-the-application)

-   [Zkontrolujte, jestli je uživatel v rámci licence týkající se aplikace](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Zkontrolujte, pokud uživatel je přiřazená k aplikaci

Pokud chcete zkontrolovat, pokud má uživatel přiřazeno k aplikaci, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

6.  **Hledání** pro název dané žádosti.

7.  Klikněte na tlačítko **uživatelů a skupin**.

8.  Zkontrolujte, pokud uživatel je přiřazena k aplikaci.

  * Pokud chcete odebrat uživatele z aplikace, **klikněte na řádek** uživatele a vyberte **odstranit**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Zkontrolujte, jestli je uživatel v rámci licence týkající se aplikace

Pokud chcete zkontrolovat uživatele přiřazené licence, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.

7.  Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.

   * Pokud uživatel je přiřazen k Office licence touto aplikací povolit první strany Office se objevily na Panel přístupu uživatele.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problémy související s přiřazování aplikací do skupin

Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace. Tady jsou některé způsoby, jak zkontrolovat:

-   [Zkontrolujte členství uživatele ve skupinách](#check-a-users-group-memberships)

-   [Zkontrolujte, jestli je uživatel členem skupiny přiřazené k licenci](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Zkontrolujte členství uživatele ve skupinách

Pokud chcete zkontrolovat členství ve skupině, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.

7.  Klikněte na tlačítko **skupiny.**

8.  Zkontrolujte, zda uživatel je součástí skupiny přiřazené k aplikaci.

   * Pokud chcete odebrat uživatele ze skupiny, **klikněte na řádek** tuto skupinu a vyberte možnost odstranit.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Zkontrolujte, jestli je uživatel členem skupiny přiřazené k licenci

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.

7.  Klikněte na tlačítko **skupiny.**

8.  Klikněte na řádek určité skupiny.

9.  Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí má přiřazen.

  * Pokud tato skupina je přiřazená k licenci Office to může povolit konkrétní aplikace Office první strany se objevily na Panel přístupu uživatele.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Pokud tyto kroky řešení potíží se není vyřešit problém

Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   ID tenanta

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
