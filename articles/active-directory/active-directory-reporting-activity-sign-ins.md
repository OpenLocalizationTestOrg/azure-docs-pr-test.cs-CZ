---
title: "sestavy aktivit aaaSign v portálu Azure Active Directory hello | Microsoft Docs"
description: "Úvod aktivitu toosign sestav na portálu Azure Active Directory hello"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 49590d625a08d7dc189a629b89bab2261c2b4780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-reports-in-hello-azure-active-directory-portal"></a>Přihlašovací aktivity sestav na portálu Azure Active Directory hello

S vytvářením sestav Azure Active Directory (Azure AD) v hello [portál Azure](https://portal.azure.com), můžete získat hello informace, které potřebujete toodetermine úspěšnost prostředí.

Hello reporting architektury v Azure Active Directory se skládá z hello následující součásti:

- **Aktivita** 
    - **Přihlašovací aktivity** – informace o využití hello spravovaných aplikací a aktivit přihlášení uživatelů
    - **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů
- **Zabezpečení** 
    - **Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu. Další podrobnosti najdete v tématu Riziková přihlášení.
    - **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.

Toto téma poskytuje přehled hello přihlašovací aktivity.

## <a name="pre-requisite"></a>Předpoklad

### <a name="who-can-access-hello-data"></a>Kdo může přistupovat k datům hello?
* Uživatelé v roli správce zabezpečení nebo zabezpečení čtečky hello
* Globální správci
* Každý uživatel (bez oprávnění správce) může přistupovat k vlastnímu přihlašování. 

### <a name="what-azure-ad-license-do-you-need-tooaccess-sign-in-activity"></a>Jaké licence Azure AD potřebujete přihlašovací aktivity tooaccess?
* Váš klient musí mít licenci na Azure AD Premium s ním spojená toosee hello všechny až přihlašovací aktivity sestavy


## <a name="signs-in-activities"></a>Aktivity přihlašování

S hello informací uvedených v sestavě přihlášení uživatele hello najít tooquestions odpovědi, například:

* Co je hello přihlášení vzor uživatele?
* Kolik uživatelů se přihlásilo za týden?
* Co je hello stav těchto přihlášení?

Vaše první vstupní bod tooall přihlašovací aktivity data **přihlášení** v části aktivity hello **Azure Active**.


![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/61.png "Aktivita přihlašování")


Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- související uživatelské Hello
- uživatel hello aplikace Hello má přihlášeného k
- Stav přihlášení Hello
- čas přihlášení Hello

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/41.png "Aktivita přihlašování")

Kliknutím můžete přizpůsobit zobrazení seznamu hello **sloupce** v panelu nástrojů hello.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/19.png "Aktivita přihlašování")

To vám umožní toodisplay další pole nebo odeberte pole, které jsou již zobrazen.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/42.png "Aktivita přihlašování")

Kliknutím na položku v zobrazení seznamu hello zobrazí všechny dostupné podrobné informace o něm.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/43.png "Aktivita přihlašování")


## <a name="filtering-sign-in-activities"></a>Filtrování aktivit přihlašování

toonarrow dolů hello nahlášené tooa dat na úrovni, funguje pro vás, data lze filtrovat hello přihlášení pomocí hello následující pole:

- Časový interval
- Uživatel
- Aplikace
- Klient
- Stav přihlášení

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/44.png "Aktivita přihlašování")


Hello **časový interval** filtr povoluje tooyou toodefine časový rámec pro hello vrátil data.  
Možné hodnoty:

- 1 měsíc
- 7 dní
- 24 hodin
- Vlastní

Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.

Hello **uživatele** filtru vám umožní toospecify hello název nebo hello hlavní název uživatele (UPN) uživatele hello kterých vám nejvíc záleží.

Hello **aplikace** filtru vám umožní toospecify hello název aplikace hello kterých vám nejvíc záleží.

Hello **klienta** filtru vám umožní toospecify informace o zařízení hello kterých vám nejvíc záleží.

Hello **stav přihlášení** filtru vám umožní tooselect jeden hello následující filtru:

- Všechny
- Úspěch
- Selhání


## <a name="sign-in-activities-shortcuts"></a>Zkratky pro aktivity přihlašování

Kromě tooAzure služby Active Directory, hello portál Azure poskytuje dva další vstupní body aktivity toosign v údaje:

- Uživatelé a skupiny
- Podnikové aplikace


### <a name="users-and-groups-sign-ins-activities"></a>Aktivity přihlašování uživatelů a skupin

S hello informací uvedených v sestavě přihlášení uživatele hello najít tooquestions odpovědi, například:

- Co je hello přihlášení vzor uživatele?
- Kolik uživatelů se přihlásilo za týden?
- Co je hello stav těchto přihlášení?



Vstupní bod toothis dat je hello uživatele přihlásit grafu v hello **přehled** oddílu pod **uživatelů a skupin**.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/45.png "Aktivita přihlašování")

Hello uživatele přihlásit graf znázorňuje týdenní agregací přihlašovací Plugin pro všechny uživatele v daném časovém období. Výchozí hodnota Hello hello časové období je 30 dní.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/46.png "Aktivita přihlašování")

Když kliknete na den v hello přihlášení grafu, zobrazí podrobný seznam hello přihlašovací aktivity pro tento den.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/41.png "Aktivita přihlašování")

Každý řádek v hello přihlašovací aktivity nabízí seznamu hello podrobné informace o přihlášení hello vybraný jako:

* Kdo se přihlásil?
* Jaká byla hello související UPN?
* Jaké aplikace se hello cíl hello přihlásit?
* Co je hello IP adresu hello přihlásit?
* Jaká byla hello stav hello přihlásit?

Hello **přihlášení** možnost poskytuje úplný přehled všech přihlášení uživatele.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/51.png "Aktivita přihlašování")



## <a name="usage-of-managed-applications"></a>Použití spravovaných aplikací

S použitím zobrazení dat přihlašování zaměřeného na aplikace můžete odpovídat na otázky tohoto typu:

* Kdo používá mé aplikace?
* Jaké jsou hlavní 3 aplikace hello ve vaší organizaci?
* Nedávno jsem zpřístupnil aplikaci. Jak to s ní vypadá?

Vstupní bod toothis dat je hello hlavních 3 aplikací ve vaší organizaci v rámci hello posledních 30 dní sestavy v hello **přehled** oddílu pod **podnikové aplikace, které**.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/64.png "Aktivita přihlašování")

Hello aplikace využití grafu týdenní agregací přihlášení pro 3 hlavních aplikací v daném časovém období. Výchozí hodnota Hello hello časové období je 30 dní.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/47.png "Aktivita přihlašování")

Pokud chcete, můžete nastavit hello zaměřit na konkrétní aplikaci.


![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Vytváření sestav")

Když kliknete na den v grafu využití aplikace hello, zobrazí podrobný seznam hello přihlašovací aktivity.


![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/48.png "Aktivita přihlašování")


Hello **přihlášení** možnost poskytuje úplný přehled všech aplikací tooyour události přihlášení.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/49.png "Aktivita přihlašování")



## <a name="next-steps"></a>Další kroky

Pokud chcete více informací o kódy chyb přihlašovací aktivita tooknow, najdete v části hello [přihlášení kódy chyb v portálu Azure Active Directory hello aktivity sestavy](active-directory-reporting-activity-sign-ins-errors.md).

