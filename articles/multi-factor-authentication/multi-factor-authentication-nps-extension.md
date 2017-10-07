---
title: "aaaUse existující NPS servery tooprovide Azure MFA možnosti | Microsoft Docs"
description: "Hello Network Policy Server rozšíření pro Azure Multi-Factor Authentication je jednoduchým řešením tooadd cloudové dvoustupňové vericiation možnosti tooyour existující ověřování infrastruktura."
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
ms.date: 08/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: e9fc495b06873d45f06233f1aa618db9d94f8bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Stávající infrastruktury serveru NPS integrovat Azure Multi-Factor Authentication

Hello rozšíření serveru NPS (Network Policy Server) pro Azure MFA přidá cloudové MFA možnosti tooyour ověřování infrastruktury pomocí existujících serverů. Hello rozšíření serveru NPS můžete přidat telefonní hovor, textová zpráva nebo telefonní aplikace ověření tooyour existující tok ověřování bez nutnosti tooinstall, konfigurovat a spravovat nové servery. 

Toto rozšíření byla vytvořena pro organizace, které mají připojení k síti VPN tooprotect bez nasazení hello Azure MFA serveru. Hello NPS rozšíření funguje jako adaptér mezi RADIUS a cloudu Azure MFA tooprovide druhý faktor ověřování pro federované nebo synchronizované uživatele.

Pokud používáte hello NPS rozšíření pro Azure MFA, zahrnuje tok ověřování hello hello následující součásti: 

1. **Server NAS nebo virtuální privátní sítě** přijímá požadavky od klientů VPN a převede je na servery tooNPS požadavky protokolu RADIUS. 
2. **NPS Server** připojí tooActive Directory tooperform hello primární ověřování pro hello RADIUS požadavky a na úspěch, předává hello požadavek tooany nainstalovaná rozšíření.  
3. **Rozšíření serveru NPS** aktivuje tooAzure požadavku vícefaktorového ověřování pro hello sekundární ověřování. Po rozšíření hello obdrží odpověď hello a pokud se podaří hello ověřovací test MFA, dokončení žádosti o ověření hello tím, že poskytuje hello NPS server s tokeny zabezpečení, které obsahují deklaraci MFA vydané službou tokenů zabezpečení Azure.  
4. **Azure MFA** komunikuje se službou Azure Active Directory tooretrieve hello podrobnosti uživatele a provede hello sekundárního ověřování pomocí uživatele toohello nakonfigurovaná metoda ověření.

Hello následující diagram znázorňuje tento tok požadavku vysoké úrovně ověřování: 

![Vývojový diagram ověřování](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Plánování nasazení

Hello rozšíření NPS automaticky zpracovává redundance, takže není nutné žádnou zvláštní konfiguraci.

Můžete vytvořit tolik serverů povolena Azure MFA serveru NPS podle potřeby. Pokud instalujete několik serverů, používejte rozdíl klientský certifikát pro každou z nich. Vytvoření certifikátu pro každý server znamená, že můžete aktualizovat jednotlivě každý certifikát a nestarat se o výpadek napříč všemi servery.

Servery VPN směrovat požadavky na ověření, potřebují toobe vědět hello nové servery NPS povolená Azure MFA.

## <a name="prerequisites"></a>Požadavky

Hello NPS rozšíření je určená toowork s vaší stávající infrastruktury. Ujistěte se, že máte následující požadavky před hello začít.

### <a name="licenses"></a>Licence

Hello NPS rozšíření pro Azure MFA je k dispozici toocustomers s [licencí pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication.md) (zahrnutá v Azure AD Premium, EMS nebo předplatné vícefaktorového ověřování).

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 nebo novější.

### <a name="libraries"></a>Knihovny

Tyto knihovny jsou automaticky nainstalovány s příponou hello.
-   [Distribuovatelné balíčky Visual C++ pro Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory modul pro prostředí Windows PowerShell verze 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

Všechny, kteří používají rozšíření hello NPS musí být synchronizovaná tooAzure služby Active Directory přes Azure AD Connect a musí být zaregistrovaný pro MFA.

Při instalaci rozšíření hello musíte hello directory ID a správce přihlašovacích údajů pro vašeho tenanta Azure AD. Vaše ID adresáře můžete najít v hello [portál Azure](https://portal.azure.com). Přihlaste se jako správce, vyberte hello **Azure Active Directory** vyberte ikonu na levé straně hello **vlastnosti**. Kopírování hello identifikátor GUID v hello **ID adresáře** pole a uložte ho. Při instalaci hello rozšíření NPS použijete jako ID klienta hello tento identifikátor GUID.

![Najít ID vašeho adresáře v rámci Azure Active Directory vlastnosti](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Příprava prostředí

Před instalací NPS rozšíření hello chcete tooprepare můžete prostředí toohandle hello ověřovací provoz.

### <a name="enable-hello-nps-role-on-a-domain-joined-server"></a>Povolit hello role NPS na serveru připojeném k doméně

Hello NPS server připojí tooAzure služby Active Directory a ověřuje požadavky vícefaktorového ověřování hello. Vyberte jeden server pro tuto roli. Doporučujeme vybrat server, který nemůže pracovat s požadavky z jiných služeb, protože hello NPS rozšíření vyvolá chyby pro všechny žádosti, které nejsou protokolu RADIUS.

1. Na serveru, otevřete hello **Průvodce přidáním rolí a funkcí** z hello nabídce rychlý start správce serveru.
2. Zvolte **instalace na základě rolí nebo na základě funkcí** pro váš typ instalace.
3. Vyberte hello **služba síťové zásady a přístup** role serveru. Okno může překryvné tooinform jste toorun požadované funkce této role.
4. Pomocí Průvodce nadále pokračujte hello dokud hello potvrzovací stránku. Vyberte **nainstalovat**.

Nyní když máte server určený pro server NPS, měli byste také nakonfigurovat tento server toohandle příchozí požadavky protokolu RADIUS z hello řešení sítě VPN.

### <a name="configure-your-vpn-solution-toocommunicate-with-hello-nps-server"></a>Konfigurace vaší toocommunicate řešení VPN pomocí serveru NPS hello

V závislosti na řešení sítě VPN, které používají, hello kroky tooconfigure vaše zásady ověřování RADIUS lišit. Nakonfigurujte server RADIUS serveru NPS tooyour toopoint této zásady.

### <a name="sync-domain-users-toohello-cloud"></a>Synchronizace domény uživatele toohello cloudu

Tento krok již mohou být dokončené na klientovi, ale je dobrým toodouble zkontrolujte že Azure AD Connect nedávno synchronizoval vaše databáze.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Vyberte **Azure Active Directory** > **Azure AD Connect**
3. Ověřte, zda je vaše stav synchronizace **povoleno** a že poslední synchronizace byla menší než hodinou.

Pokud potřebujete tookick nové kolo synchronizace, nám hello pokyny v [synchronizace Azure AD Connect: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Určit, jaké metody ověřování mohou uživatelé používat

Existují dva faktory, které ovlivňují, které metody ověřování jsou k dispozici s nasazením rozšíření serveru NPS:

1. Hello heslo šifrovacího algoritmu používaného mezi klientem RADIUS hello (sítě VPN, Netscaler server či jiné) a servery NPS hello.
   - **PAP** podporuje všechny metody ověřování hello Azure mfa v cloudu hello: telefonní hovor, jednosměrné textová zpráva, oznámení mobilní aplikace a kód ověření mobilní aplikace.
   - **CHAPV2** a **EAP** podporují telefonní hovory a oznámení mobilní aplikace.
2. Hello vstupní metody, které hello klientská aplikace (VPN, Netscaler server či jiné) může zpracovat. Například klienta VPN hello mít některé prostředky tooallow hello uživatele tootype v ověřovací kód z mobilní aplikace nebo text?

Když nasadíte hello NPS rozšíření, použijte tooevaluate tyto faktory, které metody jsou k dispozici pro vaše uživatele. Pokud váš klient RADIUS podporuje protokol PAP, ale klient hello UX nemá vstupní pole pro ověřovací kód, pak telefonního hovoru a oznámení mobilní aplikace jsou dvě podporované možnosti hello.

Můžete [zakázat nepodporované ověřování metody](multi-factor-authentication-whats-next.md#selectable-verification-methods) v Azure.

### <a name="enable-users-for-mfa"></a>Povolit uživatelům pro MFA

Před nasazením hello úplné NPS rozšíření, musíte tooenable vícefaktorového ověřování pro uživatele hello, které chcete tooperform dvoustupňové ověřování. Více okamžitě tootest hello rozšíření, jako je nasazení, musíte účet alespoň jeden test, který je plně zaregistrován u služby Multi-Factor Authentication.

Použijte tyto kroky tooget spustit testovací účet:
1. Přihlaste se příliš[https://aka.ms/mfasetup](https://aka.ms/mfasetup) s testovací účet. 
2. Postupujte podle výzvy tooset hello až metodu ověřování.
3. Buď vytvoření zásady podmíněného přístupu nebo [změnit stav uživatele hello](multi-factor-authentication-get-started-user-states.md) toorequire dvoustupňové ověřování pro testovací účet hello. 

Uživatelé také potřebovat toofollow tooenroll tyto kroky předtím, než můžete ověřit pomocí rozšíření NPS hello.

## <a name="install-hello-nps-extension"></a>Instalace serveru NPS rozšíření hello

> [!IMPORTANT]
> Nainstalujte rozšíření hello NPS na jiném serveru než hello VPN přístupový bod.

### <a name="download-and-install-hello-nps-extension-for-azure-mfa"></a>Stáhněte a nainstalujte hello NPS rozšíření pro Azure MFA

1.  [Stáhnout hello NPS rozšíření](https://aka.ms/npsmfa) z hello Microsoft Download Center.
2.  Zkopírujte binární toohello hello chcete tooconfigure Network Policy Server.
3.  Spustit *setup.exe* a postupujte podle pokynů pro instalaci hello. Pokud narazíte na chyby, zkontrolujte že hello dvě knihovny z oddílu hello požadovaných součástí byly úspěšně nainstalovány.

### <a name="run-hello-powershell-script"></a>Spustit skript prostředí PowerShell hello

Hello instalační program vytvoří skript prostředí PowerShell v tomto umístění: `C:\Program Files\Microsoft\AzureMfa\Config` (kde C:\ představuje instalační jednotce). Tento skript prostředí PowerShell provádí hello následující akce:

-   Vytvořte certifikát podepsaný svým držitelem.
-   Přidružte veřejný klíč hello hello certifikát toohello instančního objektu v Azure AD.
-   Uložte hello certifikátu do úložiště certifikátů místního počítače hello.
-   Udělení přístupu toohello certifikátu privátního klíče tooNetwork uživatele.
-   Restartujte hello serveru NPS.

Pokud chcete, aby toouse vlastní certifikáty (namísto hello podepsaný svým držitelem certifikáty, které generuje skript prostředí PowerShell text hello), spusťte toocomplete hello instalace hello skript prostředí PowerShell. Pokud instalujete hello rozšíření na více serverech, každé z nich by měl mít svůj vlastní certifikát.

1. Spusťte prostředí Windows PowerShell jako správce.
2. Změňte adresáře.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Spustíte skript prostředí PowerShell hello vytvořené hello Instalační služby.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. Prostředí PowerShell vyzve k zadání vašeho ID klienta. Použijte hello Directory ID GUID, který jste zkopírovali ze hello portálu Azure v části předpoklady hello.
5. Přihlaste se jako správce tooAzure AD.
6. PowerShell zobrazí zpráva o úspěšném provedení až po dokončení skriptu hello.  

Opakujte tyto kroky na všechny další servery NPS, které chcete tooset pro službu Vyrovnávání zatížení.

>[!NOTE]
>Pokud používáte vlastní certifikáty místo aby generovala certifikáty s hello skript prostředí PowerShell, ujistěte se, že se správně zarovnané toohello NPS zásady vytváření názvů. musí být název subjektu Hello **CN =\<TenantID\>, OU = Microsoft NPS rozšíření**. 

## <a name="configure-your-nps-extension"></a>Konfigurace serveru NPS rozšíření

Tato část obsahuje důležité informace o návrhu a návrhy pro úspěšné nasazení rozšíření serveru NPS.

### <a name="configuration-limitations"></a>Konfigurace omezení

- Hello NPS rozšíření pro Azure MFA nezahrnuje uživatelé toomigrate nástroje a nastavení z cloudu toohello MFA serveru. Z tohoto důvodu doporučujeme pomocí hello rozšíření pro nová nasazení, nikoli stávajícího nasazení. Pokud používáte rozšíření hello na stávajícího nasazení, mají vaši uživatelé tooperform výš znovu toopopulate jejich MFA podrobnosti v cloudu hello.  
- Hello NPS rozšíření používá hello UPN z hello místní služby Active directory uživatele tooidentify hello na Azure MFA pro provádění hello sekundární umožňuje hello rozšíření může být nakonfigurované toouse jiný identifikátor jako alternativního přihlašovacího ID nebo vlastní služby Active Directory pole, které není hlavní název uživatele. V tématu [rozšířených možnostech konfigurace pro hello NPS rozšíření pro službu Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) Další informace.
- Ne všechny protokoly šifrování podporují všechny metody ověřování.
   - **PAP** podporuje telefonní hovor, jednosměrné textová zpráva, oznámení mobilní aplikace a kód ověření mobilní aplikace
   - **CHAPV2** a **EAP** podporují telefonní hovory a oznámení mobilní aplikace

### <a name="control-radius-clients-that-require-mfa"></a>Ovládací prvek klientů RADIUS, které vyžadují vícefaktorové ověřování

Jakmile povolíte MFA pro klienta RADIUS pomocí hello rozšíření serveru NPS, jsou všechny ověřování pro tohoto klienta požadované tooperform MFA. Pokud chcete tooenable MFA pro některé klienty RADIUS a jiné ne, můžete nakonfigurovat dva servery NPS a nainstalovat rozšíření hello pouze pro jednu z nich. Nakonfigurujte klienty RADIUS, které chcete toorequire MFA toosend požadavky toohello NPS serveru nakonfigurovaný s rozšířením hello a ostatní klienti toohello NPS serveru RADIUS není nakonfigurovaný s rozšířením hello.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Příprava pro uživatele, která nejsou zaregistrovaná pro MFA

Pokud máte uživatele, která nejsou zaregistrovaná pro MFA, můžete určit, co se stane, když se pokusí tooauthenticate. Použít nastavení registru hello *REQUIRE_USER_MATCH* v cestě registru hello *HKLM\Software\Microsoft\AzureMFA* toocontrol hello funkce chování. Toto nastavení má možnost jeden konfigurace:

| Klíč | Hodnota | Výchozí |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | HODNOTU TRUE NEBO FALSE | Není nastavena (ekvivalentní tooTRUE) |

Hello účel tohoto nastavení je toodetermine co toodo, když uživatel není zaregistrovaný pro MFA. Když hello klíč neexistuje, není nastaven nebo je nastaven tooTRUE a uživatel hello není registrovaný. poté hello rozšíření selže ověřovací test MFA hello. Po nastavení klíče hello tooFALSE a uživatel hello není registrovaný, ověření pokračuje bez vícefaktorového ověřování.

Můžete zvolit toocreate tento klíč a nastavte ji tooFALSE, zatímco jsou vaši uživatelé registrace, a ne všechny možné registrovat pro Azure MFA ještě. Ale vzhledem k tomu, že klíč hello nastavení umožňuje uživatelům, která nejsou zaregistrovaná pro toosign MFA v, byste měli odebrat tento klíč před tooproduction probíhající.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="how-do-i-verify-that-hello-client-cert-is-installed-as-expected"></a>Jak ověřit, že hello klientského certifikátu je nainstalován podle očekávání?

Podívejte se pro certifikát podepsaný svým držitelem hello vytvořené hello instalační program v úložiště certifikátu hello a zkontrolujte, zda text hello privátní klíč má oprávnění udělená toouser **síťové služby**. obsahuje název subjektu z certifikátu Hello **CN \<tenantid\>, OU = Microsoft NPS rozšíření**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-toomy-tenant-in-azure-active-directory"></a>Jak můžete ověřit, že moje klientského certifikátu je klient přidružené toomy v Azure Active Directory?

Otevřete příkazový řádek Powershellu a spusťte hello následující příkazy:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Tyto příkazy vytisknout všechny certifikáty hello přidružení klientovi s vaší instancí hello rozšíření serveru NPS v relaci prostředí PowerShell. Vyhledejte certifikát tak, že vyexportujete klientského certifikátu jako soubor "x.509 s kódováním Base-64" bez hello soukromého klíče a jejímu porovnání s hello seznamu z prostředí PowerShell.

Platný-z a platné – dokud časová razítka, které jsou v podobě čitelný, může být použité toofilter out zřejmé misfits, pokud příkaz hello vrátí více než jeden certifikát.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Proč se nedaří Moje žádosti s ADAL Chyba tokenu?

Tato chyba může být kvůli tooone z několika důvodů. Tyto kroky řešení potíží s toohelp použijte:

1. Restartujte NPS server.
2. Ověřte, zda je tento certifikát klienta nainstalovány podle očekávání.
3. Ověřte, že tento certifikát hello je spojený s vašeho klienta na Azure AD.
4. Ověřte, že tento https://login.microsoftonline.com/ je přístupná ze serveru hello systémem hello rozšíření.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-hello-user-is-not-found"></a>Proč ověřování nezdaří s chybou v protokolech HTTP s informacemi o tom, že tento uživatel hello nebyl nalezen?

Ověřte, zda je spuštěna AD Connect a tento uživatel hello je součástí Windows Active Directory a Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Proč vidí HTTP připojit chyby v protokolech s Moje ověřování selhání?

Ověřte, že tento https://adnotifications.windowsazure.com je dostupný ze serveru hello s hello NPS rozšíření.


## <a name="next-steps"></a>Další kroky

- Konfigurovat alternativní ID pro přihlášení, nebo nastavit seznam výjimek pro IP adresy, který by neměl provádět dvoustupňové ověření v [rozšířených možnostech konfigurace pro hello NPS rozšíření pro službu Multi-Factor Authentication](nps-extension-advanced-configuration.md)

- [Vyřešte chybové zprávy z hello NPS rozšíření pro Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md)
