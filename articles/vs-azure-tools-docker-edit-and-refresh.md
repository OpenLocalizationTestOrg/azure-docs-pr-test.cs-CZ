---
title: "aaaDebugging aplikací v místním kontejner Docker | Microsoft Docs"
description: "Zjistěte, jak toomodify aplikaci, která běží v místní kontejner Docker, aktualizujte hello kontejneru prostřednictvím úpravy a aktualizace a nastavit zarážky ladění"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: ff64e62fbb93901a29b5496bd5e17d2c4ea5ca99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Ladění aplikací v místním kontejneru Dockeru
## <a name="overview"></a>Přehled
Hello Visual Studio Tools for Docker zajišťuje konzistentní způsob toodevelop v a ověřte svoji aplikaci místně v kontejner Linux Docker.
Nemáte kontejner hello toorestart pokaždé, když provedete kód změnit.
Tento článek ukazuje, jak toostart funkce pro "Upravit a aktualizovat" hello toouse ASP.NET Core webové aplikace v místním kontejner Docker, proveďte potřebné změny a potom aktualizujte prohlížeč toosee hello tyto změny.
Tento článek také ukazuje, jak tooset zarážky pro ladění.

> [!NOTE]
> Podpora kontejnerů Windows budou přicházet v budoucí verzi
>
>

## <a name="prerequisites"></a>Požadavky
musí být nainstalován Hello následující nástroje.

* [Nejnovější verze sady Visual Studio](https://www.visualstudio.com/downloads/)
* [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

toorun místně Docker kontejnery, budete potřebovat místní docker klienta.
Můžete použít hello [Docker nástrojů](https://www.docker.com/products/docker-toolbox), což vyžaduje, aby zakázáno toobe technologie Hyper-V, nebo můžete použít [Docker pro systém Windows](https://www.docker.com/get-docker), která využívá technologie Hyper-V a vyžaduje Windows 10.

Pokud používáte Docker sada nástrojů, budete potřebovat příliš[konfigurace klienta Docker hello](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Vytvoření webové aplikace
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Přidání podpory Docker
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. Upravit kód a aktualizace
tooquickly iteraci změny, můžete spustit aplikaci v rámci kontejneru a pokračovat toomake změny, jako byste službou IIS Express je zobrazení.

1. Nastavit hello konfigurace řešení příliš`Debug` a stiskněte klávesu  **&lt;kombinaci kláves CTRL + F5 >** toobuild vaše docker bitovou kopii a spustit místně.

    Po hello kontejneru image byla vytvořena a spuštěna v kontejner Docker, spustí se v sadě Visual Studio hello webové aplikace ve výchozí prohlížeč.
    Pokud používáte prohlížeč Microsoft Edge hello nebo v opačném případě obsahuje chyby, přečtěte si téma [Poradce při potížích s](vs-azure-tools-docker-troubleshooting-docker-errors.md) části.
2. Přejděte toohello o stránky, kde vytvoříme toomake naše změny.
3. Vrátí tooVisual Studio a otevřete `Views\Home\About.cshtml`.
4. Přidejte následující HTML obsahu toohello konec souboru hello hello a uložit změny hello.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Okno výstup hello, zobrazení, jakmile bude dokončena hello sestavení rozhraní .NET a zobrazit tyto řádky, přepněte zpět tooyour prohlížeče a aktualizujte hello o stránce.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C tooshut down
   ```
6. Změny se aplikovaly!

## <a name="4-debug-with-breakpoints"></a>4. Ladění pomocí zarážek
Často změny potřebovat další kontroly, využití hello ladění funkce sady Visual Studio.

1. Vrátí tooVisual Studio a otevřete`Controllers\HomeController.cs`
2. Nahraďte obsah hello hello About() metody hello následující:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. Sada toohello zarážku pravé hello `string message`... řádku.
4. Stiskněte tlačítko  **&lt;F5 >** toostart ladění.
5. Přejděte toohello o stránku toohit vaší zarážce.
6. Přepnout zarážku tooVisual Studio tooview hello – a kontrola hello hodnotu zprávy.

   ![][2]

## <a name="summary"></a>Souhrn
S [2015 nástroje sady Visual Studio pro Docker](https://aka.ms/DockerToolsForVS), můžete získat hello produktivitu místně, pracovat s produkční realism hello vývoje v rámci kontejner Docker.

## <a name="troubleshooting"></a>Řešení potíží
[Řešení potíží s Visual Studio Docker vývoj](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Další informace o Docker s Visual Studio, Windows a Azure
* [Docker nástrojů pro Visual Studio](http://aka.ms/dockertoolsforvs) – vývoj kódu .NET Core v kontejneru
* [Docker nástrojů pro Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – sestavení a nasazení kontejnerů docker
* [Docker nástrojů pro Visual Studio Code](http://aka.ms/dockertoolsforvscode) -services jazyk pro úpravy docker soubory s více scénářů e2e přicházející
* [Informace o kontejneru systému Windows](http://aka.ms/containers)-informace o systému Windows Server a Nano Server
* [Azure Container Service](https://azure.microsoft.com/services/container-service/) - [obsahu Azure Container Service](http://aka.ms/AzureContainerService)
* Další příklady práce s Docker, najdete v části [práce s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 připojit [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Další quickstarts z hello HealthClinic.biz ukázku, najdete v části [Quickstarts nástroje pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Různé nástroje Docker
[Některé skvělé docker nástroje (blog Steva Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Dobrý články
[Úvod tooMicroservices z NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Prezentace
* [Steve Lasker: VS Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Úvod tooASP.NET základní @ sestavení 2016 – kde jste v ukázce](https://channel9.msdn.com/Events/Build/2016/B810)
* [Vývoj aplikací .NET v kontejnerech, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
