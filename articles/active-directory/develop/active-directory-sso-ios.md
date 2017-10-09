---
title: "aaaHow tooenable jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL | Microsoft Docs"
description: "Jak funkce hello toouse hello ADAL SDK tooenable jednotné přihlašování v aplikacích. "
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: b7b4389a8dcd956211ffa1aaa431aaf21ded8961
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-ios-using-adal"></a>Jak tooenable jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL
Poskytuje jednotné přihlašování (SSO), aby uživatelé pouze potřebovat tooenter jejich přihlašovací údaje jednou a mají tyto přihlašovací údaje automaticky pracovní aplikace je nyní očekávanou zákazníků. Hello potíže při zadání uživatelského jména a hesla na malou obrazovku, často časy v kombinaci s další faktor (2FA) jako telefonní hovor nebo kód zasílání zpráv SMS, má za následek rychlé nespokojenosti, pokud uživatel má toodo to více než jednou pro svůj produkt.

Kromě toho pokud použijete identity platforma, která mohou používat jiné aplikace například Accounts Microsoft nebo pracovní účet z Office 365, zákazníci očekávat, že tyto přihlašovací údaje toobe dostupné toouse na všechny svoje aplikace bez ohledu na dodavatele hello.

Hello platformy Microsoft Identity, společně s naše sady SDK Microsoft Identity funguje všechny tento pevný a poskytuje hello možnost toodelight zákazníků pomocí jednotného přihlašování, buď v rámci vlastní sadu aplikací, nebo jako s naše schopností zprostředkovatele a Authenticator aplikacemi, v rámci celého zařízení hello.

Tento návod vám oznámí, jak tooconfigure naše sady SDK v rámci vaší aplikace tooprovide této výhody tooyour zákazníků.

Tento postup platí pro:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory s B2B
* Podmíněný přístup k Azure Active Directory

předchozí Hello dokumentu se předpokládá, víte, jak příliš[zřídit aplikace hello starší verze portálu pro Azure Active Directory](active-directory-how-to-integrate.md) a integraci aplikace s hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a>Koncepty jednotné přihlašování v hello Microsoft Identity Platform
### <a name="microsoft-identity-brokers"></a>Zprostředkovatelé Microsoft Identity
Microsoft poskytuje aplikace pro každou platformu mobilních, které umožňují hello přemostění přihlašovacích údajů napříč aplikacemi od různých dodavatelů a umožňuje pro speciální rozšířené funkce, které vyžadují jeden bezpečné místo, odkud toovalidate přihlašovací údaje. Tyto říkáme **makléřům**. Na iOS a Android tyto zprostředkovatelé jsou k dispozici ke stažení aplikace, aby zákazníci nainstalovat nezávisle na, nebo můžete poslat toohello zařízení ve společnosti, který spravuje některé nebo všechny hello zařízení pro své zaměstnance. Správa zabezpečení těchto zprostředkovatelé podporují jenom pro některé aplikace nebo hello zařízení podle potřeby co správci IT. V systému Windows je tato funkce poskytuje výběru účtu, který je součástí operačního systému toohello, technicky říká hello zprostředkovatele webového ověření.

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
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
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
 
 Hello nutné tooensure hello identita broker hello volání aplikace je zásadní toohello zabezpečení, které poskytujeme v s pomocí zprostředkovatele přihlášení. IOS ani Android vynucuje jedinečné identifikátory, které jsou platné pouze pro danou aplikaci, tak, aby škodlivé aplikace může "zfalšovat" identifikátor legitimní aplikace a přijímat tokeny hello určená pro aplikaci legitimní hello. tooensure, které jsme vždy komunikují s hello správné aplikace za běhu, požádáme hello vývojáře tooprovide vlastní redirectURI při registraci své aplikace se společností Microsoft. **Jak vývojáři měli vytvořit tento identifikátor URI pro přesměrování je podrobněji níže.** Tato vlastní redirectURI obsahuje hello ID sady prostředků aplikace hello a je zajištěna toobe jedinečný toohello aplikace hello Apple App Store. Když se aplikace volání hello broker, hello broker zeptá hello iOS operačního systému tooprovide hello její ID sady, který volá zprostředkovatele hello. Hello zprostředkovatele poskytuje toto ID sady tooMicrosoft systému identity tooour volání hello. Pokud hello ID sady prostředků aplikace hello neodpovídá hello poskytnuté ID sady toous vývojářem hello během registrace, jsme odepře přístup toohello tokeny pro hello prostředků hello aplikace požaduje. Tato kontrola zajistí, že pouze hello aplikace registrovaných hello vývojáře přijímá tokeny.

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
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
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
Tady používáme hello ADAL iOS SDK na:

* Zapněte bez zprostředkovatele s pomocí jednotného přihlašování pro vaše sada aplikací
* Zapnutí podpory pro jednotné přihlašování s asistencí zprostředkovatele

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Zapnout jednotné přihlašování pro bez zprostředkovatele s pomocí jednotného přihlašování
Bez zprostředkovatele odbornou jednotné přihlašování napříč aplikacemi spravovat hello SDK služby Microsoft Identity mnohem složitější hello přihlašování pro vás. To zahrnuje hello správné uživatelské hledání v mezipaměti hello a udržování seznam přihlášeného uživatele pro tooquery je.

tooenable jednotného přihlašování napříč aplikacemi, které vlastníte, které že budete potřebovat toodo hello následující:

1. Ujistěte se, všechna vaše aplikace uživatele hello stejné ID klienta, nebo ID aplikace.
2. Zajistěte, aby všechny vaše aplikace hello sdílené složky stejný podpisový certifikát od společnosti Apple, tak, že můžete sdílet řetězce klíčů
3. Žádosti o hello stejné nárocích řetězce klíčů pro každou z vašich aplikací.
4. Řekněte hello Microsoft Identity sady SDK o řetězce hello sdílených klíčů, že nám chcete toouse.

#### <a name="using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a>Pomocí hello stejné ID klienta / ID aplikace pro všechny aplikace ve vaší sadě aplikace hello
Aby tooknow platformy Microsoft Identity hello, že je povolená tooshare tokeny mezi aplikací každý z vašich aplikací bude nutné tooshare hello stejné ID klienta, nebo ID aplikace. Toto je hello jedinečný identifikátor, který byl poskytnut tooyou při registraci vaší první aplikace hello portálu.

Asi vás zajímá, jak bude identifikujete různé aplikace hello toohello služba Microsoft Identity, pokud používá stejné ID aplikace je Hello odpověď s hello **identifikátory URI přesměrování**. Každá aplikace může mít několik přesměrování identifikátory URI registrován v portálu registrace hello. Každá aplikace ve vaší sadě bude mít na jiný identifikátor URI přesměrování. Zde je příklad, jak to vypadá:

Identifikátor URI přesměrování app1:`x-msauth-mytestiosapp://com.myapp.mytestapp`

Identifikátor URI přesměrování počítači App2:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

Identifikátor URI přesměrování App3:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

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

#### <a name="create-keychain-sharing-between-applications"></a>Vytvořte sdílení mezi aplikacemi řetězce klíčů
Povolení sdílení řetězce klíčů je nad rámec tohoto dokumentu hello a jejich dokument nezabývá společností Apple [přidání schopností](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Co je důležité se rozhodnout, jak vaše toobe řetězce klíčů, názvem a přidejte tuto funkci u všech aplikací.

Pokud máte oprávnění nastavit správně jste měli vidět soubor v adresáři projektu s názvem `entitlements.plist` obsahující něco, co vypadá hello následující:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Jakmile máte nárok řetězce klíčů hello povolené v každé z vašich aplikací a jsou připravené toouse jednotné přihlašování, řekněte hello Microsoft Identity SDK o vaší řetězce klíčů pomocí hello následující nastavení v vaší `ADAuthenticationSettings` s hello následující nastavení:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Když sdílíte mezi vaší aplikace řetězce klíčů všechny aplikace můžete odstranit uživatele nebo horší odstranit všechny tokeny hello napříč aplikací. To je zvlášť katastrofální, pokud máte aplikace, které jsou závislé na práce pozadí toodo tokeny hello. Sdílení řetězce klíčů odebrat znamená, že musí být velmi opatrní při všech operací prostřednictvím sady SDK Identity Microsoft hello.
> 
> 

A to je vše! Hello Microsoft Identity SDK bude nyní sdílet přihlašovací údaje ve všech aplikací. seznam uživatelů Hello bude také sdílet mezi instancemi aplikace.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Zapnout jednotné přihlašování pro zprostředkovatele s pomocí jednotného přihlašování
Hello schopnost aplikaci toouse všechny zprostředkovatele, který je nainstalován na zařízení hello je **ve výchozím nastavení vypnuté**. V pořadí toouse vaší aplikace pomocí zprostředkovatele hello musí provést některé další konfiguraci a přidejte některé aplikaci tooyour kódu.

toofollow Hello kroky jsou:

1. Povolte režim zprostředkovatele v kódu aplikace volání toohello MS SDK.
2. Vytvořit nový identifikátor URI přesměrování a zadejte aplikaci hello tooboth a registrace aplikace.
3. Probíhá registrace schéma adresy URL.
4. Podpora iOS9: přidání souboru info.plist tooyour oprávnění.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Krok 1: Povolení režimu zprostředkovatele v aplikaci
Hello možnost pro vaše aplikace toouse hello broker zapnutý, při vytváření kontextu"hello" nebo počáteční nastavení ověřování objektu. To provedete nastavením typ vaše přihlašovací údaje ve vašem kódu:

```
/*! See hello ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Hello `AD_CREDENTIALS_AUTO` nastavení povolí hello Microsoft Identity SDK tootry toocall out toohello zprostředkovatele `AD_CREDENTIALS_EMBEDDED` zabrání volání zprostředkovatele toohello hello Microsoft Identity SDK.

#### <a name="step-2-registering-a-url-scheme"></a>Krok 2: Registrace schéma adresy URL
Platforma Microsoft Identity Hello používá zprostředkovatele hello tooinvoke adresy URL a pak návratový řízení back tooyour aplikace. toofinish tuto dobu odezvy, je nutné schéma adresy URL zaregistrován pro vaši aplikaci této hello Microsoft Identity platformy bude vědět o. To může být kromě tooany jiná schémata aplikací mít může dříve registrován u vaší aplikace.

> [!WARNING]
> Doporučujeme nastavit adresu URL schéma poměrně jedinečný toominimize hello pravděpodobnost jiné aplikaci pomocí hello hello stejné schéma adresy URL. Apple nevynucuje hello jedinečnosti schémata URL, které jsou zaregistrovány v obchodě s aplikacemi hello.
> 
> 

Níže je příklad, jak se objeví v konfigurace projektu. Také to lze provést v prostředí XCode také:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Krok 3: Stanovení na nový identifikátor URI s vaše schéma adresy URL přesměrování
V pořadí tooensure, že se vždy vrací, že pověření hello tokeny toohello správnou aplikaci potřebujeme toomake se, že jsme zpětné volání, můžete ověřit tooyour aplikace tak, aby hello operačního systému iOS. Zprostředkovatel aplikací společnosti Microsoft sestavy toohello Hello iOS operačního systému hello ID sady prostředků aplikace hello volání ho. To nelze maskování podvodný aplikace. Proto jsme využít to společně s hello URI naše tooensure aplikace zprostředkovatele že hello tokeny jsou vráceny toohello správnou aplikaci. Je nutné, můžete tooestablish tento jedinečný identifikátor URI pro přesměrování jak v aplikaci a sadu jako identifikátor URI přesměrování v našem portál pro vývojáře.

Váš identifikátor URI pro přesměrování musí být ve formátu správné hello:

`<app-scheme>://<your.bundle.id>`

například: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Tento identifikátor URI pro přesměrování musí toobe zadaný v registraci vaší aplikace pomocí hello [portál Azure](https://portal.azure.com/). Další informace o registraci aplikace Azure AD najdete v tématu [integraci s Azure Active Directory](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-toosupport-certificate-based-authentication"></a>Krok 3a: přidejte identifikátor URI pro přesměrování v aplikaci a vývojářů ověřování na základě certifikátů portálu toosupport
ověřování na základě certifikátu toosupport druhý "msauth" musí toobe zaregistrovaný v aplikaci a hello [portál Azure](https://portal.azure.com/) toohandle ověření certifikátu, pokud chcete tooadd, které podporují ve vaší aplikaci.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

například: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-tooyour-app"></a>Krok 4: iOS9: Přidat aplikaci tooyour parametr konfigurace
ADAL používá – canOpenURL: toocheck, pokud je v zařízení hello nainstalován hello zprostředkovatele. V systému iOS 9 Apple uzamčené co schémata aplikaci dotázat. Budete potřebovat tooadd "msauth" toohello LSApplicationQueriesSchemes část vaší `info.plist file`.

<key>LSApplicationQueriesSchemes</key>

<array><string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Jednotné přihlašování jste nakonfigurovali!
Nyní hello Microsoft Identity SDK budou automaticky sdílet přihlašovací údaje v rámci aplikace i vyvolání zprostředkovatele hello, pokud je k dispozici na svém zařízení.

