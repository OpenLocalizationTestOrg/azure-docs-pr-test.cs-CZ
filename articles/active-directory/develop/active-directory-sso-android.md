---
title: "Postup povolení jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL | Microsoft Docs"
description: "Jak používat funkce sady ADAL SDK povolit jednotné přihlašování v rámci vaší aplikace. "
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mtillman
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 7d832ecf3e9c64088a75cc88551879b4e09df715
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Postup povolení jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL
Pokud jednotné přihlašování (SSO), aby uživatelé stačí jednou zadat své přihlašovací údaje a mají tyto přihlašovací údaje automaticky fungovat na všech aplikací nyní očekává zákazníků. Problémy se zadáním uživatelského jména a hesla na malou obrazovku, často časy v kombinaci s další faktor (2FA) jako telefonní hovor nebo kód zasílání zpráv SMS, má za následek rychlé nespokojenosti, pokud uživatel má k tomu více než jednou pro svůj produkt.

Kromě toho pokud použijete identity platforma, která mohou používat jiné aplikace například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávají, že ty pověření být k dispozici pro použití na všechny svoje aplikace bez ohledu na dodavatele.

Do platformy Microsoft Identity, společně s naše sady SDK Identity Microsoft nepodporuje tento pevný pracovní pro vás a vám dává možnost delight zákazníků pomocí jednotného přihlašování, buď v rámci vlastní sada aplikací nebo, stejně jako u našich schopností zprostředkovatele a ověřovací aplikacemi, v rámci celého zařízení.

Tento návod popisuje postup konfigurace naše sady SDK v rámci aplikace umožní vašim zákazníkům poskytovat této výhody.

Tento postup platí pro:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory s B2B
* Podmíněný přístup k Azure Active Directory

Předchozí dokument předpokládá, že víte, jak [zřídit aplikace na portálu pro starší verze pro Azure Active Directory](active-directory-how-to-integrate.md) a integrované aplikace s [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Koncepty jednotné přihlašování v platformě Microsoft Identity
### <a name="microsoft-identity-brokers"></a>Zprostředkovatelé Microsoft Identity
Společnost Microsoft poskytuje aplikace pro každou platformu mobilních umožňujících přemostění přihlašovacích údajů napříč aplikacemi od různých dodavatelů a umožňuje pro speciální rozšířené funkce, které vyžadují jeden bezpečné místo, odkud se ověřit přihlašovací údaje. Tyto říkáme **makléřům**. Na iOS a Android tyto položky jsou poskytovány prostřednictvím ke stažení aplikací, aby zákazníci nainstalovat nezávisle na, nebo můžete nabídnutých do zařízení ve společnosti, který spravuje některých nebo všech zařízení pro své zaměstnance. Správa zabezpečení těchto zprostředkovatelé podporují jenom pro některé aplikace nebo ze zařízení podle potřeby co správci IT. V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému, známé technicky jako zprostředkovatele webového ověření.

Další informace o tom, použijeme tyto zprostředkovatelé a jak vaši zákazníci mohou zobrazit je v jejich toku přihlášení pro čtení na platformu Microsoft Identity.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Vzory pro přihlašování na mobilních zařízeních
Přístup k přihlašovacím údajům v zařízení podle dva základní vzory pro platformu Microsoft Identity:

* Přihlášení odbornou bez zprostředkovatele
* Zprostředkovatel odbornou přihlášení

#### <a name="non-broker-assisted-logins"></a>Přihlášení odbornou bez zprostředkovatele
Přihlášení odbornou non-broker jsou možností přihlášení, které dojít vložené s aplikací a použít místní úložiště na zařízení pro tuto aplikaci. Toto úložiště může být sdíleny s aplikací, ale přihlašovací údaje jsou úzce vázaný aplikace nebo sadu aplikací pomocí tohoto pověření. Jste s největší pravděpodobností došlo k to v mnoha mobilních aplikacích. když zadáte uživatelské jméno a heslo v rámci vlastní aplikace.

Tyto přihlášení mají následující výhody:

* Činnost koncového uživatele zcela v aplikaci existuje.
* Přihlašovací údaje můžete sdílet mezi aplikací, které jsou podepsány stejný certifikát, poskytování jeden přihlašování pro vaše sada aplikací.
* Ovládací prvek kolem možností přihlášení je k dispozici do aplikace před a po přihlášení.

Tyto přihlášení mají tyto nevýhody:

* Uživatele nelze jednotného přihlašování napříč všechny aplikace, které používají Microsoft Identity pouze v rámci těchto Identities Microsoft, které má vaše aplikace nakonfigurovaná.
* Aplikaci nelze použít s dalších pokročilých funkcí firmy, jako je například podmíněný přístup, nebo použijte produktů sady InTune.
* Aplikace nepodporuje ověřování pomocí certifikátů pro uživatele.

Zde je reprezentace fungování sadami SDK služby Microsoft Identity se sdíleným úložištěm aplikací k povolení přihlášení SSO:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Zprostředkovatel odbornou přihlášení
S pomocí zprostředkovatele přihlášení jsou přihlášení prostředí, v rámci zprostředkovatele aplikace, které používají úložiště a security zprostředkovatele sdílet přihlašovací údaje ve všech aplikací v zařízení, které se vztahují na platformu Microsoft Identity. To znamená, že vaše aplikace závisí na broker k přihlášení uživatele. Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete nabídnutých do zařízení ve společnosti, který spravuje zařízení pro svoje uživatele. Příkladem tento typ aplikace je aplikace Microsoft Authenticator v systému iOS. V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému, známé technicky jako zprostředkovatele webového ověření.
Možnosti se liší podle platformy a v některých případech můžou narušovat běh produktu uživatelům není správně spravovat. Jste pravděpodobně nejvíce obeznámeni s tento vzor, pokud máte nainstalovanou aplikací služby Facebook a použijete Facebook připojit z jiné aplikace. Platforma Microsoft Identity používá stejného vzoru.

Pro iOS, které to vede k "přechodu" animace, kdy se vaše aplikace odesílají na pozadí při aplikace Microsoft Authenticator obsahuje popředí pro uživatele k výběru účtu, který se chcete přihlásit.  

Pro Android a Windows výběru účtu, zobrazí se na aplikace, což je méně rušivý uživateli.

#### <a name="how-the-broker-gets-invoked"></a>Jak se zprostředkovatel volán
Pokud je kompatibilní zprostředkovatel nainstalovaný na zařízení, jako je aplikace Microsoft Authenticator sadami SDK služby Microsoft Identity automaticky provede práci při vyvolání zprostředkovatele pro vás, když uživatel označuje, že se chcete přihlásit pomocí libovolného účtu z platformy Microsoft Identity. Tento účet může být osobní Account Microsoft, pracovní nebo školní účet, nebo účtu, který zadáte a hostitele v Azure pomocí našich produktů B2C a B2B. 
 
 #### <a name="how-we-ensure-the-application-is-valid"></a>Jakým způsobem je zajištěno aplikace je platný
 
 Je třeba zajistit identity volání aplikace, které je zásadní zabezpečení, které poskytujeme ve zprostředkovateli zprostředkovatele s asistencí přihlášení. IOS ani Android vynucuje jedinečné identifikátory, které jsou platné pouze pro danou aplikaci, tak, aby škodlivé aplikace může "zfalšovat" identifikátor legitimní aplikace a přijímat tokeny určená pro oprávněné aplikaci. Ujistěte se, že jsme vždy komunikují pomocí správné aplikace za běhu, požádáme vývojáři poskytnout vlastní redirectURI při registraci své aplikace se společností Microsoft. **Jak vývojáři měli vytvořit tento identifikátor URI pro přesměrování je podrobněji níže.** Tato vlastní redirectURI obsahuje kryptografický otisk certifikátu aplikace a je zajištěna jedinečnost do aplikace obchod Google Play. Pokud aplikace zavolá zprostředkovatele, požádá zprostředkovatele operační systém Android poskytnout kryptografický otisk certifikátu, který volá zprostředkovatele. Zprostředkovatel poskytuje kryptografický otisk tohoto certifikátu společnosti Microsoft ve volání naše systém identit. Pokud kryptografický otisk certifikátu aplikace neodpovídá kryptografický otisk certifikátu uvedenou nám vývojáře při registraci, jsme odepře přístup na tokeny pro prostředek, který aplikace požaduje. Tato kontrola zajistí, že pouze aplikace, které jsou zaregistrované vývojáře přijímá tokeny.

**Vývojář musí volba, pokud sada SDK Microsoft Identity volá zprostředkovatele nebo používá odbornou toku bez zprostředkovatele.** Ale pokud vývojář vybere nepoužívat toku s asistencí služby broker nich došlo ke ztrátě výhodou použití jednotného přihlašování k přihlašovací údaje, které uživatel může na zařízení již přidali a zabrání jejich aplikaci z používaný s funkcí business, které společnost Microsoft poskytuje svým zákazníkům, jako je například podmíněný přístup, možnosti správy Intune a ověřování pomocí certifikátů.

Tyto přihlášení mají následující výhody:

* Uživatel vyskytne jednotného přihlašování na všechny svoje aplikace bez ohledu na dodavatele.
* Aplikace můžete použít dalších pokročilých funkcí firmy, jako je například podmíněný přístup nebo používat sadu InTune produktů.
* Aplikace může podporovat ověřování pomocí certifikátů pro uživatele.
* Mnohem bezpečnější přihlašovat jako identita aplikace a uživatel se ověřit pomocí zprostředkovatele aplikace s další bezpečnostní algoritmů hash a šifrování.

Tyto přihlášení mají tyto nevýhody:

* V iOS převedena uživatele mimo prostředí vaší aplikace, když je možné zvolit přihlašovací údaje.
* Ztráta schopnost spravovat přihlašování pro vaši zákazníci v rámci vaší aplikace.

Zde je reprezentace fungování sadami SDK služby Microsoft Identity s aplikacemi zprostředkovatele k povolení přihlášení SSO:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Díky této základní informace, které byste měli mít lépe pochopit a implementovat jednotné přihlašování v rámci vaší aplikace pomocí platformy Microsoft Identity a sady SDK.

## <a name="enabling-cross-app-sso-using-adal"></a>Povolení jednotného přihlašování napříč aplikacemi pomocí ADAL
Tady používáme ADAL Android SDK:

* Zapněte bez zprostředkovatele s pomocí jednotného přihlašování pro vaše sada aplikací
* Zapnutí podpory pro jednotné přihlašování s asistencí zprostředkovatele

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Zapnout jednotné přihlašování pro bez zprostředkovatele s pomocí jednotného přihlašování
Bez zprostředkovatele odbornou jednotné přihlašování napříč aplikacemi spravovat sadami SDK služby Microsoft Identity velkou část složitosti jednotného přihlašování pro vás. To zahrnuje správné uživatelské hledání v mezipaměti a udržování seznam přihlášeného uživatele pro vás k dotazování.

K povolení jednotného přihlašování napříč aplikacemi, které vlastníte, že musíte udělat následující:

1. Zkontrolujte všechny uživatelské aplikace stejné ID klienta nebo ID aplikace.
2. Zajistěte, aby že všechny aplikace mají stejnou sadu SharedUserID.
3. Ujistěte se, že všechny aplikace sdílet stejný podpisový certifikát z obchodu Google Play tak, že můžete sdílet úložiště.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Krok 1: Použití stejné ID klienta / ID aplikace pro všechny aplikace ve vaší sadě aplikací
V pořadí pro platformu Microsoft Identity vědět, že má povolené sdílet tokeny ve vašich aplikací každý z vašich aplikací bude nutné sdílejí stejné ID klienta nebo ID aplikace. Toto je jedinečný identifikátor, který jste získali při registraci vaší první aplikace v portálu.

Asi vás zajímá, jak bude identifikujete různé aplikace ke službě Microsoft Identity, pokud používá stejné ID aplikace Je odpověď **identifikátory URI přesměrování**. Každá aplikace může mít několik přesměrování identifikátory URI registrován v portálu registrace. Každá aplikace ve vaší sadě bude mít na jiný identifikátor URI přesměrování. Zde je příklad, jak to vypadá:

Identifikátor URI přesměrování app1:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

Identifikátor URI přesměrování počítači App2:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

Identifikátor URI přesměrování App3:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

....

Tyto jsou vnořeny pod stejným ID klienta nebo ID aplikace a vyhledávat podle identifikátor URI návratu do us ve vaší konfiguraci SDK přesměrování.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Všimněte si, že formát tyto identifikátory URI přesměrování jsou vysvětleny níže. Můžete použít všechny URI přesměrování, pokud chcete pro podporu zprostředkovatele, v takovém případě se musí vypadat podobně jako výše*

#### <a name="step-2-configuring-shared-storage-in-android"></a>Krok 2: Konfigurace sdílené úložiště v Android
Nastavení `SharedUserID` je nad rámec tohoto dokumentu, ale je možné zjistit přečíst v dokumentaci Google Android na [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Co je důležité se rozhodnout, jaké má vaše sharedUserID bude volána a použít na všechny aplikace.

Jakmile máte `SharedUserID` ve svých aplikacích budete chtít používat jednotné přihlašování.

> [!WARNING]
> Při sdílení úložišť na vaší aplikace pro všechny aplikace můžete odstranit uživatele nebo horší odstranit všechny tokeny napříč aplikací. To je zvlášť katastrofální, pokud máte aplikace, které jsou závislé na tokeny pro práci na pozadí. Sdílení úložiště znamená, že musí být velmi opatrní při odebrání všech operací prostřednictvím sadami SDK služby Microsoft Identity.
> 
> 

A to je vše! Sada SDK Microsoft Identity bude nyní sdílet přihlašovací údaje ve všech aplikací. Seznam uživatelů bude také sdílet mezi instancemi aplikace.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Zapnout jednotné přihlašování pro zprostředkovatele s pomocí jednotného přihlašování
Možnost pro aplikace pro použití žádné zprostředkovatele, který je nainstalován na zařízení je **ve výchozím nastavení vypnuté**. Chcete-li použít vaší aplikace pomocí zprostředkovatele musí provést některé další konfiguraci a přidejte nějaký kód do vaší aplikace.

Jak postupovat, jsou:

1. Povolit režim zprostředkovatele v kódu aplikace volání sady SDK MS
2. Vytvořit nový identifikátor URI přesměrování a stanovit, že aplikace a registrace aplikace
3. Nastavení správná oprávnění v manifestu systému Android.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Krok 1: Povolení režimu zprostředkovatele v aplikaci
Možnosti pro aplikace pomocí zprostředkovatele zapnutý, při vytváření "nastavení" nebo počáteční nastavení vaší instance ověřování. To provedete nastavením vašeho typu ApplicationSettings ve vašem kódu:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Krok 2: Vytvoření na nový identifikátor URI s vaše schéma adresy URL přesměrování
Aby se zajistilo, že vrátíme vždy tokeny přihlašovacích údajů pro správnou aplikaci, musíme Ujistěte se, že jsme zpětné volání do vaší aplikace tak, aby operační systém Android můžete ověřit. Operační systém Android používá hodnotu hash certifikátu v obchodě Google Play. To nelze maskování podvodný aplikace. Proto jsme využít to společně se identifikátor URI aplikace zprostředkovatele k zajištění, že tokeny jsou vráceny správné aplikace. Je nutné vytvořit tento jedinečný identifikátor URI pro přesměrování oba ve vaší aplikaci a nastavit jako identifikátor URI přesměrování v našem portál pro vývojáře.

Váš identifikátor URI pro přesměrování musí být ve správném formátu:

`msauth://packagename/Base64UrlencodedSignature`

například: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Tento identifikátor URI pro přesměrování musí být zadána v registrace vaší aplikace pomocí [portál Azure](https://portal.azure.com/). Další informace o registraci aplikace Azure AD najdete v tématu [integraci s Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Krok 3: Nastavení správná oprávnění v aplikaci
Naše aplikace zprostředkovatele v Android používá funkci Správce účtů operační systém Android ke správě přihlašovacích údajů napříč aplikacemi. Chcete-li použít zprostředkovatele v Android manifest aplikace musí mít oprávnění k používání AccountManager účtů. To je podrobněji v [zde Google dokumentace pro účet správce](http://developer.android.com/reference/android/accounts/AccountManager.html)

Konkrétně tato oprávnění jsou:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Jednotné přihlašování jste nakonfigurovali!
Nyní Microsoft Identity SDK budou automaticky sdílet přihlašovací údaje v rámci aplikace i vyvolání zprostředkovatele, pokud je k dispozici na svém zařízení.

