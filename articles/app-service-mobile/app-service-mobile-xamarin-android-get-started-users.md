---
title: "aaaGet Začínáme s ověřováním pro Mobile Apps v Xamarin Android"
description: "Zjistěte, jak toouse Mobile Apps tooauthenticate uživatele vaší aplikace Xamarin Android prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 500a4efa816e4f6d75d359e31d6357da56a72f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinandroid-app"></a>Přidat aplikaci Xamarin.Android tooyour ověřování
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Toto téma ukazuje, jak uživatelé tooauthenticate mobilní aplikace z klientské aplikace. V tomto kurzu přidáte projekt quickstart toohello ověřování pomocí zprostředkovatele identity, který podporuje Azure Mobile Apps. Po se úspěšně ověří a autorizuje v hello mobilní aplikace, zobrazí se hodnota ID uživatele hello.

V tomto kurzu vychází z mobilní aplikace hello rychlý start. Musíte také nejdřív dokončit kurz hello [vytvoření aplikace Xamarin.Android]. Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello ověřování rozšíření balíčku tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte aplikační služby
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování

Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci. To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello. V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu. Můžete však použít žádné schéma adresy URL, které zvolíte. Mělo by být jedinečný tooyour mobilních aplikací. tooenable hello přesměrování na straně serveru hello:

1. V [portál Azure] hello vyberte App Service.

2. Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.

3. V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.  Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).  Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.

4. Klikněte na **OK**.

5. Klikněte na **Uložit**.

## <a name="permissions"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

V sadě Visual Studio nebo Xamarin Studio spusťte hello klientského projektu na emulátoru nebo zařízení. Ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno). K tomu dochází, protože aplikace hello pokusí tooaccess váš back-end mobilní aplikace jako neověřený uživatel. Hello *TodoItem* tabulka nyní vyžaduje ověření.

Potom aktualizujte hello klienta aplikace toorequest prostředky z back-end mobilní aplikace hello s ověřeného uživatele.

## <a name="add-authentication"></a>Přidat aplikaci toohello ověřování
aplikace Hello je aktualizovaný toorequire uživatelé tootap hello **přihlášení** tlačítko a ověření, než se zobrazí data.

1. Přidejte následující kód toohello hello **TodoActivity** třídy:
   
        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide hello button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load hello data.
                OnRefreshItemsSelected();
            }
        }
   
    Tím se vytvoří nový tooauthenticate metoda uživatele a metoda obslužnou rutinu pro novou **přihlášení** tlačítko. uživatel Hello v hello výše uvedený příklad kód ověřen pomocí přihlášení Facebook. Zobrazí se dialogové okno je ID uživatele používané toodisplay hello jednou ověření.
   
   > [!NOTE]
   > Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello předán příliš**LoginAsync** výše tooone následující hello: *MicrosoftAccount*, *Twitter*, *Google*, nebo *WindowsAzureActiveDirectory*.
   > 
   > 
2. V hello **OnCreate** metoda, odstranění nebo okomentujte hello následující řádek kódu:
   
        OnRefreshItemsSelected ();
3. Hello Activity_To_Do.axml souboru přidejte následující hello *LoginUser* tlačítko definice před hello existující *AddItem* tlačítko:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Přidejte následující element toohello Strings.xml prostředky soubor hello:
   
        <string name="login_button_text">Sign in</string>
5. Otevření souboru AndroidManifest.xml hello, přidejte následující kód do hello `<application>` – element XML:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. V sadě Visual Studio nebo Xamarin Studio spusťte hello klientského projektu na emulátoru nebo zařízení a přihlaste se pomocí zprostředkovatele identity vybrané. Pokud jste úspěšně přihlášeni, hello aplikace se zobrazí přihlašovací ID a hello seznam položek todo a provádět aktualizace toohello data.

<!-- URLs. -->
[vytvoření aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md