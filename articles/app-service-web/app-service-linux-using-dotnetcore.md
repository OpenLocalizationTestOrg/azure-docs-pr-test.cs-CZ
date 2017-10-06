---
title: "aaaUse .NET Core ve webové aplikaci v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 9b7fb7185dff2c99ed88e7937d455177504937b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a>Použití .NET Core v Azure webové aplikace v systému Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

[Webová aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) v systému Linux poskytuje službu webové vysoce škálovatelnou a automatických oprav hostingu pomocí operační systém Linux hello. Tento kurz obsahuje podrobné pokyny, které se zobrazuje jak toocreate [.NET Core](https://docs.microsoft.com/aspnet/core/) aplikace na Azure webové aplikace v systému Linux. 

![Webová aplikace v Linuxu][10]

Můžete provést kroky hello níže používání počítačů Mac, Windows nebo Linux.

## <a name="prerequisites"></a>Požadavky ##

toocomplete v tomto kurzu: 

* Nainstalujte hello [.NET Core SDK](https://www.microsoft.com/net/download/core).
* Nainstalujte [Git](https://git-scm.com/downloads).

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a>Vytvořit místní aplikace .NET Core ##

Spusťte novou relaci terminálu. Vytvořte adresář s názvem `hellodotnetcore`a změňte aktuální adresář tooit hello. Poté zadejte hello následující: 

```
dotnet new web
``` 

  Tento příkaz vytvoří tři soubory (*hellodotnetcore.csproj*, *Program.cs*, a *Startup.cs*) a jeden prázdnou složku (*wwwroot /*) v aktuálním adresáři hello. Hello obsah `.csproj` soubor by měl vypadat jako následující hello: 

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

Vzhledem k tomu, že je tato aplikace k webové aplikaci, odkaz tooan ASP.NET Core balíček je automaticky přidán toohello *hellodotnetcore.csproj* souboru. číslo verze Hello hello balíčku je nastavena podle toohello vybrali framework. V tomto příkladu odkazuje ASP.NET Core verze 1.1.2, protože se používá .NET Core 1.1.

## <a name="build-and-test-hello-application-locally"></a>Vytvoření a testování aplikace hello místně ##

Můžete sestavit a spustit aplikaci .NET Core s hello `dotnet restore` příkazu s parametrem hello `dotnet run` příkaz, jak je vidět tady:

```
dotnet restore
dotnet run
```


Při spuštění aplikace hello, zobrazí se zpráva s oznámením, že aplikace hello naslouchá tooincoming žádosti na port. 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C tooshut down.
```

Testování procházením příliš`http://localhost:5000/` s prohlížeči. Pokud se vše funguje bez problémů, se zobrazí "Hello, World!" jako text hello výsledek.

![Testování pomocí prohlížeče][7]

## <a name="create-a-net-core-app-in-hello-azure-portal"></a>Vytvořit aplikaci .NET Core hello portálu Azure ##

Je třeba nejprve toocreate prázdnou webovou aplikaci. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vytvořte novou [webové aplikace v systému Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).

![Vytváření webové aplikace][1]

Když hello **vytvořit** otevře se stránka, obsahují podrobné informace o vaší webové aplikace:

![Výběr .NET Core runtime zásobníku][2]

Použití hello následující tabulky jako průvodce toofill out hello **vytvořit** stránky a pak vyberte **OK** a **vytvořit** toocreate hello aplikace.

| Nastavení      | Navrhovaná hodnota  | Popis                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| Název aplikace. | hellodotnetcore  | Hello název vaší aplikace. Tento název musí být jedinečný. |
| Předplatné | Vyberte stávající předplatné | Hello předplatného Azure. |
| Skupina prostředků | myResourceGroup |  Hello prostředků Azure skupiny toocontain hello webové aplikace. |
| Plán služby App Service | Název existující plán služby App Service |  Hello plán služby App Service.  |
| Konfiguraci kontejneru | .NET core 1.1 | typ kontejneru pro tuto webovou aplikaci Hello: integrované, Docker nebo privátní registru. |
| Zdroj bitové kopie  | Integrované  |  Hello zdroj obrázku hello. |
| Modul runtime zásobníku  | .NET core 1.1  | Hello runtime zásobníku a verze.  |

## <a name="deploy-your-application-via-git"></a>Nasazení aplikace prostřednictvím Git ##

V systému Linux pomocí Git toodeploy hello .NET Core aplikace tooAzure webové aplikace App Service.

Hello novou webovou aplikaci Azure je již nakonfigurován nasazení Git. Adresa URL nasazení Git hello zjistíte tak, že přejdete toohello následující adresu URL po vložení název vaší webové aplikace:

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

Hello adresy URL pro Git má hello následující formulář založený na název vaší webové aplikace:

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

Spusťte následující příkazy toodeploy hello místních aplikací tooyour webové aplikace Azure hello: 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

Nepotřebujete toopush všechny soubory pod *bin /* nebo *obj /* adresáře aplikace je integrovaná v cloudu hello při hello aplikace zdrojové soubory jsou nabídnutých tooAzure. Po dokončení procesu sestavení hello binární soubory se zkopírují do adresáře aplikace hello v */home/server/wwwroot/*.

Potvrďte, že operace vzdálené nasazení hello sestavy úspěch. Nabízená operace může trvat od balíčku řešení a proces spuštění v cloudu hello sestavení. Zobrazí se několik stavové zprávy, včetně těch, které jsou s oznámením, že soubory byly zkopírovány. výstup Hello by měl vypadat podobně jako toohello následující:

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
toohttps://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

Po dokončení nasazení hello restartování webové aplikace pro hello nasazení tootake vliv. toodo, přejděte toohello portál Azure a přejděte toohello **přehled** stránku vaší webové aplikace. Vyberte hello **restartujte** tlačítka na stránku hello. Když se zobrazí místní okno, vyberte **Ano** tooconfirm. Potom můžete procházet vaší webové aplikaci, jak je vidět tady:

![Procházení .NET Core aplikace nasazená tooAzure služby App Service v systému Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Další kroky
* [Webové aplikace Azure App Service v systému Linux – nejčastější dotazy](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
