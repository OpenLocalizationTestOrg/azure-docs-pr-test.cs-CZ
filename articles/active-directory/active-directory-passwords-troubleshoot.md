---
title: "Řešení potíží: Azure AD SSPR | Microsoft Docs"
description: "Řešení problémů s Azure AD samoobslužného resetování hesel"
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
ms.date: 08/08/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 361ef5f37e4e9de83d3f9d5a8e5177d186fe86d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-self-service-password-reset"></a>Jak resetování hesla pomocí samoobslužné služby tootroubleshoot

Pokud máte problémy s samoobslužné resetování hesla, hello položky, které následují vám mohou pomoci tooget věcí práce rychle.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-may-see"></a>Řešení chyb resetování hesla pomocí samoobslužné služby, které mohou se zobrazit uživatele

| Chyba | Podrobnosti | Technické podrobnosti |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože váš správce zakázal resetování hesla pro vaši organizaci. Neexistuje žádná další akce může trvat tooresolve této situaci. Prosím obraťte se na správce a požádejte je tooenable tuto funkci. toolearn víc, přečtěte si [nápovědy, zapomenuté heslo Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Zjistili jsme, že nebylo správce povoleno vytvoření nového hesla. Obraťte se na správce a požádejte je tooenable resetování hesla pro vaši organizaci. |
| WritebackNotEnabled = 10 |Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože správce nepovolil nezbytné služby pro vaši organizaci. Neexistuje žádná další akce může trvat tooresolve této situaci. Prosím obraťte se na správce a požádejte je toocheck konfigurace vaší organizace. toolearn Další informace o této potřebné služby, přečtěte si [konfigurace zpětný zápis hesla](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-writeback#configuring-password-writeback). | SSPR_0010: Zjistili jsme, že nebylo povoleno zpětný zápis hesla. Obraťte se na správce a požádejte je tooenable zpětný zápis hesla. |
| SsprNotEnabledInUserPolicy = 11 | Je nám líto  <br> V tuto chvíli nelze obnovit heslo, protože váš správce nenakonfiguroval resetování hesla pro vaši organizaci. Neexistuje žádná další akce může trvat tooresolve této situaci. Obraťte se na správce a požádejte je tooconfigure resetování hesla. toolearn Další informace o heslo resetovat konfigurace pro čtení hello článku [rychlý start: resetování hesla pomocí samoobslužné služby Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Vaše organizace nedefinoval zásady resetování hesel. Obraťte se na správce a požádejte je toodefine zásady resetování hesel. |
| UserNotLicensed = 12 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože požadované licence chybí z vaší organizace. Neexistuje žádná další akce může trvat tooresolve této situaci. Obraťte se na správce a požádejte je toocheck přiřazení licence. toolearn Další informace o licencování najdete článku hello [resetovat Licensing požadavky pro hesla pomocí samoobslužné služby Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Vaše organizace nemá resetování hesla nezbytné tooperform hello požadované licence. Obraťte se na správce a požádejte je tooreview přiřazením licencí. |
| UserNotMemberOfScopedAccessGroup = 13 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože váš správce nenakonfiguroval resetování hesla toouse vašeho účtu. Neexistuje žádná další akce může trvat tooresolve této situaci. Prosím obraťte se na správce a požádejte je tooconfigure váš účet pro resetování hesla. Další informace o konfiguraci účtu pro resetování hesla, přečtěte si článek hello toolearn [resetování hesla pro uživatele zavádět](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: Nejste členem skupiny povolen pro resetování hesla. Obraťte na správce a žádosti o toobe přidané toohello. |
| UserNotProperlyConfigured = 14 | Je nám líto <br> V tuto chvíli nelze obnovit heslo, protože chybí požadované informace z vašeho účtu. Neexistuje žádná další akce může trvat tooresolve této situaci. Obraťte se jste správce a požádejte je tooreset heslo pro vás. Až budete mít účet pro přístup k tooyour dozvíte, jak zaregistrovat hello nezbytné informace podle hello kroků v článku hello [zaregistrovat pro resetování hesla pomocí samoobslužné služby](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-reset-register). | SSPR_0014: Další bezpečnostní údaje potřebné tooreset je jím vaše heslo. tooproceed, obraťte se na správce a požádejte je tooreset heslo. Až budete mít účet pro přístup k tooyour můžete registrovat další bezpečnostní údaje na adrese https://aka.ms/ssprsetup. Správce můžete přidat další bezpečnostní údaje tooyour účtu pomocí následujících kroků hello v [sady a data čtení ověřování pro resetování hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Je nám líto <br> Nemůžeme resetovat heslo v tuto chvíli z důvodu problému s konfigurací resetování hesla vaší organizace. Neexistuje žádná další akce může trvat tooresolve této situaci. Obraťte se na správce a požádejte je tooinvestigate. Další informace o potenciální problém hello toolearn, přečtěte si článek hello [řešení zpětného zápisu hesla](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Jsme se tooreset nelze z důvodu tooan došlo k chybě v konfiguraci místní heslo. Obraťte se na správce a požádejte je tooinvestigate. |
| OnPremisesConnectivityError = 30 | Je nám líto <br> Nemůžeme resetovat heslo v tuto chvíli kvůli organizace tooyour problémy s připojením. Neexistuje žádná akce tootake nyní, ale může být hello problém vyřešen, pokud to zkusíte znovu později. Pokud hello potíže potrvají, obraťte se na správce a požádejte je tooinvestigate. číst informace o problémy s připojením k toolearn [připojení k řešení potíží s zpětný zápis hesla](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Nemůžeme resetovat heslo z důvodu tooa nízký připojení s místním prostředí. Obraťte se na správce a požádejte je tooinvestigate.|


## <a name="troubleshoot-password-reset-configuration-in-hello-azure-portal"></a>Řešení potíží s konfigurace resetování hesla v hello portálu Azure

| **Chyba** | Řešení |
| --- | --- |
| Nevidím hello **resetování hesla** části v rámci Azure AD v hello portálu Azure | To může nastat, když máte Azure AD Premium nebo Basic přiřazenou licenci toohello správce provádění hello operaci. <br> To lze vyřešit přiřazení správce účtu toohello licence dotyčném pomocí článku hello [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Možnost konkrétní konfigurace se nezobrazí | Mnoho prvků hello uživatelského rozhraní jsou skryté. dokud nebude potřeba. Zkuste povolit všechny možnosti hello chcete toosee. |
| Hello se nezobrazí **integrace v místním** karta | Tato možnost jenom se zobrazí, pokud máte stáhnout Azure AD Connect a nakonfigurovat zpětný zápis hesla. Další informace o tomto tématu najdete v článku hello [Začínáme s Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Řešení potíží s reporting resetování hesla

| **Chyba** | Řešení |
| --- | --- |
| Všechny typy aktivit správy hesla se zobrazí v hello se nezobrazí **Samoobslužná správa hesel** kategorie události auditu | To může nastat, když máte Azure AD Premium nebo Basic přiřazenou licenci toohello správce provádění hello operaci. <br> To lze vyřešit přiřazení správce účtu toohello licence dotyčném pomocí článku hello [přiřadit, ověření a vyřešit problémy s licencemi] |
| Registrace uživatele zobrazit více než jednou. | Aktuálně když se uživatel zaregistruje, jsme aktuálně protokolu každé jednotlivé část dat, která je registrována jako samostatná událost. <br> Pokud chcete tato data tooaggregate, si můžete stáhnout hello sestavy a otevřete hello dat jako kontingenční tabulku v aplikaci excel toohave větší flexibilitu.

## <a name="troubleshoot-password-reset-registration-portal"></a>Řešení potíží s registrační portál pro resetování hesla

| **Chyba** | Řešení |
| --- | --- |
| Adresář není povolen pro resetování hesla **správce nepovolil toouse můžete tuto funkci** | Přepínač hello **samoobslužném resetování hesla služby povolena** příznak příliš**skupinu** nebo **každý uživatel** a klikněte na tlačítko **uložit** |
| Uživatel nemá Azure AD Premium nebo Basic přiřazenou licenci **správce nepovolil toouse můžete tuto funkci** | To může nastat, když máte Azure AD Premium nebo Basic přiřazenou licenci toohello správce provádění hello operaci. <br> To lze vyřešit přiřazení správce účtu toohello licence dotyčném pomocí článku hello [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Chyba při zpracování žádosti | To může být způsobeno řadu problémů, ale obecně tato chyba je způsobená buď problém se službou výpadku nebo konfigurace. Pokud se zobrazí tato chyba a jeho je ovlivňující vaši firmu, kontaktujte podporu Microsoftu o další pomoc. |

## <a name="troubleshoot-password-reset-portal"></a>Řešení potíží s portálu pro resetování hesel

| **Chyba** | Řešení |
| --- | --- |
| Adresář není povolen pro resetování hesla. | Přepínač hello **samoobslužném resetování hesla služby povolena** příznak příliš**skupinu** nebo **každý uživatel** a klikněte na tlačítko **uložit** |
| Uživatel nemá Azure AD Premium nebo Basic přiřazenou licenci | To může nastat, když máte Azure AD Premium nebo Basic přiřazenou licenci toohello správce provádění hello operaci. <br> To lze vyřešit přiřazení správce účtu toohello licence dotyčném pomocí článku hello [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Adresář je povolen pro resetování hesla, ale uživatel má chybí nebo je vytvořen špatný informace o ověřování | Zajistěte, aby že tento uživatel má ve správném formátu kontaktní údaje na soubor v adresáři hello než budete pokračovat. Další informace o tomto tématu najdete v článku hello [dat používá Azure AD funkce samoobslužného resetování hesla](active-directory-passwords-data.md). |
| Adresář je povolen pro resetování hesla, ale uživatel se musí jedna část kontaktní údaje na soubor zásad nastavena toorequire dva kroky pro ověření | Ujistěte se, tento uživatel má alespoň dva způsoby kontaktování správně nakonfigurované (Příklad: mobilní telefon **a** Telefon do kanceláře) než budete pokračovat. |
| Adresář je povolen pro resetování hesla a správně nakonfigurován, ale uživatel se nemůže toobe kontaktovat | To může být výsledek hello dočasné chybě nebo nesprávné kontaktní data, která jsme se nezdařilo správně zjistit. <br> <br> Pokud uživatel hello čeká 10 sekund, zkuste to znovu a zobrazí se odkaz "obraťte se na správce". Zkuste to znovu kliknutím opakuje hello volání, zatímco kliknutím na tlačítko "obraťte se na správce" odešle tooadministrators e-mailu formulář požadování že toobe provést pro tento uživatelský účet pro vytvoření nového hesla. |
| Uživatel obdrží nikdy resetování hesla hello SMS nebo telefonní hovor | To může být výsledek vytvořen špatný telefonní číslo v adresáři hello hello. Zkontrolujte, zda text hello telefonní číslo je ve formátu hello "+ xxxyyyzzzzXeeee kopie". <br> <br> Resetování hesla nepodporuje rozšíření, i v případě, že zadáváte jednu v adresáři hello (se budou před samotným odstraněny hello volání předáno). Zkuste použít číslem bez přípony, nebo integraci hello rozšíření do hello telefonní číslo ve vašem Ústřednou. |
| Uživatel nikdy obdrží e-mail pro resetování hesla | Hello Nejčastější příčinou tohoto problému je, že zprávy hello je odmítnut filtr proti spamu. Zkontrolujte nevyžádané pošty, složka nevyžádanou poštou nebo odstraněné položky hello e-mailu. <br> Ujistěte se také, že při kontrole hello správná e-mailová zpráva hello. |
| Nastavení zásad resetování hesel, ale pokud účet správce používá resetování hesla, není použita zásad | Spravuje Microsoft a zásad tooensure hello nejvyšší úroveň zabezpečení pro resetování hesla správce hello ovládací prvky. |
| Uživateli zabránit v pokusu o vytvoření nového příliš mnohokrát za den | Můžeme implementovat, automatické omezení mechanismus tooblock uživatelé z pokus tooreset hesla příliš mnohokrát v krátké době. V takovém případě: <br> 1. Uživatel se pokusí toovalidate telefonní číslo 5krát za jednu hodinu. <br> 2. Uživatel se pokusí toouse hello bezpečnostní otázky brány 5krát za jednu hodinu. <br> 3. Uživatel se pokusí tooreset heslo pro hello stejným uživatelským účtem 5krát za jednu hodinu. <br> toofix, vyzvat uživatele toowait hello 24 hodin od posledního pokusu o hello a hello uživatele se pak být schopný tooreset své heslo. |
| Uživateli se zobrazí k chybě při ověření svého telefonního čísla | K této chybě dojde, když hello telefonní číslo, zadané neodpovídá hello telefonního čísla na soubor. Zkontrolujte, zda text hello uživatel zadá hello úplné telefonní číslo, včetně kódu oblasti a země, při pokusu o toouse metody založené na telefonu pro resetování hesla. |
| Chyba při zpracování žádosti | To může být způsobeno řadu problémů, ale obecně tato chyba je způsobená buď problém se službou výpadku nebo konfigurace. Pokud se zobrazí tato chyba a jeho je ovlivňující vaši firmu, kontaktujte podporu Microsoftu o další pomoc. |

## <a name="troubleshoot-password-writeback"></a>Řešení potíží s zpětný zápis hesla

| **Chyba** | Řešení |
| --- | --- |
| Službu resetování hesla se nespustí místně s chybou 6800 v protokolu událostí aplikace hello Azure AD Connect počítače. <br> <br> Po synchronizaci registrace, federovaný nebo hodnoty hash hesla uživatele nelze resetovat vlastní hesla. | Pokud je povolen zpětný zápis hesla, hello synchronizační modul volá hello zpětný zápis knihovny tooperform hello konfigurace (registrace) rozhovoru toohello cloudové registrace služby. Všechny chyby došlo během registrace nebo při spouštění hello WCF koncový bod pro zpětný zápis hesla výsledky v chyby v protokolu událostí hello v protokolu událostí vašeho počítače Azure AD Connect. <br> <br> Při restartování služby ADSync Pokud bylo nakonfigurované zpětný zápis, koncový bod WCF hello je spuštění. Ale pokud hello spuštění koncového bodu hello selže, jsme bude protokolu událostí 6800 a umožní spuštění služby synchronizace hello. Přítomnost Tato událost znamená, že nebyl zahájen tohoto koncového bodu hello zpětný zápis hesla. Podrobnosti protokolu událostí pro tuto událost (6800) společně s položky protokolu událostí vygenerujte PasswordResetService součást označuje, proč nebylo možné spustit koncový bod hello. Přečtěte si tyto chyby v protokolu událostí a zkuste to toorestart hello Azure AD Connect, pokud zpětný zápis hesla stále nepracuje. Pokud hello potíže potrvají, zkuste toodisable a znovu povolte zpětný zápis hesla.
| Když se uživatel pokusí tooreset heslo nebo odemknout účet s povolen zpětný zápis hesla, hello operace se nezdaří. <br> <br> Kromě toho se zobrazí události v obsahující protokolu událostí hello Azure AD Connect: "synchronizačního modulu vrátila chyba hr = 800700CE, zpráva = hello název souboru nebo rozšíření je příliš dlouhá" po operace odemknutí hello dojde. | Najděte účet služby Active Directory hello Azure AD Connect a resetovat hello heslo toocontain delší než 127 znaků. Synchronizační služba poté otevřete z nabídky Start hello. Přejděte tooConnectors a najděte hello konektor služby Active Directory. Vyberte ho a klikněte na položku Vlastnosti. Přejděte toohello přihlašovací údaje a zadejte nové heslo hello. Vyberte stránku hello OK tooclose. |
| Na hello poslední krok hello procesu instalace Azure AD Connect zobrazí chyba oznamující, že nejde nakonfigurovat zpětný zápis hesla. <br> <br> Hello protokolu událostí aplikace Azure AD Connect obsahuje chybu 32009 s textem "Chyba při získávání auth token". | K této chybě dojde v hello následujících dvou případů:<br> <br> a. Zadali jste nesprávné heslo pro účet globálního správce hello zadaný od začátku hello hello procesu instalace Azure AD Connect.<br> b. Pokusili jste toouse federované uživatele pro účet globálního správce hello zadaný od začátku hello procesu instalace hello Azure AD Connect.<br> <br> toofix tato chyba, zkontrolujte, zda nepoužíváte federovaný účet globálního správce hello jste zadali na hello začátku procesu instalace hello Azure AD Connect a že zadané hello heslo je správné. |
| Protokol událostí počítač Hello Azure AD Connect obsahuje chyby 32002 vyvolané hello PasswordResetService. <br> <br> přečte Hello Chyba: "Chyba při připojování tooServiceBus, zprostředkovatele tokenu hello je nelze tooprovide token zabezpečení..." | Vaše místní prostředí není možné tooconnect koncový bod sběrnice služby toohello v cloudu hello. Tato chyba je obvykle způsobena blokování konkrétní port pro odchozí připojení tooa nebo webovou adresu pravidlo brány firewall. V tématu [požadavky na síťovou](active-directory-passwords-how-it-works.md#network-requirements) Další informace. Po aktualizaci těchto pravidel restartování hello Azure AD Connect počítač a zpětný zápis hesla by měla začít pracovat znovu. |
| Po synchronizaci pracovní pro některé čas, federovaný nebo hodnoty hash hesla uživatele nelze resetovat vlastní hesla. | Ve výjimečných případech hello zpětný zápis hesla služby nezdaří toorestart při restartování Azure AD Connect. V těchto případech nejdřív zkontrolujte, zda se zobrazí zpětný zápis hesla toobe s povoleným v místním prostředí To lze provést pomocí Průvodce hello Azure AD Connect nebo prostředí powershell (HowTos najdete v části výše). Pokud funkce hello se zobrazí toobe povoleno, zkuste povolení nebo zakázání hello funkci znovu buď prostřednictvím hello uživatelského rozhraní nebo Powershellu. Pokud to nepomůže, zkuste úplně odinstalovat a znova nainstalovat Azure AD Connect. |
| Federovaný nebo hodnoty hash hesla synchronizovat uživatelé, kteří se pokusí tooreset hesel najdete v části k chybě po odeslání hello heslo označující, že byl problém služby. <br ><br> Kromě toho toothis, během heslo operace resetování, mohou se zobrazit, že chybu týkající se agenta pro správu byl odepřen přístup v protokolech událostí v místním. | Pokud se zobrazí tyto chyby v protokolu událostí, potvrďte, že tento hello účet AD MA, (který byl zadán v Průvodci hello v době konfigurace hello) má hello potřebná oprávnění pro zpětný zápis hesla. <br> <br> **Jakmile toto oprávnění je zadána, může trvat až hodinu too1 hello oprávnění tootrickle prostřednictvím sdprop úlohy na pozadí na hello řadiče domény.** <br> <br> Pro resetování hesla toowork, potřebuje oprávnění hello toobe razítkem na hello popisovač zabezpečení objektu hello uživatele, jejichž hesla je obnovena. Dokud nebude tato oprávnění se zobrazí na objekt uživatele hello, resetování hesla toofail pokračuje byl odepřen přístup. |
| Federovaný nebo hodnoty hash hesla synchronizovat uživatelé, kteří se pokusí tooreset hesel najdete v části k chybě po odeslání hello heslo označující, že byl problém služby. <br> <br> Kromě toothis, během heslo operace resetování, mohou se zobrazit chybu v protokolech událostí z hello indikující chybu "Objekt nelze nalézt" služba Azure AD Connect. | Tato chyba obvykle označuje, že hello synchronizační modul je nelze toofind buď hello objekt uživatele v prostoru konektoru AAD hello nebo hello propojené MV nebo AD konektoru místo objekt. <br> <br> tootroubleshoot, zajistěte, aby tento uživatel hello skutečně synchronizovaných z místní tooAAD prostřednictvím hello aktuální instance služby Azure AD Connect a zkontrolovat stav hello hello objektů v hello prostor konektoru a více hodnot. Potvrďte, že tento objekt hello AD CS je konektor objektu MV toohello prostřednictvím hello "Microsoft.InfromADUserAccountEnabled.xxx" pravidlo.|
| Federovaný nebo hodnoty hash hesla synchronizovat uživatelé, kteří se pokusí tooreset hesel najdete v části k chybě po odeslání hello heslo označující, že byl problém služby. <br> <br> Kromě toho toothis, během heslo operace resetování, mohou se zobrazit chybu v protokolech událostí ze služby Azure AD Connect hello označující "Nalezeno více odpovídajících" Chyba. | To znamená, že synchronizační modul hello zjistil, že je tento objekt hello MV připojené toomore než jeden objektů služby AD CS prostřednictvím hello "Microsoft.InfromADUserAccountEnabled.xxx". To znamená, že tento uživatel hello má aktivovaný účet ve více než jedné doménové struktuře. **Tento scénář není podporován pro zpětný zápis hesla.** |
| Operace s heslem se nezdaří s chybou konfigurace. obsahuje Hello protokolu událostí aplikace <br> <br> Azure AD Connect chyba 6329 s textem: 0x8023061f (hello operace se nezdařila, protože tento Agent pro správu není povolená synchronizace hesel.) | Tato akce proběhne, že pokud je změněné tooadd hello konfiguraci služby Azure AD Connect nový AD doménové struktury (nebo tooremove a opětovně existující doménové struktury) po hello funkce je již povolen zpětný zápis hesla. Operace s heslem pro uživatele v například nově přidaná selhání doménové struktury. toofix hello problém, zakažte a znovu povolit funkci zpětného zápisu hesla hello až dokončíte změny konfigurace doménové struktury hello. |

## <a name="password-writeback-event-log-error-codes"></a>Kódy chyb v protokolu událostí zpětný zápis hesla

Osvědčeným postupem při řešení potíží s zpětný zápis hesla je tooinspect, který do protokolu událostí aplikace na počítači pro Azure AD Connect. Tento protokol událostí neobsahuje události ze dvou zdrojů, které vás zajímají pro zpětný zápis hesla. Zdroj PasswordResetService Hello popisuje operace a problémy související toohello operace zpětného zápisu hesla. Zdroj ADSync Hello popisuje operace a problémy související toosetting hesla v prostředí AD.

### <a name="source-of-event-is-adsync"></a>Zdroj události je ADSync

| Kód | Název/zprávy | Popis |
| --- | --- | --- |
| 6329 | NICHŽ: MMS(4924) 0x80230619 – "omezení zabrání hello změnit heslo nebudou toohello aktuální verze zadaná." | K této události dojde, když se pokusí hello zpětný zápis hesla služby tooset heslo na vaše místní adresář, který nesplňuje hello stáří hesla, historie, složitost nebo filtrování požadavky domény hello. <br> <br> Pokud máte minimální heslo stáří a v nedávné době změnili heslo hello v rámci dané okno času, nejste možné toochange hello heslo znovu dokud nedosáhne hello zadaný stáří ve vaší doméně. Pro účely testování musí být minimální věk nastavená too0. <br> <br> Pokud máte požadavky na heslo historie povolena, pak je nutné vybrat heslo, které nebyl použit v hello poslední dobu N, kde N je historii hesel hello nastavení. Pokud vyberete hesla, která byla použita v hello poslední dobu N, a pak můžete zobrazit informace o selhání v tomto případě. Pro účely testování, je třeba historie nastavit too0. <br> <br> Pokud máte požadavky na složitost hesla, všechny z nich se vynutí při pokusu uživatele hello toochange nebo resetovat heslo. <br> <br> Pokud máte povolené filtry heslo, a uživatel vybere heslo, které nesplňuje hello kritéria filtrování, hello resetovat nebo změnit operace selže. |
| HR 8023042 | Synchronizační modul vrácena chyba hr = 80230402, zpráva = tooget pokusu o objektu se nezdařila, protože existují duplicitní položky s hello stejné ukotvení | Tato událost nastane, když hello stejné id uživatele je povoleno ve více doménách. Například pokud se synchronizuje doménových struktur účtu nebo prostředků a mít hello stejné id uživatele existuje a povolené v každé této chybě může dojít. <br> <br> Tato chyba může vyskytnout, pokud používáte atribut jedinečný anchor (jako alias, nebo hlavní název uživatele) a dva uživatelé sdílet tento stejný atribut ukotvení. <br> <br> tooresolve-li tento problém, ujistěte se, že nemáte žádné duplicitní uživatele v rámci domény a, že používáte atribut ukotvení jedinečný pro každého uživatele. |

### <a name="source-of-event-is-passwordresetservice"></a>Zdroj události je PasswordResetService

| Kód | Název/zprávy | Popis |
| --- | --- | --- |
| 31001 | PasswordResetStart | Tato událost označuje, že hello místní služba zjistila, že žádost o pocházející z cloudu hello hash synchronizované uživatele federované nebo heslo pro resetování hesla. Tato událost je, že hello první událost v každé heslo resetovat operace zpětného zápisu. |
| 31002 | PasswordResetSuccess | Tato událost označuje, že vybraný uživatel nové heslo během operace resetování hesla zjistili jsme, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byla úspěšně zapsána zpět toohello místní AD prostředí. |
| 31003 | PasswordResetFail | Tato událost označuje, že vybraný uživatel heslo a že hesla byly úspěšně přijaty toohello v místním prostředí, ale když jsme se pokusili tooset hello heslo v místním prostředí AD hello, došlo k selhání. To může dojít z několika důvodů: <br> <br> heslo uživatele Hello nesplňuje hello stáří, historie, složitost, nebo filtrovat požadavky pro doménu hello. Zkuste úplně nové heslo tooresolve to. <br> <br> Hello účtu služby MA nemá hello příslušná oprávnění tooset hello nové heslo na dotyčném hello uživatelský účet. <br> <br> Hello uživatelský účet je v chráněné skupině, například domény nebo enterprise admins, která zakáže heslo množinové operace. |
| 31004 | OnboardingEventStart | Tato událost v případě povolení zpětného zápisu hesla službou Azure AD Connect a určuje, že jsme spuštěné registrace vaší organizace toohello zpětný zápis hesla webové služby. |
| 31005 | OnboardingEventSuccess | Tato událost ukazuje hello procesu registrace byla úspěšná a že funkce zpětný zápis hesla je připraven toouse. |
| 31006 | ChangePasswordStart | Tato událost označuje, že hello místní služby byla nalezena žádost o změnu hesla pro federované nebo heslo uživatele hash synchronizované pocházející z cloudu hello. Tato událost je hello první událost v každé operace zpětného zápisu změny hesla. |
| 31007 | ChangePasswordSuccess | Tato událost označuje, že uživatel vybrané nové heslo během operace změny hesla zjistili jsme, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byla úspěšně zapsána zpět toohello místní AD prostředí. |
| 31008 | ChangePasswordFail | Tato událost označuje, že vybraný uživatel heslo a že hesla byly úspěšně přijaty toohello v místním prostředí, ale když jsme se pokusili tooset hello heslo v místním prostředí AD hello, došlo k selhání. To může dojít z několika důvodů: <br> <br> heslo uživatele Hello nesplňuje hello stáří, historie, složitost, nebo filtrovat požadavky pro doménu hello. Zkuste úplně nové heslo tooresolve to. <br> <br> Hello účtu služby MA nemá hello příslušná oprávnění tooset hello nové heslo na dotyčném hello uživatelský účet. <br> <br> Hello uživatelský účet je v chráněné skupině, například domény nebo enterprise admins, která zakáže heslo množinové operace. |
| 31009 | ResetUserPasswordByAdminStart | Hello místní službu zjistil, že žádost o pocházející z hello správce jménem uživatele hash synchronizované uživatele federované nebo heslo pro resetování hesla. Tato událost je hello první událost v každé operace zpětného zápisu resetování hesla spouštěná správce. |
| 31010 | ResetUserPasswordByAdminSuccess | Dobrý den, správce vybraná během operace resetování hesla správce spouštěná nové heslo, zjistili jsme, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byla úspěšně zapsána zpět toohello místní AD prostředí. |
| 31011 | ResetUserPasswordByAdminFail | Dobrý den, správce vybrané heslo jménem uživatele a toto heslo byly úspěšně přijaty toohello v místním prostředí, ale když jsme se pokusili tooset hello hesla v prostředí hello místní AD, došlo k selhání. To může dojít z několika důvodů: <br> <br> heslo uživatele Hello nesplňuje hello stáří, historie, složitost, nebo filtrovat požadavky pro doménu hello. Zkuste úplně nové heslo tooresolve to. <br> <br> Hello účtu služby MA nemá hello příslušná oprávnění tooset hello nové heslo na dotyčném hello uživatelský účet. <br> <br> Hello uživatelský účet je v chráněné skupině, například domény nebo enterprise admins, která zakáže heslo množinové operace. |
| 31012 | OffboardingEventStart | Tato událost dochází, pokud zakážete zpětný zápis hesla s Azure AD Connect a určuje, že jsme spuštěné odpojení vaší organizace toohello zpětný zápis hesla webové služby. |
| 31013| OffboardingEventSuccess| Tato událost ukazuje hello odpojení procesu bylo úspěšné a že schopností zpětný zápis hesla se úspěšně vypnul. |
| 31014| OffboardingEventFail| Tato událost označuje, že proces odpojení hello nebyla úspěšná. To může být kvůli chybě oprávnění tooa na hello cloudové nebo místní účet správce zadané během konfigurace, nebo proto, že se pokoušíte toouse globální správce federované cloudu při zakazování zpětný zápis hesla. toofix tento, zaškrtněte příslušná oprávnění pro správu a nejste si pomocí kteréhokoli federovaný účet při konfiguraci funkce hello zpětný zápis hesla.|
| 31015| WriteBackServiceStarted| Tato událost označuje, že hello zpětný zápis hesla služby byl úspěšně spuštěn a je připravený tooaccept heslo správy požadavků z cloudu hello.|
| 31016| WriteBackServiceStopped| Tato událost označuje, zda text hello zpětný zápis hesla služba byla zastavena a všechny žádosti správu hesel z cloudu hello nebude úspěšná.|
| 31017| AuthTokenSuccess| Tato událost označuje, že jsme úspěšně načíst autorizační token pro globálního správce hello zadaný během Azure AD Connect toostart hello odpojení nebo registračního procesu instalace.|
| 31018| KeyPairCreationSuccess| Tato událost označuje, že jsme vytvořili úspěšně hello heslo šifrovací klíč, který je použité tooencrypt hesla z hello cloudu toobe odeslané tooyour místní prostředí.|
| 32000| Neznámé chyby| Tato událost ukazuje na neznámou chybu během operace správy heslo. Podívejte se na text výjimky hello hello události další podrobnosti. Pokud máte problémy, zkuste zakázání a opětovného povolení zpětného zápisu hesla. Pokud to nepomůže, zahrňte kopii protokolu událostí společně s hello pracovníka podpory tooyour zevnitř zadané id pro sledování.|
| 32001| ServiceError| Tato událost označuje došlo k chybě připojení toohello cloudu heslo pro resetování služby a obecně nastane, když hello místní službě se nelze tooconnect toohello heslo resetovat webové služby.|
| 32002| ServiceBusError| Tato událost vyplývá, že došlo k chybě připojení klienta tooyour service bus instance. Toto může nastat, protože v místním prostředí blokujete odchozí připojení. Zkontrolujte tooensure vaší brány firewall povolit připojení přes TCP 443 a toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ a akci opakujte. Pokud stále dochází k potížím, zkuste zakázání a opětovného povolení zpětného zápisu hesla.|
| 32003| InPutValidationError| Tato událost označuje, že tooour web service API hello vstup byl neplatný. Opakujte operaci hello.|
| 32004| DecryptionError| Tato událost označuje, že došlo k chybě při dešifrování hello heslo, které byly přijaty z cloudu hello. Může to být způsobeno dešifrovací klíče neshodu mezi hello cloudové služby a v místním prostředí. tooresolve, zakázat a znovu povolit zpětný zápis hesla v místním prostředí.|
| 32005| ConfigurationError| Během registrace uložíme informace specifické pro klienta v konfiguračním souboru v místním prostředí. Tato událost ukazuje na došlo k chybě při ukládání tento soubor nebo, pokud existuje spuštění hello služby došlo k chybě čtení souboru hello. toofix tento problém, zkuste zakázání a opětovného povolení zpětného zápisu hesla tooforce přepsání tento konfigurační soubor.|
| 32007| OnBoardingConfigUpdateError| Během registrace odešleme data z cloudu toohello hello místní služby pro vytvoření nového hesla. Že se pak zapisovat data soubor v paměti tooan teprve pak ji bude odeslán toohello synchronizační služby toostore tyto informace bezpečně na disku. Tato událost znamená problém s zápis nebo aktualizaci dat v paměti. toofix tento problém, zkuste zakázání a opětovného povolení zpětného zápisu hesla tooforce přepsání této konfigurace.|
| 32008| ValidationError| Tato událost označuje, že jsme obdržel neplatnou odezvu z hello heslo resetovat webové služby. toofix tento problém, zkuste zakázání a opětovného povolení zpětného zápisu hesla.|
| 32009| AuthTokenError| Tato událost znamená, že jsme nelze získat o autorizační token pro účet globálního správce hello zadaný při instalaci Azure AD Connect. Tato chyba může být způsobeno chybné uživatelské jméno nebo heslo zadané pro účet globálního správce hello nebo protože zadaný účet globálního správce hello federovaný. toofix tento problém, znovu spusťte konfigurace s hello opravte uživatelské jméno a heslo a ujistěte se, že správce hello spravovaný účet (jenom pro cloud nebo synchronizovat hesla).|
| 32010| CryptoError| Tato událost označuje, že došlo k chybě při generování hello heslo šifrovací klíč nebo dešifrovací heslo, které dorazí z hello cloudové služby. Tuto chybu pravděpodobně označuje potíže s vaším prostředím. Podívejte se na podrobnosti o hello vaší další toolearn protokolu událostí a tento problém vyřešit. Můžete také zkusit akci znovu povolení a zákaz hello zpětný zápis hesla služby tooresolve to.|
| 32011| OnBoardingServiceError| Tato událost označuje, že hello místní služba nelze správně komunikovat s hello procesu resetování hesla webové služby tooinitiate hello registrace. Může to být způsobeno pravidlo brány firewall nebo potížím při získávání tokenu ověřování pro vašeho klienta. toofix, zkontrolujte, zda nejsou blokování odchozí připojení přes TCP 443 a TCP 9350-9354 nebo toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ a že hello správce účtu AAD používáte tooonboard není federovaný.|
| 32013| OffBoardingError| Tato událost označuje, že hello místní služba nelze správně komunikovat s hello procesu resetování hesla webové služby tooinitiate hello odpojení. Může to být způsobeno pravidlo brány firewall nebo potížím při získávání autorizační token pro vašeho klienta. toofix, zkontrolujte, zda nejsou blokování odchozí připojení přes 443 nebo toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ a že hello správce účtu AAD používáte toooffboard není federovaný.|
| 32014| ServiceBusWarning| Tato událost naznačuje, když jsme instance sběrnice služby tooretry tooconnect tooyour klienta. Za normálních podmínek to by nemělo být žádný problém, ale pokud se zobrazí tato událost mnohokrát, vezměte v úvahu kontrola vaše síťová připojení tooservice sběrnice, zvlášť pokud je to vysokou latencí nebo připojení s malou šířkou pásma.|
| 32015| ReportServiceHealthError| V pořadí toomonitor hello stav služby zpětný zápis hesla odešleme prezenčního signálu data tooour pro vytvoření nového hesla webové služby každých 5 minut. Tato událost označuje, že došlo k chybě při odesílání této webové služby stavu informace back toohello cloudu. Tato informace o stavu nezahrnuje OII nebo PII data a je čistě prezenčního signálu a služby na úrovni basic statistiky tak, aby můžeme poskytnout informace o stavu služby v cloudu hello.|
| 33001| ADUnKnownError| Tato událost ukazuje na to, že došlo k neznámé chybě službou Active Directory, zkontrolujte protokol událostí serveru hello Azure AD Connect pro události se zdrojem ADSync hello Další informace o této chybě.|
| 33002| ADUserNotFoundError| Tato událost označuje, že hello uživatele, který se pokouší tooreset nebo změny, které heslo, nebyl nalezen v hello místního adresáře. Tato situace může nastat, pokud byl uživatel hello odstraněné místní, ale není v cloudu hello, nebo pokud nastane problém se synchronizací. Najdete v protokolech synchronizace a hello poslední několik podrobnosti synchronizace spusťte další informace.|
| 33003| ADMutliMatchError| Když pro vytvoření nového hesla nebo žádost o změnu pochází z cloudu hello, používáme ukotvení cloudu hello zadaný během procesu instalace hello služby Azure AD Connect toodetermine jak toolink, který požadují back tooa uživatele v místním prostředí. Tato událost označuje, že jsme dva uživatele ve vašem místním adresáři s hello stejný atribut ukotvení cloudu. Najdete v protokolech synchronizace a hello poslední několik podrobnosti synchronizace spusťte další informace.|
| 33004| ADPermissionsError| Tato událost označuje, že hello účet agenta pro správu služby nemá hello příslušná oprávnění na hello účet dotyčném tooset nové heslo. Zajistěte, aby byl tento hello MA účet v doménové struktuře hello uživatele resetování a změn hesel oprávnění pro všechny objekty v doménové struktuře hello. Další informace o tom, jak provést toothis, najdete v kroku 4: nastavení hello příslušná oprávnění služby Active Directory.|
| 33005| ADUserAccountDisabled| Tato událost označuje, že jsme se pokusili tooreset nebo změnit heslo pro účet, který byl zakázán, místní. Povolit účet hello a hello operaci opakujte.|
| 33006| ADUserAccountLockedOut| Událost označuje, že jsme pokus tooreset nebo změnit heslo pro účet, který byl uzamčen místně. Uzamčení může dojít, když má uživatel o změnu nebo resetování hesla operaci příliš mnohokrát v krátké době. Odemknout účet hello a hello operaci opakujte.|
| 33007| ADUserIncorrectPassword| Tato událost označuje, že tento uživatel hello zadané nesprávné aktuální heslo při provádění heslo změnit operaci. Zadejte hello správné aktuální heslo a zkuste to znovu.|
| 33008| ADPasswordPolicyError| K této události dojde, když se pokusí hello zpětný zápis hesla služby tooset heslo na vaše místní adresář, který nesplňuje hello stáří hesla, historie, složitost nebo filtrování požadavky domény hello. <br> <br> Pokud máte minimální heslo stáří a v nedávné době změnili heslo hello v rámci dané okno času, nejste možné toochange hello heslo znovu dokud nedosáhne hello zadaný stáří ve vaší doméně. Pro účely testování musí být minimální věk nastavená too0. <br> <br> Pokud máte požadavky na heslo historie povolena, pak je nutné vybrat heslo, které nebyl použit v hello poslední dobu N, kde N je historii hesel hello nastavení. Pokud vyberete hesla, která byla použita v hello poslední dobu N, a pak můžete zobrazit informace o selhání v tomto případě. Pro účely testování, je třeba historie nastavit too0. <br> <br> Pokud máte požadavky na složitost hesla, všechny z nich se vynutí při pokusu uživatele hello toochange nebo resetovat heslo. <br> <br> Pokud máte povolené filtry heslo, a uživatel vybere heslo, které nesplňuje hello kritéria filtrování, hello resetovat nebo změnit operace selže.|
| 33009| ADConfigurationError| Tato událost ukazuje na byl problém zápis back tooyour a hesla místní adresář kvůli tooa konfigurace problém se službou Active Directory. Zkontrolujte protokol událostí aplikace hello Azure AD Connect počítače pro zprávy od hello ADSync service Další informace o jaké došlo k chybě.|


## <a name="troubleshoot-password-writeback-connectivity"></a>Řešení potíží s připojením zpětný zápis hesla

Pokud dochází k přerušení poskytování služeb se součástí hello zpětný zápis hesla služby Azure AD Connect, zde jsou některé rychlé kroky můžete tooresolve provést toto:

* [Restartování hello službu Azure AD Connect Sync](#restart-the-azure-ad-connect-sync-service)
* [Zakažte a znovu povolit funkci zpětného zápisu hesla hello](#disable-and-re-enable-the-password-writeback-feature)
* [Nainstalovat verzi hello nejnovější Azure AD Connect](#install-the-latest-azure-ad-connect-release)
* [Řešení potíží se zpětným zápisem hesla](#troubleshoot-password-writeback)

Obecně platí doporučujeme, můžete ke spouštění těchto kroků v pořadí hello výše toorecover služby nejvíce rychlé způsobem hello.

### <a name="restart-hello-azure-ad-connect-sync-service"></a>Restartování hello službu Azure AD Connect Sync

Restartování hello službu Azure AD Connect Sync může pomoci potíže s připojením k tooresolve nebo jiné přechodné problémy službou hello.

1. Jako správce, klikněte na tlačítko **spustit** na hello serveru se systémem **Azure AD Connect**.
2. Typ **"services.msc"** hello vyhledávacího pole a stiskněte **Enter**.
3. Vyhledejte hello **Microsoft Azure AD Sync** položku.
4. Klikněte pravým tlačítkem na položku služby hello, klikněte na tlačítko **restartujte**a počkat na toocomplete operace hello.

   ![Restartujte službu Azure AD Sync hello][Service Restart]

Tyto kroky znovu navázat připojení s cloudovou službou hello a vyřešte všechny přerušení, které máte. Pokud restartování hello synchronizační služba váš problém nevyřeší, doporučujeme zkuste toodisable a znovu povolit funkci zpětného zápisu hesla hello jako další krok.

### <a name="disable-and-re-enable-hello-password-writeback-feature"></a>Zakažte a znovu povolit funkci zpětného zápisu hesla hello

Zakázání a povolení znovu hello zpětný zápis hesla funkce může pomoci potíže s připojením k tooresolve.

1. Jako správce, otevřete hello **Průvodce konfigurací služby Azure AD Connect**.
2. Na hello **připojit tooAzure AD** dialogové okno, zadejte vaše **přihlašovací údaje globálního správce Azure AD**
3. Na hello **připojit tooAD DS** dialogové okno, zadejte vaše **přihlašovací údaje Správce služby AD Domain Services**.
4. Na hello **Jednoznačná identifikace uživatelů** dialogové okno, klikněte na tlačítko hello **Další** tlačítko.
5. Na hello **volitelné funkce** dialogové okno, zrušte zaškrtnutí políčka hello **zpětný zápis hesla** zaškrtávací políčko.
6. Klikněte na tlačítko **Další** prostřednictvím hello zbývajících dialogových stránkách beze změny jakéhokoli, dokud nezískáte toohello **připraven tooconfigure** stránky.
7. Ujistěte se, že hello nakonfigurovat na stránce se zobrazí hello **možnost zpětný zápis hesla jako zakázané** a pak klikněte na zelenou hello **konfigurace** tlačítko toocommit změny.
8. Na hello **dokončeno** dialogové okno, zrušte výběr hello **synchronizovat** a potom klikněte na **Dokončit** tooclose hello průvodce.
9. Znovu otevřete hello **Průvodce konfigurací služby Azure AD Connect**.
10. **Opakujte kroky 2-8**, s výjimkou si zajistěte **zkontrolujte možnost zpětný zápis hesla hello** na hello **volitelné funkce** obrazovce toore povolit hello služby.

Tyto kroky znovu navázat připojení s naší cloudové služby a vyřešte přerušení, které máte.

Pokud zakázat a znovu povolit funkci zpětného zápisu hesla hello váš problém nevyřeší, doporučujeme vám, zkuste to tooreinstall Azure AD Connect jako další krok.

### <a name="install-hello-latest-azure-ad-connect-release"></a>Nainstalovat verzi hello nejnovější Azure AD Connect

Konfigurace a problémy s připojením mezi naší cloudové služby a místního prostředí AD může vyřešit opětovné instalace Azure AD Connect.

Doporučujeme, tento krok provést až po pokusu o hello první dva kroky popsané výše.

> [!WARNING]
> Pokud jste upravili hello mimo pole synchronizační pravidla, **tyto zálohování před pokračováním upgradu a znovu je ručně, až budete hotovi**.

1. Stáhnout nejnovější verzi Azure AD Connect hello z hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
2. Vzhledem k tomu, že jste již nainstalovali Azure AD Connect, musíte tooperform upgradu tooupdate na místě nejnovější verze toohello instalace Azure AD Connect.
3. Spuštění hello Stáhnout balíček a postupujte podle hello na obrazovce pokyny tooupdate počítači Azure AD Connect.

Tyto kroky znovu navázat připojení s naší cloudové služby a vyřešte všechny přerušení, které máte.

Pokud instalaci hello nejnovější verzi server Azure AD Connect hello váš problém nevyřeší, doporučujeme po instalaci nejnovější verze hello pokus zakázání a opětovné povolení zpětného zápisu hesla v posledním kroku.

## <a name="verify-whether-azure-ad-connect-has-hello-required-permission-for-password-writeback"></a>Ověřte, zda má Azure AD Connect hello vyžaduje oprávnění pro zpětný zápis hesla 
Azure AD Connect vyžaduje AD **resetovat heslo** zpětný zápis hesla tooperform oprávnění. toofind se v případě má Azure AD Connect hello oprávnění pro danou místní AD uživatelský účet, můžete použít funkci efektivní oprávnění Windows hello:

1. Přihlaste se server tooAzure AD Connect a spustit hello **Synchronization Service Manager** (Start → synchronizace Service).
2. V části hello **konektory** vyberte hello místní **AD connector.** a klikněte na tlačítko **vlastnosti**.  
![Efektivní oprávnění – krok 2](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
3. V místním dialogovém okně hello, vyberte hello **připojit doménové struktury Directory tooActive** kartě a zapište hello **uživatelské jméno** vlastnost. Toto je hello AD DS účet používaný službou synchronizací adresářů Azure AD Connect tooperform. Pro zpětný zápis hesla tooperform Azure AD Connect hello AD DS účet musí mít oprávnění resetovat heslo.  
![Efektivní oprávnění – krok 3](./media/active-directory-passwords-troubleshoot/checkpermission02.png)  
4. Přihlášení tooan místní domény řadiče a spusťte hello **Active Directory Users and Computers** aplikace.
5. Klikněte na tlačítko **zobrazení** a zajistěte, aby **upřesňující funkce** je povolena možnost.  
![Efektivní oprávnění – krok 5](./media/active-directory-passwords-troubleshoot/checkpermission03.png)  
6. Vyhledejte hello AD uživatelský účet, který chcete tooverify. Klikněte pravým tlačítkem na účet hello a vyberte **vlastnosti**.  
![Efektivní oprávnění – krok 6](./media/active-directory-passwords-troubleshoot/checkpermission04.png)  
7. V místním dialogovém okně hello, přejděte toohello **zabezpečení** a klikněte na **Upřesnit**.  
![Efektivní oprávnění - krok 7](./media/active-directory-passwords-troubleshoot/checkpermission05.png)  
8. V dialogu automaticky otevírané okno Upřesnit nastavení zabezpečení hello, přejděte toohello **platný přístup** kartě.
9. Klikněte na **vyberte uživatele** a vyberte hello služby AD DS účet používaný službou Azure AD Connect (viz krok 3). Pak klikněte na tlačítko **zobrazit platný přístup**.  
![Efektivní oprávnění - kroku 9](./media/active-directory-passwords-troubleshoot/checkpermission06.png)  
10. Posuňte se dolů a vyhledejte **resetovat heslo**. Pokud položka hello je zaškrtnuto, znamená to, zda text hello AD DS účet má oprávnění tooreset hello heslo hello vybraný účet uživatele AD.  
![Efektivní oprávnění - krok 10](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Fóra pro Azure AD

Pokud máte obecný dotaz týkající se Azure AD a resetování hesla pomocí samoobslužné služby, můžete požádat hello komunity pro pomoc na hello [fóra Azure AD](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Členové komunity hello zahrnují Engineers, správce produktu, MVP a hlavě nehodí Odborníci v oblasti IT.

## <a name="contact-microsoft-support"></a>Obraťte se na podporu společnosti Microsoft

Pokud nemůžete najít hello odpovědí tooan problém našimi týmy podpory jsou vždy k dispozici tooassist, které jste další.

pomůže tooproperly, požádáme zadejte množství podrobností nejdříve při otevírání případu včetně:

* **Obecný popis chyby hello** -co se hello chybu? Jaká byla hello chování, které bylo zaznamenali? Jak jsme reprodukujte hello chyba? Nejdříve zadejte množství podrobností.
* **Stránka** – které stránce jste byli na když jste si všimli hello chyba? Pokud jsou možné tooand snímek uveďte adresu URL hello.
* **Podpora kódu** – jak se vygeneruje, když uživatel hello viděli hello chyba Kód podpory hello? 
    * toofind to reprodukovatelnými hello chybu, klikněte na tlačítko hello podporují kód odkaz v hello dolní části obrazovky hello a odesílání hello podporu pracovníka hello identifikátor GUID, který výsledky.
    ![Najít kód hello podpory v hello dolní části obrazovky hello][Support Code]
    * Pokud jste na stránce bez podpory kódu v dolní části hello, stiskněte klávesu F12 a vyhledejte SID a CID a odesílat tyto pracovníka podpory toohello oba výsledky.
* **Datum, čas a časové pásmo** -uveďte hello přesné datum a čas **s časové pásmo hello** že hello došlo k chybě.
* **ID uživatele** -kdo byl hello uživatele, který viděli hello chyba? (user@contoso.com)
    * Je to federovaného uživatele?
    * Je toto heslo hash synchronizované uživatele?
    * Je to cloudu jenom uživatele?
* **Licencování** -nemá uživatel hello přiřazenou licenci Azure AD Premium nebo Azure AD Basic?
* **Protokol událostí aplikace** – Pokud používáte zpětný zápis hesla a hello chyba je v infrastruktuře je místní kopii protokolu událostí aplikací z server hello Azure AD Connect komprimované vložit při kontaktování podpory.

    

[Service Restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Restartujte službu Azure AD Sync hello"
[Support Code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Podpora kódu se nachází v vpravo okna hello hello dole"

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
