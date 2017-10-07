---
title: "Azure AD předávací ověřování – rychlý Start | Microsoft Docs"
description: "Tento článek popisuje, jak tooget pracovat s Azure Active Directory (Azure AD) předávací ověřování."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: billmath
ms.openlocfilehash: d6d0f85fe144cf36cc94676f6592d37988b20647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure předávací ověřování služby Active Directory: Rychlý start

## <a name="how-toodeploy-azure-ad-pass-through-authentication"></a>Jak toodeploy Azure AD předávací ověřování

Předávací ověřování Azure Active Directory (Azure AD) umožňuje toosign vaši uživatelé v tooboth místní a cloudové aplikace pomocí hello stejnými hesly. Ho přihlášení uživatele pomocí ověřování hesla přímo pro vaše místní službu Active Directory.

>[!IMPORTANT]
>Předávací ověřování Azure AD je aktuálně ve verzi preview. Pokud používáte tuto funkci prostřednictvím preview, měli byste zajistit upgradu verze preview agentů hello ověřování pomocí pokynů hello [zde](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Je třeba toofollow tyto pokyny toodeploy předávací ověřování:

## <a name="step-1-check-prerequisites"></a>Krok 1: Kontrola předpokladů

Ujistěte se, že hello následující požadované součásti jsou na místě:

### <a name="on-hello-azure-active-directory-admin-center"></a>V Centru pro správu Azure Active Directory hello

1. Vytvořte účet globálního správce jenom pro cloud v klientovi Azure AD. Tímto způsobem můžete spravovat hello konfigurace klienta služby by měla místní služby nezdaří, nebo k dispozici. Další informace o [přidání účtu globálního správce jenom pro cloud](../active-directory-users-create-azure-portal.md). Provedením tohoto kroku je důležité tooensure, který není získat uzamčení klienta služby.
2. Přidejte jednu nebo více [názvy vlastních domén](../active-directory-add-domain.md) klienta tooyour Azure AD. Uživatelům se přihlásit pomocí některého z těchto názvů domény.

### <a name="in-your-on-premises-environment"></a>V místním prostředí

1. Určení serveru se systémem Windows Server 2012 R2 nebo novější na které toorun Azure AD Connect. Přidáte doménové struktury toohello stejnou AD server hello jako hello uživatele, jejichž hesla se musí ověřit toobe.
2. Nainstalujte hello [nejnovější verzi služby Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) na serveru hello identifikovaného v předchozím kroku. Pokud je již spuštěna Azure AD Connect, ujistěte se, tato verze hello je 1.1.557.0 nebo novější.
3. Určit další server s Windows serverem 2012 R2 nebo později na které toorun a samostatné Agent ověřování. verze agenta ověřování Hello musí toobe 1.5.193.0 nebo novější. Tento server je potřebné tooensure vysokou dostupnost žádostí o přihlášení. Přidáte doménové struktury toohello stejnou AD server hello jako hello uživatele, jejichž hesla se musí ověřit toobe.
4. Pokud je brána firewall mezi servery a Azure AD, je třeba tooconfigure hello následující položky:
   - Ujistěte se, že ověřování agentů provádět **odchozí** vyžaduje tooAzure AD přes hello následující porty:
   
   | Číslo portu | Jak se používají |
   | --- | --- |
   | **80** | Stahování odvolání certifikátu seznamy (CRL) při ověřování certifikátu SSL hello |
   | **443** | Veškerá odchozí komunikace v naší službě |
   
   Pokud brána firewall vynucuje pravidla podle toooriginating uživatelů, otevřete tyto porty pro přenos ze služby Windows, které běží jako síťové služby.
   - Pokud brána firewall nebo proxy server umožňuje povolených DNS, připojení povolených příliš**\*. msappproxy.net** a  **\*. servicebus.windows.net**. Pokud ne, povolit přístup příliš[rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), se aktualizují každý týden.
   - Ověřování agenty potřebují přístup příliš**login.windows.net** a **login.microsoftonline.com** pro počáteční registraci, takže otevřete brány firewall pro tyto adresy URL také.
   - Pro ověření certifikátu, odblokovat hello následující adresy URL: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** a  **www.microsoft.com:80**. Tyto adresy URL se používají k ověření certifikátu s dalšími produkty Microsoftu, takže už můžete mít tyto adresy URL odblokováno.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Krok 2: Povolit podporu protokolu Exchange ActiveSync (volitelné)

Postupujte podle těchto pokynů tooenable podporu protokolu Exchange ActiveSync:

1. Použití [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) toorun hello následující příkaz:
```
Get-OrganizationConfig | fl per*
```

2. Zkontrolujte hodnotu hello hello `PerTenantSwitchToESTSEnabled` nastavení. Pokud je hodnota hello **true**, váš klient je správně nakonfigurována – to je obvykle hello případu pro většinu zákazníků. Pokud je hodnota hello **false**spusťte hello následující příkaz:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Ověřte, zda text hello hodnota Dobrý den `PerTenantSwitchToESTSEnabled` nyní nastavení je příliš**true**. Počkejte, než jednu hodinu před přesunutí toohello další krok.

Pokud jste v tomto kroku musí čelit všechny problémy, zkontrolujte naše [Průvodce odstraňováním potíží s](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) Další informace.

## <a name="step-3-enable-hello-feature"></a>Krok 3: Povolení funkce hello

Předávací ověřování se dá zapnout pomocí [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Předávací ověřování se dá zapnout na server primární nebo pracovní hello Azure AD Connect. Důrazně doporučujeme, abyste povolili z primárního serveru hello.

Pokud Azure AD Connect pro hello instalujete poprvé, zvolte hello [vlastní cesta](active-directory-aadconnect-get-started-custom.md). V hello **přihlášení uživatele** vyberte **předávací ověřování** jako hello přihlašovací na metodu. Při úspěšném dokončení, je agent předávací ověřování nainstalovaný na hello stejný server jako Azure AD Connect. Kromě toho je povolena funkce předávací ověřování hello na klientovi.

![Azure AD Connect – přihlášení uživatele](./media/active-directory-aadconnect-sso/sso3.png)

Pokud jste již nainstalovali Azure AD Connect (pomocí hello [Expresní instalace](active-directory-aadconnect-get-started-express.md) nebo hello [vlastní instalaci](active-directory-aadconnect-get-started-custom.md) cesta), vyberte **změnit uživatele přihlašovací stránku** na Azure AD Připojit a klikněte na tlačítko **Další**. Potom vyberte **předávací ověřování** jako hello přihlašovací na metodu. Při úspěšném dokončení, je agent předávací ověřování nainstalovaný na hello stejný server jako funkce Azure AD Connect a hello je povolena na klientovi.

![Azure AD Connect – přihlášení uživatele změn](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Předávací ověřování je funkce úrovni klienta. Zapnutí má dopad na přihlášení pro uživatele mezi _všechny_ hello spravované domény ve vašem klientovi. Pokud přecházíte z AD FS tooPass prostřednictvím ověřování, doporučujeme, počkejte nejméně 12 hodin před ukončením infrastruktury služby AD FS – tato čekací doba je tooensure, který uživatelé můžete zachovat přihlášení tooExchange ActiveSync během přechodu.

## <a name="step-4-test-hello-feature"></a>Krok 4: Test hello funkce

Postupujte podle těchto pokynů tooverify, že jste povolili ověřování průchozí správně:

1. Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) s přihlašovacími údaji hello globální správce pro vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně hello.
3. Vyberte **Azure AD Connect**.
4. Ověřte, že hello **předávací ověřování** zobrazí jako **povoleno**.
5. Vyberte **předávací ověřování**. Toto okno obsahuje seznam serverů hello nainstalovanou agenty ověřování.

![Azure Centrum pro správu služby Active Directory – okno Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Centrum pro správu – okno předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

V této fázi se můžete přihlásit uživatele ze všech spravovaných domén ve vašem klientovi pomocí předávacího ověřování. Uživatelé z federovaných domén však stále toosign v prostřednictvím služby Active Directory Federation Services (AD FS) nebo jiného zprostředkovatele federace, kterou jste dříve nakonfigurovali. Pokud převedete domény z federované toomanaged, všichni uživatelé z této domény, automaticky spustí, přihlášení pomocí předávacího ověřování. Jenom pro cloud uživatelé nejsou dopad funkce hello předávací ověřování.

## <a name="step-5-ensure-high-availability"></a>Krok 5: Zajištění vysoké dostupnosti

Pokud máte v plánu toodeploy předávací ověřování v produkčním prostředí, musíte nainstalovat samostatnou Agent ověřování. Tento druhý ověřování agenta nainstalujte na server _jiných_ než hello jeden spuštěná Azure AD Connect a hello prvního ověření agenta. Tato instalace poskytuje můžete vysokou dostupnost žádostí o přihlášení. Postupujte podle těchto pokynů toodeploy samostatnou Agent ověřování:

1. **Stáhněte nejnovější verzi hello Agent ověřování hello (verze 1.5.193.0 nebo novější)**: Přihlaste toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně hello.
3. Vyberte **Azure AD Connect** a potom **předávací ověřování**. A vyberte **stažení agenta**.
4. Klikněte na tlačítko hello **přijmout podmínky a stahovat** tlačítko.
5. **Nainstalujte nejnovější verzi hello Agent ověřování hello**: hello spustit spustitelný soubor stáhli v předchozím kroku hello. Zadejte pověření pro globálního správce vašeho klienta po zobrazení výzvy.

![Azure Active Directory Centrum pro správu – tlačítko Stáhnout agenta ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Centrum pro správu – okno stáhnout agenta](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Můžete také stáhnout hello ověřování agenta z [zde](https://aka.ms/getauthagent). Zajistěte, aby zkontrolujte a přijměte Agent ověřování hello [podmínkami služby](https://aka.ms/authagenteula) _před_ jeho instalaci.

## <a name="next-steps"></a>Další kroky
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – tato funkce je aktuálně ve verzi preview. Zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
