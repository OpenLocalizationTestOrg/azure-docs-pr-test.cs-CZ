---
title: "Vytváření sestav: Azure AD SSPR | Microsoft Docs"
description: "Zprávy o hesla pomocí samoobslužné služby Azure AD resetovat události"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: af65b9be1e00c2819431694a5b0064b97b9e1867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Možnosti zasílání zpráv o správou hesel Azure AD

Po nasazení mnoho organizací má tooknow jak nebo pokud je SSPR doopravdy používá. Azure AD poskytuje vytváření sestav funkce, které vám pomohou tooanswer dotazy pomocí konzervované sestavy a pokud jsou správně licencován, umožňují toocreate vlastní dotazy.

Hello tyto otázky, které mohou být přijata následujícími sestavami, které existují v hello [portál Azure] (https://portal.azure.com/).

> [!NOTE]
> Musí být [globální správce](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) a musí výslovný souhlas pro shromáždění dat toobe jménem vaší organizace pomocí hostujícími hello reporting kartě nebo kontrola protokolech alespoň jednou. Až to uděláte, nebudou shromažďovány dat pro vaši organizaci

* Kolik lidí registrovali pro resetování hesla?
* Kdo má zaregistrovat pro resetování hesla?
* Jaká data jsou osoby registraci?
* Kolik uživatelů resetovat vlastní hesla v hello posledních sedmi dnů?
* Co jsou hello nejběžnější metody uživatelé nebo správci používají tooreset hesla?
* Uživatelé nebo správci vzhled co jsou běžné problémy, při pokusu o resetování hesla toouse?
* Co správci jsou často resetovat vlastní hesla?
* Je k dispozici podezřelé aktivity přejdete k resetování hesla?

## <a name="how-tooview-password-management-reports-in-hello-azure-portal"></a>Jak Správa hesel tooview sestavy v hello portálu Azure

V práci s portálem Azure hello máme lepší způsob tooview resetování a heslo aktivita resetování hesla registrace.  Postupujte podle kroků hello toofind hello heslo resetovat heslo resetovat registrace události a:

1. Přejděte příliš[**portal.azure.com**](https://portal.azure.com)
2. Klikněte na tlačítko hello **další služby** na hello hlavní Azure portálu levé navigační nabídky
3. Vyhledejte **Azure Active Directory** v hello seznam služeb a vyberte ho
4. Klikněte na tlačítko **uživatelé a skupiny** navigační nabídce hello Azure Active Directory
5. Klikněte na tlačítko hello **protokoly auditu** navigační položka navigační nabídce hello uživatelé a skupiny. Toto jsou zobrazeny všechny událostí auditu hello proti všichni uživatelé hello ve vašem adresáři. Toto zobrazení toosee můžete filtrovat všechny hello související s hesly události, i.
6. toofilter toto zobrazení tooonly hello heslo resetovat související události, klikněte na tlačítko hello **filtru** tlačítko hello horní části okna hello.
7. Z hello **filtru** nabídky, vyberte hello **kategorie** rozevíracího seznamu a změňte ji toohello **Samoobslužná správa hesel** typ kategorie.
8. Volitelně další seznam filtrů hello výběrem konkrétní hello **aktivity** vás zajímá

## <a name="how-tooretrieve-password-management-events-from-hello-azure-ad-reports-and-events-api"></a>Jak tooretrieve heslo správy události z hello sestav Azure AD a události rozhraní API

Sestavy Hello Azure AD a události rozhraní API podporuje načítání všechny hello informace uvedené v sestavách registrace resetování hesla pro obnovení a heslo. Pomocí tohoto rozhraní API si můžete stáhnout resetování hesla jednotlivých a resetování hesla registrace události pro integraci s hello reporting technologie podle svého výběru.

### <a name="how-tooget-started-with-hello-reporting-api"></a>Jak tooget pracovat s hello reporting rozhraní API

tooaccess tato data, třeba toowrite malé aplikace nebo skriptu tooretrieve z našich serverech. [Zjistěte, jak tooget pracovat s hello rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md).

Jakmile budete mít funkční skriptu, bude potřeba další tooexamine hello heslo resetování a registraci události, které lze načíst toomeet vaše scénáře.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): uvádí hello sloupce, které jsou k dispozici pro resetování hesla události
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): uvádí hello sloupce, které jsou k dispozici pro události registrace pro resetování hesla

### <a name="reporting-api-data-retrieval-limitations"></a>Omezení generování sestav načtení dat rozhraní API

V současné době hello sestav Azure AD a rozhraní API událostí načte až příliš**než 75 000 jednotlivé události** z hello [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) a [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typy , pokrývání uzlů hello **posledních 30 dní**.

Pokud potřebujete tooretrieve nebo ukládání dat nad rámec tohoto okna, doporučujeme uchování v externí databáze a pomocí rozdílů hello tooquery hello rozhraní API, které způsobit. Naše doporučení je toobegin získávání tato data po začít používat SSPR ve vaší organizaci, zachovat ho externě a poté pokračujte rozdílů hello tootrack od této chvíle dál.

## <a name="how-toodownload-password-reset-registration-events-quickly-with-powershell"></a>Jak resetování hesla toodownload registrace události rychle pomocí prostředí PowerShell

Kromě toho toousing hello sestav Azure AD a rozhraní API události přímo, můžete také používat hello níže události registrace toorecent skript prostředí PowerShell ve vašem adresáři. To je užitečné v případě, že chcete toosee, kteří je nedávno zaregistrovaná, nebo chcete tooensure, že vaše heslo resetovat zavedení dochází podle očekávání.

* [Registrace Azure AD SSPR, aktivity skript prostředí PowerShell](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Popis sloupce sestavy na portálu Azure

Hello následující seznam popisuje všechny sloupce sestavy hello podrobně:

* **Uživatel** – hello uživatele, který se pokusil heslo resetovat operace Registrování.
* **Role** – hello role hello uživatele v adresáři hello.
* **Datum a čas** – hello datum a čas hello pokus.
* **Data registrovaná** – registrace pro resetování uživatele hello data ověřování zadané během heslo.

### <a name="description-of-report-values-in-azure-portal"></a>Popis hodnoty sestavy na portálu Azure

Hello následující tabulka popisuje různé hodnoty hello povolené pro každý sloupec:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Data zaregistrován |**Alternativní e-mailu** – uživatel použitý alternativní e-mailu nebo ověřování tooauthenticate e-mailu<p><p>**Telefon do kanceláře**– použít office phone tooauthenticate uživatele<p>**Mobilní telefon** -použít mobilní telefon nebo ověřování tooauthenticate Telefon uživatele<p>**Bezpečnostní otázky** – otázky, které uživatel používá zabezpečení tooauthenticate<p>**Libovolnou kombinaci hello výše (například alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje, které dvě metody hello tooauthentication uživatel použitý žádost o resetování hesla. |

## <a name="view-password-reset-activity-in-hello-classic-portal"></a>Aktivita portálu classic hello resetování hesla zobrazení

Tato sestava zobrazí že všechny heslo resetovat pokusy, k nimž došlo ve vaší organizaci.

* **Maximální časový rozsah**: 30 dnů
* **Maximální počet řádků**: 75 000
* **Zaváděná**: Ano, pomocí souboru CSV

### <a name="description-of-report-columns-in-azure-classic-portal"></a>Popis sloupce sestavy na portálu Azure classic

Hello následující seznam popisuje všechny sloupce sestavy hello podrobně:

1. **Uživatel** – hello uživatele, který se pokusil heslo resetovat operace (podle pole ID uživatele hello poskytnutý při hello uživatele dodává tooreset heslo).
2. **Role** – hello role hello uživatele v adresáři hello.
3. **Datum a čas** – hello datum a čas hello pokus.
4. **Metody používá** – jaké metody ověřování hello uživatel použitý pro toto resetování operaci.
5. **Výsledek** – hello výsledek hello heslo resetovat operaci.
6. **Podrobnosti o** – podrobnosti hello proč resetování hesla hello výsledkem hello hodnota neodpovídala.  Také zahrnuje všechny kroky zmírňující rizika, může trvat tooresolve k neočekávané chybě.

### <a name="description-of-report-values-in-azure-classic-portal"></a>Popis hodnoty sestavy na portálu Azure classic

Hello následující tabulka popisuje různé hodnoty hello povolené pro každý sloupec:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Metody |**Alternativní e-mailu** – uživatel použitý alternativní e-mailu nebo ověřování tooauthenticate e-mailu<p>**Telefon do kanceláře** – použít office phone tooauthenticate uživatele<p>**Mobilní telefon** – použít mobilní telefon nebo ověřování tooauthenticate Telefon uživatele<p>**Bezpečnostní otázky** – otázky, které uživatel používá zabezpečení tooauthenticate<p>**Libovolnou kombinaci hello výše (například alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje, které dvě metody hello tooauthentication uživatel použitý žádost o resetování hesla. |
| výsledek |**Opuštění** – uživatel spustil resetování hesla, ale pak zastavena polovině vzdálenosti prostřednictvím bez dokončení<p>**Blokované** – uživatelský účet byl zabránila toouse heslo resetovat kvůli stránku pro reset hesla tooattempting toouse hello nebo jedno heslo resetování brány příliš mnohokrát v období 24 hodin<p>**Došlo ke zrušení** – uživatel spustil resetování hesla, ale pak jste klikli na hello Storno tlačítko toocancel hello relace součást způsobem pomocí <p>**Kontaktovat správce** – uživatele došlo k potížím při jeho relaci, která mu nebylo možné přeložit, takže hello uživatel klepl na odkaz "Obraťte se na správce" hello místo dokončení resetování hesla hello toku<p>**Se nezdařilo** – uživatel není možné tooreset heslo, pravděpodobně proto hello uživatele není nakonfigurované toouse hello funkce (například žádné licence, chybějící informace o ověřování, heslo spravované místní ale zpětný zápis je vypnutý).<p>**Úspěšné** – resetování hesla byla úspěšná. |
| Podrobnosti |Viz následující tabulka |

### <a name="allowed-values-for-details-column"></a>Povolené hodnoty pro sloupec podrobnosti

Níže je seznam hello typy výsledků, které můžete očekávat při použití hello heslo resetujte sestavu aktivity:

| Podrobnosti | Typ výsledku |
| --- | --- |
| Uživatel opuštění po dokončení hello možnost ověření e-mailu |opuštění |
| Uživatel opuštění po dokončení hello možnost ověření mobilní služby SMS |opuštění |
| Opuštění po dokončení možnost hello mobilní hlasové volání ověření uživatele |opuštění |
| Uživatel opuštění po dokončení možnost ověření hello office hlasového hovoru |opuštění |
| Uživatel opuštění po dokončení možnosti otázky zabezpečení hello |opuštění |
| Uživatel opuštění po zadání jejich ID uživatele |opuštění |
| Uživatel opuštění po spuštění hello možnost ověření e-mailu |opuštění |
| Uživatel opuštění po spuštění hello možnost ověření mobilní služby SMS |opuštění |
| Opuštění po spuštění možnost hello mobilní hlasové volání ověření uživatele |opuštění |
| Uživatel opuštění po spuštění možnost ověření hello office hlasového hovoru |opuštění |
| Uživatel opuštění po spuštění možnosti otázky zabezpečení hello |opuštění |
| Opuštění před výběrem nové heslo uživatele |opuštění |
| Uživatel opuštění, že při výběru nového hesla |opuštění |
| Uživatele zadali Mockrát neplatný kód ověření SMS a je blokovaná 24 hodin |Blokovaný |
| Uživatel pokusili příliš mnohokrát hlasové ověření mobilního telefonu a je blokovaná 24 hodin |Blokovaný |
| Uživatel pokusil hlasové ověření telefonu v office příliš mnohokrát a blokováno 24 hodin |Blokovaný |
| Pokusili příliš mnohokrát tooanswer bezpečnostní otázky uživatele a je blokovaná 24 hodin |Blokovaný |
| Pokusili příliš mnohokrát tooverify telefonní číslo uživatele a je blokovaná 24 hodin |Blokovaný |
| Uživatel stornoval před předáním hello požadované metody ověřování |Zrušeno |
| Uživatel stornoval před odesláním nové heslo |Zrušeno |
| Uživatele kontaktovat správce po pokusu o možnost ověření e-mailu hello |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o hello možnost ověření mobilní služby SMS |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o možnost ověření volání mobilní hlasové hello |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o možnost ověření hello office hlasového hovoru |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o možnost ověření otázky zabezpečení hello |Kontaktovala správce |
| Pro tohoto uživatele není povoleno vytvoření nového hesla. Povolit heslo resetovat pod hello karta tooresolve touto konfigurací |Se nezdařilo |
| Uživatel nemá licenci. Můžete přidat licence toohello uživatele tooresolve to |Se nezdařilo |
| Uživatel se pokusil tooreset ze zařízení bez povoleny soubory cookie |Se nezdařilo |
| Uživatelský účet má dostatečná ověřování metody definované. Přidejte tooresolve informace o ověřování |Se nezdařilo |
| Heslo uživatele je spravovaná místně. Můžete ho povolit zpětný zápis hesla tooresolve |Se nezdařilo |
| Je nelze spojit místní služby resetování hesla. Synchronizace počítače v protokolu událostí |Se nezdařilo |
| Při resetování hesla pro místní uživatele hello nastal nějaký problém. Synchronizace počítače v protokolu událostí |Se nezdařilo |
| Tento uživatel není členem skupiny users resetování hesla hello. Přidejte tento tooresolve toothat skupiny uživatelů. |Se nezdařilo |
| Resetování hesla byla zakázána zcela pro tohoto klienta. V tématu [sem](http://aka.ms/ssprtroubleshoot) tooresolve to. |Se nezdařilo |
| Uživatel úspěšně resetovat heslo |Úspěch |

## <a name="self-service-password-management-activity-types"></a>Samoobslužné služby typy aktivit správy hesel

Hello následující typy aktivit se zobrazí v hello **Samoobslužná správa hesel** kategorie události auditu.  Popis pro každou způsobem.

* [**Blokovat samoobslužné resetování hesla** ](#activity-type-blocked-from-self-service-password-reset) -Určuje, uživatel se pokusil tooreset heslo, použít konkrétní brány, nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* [**Změnit heslo (samoobslužné)** ](#activity-type-change-password-self-service) -označuje uživatel provést dobrovolná nebo vynutit (kvůli tooexpiry) změna hesla.
* [**Resetovat heslo (správcem)** ](#activity-type-reset-password-by-admin) -označuje správce provést heslo resetovat jménem uživatele z hello portálu Azure.
* [**Resetovat heslo (samoobslužné)** ](#activity-type-reset-password-self-service) -určuje uživatel úspěšně resetovat heslo služby hello [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* [**Resetování hesla pomocí samoobslužné služby průběh aktivity toku** ](#activity-type-self-serve-password-reset-flow-activity-progress) -označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást hello heslo proces obnovení.
* [**Odemknutí uživatelský účet (samoobslužné)** ](#activity-type-unlock-user-account-self-service) -označuje uživatele byla úspěšně odemknuta účtu služby Active Directory bez resetování hesla z hello [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pomocí Hello AD účet odemknout bez resetování funkce.
* [**Uživatel zaregistrovat pro resetování hesla pomocí samoobslužné služby** ](#activity-type-user-registered-for-self-service-password-reset) -označuje uživatele registrován všechny hello požadované informace toobe možné tooreset své heslo v souladu s hello aktuálně zadaný zásady resetování hesel klienta.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Typ aktivity: blokovat samoobslužné resetování hesla

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele se pokusila tooreset heslo, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* **Aktivita objektu Actor** -hello uživatele, který byl omezeny provedení další operace resetování. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -hello uživatele, který byl omezeny provedení další operace resetování. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele byla omezena provádět žádné další nastavení, pokus žádné další ověřovací metody nebo ověřit žádné další telefonní čísla pro hello dalších 24 hodin.
* **Důvod selhání stavu aktivity** – není k dispozici

### <a name="activity-type-change-password-self-service"></a>Typ aktivity: Změna hesla (samoobslužné)

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel provést dobrovolná nebo vynutit (kvůli tooexpiry) změna hesla.
* **Aktivita objektu Actor** -hello uživatele, který změnit své heslo. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -hello uživatele, který změnit své heslo. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatel úspěšně změnit své heslo
  * _Selhání_ -označuje uživatele nepodařilo toochange své heslo. Kliknutím na řádek hello vám umožní toosee hello **důvod stavu aktivity** kategorie toolearn více informací o proč hello došlo k chybě.
* **Důvod selhání stavu aktivity** - 
  * _FuzzyPolicyViolationInvalidPassword_ -hello uživatel vybral hesla, který byl automaticky zakázán z důvodu možností tooMicrosoft na zakázané detekce heslo je moc známé nebo zejména slabé hledání toobe.

### <a name="activity-type-reset-password-by-admin"></a>Typ aktivity: resetování hesla (správcem)

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje správce provést heslo resetovat jménem uživatele z hello portálu Azure.
* **Aktivita objektu Actor** -hello správce, který provedl resetování jménem jiného koncový uživatel nebo správce hello hesla. Musí být buď globální správce, heslo správce, Správce uživatelů nebo správce technické podpory.
* **Cíl aktivity** -hello uživatele, jehož heslo byl resetován. Může být koncovým nebo jiný správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -určuje správce úspěšně resetovat heslo uživatele
  * _Selhání_ -označuje správce nepodařilo toochange heslo uživatele. Kliknutím na řádek hello vám umožní toosee hello **důvod stavu aktivity** kategorie toolearn více informací o proč hello došlo k chybě.

### <a name="activity-type-reset-password-self-service"></a>Typ aktivity: resetování hesla (samoobslužné)

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel úspěšně obnovit své heslo z hello [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* **Aktivita objektu Actor** -hello uživatele, který obnovit své heslo. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -hello uživatele, který obnovit své heslo. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -určuje uživatel úspěšně resetovat vlastní hesla
  * _Selhání_ -označuje uživatele nepodařilo tooreset své vlastní heslo. Kliknutím na řádek hello vám umožní toosee hello **důvod stavu aktivity** kategorie toolearn více informací o proč hello došlo k chybě.
* **Důvod selhání stavu aktivity** -
  * _FuzzyPolicyViolationInvalidPassword_ -Dobrý den, správce vybrané heslo, který byl automaticky zakázán z důvodu možností tooMicrosoft na zakázané detekce heslo je moc známé nebo zejména slabé hledání toobe.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Typ aktivity: vlastní sloužit průběh aktivity toku resetování hesla

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást hello heslo proces obnovení.
* **Aktivita objektu Actor** -hello uživatel, který provedl součástí hello heslo resetovat toku. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -hello uživatel, který provedl součástí hello heslo resetovat toku. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatel úspěšně dokončil konkrétní krok hello procesu resetování hesla.
  * _Selhání_ -určuje konkrétní krok hello heslo resetovat toku se nezdařilo. Kliknutím na řádek hello vám umožní toosee hello **důvod stavu aktivity** kategorie toolearn více informací o proč hello došlo k chybě.
* **Povolené důvody stavu aktivity**
  * V následující tabulce najdete v části [všechny povolené aktivita resetování důvody stavu](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Typ aktivity: odemčení uživatelského účtu (samoobslužné)

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byla úspěšně odemknuta účtu služby Active Directory bez resetování hesla z hello [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) účtem hello AD odemčení bez resetování funkce.
* **Aktivita objektu Actor** -hello uživatele, který odemknuli svůj účet bez resetování hesla. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -hello uživatele, který odemknuli svůj účet bez resetování hesla. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele byla úspěšně odemknuta vlastní účet.
  * _Selhání_ -označuje uživatele nepodařilo toounlock svého účtu. Kliknutím na řádek hello vám umožní toosee hello **důvod stavu aktivity** kategorie toolearn více informací o proč hello došlo k chybě.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Typ aktivity: uživatel zaregistrován pro samoobslužné resetování hesla

Hello následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele registrován všechny hello požadované informace toobe možné tooreset své heslo v souladu s hello aktuálně zadaný zásady resetování hesel klienta. 
* **Aktivita objektu Actor** -hello uživatele, který zaregistrovat pro resetování hesla. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -hello uživatele, který zaregistrovat pro resetování hesla. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele úspěšně registrován pro v souladu s hello aktuální zásady resetování hesel. 
  * _Selhání_ -označuje tooregister uživatele se nezdařilo pro resetování hesla. Kliknutím na řádek hello vám umožní toosee hello **důvod stavu aktivity** kategorie toolearn více informací o proč hello došlo k chybě. Poznámka: - neznamená to uživatele je nelze tooreset své vlastní heslo, stačí, aby jejich nebyla dokončena, proces registrace hello. Pokud je neověřené data na jejich účtu, který je správný (například telefonní číslo, které není ověřený), i když jejich nebyly ověřeny toto telefonní číslo, přesto jej mohou používat tooreset své heslo. Další informace najdete v tématu [co se stane, když se uživatel zaregistruje?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [Protokoly auditu správy toouser zástupce](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) – přejděte přímo protokoly auditu správy uživatelů tooyour klienta
* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
