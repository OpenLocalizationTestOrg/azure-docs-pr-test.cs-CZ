---
title: "Řešení potíží s Enterprise State Roaming nastavení ve službě Azure Active Directory | Microsoft Docs"
description: "Poskytuje odpovědi toosome otázky správci IT můžou mít o nastavení a synchronizaci dat aplikací."
services: active-directory
keywords: "Enterprise stavu nastavení roamingu windows cloudu, nejčastější dotazy na enterprise stavu roaming"
documentationcenter: 
author: tanning
manager: swadhwa
editor: 
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 4636d016983426e30d696459f54167b8ad2babcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Řešení potíží s Enterprise State Roaming nastavení ve službě Azure Active Directory

Toto téma obsahuje informace o tom, tootroubleshoot a diagnostikovat problémy s Enterprise State Roaming a poskytuje seznam známých problémů.

## <a name="preliminary-steps-for-troubleshooting"></a>Přípravné kroky pro řešení potíží 
Před zahájením řešení potíží s Ověřte, že hello uživatelů a zařízení správně nakonfigurovány a splnění všech požadavků hello Enterprise State Roaming hello zařízení a uživatele hello. 

1. Windows 10 s hello nejnovější aktualizace a minimální verzi 1511 (sestavení operačního systému 10586 nebo novější) je nainstalována na zařízení hello. 
2. Hello zařízení je Azure AD připojený, nebo připojené k doméně a registraci do Azure AD.
3. V portálu Azure Active Directory hello **Enterprise State Roaming** je povolený pro adresář hello **konfigurace** > **zařízení**  >  **Uživatelé se mohou synchronizovat nastavení a dat aplikací Enterprise**. Buď je vybraných všichni uživatelé, nebo uživatel hello je povolena pro synchronizaci prostřednictvím hello vybrali možnost a součástí skupiny zabezpečení hello.
4. předplatné služby Azure Active Directory Premium, který je přiřazen toothem má uživatel Hello.  
5. Po restartování zařízení Hello a hello přihlášení uživatele po Enterprise State Roaming bylo povoleno.

## <a name="information-tooinclude-when-you-need-help"></a>Informace o tooinclude, pokud potřebujete pomoc
Pokud se vám nepovede vyřešit problém s pokyny hello níže, můžete kontaktovat naši pracovníci technické podpory. Při kontaktování je, se doporučuje tooinclude hello následující informace:

- **Obecný popis chyby hello** – jsou zprávy o chybách pohledu hello uživatele? Pokud se žádná chybová zpráva, popisují hello neočekávanému chování, které jste si všimli, podrobně. Jaké funkce jsou povolené pro synchronizaci a co je hello uživatele očekává toosync? Nejsou synchronizuje více funkcí nebo je it izolované tooone?
- **Ovlivněných uživatelů** – pracovní nebo selhání pro jeden nebo více uživateli je synchronizace? Kolik zařízení se podílejí na uživatele? Jsou všechny z nich nesynchronizuje nebo jsou některé z nich, synchronizaci a některé nesynchronizuje?
- **Informace o uživateli hello** – co identita je hello uživatele s využitím toolog toohello zařízení? Jak se v zařízení toohello protokolování hello uživatele? Budou-li součástí vybrané skupiny zabezpečení povolené toosync? 
- **Informace o zařízení hello** – je toto zařízení připojení k Azure AD nebo připojený k doméně? Jaké sestavení je hello zařízení na? Jaké jsou hello nejnovější aktualizace?
- **Datum a čas / časové pásmo** – jak se hello přesné datum a čas, které jste viděli hello chyby (včetně časového pásma hello)?
- Tyto informace včetně pomáhá nám co nejdříve vyřešit problém.

## <a name="troubleshooting-and-diagnosing-issues"></a>Řešení potíží a diagnostice problémů
Tato část nabízí návrhy na postupy tootroubleshoot a diagnostikovat problémy související tooEnterprise State Roaming.

## <a name="verify-sync-and-hello-sync-your-settings-settings-page"></a>Ověření synchronizace a hello "Synchronizace nastavení" Stránka nastavení 

1. Po připojení k vaší počítačích s Windows 10 tooa domény, který je nakonfigurován tooallow Enterprise State Roaming, přihlášení pomocí svého pracovního účtu. Přejděte příliš**nastavení** > **účty** > **vaše nastavení synchronizace** a ověřte, zda synchronizace a hello jednotlivá nastavení a že hello začátek Stránka nastavení Hello označuje, že se synchronizuje pomocí svého pracovního účtu. Potvrďte hello stejný účet se používá jako váš účet pro přihlášení v **nastavení** > **účty** > **vaše údaje**. 
2. Ověřte, že synchronizace funguje napříč více počítačů tím, že některé změny na hello původní počítač, například přesun hello panelu toohello pravé nebo horní straně úvodní obrazovka. Podívejte se na změnu hello rozšířit toohello druhý počítač do pěti minut. 
 - Zamykání a odemykání úvodní obrazovka (Win + L) může pomoct aktivaci synchronizace.
 - Je třeba použít stejný účet pro přihlášení na obou počítačích pro synchronizaci toowork – text hello, protože Enterprise State Roaming je vázané toohello uživatelský účet a hello účet počítače.

**Potenciální problém**: Stránka nastavení hello má hello přepínačů šedě a místo zobrazuje účtu, zobrazí hello text "některými funkcemi Windows, jsou k dispozici pouze pokud používáte účet Microsoft nebo pracovní účet". Tento problém může způsobit pro zařízení, které již byly vytvořeny toobe připojený k doméně a zaregistrované tooAzure AD, ale zařízení hello nebyl úspěšně ověřen tooAzure AD. Možnou příčinou je, že hello zařízení musí být zásada, ale tato aplikace probíhá asynchronně a může být zpožděn několik hodin. tooverify ověřit tento problém, postupujte podle kroků hello v hello toocheck stav registrace zařízení v případě hello.

### <a name="verify-hello-device-registration-status"></a>Ověření stavu registrace zařízení hello
Enterprise State Roaming vyžaduje toobe hello zařízení zaregistrované s Azure AD. I když nejsou specifické tooEnterprise State Roaming, hello pokynů níže může pomoci potvrďte, že hello je zaregistrován klienta Windows 10 a kryptografický otisk, nastavení adresy URL služby Azure AD, NGC stav a další informace.

1.  Příkazový řádek otevřete hello pozastavené. toodo v systému Windows, otevře hello spustit Spouštěče (Win + R) a zadejte tooopen "cmd".
2.  Jakmile hello příkazového řádku je otevřený, zadejte "*/dsregcmd.exe status*".
3.  Očekávaný výstup hello **AzureAdJoined** pole hodnota by měla být "Ano", **WamDefaultSet** pole hodnota by měla být "Ano" a hello **WamDefaultGUID** pole hodnota by měla být Identifikátor GUID s "(AzureAd)" na konci hello.

**Potenciální problém**: **WamDefaultSet** a **AzureAdJoined** mějte hello hodnota pole "Ne", hello zařízení byl připojený k doméně a zaregistrovaná v Azure AD, i zařízení hello není synchronizovaná. Pokud je to zobrazeno, může zařízení hello toowait nutnost použít toobe zásad nebo hello ověřování pro zařízení hello došlo k chybě při připojování tooAzure AD. Hello uživatel může mít toowait několik hodin, než toobe hello zásady použít. Při řešení potíží může zahrnovat automatickou registraci opakování podpisový odhlásit a zpátky v nebo spuštění úlohy hello v Plánovači úloh. V některých případech systémem "*dsregcmd.exe /leave*" v okně příkazového řádku se zvýšenými oprávněními, restartování a zkusit to znovu registrace mohou pomoci s tímto problémem.


**Potenciální problém**: hello pole pro **AzureAdSettingsUrl** je prázdná, a hello zařízení nemá synchronizace. hello může mít posledního přihlášení v zařízení toohello před Enterprise State Roaming byl povolen v Azure Active hello Directory portálu. Restartování zařízení hello a mít hello přihlášení uživatele. Volitelně hello portálu, zkuste s hello správce IT zakázat a znovu povolte nastavení synchronizace mohou uživatelé a podniková Data aplikace. Jakmile tato funkce znovu povolena, restartování hello zařízení a mít hello přihlášení uživatele. Pokud to hello problém nevyřeší **AzureAdSettingsUrl** může být prázdný, v případě hello certifikátu chybný zařízení. V takovém případě systémem "*dsregcmd.exe /leave*" v okně příkazového řádku se zvýšenými oprávněními, restartování a zkusit to znovu registrace mohou pomoci s tímto problémem.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Enterprise State Roaming a vícefaktorového ověřování 
Za určitých podmínek Enterprise State Roaming může selhat toosync dat. Pokud je nakonfigurované ověřování Azure Multi-Factor Authentication. Další podrobnosti o těchto příznaků, naleznete v dokumentu podporu hello [KB3193683](https://support.microsoft.com/kb/3193683). 

**Potenciální problém**: Pokud je zařízení nakonfigurované toorequire služby Multi-Factor Authentication na portálu Azure Active Directory hello, může selhat toosync nastavení při přihlášení pomocí hesla zařízení tooa Windows 10. Tento typ konfigurace služby Multi-Factor Authentication je určený tooprotect účet správce služby Azure. Uživatelé Správce může být schopný toosync přihlášením tootheir Windows 10 zařízení s jejich Microsoft Passport pro pracovní PIN kód nebo pomocí služby Multi-Factor Authentication při přístupu k jiným službám Azure, jako je Office 365.

**Potenciální problém**: synchronizace může selhat, pokud Dobrý den, správce nakonfiguruje zásady podmíněného přístupu hello Active Directory Federation služby Multi-Factor Authentication a vyprší platnost přístupového tokenu hello na hello zařízení. Zajistěte, aby přihlášení a odhlášení pomocí hello Microsoft Passport pro pracovní PIN kód nebo dokončení služby Multi-Factor Authentication při přístupu k jiným službám Azure, jako je Office 365.

###<a name="event-viewer"></a>Prohlížeč událostí
Pro řešení potíží pro pokročilé, Prohlížeč událostí lze použít toofind konkrétní chyby. Ty jsou popsané v tabulce hello. Hello události lze nalézt v prohlížeči událostí > protokoly aplikací a služeb > **Microsoft** > **Windows** > **SettingSync** a identity s problémy se synchronizací **Microsoft** > **Windows** > **Azure AD**.


## <a name="known-issues"></a>Známé problémy

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Synchronizace nefunguje na zařízeních s nainstalovanými aplikacemi zkušebně načtených pomocí softwaru MDM software

Hello ovlivňuje zařízení se systémem Windows 10 Anniversary Update (verze 1607). V prohlížeči událostí pod protokoly hello SettingSync Azure je často seznámili s chybou 80070259 hello 6013 ID události.

**Doporučená akce**  
Ověřte, zda klient v1607 hello Windows 10 má hello 23 srpna 2016 kumulativní aktualizace ([KB3176934](https://support.microsoft.com/kb/3176934) 14393.82 sestavení operačního systému). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Není synchronizována Oblíbené aplikace Internet Explorer

Hello ovlivňuje zařízení se systémem Windows 10 listopadu Update (verze 1511).

**Doporučená akce**  
Ověřte, zda klient v1511 hello Windows 10 má hello července 2016 kumulativní aktualizace ([KB3172985](https://support.microsoft.com/kb/3172985) 10586.494 sestavení operačního systému).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Motiv nesynchronizuje, a také data chráněná pomocí Windows Information Protection 

tooprevent úniku dat, data, která je chráněná pomocí [Windows Information Protection](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) nebude synchronizovat prostřednictvím Enterprise State Roaming pro zařízení pomocí hello Windows 10 Anniversary Update.



**Doporučená akce**  
Žádné. Tento problém může vyřešit tooWindows budoucí aktualizace.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Datum, čas a oblast nastavení není synchronizována na zařízení připojeném k doméně 
  
Zařízení, které jsou připojené k doméně už nebude synchronizace hello nastavení Date, Time a oblast: automatické čas. Pomocí automatické času mohou přepsání hello další data, času a oblasti nastavení a způsobit, že tato nastavení nejsou toosync. 

**Doporučená akce**  
Žádné. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>Řízení uživatelských účtů vyzve při synchronizaci hesel

Ovlivňuje zařízení se systémem hello Windows 10 listopadu aktualizaci (verze 1511) s bezdrátové síťové adaptéry, které je nakonfigurované toosync hesla.

**Doporučená akce**  
Ověřte, zda má klient v1511 hello Windows 10 hello kumulativní aktualizace ([KB3140743](https://support.microsoft.com/kb/3140743) 10586.494 sestavení operačního systému).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Synchronizace nefunguje na zařízení, která používají čipové karty pro přihlášení
Když zkusíte toosign v zařízení s Windows tooyour pomocí čipové karty a virtuální čipové karty, nastavení synchronizace se zastaví práce.     

**Doporučená akce**  
Žádné. Tento problém může vyřešit tooWindows budoucí aktualizace.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Po opuštění podnikové síti není synchronizaci zařízení připojených k doméně     
Registruje zařízení připojených k doméně tooAzure AD může zaznamenat selhání synchronizace, pokud je pro dlouhou dobu odlehlého hello zařízení, a nemůže dokončit ověřování v doméně.

**Doporučená akce**  
Připojte hello zařízení tooa podnikové sítě tak, aby mohli obnovit synchronizace.

---

 ### <a name="azure-ad-joined-device-is-not-syncing-and-hello-user-has-a-mixed-case-user-principal-name"></a>Azure AD připojení k zařízení není synchronizuje a hello uživatelem smíšený případu hlavní název uživatele.
 Pokud má uživatel hello smíšený případu hlavní název uživatele (například uživatelské jméno místo uživatelské jméno) a hello uživatel na zařízení s připojení k Azure AD, kterou má upgradovat z too14393 10586 sestavení Windows 10, mohou selhat zařízení hello uživatele toosync. 

**Doporučená akce**  
Hello uživatel bude muset toounjoin a znovu vstoupit hello zařízení toohello cloud. toodo toto, přihlášení jako uživatel hello místního správce a zrušení služby hello zařízení tak, že přejdete příliš**nastavení** > **systému** > **o** a vyberte možnost " Spravovat nebo odpojení od práci nebo ve škole". Vyčištění souborů hello níže a pak Azure AD Join hello zařízení znovu v **nastavení** > **systému** > **o** a výběrem "připojení tooWork nebo ve škole". Pokračujte v toojoin hello zařízení tooAzure služby Active Directory a dokončení hello toku.

V kroku čištění hello čištění hello následující soubory:
- Settings.dat v`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Všechny soubory ve složce hello hello`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>ID události 6065:80070533, které tento uživatel nemůže přihlásit, protože tento účet je aktuálně zakázáno.  
V prohlížeči událostí pod protokoly hello SettingSync/Debug lze je zobrazit tuto chybu, pokud vypršela platnost přihlašovacích údajů uživatele hello. Kromě toho může dojít, když klient hello neměl automaticky AzureRMS zřízený. 

**Doporučená akce**  
V prvním případě hello být hello uživatel aktualizovat své přihlašovací údaje a zařízení toohello přihlášení s novými pověřeními hello. hello toosolve AzureRMS problém, pokračujte hello kroků uvedených v [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>ID události. 1098: Chyba: 0xCAA5001C Token zprostředkovatele operace se nezdařila  
V prohlížeči událostí pod hello AAD nebo provozní protokoly, tato chyba může vidět s událostí 1104: AAD cloudu Asie modulu plug-in volání Get token vrátil chybu: 0xC000005F. K tomuto problému dochází, pokud jsou chybí oprávnění nebo vlastnictví atributy.    

**Doporučená akce**  
Postupujte podle uvedených kroků hello [KB3196528](https://support.microsoft.com/kb/3196528).  



## <a name="next-steps"></a>Další kroky

- Použití hello [User Voice fórum](https://feedback.azure.com/forums/169401-azure-active-directory/category/158658-enterprise-state-roaming) tooprovide zpětnou vazbu a zkontrolujte návrhy na jak tooimprove Enterprise stavu Roaming.

- Další informace najdete v tématu hello [Enterprise State Roaming přehled](active-directory-windows-enterprise-state-roaming-overview.md). 

## <a name="related-topics"></a>Související témata
* [Přehled roamingu stavu Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Povolit stav enterprise roaming v Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Nastavení a datový roaming – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Nastavení MDM pro synchronizaci nastavení zásad skupiny a](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Odkaz nastavení roamingu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
