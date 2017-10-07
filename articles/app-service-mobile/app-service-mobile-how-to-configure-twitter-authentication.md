---
title: "ověřování služby Twitter tooconfigure aaaHow pro vaši aplikaci aplikační služby"
description: "Zjistěte, jak ověřováním služby Twitter tooconfigure pro vaši aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 0d16ac44d7b54e3567b793a904059d31ab1d8911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-twitter-login"></a>Jak tooconfigure přihlašování Twitter toouse aplikace služby App Service
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak Azure App Service toouse tooconfigure Twitter jako zprostředkovatel ověřování.

toocomplete hello postup v tomto tématu, musíte mít účet služby Twitter, který má ověřenou e-mailovou adresu a telefonní číslo. toocreate nový účet služby Twitter, přejděte příliš<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Aplikaci zaregistrovat u služby Twitter.
1. Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace. Kopie vašeho **URL**. Tato tooconfigure použijete aplikaci služby Twitter.
2. Přejděte toohello [Twitter vývojáři] web, přihlaste se pomocí přihlašovacích údajů účtu služby Twitter a klikněte na **vytvořit novou aplikaci**.
3. Typ v hello **název** a **popis** pro novou aplikaci. Vložení ve vaší aplikaci **URL** pro hello **webu** hodnotu. Potom hello **adresu URL zpětné volání**, vložte hello **adresu URL zpětné volání** jste zkopírovali dříve. Toto je vaše mobilní aplikace brána připojena s cestou hello, */.auth/login/twitter/callback*. Například, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Ujistěte se, že používáte hello schéma HTTPS.
4. Na stránce hello dolní hello přečtěte si a přijměte podmínky hello. Pak klikněte na tlačítko **vytvořit aplikaci služby Twitter**. Tato aplikace hello registry zobrazí podrobnosti o aplikace hello.
5. Klikněte na tlačítko hello **nastavení** zkontrolujte **povolit toosign toobe používá tato aplikace se přes Twitter**, pak klikněte na tlačítko **nastavení aktualizace**.
6. Vyberte hello **klíče a přístupové tokeny** kartě. Poznamenejte si hodnoty hello **uživatelský klíč (klíč rozhraní API)** a **uživatelský tajný klíč (tajný klíč rozhraní API)**.
   
   > [!NOTE]
   > Hello uživatelský tajný klíč je důležitým bezpečnostním pověřením. S kýmkoli sdílet tento tajný klíč nebo distribuovat s vaší aplikací.
   > 
   > 

## <a name="secrets"></a>Přidat Twitter informace tooyour aplikace
1. Zpět v hello [portál Azure], přejděte tooyour aplikace. Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.
2. Pokud hello ověřování / autorizace funkce není povolena, zapněte přepínač hello příliš**na**.
3. Klikněte na tlačítko **Twitter**. Vložte hello ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve. Pak klikněte na **OK**.
   
   ![][1]
   
   Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
4. (Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Twitter, nastavte **tootake akce, když požadavek nebude ověřený** příliš**Twitter**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooTwitter pro ověřování.
5. Klikněte na **Uložit**.

Jste nyní připraven toouse Twitter pro ověřování v aplikaci.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter vývojáři]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[portál Azure]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
