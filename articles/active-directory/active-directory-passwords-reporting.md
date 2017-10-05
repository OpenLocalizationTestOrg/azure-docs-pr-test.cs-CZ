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
ms.openlocfilehash: ba7b36e654aa0bf3b74d42a2b0ae96ae2a9b6241
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Možnosti zasílání zpráv o správou hesel Azure AD

Po nasazení mnoho organizací bude chtít vědět, jak nebo pokud SSPR je doopravdy používá. Azure AD poskytuje vytváření sestav funkce, které pomáhají při odpovědi na otázky pomocí specifických sestav a pokud máte správně licenci, umožňují vytvářet vlastní dotazy.

Sestavy, které existují v [portál Azure] může odpovědět na následující otázky (https://portal.azure.com/).

> [!NOTE]
> Musí být [globální správce](active-directory-assign-admin-roles.md#assign-or-remove-administrator-roles) a musí výslovný souhlas pro tato data shromáždit jménem vaší organizace, navštivte reporting kartě nebo auditu protokoly alespoň jednou. Až to uděláte, nebudou shromažďovány dat pro vaši organizaci

* Kolik lidí registrovali pro resetování hesla?
* Kdo má zaregistrovat pro resetování hesla?
* Jaká data jsou osoby registraci?
* Kolik uživatelů resetovat vlastní hesla v posledních sedmi dnů?
* Jaké jsou nejběžnější metody uživatelé nebo správci použít k resetování hesla?
* Jaké jsou běžné problémy uživatelé nebo správci setkávají při pokusu o použití resetování hesla?
* Co správci jsou často resetovat vlastní hesla?
* Je k dispozici podezřelé aktivity přejdete k resetování hesla?

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Postup zobrazení sestav správy hesel na portálu Azure

V portálu Azure prostředí budeme mít lepší způsob zobrazení resetování hesla a registrace aktivita resetování hesla.  Použijte následující postup a zjistit, že resetování hesla a heslo resetovat registrace události:

1. Přejděte na [ **portal.azure.com**](https://portal.azure.com)
2. Klikněte na tlačítko **další služby** na hlavní Azure portálu levé navigační nabídky
3. Vyhledejte **Azure Active Directory** v seznamu služeb a vyberte ho
4. Klikněte na tlačítko **uživatelé a skupiny** v navigační nabídce Azure Active Directory
5. Klikněte **protokoly auditu** navigační položka v navigační nabídce Uživatelé a skupiny. Toto jsou zobrazeny všechny události auditu, ke kterým dochází u všech uživatelů ve vašem adresáři. Toto zobrazení zobrazíte všechny související s hesly události, také můžete filtrovat.
6. Chcete-li filtrovat tohoto zobrazení a jen heslo resetovat související události, klikněte na tlačítko **filtru** tlačítka v horní části okna.
7. Z **filtru** nabídce vyberte možnost **kategorie** rozevíracího seznamu a změňte ji na **Samoobslužná správa hesel** typ kategorie.
8. Volitelně další filtrování seznamu výběrem konkrétní **aktivity** vás zajímá

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Jak načíst heslo správy události z sestav Azure AD a události rozhraní API

Sestavy služby Azure AD a události rozhraní API podporuje načítání všechny informace, které jsou zahrnuty v sestavách registrace resetování hesla pro obnovení a heslo. Pomocí tohoto rozhraní API si můžete stáhnout resetování hesla jednotlivých a registrace události pro integraci v rámci technologie vytváření sestav podle svého výběru pro resetování hesla.

### <a name="how-to-get-started-with-the-reporting-api"></a>Jak začít pracovat s rozhraním API pro generování sestav

Pro přístup k těmto datům, budete muset zápisu malých aplikace nebo skriptu načíst z našich serverech. [Zjistěte, jak začít pracovat s rozhraním API pro Azure AD Reporting](active-directory-reporting-api-getting-started.md).

Až budete mít skript práci, budete dále chcete prověřit heslo resetování a registraci události, které můžete načíst do splňují vaše scénáře.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): seznam sloupců, které jsou k dispozici pro resetování hesla události
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): seznam sloupců, které jsou k dispozici pro události registrace pro resetování hesla

### <a name="reporting-api-data-retrieval-limitations"></a>Omezení generování sestav načtení dat rozhraní API

V současné době sestav Azure AD a rozhraní API událostí načte až **než 75 000 jednotlivé události** z [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) a [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typů pokrývání uzlů **posledních 30 dní**.

Pokud budete potřebovat načíst nebo ukládání dat nad rámec tohoto okna, doporučujeme uchování v externí databáze a dotaz rozdílů, kterých se pomocí rozhraní API. Naše doporučení je zahájit načítání tato data po spuštění pomocí SSPR ve vaší organizaci, je zachování externě a poté pokračujte ke sledování se rozdíly od tohoto okamžiku.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Stažení události registrace resetování hesla rychle pomocí prostředí PowerShell

Kromě používání sestav Azure AD a rozhraní API události přímo, můžete také používat níže uvedený skript prostředí PowerShell na poslední události registrace ve vašem adresáři. To je užitečné v případě, že chcete se podívat, kdo zaregistrovala nedávno nebo by rádi zajistili, že vaše heslo resetovat zavedení dochází podle očekávání.

* [Registrace Azure AD SSPR, aktivity skript prostředí PowerShell](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

### <a name="description-of-report-columns-in-azure-portal"></a>Popis sloupce sestavy na portálu Azure

Následující seznam popisuje všechny sloupce sestavy podrobně:

* **Uživatel** – uživatel, který se pokusil heslo resetovat operace Registrování.
* **Role** – role uživatele v adresáři.
* **Datum a čas** – datum a čas pokus.
* **Data registrovaná** – registrace pro resetování jaká data ověřování uživatele zadali během heslo.

### <a name="description-of-report-values-in-azure-portal"></a>Popis hodnoty sestavy na portálu Azure

Následující tabulka popisuje různé hodnoty pro každý sloupec povolená:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Data zaregistrován |**Alternativní e-mailu** – uživatel používá k ověření alternativní e-mailu nebo ověřování e-mailu<p><p>**Telefon do kanceláře**– telefonní číslo do kanceláře uživatel použitý k ověření<p>**Mobilní telefon** -uživatele použít mobilní telefon nebo telefon pro ověření k ověření<p>**Bezpečnostní otázky** – uživatel používá k ověření bezpečnostních otázek<p>**Libovolnou kombinaci výše (například alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje které dvě metody uživatel použitý k ověření žádost o resetování hesla. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Aktivita portálu classic resetování hesla zobrazení

Tato sestava zobrazí že všechny heslo resetovat pokusy, k nimž došlo ve vaší organizaci.

* **Maximální časový rozsah**: 30 dnů
* **Maximální počet řádků**: 75 000
* **Zaváděná**: Ano, pomocí souboru CSV

### <a name="description-of-report-columns-in-azure-classic-portal"></a>Popis sloupce sestavy na portálu Azure classic

Následující seznam popisuje všechny sloupce sestavy podrobně:

1. **Uživatel** – uživatel, který se pokusil heslo resetovat operace (podle pole ID uživatele zadané při přístupu uživatele k resetování hesla).
2. **Role** – role uživatele v adresáři.
3. **Datum a čas** – datum a čas pokus.
4. **Metody používá** – resetovat jaké metody ověřování, které uživatel používá pro tuto operaci.
5. **Výsledek** – výsledek heslo resetovat operaci.
6. **Podrobnosti o** – podrobnosti o proč resetovat heslo, které jsou výsledkem hodnota neodpovídala.  Také zahrnuje všechny kroky zmírňující rizika, může trvat řešení k neočekávané chybě.

### <a name="description-of-report-values-in-azure-classic-portal"></a>Popis hodnoty sestavy na portálu Azure classic

Následující tabulka popisuje různé hodnoty pro každý sloupec povolená:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Metody |**Alternativní e-mailu** – uživatel používá k ověření alternativní e-mailu nebo ověřování e-mailu<p>**Telefon do kanceláře** – telefonní číslo do kanceláře uživatel použitý k ověření<p>**Mobilní telefon** – uživatel použít mobilní telefon nebo telefon pro ověření k ověření<p>**Bezpečnostní otázky** – uživatel používá k ověření bezpečnostních otázek<p>**Libovolnou kombinaci výše (například alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje které dvě metody uživatel použitý k ověření žádost o resetování hesla. |
| výsledek |**Opuštění** – uživatel spustil resetování hesla, ale pak zastavena polovině vzdálenosti prostřednictvím bez dokončení<p>**Blokované** – účet uživatele zabránily používat kvůli pokusu o resetování hesla na stránku resetování hesla nebo jedno heslo resetování brány příliš mnohokrát v období 24 hodin<p>**Došlo ke zrušení** – uživatel spustil resetování hesla, ale pak kliknutí na tlačítko Storno zrušit součást způsobem pomocí relace <p>**Kontaktovat správce** – uživatele došlo k potížím při jeho relaci, která mu nebylo možné přeložit, tak uživatel klikli na odkaz "Obraťte se na správce" namísto dokončení heslo resetovat toku<p>**Se nezdařilo** – uživatele se nepodařilo resetovat heslo, pravděpodobně, protože uživatel nebyl nakonfigurován na použití funkce (například žádné licence, chybějící informace o ověřování, heslo spravované místní ale zpětný zápis je vypnutý).<p>**Úspěšné** – resetování hesla byla úspěšná. |
| Podrobnosti |Viz následující tabulka |

### <a name="allowed-values-for-details-column"></a>Povolené hodnoty pro sloupec podrobnosti

Níže je seznam typy výsledků, které můžete očekávat při použití heslo resetujte sestavu aktivity:

| Podrobnosti | Typ výsledku |
| --- | --- |
| Uživatel opuštění po dokončení možnost ověření e-mailu |opuštění |
| Uživatel opuštění po dokončení mobilní možnost ověření serveru SMS |opuštění |
| Opuštění po dokončení možnost mobilní hlasový hovor ověření uživatele |opuštění |
| Opuštění po dokončení možnost office hlasového hovoru ověření uživatele |opuštění |
| Uživatel opuštění po dokončení zabezpečení otázky, které možnost |opuštění |
| Uživatel opuštění po zadání jejich ID uživatele |opuštění |
| Uživatel opuštění po spuštění možnost ověření e-mailu |opuštění |
| Uživatel opuštění po spuštění mobilní možnost ověření serveru SMS |opuštění |
| Opuštění po spuštění možnost mobilní hlasový hovor ověření uživatele |opuštění |
| Opuštění po spuštění možnost office hlasového hovoru ověření uživatele |opuštění |
| Uživatel opuštění po spuštění zabezpečení otázky, které možnost |opuštění |
| Opuštění před výběrem nové heslo uživatele |opuštění |
| Uživatel opuštění, že při výběru nového hesla |opuštění |
| Uživatele zadali Mockrát neplatný kód ověření SMS a je blokovaná 24 hodin |Blokovaný |
| Uživatel pokusili příliš mnohokrát hlasové ověření mobilního telefonu a je blokovaná 24 hodin |Blokovaný |
| Uživatel pokusil hlasové ověření telefonu v office příliš mnohokrát a blokováno 24 hodin |Blokovaný |
| Uživatel se pokusil odpoví na bezpečnostní otázky příliš mnohokrát a je blokovaná 24 hodin |Blokovaný |
| Uživatel se pokusil ověřit telefonní číslo příliš mnohokrát a je blokovaná 24 hodin |Blokovaný |
| Uživatel stornoval před předáním metod vyžaduje ověřování |Zrušeno |
| Uživatel stornoval před odesláním nové heslo |Zrušeno |
| Uživatele kontaktovat správce po pokusu o možnost ověření e-mailu |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o mobilní možnost ověření serveru SMS |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o možnost ověření mobilní hlasový hovor |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o možnost ověření office hlasového hovoru |Kontaktovala správce |
| Uživatele kontaktovat správce po pokusu o ověření možnost bezpečnostní otázku |Kontaktovala správce |
| Pro tohoto uživatele není povoleno vytvoření nového hesla. Povolit na kartě Konfigurace tento problém vyřešíte resetování hesla |Se nezdařilo |
| Uživatel nemá licenci. Můžete přidat licenci uživateli, aby to vyřešit |Se nezdařilo |
| Uživatel se pokusil obnovit ze zařízení bez povoleny soubory cookie |Se nezdařilo |
| Uživatelský účet má dostatečná ověřování metody definované. Přidat informace o ověřování to vyřešit |Se nezdařilo |
| Heslo uživatele je spravovaná místně. Můžete povolit zpětný zápis hesla to vyřešit |Se nezdařilo |
| Je nelze spojit místní služby resetování hesla. Synchronizace počítače v protokolu událostí |Se nezdařilo |
| Došlo k potížím při resetování uživatele místní heslo. Synchronizace počítače v protokolu událostí |Se nezdařilo |
| Tento uživatel není členem skupiny users resetování hesla. Přidejte tohoto uživatele do této skupiny, abyste tento problém vyřešili. |Se nezdařilo |
| Resetování hesla byla zakázána zcela pro tohoto klienta. V tématu [sem](http://aka.ms/ssprtroubleshoot) to vyřešit. |Se nezdařilo |
| Uživatel úspěšně resetovat heslo |Úspěch |

## <a name="self-service-password-management-activity-types"></a>Samoobslužné služby typy aktivit správy hesel

Následující typy aktivit v zobrazí **Samoobslužná správa hesel** kategorie události auditu.  Popis pro každou způsobem.

* [**Blokovat samoobslužné resetování hesla** ](#activity-type-blocked-from-self-service-password-reset) -Určuje, uživatel se pokusil obnovit heslo, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* [**Změnit heslo (samoobslužné)** ](#activity-type-change-password-self-service) -označuje uživatel provést dobrovolná nebo vynutit (z důvodu vypršení platnosti) změna hesla.
* [**Resetovat heslo (správcem)** ](#activity-type-reset-password-by-admin) -označuje správce provést heslo resetovat jménem uživatele z portálu Azure.
* [**Resetovat heslo (samoobslužné)** ](#activity-type-reset-password-self-service) -určuje uživatel úspěšně resetovat heslo služby [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* [**Resetování hesla pomocí samoobslužné služby průběh aktivity toku** ](#activity-type-self-serve-password-reset-flow-activity-progress) -označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást heslo proces obnovení.
* [**Odemknutí uživatelský účet (samoobslužné)** ](#activity-type-unlock-user-account-self-service) -označuje uživatele byla úspěšně odemknuta účtu služby Active Directory bez resetování hesla z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pomocí služby AD odemknutí účtu bez resetování funkce.
* [**Uživatel zaregistrovat pro resetování hesla pomocí samoobslužné služby** ](#activity-type-user-registered-for-self-service-password-reset) -označuje uživatele registrován všechny požadované informace, abyste mohli obnovit své heslo v souladu s zásady resetování hesel aktuálně zadaného klienta.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Typ aktivity: blokovat samoobslužné resetování hesla

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byl proveden pokus o resetování hesla, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* **Aktivita objektu Actor** -uživatel, který byl omezeny provedení další operace resetování. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který byl omezeny provedení další operace resetování. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele byla omezena provádět žádné další nastavení, pokus žádné další ověřovací metody nebo ověření žádné další telefonní čísla pro dalších 24 hodin.
* **Důvod selhání stavu aktivity** – není k dispozici

### <a name="activity-type-change-password-self-service"></a>Typ aktivity: Změna hesla (samoobslužné)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel provést dobrovolná nebo vynutit (z důvodu vypršení platnosti) změna hesla.
* **Aktivita objektu Actor** -uživatel, který změnit své heslo. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který změnit své heslo. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatel úspěšně změnit své heslo
  * _Selhání_ -označuje uživatele se nepodařilo změnit své heslo. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Důvod selhání stavu aktivity** - 
  * _FuzzyPolicyViolationInvalidPassword_ -uživatel vybral hesla, který byl automaticky zakázán z důvodu společnosti Microsoft zakázané heslo možností detekce hledání je moc známé nebo zejména slabé.

### <a name="activity-type-reset-password-by-admin"></a>Typ aktivity: resetování hesla (správcem)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje správce provést heslo resetovat jménem uživatele z portálu Azure.
* **Aktivita objektu Actor** -správce, který provedl resetování jménem jiného koncový uživatel nebo správce hesel. Musí být buď globální správce, heslo správce, Správce uživatelů nebo správce technické podpory.
* **Cíl aktivity** -uživatele, jehož heslo byl resetován. Může být koncovým nebo jiný správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -určuje správce úspěšně resetovat heslo uživatele
  * _Selhání_ -označuje správce se nepodařilo změnit heslo uživatele. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.

### <a name="activity-type-reset-password-self-service"></a>Typ aktivity: resetování hesla (samoobslužné)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel úspěšně obnovit své heslo z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* **Aktivita objektu Actor** -uživatel, který obnovit své heslo. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který obnovit své heslo. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -určuje uživatel úspěšně resetovat vlastní hesla
  * _Selhání_ -udává uživatele se nepodařilo obnovit své vlastní heslo. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Důvod selhání stavu aktivity** -
  * _FuzzyPolicyViolationInvalidPassword_ -správce vybrané heslo, který byl automaticky zakázán z důvodu společnosti Microsoft zakázané heslo možností detekce hledání je moc známé nebo zejména slabé.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Typ aktivity: vlastní sloužit průběh aktivity toku resetování hesla

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást heslo proces obnovení.
* **Aktivita objektu Actor** -uživatele, který provedl součástí heslo resetovat toku. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatele, který provedl součástí heslo resetovat toku. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatel úspěšně dokončil konkrétní krok v procesu resetování hesla.
  * _Selhání_ -určuje konkrétní krok pro heslo resetovat toku se nezdařilo. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Povolené důvody stavu aktivity**
  * V následující tabulce najdete v části [všechny povolené aktivita resetování důvody stavu](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Typ aktivity: odemčení uživatelského účtu (samoobslužné)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byla úspěšně odemknuta účtu služby Active Directory bez resetování hesla z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pod účtem AD odemčení bez resetování funkce.
* **Aktivita objektu Actor** -uživatel, který odemknuli svůj účet bez resetování hesla. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který odemknuli svůj účet bez resetování hesla. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele byla úspěšně odemknuta vlastní účet.
  * _Selhání_ -udává uživatele se nepodařilo odemknout svůj účet. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Typ aktivity: uživatel zaregistrován pro samoobslužné resetování hesla

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele registrován všechny požadované informace, abyste mohli obnovit své heslo v souladu s zásady resetování hesel aktuálně zadaného klienta. 
* **Aktivita objektu Actor** -uživatel, který zaregistrovat pro resetování hesla. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který zaregistrovat pro resetování hesla. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele úspěšně registrován pro v souladu s aktuální zásady resetování hesel. 
  * _Selhání_ -udává uživatele se nepodařilo zaregistrovat pro resetování hesla. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě. Všimněte si – to neznamená, uživatel se nemůže resetovat vlastní heslo, právě, že nebyla dokončena proces registrace. Pokud je neověřené data na jejich účtu, který je správný (například telefonní číslo, které není ověřený), i když jejich nebyly ověřeny toto telefonní číslo, můžete stále používají ho resetovat heslo. Další informace najdete v tématu [co se stane, když se uživatel zaregistruje?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Další kroky

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [Zástupce protokoly auditu správy uživatele](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit) – přejděte přímo na správu uživatelů vašeho klienta protokoly auditu
* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.
* [**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.
* [**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? – Odpovědi na otázky, na které jste se vždy chtěli zeptat.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
