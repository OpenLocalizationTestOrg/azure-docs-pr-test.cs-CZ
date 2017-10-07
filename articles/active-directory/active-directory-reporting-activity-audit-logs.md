---
title: "Aktivita aaaAudit sestavy v portálu Azure Active Directory hello | Microsoft Docs"
description: "Úvod toohello audit sestavy aktivit v portálu Azure Active Directory hello"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 1567673f5030fc707b017c069f2ba7587962e5cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="audit-activity-reports-in-hello-azure-active-directory-portal"></a>Audit aktivity sestav na portálu Azure Active Directory hello 

S vytvářením sestav v Azure Active Directory (Azure AD), můžete získat hello informace, které potřebujete toodetermine úspěšnost prostředí.

Hello reporting architektura ve službě Azure AD se skládá z hello následující součásti:

- **Aktivita** 
    - **Přihlašovací aktivity** – informace o využití hello spravovaných aplikací a aktivit přihlášení uživatelů
    - **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů
- **Zabezpečení** 
    - **Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu. Další podrobnosti najdete v tématu Riziková přihlášení.
    - **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.

Toto téma poskytuje přehled hello audit aktivity.
 
## <a name="who-can-access-hello-data"></a>Kdo může přistupovat k datům hello?
* Uživatelé v roli správce zabezpečení nebo zabezpečení čtečky hello
* Globální správci
* Jednotliví uživatelé (bez oprávnění správce) mohou zobrazit své vlastní aktivity.


## <a name="audit-logs"></a>Protokoly auditu

protokoly auditu Hello v Azure Active Directory zadejte záznamů aktivit systému pro dodržování předpisů.  
Je vaší první tooall bodu položka auditování dat **protokoly auditu** v hello **aktivity** části **Azure Active Directory**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/61.png "Protokoly auditu")

Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- Hello datum a čas výskytu hello
- Hello iniciátor nebo objektu actor (*kdo*) aktivity 
- Hello aktivity (*co*) 
- cíl Hello

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/18.png "Protokoly auditu")

Kliknutím můžete přizpůsobit zobrazení seznamu hello **sloupce** v panelu nástrojů hello.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/19.png "Protokoly auditu")

To vám umožní toodisplay další pole nebo odeberte pole, které jsou již zobrazen.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/21.png "Protokoly auditu")


Kliknutím na položku v zobrazení seznamu hello zobrazí všechny dostupné podrobné informace o něm.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/22.png "Protokoly auditu")


## <a name="filtering-audit-logs"></a>Filtrování protokolů auditu

toonarrow dolů hello nahlášené tooa dat na úrovni, funguje pro vás, můžete filtrovat data auditu hello pomocí hello následující pole:

- Rozsah dat
- Spustil(a) (činitel)
- Kategorie
- Typ prostředku aktivity
- Aktivita

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/23.png "Protokoly auditu")


Hello **rozsah dat** filtr povoluje tooyou toodefine časový rámec pro hello vrátil data.  
Možné hodnoty:

- 1 měsíc
- 7 dní
- 24 hodin
- Vlastní

Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.

Hello **iniciovaná** filtr umožňuje vám toodefine objektu actor název nebo jeho universal hlavní název (UPN).

Hello **kategorie** filtru vám umožní tooselect jeden hello následující filtru:

- Všechny
- Základní kategorie
- Základní adresář
- Samoobslužná správa hesel
- Samoobslužná správa skupin
- Zřizování účtů – automatická změna hesel
- Pozvaní uživatelé
- Služba MIM
- Identity Protection
- B2C

Hello **typ prostředku aktivity** filtru vám umožní tooselect jedna z následujících hello filtry:

- Všechny 
- Skupina
- Adresář
- Uživatel
- Aplikace
- Zásada
- Zařízení
- Ostatní

Když vyberete **skupiny** jako **typ prostředku aktivity**, dojde další filtr kategorii, která vám umožní tooalso poskytují **zdroj**:

- Azure AD
- O365


Hello **aktivity** filtru je založené na kategorii hello a výběr typu prostředku aktivity provedete. Můžete vybrat konkrétní aktivity mají toosee nebo zvolit vše. 

Můžete získat hello seznam všech aktivit auditu pomocí hello rozhraní Graph API https://graph.windows.net/$ tenantdomain/aktivity nebo auditActivityTypes? api-version = beta, kde $tenantdomain = název domény nebo najdete v článku toohello [audit sestavy události](active-directory-reporting-audit-events.md).


## <a name="audit-logs-shortcuts"></a>Zástupci pro protokoly auditu

Kromě toho příliš**Azure Active Directory**, hello portál Azure poskytuje dva další vstupní body tooaudit dat:

- Uživatelé a skupiny
- Podnikové aplikace

### <a name="users-and-groups-audit-logs"></a>Protokoly auditu uživatelů a skupin

Uživatele a sestavy na základě skupiny auditu můžete získat odpovědi tooquestions jako:

- Jaké typy aktualizací byly použité hello uživatelů?

- Kolik uživatelů bylo změněno?

- Kolik hesel bylo změněno?

- Co provedl správce v adresáři?

- Co jsou hello skupiny, které byly přidány?

- Došlo u některých skupin ke změnám členství?

- Byl změněn hello vlastníků skupiny?

- Jaké licence byly přiřazeny tooa skupinu nebo uživatele?

Pokud chcete tooreview auditování data, která je související toousers a skupiny, můžete najít v části filtrované zobrazení **protokoly auditu** v hello **aktivity** části hello **uživatelů a skupin**. Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Uživatelé a skupiny**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/93.png "Protokoly auditu")

### <a name="enterprise-applications-audit-logs"></a>Protokoly auditu podnikových aplikací

S založené na aplikaci audit sestavy, můžete získat odpovědi tooquestions jako:

* Jaké jsou hello aplikace, které byl přidán nebo aktualizován?
* Jaké jsou hello aplikace, které byly odebrány?
* Změnil se instanční objekt pro aplikaci?
* Byl změněn hello názvy aplikací?
* Kdo jste dali souhlas tooan aplikace?

Pokud chcete tooreview auditování data, která jsou aplikace související tooyour, můžete najít filtrované zobrazení pod **protokoly auditu** v hello **aktivity** části hello **podnikové aplikace**  okno. Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Podniková aplikace**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/134.png "Protokoly auditu")

Můžete filtrovat toto zobrazení další dolů toojust **skupiny** nebo právě **uživatelé**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/25.png "Protokoly auditu")


## <a name="next-steps"></a>Další kroky

Přehled vytváření sestav najdete v tématu hello [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).

