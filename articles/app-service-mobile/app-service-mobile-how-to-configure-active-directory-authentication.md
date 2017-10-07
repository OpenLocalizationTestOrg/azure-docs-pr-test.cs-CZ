---
title: "aaaHow tooconfigure ověřování Azure Active Directory pro vaši aplikaci aplikační služby"
description: "Zjistěte, jak tooconfigure ověřování Azure Active Directory pro vaši aplikaci aplikační služby."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 5b1d73f8fdf5831a266d900cd07f4e3b0917a7f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-azure-active-directory-login"></a>Jak tooconfigure přihlášení vaší služby App Service toouse aplikace Azure Active Directory
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak Azure App Services toouse tooconfigure Azure Active Directory k ověřování.

## <a name="express"></a>Konfigurace Azure Active Directory s použitím expresního nastavení
1. V hello [portál Azure], přejděte tooyour aplikace. Klikněte na tlačítko **nastavení**a potom **ověřování/autorizace**.
2. Pokud hello ověřování / autorizace funkce není povolena, zapněte přepínač hello příliš**na**.
3. Klikněte na tlačítko **Azure Active Directory**a potom klikněte na **Express** pod **režim správy**.
4. Klikněte na tlačítko **OK** tooregister hello aplikace v Azure Active Directory. Tím se vytvoří nové registrace. Pokud chcete místo toho toochoose existující registraci, klikněte na tlačítko **vybrat existující aplikaci** a poté vyhledejte název hello dříve vytvořenou registrace v rámci vašeho klienta.
   Klikněte na možnost registrace tooselect hello ho a klikněte na tlačítko **OK**. Pak klikněte na tlačítko **OK** v okně Nastavení hello Azure Active Directory.
   
   ![][0]
   
   Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
5. (Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Azure Active Directory, nastavte **tootake akce, když požadavek nebude ověřený** příliš**přihlásit se přes Azure Active Directory**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooAzure služby Active Directory pro ověřování.
6. Klikněte na **Uložit**.

Jste nyní připraven toouse Azure Active Directory pro ověřování v aplikaci.

## <a name="advanced"></a>(Alternativní metoda) ručně nakonfigurovat upřesňující nastavení Azure Active Directory
Můžete také zvolit tooprovide nastavení konfigurace ručně. Toto je hello preferované řešení, pokud chcete toouse klienta AAD hello se liší od hello klienta, se kterým jste přihlášení do Azure. Konfigurace hello toocomplete, musíte nejdřív vytvořit registrace v Azure Active Directory a potom musíte poskytnout některé hello registrace podrobnosti tooApp služby.

### <a name="register"></a>Registrace vaší aplikace v Azure Active Directory
1. Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace. Kopie vašeho **URL**. Tato tooconfigure použijete aplikaci Azure Active Directory.
2. Přihlaste se toohello [portál Azure classic] a přejděte příliš**služby Active Directory**.
   
    ![][2]
3. Vyberte adresář a potom vyberte hello **aplikace** karty v horní části hello. Klikněte na tlačítko **přidat** na hello dolní toocreate nové registrace aplikace.
4. Klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
5. V hello Průvodce přidáním aplikace, zadejte **název** pro vaše aplikace a klikněte na tlačítko hello **webové aplikace nebo webové rozhraní API** typu. Klikněte na tlačítko toocontinue.
6. V hello **adresa URL přihlašování** pole, vložte adresu URL aplikace hello, které jste zkopírovali dříve. Zadejte stejné adresy URL v hello **identifikátor ID URI aplikace** pole. Klikněte na tlačítko toocontinue.
7. Po přidání aplikace hello, klikněte na tlačítko hello **konfigurace** kartě. Upravit hello **adresa URL odpovědi** pod **jednotné přihlašování** toobe hello adresu URL aplikace připojí s cestou hello */.auth/login/aad/callback*. Například, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Ujistěte se, že používáte hello schéma HTTPS.
   
    ![][3]
8. Klikněte na **Uložit**. Potom kopie hello **ID klienta** aplikace hello. Nakonfigurujete toouse vaší aplikace to později.
9. V hello dolní řádku nabídek klikněte na **zobrazit koncové body**a potom kopie hello **dokument federačních metadat** adresu URL a stažení dokumentů nebo přejděte tooit v prohlížeči.
10. V rámci kořenového hello **EntityDescriptor** elementu, by měl být **entityID** atribut hello formuláře `https://sts.windows.net/` následuje identifikátor GUID klienta konkrétní tooyour (nazývané "ID klienta"). Tato hodnota pro kopírování – bude sloužit jako vaše **URL vystavitele**. Nakonfigurujete toouse vaší aplikace to později.

### <a name="secrets"></a>Aplikaci tooyour informace o přidání Azure Active Directory
1. Zpět v hello [portál Azure], přejděte tooyour aplikace. Klikněte na tlačítko **nastavení**a potom **ověřování/autorizace**.
2. Pokud není povolená funkce ověřování/autorizace hello, zapněte přepínač hello příliš**na**.
3. Klikněte na tlačítko **Azure Active Directory**a potom klikněte na **Upřesnit** pod **režim správy**. Vložením hello ID klienta a adresy URL vystavitele hodnoty, které jste získali dříve. Pak klikněte na **OK**.
   
   ![][1]
   
   Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
4. (Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Azure Active Directory, nastavte **tootake akce, když požadavek nebude ověřený** příliš**přihlásit se přes Azure Active Directory**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooAzure služby Active Directory pro ověřování.
5. Klikněte na **Uložit**.

Jste nyní připraven toouse Azure Active Directory pro ověřování v aplikaci.

## <a name="optional-configure-a-native-client-application"></a>(Volitelné) Konfigurovat nativní klientskou aplikaci
Azure Active Directory můžete taky tooregister nativních klientů, které poskytuje větší kontrolu nad oprávněními mapování. Tuto funkci potřebujete Pokud chcete tooperform přihlášení pomocí knihovny například hello **Active Directory Authentication Library**.

1. Přejděte příliš**služby Active Directory** v hello [portál Azure classic].
2. Vyberte adresář a potom vyberte hello **aplikace** karty v horní části hello. Klikněte na tlačítko **přidat** na hello dolní toocreate nové registrace aplikace.
3. Klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
4. V hello Průvodce přidáním aplikace, zadejte **název** pro vaše aplikace a klikněte na tlačítko hello **nativní klientská aplikace** typu. Klikněte na tlačítko toocontinue.
5. V hello **identifikátor URI pro přesměrování** zadejte váš web */.auth/login/done* koncový bod, pomocí hello schéma HTTPS. Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*. Pokud vytváříte aplikace pro systém Windows, použijte místo toho hello [balíček SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) jako hello identifikátor URI.
6. Po přidání hello nativní aplikace, klikněte na tlačítko hello **konfigurace** kartě. Najde hello **ID klienta** a poznamenejte si tuto hodnotu.
7. Stránku hello přejděte dolů toohello **oprávnění tooother aplikace** části a klikněte na tlačítko **přidat aplikaci**.
8. Vyhledejte hello webové aplikace, které jste dříve zaregistrovali a klikněte na hello plus ikonu. Klikněte na tlačítko hello kontrola tooclose hello dialogu. Pokud webová aplikace hello nelze najít, přejděte tooits registrace a přidat nové adresa URL odpovědi (například hello HTTP verze vaše aktuální adresa URL), klikněte na Uložit a potom opakujte tyto kroky – hello aplikace by měla zobrazí v seznamu hello.
9. Na nový záznam hello jste právě přidali, otevřete hello **delegovaná oprávnění** rozevírací seznam a vyberte **přístup (appName)**. Potom klikněte na **Uložit**.

Nyní máte nakonfigurovaný nativní klientskou aplikaci, který má přístup k aplikaci služby App Service.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[portál Azure]: https://portal.azure.com/
[portál Azure classic]: https://manage.windowsazure.com/
[alternative method]:#advanced
