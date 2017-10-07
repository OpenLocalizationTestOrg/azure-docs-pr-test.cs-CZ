---
title: "Generování sestav Azure Active Directory: Začínáme | Dokumentace Microsoftu"
description: "Seznamy hello různých dostupných sestav generovaných v generování sestav Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: f47875708398391dd7f3efdc56a741fdba273b76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Začínáme s generováním sestav v Azure Active Directory
## <a name="what-it-is"></a>Co to je
Azure Active Directory (Azure AD) obsahuje různé sestavy zabezpečení, aktivit a auditu pro váš adresář. Tady je seznam hello sestavy zahrnuté:

### <a name="security-reports"></a>Sestavy zabezpečení
* Přihlášení z neznámých zdrojů
* Přihlášení po několika neúspěších
* Přihlášení z více geografických poloh
* Přihlášení z IP adres s podezřelou aktivitou
* Nestandardní přihlašovací aktivita
* Přihlášení z možných nakažených zařízení
* Uživatelé s neobvyklou přihlašovací aktivitou

### <a name="activity-reports"></a>Sestavy aktivit
* Využití aplikací: souhrn
* Využití aplikací: podrobnosti
* Řídicí panel aplikací
* Chyby zřizování účtů
* Zařízení jednotlivých uživatelů
* Aktivity jednotlivých uživatelů
* Sestava aktivit skupin
* Sestava aktivit registrace resetování hesla
* Aktivity resetování hesla

### <a name="audit-reports"></a>Sestavy auditu
* Sestava auditu adresáře

> [!TIP]
> Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="how-it-works"></a>Jak to funguje
### <a name="reporting-pipeline"></a>Kanál generování sestav
Hello kanál generování sestav zahrnuje tři hlavní kroky. Pokaždé, když se uživatel přihlásí nebo je provedeno ověření, se stane hello následující:

* Nejprve hello uživatel ověřen (úspěšně nebo neúspěšně) a hello výsledek je uložen do databází služby Azure Active Directory hello.
* V pravidelných intervalech jsou zpracovávána všechna poslední přihlášení. V tom okamžiku naše algoritmy zabezpečení a neobvyklých aktivit vyhledávají a zkouší identifikovat podezřelé aktivity ve všech posledních přihlášeních.
* Po zpracování hello sestavy jsou zapsány, do mezipaměti a zpracuje v hello portál Azure classic.

### <a name="report-generation-times"></a>Časy generování sestav
Z důvodu velkého objemu toohello ověřování a přihlašování, že in zpracovává hello platformě Azure AD hello nejnovější zpracovaná přihlášení se v průměru hodinu stará. Ve výjimečných případech to může trvat až too8 hodin tooprocess hello poslední přihlášení.

Hello nejnovější zpracované přihlášení najdete tak, že prověří textu hello nápovědy hello horní části každé sestavy.

![Pomocný text v hello horní části každé sestavy](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="getting-started"></a>Začínáme
### <a name="sign-into-hello-azure-classic-portal"></a>Přihlaste se k hello portál Azure classic
Nejprve budete potřebovat toosign do hello [portál Azure classic](https://manage.windowsazure.com) jako dodržování předpisů nebo globální správce. Musí být také předplatné služby správce nebo spolusprávce nebo používat hello "přístup tooAzure AD" předplatného Azure.

### <a name="navigate-tooreports"></a>Přejděte tooReports
Sestavy tooview přejděte kartě sestavy toohello hello horní části adresáře.

Pokud je to poprvé prohlížení sestav hello, budete potřebovat dialogové okno tooa tooagree před zobrazením sestavy hello. Toto je tooensure, že je přijatelné pro správce ve vaší organizaci tooview tato data, která lze považovat za soukromé informace v některých zemích.

![Dialogové okno](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Zkoumání jednotlivých sestav
Přejděte na každý shromažďovaných dat toosee hello sestavy a zpracovat hello přihlášení. Můžete najít [seznam všech sestav hello zde](active-directory-reporting-guide.md).

![Všechny sestavy](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-hello-reports-as-csv"></a>Stáhnout hello sestavy jako sdílený svazek clusteru
Každou sestavu lze stáhnout jako soubor CSV (hodnoty oddělené čárkami). Můžete použít tyto soubory v aplikaci Excel, PowerBI nebo programy toofurther analyzovat data analysis třetích stran.

toodownload všechny sestavy jako sdílený svazek clusteru, přejděte toohello sestavy a klikněte na tlačítko "Stáhnout" v dolní části hello.

![Tlačítko Stáhnout](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="next-steps"></a>Další kroky
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Přizpůsobení upozornění na neobvyklé přihlašovací aktivity
Přejděte toohello "Konfigurace" kartě adresáře.

Posuňte se toohello části "Oznámení".

Povolit nebo zakázat část "E-mailová oznámení neobvyklých přihlášení" hello.

![Hello sekce oznámení](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-hello-azure-ad-reporting-api"></a>Integrovat hello rozhraní API pro vytváření sestav Azure AD
V tématu [Začínáme s hello Reporting rozhraní API](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Povolení služby Multi-Factor Authentication pro uživatele
Vyberte uživatele v sestavě.

Klikněte na tlačítko "Povolit MFA" hello v hello dolní části obrazovky hello.

![tlačítko služby Multi-Factor Authentication Hello v hello dolní části obrazovky hello](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="learn-more"></a>Další informace
### <a name="audit-events"></a>Auditování událostí
Další informace o událostech, které jsou v adresáři hello v auditovat [Azure Active Directory Reporting událostí auditu](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>Integrace rozhraní API
V tématu [Začínáme s hello Reporting rozhraní API](active-directory-reporting-api-getting-started.md) a hello [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Kontakt
Pro zpětnou vazbu, pomoc a případné dotazy slouží e-mailová adresa [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com).

> [!TIP]
> Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).
> 
> 

