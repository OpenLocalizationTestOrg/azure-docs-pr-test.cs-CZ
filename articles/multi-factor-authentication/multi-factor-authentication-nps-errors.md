---
title: "kódy chyb aaaTroubleshoot pro hello rozšíření Azure MFA serveru NPS | Microsoft Docs"
description: "Získat pomoc při řešení problémů s hello NPS rozšíření pro Azure Multi-Factor Authentication s konkrétní řešení pro běžné chybové zprávy"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 8b602954364c6e9f801d86edca6432bd8446c58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-error-messages-from-hello-nps-extension-for-azure-multi-factor-authentication"></a>Vyřešte chybové zprávy z hello NPS rozšíření pro Azure Multi-Factor Authentication

Pokud narazíte na chyby s hello NPS rozšíření pro Azure Multi-Factor Authentication, použijte tento článek tooreach řešení rychlejší. 

## <a name="troubleshooting-steps-for-common-errors"></a>Řešení potíží pro běžné chyby

| Kód chyby | Řešení potíží |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Obraťte se na podporu](#contact-microsoft-support)a zmínili hello seznam kroků pro shromažďování protokolů. Zadejte co nejvíce informací můžete o co se stalo před hello chybu, včetně id klienta a hlavní název uživatele (UPN). |
| **CLIENT_CERT_INSTALL_ERROR** | Pravděpodobně problém s jak hello klientský certifikát byl nainstalován nebo ve spojení s vašeho klienta. Postupujte podle pokynů hello v [hello Poradce při potížích s MFA NPS rozšíření](multi-factor-authentication-nps-extension.md#troubleshooting) tooinvestigate problémů certifikátu klienta. |
| **ESTS_TOKEN_ERROR** | Postupujte podle pokynů hello v [hello Poradce při potížích s MFA NPS rozšíření](multi-factor-authentication-nps-extension.md#troubleshooting) tooinvestigate klientského certifikátu a ADAL token problémy. |
| **HTTPS_COMMUNICATION_ERROR** | Hello NPS server je nelze tooreceive odpovědí z Azure MFA. Ověřte, zda jsou vaší brány firewall pro přenosy tooand z https://adnotifications.windowsazure.com otevřete obousměrně |
| **HTTP_CONNECT_ERROR** | Ověřte na hello serveru, který spouští hello NPS rozšíření, můžete dosáhnout https://adnotifications.windowsazure.com a https://login.microsoftonline.com/. Pokud nemáte zatížení u těchto lokalit, řešení potíží s připojením na tomto serveru. |
| **REGISTRY_CONFIG_ERROR** | Klíč nebyl nalezen v registru hello hello aplikaci, která může být způsobeno hello [skript prostředí PowerShell](multi-factor-authentication-nps-extension.md#install-the-nps-extension) nebyl po instalaci spustit. Hello chybová zpráva by měla obsahovat hello chybí klíč. Ujistěte se, že máte hello klíč HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. |
| **REQUEST_FORMAT_ERROR** <br> Chybí povinný atribut protokolu Radius userName\Identifier požadavku protokolu RADIUS. Ověřte, zda je server NPS přijímají požadavky protokolu RADIUS | Tato chyba obvykle odráží potíže s instalací. Hello rozšíření NPS musí být nainstalován na serverech NPS, které můžou přijímat požadavky protokolu RADIUS. Servery NPS, které se instalují jako závislosti pro služby, jako je RDG a RRAS nemáte přijímal požadavky protokolu radius. Rozšíření serveru NPS nefunguje, pokud je nainstalován v takové instalací a chyb se od hello podrobnosti nelze přečíst z žádosti o ověření hello. |
| **REQUEST_MISSING_CODE** | Ujistěte se, že protokol šifrování hesla hello mezi hello NPS a servery NAS podporuje hello sekundární metodu ověřování, kterou používáte. **PAP** podporuje všechny metody ověřování hello Azure mfa v cloudu hello: telefonní hovor, jednosměrné textová zpráva, oznámení mobilní aplikace a kód ověření mobilní aplikace. **CHAPV2** a **EAP** podporují telefonní hovory a oznámení mobilní aplikace. |
| **USERNAME_CANONICALIZATION_ERROR** | Ověřte, zda hello uživatel byl přítomen v instanci služby Active Directory v místě, a že hello službu NPS má oprávnění tooaccess hello adresář. Pokud používáte vztahy důvěryhodnosti mezi doménovými strukturami, [obraťte se na podporu](#contact-microsoft-support) pro další pomoc. |


   

### <a name="alternate-login-id-errors"></a>Alternativního přihlašovacího ID chyby

| Kód chyby | Chybová zpráva | Řešení potíží |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Chyba: userObjectSid vyhledávání se nezdařilo | Ověřte, že tento hello uživatel existuje v místní instanci služby Active Directory. Pokud používáte vztahy důvěryhodnosti mezi doménovými strukturami, [obraťte se na podporu](#contact-microsoft-support) pro další pomoc. |
| **ALTERNATE_LOGIN_ID_ERROR** | Chyba: Alternativní LoginId vyhledávání se nezdařilo | Ověřte, zda je LDAP_ALTERNATE_LOGINID_ATTRIBUTE nastavena tooa [atribut platný active directory](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> Pokud LDAP_FORCE_GLOBAL_CATALOG nastavená tooTrue nebo LDAP_LOOKUP_FORESTS je nakonfigurován s hodnotou není prázdný, ověřte, zda jste nakonfigurovali na globální katalog a tento atribut AlternateLoginId hello je přidána tooit. <br><br> Pokud je nakonfigurované LDAP_LOOKUP_FORESTS neprázdnou hodnotu, ověřte správnost hello hodnotu. Pokud existuje více než jeden název doménové struktury, musí být odděleny hello názvy oddělte středníkem, ne mezery. <br><br> Pokud tyto kroky nejsou opravit problém hello [obraťte se na podporu](#contact-microsoft-support) pro další pomoc. |
| **ALTERNATE_LOGIN_ID_ERROR** | Chyba: Alternativní LoginId hodnota je prázdná | Ověřte, zda že je tento atribut AlternateLoginId hello nakonfigurovaný pro uživatele hello. |


## <a name="errors-your-users-may-encounter"></a>Chyby vaši uživatelé setkat.

| Kód chyby | Chybová zpráva | Řešení potíží |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | Volající klient nemá ověřování toodo oprávnění přístupu pro uživatele hello | Zkontrolujte, zda hello klienta domény a domény hello hello hlavní název uživatele (UPN) jsou hello stejné. Například, ujistěte se, že user@contoso.com se pokouší tooauthenticate toohello Contoso klienta. Hello UPN představuje platného uživatele pro klienta hello v Azure. |
| **AuthenticationMethodNotConfigured** | Hello zadané metody ověřování nebyla nakonfigurována pro uživatele hello | Mít hello uživatele, přidání nebo ověřte své metody ověřování podle pokynů toohello v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Zadané metody ověřování není podporována. | Shromažďovat všechny protokoly, které obsahují tato chyba, a [obraťte se na podporu](#contact-microsoft-support). Při kontaktování podpory, zadejte uživatelské jméno hello a metodu hello sekundární ověřování, který aktivoval hello chyba. |
| **BecAccessDenied** | Volání MSODS Bec vrátil odepření přístupu, pravděpodobně hello username není definované v klientovi hello | uživatel Hello se nachází ve službě Active Directory v místě, ale není synchronizovat do Azure AD pomocí AD Connect. Nebo pro klienta hello chybí hello uživatele. Přidat uživatele tooAzure hello AD a požádejte o přidání své metody ověřování podle pokynů toohello v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** nebo **StrongAuthenticationServiceInvalidParameter** | Hello telefonní číslo je ve formátu nerozpoznatelný | Máte hello uživatele opravte jejich ověření telefonních čísel. |
| **InvalidSession** | Hello zadané relace je neplatná nebo platnost vypršela | Hello relace trvá víc než tři minuty toocomplete. Zkontrolujte tento uživatel hello zadávání hello ověřovací kód, nebo neodpovídá toohello oznámení aplikace, do tří minut po inicializaci hello žádosti o ověření. Jestliže problém hello problém, zkontrolujte, že neexistují žádné síťovou latenci mezi klientem, na Server, NPS Server a koncový bod Azure MFA hello.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Žádná metoda ověření výchozí byla nakonfigurována pro uživatele hello | Mít hello uživatele, přidání nebo ověřte své metody ověřování podle pokynů toohello v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md). Ověřte tento uživatel hello je vybraná výchozí metodou ověřování a nakonfigurovaná dané metody pro svůj účet. |
| **OathCodePinIncorrect** | Nesprávný kód a zadán kód pin. | Tato chyba není očekáván v hello rozšíření serveru NPS. Pokud uživatel zaznamená, [obraťte se na podporu](#contact-microsoft-support) Poradce při potížích. |
| **ProofDataNotFound** | Doklad dat nebylo nakonfigurováno pro hello zadané metody ověřování. | Uživatel hello opakujte metodu různých ověřování, nebo přidejte nové metody ověřování podle pokynů toohello v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md). Pokud uživatel hello pokračuje toosee tato chyba po můžete potvrdit, správně, nastavit jejich metoda ověření [obraťte se na podporu](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Nesprávný kód a zadán kód pin. (OneWaySMS) | Tato chyba není očekáván v hello rozšíření serveru NPS. Pokud uživatel zaznamená, [obraťte se na podporu](#contact-microsoft-support) Poradce při potížích. |
| **TenantIsBlocked** | Klient je blokovaný. | [Obraťte se na podporu](#contact-microsoft-support) s ID adresáře na stránce Vlastnosti hello Azure AD v hello portálu Azure. |
| **UserNotFound** | Zadaný uživatel Hello nebyl nalezen. | Hello klienta není již zobrazen jako aktivní v Azure AD. Zkontrolujte, zda je vaše předplatné aktivní, a máte hello požadované první strany aplikace. Také zkontrolujte, že hello klienta v předmětu certifikátu hello podle očekávání a hello certifikát je stále platný a registrované v části hello instanční objekt. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Zprávy, že uživatelé setkat, která nejsou chyby

Uživatelé mohou někdy zobrazit zprávy z aplikace Multi-Factor Authentication vzhledem k tomu, že jejich žádosti o ověření se nezdařilo. Tyto nejsou chyby v produktu hello konfigurace, ale jsou úmyslně upozornění vysvětlením, proč byl odepřen požadavek ověřování.

| Kód chyby | Chybová zpráva | Doporučené kroky | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Nesprávný kód entered\OATH nesprávný kód | Není k chybě, uživatel zadal nesprávný kód. | Hello uživatel zadal nesprávný kód hello. Kliknul opakujte žádají o nový kód nebo znovu se přihlaste. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Maximální povolené kód opakování dosaženo | Hello uživatele se nezdařilo ověření výzvy hello příliš mnohokrát. V závislosti na nastavení které mohou potřebovat toobe odblokováno pomocí Správce nyní.  |
| **SMSAuthFailedWrongCodeEntered** | Pokazilo zadali nebo textu zprávy jednorázového HESLA nesprávný kód | Hello uživatel zadal nesprávný kód hello. Kliknul opakujte žádají o nový kód nebo znovu se přihlaste. |

## <a name="errors-that-require-support"></a>Chyby, které vyžadují podporu

Pokud některé z těchto chyb narazíte, doporučujeme vám [obraťte se na podporu](#contact-microsoft-support) diagnostiky nápovědu. Neexistuje žádné standardní sadu kroky, které můžete vyřešit tyto chyby. Když kontaktujte podporu, aby tooinclude jako mnoho informací nejblíže hello kroky, které vedly tooan chyba a informace o klientovi.

| Kód chyby | Chybová zpráva |
| ---------- | ------------- |
| **InvalidParameter** | Požadavek nesmí mít hodnotu null |
| **InvalidParameter** | ObjectId nesmí být null nebo prázdná pro ReplicationScope: {0} |
| **InvalidParameter** | Hello délka NázevSpolečnosti \{0} \ je delší než maximální povolenou délku {1} hello |
| **InvalidParameter** | UserPrincipalName nesmí být null nebo prázdný |
| **InvalidParameter** | Hello za předpokladu, že TenantId není ve správném formátu |
| **InvalidParameter** | ID relace nesmí být null nebo prázdný |
| **InvalidParameter** | Nepodařilo se přeložit všechny ProofData z požadavku nebo Msods. Hello ProofData neznámý. |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Další kroky

### <a name="troubleshoot-user-accounts"></a>Řešení potíží s uživatelské účty

Pokud jsou vaši uživatelé [došlo k potížím s dvoustupnovym overovanim](./end-user/multi-factor-authentication-end-user-troubleshoot.md), pomoci jim samoobslužné diagnostikovat problémy. 

### <a name="contact-microsoft-support"></a>Obraťte se na podporu společnosti Microsoft

Pokud potřebujete další pomoc, obraťte se na pracovníky technické podpory prostřednictvím [podpory Azure Multi-Factor Authentication Server](https://support.microsoft.com/oas/default.aspx?prid=14947). Při kontaktování nám, je užitečné, pokud zahrnete tolik informací o problému nejdříve. Informace, které můžete zadat zahrnují hello stránky, kde jste viděli hello chybu, hello chybový kód, hello ID konkrétní relace, hello ID uživatele hello kdo viděli hello chybě a protokoly pro ladění.

toocollect ladění protokolů pro diagnostiku podporu, použijte hello následující kroky: 

1. Otevřete příkazový řádek správce a spusťte tyto příkazy:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

2. Reprodukujte problém hello

3. Zastavte trasování hello pomocí těchto příkazů:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

4. ZIP hello obsah složky C:\NPS hello a připojte případu podpory toohello soubor ZIP hello.


