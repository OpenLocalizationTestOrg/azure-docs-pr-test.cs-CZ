---
title: "aaaAzure AD Cordova Začínáme | Microsoft Docs"
description: "Jak toobuild aplikace Cordova, se integruje se službou Azure AD pro přihlášení a zavolá rozhraní API Azure AD chráněné pomocí OAuth."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: 573ed638c2180c5231648bcb8c49ceb6f53296f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integrace Azure AD s platformě Apache Cordova app
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Můžete použít Apache Cordova toodevelop HTML5/JavaScript aplikace, které můžou běžet na mobilních zařízeních jako plně kvalifikované nativních aplikací. S Azure Active Directory (Azure AD) můžete přidat podnikové úrovni ověřování možnosti tooyour Cordova aplikace.

Modulu plug-in Cordova zabalí Azure AD nativních sad SDK pro iOS, Android, Windows Store a Windows Phone. Pomocí, že modul plug-in, můžete vylepšit aplikace toosupport přihlašování pomocí účtů služby Windows Server Active Directory, získat přístup k tooOffice 365 a rozhraní API Správce Azure vašich uživatelů a to i v ochraně volání tooyour vlastní vlastní rozhraní web API.

V tomto kurzu použijeme hello Apache Cordova modulu plug-in pro Active Directory Authentication Library (ADAL) tooimprove jednoduchou aplikaci přidáním hello následující funkce:

* Pomocí několika řádků kódu ověření uživatele a získat token.
* Pomocí tohoto tokenu tooinvoke hello rozhraní Graph API tooquery adresáře a zobrazit výsledky hello.  
* Použít hello ADAL mezipamětí tokenů toominimize ověřování vyzve k zadání uživatele hello.

toomake těchto vylepšení, budete muset:

1. Zaregistrovat aplikaci s Azure AD.
2. Přidejte kód tooyour aplikace toorequest tokeny.
3. Přidání kódu toouse hello tokenu pro dotazování hello rozhraní Graph API a zobrazit výsledky.
4. Vytvoření projektu nasazení hello Cordova s všechny platformy hello má tootarget, přidejte hello Cordova ADAL modulu plug-in a testování hello řešení v emulátorů.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu potřebujete:

* Klient služby Azure AD, kde máte účet s právy pro vývoj aplikací.
* Vývojové prostředí, který byl nakonfigurován toouse Apache Cordova.  

Jak už máte-li nastavit, pokračovat přímo toostep 1.

Pokud nemáte klient služby Azure AD, použijte hello [pokyny, jak tooget jeden](active-directory-howto-tenant.md).

Pokud nemáte nastavení na počítači pro Apache Cordova, nainstalujte hello následující:

* [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Node.js](https://nodejs.org/download/)
* [Rozhraní příkazového řádku Cordova](https://cordova.apache.org/) (se dá snadno nainstalovat prostřednictvím Správce balíčku NPM: `npm install -g cordova`)

Před instalací Hello by měly fungovat na hello PC i na hello Mac.

Každý Cílová platforma má jiné předpoklady:

* toobuild a spusťte aplikaci pro Windows Tabletu nebo Windows Phone:
  * Nainstalujte [Visual Studio 2013 pro Windows s aktualizací 2 nebo novější](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express nebo jinou verzi) nebo [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).

* toobuild a spusťte aplikaci pro iOS:

  * Instalaci Xcode 6.x nebo novější. Stáhnout z hello [vývojáře Apple lokality](http://developer.apple.com/downloads) nebo hello [Mac App Storu](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  * Nainstalujte [ios-sim](https://www.npmjs.org/package/ios-sim). Můžete ho toostart aplikací pro iOS v simulátoru iOS z příkazového řádku hello. (Můžete ho snadno nainstalovat prostřednictvím hello terminálu: `npm install -g ios-sim`.)
* toobuild a spusťte aplikaci pro Android:

  * Nainstalujte [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo novější. Zajistěte, aby `JAVA_HOME` (proměnnou prostředí) je správně nastavena podle toohello JDK instalační cestu (například C:\Program Files\Java\jdk1.7.0_75).
  * Nainstalujte [sady SDK pro Android](http://developer.android.com/sdk/installing/index.html?pkg=tools) a přidejte hello `<android-sdk-location>\tools` umístění (například C:\tools\Android\android-sdk\tools) tooyour `PATH` proměnné prostředí.
  * Otevřete Android SDK Manageru (například prostřednictvím hello terminálu: `android`) a nainstalujte:
    * *Android – 5.0.1 (rozhraní API 21)* SDK platformy
    * *Nástroje sestavení Android SDK* verze 19.1.0 nebo novější
    * *Podpora pro Android úložiště* (funkce)

  Hello Android SDK neposkytuje žádné výchozí emulátor instance. Vytvořit spuštěním `android avd` z hello terminálu a potom vyberete **vytvořit**, pokud chcete, aby toorun hello aplikace pro Android na emulátor. Doporučujeme, abyste API úrovně 19 nebo vyšší. Další informace o možnostech Android emulátoru a vytvoření hello najdete v tématu [správce AVD](http://developer.android.com/tools/help/avd-manager.html) na lokality Android hello.

## <a name="step-1-register-an-application-with-azure-ad"></a>Krok 1: Zaregistrujte aplikaci s Azure AD
Tento krok je volitelný. Tento kurz obsahuje předem zřízená hodnoty, které můžete použít toosee hello ukázka v akci bez provádění zřizování v vlastního klienta. Ale doporučujeme provést tento krok a seznámit se s hello procesu, protože se bude vyžadovat, při vytváření vlastních aplikací.

Azure AD vydá tokeny tooonly známé aplikace. Než budete moct použít Azure AD z vaší aplikace, musíte toocreate položku pro něj ve vašem klientovi. tooregister novou aplikaci v klientovi služby:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na váš účet. V hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle pokynů hello a vytvořit **nativní klientská aplikace**. (I když jsou aplikace Cordova na základě HTML, vytváříme nativní klientskou aplikaci. Hello **nativní klientská aplikace** musí být vybraná volba nebo hello aplikace nebude fungovat.)
  * **Název** popisuje toousers vaší aplikace.
  * **Identifikátor URI pro přesměrování** je hello identifikátor URI, který byl použit tooreturn tokeny tooyour aplikace. Zadejte **http://MyDirectorySearcherApp**.

Po dokončení registrace přiřadí Azure AD aplikace jedinečné ID tooyour aplikace. Budete potřebovat tuto hodnotu v dalších částech hello. Najdete ho na kartě aplikace hello hello nově vytvořené aplikace.

toorun `DirSearchClient Sample`, udělte hello nově vytvořený aplikaci oprávnění tooquery hello Azure AD Graph API:

1. Z hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.  
2. Hello aplikaci Azure Active Directory, vyberte **Microsoft Graph** jako hello rozhraní API a přidejte hello **přístup k adresáři hello jako hello přihlášeného uživatele** oprávnění v rámci **delegovaní Oprávnění**.  To umožňuje vaší aplikace tooquery hello rozhraní Graph API pro uživatele.

## <a name="step-2-clone-hello-sample-app-repository"></a>Krok 2: Klonování hello ukázkové aplikace úložiště
Vaše prostředí nebo na příkazovém řádku zadejte následující příkaz hello:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-hello-cordova-app"></a>Krok 3: Vytvoření aplikace Cordova hello
Existuje několik způsobů toocreate Cordova aplikací. V tomto kurzu použijeme hello Cordova rozhraní příkazového řádku (CLI).

1. Vaše prostředí nebo na příkazovém řádku zadejte následující příkaz hello:

        cordova create DirSearchClient

   Tento příkaz vytvoří strukturu složek hello a generování uživatelského rozhraní pro projekt Cordova hello.

2. Přesunutí nové složky DirSearchClient toohello:

        cd .\DirSearchClient

3. Kopírovat obsah hello hello starter projektu v podsložce www hello pomocí Správce souborů nebo hello následující příkaz do vašeho prostředí:

  * Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`
  * Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`

4. Přidejte hello povolených modulu plug-in. To je nezbytné pro vyvolání hello rozhraní Graph API.

        cordova plugin add cordova-plugin-whitelist

5. Přidejte všechny hello platformy, které chcete toosupport. toohave pracovní vzorek, musíte tooexecute alespoň jeden Dobrý den, následující příkazy. Poznámka: nebude se moct tooemulate iOS v systému Windows nebo emulovat Windows v počítačích Mac.

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. Přidejte hello ADAL pro modul plug-in tooyour projektu Cordova:

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-tooauthenticate-users-and-obtain-tokens-from-azure-ad"></a>Krok 4: Přidejte kód tooauthenticate uživatele a získat tokeny z Azure AD
Hello aplikace, které vyvíjíte v tomto kurzu vám poskytne jednoduchý directory funkce vyhledávání. Hello uživatele můžete pak zadejte alias hello všechny uživatele v adresáři hello a vizualizovat některé základní atributy. Hello starter projekt obsahuje definici hello hello základní uživatelské rozhraní aplikace hello (ve www/index.html) a hello generování uživatelského rozhraní, která sváže základní aplikaci událostí cyklů vazby uživatelské rozhraní a výsledky zobrazení logiku (v www/js/index.js). Hello pouze úloha ponecháno pro vás je tooadd hello logiky, která implementuje identity úlohy.

Hello všeho nejdřív musíte toodo ve vašem kódu je zavést hello protokol hodnoty, které používá Azure AD pro identifikaci vaší aplikace a prostředky text hello, že cíl je. Tyto hodnoty budou použité tooconstruct žádosti o tokeny hello později. Vložte následující fragment kódu hello horní části souboru index.js hello hello:

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Hello `redirectUri` a `clientId` hodnoty by měla odpovídat hello hodnoty, které popisují aplikace ve službě Azure AD. Můžete najít ty z hello **konfigurace** kartě v hello portál Azure, jak je popsáno v kroku 1 v tomto kurzu.

> [!NOTE]
> Pokud jste se rozhodli pro novou aplikaci není registraci v vlastního klienta, můžete jednoduše vložit hello předkonfigurované hodnoty, jako je. Můžete se podívat, hello ukázka spuštěn, když měli vždycky vytvořit vlastní položku pro aplikace, které jsou určené pro produkční prostředí.

Dál přidejte kód žádosti o token hello. Vložte následující fragment kódu mezi hello hello `search` a `renderData` definice:

```javascript
    // Shows hello user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
        });

    },
```
Podle jeho rozdělení na dvě hlavní části Podívejme se na této funkce.
Tato ukázka je navrženou toowork s žádným klientem jako názvem na rozdíl od toobeing svázané tooa některá. Používá hello "/ běžné" koncový bod, který umožňuje hello uživatele tooenter žádnému účtu v době ověřování a přesměruje klienta toohello hello požadavku, kam patří.

Tato první část hello metoda zkontroluje toosee ADAL mezipaměti hello, pokud token je již uložen. Pokud ano, metoda hello používá hello klientům a odkud hello tokenu pochází pro provést novou inicializaci ADAL. To je nezbytné tooavoid další výzvy, protože použití hello z "/ běžné" vždy výsledkem žádostí hello uživatele tooenter nový účet.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Druhá část Hello hello metody provede hello žádosti o token správné. Hello `acquireTokenSilentAsync` metoda ADAL tooreturn token vyzve k zadání hello zadaný prostředek bez zobrazení všech UX Může dojít, pokud mezipaměti hello již vhodný přístupový token, uložené, nebo pokud token obnovení lze použít tooget nový přístupový token bez zobrazuje všechny řádku. Pokud se tento pokus selže, jsme vrátit zpět `acquireTokenAsync`– viditelně, který vyzve uživatele tooauthenticate hello.

```javascript
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
```
Teď, když máme hello token, jsme nakonec vyvolání hello rozhraní Graph API a provádění hello vyhledávací dotaz, který má být. Vložte následující fragment kódu níže hello hello `authenticate` definice:

```javascript
// Makes an API call tooreceive hello user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
soubory od bodu Hello zadat jednoduchý UX pro zadání uživatele alias v textovém poli. Tato metoda používá tuto hodnotu tooconstruct dotazu, kombinovat s hello přístupový token, odešle tooMicrosoft graf a analyzovat hello výsledky. Hello `renderData` metoda, již existuje v souboru od bodu hello postará vizualizace výsledků hello.

## <a name="step-5-run-hello-app"></a>Krok 5: Spuštění aplikace hello
Nakonec připravené toorun je vaše aplikace. Jeho provoz je jednoduchý: při spuštění aplikace hello zadejte hello alias uživatele hello chcete toolook nahoru a pak klikněte na tlačítko hello. Se zobrazí výzva k ověření. Po úspěšném ověření a hledání úspěšné zobrazí se hello atributy hello hledat uživatele.

Při dalším spuštění provede vyhledávání hello bez zobrazení všech řádku, thanks toohello přítomnost hello dříve získat token v mezipaměti.

Hello konkrétní kroky pro spuštění aplikace hello se liší podle platformy.

### <a name="windows-10"></a>Windows 10
   Tabletu:`cordova run windows --archs=x64 -- --appx=uap`

   Mobilní zařízení (vyžaduje tooa zařízení připojené Windows 10 Mobile PC):`cordova run windows --archs=arm -- --appx=uap --phone`

   > [!NOTE]
   > Při prvním spuštění hello můžete být požádáni toosign v pro vývojáře licenci. Další informace najdete v tématu [vývojáře licence](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-81-tabletpc"></a>Windows 8.1 Tabletu
   `cordova run windows`

   > [!NOTE]
   > Při prvním spuštění hello můžete být požádáni toosign v pro vývojáře licenci. Další informace najdete v tématu [vývojáře licence](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-phone-81"></a>Windows Phone 8.1
   toorun na připojeném zařízení:`cordova run windows --device -- --phone`

   toorun na výchozí emulátor hello:`cordova emulate windows -- --phone`

   Použití `cordova run windows --list -- --phone` toosee všechny dostupné cíle a `cordova run windows --target=<target_name> -- --phone` aplikace hello toorun na určité zařízení nebo emulátoru (například `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

### <a name="android"></a>Android
   toorun na připojeném zařízení:`cordova run android --device`

   toorun na výchozí emulátor hello:`cordova emulate android`

   Ujistěte se, že jste vytvořili instanci emulátoru pomocí Správce AVD, jak je popsáno výše v části "Požadavky" hello.

   Použití `cordova run android --list` toosee všechny dostupné cíle a `cordova run android --target=<target_name>` aplikace hello toorun na určité zařízení nebo emulátoru (například `cordova run android --target="Nexus4_emulator"`).

### <a name="ios"></a>iOS
   toorun na připojeném zařízení:`cordova run ios --device`

   toorun na výchozí emulátor hello:`cordova emulate ios`

   > [!NOTE]
   > Zajistěte, aby byla hello `ios-sim` toorun balíček nainstalován v emulátoru hello. Další informace najdete v tématu požadavky"hello" oddílu.

    Use `cordova run ios --list` toosee all available targets and `cordova run ios --target=<target_name>` toorun hello application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` toosee additional build and run options.

## <a name="next-steps"></a>Další kroky
Pro srovnání je k dispozici v hello dokončit ukázka (bez vašich hodnot nastavení) [Githubu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).

Můžete teď scénáře přesunout na toomore rozšířené (a další zajímavé). Můžete chtít tootry: [zabezpečit webové rozhraní Node.js API s Azure AD](active-directory-devquickstarts-webapi-nodejs.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
