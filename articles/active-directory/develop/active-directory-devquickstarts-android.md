---
title: "aaaAzure AD Android Začínáme | Microsoft Docs"
description: "Tom, jak toobuild aplikace platformy Android, který se integruje s Azure AD pro přihlášení a volání služby Azure AD chráněný rozhraní API pomocí OAuth."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1aedc8ff60874b405a182a4ccbfb2c8b4d9d3704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a>Integrace Azure AD do aplikace pro Android
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Vyzkoušení verze preview hello naší nové [portál pro vývojáře](https://identity.microsoft.com/Docs/Android), který vám pomůže zprovoznění s Azure AD za několik minut. portál pro vývojáře Hello vás provede procesem hello registrace aplikace a integraci služby Azure AD do vašeho kódu. Jakmile budete hotovi, budete mít jednoduchou aplikaci, která může ověřit uživatele v klientovi a back-end, které mohou přijímat tokeny a provést ověření.
>
>

Pokud vyvíjíte aplikace pracovní plochy, Azure Active Directory (Azure AD) je jednoduchá a přímočará pro tooauthenticate můžete uživatelům pomocí jejich místních účtů služby Active Directory. Umožňuje také toosecurely vaše aplikace využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.

Pro Android klienti, kteří potřebují tooaccess chráněné prostředky Azure AD poskytuje hello Active Directory Authentication Library (ADAL). jediným účelem Hello adal je toomake ho snadno pro vaše aplikace tooget přístupových tokenů. jak snadné je, nemůžeme budete sestavit aplikaci Android seznam úkolů, která toodemonstrate:

* Získá přístup k tokeny pro volání rozhraní API seznamu úkolů pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Získá seznam úkolů uživatele.
* Provede odhlášení uživatele.

tooget spustit, musíte klienta služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci. Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-hello-nodejs-rest-api-todo-sample-server"></a>Krok 1: Stáhněte a spusťte server ukázka TODO rozhraní API REST Node.js hello
Ukázka TODO rozhraní API REST Node.js Hello je zapsán konkrétně toowork proti naše existující vzorek pro vytváření jednoho klienta úkolů REST API pro Azure AD. Toto je předpokladem pro hello rychlý Start.

Informace o tom, jak tooset to, najdete v části ukázek existující v [Microsoft Azure Active Directory ukázka REST API Service pro Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Krok 2: Registrace webového rozhraní API s klientovi Azure AD
Služba Active Directory podporuje přidání dva typy aplikací:

- Webové rozhraní API, které nabízejí služby toousers
- Aplikace (běžící na webu hello nebo na zařízení), které ty přístup k webovým rozhraním API

V tomto kroku se registrace webového rozhraní API hello, kterou používáte místně pro testování této ukázce. Za normálních okolností je tomuto webovému rozhraní API REST službu, která je nabídka funkce, které chcete aplikaci tooaccess. Azure AD můžete chránit libovolný koncový bod.

Jsme se za předpokladu, že jste registrace hello TODO REST API odkazuje dříve. Ale tento postup funguje pro všechny webové rozhraní API, který chcete Azure Active Directory toohelp chránit.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na váš účet. V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Zadejte popisný název aplikace hello (například **TodoListService**), vyberte **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko **Další**.
6. Hello přihlašování adresy URL zadejte hello základní adresu URL pro ukázku hello. Ve výchozím nastavení je to `https://localhost:8080`.
7. Klikněte na tlačítko **OK** toocomplete hello registrace.
8. Během hello portálu Azure, přejděte na stránku aplikace tooyour, najít hodnotu ID aplikace hello a zkopírujte jej. Budete potřebovat to později při konfiguraci vaší aplikace.
9. Z hello **nastavení** -> **vlastnosti** stránky, aktualizovat identifikátor ID URI aplikace hello – zadejte `https://<your_tenant_name>/TodoListService`. Nahraďte `<your_tenant_name>` s názvem hello klienta služby Azure AD.

## <a name="step-3-register-hello-sample-android-native-client-application"></a>Krok 3: Registrace aplikace Android Native Client hello ukázka
V této ukázce je nutné zaregistrovat webové aplikace. To umožňuje toocommunicate vaší aplikace s hello právě zaregistrován webového rozhraní API. Azure AD odmítne tooeven povolit tooask vaší aplikace pro přihlášení, pokud je zaregistrována. Který je součástí hello zabezpečení hello modelu.

Jsme jste za předpokladu, že jste registrace hello ukázkovou aplikaci odkazuje dříve. Ale tento postup funguje pro každou aplikaci, která jste vývoj.

> [!NOTE]
> Může vás zajímat, proč jste uvedení aplikace i webové rozhraní API v jednoho klienta. Jak vám může mít uhádnout, můžete vytvořit aplikaci, která přistupuje k externí rozhraní API, která je zaregistrovaná ve službě Azure AD z jiného klienta. Pokud tak učiníte, budou vaši zákazníci výzva tooconsent toohello použití hello rozhraní API v aplikaci hello. Knihovna ověřování Active Directory pro iOS postará svůj souhlas pro vás. Jak jsme prozkoumat nabízí vyspělejší funkce, uvidíte, že se jedná o důležitou součástí hello práce potřebné tooaccess hello sada Microsoft APIs z Azure a Office, stejně jako ostatní poskytovatele služeb. Teď, protože jste zaregistrovali webového rozhraní API a aplikace v rámci hello stejné klienta, neuvidíte zobrazování výzev k vyjádření souhlasu. Toto je obvykle případ hello, pokud vyvíjíte aplikaci jen pro vlastní toouse společnosti.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na váš účet. V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Zadejte popisný název aplikace hello (například **TodoListClient Android**), vyberte **nativní klientská aplikace**a klikněte na tlačítko **Další**.
6. Identifikátor URI přesměrování pro hello, zadejte `http://TodoListClient`. Klikněte na **Dokončit**.
7. Na stránce aplikace hello najít hello hodnota ID aplikace a zkopírujte ho. Budete potřebovat to později při konfiguraci vaší aplikace.
8. Z hello **nastavení** vyberte **požadovaných oprávnění** a vyberte **přidat**.  Vyhledejte a vyberte TodoListService, přidejte hello **přístup TodoListService** oprávnění v rámci **delegovaná oprávnění**a klikněte na tlačítko **provádí**.

toobuild s Maven, můžete použít pom.xml na nejvyšší úrovni hello:

1. Klonování tohoto úložiště do adresáře podle vašeho výběru:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Postupujte podle kroků hello v hello [tooset požadavky prostředí Maven pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
3. Nastavte hello emulátoru s SDK 19.
4. Přejděte toohello kořenové složky, které jste naklonovali úložiště hello.
5. Spusťte tento příkaz:`mvn clean install`
6. Změna hello directory toohello rychlý Start ukázka:`cd samples\hello`
7. Spusťte tento příkaz:`mvn android:deploy android:run`

   Měli byste vidět, spouštění aplikace hello.
8. Zadejte tootry přihlašovací údaje testovacího uživatele.

Balíčky JAR bude odeslána vedle hello AAR balíčku.

## <a name="step-4-download-hello-android-adal-and-add-it-tooyour-eclipse-workspace"></a>Krok 4: Stažení hello knihovny ADAL pro Android a přidejte ji tooyour Eclipse prostoru
Provedli jsme ho snadno můžete toohave více možností toouse ADAL v projektu Android:

* Hello zdrojového kódu tooimport můžete použít tuto knihovnu do aplikace tooyour Eclipse a odkaz.
* Pokud používáte Android Studio, můžete použít hello AAR balíček formátu a referenční dokumentace hello binární soubory.

### <a name="option-1-source-zip"></a>Možnost 1: Zdroj Zip
Klikněte na tlačítko toodownload kopii hello zdrojový kód, **stáhnout ZIP** na pravé straně hello hello stránky. Nebo můžete [stáhnout z webu GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>Možnost 2: Zdroj pomocí Gitu
zdrojový kód tooget hello hello SDK prostřednictvím Git, zadejte:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Možnost 3: Binární soubory přes Gradle
Binární soubory hello můžete získat z hello Maven centrální úložiště. balíček AAR Hello můžou být takto součástí projekt v Android Studio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Možnost 4: AAR prostřednictvím Maven
Pokud používáte hello M2Eclipse modul plug-in, můžete určit hello závislostí v souboru pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-hello-libs-folder"></a>Možnost 5: JAR balíčku uvnitř složky libs hello
Můžete získat soubor JAR hello z hello Maven úložišti a umístěte jej do hello **knihovny** složku ve vašem projektu. Je nutné projektu tooyour požadované prostředky toocopy hello je také možné, protože balíčky JAR hello neobsahují.

## <a name="step-5-add-references-tooandroid-adal-tooyour-project"></a>Krok 5: Přidejte odkazy na tooAndroid ADAL tooyour projektu
1. Přidat odkaz na projekt tooyour a zadejte jej jako knihovna pro Android. Pokud si nejste jisti, jak toodo, získáte další informace o hello [lokality Android Studio](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Přidáte závislost projektu hello ladění do nastavení projektu.
3. Aktualizace tooinclude souboru AndroidManifest.xml vašeho projektu:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Vytvoření instance kontextu AuthenticationContext v hlavní aktivitě. Hello podrobnosti tohoto volání je nad rámec hello rozsahu tohoto tématu, ale můžete začít funkčním prohlížením hello [ukázka Android Native Client](https://github.com/AzureADSamples/NativeClient-Android). V následujícím příkladu hello, SharedPreferences je hello výchozí mezipaměti a autority hello tvar `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Zkopírujte tento kód bloku toohandle hello konec AuthenticationActivity po hello uživatel zadá přihlašovací údaje a přijímá autorizační kód:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. tooask pro token, definovat zpětné volání:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Nakonec požádejte o token pomocí že zpětné volání:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Následuje vysvětlení parametrů hello:

* *prostředek* je povinná a je hello prostředků, které se snažíte tooaccess.
* *ClientID* je povinná a pochází z Azure AD.
* *RedirectUri* není požadovaná toobe zadaná pro volání acquireToken hello. Můžete ho nastavit tak jako název balíčku.
* *PromptBehavior* pomáhá tooask pro přihlašovací údaje tooskip hello mezipaměti a souboru cookie.
* *zpětné volání* je volána po hello autorizační kód se vyměňují pro token. Obsahuje objekt AuthenticationResult, který obsahuje přístupový token, datum vypršela platnost a ID informace o tokenu.
* *acquireTokenSilent* je volitelný. Můžete ji volat toohandle ukládání do mezipaměti a token obnovení. Poskytuje také verzi služby sync hello. Přijímá *userId* jako parametr.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Pomocí tohoto návodu, byste měli mít, co budete potřebovat toosuccessfully integraci s Azure Active Directory. Další příklady tato práce, najdete v článku hello AzureADSamples nebo úložišti na Githubu.

## <a name="important-information"></a>Důležité informace
### <a name="customization"></a>Přizpůsobení
Vaše prostředky aplikace můžete přepsat projektu prostředky knihovny. To se stane, když sestavuje vaší aplikace. Z tohoto důvodu můžete přizpůsobit ověřování aktivity rozložení hello požadovaným způsobem. Být jisti ID hello tookeep prvků hello používá této ADAL (webového zobrazení).

### <a name="broker"></a>Zprostředkovatel
aplikace portál společnosti Intune Microsoft Hello poskytuje součást zprostředkovatele hello. Hello účet je vytvořen v AccountManager. Typ účtu Hello je "com.microsoft.workaccount." AccountManager umožňuje jenom jeden účet jednotné přihlašování. Po dokončení hello zařízení výzvu pro některé z aplikací hello vytvoří soubor cookie jednotného přihlašování pro uživatele hello.

ADAL používá účet broker hello, pokud jeden uživatelský účet je vytvořena při tato ověřovací data a vy zvolíte není tooskip ho. Můžete přeskočit hello zprostředkovatele uživatele pomocí:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

Potřebujete tooregister speciální RedirectUri pro použití zprostředkovatele. RedirectUri je ve formátu hello `msauth://packagename/Base64UrlencodedSignature`. Vaše RedirectUri pro vaši aplikaci můžete získat pomocí skriptu brokerRedirectPrint.ps1 hello nebo mContext.getBrokerRedirectUri volání hello rozhraní API. podpis Hello je související tooyour podpisové certifikáty.

aktuální model zprostředkovatele Hello je pro jednoho uživatele. Kontextu AuthenticationContext poskytuje hello API metoda tooget hello zprostředkovatele uživatele.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

Manifest aplikace by měl mít následující oprávnění toouse AccountManager účty hello. Podrobnosti najdete v tématu hello [AccountManager informace na webu Android hello](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>Adresa URL autority a AD FS
Active Directory Federation Services (AD FS) není rozpoznán jako produkční služby tokenů zabezpečení, takže potřebujete tooturn instance zjišťování a předá hodnotu false v konstruktoru kontextu AuthenticationContext hello.

Adresa URL autority Hello musí instance služby tokenů zabezpečení a [název klienta](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).

### <a name="querying-cache-items"></a>Dotazování na položky mezipaměti
ADAL poskytuje výchozí mezipaměti SharedPreferences s některé jednoduché mezipaměti funkce dotazování. Aktuální mezipaměť hello můžete získat z kontextu AuthenticationContext s použitím:

    ITokenCacheStore cache = mContext.getCache();

Můžete zadat také implementace mezipaměti, pokud chcete, aby toocustomize ho.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Chování výzvy
ADAL poskytuje chování výzvy možnost toospecify. Pokud token obnovení hello je neplatný a jsou vyžadována pověření uživatele, se zobrazí PromptBehavior.Auto hello uživatelského rozhraní. PromptBehavior.Always bude přeskočit využití mezipaměti hello a vždy zobrazovat hello uživatelského rozhraní.

### <a name="silent-token-request-from-cache-and-refresh"></a>Tichou žádosti o token z mezipaměti a aktualizace
Tichou žádosti o token nepoužívá hello automaticky otevírané okno uživatelského rozhraní a nevyžaduje aktivitu. Vrátí token z mezipaměti hello, pokud je k dispozici. Pokud hello tokenu vypršela platnost, tato metoda se pokusí toorefresh ho. Pokud token obnovení hello vyprší nebo se nezdařilo, vrátí authenticationexception –.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Můžete provést také synchronizace volat pomocí této metody. Můžete nastavit hodnotu null toocallback nebo použít acquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnostika
Toto jsou primární zdroje informací pro diagnostiku problémů hello:

* Výjimky
* Logs
* Trasování sítě

Všimněte si, že ID korelace jsou centrální toohello diagnostiky v hello knihovně. Vaše ID korelace na základě požadavků můžete nastavit, pokud chcete, aby toocorrelate ADAL žádosti s dalšími operacemi v kódu. Pokud není nastavený ID korelace, ADAL vygeneruje náhodné. Všechny zprávy protokolu a volání síť pak bude být označený hello ID korelace. Hello generovaný sám sebou ID změny na každý požadavek.

#### <a name="exceptions"></a>Výjimky
Výjimky jsou hello nejprve diagnostiky. Pokusíme tooprovide užitečné chybové zprávy. Pokud zjistíte, ten, který není užitečné, oznamte problém a dejte nám vědět. Zahrnují informace o zařízení, jako je například modelu a číslo SDK.

#### <a name="logs"></a>Logs
Můžete nakonfigurovat hello knihovně toogenerate zprávy protokolu, které můžete použít toohelp diagnostikovat problémy. Konfigurace protokolování tak, že hello následující volání tooconfigure zpětné volání, jako je generován budou používat ADAL toohand vypnout každé zprávě protokolu.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this toolog file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Může být zprávy zapisovány tooa vlastního souboru protokolu, jak je znázorněno v následujícím kódu hello. Bohužel neexistuje žádný standardní způsob získání protokolů ze zařízení. Existují některé služby, které vám mohou pomoci s to. Můžete také vytvořte vlastní, jako třeba odesílání hello souboru tooa server.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Toto jsou hello úrovní protokolování:
* Chyba (výjimek)
* Warn (upozornění)
* Informace o (informační účely)
* Verbose (podrobnosti)

Můžete nastavit úroveň protokolu hello takto:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Všechny zprávy protokolu jsou zasílány toologcat, v přidání tooany vlastní protokol zpětných volání.
Soubor protokolu tooa z logcat můžete získat následujícím způsobem:

    adb logcat > "C:\logmsg\logfile.txt"

 Podrobnosti o adb příkazů najdete v tématu hello [logcat informace na webu Android hello](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Trasování sítě
Můžete použít různé nástroje toocapture hello HTTP provoz, který generuje ADAL.  To je velmi užitečné, pokud jste obeznámeni s hello protokolu OAuth, nebo pokud potřebujete tooprovide diagnostické informace tooMicrosoft nebo jiné kanály podpory.

Fiddler je hello nejjednodušší nástroj, trasování protokolu HTTP. Použití hello následující odkazy tooset ho nahoru toocorrectly záznam ADAL síťový provoz. Pro trasování nástroje, jako je Fiddler nebo Charlese toobe užitečné musíte ho nakonfigurovat provoz toorecord bez šifrování SSL.  

> [!NOTE]
> Trasování generované tímto způsobem může obsahovat vysoce důvěrné informace, například přístupové tokeny, uživatelská jména a hesla. Pokud používáte produkční účty, nesdílí toto trasování s třetími stranami. Pokud potřebujete toosupply toosomeone trasování v pořadí tooget podpory, reprodukujte problém hello pomocí dočasného účtu s uživatelská jména a hesla, aby vás sdílení.

* Z webu webu Telerik hello: [nastavení si aplikaci Fiddler pro Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* Z Githubu: [nakonfigurovat aplikaci Fiddler pravidla pro ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Dialogové okno režimu
Metoda acquireToken Hello bez aktivity podporuje řádku dialogové okno.

### <a name="encryption"></a>Šifrování
ADAL šifruje hello tokeny a úložiště v SharedPreferences ve výchozím nastavení. Můžete si prohlédnout podrobnosti hello hello StorageHelper třídy toosee. Android zavedl Android úložiště klíčů pro 4.3 (rozhraní API 18) bezpečné uložení privátního klíče. ADAL použije tento 18 rozhraní API a vyšší. Pokud chcete toouse ADAL pro nižší verze sady SDK, je nutné tooprovide tajný klíč v AuthenticationSettings.INSTANCE.setSecretKey.

### <a name="oauth2-bearer-challenge"></a>Výzvy nosiče OAuth2
Hello AuthenticationParameters třída poskytuje funkce tooget authorization_uri z hello výzvy nosiče OAuth2.

### <a name="session-cookies-in-webview"></a>Soubory cookie relace ve webovém zobrazení
Android webové zobrazení nevymaže soubory cookie relace po zavření aplikace hello. Která dokáže zpracovat pomocí ukázkový kód:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Podrobnosti o souborech cookie najdete v tématu hello [CookieSyncManager informace na webu Android hello](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Přepsání prostředků
Knihovna ADAL Hello zahrnuje anglické řetězce pro zprávy ProgressDialog. Aplikace je měli přepsat, pokud chcete, aby lokalizovaných řetězců.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>Dialogové okno protokolu NTLM
ADAL verze 1.1.0 podporuje dialogu protokolu NTLM, který zpracovává se pomocí hello onReceivedHttpAuthRequest událost z WebViewClient. Můžete přizpůsobit hello rozložení a řetězce pro dialogové okno hello.

### <a name="cross-app-sso"></a>Jednotné přihlašování napříč aplikacemi
Další informace [jak tooenable jednotného přihlašování napříč aplikacemi v systému Android pomocí ADAL](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
