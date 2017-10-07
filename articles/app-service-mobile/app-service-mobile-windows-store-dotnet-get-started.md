---
title: "aaaCreate univerzální platformu Windows (UWP) používající v Mobile Apps | Microsoft Docs"
description: "Využijte tento kurz tooget začít s pomocí back-EndY mobilní aplikace Azure pro vývoj aplikací pro univerzální platformu Windows (UWP) v C#, Visual Basic nebo JavaScript."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: d0f57bca5a8195b8b0461b8f7a0d8516371d56a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-app"></a>Vytvoření aplikace pro Windows
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak tooadd back-end cloudové služby tooa univerzální platformu Windows (UWP) aplikace. Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md). Následující Hello jsou z aplikace hello dokončit zachycení obrazovky:

![Hotová desktopová aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Spuštěná na ploše

![Hotová telefonní aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Spuštěná na telefonu

Ve všech dalších kurzech Mobile App pro aplikace UPW se předpokládá dokončení tohoto kurzu.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. Pokud nemáte účet, můžete si zaregistrovat zkušební verzi Azure a získat až too10 bezplatné mobilní aplikace, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio Community 2015] nebo novější verzi

## <a name="create-a-new-azure-mobile-app-backend"></a>Vytvoření nového back-endu mobilní aplikace Azure
Postupujte podle těchto kroků toocreate nový back-end mobilní aplikace.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. Dále si stáhnete serverový projekt pro jednoduchý "seznam úkolů" back-end a publikujete ho v tooAzure.

## <a name="configure-hello-server-project"></a>Konfigurace projektu server hello
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-client-project"></a>Stažení a spuštění projektu klienta hello
Jakmile jste nakonfigurovali váš back-end mobilní aplikace, můžete vytvořit novou klientskou aplikaci nebo upravovat existující tooconnect tooAzure aplikace. V této části stáhnete projekt šablony aplikace UPW, který je přizpůsobené tooconnect tooyour mobilní aplikace back-end.

1. Zpět v hello **úvodní** back-endu mobilní aplikace, klikněte na **vytvořit novou aplikaci** > **Stáhnout**, extrahujte hello komprimované soubory projektu tooyour místního počítače.

    ![Stáhnutí projektu typu rychlý start pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. (Volitelné) Přidat toohello projekt aplikace UPW hello stejného řešení jako hello serverový projekt. To usnadňuje toodebug a testovací obě hello aplikace a hello back-end v hello stejné řešení sady Visual Studio, pokud si zvolíte toodo tak. tooadd řešení toohello projekt aplikace UPW, používejte Visual Studio 2015 nebo novější verze.
3. Hello aplikace pro UPW jako spouštěný projekt hello stiskněte klíče toodeploy hello F5 a spusťte hello aplikace.
4. Hello aplikace zadejte smysluplný text, například *hello dokončení kurzu*, v hello **vložení úkolu** textového pole a pak klikněte na tlačítko **Uložit**.

    ![Dokončení desktopového projektu s rychlým startem pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Tím se odešle POST požadavek toohello nový mobilní aplikace back-end hostovaný v Azure.
5. (Volitelné) Aplikace hello zastavte a restartujte ji na jiném zařízení nebo emulátoru mobilního.

    ![Dokončení telefonního projektu s rychlým startem pro Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Všimněte si, že data uložená hello v předchozím kroku načetla z Azure po spuštění aplikace pro UPW hello.

## <a name="next-steps"></a>Další kroky
* [Přidat aplikaci tooyour ověřování](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Přidat nabízená oznámení tooyour aplikaci](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend nabízených oznámení.
* [Povolení offline synchronizace u aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
