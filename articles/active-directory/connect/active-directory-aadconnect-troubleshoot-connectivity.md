---
title: "Azure AD Connect: Problémů s připojením | Microsoft Docs"
description: "Vysvětluje, jak tootroubleshoot připojení problémy s Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 60d6b7c4ad8a3ab907c20e598ec9443f115df287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Řešení potíží s připojením službou Azure AD Connect
Tento článek vysvětluje, jak funguje připojení mezi Azure AD Connect a službou Azure AD a jak problémy tootroubleshoot připojení. Tyto problémy jsou pravděpodobně toobe zobrazená v prostředí s proxy serverem.

## <a name="troubleshoot-connectivity-issues-in-hello-installation-wizard"></a>Řešení potíží s připojením v Průvodci instalací hello
Azure AD Connect používá moderní ověřování (pomocí knihovny ADAL hello) pro ověřování. Průvodce instalací Hello a hello synchronizační modul správné vyžadují machine.config toobe správně nakonfigurovány, protože jsou tyto dva aplikací .NET.

V tomto článku jsme ukazují, jak společnost Fabrikam připojí tooAzure AD přes jeho proxy serveru. Hello proxy server jmenuje fabrikamproxy a používá port 8080.

Nejdřív potřebujeme toomake zda [ **machine.config** ](active-directory-aadconnect-prerequisites.md#connectivity) je správně nakonfigurován.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

> [!NOTE]
> V některých jiných společností než Microsoft blogy je popsané, že by měl být provedeny změny toomiiserver.exe.config místo. Tento soubor je však na každém upgradu, tak i pokud funguje během počáteční instalace, hello systém přestane fungovat v prvním upgradu přepsána. Z tohoto důvodu se doporučení hello místo toho tooupdate souboru machine.config.
>
>

Hello proxy server musí mít také hello požadované adresy URL otevřít. oficiální seznamu Hello je popsána v [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Tyto adresy URL je hello následující tabulka vůbec hello absolutní úplné minimální toobe možné tooconnect tooAzure AD. Tento seznam neobsahuje žádné volitelné funkce, jako je například zpětný zápis hesla nebo Azure AD Connect Health. Je zdokumentovaných sem toohelp při řešení potíží pro počáteční konfiguraci hello.

| ADRESA URL | Port | Popis |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |Zobrazí seznam použitých toodownload seznamu CRL. |
| \*. verisign.com |HTTP/80 |Zobrazí seznam použitých toodownload seznamu CRL. |
| \*. entrust.com |HTTP/80 |Použít toodownload CRL jsou uvedené pro MFA. |
| \*.windows.net |PROTOKOL HTTPS NEBO 443 |Použít toosign v tooAzure AD. |
| Secure.aadcdn.microsoftonline p.com |PROTOKOL HTTPS NEBO 443 |Použít pro MFA. |
| \*.microsoftonline.com |PROTOKOL HTTPS NEBO 443 |Použít tooconfigure Azure AD data adresáře a importu a exportu. |

## <a name="errors-in-hello-wizard"></a>Chyby v Průvodci hello
Průvodce instalací Hello používá dvou různých kontextech zabezpečení. Na stránce hello **připojit tooAzure AD**, používá hello aktuálně přihlášený uživatel. Na stránce hello **konfigurace**, mění toohello [účet, který spouští hello služby pro synchronizační modul hello](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). Pokud nastane problém, zobrazí se pravděpodobně už na hello **připojit tooAzure AD** stránky v Průvodci hello vzhledem k tomu, že je globální konfiguraci proxy serveru hello.

Hello následující problémy jsou hello nejběžnějších chyb, na které narazíte v Průvodci instalací hello.

### <a name="hello-installation-wizard-has-not-been-correctly-configured"></a>Průvodce instalací Hello nebyla nakonfigurována správně
Tato chyba se zobrazí, pokud hello Průvodce nemůže dosáhnou hello proxy.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

* Pokud se zobrazí tato chyba, zkontrolujte hello [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) byl správně nakonfigurován.
* Pokud toto vypadá správné, postupujte podle kroků hello v [ověření proxy serveru konektivity](#verify-proxy-connectivity) toosee, pokud se problém hello nachází mimo hello Průvodce také.

### <a name="a-microsoft-account-is-used"></a>Účet Microsoft se používá.
Pokud používáte **účtu Microsoft** ne **školní nebo organizace** účet, najdete v části obecné chybě.  
![Account Microsoft se používá.](./media/active-directory-aadconnect-troubleshoot-connectivity/unknownerror.png)

### <a name="hello-mfa-endpoint-cannot-be-reached"></a>není dostupný koncový bod Hello vícefaktorového ověřování
Tato chyba se zobrazí, pokud hello koncový bod **https://secure.aadcdn.microsoftonline-p.com** je nedostupné a globálního správce povolené ověřování MFA.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

* Pokud se zobrazí tato chyba, ověřte, že koncový bod hello **secure.aadcdn.microsoftonline p.com** přidala toohello proxy.

### <a name="hello-password-cannot-be-verified"></a>nelze ověřit heslo Hello
Pokud Průvodce instalací hello je při připojování tooAzure AD úspěšný, ale heslo hello, samotné nelze ověřit, že se zobrazí tato chyba:  
![BadPassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

* Je heslo hello dočasné heslo a je třeba změnit? Je je ve skutečnosti hello správné heslo? Zkuste toosign v toohttps://login.microsoftonline.com (na jiném počítači než server Azure AD Connect hello) a ověřte, zda je možné použít účet hello.

### <a name="verify-proxy-connectivity"></a>Ověřte připojení k proxy serveru
tooverify Pokud hello server Azure AD Connect má skutečné připojení s hello Proxy a Internetu, použijte některé toosee prostředí PowerShell Pokud hello proxy umožňuje webových požadavků nebo ne. V příkazovém řádku prostředí PowerShell, spusťte `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technicky hello první volání je funguje i toohttps://login.microsoftonline.com a tento identifikátor URI, ale hello jiných identifikátor URI je rychlejší toorespond.)

PowerShell používá hello konfigurace v souboru machine.config toocontact hello proxy. nastavení Hello v winhttp nebo netsh by neměla mít vliv na tyto rutiny.

Pokud proxy server hello je správně nakonfigurovaná, měli byste obdržet stav úspěšného dokončení: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Pokud se zobrazí **vzdáleného serveru nelze tooconnect toohello**, prostředí PowerShell se pokouší toomake přímé volání bez použití hello proxy nebo DNS není správně nakonfigurováno. Ujistěte se, zda text hello **machine.config** souboru je správně nakonfigurován.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Pokud proxy server hello není nakonfigurovaná správně, dojde k chybě: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

| Chyba | Text chyby | Komentář |
| --- | --- | --- |
| 403 |Je zakázané |Proxy Server Hello nebyl otevřen pro hello požadovaná adresa URL. Pokroku hello konfiguraci proxy serveru a ujistěte se, zda text hello [adresy URL](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) jsou otevřené. |
| 407 |Vyžadováno ověření proxy serveru |Hello proxy server vyžaduje přihlášení a nebyl poskytnut žádný. Pokud proxy server vyžaduje ověřování, ujistěte se, že toohave toto nastavení nakonfigurované v souboru machine.config hello. Ujistěte se, že používáte doménové účty pro uživatele hello spuštěním Průvodce hello a pro účet služby hello také. |

## <a name="hello-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>vzor Hello komunikace mezi Azure AD Connect a službou Azure AD
Pokud jste provedli všechny předchozí kroky a pořád nemůžete připojit, může v tuto chvíli spustit vyhledávání v síťových protokolech. Tato část je dokumentace se vzorem normálního a úspěšné připojení. Je také je výpis běžné red herrings, které můžete ignorovat při čtení hello síťových protokolů.

* Existují toohttps://dc.services.visualstudio.com volání. Není požadovaná toohave, který tuto adresu URL otevřete hello proxy serveru pro instalaci toosucceed hello a těchto volání můžete ignorovat.
* Uvidíte, že překlad názvů dns uvádí hello toobe skutečné hostitelů v hello DNS název místa nsatc.net a jiných oborech názvů není v části microsoftonline.com. Ale na názvy hello skutečné serveru nejsou k dispozici žádné žádosti webové služby a nemáte tooadd proxy toohello tyto adresy URL.
* adminwebservice Hello koncových bodů a provisioningapi jsou koncové body pro zjišťování a používají toofind hello skutečný koncový bod toouse. Tyto koncové body se liší v závislosti na vaší oblasti.

### <a name="reference-proxy-logs"></a>Referenční dokumentace proxy protokoly
Tady je výpis z skutečné proxy protokolu a hello stránce Průvodce instalací odkud pořízení (toohello duplicitní položky byly odebrány stejné koncového bodu). V této části můžete použít jako odkaz pro svoje vlastní protokoly proxy serveru a sítě. skutečné koncové body Hello se mohou lišit ve vašem prostředí (zejména tyto adresy URL v *Kurzíva*).

**Připojit tooAzure AD**

| Čas | ADRESA URL |
| --- | --- |
| 1/11/2016 8:31 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:31 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |připojení: / /*bba800 ukotvení*. microsoftonline.com:443 |
| 1/11/2016 8:32 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:33 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |připojení: / /*bwsc02 předávání*. microsoftonline.com:443 |

**Konfigurace**

| Čas | ADRESA URL |
| --- | --- |
| 1/11/2016 8:43 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:43 |připojení: / /*bba800 ukotvení*. microsoftonline.com:443 |
| 1/11/2016 8:43 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |připojení: / /*bba900 ukotvení*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |připojení: / /*bba800 ukotvení*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:46 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |připojení: / /*bwsc02 předávání*. microsoftonline.com:443 |

**Počáteční synchronizace**

| Čas | ADRESA URL |
| --- | --- |
| 1/11/2016 8:48 |Connect://Login.Windows.NET:443 |
| 1/11/2016 8:49 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |připojení: / /*bba900 ukotvení*. microsoftonline.com:443 |
| 1/11/2016 8:49 |připojení: / /*bba800 ukotvení*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Chybám při ověřování
Tato část obsahuje chyby, které mohou být vráceny z ADAL (hello ověřování knihovny používané službou Azure AD Connect) a prostředí PowerShell. Chyba Hello vysvětlené by vám pomůže v pochopit další kroky.

### <a name="invalid-grant"></a>Neplatný Grant
Neplatné uživatelské jméno nebo heslo Další informace najdete v tématu [nelze ověřit heslo hello](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Typ Neznámý uživatele
Adresář Azure AD nelze nalézt nebo přeložit. Možná pokusíte toologin s uživatelským jménem v neověřené domény?

### <a name="user-realm-discovery-failed"></a>Zjišťování sféry uživatele se nezdařilo
Problémy s konfigurací sítě nebo proxy server. Hello síti není dosažitelné. V tématu [problémů s připojením v Průvodci instalací hello](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Platnost hesla uživatele
Vypršela platnost přihlašovacích údajů. Změňte heslo.

### <a name="authorizationfailure"></a>AuthorizationFailure
Neznámý problém.

### <a name="authentication-cancelled"></a>Ověřování zrušena
výzvy Hello vícefaktorové ověřování (MFA) byla zrušena.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Ověření bylo úspěšné, ale Azure AD PowerShell má problém s ověřování.

### <a name="azurerolemissing"></a>AzureRoleMissing
Ověření bylo úspěšné. Nejste globálním správcem.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Ověření bylo úspěšné. Správa privilegovaných identit byl povolen a aktuálně nejste globálním správcem. Další informace najdete v tématu [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Ověření bylo úspěšné. Nelze načíst informace o společnosti z Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Ověření bylo úspěšné. Nelze načíst informace o doméně z Azure AD.

### <a name="unexpected-exception"></a>Došlo k neočekávané výjimce
Zobrazit jako Neočekávaná chyba v Průvodci instalací hello. Může dojít, pokud se pokusíte toouse **Account Microsoft** ne **školní nebo organizace účet**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Řešení potíží pro předchozí verze.
S verzemi počínaje číslo sestavení 1.1.105.0 (vydané. února 2016) vyřazenou hello Pomocníka pro přihlášení. Tato část a hello konfigurace musí už být požadované, ale se ukládají jako odkaz.

Hello jednotné přihlašování v toowork pomocníka musí být nakonfigurované winhttp. Tato konfigurace se provádí pomocí [ **netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="hello-sign-in-assistant-has-not-been-correctly-configured"></a>Hello Pomocníka pro přihlášení není správně nakonfigurován.
Tato chyba se zobrazí, když hello Pomocníka pro přihlášení nelze dosáhnout hello proxy nebo hello proxy není povolením hello požadavku.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

* Pokud se zobrazí tato chyba, podívejte se na konfiguraci proxy serveru hello v [netsh](active-directory-aadconnect-prerequisites.md#connectivity) a ověřte je správná.
  ![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
* Pokud toto vypadá správné, postupujte podle kroků hello v [ověření proxy serveru konektivity](#verify-proxy-connectivity) toosee, pokud se problém hello nachází mimo hello Průvodce také.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
