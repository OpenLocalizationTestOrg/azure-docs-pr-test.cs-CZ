---
title: "aaaHow tooconfigure ověřování Facebook pro aplikaci aplikační služby"
description: "Zjistěte, jak tooconfigure ověřování Facebook pro aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 53d03445a2ad17de1d2f69f5e770d14385b48ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-facebook-login"></a>Jak tooconfigure přihlašování Facebook toouse aplikace služby App Service
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak Azure App Service toouse tooconfigure Facebook jako zprostředkovatel ověřování.

toocomplete hello postup v tomto tématu, musíte mít účet Facebook, který má ověřenou e-mailovou adresu a číslo mobilního telefonu. toocreate nový účet Facebook, přejděte příliš[facebook.com].

## <a name="register"></a>Registrace vaší aplikace se službou Facebook.
1. Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace. Kopie vašeho **URL**. Použijete tento tooconfigure aplikace Facebook.
2. V jiném okně prohlížeče přejděte toohello [Facebook vývojáři] webu a přihlaste se pomocí vaší sítě Facebook přihlašovací údaje účtu.
3. (Volitelné) Pokud jste ještě nezaregistrovali, klikněte na tlačítko **aplikace** > **zaregistrujte se jako vývojář**, přijměte zásady hello a postupujte podle kroků registrace hello.
4. Klikněte na tlačítko **Moje aplikace** > **přidejte novou aplikaci** > **webu** > **přeskočit a vytvořit ID aplikace**. 
5. V **zobrazovaný název**, zadejte jedinečný název pro vaši aplikaci, typ vaše **e-mailu kontaktujte**, vyberte **kategorie** pro vaši aplikaci, pak klikněte na tlačítko **vytvoření ID aplikace**a dokončit kontrolu zabezpečení hello. Tím přejdete toohello vývojáře řídicí panel pro novou aplikaci Facebook.
6. V části "Facebook Login", klikněte na **Začínáme**. Přidejte svoji aplikaci **identifikátor URI pro přesměrování** příliš**identifikátory URI přesměrování platný OAuth**, pak klikněte na tlačítko **uložit změny**. 
   
   > [!NOTE]
   > Vaše přesměrování identifikátor URI je adresa URL hello aplikace připojí s cestou hello */.auth/login/facebook/callback*. Například, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Ujistěte se, že používáte hello schéma HTTPS.
   > 
   > 
7. V levém navigačním hello, klikněte na **nastavení**. Na hello **tajný klíč aplikace** pole, klikněte na tlačítko **zobrazit**, zadejte heslo, pokud požadovaný, poznamenejte si hodnoty hello **ID aplikace** a **tajný klíč aplikace**. Použijte tyto novější tooconfigure vaší aplikace v Azure.
   
   > [!IMPORTANT]
   > tajný klíč aplikace Hello je důležitým bezpečnostním pověřením. S kýmkoli sdílet tento tajný klíč nebo distribuovat v rámci klientské aplikace.
   > 
   > 
8. Hello účtu sítě Facebook, která byla aplikace hello použité tooregister je správcem aplikace hello. V tomto okamžiku se jenom správci moct přihlásit do této aplikace. tooauthenticate klikněte na tlačítko Další účty Facebook **revize aplikace** a povolte **veřejné zkontrolujte <-název_aplikace >** tooenable obecné veřejný přístup pomocí sítě Facebook ověřování.

## <a name="secrets"></a>Aplikace tooyour informace o přidání Facebook
1. Zpět v hello [portál Azure], přejděte tooyour aplikace. Klikněte na tlačítko **nastavení** > **ověřování / autorizace**a ujistěte se, že **ověřování služby aplikace** je **na**.
2. Klikněte na tlačítko **Facebook**, vložte hello ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve, můžete také povolit všechny obory, které vyžaduje vaše aplikace a pak klikněte na **OK**.
   
    ![][0]
   
    Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
3. (Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Facebook, nastavte **tootake akce, když požadavek nebude ověřený** příliš**Facebook**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooFacebook pro ověřování.
4. Po dokončení konfigurace ověřování, klikněte na tlačítko **Uložit**.

Jste nyní připraven toouse Facebook pro ověření ve vaší aplikaci.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[portál Azure]: https://portal.azure.com/
