---
title: "kódy chyb aktivitu aaaSign sestav na portálu Azure Active Directory hello | Microsoft Docs"
description: "Referenční informace ke kódům chyb v sestavě aktivit přihlašování."
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
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: a0ca5b706bfeb0c7ce669712468a083a394712b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-report-error-codes-in-hello-azure-active-directory-portal"></a>Kódy chyb sestavy přihlašovací aktivita v portálu Azure Active Directory hello

S hello informací uvedených v sestavě přihlášení uživatele hello najít tooquestions odpovědi, například:

- Kdo se přihlásil pomocí Azure Active Directory?
- Ke kterým aplikacím se někdo přihlásil?
- Která přihlášení selhala a proč?

Tato chyba zobrazí hello tématu kódy a hello související popisy. 

## <a name="how-can-i-display-failed-sign-ins"></a>Jak se dají zobrazit přihlášení, která selhala? 

Vaše první vstupní bod tooall přihlašovací aktivity data  **[přihlášení](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)**  v hello **aktivity** části **Azure Active**.


![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Aktivita přihlašování")


V sestavě přihlášení můžete zobrazit všechna přihlášení, která selhala, tím, že jako **Stav přihlášení** vyberete **Selhání**.


![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Aktivita přihlašování")

Kliknutím na položku v seznamu zobrazí text hello, otevře hello **podrobnosti o aktivitě: přihlášení** okno. Toto zobrazení vám poskytne všechny hello podrobnosti, které sleduje Azure Active Directory o přihlášení, včetně hello **kód chyby přihlášení** a **důvod selhání**.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Aktivita přihlašování")


Jako hello alternativní toousing Azure portálu tooaccess hello přihlášení data, můžete použít také hello [reporting rozhraní API](active-directory-reporting-api-getting-started-azure-portal.md).


Hello následující část vám poskytne úplný přehled všechny možné chyby a hello související popisy. 

## <a name="error-codes"></a>Kódy chyb

| Chyba| Popis |
| --- | --- |
| 50001| Hello instanční objekt s názvem X nebyl nalezen v hello klienta s názvem Y. To může nastat, když aplikace hello není nainstalován správce hello hello klienta. Objekt zabezpečení prostředků nebyla nalezena v adresáři hello nebo je neplatný nebo|
| 50008| Kontrolního výrazu SAML jsou chybějící nebo nesprávně nakonfigurované v tokenu hello.|
| 50011| Adresa pro odpověď Hello chybí, nesprávné nebo neodpovídá odpovědi adresy nakonfigurované pro aplikace hello.|
| 50053| Účet je uzamčen, protože uživatel se pokusil toosign v příliš mnohokrát pomocí nesprávného ID uživatele nebo hesla.|
| 50054| K ověřování se používá staré heslo.|
| 50055| Neplatné heslo, zadáno prošlé heslo.|
| 50057| Uživatelský účet je neaktivní.|
| 50058| Žádné informace o identitě uživatele nachází mezi zadané přihlašovací údaje nebo uživatel nebyl nalezen v klientovi nebo byla odeslána žádost tichou přihlášení, ale je přihlášený žádný uživatel nebo služby nelze tooauthenticate hello uživatele.|
| 50074| Vyžaduje se silné ověřování (druhý faktor).|
| 50079| Uživatel potřebuje tooenroll pro druhý faktor ověřování|
| 50126| Neplatné uživatelské jméno nebo heslo nebo Neplatné místní uživatelské jméno nebo heslo.|
| 50131| Používá se při různých chybách podmíněného přístupu. Zařízení se systémem Windows chybný např stavu žádosti blokovaný z důvodu toosuspicious aktivity, zásady přístupu a zásady zabezpečení rozhodnutí.|
| 50133| Relace je neplatná kvůli tooexpiration nebo poslední změny hesla.|
| 50144| Vypršela platnost hesla uživatele pro Active Directory.|
| 65001| X aplikace nemá oprávnění tooaccess aplikace Y nebo hello oprávnění je odvolaný. Nebo hello uživatel nebo správce nedala souhlas aplikace hello toouse s ID X. odeslat požadavek interaktivní autorizace pro tohoto uživatele a prostředků. Nebo hello uživatel nebo správce nedala souhlas toouse hello aplikace s ID X. odesílání tooact správce klienta autorizace požadavku tooyour jménem hello aplikace: Y pro prostředek: Z.|
| 65005| Hello aplikace vyžaduje přístup k seznamu prostředků neobsahuje aplikace zjistitelný prostředkem hello nebo hello klientská aplikace vyžaduje tooresource přístup, který nebyl zadán v jeho přístup k seznamu požadovaný prostředek nebo grafu služba vrátila chybný požadavek nebo prostředek nebyl nalezen.|
| 70001| Hello aplikaci s názvem X nebyl nalezen v hello klienta s názvem Y. To může dojít pokud není nainstalován hello aplikace hello správce klienta hello nebo svolení tooby všechny uživatele v klientovi hello. Možná jste odeslali klienta nesprávný toohello požadavek ověřování.|
| 80001| Není k dispozici žádný ověřovací agent.|
| 80002| Vypršel časový limit žádosti o ověření hesla ověřovacího agenta.|
| 80003| Ověřovací agent přijal neplatnou odpověď.|
| 80004| V žádosti o přihlášení byl použit nesprávný hlavní název uživatele (UPN).|
| 80005| Ověřovací agent: Došlo k chybě.|
| 80007| Ověřování agenta nelze tooconnect tooActive adresáře.|
| 80010| Heslo nelze toodecrypt agenta pro ověřování.|
| 81001| Lístek Kerberos uživatele je příliš velký.|
| 81002| Uživatele nelze toovalidate lístek protokolu Kerberos.|
| 81003| Uživatele nelze toovalidate lístek protokolu Kerberos.|
| 81004| Pokus o ověření protokolu Kerberos selhal.|
| 81008| Uživatele nelze toovalidate lístek protokolu Kerberos.|
| 81009| Uživatele nelze toovalidate lístek protokolu Kerberos.|
| 81010| Bezproblémové jednotné přihlašování se nezdařilo, protože lístek protokolu Kerberos hello uživatele vypršela nebo je neplatný.|
| 81011| Objekt nelze toofind uživatele na základě informací v lístku protokolu Kerberos hello uživatele.|
| 81012| Hello uživatele při toosign v tooAzure AD se liší od uživatele hello přihlásili hello zařízení.|
| 81013| Objekt nelze toofind uživatele na základě informací v lístku protokolu Kerberos hello uživatele.|
| 90014| Použít v různých případech, pokud očekávaný pole není přítomen v hello přihlašovacích údajů.|
| 90093| Graf vrátil s kódem chyby zakázané pro požadavek hello.|



## <a name="next-steps"></a>Další kroky

Další podrobnosti najdete v tématu hello [přihlášení sestavy aktivit v portálu Azure Active Directory hello](active-directory-reporting-activity-sign-ins.md).

