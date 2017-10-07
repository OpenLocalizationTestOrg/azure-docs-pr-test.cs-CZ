---
title: aaaTroubleshoot Proxy aplikace | Microsoft Docs
description: Zahrnuje jak tootroubleshoot chyby v Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 970caafb-40b8-483c-bb46-c8b032a4fb74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: d426708a2ee32da6674987b5794602ed984b06b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Řešení potíží s Proxy aplikace problémy a chybové zprávy
Pokud dojde k chybám při přístupu k publikované aplikaci nebo v publikování aplikací, zkontrolujte následující možnosti toosee-li Microsoft Azure AD Application Proxy pracuje správně hello:

* Služby systému Windows otevřete hello konzole a ověřte, že hello **Microsoft AAD Application Proxy Connector** je povolená a spuštěná služba. Můžete také toolook na stránku hello Proxy aplikace služby vlastnosti, jak ukazuje následující obrázek hello:  
  ![Snímek obrazovky okna vlastností konektoru Microsoft AAD Application Proxy](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)
* Otevřete Prohlížeč událostí a vyhledejte události konektoru Proxy aplikace v **protokoly aplikací a služeb** > **Microsoft** > **AadApplicationProxy** > **konektor** > **správce**.
* V případě potřeby podrobnější protokoly jsou dostupné ve [zapnete hello protokoly relace konektoru Proxy aplikace](application-proxy-understand-connectors.md#under-the-hood).

Další informace o nástroji Poradce při potížích s hello Azure AD najdete v tématu [řešení potíží s nástroji toovalidate konektor síťové požadavky](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites).

## <a name="hello-page-is-not-rendered-correctly"></a>stránku Hello není správně vykreslen.
Můžete mít problémy s aplikací vykreslování nebo funguje správně, aniž by mu musela specifické chybové zprávy. Tato situace může nastat, pokud jste publikovali hello článku cesty, ale aplikace hello vyžaduje obsah, který existuje mimo této cestě.

Například pokud publikujete hello cesta https://yourapp/app, ale aplikace hello zavolá bitové kopie v https://yourapp/media, že nebude vykreslit. Ujistěte se, že publikujete pomocí cesty hello nejvyšší úrovně je nutné tooinclude všechny relevantní obsah aplikace hello. V tomto příkladu je http://yourapp/.

Je-li změnit váš obsah tooinclude odkazuje cesta, ale potřebujete ještě další uživatelé tooland na hlubší propojení v cestě hello, najdete v příspěvku blogu hello [nastavení hello správné odkaz pro aplikace Proxy aplikace v panelu přístup hello Azure AD a Office 365 aplikace Spouštěč](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="connector-errors"></a>Konektor chyby

Použití hello [Azure AD Application proxy serveru konektoru porty nástroj pro testování](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify, aby vaše konektor přístup hello Proxy aplikace služby. Minimálně zkontrolujte, že oblast hello střed USA a tooyou nejbližší oblast hello jsou všechny zelené značky zaškrtnutí. Kromě toho další zelené značky zaškrtnutí znamená větší odolnost proti chybám. 

Pokud se registrace nezdařila během hello průvodce Instalace konektoru, existují dva způsoby tooview hello příčinu selhání hello. Buď vyhledejte v protokolu událostí hello pod **aplikace a služby Logs\Microsoft\AadApplicationProxy\Connector\Admin**, nebo hello spusťte následující příkaz prostředí Windows PowerShell:

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

Jakmile zjistíte hello konektor došlo k chybě z protokolu událostí hello, použijte tuto tabulku běžné chyby tooresolve hello problému:

| Chyba | Doporučené kroky |
| ----- | ----------------- |
| Registrace konektoru se nezdařilo: Zajistěte, aby povolené Proxy aplikace v hello portálu pro správu Azure a správně zadali služby Active Directory uživatelské jméno a heslo. : 'Jedné nebo více chybám došlo k chybě." | Pokud jste zavřeli okno registrace hello bez přihlášení tooAzure AD, znovu spusťte Průvodce pro konektor hello a zaregistrujte hello konektor. <br><br> Pokud hello registrace okno otevře a hned zavře bez povolení jste toolog v, zobrazí se pravděpodobně této chybě. K této chybě dojde, když dojde k chybě sítě v systému. Ujistěte se, že je možné tooconnect z veřejného webu tooa prohlížeče a že jsou otevřené zadané v hello porty [požadavky na Proxy aplikace](active-directory-application-proxy-enable.md). |
| Vymazat chyba se zobrazí v okně registrace hello. Nelze pokračovat. | Pokud se zobrazí tato chyba a hello okno se zavře, jste zadali hello nesprávné uživatelské jméno nebo heslo. Zkuste to znovu. |
| Registrace konektoru se nezdařilo: Zajistěte, aby povolené Proxy aplikace v hello portálu pro správu Azure a správně zadali služby Active Directory uživatelské jméno a heslo. Chyba: ' AADSTS50059: žádné informace o identifikaci klienta najít v žádosti o buď hello nebo implicitní podle zadané přihlašovací údaje a hledání službou se nezdařilo hlavní identifikátor URI. | Pokoušíte toosign pomocí Account Microsoft a ne doméně, která je součástí ID organizace hello hello adresáře se pokoušíte tooaccess. Ujistěte se, že Dobrý den, správce je součástí hello stejným názvem domény jako hello doména klienta, například pokud hello Azure AD doména je contoso.com, dobrý den, Správce musí být admin@contoso.com. |
| Nepodařilo tooretrieve hello aktuální provádění zásady pro spuštěné skripty prostředí PowerShell. | Pokud hello instalace konektoru selže, zkontrolujte toomake jistotu, že není zakázána zásady spouštění prostředí PowerShell. <br><br>1. Hello otevřete Editor zásad skupiny.<br>2. Přejděte příliš**konfigurace počítače** > **šablony pro správu** > **součásti systému Windows**  >   **Prostředí Windows PowerShell** a dvakrát klikněte na **zapnout provádění skriptu**.<br>3. zásady spouštění hello můžete nastavit tooeither **není nakonfigurováno** nebo **povoleno**. Pokud nastavení příliš**povoleno**, ujistěte se, že v nabídce Možnosti, hello zásada spouštění nastavena tooeither **povolit skripty místní a vzdálené podepsaných skriptů** nebo příliš**povolit všechny skripty**. |
| Connector se nepovedlo toodownload hello konfigurace. | klientský certifikát, Hello konektor, který se používá k ověřování, vypršela platnost. To může taky dojít, pokud máte nainstalovaný konektor za proxy server hello. V takovém případě hello Connector nemůže získat přístup k Internetu hello a nebude aplikací mít tooprovide tooremote uživatele. Obnovit ručně pomocí hello důvěryhodnosti `Register-AppProxyConnector` rutiny v prostředí Windows PowerShell. Pokud vaše konektor je za proxy server, je nutné toogrant Internet toohello konektor účty pro přístup k "síťové služby" a "místní systém." Můžete to provést, poskytněte jim přístup k toohello Proxy nebo nastavením je toobypass hello proxy serveru. |
| Registrace konektoru se nezdařilo: Zkontrolujte, zda jsou globální správce vaší služby Active Directory tooregister hello konektor. Chyba: ' hello registrace žádost byla zamítnuta." | Hello alias, který se pokoušíte toolog, není správce v této doméně. Vaše konektor je vždy nainstalován pro hello adresář, který vlastní hello uživatele domény. Ujistěte se, zda má účet správce hello, ke kterému se pokoušíte toosign s globální oprávnění toohello Azure AD klienta. |

## <a name="kerberos-errors"></a>Chyby protokolu Kerberos

Tato tabulka obsahuje hello běžných chyb, které pocházejí z nastavení protokolu Kerberos a konfigurace i návrhy na řešení.

| Chyba | Doporučené kroky |
| ----- | ----------------- |
| Nepodařilo tooretrieve hello aktuální provádění zásady pro spuštěné skripty prostředí PowerShell. | Pokud hello instalace konektoru selže, zkontrolujte toomake jistotu, že není zakázána zásady spouštění prostředí PowerShell.<br><br>1. Hello otevřete Editor zásad skupiny.<br>2. Přejděte příliš**konfigurace počítače** > **šablony pro správu** > **součásti systému Windows**  >   **Prostředí Windows PowerShell** a dvakrát klikněte na **zapnout provádění skriptu**.<br>3. zásady spouštění hello můžete nastavit tooeither **není nakonfigurováno** nebo **povoleno**. Pokud nastavení příliš**povoleno**, ujistěte se, že v nabídce Možnosti, hello zásada spouštění nastavena tooeither **povolit skripty místní a vzdálené podepsaných skriptů** nebo příliš**povolit všechny skripty**. |
| 12008 – azure AD překročena hello maximální počet povolených ověřování protokolem Kerberos pokusí toohello back-end serveru. | Tato chyba může znamenat nesprávné konfiguraci mezi službou Azure AD a hello back-end serveru aplikace nebo o problém v konfiguraci data a času na obou počítačích. Hello back-end server odmítl lístek protokolu Kerberos hello vytvořené Azure AD. Ověřte, že Azure AD a hello back-end aplikačního serveru jsou správně nakonfigurované. Ujistěte se, že jsou synchronizovány hello datum a čas konfigurace na hello Azure AD a hello back-end aplikačním serveru. |
| 13016 – azure AD nelze načíst lístek protokolu Kerberos jménem uživatele hello, protože není žádné UPN v hello hraniční token nebo hello přístup k souboru cookie. | Došlo k potížím s konfigurací hello služby tokenů zabezpečení. Opravte hello deklarace konfigurace v hello služby tokenů zabezpečení. |
| 13019 – azure AD nelze získat lístek protokolu Kerberos jménem uživatele hello z důvodu hello následující obecné chybě rozhraní API. | Tato událost může znamenat nesprávné konfiguraci mezi službou Azure AD a server řadiče domény hello nebo o problém v konfiguraci data a času na obou počítačích. řadič domény Hello odmítl lístek protokolu Kerberos hello vytvořené Azure AD. Ověřte, že Azure AD a hello back-end aplikačního serveru jsou správně nakonfigurovány, zejména hello hlavního názvu služby konfigurace. Zkontrolujte, zda hello Azure AD je připojený k doméně toohello stejné doméně jako hello tooensure řadiče domény, který hello řadič domény vytváří vztah důvěryhodnosti s Azure AD. Ujistěte se, že jsou synchronizovány hello datum a čas konfigurace na hello Azure AD a řadič domény hello. |
| 13020 – azure AD nelze načíst lístek protokolu Kerberos jménem uživatele hello, protože není definováno hello back-end serveru SPN. | Tato událost může znamenat nesprávné konfiguraci mezi službou Azure AD a server řadiče domény hello nebo o problém v konfiguraci data a času na obou počítačích. řadič domény Hello odmítl lístek protokolu Kerberos hello vytvořené Azure AD. Ověřte, že Azure AD a hello back-end aplikačního serveru jsou správně nakonfigurovány, zejména hello hlavního názvu služby konfigurace. Zkontrolujte, zda hello Azure AD je připojený k doméně toohello stejné doméně jako hello tooensure řadiče domény, který hello řadič domény vytváří vztah důvěryhodnosti s Azure AD. Ujistěte se, že jsou synchronizovány hello datum a čas konfigurace na hello Azure AD a řadič domény hello. |
| 13022 – azure AD nemůže ověřit uživatele hello, protože hello back-end server odpoví tooKerberos pokusy o přihlášení s chybou HTTP 401. | Tato událost může znamenat nesprávné konfiguraci mezi službou Azure AD a hello back-end serveru aplikace nebo o problém v konfiguraci data a času na obou počítačích. Hello back-end server odmítl lístek protokolu Kerberos hello vytvořené Azure AD. Ověřte, že Azure AD a hello back-end aplikačního serveru jsou správně nakonfigurované. Ujistěte se, že jsou synchronizovány hello datum a čas konfigurace na hello Azure AD a hello back-end aplikačním serveru. |

## <a name="end-user-errors"></a>Chyby koncového uživatele

Tento seznam obsahuje chyby, které vaši koncoví uživatelé setkat, když se zkuste tooaccess hello aplikace a selhání. 

| Chyba | Doporučené kroky |
| ----- | ----------------- |
| Hello webu nelze zobrazit stránku hello. | Uživatel může dojít k této chybě při pokusu o tooaccess hello aplikaci, kterou jste publikovali, pokud je aplikace hello aplikací integrované ověřování systému Windows. pro tuto aplikaci může být nesprávný, definována Hello SPN. Pro aplikace, integrované ověřování systému Windows Ujistěte se, že hello SPN nakonfigurován pro tuto aplikaci správné. |
| Hello webu nelze zobrazit stránku hello. | Uživatel může dojít k této chybě při pokusu o tooaccess hello aplikaci, kterou jste publikovali, pokud aplikace hello aplikace OWA. To může být způsobeno jedním z následujících hello:<br><li>Hello definovaný název SPN pro tuto aplikaci je nesprávný. Ujistěte se, že hello SPN nakonfigurován pro tuto aplikaci správné.</li><li>Hello uživatele, který se pokusila tooaccess hello aplikace se pomocí účtu Microsoft a nikoli hello toosign správné podnikový účet v nebo hello uživatele je uživatel typu Host. Ujistěte se, hello uživatel se přihlásí pomocí podnikovém účtu, odpovídá hello domény hello publikované aplikace. Uživatelé s Account Microsoft a hostů nelze přístup k aplikacím integrované ověřování systému Windows.</li><li>Hello uživatele, který se pokusila tooaccess hello aplikace není správně definované pro tuto aplikaci na straně místní hello. Ujistěte se, jestli tento uživatel má příslušná oprávnění hello definovaný pro tuto aplikaci back-end na hello místní počítač. |
| Nelze získat přístup k této podnikové aplikace. Můžete se není autorizovaný tooaccess této aplikace. Ověření se nezdařilo. Ujistěte se, zda tooassign hello uživatele s aplikací toothis přístup. | Vaši uživatelé může dojít k této chybě při pokusu o tooaccess hello aplikaci, kterou jste publikovali, pokud používají účty Microsoft místo jejich toosign podnikový účet v. Uživatele typu Host může také dojít k této chybě. Uživatelé s Account Microsoft a hosté nelze přístup k aplikacím integrované ověřování systému Windows. Ujistěte se, hello uživatel se přihlásí pomocí podnikovém účtu, odpovídá hello domény hello publikované aplikace.<br><br>Nemůže mít přiřazeno hello uživatele pro tuto aplikaci. Přejděte toohello **aplikace** kartě a v části **uživatelů a skupin**, přiřadit tomuto uživateli nebo aplikaci toothis skupiny uživatelů. |
| Nyní je nepřístupný této podnikové aplikace. Zkuste to prosím znovu později... hello konektor vypršel časový limit. | Vaši uživatelé může dojít k této chybě při pokusu o tooaccess hello aplikaci, kterou jste publikovali, pokud nejsou správně definované pro tuto aplikaci na straně místní hello. Ujistěte se, že uživatelé mají hello příslušná oprávnění, jak jsou definovány pro tuto aplikaci back-end na hello místní počítač. |
| Nelze získat přístup k této podnikové aplikace. Můžete se není autorizovaný tooaccess této aplikace. Ověření se nezdařilo. Zajistěte, aby že tento uživatel hello, který má licenci pro Azure Active Directory Premium nebo Basic. | Vaši uživatelé může dojít k této chybě při pokusu o tooaccess hello aplikaci, kterou jste publikovali, pokud nebyla přiřazena explicitně s licencí Premium nebo Basic správcem hello odběratele. Přejděte služby Active Directory toohello odběratele **licence** kartě a ujistěte se, že tohoto uživatele nebo skupiny uživatelů je přiřazena licence Premium nebo Basic. |

## <a name="my-error-wasnt-listed-here"></a>Moje chyba nebyl zde uvedené.

Pokud dojde k chybě nebo problém s Azure AD Application Proxy, který není uveden v této příručce pro řešení potíží, rádi bychom znali toohear o něm. Odeslat e-mailu tooour [názor týmu](mailto:aadapfeedback@microsoft.com) hello podrobné informace o této chybě hello je.

## <a name="see-also"></a>Viz také
* [Povolení Proxy aplikace služby Azure Active Directory](active-directory-application-proxy-enable.md)
* [Publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md)
* [Povolit jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md)
* [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
