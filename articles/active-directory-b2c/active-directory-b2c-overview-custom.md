---
title: "Služby Azure Active Directory B2C: Vlastní zásady | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1ff398a4-2079-4615-94f1-57de22c0aad6
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: bada9cf5f1a86f3dd047536f6fd9359c0231a384
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C: Vlastní zásady

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Jaké jsou vlastní zásady?

Vlastní zásady jsou konfigurační soubory, které definují chování hello klienta služby Azure AD B2C. Zatímco **integrovaných zásad** předdefinovaných portálu hello Azure AD B2C hello nejčastějších úloh, identity, vlastních zásad může být plně upravit toocomplete vývojáře identity near neomezený počet úloh. Přečtěte si toodetermine Pokud jsou pro vás ideální a váš scénář identity vlastní zásady.

**Vlastní zásady pro úpravy nejsou pro všechny uživatele.** křivku Hello je náročné, hello spuštění déle, a budoucí změny toocustom zásady bude vyžadovat podobné toomaintain odborných znalostí. Předdefinované zásady je třeba pečlivě zvážit nejprve pro váš scénář před použitím vlastní zásady.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Porovnání zásad předdefinované a vlastní zásady

| | Předdefinované zásady | vlastní zásady |
|-|-------------------|-----------------|
|Cíloví uživatelé | Všechny vývojáři aplikací s nebo bez znalosti identity | Odborníci na identitu: integrátorem systémů, konzultanty a týmy interní identity. Jsou tedy se OpenIDConnect toky a pochopit poskytovatelů identit a ověřování na základě deklarace identity |
|Konfigurace – metoda | Portál Azure s uživatelsky přívětivý uživatelského rozhraní | Přímo úpravy souborů XML a pak odesílání toohello portálu Azure |
|Přizpůsobení uživatelského rozhraní | Úplné přizpůsobení uživatelského rozhraní, včetně podpory jazyka HTML, CSS a jscript (vyžaduje vlastní domény)<br><br>Podpora více jazyků s vlastní řetězce | stejné |
| Vlastní nastavení atributu | Standardní a vlastní atributy | stejné |
|Token a relace správy | Vlastní token a více možností relace | stejné |
|Zprostředkovatelé identity| **Dnes**: předdefinované místní, sociálních zprostředkovatele<br><br>**Budoucí**: založených na standardech OIDC SAML, OAuth | **Dnes**: založených na standardech OIDC OAUTH, SAML<br><br>**Budoucí**: WsFed |
|Úlohy identity (příklady) | Registrace nebo přihlášení s mnoha sociálních účty a místní<br><br>Samoobslužné resetování hesla<br><br>Úpravy profilu<br><br>Služba Multi-Factor Auth scénáře<br><br>Přizpůsobení tokeny a relace<br><br>Tok tokenu přístupu | Dokončení hello stejné úlohy jako integrovaných zásad pomocí poskytovatelů vlastní identitu nebo používat vlastní obory<br><br>Zřízení uživatele v jiném systému v době hello registrace<br><br>Odeslání Uvítacího e-mailu pomocí vlastního zprostředkovatele služby e-mailu<br><br>Použít úložiště uživatele mimo B2C<br><br>Ověření uživatele informace s důvěryhodné systému prostřednictvím rozhraní API |

## <a name="policy-files"></a>Soubory zásad

Vlastní zásady je reprezentován jako jednoho nebo několika souborů ve formátu XML které tooeach jiné v hierarchické řetězu. elementy XML Hello definovat: schéma deklarace identity, deklarace transformace, obsahu definice, profily poskytovatelů a technické deklarace identity a Userjourney kroků Orchestrace, mezi další prvky.

Doporučujeme hello používá tři typy souborů zásad:

- **ZÁKLADNÍHO souboru**, který obsahuje většinu hello definic a pro které Azure poskytuje kompletní příklad.  Doporučujeme, že abyste vytvořili minimální počet změny toothis souboru toohelp při řešení problémů a dlouhodobé údržby zásad
- **soubor rozšíření** , obsahuje změny hello jedinečné konfigurace pro vašeho klienta
- **soubor předávající strany (RP)** tedy hello jeden zaměřené na úloh soubor, který je vyvolat přímo hello aplikace nebo služby (neboli předávající strany).  Přečtěte si článek hello v souboru definice zásad Další informace.  Každý úkol jedinečný vyžaduje vlastní RP a v závislosti na branding požadavky hello číslo může být "Celkový počet aplikací x celkový počet případů použití".


Předdefinované zásady v Azure AD B2C, postupujte podle vzoru souboru 3 hello znázorněný výše, ale hello vývojáře jen vidí hello soubor předávající strany (RP), zatímco hello portál provede změny v souboru EXTenstions toohello pozadí hello.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Základní koncepty, které byste měli vědět při použití vlastních zásad

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azure zákazníka identit a přístupu (CIAM) služba management. Služba Hello obsahuje:

1. Adresář uživatele v podobě hello speciální Azure Active Directory přístupné přes Microsoft Graph a který obsahuje data o místní účty a účty federované uživatele 
2. Přístup k toohello **Identity rozhraní Framework** která orchestruje vztah důvěryhodnosti mezi uživateli a entity a předá deklarací identity mezi nimi toocomplete úlohy správy identitě/přístup 
3. Služby tokenů zabezpečení (STS) vydávání id tokeny, aktualizace tokeny a přístup tokeny (a ekvivalentní kontrolní výrazy SAML) a ověřování je tooprotect prostředky.

Azure AD B2C komunikuje s zprostředkovatelů identity jiných systémů, uživatelů a adresářem hello místního uživatele v pořadí tooachieve úkol identity aplikace (například přihlášení uživatele, registraci nového uživatele, resetovat heslo). Hello základní platformu, která vytváří více stran vztah důvěryhodnosti a provede tyto kroky je volána hello Identity Framework prostředí a zásady (také nazývané cesty uživatele nebo zásady důvěryhodnosti framework) explicitně definuje hello aktéři, akce hello hello protokoly a hello posloupnost kroků toocomplete.

### <a name="identity-experience-framework"></a>Architektura prostředí identit

Plně konfigurovatelné řízená zásadami, cloudové platformy Azure, které orchestrují vztah důvěryhodnosti mezi entity (široce zprostředkovatelů deklarací) v standardní protokol formáty například OpenIDConnect, OAuth, SAML, WSFed a několik nestandardní ty (například rozhraní REST API založené na výměnu deklarací na systému). Vytvoří uživatelsky přívětivý Hello I2E, whitelabelled vyskytne, který podporuje HTML, CSS a jscript.  V současné době hello Identity Framework prostředí je k dispozici pouze v kontextu hello hello služby Azure AD B2C a prioritu pro tooCIAM související úlohy.

### <a name="built-in-policies"></a>Předdefinované zásady

Předdefinované konfigurační soubory, že přímý hello chování Azure AD B2C tooperform hello nejčastěji používaná identity úlohy (tj. uživatel registrace, přihlášení, resetování hesla) a interakci s důvěryhodná strana, jehož relace je také předdefinovaných (Azure AD B2C například Facebook zprostředkovatele identity, LinkedIn, Account Microsoft, Google účty).  V budoucích hello může taky poskytnout integrovaných zásad pro přizpůsobení zprostředkovatelů identity, které jsou obvykle v podnikové sféře hello například Azure Active Directory Premium, Active Directory nebo ADFS Salesforce ID zprostředkovatele.


### <a name="custom-policies"></a>vlastní zásady

Konfigurační soubory, které definují chování hello Framework prostředí Identity ve vašem klientovi Azure AD B2C. Vlastní zásady přístupný jako jednoho nebo několika souborů XML (viz definice soubory zásad), které jsou spouštěny hello Identity rozhraní Framework při vyvolání předávající strana (například aplikace). Vlastní zásady je možné přímo upravit toocomplete vývojáře identity near neomezený počet úloh. Vývojáři musí definovat vlastní zásady Konfigurace hello důvěryhodné vztahy v koncové body metadat tooinclude pečlivě podrobností, přesný deklarací exchange definice a konfigurace tajné klíče, klíče a certifikáty podle potřeby jednotlivých poskytovatele identit.

## <a name="policy-file-definitions-for-identity-experience-framework-trustframeworks"></a>Soubor definice zásady pro Trustframeworks Framework prostředí Identity

### <a name="policy-files"></a>Soubory zásad

Vlastní zásady je reprezentován jako jednoho nebo několika souborů ve formátu XML které tooeach jiné v hierarchické řetězu. elementy XML Hello definovat: schéma deklarace identity, deklarace transformace, obsahu definice, profily poskytovatelů a technické deklarace identity a Userjourney kroků Orchestrace, mezi další prvky.  Doporučujeme hello používá tři typy souborů zásad:

- **ZÁKLADNÍHO souboru**, který obsahuje většinu hello definic a pro které Azure poskytuje kompletní příklad.  Doporučujeme, že abyste vytvořili minimální počet změny toothis souboru toohelp při řešení problémů a dlouhodobé údržby zásad
- **soubor rozšíření** , obsahuje změny hello jedinečné konfigurace pro vašeho klienta
- **soubor předávající strany (RP)** tedy hello jeden zaměřené na úloh soubor, který je vyvolat přímo hello aplikace nebo služby (neboli předávající strany).  Přečtěte si článek hello v souboru definice zásad Další informace.  Každý úkol jedinečný vyžaduje vlastní RP a v závislosti na branding požadavky hello číslo může být "Celkový počet aplikací x celkový počet případů použití".

![Typy souborů zásad](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| Typ zásad souboru | Příklady název souboru | Doporučené použití | Dědí z |
|---------------------|--------------------|-----------------|---------------|
| ZÁKLADNÍ |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com. B2C 1A_BASE1.xml | Zahrnuje hello základní deklarace identity schématu, transformace deklarací, zprostředkovatelů deklarací identit a cesty uživatele konfigurovat tak, že Microsoft<br><br>Zajistěte minimální změny toothis soubor | Žádný |
| Rozšíření (EXT) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com. B2C 1A_EXT.xml | Vaše změny toohello základního souboru zde konsolidovat<br><br>Zprostředkovatelé upravené deklarací<br><br>Cesty upravené uživatele<br><br>Vlastní definice vlastní schéma | ZÁKLADNÍHO souboru |
| Předávající stranu | B2C_1A_sign_up_sign_in.XML| Token tvar a relace nastavení změnit tady| Soubor Extensions(ext) |

### <a name="inheritance-model"></a>Model dědičnosti

Pokud aplikace zavolá soubor zásad RP hello, hello Identity Framework prostředí v B2C přidáte všechny elementy hello ze základní a pak z rozšíření a nakonec z hello RP zásad souboru tooassemble hello aktuální zásady platit.  Elementy hello stejný typ a název souboru RP hello potlačí hello rozšíření a přepsání rozšíření základní.

**Předdefinované zásady** v Azure AD B2C, postupujte podle vzoru souboru 3 hello znázorněný výše, ale hello vývojáře jen vidí hello soubor předávající strany (RP), zatímco hello portál provede změny v souboru EXTenstions toohello pozadí hello.  Všechny Azure AD B2C sdílí základní zásady souboru, který je ve správě hello hello Azure B2C týmu a se často aktualizuje.
