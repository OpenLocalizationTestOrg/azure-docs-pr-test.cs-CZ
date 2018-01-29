---
title: "Najít sestavy aktivity uživatele Azure Active Directory na portálu Azure | Microsoft Docs"
description: "Zjistěte, kde jsou sestavy aktivity uživatele Azure Active Directory na portálu Azure."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/06/2017
ms.author: curtand
ms.reviewer: dhanyahk
ms.openlocfilehash: 23c186e268e9a43982ec6c34d350900793fad8de
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Najít sestavy aktivity na portálu Azure

V tomto článku jsme popisují, jak najít sestavy aktivity uživatele Azure Active Directory na portálu Azure.

## <a name="whats-new"></a>Co je nového

Sestavy v portálu Azure classic byly rozdělené do kategorií:
* Sestavy zabezpečení
* Sestavy aktivit
* Sestavy integrované aplikace

### <a name="activity-and-integrated-app-reports"></a>Aktivity a integrované aplikace sestavy

Na základě kontextu vytváření sestav v portálu Azure, existující sestavy jsou sloučeny do jednoho zobrazení. Jedinou základní rozhraní API poskytuje data k zobrazení.

V tomto zobrazení **Azure Active Directory** okno, v části **aktivity**, vyberte **protokoly auditu**.

![Protokoly auditu](./media/active-directory-reporting-migration/482.png "Protokoly auditu")

V tomto zobrazení jsou konsolidovány následující sestavy:

* Sestava auditu
* Aktivity resetování hesla
* Registrace aktivita resetování hesla
* Aktivita samoobslužných skupin
* Změny názvu skupiny Office 365
* Účet zřizování aktivity
* Stav Změna hesla
* Chyby zřizování účtů


Sestava využití aplikace je vylepšený a je součástí **přihlášení** zobrazení. V tomto zobrazení **Azure Active Directory** okno, v části **aktivity**, vyberte **přihlášení**.

![Zobrazení přihlášení](./media/active-directory-reporting-migration/483.png "zobrazení přihlášení")

**Přihlášení** zobrazení zahrnuje všechny přihlášení uživatele. Tyto informace můžete získat informace o využití aplikace. Také můžete zobrazit informace o použití aplikace v **podnikové aplikace, které** přehled v **SPRAVOVAT** části.

![Podnikové aplikace, které](./media/active-directory-reporting-migration/484.png "podnikové aplikace")

## <a name="access-a-specific-report"></a>Přístup k určité sestavy

I když na portál Azure nabízí jediné zobrazení, také můžete si prohlédnout konkrétní sestavy.

### <a name="audit-logs"></a>Protokoly auditu

V reakci na názory zákazníků na portálu Azure můžete upřesnit filtrování pro přístup k datům, které chcete. Je jeden filtr, můžete použít *kategorie aktivity*, který uvádí různé typy aktivit protokoly ve službě Azure AD. Chcete-li zúžit výsledky co hledáte, můžete vybrat kategorii.

Například pokud vás zajímá jenom v činnosti týkající se resetování hesla pomocí samoobslužné služby, můžete **Samoobslužná správa hesel** kategorie. Kategorie, které vidíte jsou založené na prostředek, který pracujete v.  

![Kategorie možnosti na stránce protokoly auditu filtru](./media/active-directory-reporting-migration/06.png "kategorie možnosti na stránce protokoly auditu filtru")

Kategorie aktivit patří:

- Základní adresář
- Samoobslužná správa hesel
- Samoobslužná správa skupin
- Zřizování účtů

### <a name="application-usage"></a>Využití aplikací

Chcete-li zobrazit podrobnosti o použití aplikací pro všechny aplikace nebo pro jenom jedna aplikace, v části **aktivity**, vyberte **přihlášení**. Chcete-li zúžit výsledky, můžete filtrovat podle uživatelské jméno nebo název aplikace.

![Stránka filtru událostí přihlášení](./media/active-directory-reporting-migration/07.png "události přihlášení filtru stránky")

### <a name="security-reports"></a>Sestavy zabezpečení

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD neobvyklé aktivity sestavy

Zabezpečení Azure AD neobvyklé aktivity konsolidovaný sestavy z portálu Azure classic, kde přinášejí jedno, centrální zobrazení. Toto zobrazení uvádí všechny události související se zabezpečením riziko, že Azure AD můžete zjišťovat a tvorba sestav o.

Následující tabulka seznamy Azure AD neobvyklé aktivity zabezpečení sestavy a odpovídající typy událostí riziko na portálu Azure.

| Sestava neobvyklé aktivity Azure AD |  Typ události riziko ochranu identity|
| :--- | :--- |
| Uživatelé s uniklé přihlašovací údaje | Uniklé přihlašovací údaje |
| Nestandardní přihlašovací aktivita | Nemožná cesta do netypických míst |
| Přihlášení z možných nakažených zařízení | Přihlášení z nakažených zařízení|
| Přihlášení z neznámých zdrojů | Přihlášení z anonymních IP adres |
| Přihlášení z IP adres s podezřelou aktivitou | Přihlášení z IP adres s podezřelou aktivitou |
| - | Přihlášení z neznámých míst |

Následující zabezpečení Azure AD neobvyklé aktivity sestavy nejsou zahrnuta jako riziko události na portálu Azure:

* Přihlášení po několika neúspěších
* Přihlášení z více geografických poloh

Tyto sestavy jsou stále k dispozici na portálu Azure classic, ale přestanou někdy v budoucnu.

Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).  


#### <a name="detected-risk-events"></a>Zjištěné rizikových událostí

Na portálu Azure, je k dispozici sestavy o zjištěných rizikových událostí na **Azure Active Directory** okno, v části **zabezpečení**. Zjištěné rizikových událostí jsou sledovány v následující sestavy:   

- Ohrožených uživatelích
- Riziková přihlášení

![Sestavy zabezpečení](./media/active-directory-reporting-migration/04.png "sestavy zabezpečení")

Další informace o zabezpečení najdete v tématu:

- [Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory](active-directory-reporting-security-user-at-risk.md)
- [Rizikové přihlášení sestav na portálu Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal"></a>Sestavy aktivit v portálu Azure classic oproti portálu Azure

V tabulce v této části jsou uvedeny stávajících sestav na portálu Azure classic. Také popisuje, jak stejné informace můžete získat na portálu Azure.

Pokud chcete zobrazit všechna data auditu, na **Azure Active Directory** okno, v části **aktivity**, přejděte na **protokoly auditu**.

![Protokoly auditu](./media/active-directory-reporting-migration/61.png "Protokoly auditu")

| klasický portál Azure                 | Najít na portálu Azure                                                         |
| ---                                  | ---                                                                        |
| Protokoly auditu                           | Pro **kategorie aktivity**, vyberte **základní Directory**.                       |
| Aktivity resetování hesla              | Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**. |
| Registrace aktivita resetování hesla | Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.     |
| Aktivita samoobslužných skupin         | Pro **kategorie aktivity**, vyberte **Samoobslužná správa skupiny**.        |
| Účet zřizování aktivity        | Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.         |
| Stav Změna hesla             | Pro **kategorie aktivity**, vyberte **Automatická změna hesla aplikace**.      |
| Chyby zřizování účtů          | Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.        |
| Změny názvu skupiny Office 365         | Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**. Pro **typ prostředku aktivity**, vyberte **skupiny**. Pro **zdroj aktivity**, vyberte **O365 skupiny**.|

Chcete-li zobrazit **využití aplikace** sestavy na **Azure Active Directory** okno v části **SPRAVOVAT**, vyberte **podnikové aplikace, které**a potom vyberte **přihlášení**.


![Sestava podnikové aplikace přihlášení](./media/active-directory-reporting-migration/199.png "podnikové aplikace přihlášení sestavy")

## <a name="next-steps"></a>Další kroky

Přehled generování sestav najdete v tématu [Generování sestav v Azure Active Directory](active-directory-reporting-azure-portal.md).
