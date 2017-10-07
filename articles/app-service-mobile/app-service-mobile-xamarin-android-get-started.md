---
title: "aaaGet Začínáme s Azure Mobile Apps pro aplikace pro Xamarin.Android"
description: "Postupujte podle tohoto kurzu tooget spuštění pomocí Azure Mobile Apps pro vývoj Xamarin Android"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 38710660d9328fe3c068efca972f76aa8b6e049b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinandroid-app"></a>Vytvoření aplikace Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak tooadd back-end cloudové služby tooa aplikace Xamarin.Android. Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md).

Zde je snímek obrazovky aplikace hello dokončena:

![][0]

Ve všech dalších kurzech k používání funkce Mobile Apps pro Xamarin.Android se předpokládá dokončení tohoto kurzu.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující požadavky:

* Aktivní účet Azure. Pokud nemáte účet, zaregistrovat zkušební verzi Azure a zprovoznění too10 bezplatných mobilních aplikací. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio s Xamarinem. Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

## <a name="create-an-azure-mobile-app-backend"></a>Vytvoření back-endu mobilní aplikace Azure
Postupujte podle těchto kroků toocreate back-end mobilní aplikace.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. V dalším kroku stáhnout serverový projekt pro jednoduchý "seznam úkolů" back-end a publikujete ho v tooAzure.

## <a name="configure-hello-server-project"></a>Konfigurace projektu server hello
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinandroid-app"></a>Stažení a spuštění aplikace Xamarin.Android hello
1. V části **stáhnout a spustit projekt Xamarin.Android**, klikněte na tlačítko hello **Stáhnout** tlačítko.

      Uložte hello projektu komprimovaný soubor tooyour místního počítače a poznamenejte si kam jste jej uložili.
2. Stiskněte klávesu hello **F5** klíče toobuild hello projektu a spusťte aplikaci hello.
3. Hello aplikace zadejte smysluplný text, například *hello dokončení kurzu* a pak klikněte na tlačítko hello **přidat** tlačítko.

    ![][10]

    Data z požadavku hello je vloženy do tabulky TodoItem hello. Položky uložené v tabulce hello se vrátí pomocí back-end mobilní aplikace hello a data se zobrazí v seznamu hello.

   > [!NOTE]
   > Můžete zkontrolovat hello kód, který má přístup k vaší tooquery back-end mobilní aplikace a vkládání dat, který se nachází v souboru C# ToDoActivity.cs hello.
   >
   >

## <a name="next-steps"></a>Další kroky
* [Přidat aplikaci tooyour Offline synchronizace](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Přidat aplikaci tooyour ověřování](app-service-mobile-xamarin-android-get-started-users.md)
* [Přidat nabízená oznámení tooyour Xamarin.Android aplikaci](app-service-mobile-xamarin-android-get-started-push.md)
* [Jak toouse hello spravovat klienta pro Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
