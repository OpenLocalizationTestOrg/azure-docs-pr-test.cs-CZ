---
title: "aaaGet začít s Mobile Apps pomocí Xamarin.Forms"
description: "Postupujte podle tohoto kurzu toostart používat Mobile Apps pro vývoj s Xamarin.Forms."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: af6b1c1ce4cf91c397552aa3d8ee40728129238c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinforms-app"></a>Vytvoření aplikace na platformě Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak hello tooadd Cloudová služba back-end tooa Xamarin.Forms mobilní aplikace pomocí funkce Mobile Apps služby Azure App Service jako hello back-end. Vytvoříte jak nový back-end Mobile Apps, tak jednoduchou aplikaci Xamarin.Forms, která bude představovat seznam úkolů a ukládat data do Azure.

Ve všech dalších kurzech k Mobile Apps týkajících se Xamarin.Forms se předpokládá dokončení tohoto kurzu.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. Pokud nemáte účet, můžete si zaregistrovat zkušební verzi Azure a získat až too10 bezplatné mobilní aplikace, které můžete používat i po skončení zkušebního období. Další informace najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarinem. Informace najdete v tématu hello [nastavit až a nainstalujte Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) stránky.

* Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community. Informace najdete v článcích věnovaných [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx) a [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Vytvoření nového back-endu Mobile Apps

toocreate ukončit nové mobilní aplikace zpět hello následující:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Právě jste nastavili back-end Mobile Apps, který můžou používat vaše mobilní klientské aplikace. V dalším kroku stáhnout serverový projekt pro jednoduchý úkolů seznamu back-end a pak ho publikujete tooAzure.

## <a name="configure-hello-server-project"></a>Konfigurace projektu server hello

tooconfigure hello serveru projektu toouse hello Node.js, nebo .NET back-end, hello následující:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinforms-solution"></a>Stažení a spuštění řešení Xamarin.Forms hello

Můžete si stáhnout hello řešení v některém ze dvou způsobů. Stažení tooa Mac a otevřít v aplikaci Xamarin Studio nebo ho stáhnout na počítač se systémem Windows tooa a otevřete v sadě Visual Studio pomocí síťově připojeného počítače Mac pro vytvoření aplikace pro iOS hello. Další informace najdete v článku věnovaném [nastavení a instalaci sady Visual Studio a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).

V počítači Mac nebo Windows hello následující:

1. Přejděte toohello [portál Azure].

2. Na hello **nastavení** své mobilní aplikace, v části **Mobile**, vyberte **Začínáme** > **Xamarin.Forms**. V **kroku 3** vyberte **Vytvořit novou aplikaci** a pak vyberte **Stáhnout**.

   Tato akce stáhne projekt, který obsahuje klientskou aplikaci, která je připojená tooyour mobilní aplikace. Uložte hello projektu komprimovaný soubor tooyour místního počítače a poznamenejte si kam jste jej uložili.

3. Extrahujte hello projekt, který jste stáhli a potom ho otevřete v nástroji Xamarin Studio (Mac) nebo Visual Studio (Windows).

   ![Extrahovaný projekt v nástroji Xamarin Studio][9]

   ![Extrahovaný projekt v sadě Visual Studio][8]

## <a name="optional-run-hello-ios-project"></a>(Volitelné) Spuštění projektu iOS hello
V této části spustíte hello projektu Xamarin iOS pro zařízení s iOS. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-xamarin-studio"></a>V nástroji Xamarin Studio
1. Klikněte pravým tlačítkem na projekt pro iOS hello a potom vyberte **nastavit jako spouštěný projekt**.

2. Na hello **spustit** nabídce vyberte možnost **spustit ladění** toobuild hello projektu a spusťte hello aplikaci v emulátoru Iphonu hello.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio
1. Klikněte pravým tlačítkem na projekt pro iOS hello a potom vyberte **nastavit jako spouštěný projekt**.

2. Na hello **sestavení** nabídce vyberte možnost **nástroje Configuration Manager**.

3. V hello **nástroje Configuration Manager** dialogové okno, vyberte hello **sestavení** a **nasadit** projektu iOS toohello další zaškrtávací políčka.

4. toobuild hello projektu a spusťte hello aplikaci v emulátoru Iphonu hello, vyberte hello **F5** klíč.

   > [!NOTE]
   > Pokud máte potíže s sestavení projektu hello, spusťte hello NuGet balíček správce a aktualizace toohello nejnovější verzi podpůrných balíčků Xamarin hello. Projekty rychlý start může být pomalé tooupdate toohello nejnovější verze.    
   >
   >

5. Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*, a pak vyberte hello znaménko plus (**+**).

    ![][10]

    Tato akce odešle toohello požadavek post nové Mobile Apps back-end, který je hostován v Azure. Data z požadavku hello je vloženy do tabulky TodoItem hello. Položky, které jsou uložené v tabulce hello se vrátí pomocí hello Mobile Apps zpět ukončení a text hello, že data se zobrazí v seznamu hello.

    > [!NOTE]
    > Najdete tu hello kód, který přistupuje k Mobile Apps back-end vašeho v hello souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd hello vašeho řešení.
    >
    >

## <a name="optional-run-hello-android-project"></a>(Volitelné) Spusťte projekt Android hello
V této části spuštění projektu Xamarin Android hello pro Android. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-xamarin-studio"></a>V nástroji Xamarin Studio

1. Klikněte pravým tlačítkem na projekt pro Android hello a potom vyberte **nastavit jako spouštěný projekt**.

2. toobuild hello projektu a spusťte aplikaci hello v Android emulátoru, na hello **spustit** nabídce vyberte možnost **spustit ladění**.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio

1. Klikněte pravým tlačítkem na projekt hello Android (Droid) a potom vyberte **nastavit jako spouštěný projekt**.

2. Na hello **sestavení** nabídce vyberte možnost **nástroje Configuration Manager**.

3. V hello **nástroje Configuration Manager** dialogové okno, vyberte hello **sestavení** a **nasadit** zaškrtávací políčka další toohello projekt pro Android.

4. toobuild hello projektu a spusťte aplikaci hello v Android emulátoru, vyberte hello **F5** klíč.

   > [!NOTE]
   > Pokud máte potíže s sestavení projektu hello, spusťte hello NuGet balíček správce a aktualizace toohello nejnovější verzi podpůrných balíčků Xamarin hello. Projekty rychlý start může být pomalé tooupdate toohello nejnovější verze.    
   >
   >

5. Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*, a pak vyberte hello znaménko plus (**+**).

    ![][11]
    
    Tato akce odešle toohello požadavek post nové Mobile Apps back-end, který je hostován v Azure. Data z požadavku hello je vloženy do tabulky TodoItem hello. Položky, které jsou uložené v tabulce hello se vrátí pomocí hello Mobile Apps zpět ukončení a text hello, že data se zobrazí v seznamu hello.
    
    > [!NOTE]
    > Najdete tu hello kód, který přistupuje k Mobile Apps back-end vašeho v hello souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd hello vašeho řešení.
    >
    >

## <a name="optional-run-hello-windows-project"></a>(Volitelné) Spusťte projekt Windows hello

V této části spuštění projektu Xamarin WinApp pro zařízení se systémem Windows hello. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="in-visual-studio"></a>V nástroji Visual Studio

1. Klikněte pravým tlačítkem na projekty pro Windows hello a potom vyberte **nastavit jako spouštěný projekt**.

2. Na hello **sestavení** nabídce vyberte možnost **nástroje Configuration Manager**.

3. V hello **nástroje Configuration Manager** dialogové okno, vyberte hello **sestavení** a **nasadit** zaškrtávací políčka další toohello Windows projekt, který jste zvolili.

4. toobuild hello projektu a spusťte hello aplikaci v emulátoru systému Windows, vyberte hello **F5** klíč.

   > [!NOTE]
   > Pokud máte potíže s sestavení projektu hello, spusťte hello NuGet balíček správce a aktualizace toohello nejnovější verzi podpůrných balíčků Xamarin hello. Projekty rychlý start může být pomalé tooupdate toohello nejnovější verze.    
   >
   >

5. Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*, a pak vyberte hello znaménko plus (**+**).

    Tato akce odešle toohello požadavek post nové Mobile Apps back-end, který je hostován v Azure. Data z požadavku hello je vloženy do tabulky TodoItem hello. Položky, které jsou uložené v tabulce hello se vrátí pomocí hello Mobile Apps zpět ukončení a text hello, že data se zobrazí v seznamu hello.
    
    ![][12]
    
    > [!NOTE]
    > Najdete tu hello kód, který přistupuje k Mobile Apps back-end vašeho v hello souboru C# TodoItemManager.cs v projektu knihovny přenosných tříd hello vašeho řešení.
    >
    >

## <a name="next-steps"></a>Další kroky

* [Přidat aplikaci tooyour ověřování](app-service-mobile-xamarin-forms-get-started-users.md)  
  Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Přidat nabízená oznámení tooyour aplikaci](app-service-mobile-xamarin-forms-get-started-push.md)  
  Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend hello nabízených oznámení.

* [Povolení offline synchronizace u aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Zjistěte, jak tooadd offline podporu pro vaši aplikaci pomocí Mobile Apps back-end. Offline synchronizace umožňuje zobrazovat, přidávat nebo upravovat data mobilní aplikace i když nemá připojení k síti.

* [Použití hello spravovaného klienta pro mobilní aplikace](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Zjistěte, jak spravovat toowork s hello klienta SDK v aplikaci Xamarin.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[portál Azure]: https://portal.azure.com/
