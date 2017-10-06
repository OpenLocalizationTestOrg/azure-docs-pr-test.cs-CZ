---
title: "Aktivita aaaFind sestavy v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toofind aktivita služby Azure Active Directory sestavy v hello portálu Azure."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f8d7a968403e10ccc5319f27fedad38b1553ded0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-activity-reports-in-hello-azure-portal"></a>Najít sestavy aktivit v hello portálu Azure

Pokud přecházíte z hello Azure classic portálu toohello portálu Azure, můžete získat nový pohled na protokoly aktivity Azure Active Directory (Azure AD). V poslední [příspěvku na blogu](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), vám vysvětlíme, jak vidíte aktivitu protokolů v kontextu hello hello prostředku pracujete v hello portálu Azure. V tomto článku jsme popisují, jak toofind sestavy, který jste použili v hello portál Azure classic hello portálu Azure.

## <a name="whats-new"></a>Co je nového

Sestavy v hello portál Azure classic jsou rozdělené do kategorií:

1.  Sestavy zabezpečení
2.  Sestavy aktivit
3.  Sestavy integrované aplikace

### <a name="activity-and-integrated-app-reports"></a>Aktivity a integrované aplikace sestavy

Na základě kontextu vytváření sestav v hello portálu Azure, existující sestavy jsou sloučeny do jednoho zobrazení. Jeden, základní API poskytuje zobrazení toohello dat hello.

Toto zobrazení na hello toosee **Azure Active Directory** okno, v části **aktivity**, vyberte **protokoly auditu**.

![Protokoly auditu](./media/active-directory-reporting-migration/482.png "Protokoly auditu")

v tomto zobrazení jsou konsolidovány Hello následující sestavy:

-   Sestava auditu
-   Aktivity resetování hesla
-   Registrace aktivita resetování hesla
-   Aktivita samoobslužných skupin
-   Změny názvu skupiny Office 365
-   Účet zřizování aktivity
-   Stav Změna hesla
-   Chyby zřizování účtů


Hello sestav využití aplikace je vylepšený a je součástí hello **přihlášení** zobrazení. Toto zobrazení na hello toosee **Azure Active Directory** okno, v části **aktivity**, vyberte **přihlášení**.

![Zobrazení přihlášení](./media/active-directory-reporting-migration/483.png "zobrazení přihlášení")

Hello **přihlášení** zobrazení zahrnuje všechny přihlášení uživatele. Můžete použít informace o použití této informace tooget aplikaci. Také můžete zobrazit informace o použití aplikace v hello **podnikové aplikace, které** přehled v hello **SPRAVOVAT** části.

![Podnikové aplikace, které](./media/active-directory-reporting-migration/484.png "podnikové aplikace")

## <a name="access-a-specific-report"></a>Přístup k určité sestavy

I když hello portál Azure nabízí jediné zobrazení, také můžete si prohlédnout konkrétní sestavy.

### <a name="audit-logs"></a>Protokoly auditu

V odpovědi toocustomer zpětnou vazbu v hello portál Azure, můžete použít rozšířené filtrování tooaccess hello dat, které chcete. Je jeden filtr, můžete použít *kategorie aktivity*, který uvádí různé typy aktivit hello protokoly ve službě Azure AD. toonarrow výsledků toowhat, které hledáte, můžete vybrat kategorii.

Například pokud vás zajímá jenom v resetování hesla související služby tooself aktivity, můžete zvolit hello **Samoobslužná správa hesel** kategorie. Hello kategorií, které vidíte jsou založené na hello prostředků, které pracují v.  

![Kategorie možnosti na stránce protokoly auditu filtru hello](./media/active-directory-reporting-migration/06.png "kategorie možnosti na stránce protokoly auditu filtru hello")

Kategorie aktivit patří:

- Základní adresář
- Samoobslužná správa hesel
- Samoobslužná správa skupin
- Zřizování účtů

### <a name="application-usage"></a>Využití aplikací

tooview podrobnosti o použití aplikací pro všechny aplikace nebo pro jenom jedna aplikace, v části **aktivity**, vyberte **přihlášení**. toonarrow hello výsledky můžete filtrovat podle uživatelské jméno nebo název aplikace.

![Stránka filtru událostí přihlášení](./media/active-directory-reporting-migration/07.png "události přihlášení filtru stránky")

### <a name="security-reports"></a>Sestavy zabezpečení

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD neobvyklé aktivity sestavy

Zabezpečení Azure AD neobvyklé aktivity sestavy z portálu Azure classic hello byly konsolidované tooprovide můžete pomocí nich centrální zobrazení. Toto zobrazení uvádí všechny události související se zabezpečením riziko, že Azure AD můžete zjišťovat a tvorba sestav o.

Hello následující tabulka uvádí sestavy zabezpečení neobvyklé aktivity hello Azure AD a odpovídající typy událostí riziko v hello portálu Azure.

| Sestava neobvyklé aktivity Azure AD |  Typ události riziko ochranu identity|
| :--- | :--- |
| Uživatelé s uniklými přihlašovacími údaji | Uniklé přihlašovací údaje |
| Nestandardní přihlašovací aktivita | Neuskutečnitelná cesta tooatypical umístění |
| Přihlášení z možných nakažených zařízení | Přihlášení z nakažených zařízení|
| Přihlášení z neznámých zdrojů | Přihlášení z anonymních IP adres |
| Přihlášení z IP adres s podezřelou aktivitou | Přihlášení z IP adres s podezřelou aktivitou |
| - | Přihlášení z neznámých míst |

Hello následující zabezpečení Azure AD neobvyklé aktivity sestavy nejsou zahrnuta jako riziko události v hello portálu Azure:

* Přihlášení po několika neúspěších
* Přihlášení z více geografických poloh

Tyto sestavy jsou stále k dispozici v hello portál Azure classic, ale přestanou někdy v budoucnu hello.

Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).  


#### <a name="detected-risk-events"></a>Zjištěné rizikových událostí

V hello portálu Azure, můžete přistupovat k sestavám o zjištěných rizikových událostí na hello **Azure Active Directory** okno, v části **zabezpečení**. Zjištěné rizikových událostí jsou sledovány v hello následující sestavy:   

- Ohrožených uživatelích
- Riziková přihlášení

![Sestavy zabezpečení](./media/active-directory-reporting-migration/04.png "sestavy zabezpečení")

Další informace o zabezpečení najdete v tématu:

- [Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory hello](active-directory-reporting-security-user-at-risk.md)
- [Rizikové přihlášení sestavy v portálu Azure Active Directory hello](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-hello-azure-classic-portal-vs-hello-azure-portal"></a>Sestavy aktivit v hello portál Azure classic oproti hello portálu Azure

Tabulka Hello v této části uvádí existující sestavy v hello portál Azure classic. Také popisuje, jak můžete získat hello stejné informace v hello portálu Azure.

všechny tooview auditování dat na hello **Azure Active Directory** okno, v části **aktivity**, přejděte příliš**protokoly auditu**.

![Protokoly auditu](./media/active-directory-reporting-migration/61.png "Protokoly auditu")

| klasický portál Azure                 | toofind v hello portálu Azure                                                         |
| ---                                  | ---                                                                        |
| Protokoly auditu                           | Pro **kategorie aktivity**, vyberte **základní Directory**.                       |
| Aktivity resetování hesla              | Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**. |
| Registrace aktivita resetování hesla | Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.     |
| Aktivita samoobslužných skupin         | Pro **kategorie aktivity**, vyberte **Samoobslužná správa skupiny**.        |
| Účet zřizování aktivity        | Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.         |
| Stav Změna hesla             | Pro **kategorie aktivity**, vyberte **Automatická změna hesla aplikace**.      |
| Chyby zřizování účtů          | Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.        |
| Změny názvu skupiny Office 365         | Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**. Pro **typ prostředku aktivity**, vyberte **skupiny**. Pro **zdroj aktivity**, vyberte **O365 skupiny**.|

tooview hello **využití aplikace** zprávu o hello **Azure Active Directory** okno, v části **SPRAVOVAT**, vyberte **podnikové aplikace, které**a potom vyberte **přihlášení**.


![Sestava podnikové aplikace přihlášení](./media/active-directory-reporting-migration/199.png "podnikové aplikace přihlášení sestavy")

## <a name="next-steps"></a>Další kroky

Přehled vytváření sestav najdete v tématu hello [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).
