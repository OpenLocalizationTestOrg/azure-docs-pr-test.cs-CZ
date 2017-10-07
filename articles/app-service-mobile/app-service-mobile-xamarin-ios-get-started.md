---
title: "aaaGet začít s Azure App Service Mobile Apps pro aplikace pro Xamarin.iOS | Microsoft Docs"
description: "Využijte tento kurz tooget začít s pomocí Mobile Apps pro vývoj na platformě Xamarin.iOS."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: syntaxc4
ms.openlocfilehash: 524c5ac4d8a29d7cb858f74132aad5d6e2201d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinios-app"></a>Vytvoření aplikace Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak tooadd back-end cloudové služby mobilní aplikace Xamarin.iOS tooa pomocí back-end mobilní aplikace Azure.  Vytvoříte nový back-end mobilní aplikace i jednoduchou aplikaci Xamarin.iOS, která bude představovat *seznam úkolů* a bude ukládat data do Azure.

Dokončení tohoto kurzu je předpokladem pro Xamarin.iOS o všech dalších kurzech k používání funkce Mobile Apps hello v Azure App Service.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující požadavky:

* Aktivní účet Azure. Pokud nemáte účet, zaregistrovat zkušební verzi Azure a zprovoznění too10 bezplatné mobilní aplikace, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio s Xamarinem. Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community. Přečtěte si témata o [nastavení a instalaci nástrojů Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a o [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-an-azure-mobile-app-backend"></a>Vytvoření back-endu mobilní aplikace Azure
Postupujte podle těchto kroků toocreate back-end mobilní aplikace.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a>Konfigurace projektu server hello
Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. V dalším kroku stáhnout serverový projekt pro jednoduchý "seznam úkolů" back-end a publikujete ho v tooAzure.

Postupujte podle následujících kroků tooconfigure hello serveru projektu toouse hello buď hello Node.js, nebo .NET back-end.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinios-app"></a>Stažení a spuštění aplikace Xamarin.iOS hello
1. Otevřete hello [portál Azure] v okně prohlížeče.
2. V okně Nastavení hello mobilní aplikace, klikněte na tlačítko **Začínáme** > **Xamarin.iOS**. V kroku 3 klikněte na možnost **Vytvořit novou aplikaci**, pokud ještě nebyla vybrána.  Pak klikněte na hello **Stáhnout** tlačítko.

      Je Stáhnout klientskou aplikaci, která se připojuje tooyour mobilního back-endu. Uložte hello komprimovaný soubor projektu do místního počítače a poznamenejte si kam jste jej uložili.
3. Extrahujte hello projekt, který jste stáhli a otevřete jej v nástroji Xamarin Studio (nebo Visual Studio).

    ![][9]

    ![][8]
4. Stiskněte klávesu hello F5 klíče toobuild hello projektu a spusťte hello aplikaci v emulátoru Iphonu hello.
5. Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*a potom klikněte na hello  **+**  tlačítko.

    ![][10]

    Data z požadavku hello je vloženy do tabulky TodoItem hello. Položky uložené v tabulce hello se vrátí pomocí back-end mobilní aplikace hello a data se zobrazí v seznamu hello.

> [!NOTE]
> Můžete zkontrolovat hello kód, který má přístup k vaší tooquery back-end mobilní aplikace a vkládání dat v souboru C# QSTodoService.cs hello.
>
>

## <a name="next-steps"></a>Další kroky
* [Přidat aplikaci tooyour Offline synchronizace](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Přidat aplikaci tooyour ověřování](app-service-mobile-xamarin-ios-get-started-users.md)
* [Přidat nabízená oznámení tooyour Xamarin.Android aplikaci](app-service-mobile-xamarin-ios-get-started-push.md)
* [Jak toouse hello spravovat klienta pro Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[portál Azure]: https://portal.azure.com/
