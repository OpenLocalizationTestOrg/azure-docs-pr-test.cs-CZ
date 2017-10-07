---
title: "ověřování Google tooconfigure aaaHow pro vaši aplikaci aplikační služby"
description: "Zjistěte, jak ověřování Google tooconfigure pro vaši aplikaci aplikační služby."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 9175c40b78c859e9e191504c41cd0bb9a3380ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-google-login"></a>Jak tooconfigure přihlašování Google toouse aplikace služby App Service
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak Azure App Service toouse tooconfigure jako zprostředkovatel ověřování Google.

toocomplete hello postup v tomto tématu, musíte mít účet Google s ověřenou e-mailovou adresu. toocreate nový účet Google, přejděte příliš[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"></a>Registrace vaší aplikace službou Google
1. Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace. Kopie vašeho **URL**, které použijete novější tooconfigure aplikace Google.
2. Přejděte toohello [rozhraní Google API](http://go.microsoft.com/fwlink/p/?LinkId=268303) klikněte na web, přihlaste se pomocí svých přihlašovacích údajů účtu Google **vytvoření projektu**, poskytovat **název projektu**, pak klikněte na tlačítko  **Vytvoření**.
3. V části **sociálních rozhraní API** klikněte na tlačítko **rozhraní API Google +** a potom **povolit**.
4. V levé navigaci, hello **pověření** > **obrazovky souhlas OAuth**, pak vyberte vaše **e-mailová adresa**, zadejte **název produktu**a klikněte na tlačítko **Uložit**.
5. V hello **pověření** , klikněte na **vytvořit přihlašovací údaje** > **ID klienta OAuth**, pak vyberte **webové aplikace**.
6. Vložení hello služby App Service **URL** jste zkopírovali dříve do **oprávnění zdroje JavaScript**, vložte vaší přesměrování URI do **oprávnění identifikátor URI pro přesměrování**. Hello hello adresu URL aplikace připojí s cestou hello je identifikátor URI přesměrování */.auth/login/google/callback*. Například, `https://contoso.azurewebsites.net/.auth/login/google/callback`. Ujistěte se, že používáte hello schéma HTTPS. Poté klikněte na **Vytvořit**.
7. Na další obrazovce hello si poznamenejte hodnoty hello klienta hello ID a klienta tajný klíč.

    > [!IMPORTANT]
    > tajný klíč klienta Hello je důležitým bezpečnostním pověřením. S kýmkoli sdílet tento tajný klíč nebo distribuovat v rámci klientské aplikace.


## <a name="secrets"></a>Přidat Google informace tooyour aplikace
1. Zpět v hello [portál Azure], přejděte tooyour aplikace. Klikněte na tlačítko **nastavení**a potom **ověřování / autorizace**.
2. Pokud hello ověřování / autorizace funkce není povolena, zapněte přepínač hello příliš**na**.
3. Klikněte na tlačítko **Google**. Vložte hello ID aplikace a tajný klíč aplikace pro hodnoty, které jste získali dříve a volitelně povolte všechny obory, které vaše aplikace vyžaduje. Pak klikněte na **OK**.
   
   ![][1]
   
   Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
4. (Volitelné) toorestrict přístup tooyour lokality tooonly uživatele ověřeného službou Google, nastavte **tootake akce, když požadavek nebude ověřený** příliš**Google**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky jsou přesměrovaného tooGoogle pro ověřování.
5. Klikněte na **Uložit**.

Jste nyní připraven toouse Google pro ověřování v aplikaci.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[portál Azure]: https://portal.azure.com/

