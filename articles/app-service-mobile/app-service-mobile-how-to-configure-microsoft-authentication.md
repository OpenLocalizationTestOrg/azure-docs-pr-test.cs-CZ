---
title: "aaaHow tooconfigure Account Microsoft ověřování pro aplikaci aplikační služby"
description: "Zjistěte, jak tooconfigure Account Microsoft ověřování pro aplikaci aplikační služby."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d86d8dab26a189f4454082fc18e44e3fb6e0a01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-microsoft-account-login"></a>Jak tooconfigure přihlašování Account Microsoft toouse aplikace služby App Service
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Toto téma ukazuje, jak Azure App Service toouse tooconfigure Account Microsoft jako zprostředkovatel ověřování. 

## <a name="register-microsoft-account"></a>Svou aplikaci zaregistrovat pomocí účtu Microsoft
1. Přihlaste se toohello [portál Azure]a přejděte tooyour aplikace. Kopie vašeho **URL**, které později použijete tooconfigure vaší aplikace s Account Microsoft.
2. Přejděte toohello [Moje aplikace] stránku hello Microsoft Account Developer Center a přihlaste se pomocí účtu Microsoft, v případě potřeby.
3. Klikněte na tlačítko **přidat aplikaci**, pak zadejte název aplikace a klikněte na **vytvořit aplikaci**.
4. Poznamenejte si hello **ID aplikace**, protože jej budete potřebovat později. 
5. V části "Platformy", klikněte na **přidejte platformu** a vyberte možnost "Web".
6. V části "Identifikátory URI přesměrování" koncový bod zadejte hello pro vaši aplikaci, pak klikněte na **Uložit**. 
   
   > [!NOTE]
   > Vaše přesměrování identifikátor URI je adresa URL hello aplikace připojí s cestou hello */.auth/login/microsoftaccount/callback*. Například, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Ujistěte se, že používáte hello schéma HTTPS.
   
7. V části "Aplikace tajné údaje", klikněte na **generovat nové heslo**. Poznamenejte si hello hodnotu, která se zobrazí. Jakmile hello stránku opustíte, nebudou zobrazeny znovu.

    > [!IMPORTANT]
    > heslo Hello je důležitým bezpečnostním pověřením. S kýmkoli sdílet hello heslo nebo distribuovat v rámci klientské aplikace.

## <a name="secrets"></a>Tooyour informace přidat účet Microsoft aplikace služby App Service
1. Zpět v hello [portál Azure]přejděte tooyour aplikace, klikněte na tlačítko **nastavení** > **ověřování / autorizace**.
2. Pokud hello ověřování / autorizace funkce není povolena, je přepínač **na**.
3. Klikněte na tlačítko **účtu Microsoft**. Vložte hodnoty hello ID aplikace a heslo, které jste získali dříve a volitelně povolte všechny obory, které vaše aplikace vyžaduje. Pak klikněte na **OK**.
   
    ![][1]
   
    Ve výchozím nastavení služby App Service poskytuje ověřování, ale neomezuje obsahu webu tooyour autorizovaný přístup a rozhraní API. Je nutné autorizovat uživatele v kódu aplikace.
4. (Volitelné) toorestrict přístup tooyour lokality tooonly uživatelé ověřit účet Microsoft nastavit **tootake akce, když požadavek nebude ověřený** příliš**Account Microsoft**. To vyžaduje, že všechny žádosti o ověření a všechny neověřené požadavky se přesměrují tooMicrosoft účet pro ověřování.
5. Klikněte na **Uložit**.

Jste nyní připraven toouse Account Microsoft pro ověřování v aplikaci.

## <a name="related-content"></a>Související obsah
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[portál Azure]: https://portal.azure.com/
