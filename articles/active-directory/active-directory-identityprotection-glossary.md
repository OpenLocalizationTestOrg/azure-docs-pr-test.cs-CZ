---
title: "aaaAzure slovník ochrany Identity služby Active Directory | Microsoft Docs"
description: "Slovník ochrany identit Azure Active Directory"
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení, Glosář"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ff2e96d20e2a3f1df24b78e66be5a0c6807e60a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Slovník ochrany identit Azure Active Directory
### <a name="at-risk-user"></a>Riziko (uživatel)
Uživatel se jeden nebo více událostí active riziko. 

### <a name="atypical-sign-in-location"></a>Umístění netypických přihlášení
Přihlášení z zeměpisné polohy, který není typické pro konkrétního uživatele hello, podobně jako uživatele nebo hello klienta.

### <a name="azure-ad-identity-protection"></a>Azure AD Identity Protection
Modul zabezpečení služby Azure Active Directory, která poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity organizace.

### <a name="conditional-access"></a>Podmíněný přístup
Zásady pro zabezpečení tooresources přístup. Pravidla podmíněného přístupu jsou uložené v hello Azure Active Directory a vyhodnocují se službou Azure Active Directory před udělením přístupu toohello prostředků.  Příklad pravidla zahrnují také omezení přístupu podle umístění uživatele, metodu ověřování stavu nebo uživatel zařízení.

### <a name="credentials"></a>Přihlašovací údaje
Informace, které zahrnují identifikace a ověření identifikaci, který je použité toogain přístup toolocal a síťových prostředků. Příkladem přihlašovací údaje jsou uživatelská jména a hesla, čipové karty a certifikáty.

### <a name="event"></a>Událost
Záznam aktivity v Azure Active Directory.

### <a name="false-positive-risk-event"></a>Falešně pozitivní (riziko událost)
Stav události riziko podle ochrany identit uživatelů, indikující, že událost riziko hello byl prozkoumat a byla nesprávně označený jako událost riziko nastavit ručně.

### <a name="identity"></a>Identita
Osoba nebo entita, které se musí ověřit prostřednictvím ověřování, na základě kritérií, například heslo nebo certifikát.

### <a name="identity-risk-event"></a>Identity riziko událostí
AAD událost, která byla označena jako neobvyklé službou Identity Protection a může znamenat, že byl napaden identity.

### <a name="ignored-risk-event"></a>Ignorovat (riziko událost)
Stav události riziko podle ochrany identit uživatelů, což značí, že tuto událost riziko hello je uzavřený bez nutnosti převádět akci automatické nápravy nastavit ručně.

### <a name="impossible-travel-from-atypical-locations"></a>Neuskutečnitelná cesta z netypických míst
Událost související s riziko aktivované, když se dvě přihlášení pro hello stejného uživatele jsou zjištěny, kde alespoň jeden z nich je z netypických umístění přihlášení a tam, kde je kratší než minimální hello hello čas mezi přihlášení hello čas by trvat toophysically při přenosu mezi tyto umístění.  

### <a name="investigation"></a>Šetření
Hello kontroly hello aktivity, protokoly a další důležité informace související s procesem tooa riziko událostí toodecide zda zmírnění nebo nápravy kroky jsou nezbytné, zjištění, zda a jak hello identity došlo k ohrožení a pochopit, jak hello. byl použit ohroženými identity.

### <a name="leaked-credentials"></a>Uniklé přihlašovací údaje
Ve webové tmavý hello naše výzkumní pracovníci odeslány veřejně riziko událostí, aktivuje, když jsou nalezeny přihlašovací údaje uživatele (uživatelské jméno a heslo).

### <a name="mitigation"></a>Omezení rizik
Toolimit akce nebo odstranit hello možnost útočník tooexploit ohroženými identity nebo zařízení bez hello identity nebo zařízení tooa bezpečné stavu obnovení. Zmírnění nevyřeší předchozí rizikových událostech spojených s hello identity nebo zařízení.

### <a name="multi-factor-authentication"></a>Ověřování pomocí služby Multi-Factor Authentication
Metoda ověřování, která vyžaduje dva nebo více metod ověřování, které mohou zahrnovat něco, co má uživatel hello, tento certifikát; něco hello uživatel zná, například uživatelská jména, hesla či fráze průchodu; fyzické atributy, jako je například thumbprint; a osobní atributy, jako například osobní podpis.

### <a name="offline-detection"></a>Offline detekce
Hello detekce anomálií a vyhodnocení rizik hello události například pokus o přihlášení po hello fakt událost, která již došlo.

### <a name="policy-condition"></a>Stav zásad
Část zásad zabezpečení, který definuje hello entity (skupiny uživatelů, aplikace, platformy zařízení, stavy zařízení, rozsahy IP adres, typů klientů) obsažena v zásadách hello nebo z něj vyloučeny.

### <a name="policy-rule"></a>Pravidlo zásad
Hello část zásad zabezpečení, která popisuje hello okolnosti, které by aktivovat hello zásady a hello akce prováděné při aktivaci hello zásad.

### <a name="prevention"></a>Prevention (Prevence)
Akce tooprevent škody toohello organizace prostřednictvím zneužití identity nebo zařízení by mohly vzbuzovat podezření nebo znát toobe ohrožení zabezpečení. Prevence akce nezabezpečuje hello zařízení nebo identity a nevyřeší předchozí rizikových událostech.

### <a name="privileged-user"></a>Privilegovaný (uživatel)
Uživatel, který v době hello riziko události, jako trvalé nebo dočasné správce oprávnění tooone nebo více prostředků v Azure Active Directory, jako je například globální správce, správce fakturace, Správce služby, správce uživatele a heslo Správce. 

### <a name="real-time"></a>V reálném čase
V tématu detekce v reálném čase.

### <a name="real-time-detection"></a>Detekce v reálném čase
Hello detekci anomálií a vyhodnocení rizik hello události například pokus přihlášení před událostí hello je umožněno tooproceed.

### <a name="remediated-risk-event"></a>Opravené (riziko událost)
Stav události riziko automaticky nastavuje ochrany identit, což značí, že tuto událost riziko hello nápravy pomocí hello standardní nápravné akce pro tento typ události riziko. Například když se resetuje heslo uživatele hello, mnoho událostí riziko, který indikuje, že došlo k ohrožení toto heslo předchozí hello napravují automaticky.

### <a name="remediation"></a>Nápravy
Akci toosecure identity nebo zařízení, které byly dříve by mohly vzbuzovat podezření nebo známé toobe ohrožení. Akce nápravy obnoví hello identity nebo zařízení tooa bezpečné stav a odstraňuje předchozí rizikových událostech spojených s hello identity nebo zařízení.

### <a name="resolved-risk-event"></a>Vyřešit (riziko událost)
Stav událostí riziko podle ochrany identit uživatelů, označující, že uživatel hello trvalo akce odpovídající nápravu mimo ochrany identit a považovat za tuto událost riziko hello nastavit ručně ukončeno.

### <a name="risk-event-status"></a>Riziko stav události.
Vlastnosti události riziko, která udává, zda text hello událostí je aktivní a pokud zavřená, hello důvod pro zavření ho.

### <a name="risk-event-type"></a>Typ události rizik
Kategorie pro hello rizik událost, která určuje typ hello anomálií, která způsobila, že toobe hello událost považována za rizikové.

### <a name="risk-level-risk-event"></a>Úroveň rizika (riziko událost)
Označení (vysoká, střední nebo nízká) hello závažnosti hello riziko událostí toohelp ochrany identit uživatelů prioritu hello akce jejich trvat tooreduce hello riziko tootheir organizace. 

### <a name="risk-level-sign-in"></a>Úroveň rizika (sign-in)
Uvedení (vysoká, střední nebo nízká) hello pravděpodobnost, že pro konkrétní sign-in, někdo jiný pokouší identitu uživatele hello toouse.

### <a name="risk-level-user-compromise"></a>Úroveň rizika (ohrožení zabezpečení uživatele)
Uvedení (vysoká, střední nebo nízká) hello pravděpodobnost, že byl napaden identity.

### <a name="risk-level-vulnerability"></a>Úroveň rizika (chyba)
Označení (vysoká, střední nebo nízká) hello závažnosti hello ohrožení zabezpečení toohelp ochrany identit uživatelů prioritu hello akce jejich trvat tooreduce hello riziko tootheir organizace.

### <a name="secure-identity"></a>Zabezpečení (identity)
Akce nápravy třeba změnit heslo nebo počítač obnovování toorestore tooan neohrožený stavu potenciálně ohroženými identity.

### <a name="security-policy"></a>Zásady zabezpečení
Kolekce pravidel zásad a podmínku. Zásada může být použité tooentities, jako jsou uživatelé, skupiny, aplikace, zařízení, platformy zařízení, stavy zařízení, rozsahy IP adres a Auth2.0 typů klientů. Pokud je povoleno zásadu, vyhodnotí se vždy, když je obsažena v zásadách hello entity vystaví token pro prostředek.

### <a name="sign-in-v"></a>Přihlaste se (v)
Identita tooan tooauthenticate v Azure Active Directory.

### <a name="sign-in-n"></a>Přihlášení (ne)
proces Hello nebo akce ověřování identity v Azure Active Directory a hello událostí, který zachycuje tuto operaci.

### <a name="sign-in-from-anonymous-ip-address"></a>Přihlášení z anonymních IP adresy
Riziko událost se spustí po úspěšného přihlášení z IP adresy, které bylo zjištěno, že IP adresa anonymního proxy serveru.

### <a name="sign-in-from-infected-device"></a>Přihlášení z nakažených zařízení
Událost riziko, aktivuje, když přihlášení pochází z IP adresy, která se označuje toobe používá jedno nebo více ohroženého zařízení, které se pokoušíte aktivně toocommunicate se serverem robota.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Přihlášení z IP adres s podezřelou aktivitou
Riziko události aktivované po úspěšného přihlášení z IP adres s vysoký počet neúspěšných pokusů o přihlášení několika uživatelským účtům během krátké doby času.

### <a name="sign-in-from-unfamiliar-location"></a>Přihlášení z neznámého umístění
Událost riziko, aktivuje, když se uživatel úspěšně přihlásí z nového místa (IP adresy, zeměpisnou šířku a délku a ASN).

### <a name="sign-in-risk"></a>Riziko přihlášení
V tématu riziko úroveň (sign-in)

### <a name="sign-in-risk-policy"></a>Zásady přihlášení rizik
Zásady podmíněného přístupu, která vyhodnotí hello riziko tooa konkrétní přihlášení a použije způsoby zmírnění rizik na základě předem definované podmínky a pravidla.

### <a name="user-compromise-risk"></a>Riziko ohrožení zabezpečení pro uživatele
V tématu riziko úroveň (ohrožení zabezpečení uživatele)

### <a name="user-risk"></a>Riziko pro uživatele
V tématu riziko úroveň (ohrožení zabezpečení uživatele).

### <a name="user-risk-policy"></a>Riziko zásady uživatele
Zásady podmíněného přístupu, který zvažuje hello přihlásit a způsoby zmírnění rizik na základě předem definované podmínky a pravidla se vztahují.

### <a name="users-flagged-for-risk"></a>Uživatelé označení příznakem rizika
Uživatelé, kteří mají rizikových událostech, které jsou aktivní nebo napravených

### <a name="vulnerability"></a>Ohrožení zabezpečení
Konfigurace nebo podmínku v Azure Active Directory, takže je náchylný tooexploits directory hello nebo hrozeb.

## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)

