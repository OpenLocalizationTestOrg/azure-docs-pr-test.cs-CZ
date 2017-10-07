---
title: "aaaAdd ověřování v systému Android s Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak toouse hello funkce Mobile Apps služby Azure App Service tooauthenticate uživatelů vaší aplikace Android prostřednictvím řady různých zprostředkovatelů identity, včetně Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 01f608f996c931c643790ed2778df11cf590c903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-android-app"></a>Přidat ověřování tooyour aplikace pro Android
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Souhrn
V tomto kurzu přidáte toohello ověřování, seznamu úkolů projektu typu rychlý start v systému Android pomocí zprostředkovatele identity podporované. V tomto kurzu vychází z hello [Začínáme s Mobile Apps] kurz, který je třeba nejprve provést.

## <a name="register"></a>Registrace aplikace pro ověřování a konfigurace Azure App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování

Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci. To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello. V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu. Můžete však použít žádné schéma adresy URL, které zvolíte. Mělo by být jedinečný tooyour mobilních aplikací. tooenable hello přesměrování na straně serveru hello:

1. V [portál Azure] hello vyberte App Service.

2. Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.

3. V hello **povoleno externí adres URL pro přesměrování**, zadejte `appname://easyauth.callback`.  Hello _appname_ v tento řetězec je hello schéma adresy URL pro mobilní aplikace.  Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).  Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.

4. Klikněte na **OK**.

5. Klikněte na **Uložit**.

## <a name="permissions"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* V nástroji Android Studio otevřete projekt hello jste dokončili s hello kurzu [Začínáme s Mobile Apps]. Z hello **spustit** nabídky, klikněte na tlačítko **spuštění aplikace**a ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).

     Tato výjimka se stane, protože pokusy aplikace hello tooaccess hello zpět ukončení jako neověřený uživatel, ale hello *TodoItem* tabulka nyní vyžaduje ověření.

Potom aktualizujte uživatele tooauthenticate aplikace hello před vyžádáním prostředky z hello končit Mobile Apps zpět. 

## <a name="add-authentication-toohello-app"></a>Přidat aplikaci toohello ověřování
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>Tokeny ověřování mezipaměti na klientovi hello
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili kurz základní ověřování, zvažte, budete pokračovat v tooone hello následující kurzy:

* [Přidat nabízená oznámení tooyour Android aplikaci](app-service-mobile-android-get-started-push.md).
  Zjistěte, jak tooconfigure Mobile Apps zpět ukončení toouse Azure notification hubs toosend nabízená oznámení.
* [Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).
  Zjistěte, jak tooadd offline podporovat tooyour aplikaci pomocí back-end mobilní aplikace. S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions tooauthenticated users]: #permissions
[Add authentication toohello app]: #add-authentication
[Store authentication tokens on hello client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Začínáme s Mobile Apps]: app-service-mobile-android-get-started.md
