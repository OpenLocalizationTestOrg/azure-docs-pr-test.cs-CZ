---
title: "Azure AD Connect: Řešení potíží s předávací ověřování | Microsoft Docs"
description: "Tento článek popisuje, jak tootroubleshoot předávací ověřování Azure Active Directory (Azure AD)."
services: active-directory
keywords: "Řešení potíží s Azure AD Connect předávací ověřování, nainstalovat službu Active Directory, požadovaných součástí pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 87130952f660762f91b0a34b05287603b565639f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Řešení potíží s předávací ověřování Azure Active Directory

Tento článek vám pomůže najít řešení potíží s informace o běžných problémech týkající se Azure AD předávací ověřování.

>[!IMPORTANT]
>Pokud se setkávají problémů přihlášení uživatele pomocí předávacího ověřování, nemusíte hello funkci zakázat nebo odinstalovat agenty předávací ověřování bez nutnosti jenom pro cloud toofall účet globálního správce zpět na. Další informace o [přidání účtu globálního správce jenom pro cloud](../active-directory-users-create-azure-portal.md). Tento krok je velmi důležité a zajistí, že nezůstanete z vašeho klienta.

## <a name="general-issues"></a>Běžné problémy

### <a name="check-status-of-hello-feature-and-authentication-agents"></a>Zkontrolujte stav funkce hello a ověřování agentů

Ujistěte se, tato funkce předávací ověřování hello je stále **povoleno** na vašeho klienta a hello stav agentů ověřování zobrazuje **Active**a ne **neaktivní**. Stav můžete zkontrolovat pomocí budete toohello **Azure AD Connect** okně hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Azure Centrum pro správu služby Active Directory – okno Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Centrum pro správu – okno předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Zobrazující se uživatelům přihlášení chybové zprávy

Pokud je uživatel hello nelze toosign do aplikace pomocí předávacího ověřování, se může zobrazit jedna z hello následujícím chybám uživatelsky orientovaný na přihlašovací obrazovce hello Azure AD: 

|Chyba|Popis|Řešení
| --- | --- | ---
|AADSTS80001|Nelze tooconnect tooActive adresáře|Zkontrolujte, zda agent servery jsou členové hello stejnou AD doménové struktury jako hello uživatele, jejichž hesla se musí ověřit toobe a budou mít tooconnect tooActive adresáře.  
|AADSTS8002|Vypršel časový limit připojování tooActive adresáře|Zkontrolujte, že služby Active Directory je k dispozici a bude reagovat toorequests od agentů hello tooensure.
|AADSTS80004|uživatelské jméno Hello předán toohello agent byl neplatný|Zkontrolujte, zda uživatel hello se pokouší toosign pomocí hello správné uživatelské jméno.
|AADSTS80005|Ověření došlo k neočekávané výjimku WebException|K přechodné chybě. Opakujte žádost hello. Pokud toofail pokračuje, obraťte se na podporu společnosti Microsoft.
|AADSTS80007|Došlo k chybě komunikuje se službou Active Directory|Zkontrolujte hello agenta protokoly pro další informace a ověřte, že služby Active Directory funguje podle očekávání.

### <a name="sign-in-failure-reasons-on-hello-azure-active-directory-admin-center"></a>Chyba přihlášení důvodů v Centru pro správu Azure Active Directory hello

Při řešení potíží při přihlašování uživatele prohlížení hello začněte [přihlašovací aktivita sestavy](../active-directory-reporting-activity-sign-ins.md) na hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Azure Active Directory Centrum pro správu – sestava přihlášení](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Přejděte příliš**Azure Active Directory** -> **přihlášení** na hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com/) a klikněte na aktivitu přihlášení příslušného uživatele. Vyhledejte hello **kód chyby SIGN-IN** pole. Mapování hello hodnotu tohoto pole důvodem selhání tooa a řešení pomocí hello následující tabulka:

|Kód chyby přihlášení|Důvod selhání přihlášení|Řešení
| --- | --- | ---
| 50144 | Vypršela platnost hesla uživatele pro Active Directory. | Resetování hesla hello uživatele ve vaší místní službě Active Directory.
| 80001 | Není k dispozici žádný ověřovací agent. | Instalace a registrace agenta ověřování.
| 80002 | Vypršel časový limit žádosti o ověření hesla ověřovacího agenta. | Zkontrolujte, jestli je dosažitelný z hello Agent ověřování služby Active Directory.
| 80003 | Ověřovací agent přijal neplatnou odpověď. | Pokud problém hello konzistentně reprodukovatelnou napříč více uživatelů, zkontrolujte konfiguraci služby Active Directory.
| 80004 | V žádosti o přihlášení byl použit nesprávný hlavní název uživatele (UPN). | Požádejte uživatele toosign hello pomocí hello správné uživatelské jméno.
| 80005 | Ověřovací agent: Došlo k chybě. | Došlo k přechodné chybě. Zkuste to znovu později.
| 80007 | Ověřování agenta nelze tooconnect tooActive adresáře. | Zkontrolujte, jestli je dosažitelný z hello Agent ověřování služby Active Directory.
| 80010 | Heslo nelze toodecrypt agenta pro ověřování. | Pokud problém hello konzistentně reprodukovatelnou, nainstalujte a zaregistrujte novou agenta ověřování. A odinstalace hello stávající. 
| 80011 | Ověřování agenta nelze tooretrieve dešifrovací klíč. | Pokud problém hello konzistentně reprodukovatelnou, nainstalujte a zaregistrujte novou agenta ověřování. A odinstalace hello stávající.

## <a name="authentication-agent-installation-issues"></a>Problémy instalace agenta ověřování

### <a name="an-unexpected-error-occurred"></a>Došlo k neočekávané chybě

[Shromažďování protokolů agenta](#collecting-pass-through-authentication-agent-logs) z hello serveru a kontaktujte Microsoft Support s tímto problémem.

## <a name="authentication-agent-registration-issues"></a>Problémy s ověřením agenta registrace

### <a name="registration-of-hello-authentication-agent-failed-due-tooblocked-ports"></a>Registrace hello ověření agenta se nezdařila z důvodu tooblocked porty

Zkontrolujte, na které hello byl nainstalován Agent pro ověřování serveru hello může komunikovat v naší službě uvedené adresy URL a portů [zde](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).

### <a name="registration-of-hello-authentication-agent-failed-due-tootoken-or-account-authorization-errors"></a>Registrace hello ověření agenta se nezdařila z důvodu chyb autorizace tootoken nebo účet

Ujistěte se, že používáte účet globálního správce jenom pro cloud pro Azure AD Connect nebo samostatná Agent ověřování instalace a registrace operace. Je známý problém s povoleným MFA globálního správce účty; vypněte dočasně MFA (pouze toocomplete hello operace) jako alternativní řešení.

### <a name="an-unexpected-error-occurred"></a>Došlo k neočekávané chybě

[Shromažďování protokolů agenta](#collecting-pass-through-authentication-agent-logs) z hello serveru a kontaktujte Microsoft Support s tímto problémem.

## <a name="authentication-agent-uninstallation-issues"></a>Problémy s ověřením agenta odinstalace

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Upozornění při odinstalaci Azure AD Connect

Pokud máte předávací ověřování povoleno na vašeho klienta a opakujte toouninstall Azure AD Connect, zobrazuje hello následující upozornění: "uživatelé nebudou moct toosign v tooAzure AD Pokud máte jiné agenty pro předávací ověřování nainstalováno na jiných serverech."

Zajistěte, aby vaše instalace [vysoké dostupné](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) před odinstalováním Azure AD Connect tooavoid nejnovější přihlášení uživatele.

## <a name="issues-with-enabling-hello-feature"></a>Problémy s povolováním hello funkce

### <a name="enabling-hello-feature-failed-because-there-were-no-authentication-agents-available"></a>Povolení funkce hello se nezdařila, protože nebyly k dispozici žádní agenti ověřování

Je třeba toohave alespoň jeden aktivní Agent ověřování tooenable předávací ověřování na klientovi. Můžete nainstalovat agenta ověřování instalaci Azure AD Connect nebo samostatný Agent ověřování.

### <a name="enabling-hello-feature-failed-due-tooblocked-ports"></a>Povolení funkce hello se nezdařilo z důvodu tooblocked porty

Zkontrolujte, že hello server, na kterém je nainstalován Azure AD Connect může komunikovat v naší službě uvedené adresy URL a portů [zde](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).

### <a name="enabling-hello-feature-failed-due-tootoken-or-account-authorization-errors"></a>Povolení funkce hello se nezdařilo z důvodu chyb autorizace tootoken nebo účet

Ujistěte se, že používáte účet globálního správce jenom pro cloud při povolování funkce hello. Je známý problém s službou Multi-Factor authentication (MFA)-povolit globální správce účtů; vypněte dočasně MFA (pouze toocomplete hello operace) jako alternativní řešení.

## <a name="exchange-activesync-configuration-issues"></a>Problémy s konfigurací protokolu Exchange ActiveSync

Toto jsou hello běžné problémy, pokud nakonfigurujete podporu protokolu Exchange ActiveSync pro předávací ověřování.

### <a name="exchange-powershell-issue"></a>Problém Exchange PowerShell

Pokud se zobrazí hello "**parametr nebyl nalezen odpovídající název parametru 'PerTenantSwitchToESTSEnabled'\.**" Chyba při spuštění hello `Set-OrganizationConfig` Exchange PowerShell příkaz, obraťte se na Microsoft Support.

### <a name="exchange-activesync-not-working"></a>Exchange ActiveSync nepracuje

Konfigurace Hello projeví některé čas tootake – hello časové období, závisí na vašem prostředí. Pokud hello situace přetrvává po dlouhou dobu, obraťte se na Microsoft Support.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Shromažďování protokolů předávací ověřování agenta

V závislosti na typu hello problém, který může mít musíte pro předávací ověřování agenta protokoly toolook na různých místech.

### <a name="authentication-agent-event-logs"></a>Protokoly událostí ověření agenta

Chyby související s toohello ověřování agenta, otevře hello prohlížeče událostí aplikace na serveru hello a zkontrolujte v části **aplikace a služby Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Podrobné analýzy povolte protokol "Relace" hello. Nespouštět hello Agent ověřování s Tento protokol povolen během normálních operací; Používejte pouze pro řešení potíží. obsah protokolu Hello jsou viditelné pouze po zakázání protokolu hello je znovu.

### <a name="detailed-trace-logs"></a>Protokoly podrobné trasování

tootroubleshoot uživatele neúspěšných přihlášení, vyhledejte protokoly trasování v **%programdata%\Microsoft\Azure AD připojit Agent\Trace ověřování\\**. Tyto protokoly obsahovat důvodů, proč přihlášení konkrétního uživatele se nepodařilo pomocí funkce hello předávací ověřování. Tyto chyby jsou také příčin selhání přihlášení namapované toohello uvedené v předchozí hello [tabulky](#sign-in-failure-reasons-on-the-Azure-portal). Následuje příklad položky protokolu:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Popisné informace hello chyby ('1328' v předchozím příkladu hello) získáte otevřením hello příkazový řádek a spuštěné hello následující příkaz (Poznámka: '1328' nahraďte hello skutečné chybové číslo, které se zobrazí v protokolů):

`Net helpmsg 1328`

![Předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Řadič domény

Pokud je povoleno protokolování auditu, další informace naleznete v protokolech zabezpečení hello řadiče domény. Jednoduchý způsob tooquery přihlášení požadavky odeslané agenty předávací ověřování vypadá takto:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

### <a name="performance-monitor-counters"></a>Čítače sledování výkonu

Jiný způsob toomonitor ověřování agentů je čítačů sledování výkonu specifických tootrack na každém serveru, kde je nainstalován Agent ověřování hello. Použití hello následující globální čítače (**# PTA ověřování**, **#PTA se nezdařilo ověření** a **úspěšných ověření #PTA**) a čítače chyb (**Chybám při ověřování # PTA**):

![Průchozí čítačů sledování výkonu ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Předávací ověřování poskytuje vysokou dostupnost, pomocí více ověřování agentů a _není_ Vyrovnávání zatížení. V závislosti na konfiguraci _není_ přijímat všechny agenty ověřování zhruba _rovna_ počet požadavků. Je možné konkrétní Agent ověřování vůbec obdrží žádný provoz.
