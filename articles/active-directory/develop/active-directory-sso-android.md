---
title: "aaaHow tooenable jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL | Microsoft Docs"
description: "Jak funkce hello toouse hello ADAL SDK tooenable jednotné přihlašování v aplikacích. "
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
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
ms.openlocfilehash: 3867e15030e5516464e4dbd92ba35894430daf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-android-using-adal"></a>Jak tooenable jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL
Poskytuje jednotné přihlašování (SSO), aby uživatelé pouze potřebovat tooenter jejich přihlašovací údaje jednou a mají tyto přihlašovací údaje automaticky pracovní aplikace je nyní očekávanou zákazníků. Hello potíže při zadání uživatelského jména a hesla na malou obrazovku, často časy v kombinaci s další faktor (2FA) jako telefonní hovor nebo kód zasílání zpráv SMS, má za následek rychlé nespokojenosti, pokud uživatel má toodo to více než jednou pro svůj produkt.

Kromě toho pokud použijete identity platforma, která mohou používat jiné aplikace například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávat, že tyto přihlašovací údaje toobe dostupné toouse na všechny svoje aplikace bez ohledu na dodavatele hello.

Hello platformy Microsoft Identity, společně s naše sady SDK Microsoft Identity funguje všechny tento pevný a poskytuje hello možnost toodelight zákazníků pomocí jednotného přihlašování, buď v rámci vlastní sadu aplikací, nebo jako s naše schopností zprostředkovatele a Authenticator aplikacemi, v rámci celého zařízení hello.

Tento návod vám oznámí, jak tooconfigure naše sady SDK v rámci vaší aplikace tooprovide této výhody tooyour zákazníků.

Tento postup platí pro:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory s B2B
* Podmíněný přístup k Azure Active Directory

předchozí Hello dokumentu se předpokládá, víte, jak příliš[zřídit aplikace hello starší verze portálu pro Azure Active Directory](active-directory-how-to-integrate.md) a integraci aplikace s hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) .

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a>Koncepty jednotné přihlašování v hello Microsoft Identity Platform
### <a name="microsoft-identity-brokers"></a>Zprostředkovatelé Microsoft Identity
Microsoft poskytuje aplikace pro každou platformu mobilních, které umožňují hello přemostění přihlašovacích údajů napříč aplikacemi od různých dodavatelů a umožňuje pro speciální rozšířené funkce, které vyžadují jeden bezpečné místo, odkud toovalidate přihlašovací údaje. Tyto říkáme **makléřům**. Na iOS a Android tyto položky jsou poskytovány prostřednictvím ke stažení aplikací, aby zákazníci nainstalovat nezávisle na, nebo můžete poslat toohello zařízení ve společnosti, který spravuje některé nebo všechny hello zařízení pro své zaměstnance. Správa zabezpečení těchto zprostředkovatelé podporují jenom pro některé aplikace nebo hello zařízení podle potřeby co správci IT. V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému toohello, technicky říká hello zprostředkovatele webového ověření.

Další informace o tom, použijeme tyto zprostředkovatelé a jak vaši zákazníci mohou zobrazit je v jejich toku přihlášení pro platformu Microsoft Identity hello číst na.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Vzory pro přihlašování na mobilních zařízeních
Toocredentials přístup na zařízení podle dva základní vzory pro platformu Microsoft Identity hello:

* Přihlášení odbornou bez zprostředkovatele
* Zprostředkovatel odbornou přihlášení

#### <a name="non-broker-assisted-logins"></a>Přihlášení odbornou bez zprostředkovatele
Přihlášení odbornou non-broker jsou přihlášení prostředí, které dojít vložené s hello aplikací a použít místní úložiště hello na hello zařízení pro tuto aplikaci. Toto úložiště může být sdíleny s aplikací, ale hello přihlašovací údaje jsou úzce vázané toohello aplikace nebo sadu aplikací pomocí tohoto pověření. Jste s největší pravděpodobností došlo k to v mnoha mobilních aplikacích. když zadáte uživatelské jméno a heslo v rámci vlastní aplikace hello.

Tyto přihlášení mít hello následující výhody:

* Činnost koncového uživatele existuje zcela v rámci aplikace hello.
* Přihlašovací údaje můžete sdílet mezi aplikací, které jsou podepsány hello stejný certifikát, poskytuje prostředí přihlašování tooyour sady aplikací.
* Ovládací prvek kolem hello prostředí přihlášení je k dispozici toohello aplikace před a po přihlášení.

Tyto přihlášení mít hello následující nevýhody:

* Uživatele nelze jednotného přihlašování napříč všechny aplikace, které používají Microsoft Identity pouze v rámci těchto Identities Microsoft, které má vaše aplikace nakonfigurovaná.
* Aplikaci nelze použít s pokročilejší obchodní funkce jako je například podmíněný přístup nebo použití hello InTune sadu produktů.
* Aplikace nepodporuje ověřování pomocí certifikátů pro uživatele.

Zde je reprezentace jak hello Microsoft Identity sady SDK pracovat s hello sdílené úložiště vaše aplikace tooenable jednotné přihlašování:

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
S pomocí zprostředkovatele přihlášení jsou přihlášení prostředí, v rámci aplikace hello zprostředkovatele, které používají úložiště hello a zabezpečení hello zprostředkovatele tooshare přihlašovací údaje ve všech aplikacích na hello zařízení, které se vztahují hello platformy Microsoft Identity. To znamená, že vaše aplikace závisí na hello zprostředkovatele toosign uživatele v. Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete poslat toohello zařízení ve společnosti, který spravuje hello zařízení pro svoje uživatele. Příkladem tento typ aplikace je aplikace Microsoft Authenticator hello v systému iOS. V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému toohello, technicky říká hello zprostředkovatele webového ověření.
Hello prostředí se liší podle platformy a v některých případech může být rušivý toousers není správně spravovat. Jste pravděpodobně nejvíce obeznámeni s tento vzor, pokud máte nainstalovanou aplikací hello Facebook a použijete Facebook připojit z jiné aplikace. Hello používá platformy Microsoft Identity hello stejného vzoru.

Pro iOS důsledkem tooa "přechodu" animace kam aplikace je odeslána toohello pozadí při aplikace Microsoft Authenticator hello dodává popředí toohello pro uživatele tooselect hello účtu, který mu toosign s.  

Pro Android a Windows hello účet výběru, zobrazí se na aplikace, což je méně rušivý toohello uživatele.

#### <a name="how-hello-broker-gets-invoked"></a>Jak získá vyvolána hello zprostředkovatele
Pokud je v hello zařízení, jako jsou hello Microsoft Authenticator aplikace hello SDK služby Microsoft Identity bude automaticky nainstalován kompatibilní zprostředkovatele hello pracovní vyvolání hello zprostředkovatele pro vás, když uživatel označuje, že si přejí toolog pomocí libovolného účtu z Platforma Microsoft Identity Hello. Tento účet může být osobní Account Microsoft, pracovní nebo školní účet, nebo účtu, který zadáte a hostitele v Azure pomocí našich produktů B2C a B2B. 
 
 #### <a name="how-we-ensure-hello-application-is-valid"></a>Jak jsme zkontrolujte aplikace hello je platný
 
 Hello nutné tooensure hello identita broker hello volání aplikace je zásadní toohello zabezpečení, které poskytujeme v s pomocí zprostředkovatele přihlášení. IOS ani Android vynucuje jedinečné identifikátory, které jsou platné pouze pro danou aplikaci, tak, aby škodlivé aplikace může "zfalšovat" identifikátor legitimní aplikace a přijímat tokeny hello určená pro aplikaci legitimní hello. tooensure, které jsme vždy komunikují s hello správné aplikace za běhu, požádáme hello vývojáře tooprovide vlastní redirectURI při registraci své aplikace se společností Microsoft. **Jak vývojáři měli vytvořit tento identifikátor URI pro přesměrování je podrobněji níže.** Tato vlastní redirectURI obsahuje kryptografický otisk certifikátu hello hello aplikace a je zajištěna toobe jedinečný toohello aplikace hello Google Play Storu. Pokud aplikace volá zprostředkovatele hello hello zprostředkovatele požádá tooprovide operační systém Android hello její hello kryptografický otisk certifikátu tohoto zprostředkovatele volané hello. Zprostředkovatel Hello poskytuje tooMicrosoft kryptografický otisk tohoto certifikátu v hello volání tooour identity systému. Pokud během registrace zadat toous vývojářem hello hello certifikát, že kryptografický otisk aplikace hello neodpovídá hello kryptografický otisk certifikátu, jsme odepře přístup toohello tokeny pro hello prostředků hello aplikace požaduje. Tato kontrola zajistí, že pouze hello aplikace registrovaných hello vývojáře přijímá tokeny.

**Hello vývojáře má hello volbu, pokud hello Microsoft Identity SDK volá zprostředkovatele hello nebo používá odbornou toku hello bez zprostředkovatele.** Ale pokud hello vývojáře vybere není toouse hello s asistencí služby broker toku nich došlo ke ztrátě výhodou hello pomocí jednotného přihlašování k přihlašovacích údajů tohoto uživatele hello může mít již přidán do zařízení hello a zabrání jejich aplikaci z používá s funkcemi obchodní Microsoft nabízí svým zákazníkům, jako je například podmíněný přístup, možnosti správy Intune a ověřování pomocí certifikátů.

Tyto přihlášení mít hello následující výhody:

* Uživatel narazí na všechny svoje aplikace bez ohledu na dodavatele hello jednotné přihlašování.
* Aplikace můžete použít dalších pokročilých funkcí firmy, jako je například podmíněný přístup nebo pomocí sady InTune hello produktů.
* Aplikace může podporovat ověřování pomocí certifikátů pro uživatele.
* Mnohem bezpečnější přihlašování hello identity aplikace hello a hello uživatele jsou ověřené aplikací hello zprostředkovatele s další bezpečnostní algoritmů hash a šifrování.

Tyto přihlášení mít hello následující nevýhody:

* V iOS převedena hello uživatele mimo prostředí vaší aplikace, když je možné zvolit přihlašovací údaje.
* Ztrátě hello možnost toomanage hello přihlašovací prostředí pro vaši zákazníci v rámci vaší aplikace.

Zde je reprezentace jak pracují SDK služby Microsoft Identity hello se hello zprostředkovatel aplikace tooenable jednotné přihlašování:

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

Díky této základní informace musí být schopný toobetter pochopit a implementovat jednotné přihlašování v rámci vaší aplikace pomocí platformy Microsoft Identity hello a sady SDK.

## <a name="enabling-cross-app-sso-using-adal"></a>Povolení jednotného přihlašování napříč aplikacemi pomocí ADAL
Tady používáme hello ADAL sady SDK pro Android na:

* Zapněte bez zprostředkovatele s pomocí jednotného přihlašování pro vaše sada aplikací
* Zapnutí podpory pro jednotné přihlašování s asistencí zprostředkovatele

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Zapnout jednotné přihlašování pro bez zprostředkovatele s pomocí jednotného přihlašování
Bez zprostředkovatele odbornou jednotné přihlašování napříč aplikacemi spravovat hello SDK služby Microsoft Identity mnohem složitější hello přihlašování pro vás. To zahrnuje hello správné uživatelské hledání v mezipaměti hello a udržování seznam přihlášeného uživatele pro tooquery je.

tooenable jednotného přihlašování napříč aplikacemi, které vlastníte, které že budete potřebovat toodo hello následující:

1. Ujistěte se, všechna vaše aplikace uživatele hello stejné ID klienta, nebo ID aplikace.
2. Zajistěte, aby že všechny aplikace mají hello stejné SharedUserID nastavit.
3. Zajistěte, aby všechny vaše aplikace hello sdílené složky stejný podpisový certifikát z hello Google Play úložiště tak, že můžete sdílet úložiště.

#### <a name="step-1-using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a>Krok 1: Použití hello stejné ID klienta / ID aplikace pro všechny aplikace ve vaší sadě aplikace hello
Aby tooknow platformy Microsoft Identity hello, že je povolená tooshare tokeny mezi aplikací každý z vašich aplikací bude nutné tooshare hello stejné ID klienta, nebo ID aplikace. Toto je hello jedinečný identifikátor, který byl poskytnut tooyou při registraci vaší první aplikace hello portálu.

Asi vás zajímá, jak bude identifikujete různé aplikace hello toohello služba Microsoft Identity, pokud používá stejné ID aplikace je Hello odpověď s hello **identifikátory URI přesměrování**. Každá aplikace může mít několik přesměrování identifikátory URI registrován v portálu registrace hello. Každá aplikace ve vaší sadě bude mít na jiný identifikátor URI přesměrování. Zde je příklad, jak to vypadá:

Identifikátor URI přesměrování app1:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

Identifikátor URI přesměrování počítači App2:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

Identifikátor URI přesměrování App3:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

....

Tyto jsou vnořeny pod stejným ID klienta hello / ID aplikace a vyhledávat na základě hello redirect URI vrátíte toous v konfiguraci sady SDK.

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


*Všimněte si, že hello formát tyto identifikátory URI přesměrování jsou vysvětleny níže. Může použít jakékoli identifikátor URI pro přesměrování, pokud chcete toosupport hello zprostředkovatele, v takovém případě se musí vypadat podobně jako výše hello*

#### <a name="step-2-configuring-shared-storage-in-android"></a>Krok 2: Konfigurace sdílené úložiště v Android
Nastavení hello `SharedUserID` , je nad rámec hello rámec tohoto dokumentu, ale je možné zjistit načtením hello Google Android dokumentaci na hello [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Co je důležité se rozhodnout, jaké má vaše sharedUserID bude volána a použít na všechny aplikace.

Jakmile máte hello `SharedUserID` ve svých aplikacích jsou připravené toouse jednotné přihlašování.

> [!WARNING]
> Při sdílení úložišť na vaší aplikace pro všechny aplikace můžete odstranit uživatele nebo horší odstranit všechny tokeny hello napříč aplikací. To je zvlášť katastrofální, pokud máte aplikace, které jsou závislé na práce pozadí toodo tokeny hello. Sdílení úložiště znamená, že musí být velmi opatrní při odebrání všech operací prostřednictvím hello SDK služby Microsoft Identity.
> 
> 

A to je vše! Hello Microsoft Identity SDK bude nyní sdílet přihlašovací údaje ve všech aplikací. seznam uživatelů Hello bude také sdílet mezi instancemi aplikace.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Zapnout jednotné přihlašování pro zprostředkovatele s pomocí jednotného přihlašování
Hello schopnost aplikaci toouse všechny zprostředkovatele, který je nainstalován na zařízení hello je **ve výchozím nastavení vypnuté**. V pořadí toouse vaší aplikace pomocí zprostředkovatele hello musí provést některé další konfiguraci a přidejte některé aplikaci tooyour kódu.

toofollow Hello kroky jsou:

1. Povolit režim zprostředkovatele v kódu aplikace volání toohello MS SDK
2. Vytvořit nový identifikátor URI přesměrování a zadejte aplikaci hello tooboth a registrace aplikace
3. Nastavení hello správná oprávnění v hello manifestu systému Android.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Krok 1: Povolení režimu zprostředkovatele v aplikaci
Hello možnost pro vaše aplikace toouse hello broker zapnutý, při vytváření hello "nastavení" nebo počáteční nastavení vaší instance ověřování. To provedete nastavením vašeho typu ApplicationSettings ve vašem kódu:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Krok 2: Vytvoření na nový identifikátor URI s vaše schéma adresy URL přesměrování
V pořadí tooensure, že se vždy vrací, že pověření hello tokeny toohello správnou aplikaci potřebujeme toomake se, že jsme zpětné volání, můžete ověřit tooyour aplikace tak, aby hello operační systém Android. operační systém Android Hello používá hello algoritmus hash certifikátu hello v hello obchodu Google Play. To nelze maskování podvodný aplikace. Proto jsme využít to společně s hello URI naše tooensure aplikace zprostředkovatele že hello tokeny jsou vráceny toohello správnou aplikaci. Je nutné, můžete tooestablish tento jedinečný identifikátor URI pro přesměrování jak v aplikaci a sadu jako identifikátor URI přesměrování v našem portál pro vývojáře.

Váš identifikátor URI pro přesměrování musí být ve formátu správné hello:

`msauth://packagename/Base64UrlencodedSignature`

například: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Tento identifikátor URI pro přesměrování musí toobe zadaný v registraci vaší aplikace pomocí hello [portál Azure](https://portal.azure.com/). Další informace o registraci aplikace Azure AD najdete v tématu [integraci s Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-hello-correct-permissions-in-your-application"></a>Krok 3: Nastavení hello správná oprávnění v aplikaci
Naše aplikace zprostředkovatele v Android používá funkce Správce účtů hello hello operační systém Android toomanage přihlašovacích údajů napříč aplikacemi. V pořadí toouse hello zprostředkovatele Android manifest aplikace musí mít oprávnění toouse AccountManager účty. To je podrobněji v hello [zde Google dokumentace pro účet správce](http://developer.android.com/reference/android/accounts/AccountManager.html)

Konkrétně tato oprávnění jsou:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Jednotné přihlašování jste nakonfigurovali!
Nyní hello Microsoft Identity SDK budou automaticky sdílet přihlašovací údaje v rámci aplikace i vyvolání zprostředkovatele hello, pokud je k dispozici na svém zařízení.

