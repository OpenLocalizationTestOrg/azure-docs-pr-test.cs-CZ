---
title: "Použít .NET Core ve webové aplikaci v systému Linux | Microsoft Docs"
description: "Pomocí .NET Core ve webové aplikaci v systému Linux."
keywords: "služby Azure app service, webové aplikace, dotnet, core, linux, operačních systémů"
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.openlocfilehash: 9226dfb90e52ac2cae2cfc4af7c0705a93f56b44
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a>Použití .NET Core v Azure webové aplikace v systému Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Webová aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) v systému Linux nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby pomocí operační systém Linux. Tento kurz obsahuje podrobné pokyny znázorňující postup tvorby [.NET Core](https://docs.microsoft.com/aspnet/core/) aplikace na Azure webové aplikace v systému Linux. 

![Webová aplikace v Linuxu][10]

Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux.

## <a name="prerequisites"></a>Požadavky ##

Pro absolvování tohoto kurzu potřebujete: 

* Nainstalujte [.NET Core SDK](https://www.microsoft.com/net/download/core).
* Nainstalujte [Git](https://git-scm.com/downloads).

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a>Vytvořit místní aplikace .NET Core ##

Spusťte novou relaci terminálu. Vytvořte adresář s názvem `hellodotnetcore`a k jeho změně aktuálního adresáře. Potom zadejte následující příkaz: 

```
dotnet new web
``` 

  Tento příkaz vytvoří tři soubory (*hellodotnetcore.csproj*, *Program.cs*, a *Startup.cs*) a jeden prázdnou složku (*wwwroot /*) v aktuálním adresáři. Obsah `.csproj` soubor by měl vypadat takto: 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

Vzhledem k tomu, že tato aplikace je webová aplikace, odkaz na balíček ASP.NET Core se automaticky přidá do *hellodotnetcore.csproj* souboru. Číslo verze balíčku je nastavena podle zvolené framework. V tomto příkladu odkazuje ASP.NET Core verze 1.1.2, protože se používá .NET Core 1.1.

## <a name="build-and-test-the-application-locally"></a>Vytvoření a testování aplikace místně ##

Můžete sestavit a spustit aplikace .NET Core pomocí `dotnet restore` příkazu s parametrem `dotnet run` příkaz, jak je vidět tady:

```
dotnet restore
dotnet run
```


Při spuštění aplikace, zobrazí se zpráva s oznámením, že aplikace naslouchá na příchozí požadavky na port. 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Testování procházením `http://localhost:5000/` s prohlížeči. Pokud se vše funguje bez problémů, se zobrazí "Hello, World!" jako výsledek text.

![Testování pomocí prohlížeče][7]

## <a name="create-a-net-core-app-in-the-azure-portal"></a>Vytvoření .NET Core aplikaci na portálu Azure ##

Je třeba nejprve vytvořit prázdnou webovou aplikaci. Přihlaste se k [portál Azure](https://portal.azure.com/) a vytvořte novou [webové aplikace v systému Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).

![Vytváření webové aplikace][1]

Když **vytvořit** otevře se stránka, obsahují podrobné informace o vaší webové aplikace:

![Výběr .NET Core runtime zásobníku][2]

Následující tabulku použijte jako vodítko k vyplnění **vytvořit** stránky a pak vyberte **OK** a **vytvořit** k vytvoření dané aplikace.

| Nastavení      | Navrhovaná hodnota  | Popis                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| Název aplikace. | hellodotnetcore  | Název aplikace. Tento název musí být jedinečný. |
| Předplatné | Vyberte stávající předplatné | Předplatné Azure. |
| Skupina prostředků | myResourceGroup |  Skupina prostředků Azure tak, aby obsahovala webové aplikace. |
| Plán služby App Service | Název existující plán služby App Service |  Plán služby App Service.  |
| Konfiguraci kontejneru | .NET core 1.1 | Typ kontejneru pro tuto webovou aplikaci: integrované, Docker nebo privátní registru. |
| Zdroj bitové kopie  | Integrované  |  Zdroj bitové kopie. |
| Modul runtime zásobníku  | .NET core 1.1  | Modul runtime zásobníku a verze.  |

## <a name="deploy-your-application-via-git"></a>Nasazení aplikace prostřednictvím Git ##

Pomocí Git Nasaďte aplikaci .NET Core do webové aplikace Azure App Service v systému Linux.

Nové webové aplikace Azure je již nakonfigurován nasazení Git. Adresy URL pro Git nasazení zjistíte tak, že přejdete na tuto adresu URL po vložení název vaší webové aplikace:

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

Adresy URL pro Git má následující formulář založený na název vaší webové aplikace:

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

Spusťte následující příkazy k nasazení místní aplikace do Azure webové aplikace: 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

Nemusíte push všechny soubory pod *bin /* nebo *obj /* adresáře aplikace je integrovaná v cloudu, pokud zdrojové soubory aplikace se instaluje do Azure. Po dokončení procesu sestavení binární soubory se zkopírují do adresáře aplikace v */home/server/wwwroot/*.

Potvrďte, že operace vzdálené nasazení sestav úspěch. Nabízená operace může trvat od balíčku řešení a sestavení procesu spouštění v cloudu. Zobrazí se několik stavové zprávy, včetně těch, které jsou s oznámením, že soubory byly zkopírovány. Výstup by měl vypadat podobně jako následující:

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
To https://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

Po dokončení nasazení restartování webové aplikace pro nasazení vstoupily v platnost. Chcete-li to provést, přejděte na portál Azure a přejděte do **přehled** stránku vaší webové aplikace. Vyberte **restartujte** tlačítka na stránce. Když se zobrazí místní okno, vyberte **Ano** k potvrzení. Potom můžete procházet vaší webové aplikaci, jak je vidět tady:

![Procházení .NET Core aplikace nasazené do služby Azure App Service v systému Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Další kroky
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
