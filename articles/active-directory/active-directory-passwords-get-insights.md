---
title: "Get Insights: Sestavy Správa hesel služby Azure AD | Microsoft Docs"
description: "Tento článek popisuje postup používání sestav pro získání přehledu o operacích správy hesel ve vaší organizaci."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 1472b51d-53f4-4b0f-b1be-57f6fa88fa65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: ae83df618e3c392fe89878bcd1be0d6c6cb1edb4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-get-operational-insights-with-password-management-reports"></a>Jak získat provozní informace pomocí sestav správy hesel
> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
>
>

Tato část popisuje, jak můžete pomocí sestav správy hesel Azure Active Directory zobrazíte jak uživatelé používají resetování hesla a změňte ve vaší organizaci.

* [**Přehled sestavy správy hesel**](#overview-of-password-management-reports)
* [**Postup zobrazení sestav správy hesel v nový portál Azure**](#how-to-view-password-management-reports)
 * [Role Directory povoleno čtení sestavy](#directory-roles-allowed-to-read-reports)
* [**Samoobslužné služby typy aktivit správy hesel v nový portál Azure**](#self-service-password-management-activity-types)
 * [Blokovat samoobslužné resetování hesla](#activity-type-blocked-from-self-service-password-reset)
 * [Změnit heslo (samoobslužné)](#activity-type-change-password-self-service)
 * [Resetování hesla (správcem)](#activity-type-reset-password-by-admin)
 * [Resetování hesla (samoobslužné)](#activity-type-reset-password-self-service)
 * [Resetování hesla používat vlastní aktivity průběhu toku](#activity-type-self-serve-password-reset-flow-activity-progress)
 * [Odemknutí uživatelský účet (samoobslužné)](#activity-type-unlock-user-account-self-service)
 * [Uživatel zaregistrovaný pro samoobslužné resetování hesla](#activity-type-user-registered-for-self-service-password-reset)
* [**Jak načíst heslo správy události z sestav Azure AD a události rozhraní API**](#how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api)
 * [Omezení generování sestav načtení dat rozhraní API](#reporting-api-data-retrieval-limitations)
* [**Stažení události registrace resetování hesla rychle pomocí prostředí PowerShell**](#how-to-download-password-reset-registration-events-quickly-with-powershell)
* [**Postup zobrazení sestav správy hesel portálu classic**](#how-to-view-password-management-reports-in-the-classic-portal)
* [**Činnost registrace v organizaci na klasickém portálu pro resetování hesla zobrazení**](#view-password-reset-registration-activity-in-the-classic-portal)
* [**Aktivita ve vaší organizaci na portálu classic resetování hesla zobrazení**](#view-password-reset-activity-in-the-classic-portal)


## <a name="overview-of-password-management-reports"></a>Přehled sestavy správy hesel
Jakmile nasadíte resetování hesla, jedním z nejčastějších další kroky je chcete zobrazit, jak je používán ve vaší organizaci.  Například můžete chtít pohled na tom, jak jsou uživatelé registrace pro resetování hesla nebo heslo, kolik resetování jsou prováděny v posledních několik dnů.  Zde jsou některé běžné otázky, které mají být schopen s sestavy správy heslo, které existují v [portálu pro správu Azure](https://manage.windowsazure.com) dnes:

* Kolik lidí registrovali pro resetování hesla?
* Kdo má zaregistrovat pro resetování hesla?
* Jaká data jsou osoby registraci?
* Kolik uživatelů resetovat vlastní hesla v posledních 7 dnů?
* Jaké jsou nejběžnější metody uživatelé nebo správci použít k resetování hesla?
* Jaké jsou běžné problémy uživatelé nebo správci setkávají při pokusu o použití resetování hesla?
* Co správci jsou často resetovat vlastní hesla?
* Je k dispozici podezřelé aktivity přejdete k resetování hesla?

## <a name="how-to-view-password-management-reports"></a>Postup zobrazení sestav správy hesel
V novém [portálu Azure](https://portal.azure.com) prostředí, budeme mít lepší způsob zobrazení resetování hesla a registrace aktivita resetování hesla.  Postupujte podle následujících kroků a najít resetování hesla a resetování hesla v nové registrace události [portálu Azure](https://portal.azure.com):

1. Přejděte na [ **portal.azure.com**](https://portal.azure.com)
2. Klikněte na **další služby** nabídky na hlavní navigaci vlevo portálu Azure
3. Vyhledejte **Azure Active Directory** v seznamu služeb a vyberte ho
4. Klikněte na **uživatelé a skupiny** v navigační nabídce Azure Active Directory
5. Klikněte na **protokoly auditu** navigační položka v navigační nabídce Uživatelé a skupiny. Tím zobrazíte všechny vyskytující události auditu pro všechny uživatele ve vašem adresáři. Toto zobrazení zobrazíte všechny související s hesly události, také můžete filtrovat.
6. Chcete-li filtrovat tohoto zobrazení a jen události týkající se správou hesel, klikněte na tlačítko **filtru** tlačítka v horní části okna.
7. Z **filtru** nabídce vyberte možnost **kategorie** rozevíracího seznamu a změňte ji na **Samoobslužná správa hesel** typ kategorie.
8. Volitelně další filtrování seznamu výběrem konkrétní **aktivity** vás zajímá
### <a name="direct-link-to-user-audit-blade"></a>Přímý odkaz na okno uživatele auditu
Pokud se přihlásíte na portál, zde je přímý odkaz na okno auditu uživatele kde se můžete podívat tyto události:

* [Přejděte na správu auditu zobrazení uživatelského přímo](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Audit)

### <a name="directory-roles-allowed-to-read-reports"></a>Role Directory povoleno čtení sestavy
Následující role adresáře v současné době lze číst sestav správy hesel služby Azure AD na portálu Azure classic:

* Globální správce

Teprve pak ji bude moct číst tyto sestavy, globálního správce ve společnosti musí mít vyjádřit výslovný souhlas in pro tato data načíst jménem organizace návštěvou reporting kartě nebo protokoly auditu alespoň jednou. Až to uděláte, nebudou shromažďovány dat pro vaši organizaci.

Další informace o rolích adresáře a co mohou provádět, najdete v části [přiřazení rolí správce v Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles).

## <a name="self-service-password-management-activity-types"></a>Samoobslužné služby typy aktivit správy hesel
Následující typy aktivit v zobrazí **Samoobslužná správa hesel** kategorie události auditu.  Popis pro každou z těchto způsobem.

* [**Blokovat samoobslužné resetování hesla** ](#activity-type-blocked-from-self-service-password-reset) -Určuje, uživatel se pokusil obnovit heslo, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* [**Změnit heslo (samoobslužné)** ](#activity-type-change-password-self-service) -označuje uživatel provést dobrovolná nebo vynutit (z důvodu vypršení platnosti) změna hesla.
* [**Resetovat heslo (správcem)** ](#activity-type-reset-password-by-admin) -označuje správce provést heslo resetovat jménem uživatele z portálu Azure.
* [**Resetovat heslo (samoobslužné)** ](#activity-type-reset-password-self-service) -určuje uživatel úspěšně resetovat své heslo z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* [**Resetování hesla používat vlastní aktivity průběhu toku** ](#activity-type-self-serve-password-reset-flow-activity-progress) -označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást heslo proces obnovení.
* [**Odemknutí uživatelský účet (samoobslužné)** ](#activity-type-unlock-user-account-self-service) -označuje uživatele byla úspěšně odemknuta svůj účet služby Active Directory bez obnovení jeho hesla z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pomocí [odemknutí účtu AD bez resetování](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) funkce.
* [**Uživatel zaregistrovat pro resetování hesla pomocí samoobslužné služby** ](#activity-type-user-registered-for-self-service-password-reset) -označuje uživatel zaregistroval všechny požadované informace, abyste mohli obnovit svůj nebo jeho heslo v souladu s aktuálně zadaný klienta heslo resetovat zásady.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Typ aktivity: blokovat samoobslužné resetování hesla
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byl proveden pokus o resetování hesla, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* **Aktivita objektu Actor** -uživatel, který byl omezeny provedení další operace resetování. Může být koncovým nebo správcem.
* **Cíl aktivity** -uživatel, který byl omezeny provedení další operace resetování. Může být koncovým nebo správcem.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -označuje uživatele byla omezena provádět žádné další nastavení, pokus žádné další ověřovací metody nebo ověření žádné další telefonní čísla pro dalších 24 hodin.
* **Důvod selhání stavu aktivity** – není k dispozici

### <a name="activity-type-change-password-self-service"></a>Typ aktivity: Změna hesla (samoobslužné)
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel provést dobrovolná nebo vynutit (z důvodu vypršení platnosti) změna hesla.
* **Aktivita objektu Actor** -uživatel, který změnit své heslo. Může být koncovým nebo správcem.
* **Cíl aktivity** -uživatel, který změnit své heslo. Může být koncovým nebo správcem.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -označuje uživatel úspěšně změnit své heslo
 * _Selhání_ -označuje uživatele se nepodařilo změnit své heslo. Kliknutím na řádek vám umožní najdete v článku **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Důvod selhání stavu aktivity** -
 * _FuzzyPolicyViolationInvalidPassword_ -uživatel vybral heslo, který byl automaticky zakázán z důvodu společnosti Microsoft zakázané heslo možností detekce hledání je moc známé nebo zejména slabé.

### <a name="activity-type-reset-password-by-admin"></a>Typ aktivity: resetování hesla (správcem)
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje správce provést heslo resetovat jménem uživatele z portálu Azure.
* **Aktivita objektu Actor** -správce, který provedl resetování jménem jiného koncového uživatele nebo správce hesel. Musí být buď globální správce, heslo správce, Správce uživatelů nebo správce technické podpory.
* **Cíl aktivity** -uživatele, jehož heslo byl resetován. Může být koncovým nebo jiný správce.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -určuje správce úspěšně resetovat heslo uživatele
 * _Selhání_ -označuje správce se nepodařilo změnit heslo uživatele. Kliknutím na řádek vám umožní najdete v článku **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.

### <a name="activity-type-reset-password-self-service"></a>Typ aktivity: resetování hesla (samoobslužné)
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel úspěšně resetovat své heslo z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* **Aktivita objektu Actor** -uživatel, který resetovat své heslo. Může být koncovým nebo správcem.
* **Cíl aktivity** -uživatel, který resetovat své heslo. Může být koncovým nebo správcem.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -Určuje uživatele s úspěšně resetovat vlastní hesla
 * _Selhání_ -Určuje, uživatelé se nepodařilo resetovat vlastní heslo. Kliknutím na řádek vám umožní najdete v článku **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Důvod selhání stavu aktivity** -
 * _FuzzyPolicyViolationInvalidPassword_ -správce vybrané heslo, který byl automaticky zakázán z důvodu společnosti Microsoft zakázané heslo možností detekce hledání je moc známé nebo zejména slabé.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Typ aktivity: vlastní sloužit průběh aktivity toku resetování hesla
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást heslo proces obnovení.
* **Aktivita objektu Actor** -uživatele, který provedl součástí heslo resetovat toku. Může být koncovým nebo správcem.
* **Cíl aktivity** -uživatele, který provedl součástí heslo resetovat toku. Může být koncovým nebo správcem.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -označuje uživatel úspěšně dokončil konkrétní krok v procesu resetování hesla.
 * _Selhání_ -určuje konkrétní krok pro heslo resetovat toku se nezdařilo. Kliknutím na řádek vám umožní najdete v článku **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Povolené důvody stavu aktivity**
 * V následující tabulce najdete v části [všechny povolené aktivita resetování důvody stavu](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Typ aktivity: odemčení uživatelského účtu (samoobslužné)
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byla úspěšně odemknuta svůj účet služby Active Directory bez obnovení jeho hesla z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pomocí [účet AD odemčení bez resetování](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-customize#allow-users-to-unlock-accounts-without-resetting-their-password) funkce.
* **Aktivita objektu Actor** -uživatel, který odemknuli svůj účet bez resetování hesla. Může být koncovým nebo správcem.
* **Cíl aktivity** -uživatel, který odemknuli svůj účet bez resetování hesla. Může být koncovým nebo správcem.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -označuje uživatele s úspěšně odemknuli svůj vlastní účet
 * _Selhání_ -označuje uživateli se nepodařilo odemknout svůj účet. Kliknutím na řádek vám umožní najdete v článku **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Typ aktivity: uživatel zaregistrován pro samoobslužné resetování hesla
Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel zaregistroval všechny požadované informace, abyste mohli obnovit svůj nebo jeho heslo v souladu s aktuálně zadaný klienta heslo resetovat zásady.
* **Aktivita objektu Actor** -uživatel, který zaregistrovat pro resetování hesla. Může být koncovým nebo správcem.
* **Cíl aktivity** -uživatel, který zaregistrovat pro resetování hesla. Může být koncovým nebo správcem.
* **Stavy, které jsou povolené aktivity**
 * _Úspěch_ -označuje uživatele s úspěšně registrován pro v souladu s aktuální zásady resetování hesel.
 * _Selhání_ -udává uživatele se nepodařilo zaregistrovat pro resetování hesla. Kliknutím na řádek vám umožní najdete v článku **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě. Poznámka: - to neznamená, že uživatel není možné obnovit svůj nebo svoje heslo, stačí, že nebyla dokončena proces registrace. Pokud je neověřené data na jejich účtu, který je správný (například telefonní číslo, které není ověřený), i když jejich nebyly ověřeny toto telefonní číslo, můžete stále používají ho resetovat heslo. Další informace najdete v tématu [co se stane, když se uživatel zaregistruje?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Jak načíst heslo správy události z sestav Azure AD a události rozhraní API
K srpnu 2015 sestav Azure AD a rozhraní API události teď podporuje načítání všechny informace, které jsou součástí heslo pro resetování a heslo resetovat zprávy o registraci. Pomocí tohoto rozhraní API si můžete stáhnout resetování hesla jednotlivých a registrace události pro integraci s technologie vytváření sestav vaší choce resetování hesla.

### <a name="how-to-get-started-with-the-reporting-api"></a>Jak začít pracovat s rozhraním API pro generování sestav
Pro přístup k těmto datům, budete potřebovat k zápisu malých aplikace nebo skriptu načíst z našich serverech. [Zjistěte, jak začít pracovat s rozhraním API pro Azure AD Reporting](active-directory-reporting-api-getting-started.md).

Až budete mít skript práci, budete dále chcete prověřit heslo resetování a registraci události, které můžete načíst do splňují vaše scénáře.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): seznam sloupců, které jsou k dispozici pro resetování hesla události
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): seznam sloupců, které jsou k dispozici pro události registrace pro resetování hesla

### <a name="reporting-api-data-retrieval-limitations"></a>Omezení generování sestav načtení dat rozhraní API
V současné době sestav Azure AD a rozhraní API událostí načte až **než 75 000 jednotlivé události** z [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) a [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typů pokrývání uzlů **posledních 30 dní**.

Pokud budete potřebovat načíst nebo ukládání dat nad rámec tohoto okna, doporučujeme uchování v externí databáze a dotaz rozdílů, kterých se pomocí rozhraní API. Osvědčeným postupem je zahájení načítání tato data po spuštění vaše heslo resetovat procesu registrace v organizaci, zachovat ho externě a poté pokračujte ke sledování se rozdíly od tohoto okamžiku.

## <a name="how-to-download-password-reset-registration-events-quickly-with-powershell"></a>Stažení události registrace resetování hesla rychle pomocí prostředí PowerShell
Kromě používání sestav Azure AD a rozhraní API události přímo, můžete také používat níže uvedený skript prostředí PowerShell na poslední události registrace ve vašem adresáři. To je užitečné v případě, že chcete se podívat, kdo zaregistrovala nedávno nebo by rádi zajistili, že vaše heslo resetovat zavedení dochází podle očekávání.

* [Registrace Azure AD SSPR, aktivity skript prostředí PowerShell](https://gallery.technet.microsoft.com/scriptcenter/azure-ad-self-service-e31b8aee)

## <a name="how-to-view-password-management-reports-in-the-classic-portal"></a>Postup zobrazení sestav správy hesel portálu classic
Najít sestavy správy hesla, postupujte podle následujících kroků:

1. Klikněte na **služby Active Directory** rozšíření v [portál Azure classic](https://manage.windowsazure.com).
2. Vyberte svůj adresář ze seznamu, který se zobrazí na portálu.
3. Klikněte na **sestavy** kartě.
4. Podívejte se do části **protokoly aktivity** části.
5. Vyberte buď **aktivita resetování hesla** sestavy nebo **registrace aktivita resetování hesla** sestavy.

## <a name="view-password-reset-registration-activity-in-the-classic-portal"></a>Resetování hesla zobrazení činnost registrace na portálu classic
Sestava aktivit registrace resetování hesla zobrazí všechny heslo pro resetování registrace, které mají ve vaší organizaci došlo k chybě.  Registraci k resetování hesla se zobrazí v této sestavě pro každý uživatel, který úspěšně zaregistrovala informace o ověřování v heslo resetovat portálu pro registraci ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)).

* **Maximální časový rozsah**: 30 dnů
* **Maximální počet řádků**: 75 000
* **Zaváděná**: Ano, pomocí souboru CSV

### <a name="description-of-report-columns"></a>Popis sloupce sestavy
Následující seznam popisuje všechny sloupce sestavy podrobně:

* **Uživatel** – uživatel, který se pokusil heslo resetovat operace Registrování.
* **Role** – role uživatele v adresáři.
* **Datum a čas** – datum a čas pokus.
* **Data registrovaná** – registrace pro resetování jaká data ověřování uživatele zadali během heslo.

### <a name="description-of-report-values"></a>Popis hodnoty sestavy
Následující tabulka popisuje různé hodnoty pro každý sloupec povolená:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Data zaregistrován |**Alternativní e-mailu** – uživatel používá k ověření alternativní e-mailu nebo ověřování e-mailu<p><p>**Telefon do kanceláře**– telefonní číslo do kanceláře uživatel použitý k ověření<p>**Mobilní telefon** -uživatele použít mobilní telefon nebo telefon pro ověření k ověření<p>**Bezpečnostní otázky** – uživatel používá k ověření bezpečnostních otázek<p>**Libovolnou kombinaci výše (např. alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje které dvě metody uživatel použitý k ověření žádost o resetování svého hesla. |

## <a name="view-password-reset-activity-in-the-classic-portal"></a>Aktivita portálu classic resetování hesla zobrazení
Tato sestava zobrazí že všechny heslo resetovat pokusy, k nimž došlo ve vaší organizaci.

* **Maximální časový rozsah**: 30 dnů
* **Maximální počet řádků**: 75 000
* **Zaváděná**: Ano, pomocí souboru CSV

### <a name="description-of-report-columns"></a>Popis sloupce sestavy
Následující seznam popisuje všechny sloupce sestavy podrobně:

1. **Uživatel** – uživatel, který se pokusil heslo resetovat operace (podle pole ID uživatele zadané při přístupu uživatele k resetování hesla).
2. **Role** – role uživatele v adresáři.
3. **Datum a čas** – datum a čas pokus.
4. **Metodu nebo metody použité** – resetovat jaké metody ověřování, které uživatel používá pro tuto operaci.
5. **Výsledek** – konečný výsledek heslo resetovat operaci.
6. **Podrobnosti o** – podrobnosti o proč resetovat heslo, které jsou výsledkem hodnota neodpovídala.  Také zahrnuje všechny kroky zmírňující rizika, může trvat řešení k neočekávané chybě.

### <a name="description-of-report-values"></a>Popis hodnoty sestavy
Následující tabulka popisuje různé hodnoty pro každý sloupec povolená:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Metody |**Alternativní e-mailu** – uživatel používá k ověření alternativní e-mailu nebo ověřování e-mailu<p>**Telefon do kanceláře** – telefonní číslo do kanceláře uživatel použitý k ověření<p>**Mobilní telefon** – uživatel použít mobilní telefon nebo telefon pro ověření k ověření<p>**Bezpečnostní otázky** – uživatel používá k ověření bezpečnostních otázek<p>**Libovolnou kombinaci výše (např. alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje které dvě metody uživatel použitý k ověření žádost o resetování svého hesla. |
| výsledek |**Opuštění** – uživatel spustil resetování hesla, ale pak zastavena polovině vzdálenosti prostřednictvím bez dokončení<p>**Blokované** – účet uživatele zabránily používat kvůli pokusu o resetování hesla na stránku resetování hesla nebo jedno heslo resetovat bránu příliš mnohokrát za období 24 hodin<p>**Zrušena** – uživatel spustil resetování hesla, ale pak kliknutí na tlačítko Storno zrušit součást způsobem pomocí relace <p>**Kontaktovat správce** – uživatele došlo k potížím při jeho relaci, která mu nebylo možné přeložit, tak uživatel klikli na odkaz "Obraťte se na správce" namísto dokončení heslo resetovat toku<p>**Se nezdařilo** – uživatele se nepodařilo resetovat heslo, pravděpodobně, protože uživatel nebyl nakonfigurován na použití funkce (například žádná licence, chybí informace o ověřování, heslo spravovat místní ale zpětný zápis je vypnutý).<p>**Úspěšné** – resetování hesla byla úspěšná. |
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
| Uživatel zrušil před předáním metod vyžaduje ověřování |Zrušena |
| Uživatel zrušil před odesláním nové heslo |Zrušena |
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

## <a name="next-steps"></a>Další kroky
Níže naleznete odkazy na všechny stránky dokumentace k resetování hesel služby Azure AD:

* **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account).
* [**Jak to funguje**](active-directory-passwords-how-it-works.md) – Přečtěte si o šesti různých komponentách služby a o tom, jaké mají funkce.
* [**Začínáme** ](active-directory-passwords-getting-started.md) – zjistěte, jak umožnit uživatelům resetovat a změnit hesla cloudu nebo místně
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přečtěte si, jak můžete přizpůsobit vzhled a funkce služby potřebám své organizace.
* [**Osvědčené postupy**](active-directory-passwords-best-practices.md) – Přečtěte si, jak můžete ve své organizaci rychle nasazovat a efektivně spravovat hesla.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md)  – Získejte odpovědi na časté otázky.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Přečtěte si, jak můžete rychle řešit potíže se službou.
* [**Další informace**](active-directory-passwords-learn-more.md) – Prostudujte si podrobné technické informace o tom, jak služba funguje.
