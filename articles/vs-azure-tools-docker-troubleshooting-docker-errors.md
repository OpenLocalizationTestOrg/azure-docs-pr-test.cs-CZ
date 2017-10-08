---
title: "aaaTroubleshooting chyby Docker klienta v systému Windows pomocí sady Visual Studio | Microsoft Docs"
description: "Řešení problémů, které zaznamenáte při používání sady Visual Studio toocreate a nasaďte tooDocker webové aplikace v systému Windows pomocí sady Visual Studio."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 7421ae8e044d58fc412d748fb870da4c9b2fdb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>Řešení potíží s Visual Studio Docker vývoj

Při práci s nástroji Visual Studio Tools pro Docker Preview, se může vyskytnout některé problémy, kvůli hello povaha hello preview.
Toto jsou některé běžné problémy a řešení.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Kontejnery Linux**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Sestavení dojít k chybám při ladění aplikace web nebo konzoly .NET Core  

To může být související toonot sdílení s Docker pro Windows hello jednotky, kde se nachází hello projektu.  Může dojít chybě jako hello následující:

```
hello "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with hello default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
tooresolve tohoto problému:

1. Klikněte pravým tlačítkem na **Docker pro systém Windows** v oznamovací oblasti text hello a potom vyberte **nastavení**.  
2. Vyberte **sdílené disky** a sdílet hello jednotku, kde se nachází hello projektu.

### <a name="windows-containers"></a>**Kontejnery Windows**

Hello následující problémy jsou konkrétní toodebugging rozhraní .NET Framework web a konzole aplikace v kontejnerech systému Windows.

#### <a name="prerequisites"></a>Požadavky

1. Visual Studio 2017 RC (nebo novější) s hello .NET Core a zatížení Docker Preview musí být nainstalován.
2. Windows 10 Anniversary aktualizaci s aby Windows Update nejnovější opravy. Konkrétně [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) musí být nainstalován. 
3. [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/) (sestavení 1.13.0 nebo novější) musí být nainstalován.
4. **Přepínač tooWindows kontejnery** musí být vybrán. V oznamovací oblasti hello, klikněte na tlačítko **Docker pro systém Windows**a potom vyberte **přepínač kontejnery tooWindows**. Po restartování počítače hello, ujistěte se, že toto nastavení je zachován.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Výstup konzoly se nezobrazí v okně výstupu sady Visual Studio při ladění aplikace konzoly

Jedná se o známý problém s hello ladicí program Visual Studio (msvsmon.exe), který není aktuálně určený pro tento scénář. Podpora pro tento scénář mohou být zahrnuta v budoucí verzi. toosee výstup hello konzolové aplikace v sadě Visual Studio, použijte **Docker: spuštění projektu**, což je totéž příliš**spustit bez ladění**.

#### <a name="debugging-web-applications-with-hello-release-configuration-fails-with-403-forbidden-error"></a>Ladění webových aplikací s hello verze konfigurace selže s chybou (403) zakázáno

toowork tento problém odstranit, otevřete web.release.config v hello řešení a poznámky nebo odstranit hello následující řádky:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Kontejnery Linux**

#### <a name="unable-toovalidate-volume-mapping"></a>Mapování nelze toovalidate svazku
Mapování svazek je požadovaná tooshare hello zdrojový kód a binární soubory aplikace se složkou aplikace hello v kontejneru hello.  Mapování konkrétní svazku jsou obsaženy v rámci docker compose.dev.debug.yml a docker compose.dev.release.yml. Jak se změnily soubory na hostitelském počítači, hello kontejnery podle těchto změn v podobné struktura složek.

mapování tooenable svazku:

1. Klikněte na tlačítko **Moby** v oznamovací oblasti hello a vyberte **nastavení**.
2. Vyberte **sdílené disky**.
3. Vyberte jednotku hello, který je hostitelem vašeho projektu a hello disku, kde se nachází % USERPROFILE %.
4. Klikněte na tlačítko **Použít**.

tootest Pokud funguje mapování svazek znovu sestavit a po jedné nebo více jednotek byly sdílené, nebo spusťte následující kód z příkazového řádku hello možnost F5 z Visual Studia.

> [!NOTE]
> Tento příklad předpokládá, že vaši uživatelé složka je umístěna na jednotce C a zda je sdílen.
> Upraveno v případě potřeby, pokud jste sdíleli jinou jednotku.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Spusťte následující kód v kontejneru Linux hello hello.

```
/ # ls
```

Měli byste vidět ze složky uživatele a veřejného hello výpis adresáře. Pokud jsou zobrazeny žádné soubory a složky /c/Users/Public není prázdný, mapování svazek není správně nakonfigurována.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Změňte adresář díry toohello toosee hello obsah hello `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> Při práci s virtuálními počítači Linux, systém souborů kontejneru hello je malá a velká písmena.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Sestavení: Úlohy "PrepareForBuild" se nezdařilo neočekávaně

Microsoft.DotNet.Docker.CommandLine.ClientException: Došlo k chybě snažíme tooconnect.

Ověřte, že tento hello výchozího Docker hostitele běží. Otevřete příkazový řádek a spusťte:

```
docker info
```

Pokud tento příkaz vrátí chybu, poté pokus toostart hello **Docker pro systém Windows** aplikace na ploše. Pokud je spuštěná aplikace na ploše hello, pak **Moby** by měly jít vidět v oznamovací oblasti hello. Klikněte pravým tlačítkem na **Moby** a otevřete **nastavení**. Klikněte na tlačítko **resetovat**a pak restartujte Docker.

## <a name="an-error-dialog-occurs-when-attempting-tooadd-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Dialogové okno chyby dojde při pokusu o tooadd podporu Docker nebo ladění aplikace ASP.NET Core v kontejneru (F5)

Po odinstalaci a instalace rozšíření, může být poškozen hello mezipaměti Managed Extensibility Framework (MEF) v sadě Visual Studio. V takovém případě může způsobit různé chybové zprávy, když se přidání podpory Docker nebo pokusu o toorun nebo ladění aplikace ASP.NET Core (F5). Jako dočasné řešení použijte následující kroky toodelete a znovu vygenerovat hello MEF mezipaměti hello.

1. Zavřete všechny instance sady Visual Studio.
1. Otevřete % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.
1. Odstraňte hello následující složky:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Otevřete sadu Visual Studio.
1. Scénář hello pokus opakujte.
