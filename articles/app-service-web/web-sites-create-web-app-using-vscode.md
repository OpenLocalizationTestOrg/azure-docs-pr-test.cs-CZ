---
title: "aaaCreate webové aplikace ASP.NET Core v kódu Visual Studio"
description: "Tento kurz ukazuje, jak toocreate ASP.NET Core webové aplikaci pomocí Visual Studio Code."
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.openlocfilehash: 1c18c94984d71e88d2a5b792d68cb1c81e4a96d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Vytvoření webové aplikace ASP.NET Core v kódu Visual Studio
## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak toocreate ASP.NET Core webové aplikace pomocí [Visual Studio Code (kód VS)](http://code.visualstudio.com//Docs/whyvscode) a nasaďte ho příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md). 

> [!NOTE]
> Přestože tento článek se týká tooweb aplikací, platí také tooAPI a mobilní aplikace. 
> 
> 

ASP.NET Core je důležité změnit některé technologie ASP.NET. ASP.NET Core je nové open source a napříč platformami architektura pro vytváření webových moderní cloudové aplikace pomocí rozhraní .NET. Další informace najdete v tématu [Úvod tooASP.NET základní](http://docs.asp.net/latest/conceptual-overview/aspnet.html). Informace o službě Azure App Service webových aplikací najdete v tématu [webových aplikací – přehled](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Požadavky
* Nainstalujte [VS Code](http://code.visualstudio.com/Docs/setup).
* Nainstalovat Git – můžete ho nainstalovat pomocí kteréhokoli z těchto umístění: [Chocolatey](https://chocolatey.org/packages/git) nebo [git scm.com](http://git-scm.com/downloads). Pokud jste nový tooGit, vyberte možnost [git scm.com](http://git-scm.com/downloads) a vyberte možnost hello příliš**použití Git z příkazového řádku Windows hello**. Po instalaci Git, budete také potřebovat tooset hello Git uživatelské jméno a e-mailu to je nutné později v kurzu hello (při provádění potvrzení z VS Code).  

## <a name="install-aspnet-core"></a>Instalace technologie ASP.NET Core
ASP.NET Core je Štíhlá zásobníku .NET pro tvorbu moderní cloudové a webové aplikace, které běží na OS X, Linux a Windows. Byla postavena z hello pozadí až tooprovide rozhraní pro vývoj optimalizované pro aplikace, které jsou buď nasazené toohello cloudové nebo místní spouštění. Se skládá z modulární komponenty s minimální režií, takže zachovat flexibilitu při vytváření řešení.

Tento kurz je určen navrženou tooget zahájeno vytváření aplikací s hello nejnovější vývoj verze ASP.NET Core. Hello následující pokyny jsou konkrétní tooWindows. Pokyny k instalaci na OS X, Linux a Windows, najdete v části [Začínáme s ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Podrobné pokyny k instalaci pro OS X, Linux a Windows, najdete v části [instalaci ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx). 
> 
> 

## <a name="create-hello-web-app"></a>Vytvoření webové aplikace hello
Tato část uvádí, jak tooscaffold novou aplikaci ASP.NET webové aplikace pomocí nástroje příkazového řádku .NET hello. 

1. Zadejte následující hello v hello příkazového řádku toocreate hello projektu složky a vygenerované uživatelské rozhraní hello aplikace.
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![rozhraní příkazového řádku - ASP.NET Core generátor DotNet.](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. toorestore hello potřebné balíčky NuGet, spusťte následující příkaz hello:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-hello-web-app-locally"></a>Místní spuštění webové aplikace hello
Teď, když jste vytvořili hello webové aplikace a načíst všechny balíčky NuGet hello hello aplikace, můžete spustit hello webovou aplikaci místně.

1. Spuštění aplikace hello (hello `dotnet run` příkaz bude sestavení aplikace hello, pokud je zastaralý):
    ```terminal
    dotnet run
    ```
2. Otevřete prohlížeč a přejděte toohello následující adresu URL.
   
    **http://localhost: 5000**
   
    takto se zobrazí stránka výchozí Hello hello webové aplikace.
   
    ![Místní webové aplikace v prohlížeči](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Zavřete webový prohlížeč. V hello **příkazové okno**, stiskněte klávesu **Ctrl + C** tooshut dolů hello aplikace a zavřít hello **příkazové okno**. 

## <a name="create-a-web-app-in-hello-azure-portal"></a>Vytvoření webové aplikace v hello portálu Azure
Hello následující postup vás provede vytvořením webové aplikace v hello portálu Azure.

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com).
2. Klikněte na tlačítko **nový** v hello levém horním rohu hello portálu.
3. Klikněte na tlačítko **webové aplikace > Webová aplikace**.
   
    ![Azure nové webové aplikace](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Zadejte hodnotu pro **název**, jako například **SampleWebAppDemo**. Upozorňujeme, že tento název musí toobe jedinečný a hello portál vynutí, který chcete-li název hello tooenter. Proto pokud vyberete, zadejte jinou hodnotu, budete potřebovat toosubstitute, která hodnotu pro každý výskyt **SampleWebAppDemo** , které vidíte v tomto kurzu. 
5. Vyberte existující **plán služby App Service** nebo vytvořte novou. Pokud vytvoříte nový plán, vyberte hello cenová úroveň, umístění a další možnosti. Další informace o plánech služby App Service najdete v článku hello, [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Azure nové okno webové aplikace](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Klikněte na možnost **Vytvořit**.
   
    ![okně webové aplikace](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-hello-new-web-app"></a>Povolení publikování Git pro hello nové webové aplikace
Git je distribuovaný systém správy verzí, které můžete použít toodeploy webové aplikace služby Azure App Service. Budete ukládat hello kód napsaný pro webovou aplikaci v místní úložiště Git a tooAzure váš kód budete nasazovat vynucením tooa vzdáleného úložiště.   

1. Přihlaste se k hello [portálu Azure](https://portal.azure.com).
2. Klikněte na **Browse** (Procházet).
3. Klikněte na tlačítko **webové aplikace** tooview seznam hello webové aplikace, která je spojená s předplatným Azure.
4. Vyberte hello webovou aplikaci, kterou jste vytvořili v tomto kurzu.
5. V okně webové aplikace hello, klikněte na **nastavení** > **průběžné nasazování**. 
   
    ![Službě Azure web app hostitele](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Klikněte na tlačítko **zvolit zdroj > místní úložiště Git**.
7. Klikněte na **OK**.
   
    ![Azure místní úložiště Git](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Pokud jste dříve vytvořili přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service, nastavte je nyní:
   
   * Klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení**. Hello **nastavit přihlašovací údaje nasazení** zobrazí se okno.
   * Vytvořte uživatelské jméno a heslo.  Toto heslo budete potřebovat později při nastavování Git.
   * Klikněte na **Uložit**.
9. V okně vaší webové aplikace, klikněte na tlačítko **Nastavení > vlastnosti**. Adresa URL Hello hello vzdáleného úložiště Git, které budete nasazovat toois v části **adresy URL pro GIT**.
10. Kopírování hello **adresy URL pro GIT** hodnoty pro pozdější použití v kurzu hello.
    
    ![Adresa URL Azure Git](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-tooazure-app-service"></a>Publikování tooAzure vaší webové aplikace služby App Service
V této části vytvoříte místní úložiště Git a nabízet z tohoto úložiště tooAzure toodeploy tooAzure vaší webové aplikace.

1. V produktu VS Code, vyberte hello **Git** možnost hello levém navigačním panelu.
   
    ![Ikona Git v produktu VS Code](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Vyberte **úložiště git inicializovat** toomake, že pracovní prostor je pod git zdrojového kódu. 
   
    ![Inicializace Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Otevřete příkazové okno hello a změnit adresář toohello adresáře vaší webové aplikace. Potom zadejte hello následující příkaz:
   
        git config core.autocrlf false
   
    Tento příkaz zabrání problém o textu, které se podílejí Line FEED zakončení a LF zakončení.
4. V produktu VS Code přidat zprávu o potvrzení a klikněte na hello **potvrzení všechny** ikona zaškrtnutí.
   
    ![Git potvrdit všechny](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. Po dokončení zpracování Git, se zobrazí, že nejsou žádné soubory uvedené v okně Git hello pod **změny**. 
   
    ![Git žádné změny](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Změňte back toohello příkazové okno kde hello příkazového řádku body toohello adresáře, kde se nachází vaší webové aplikace.
7. Vytvořte vzdálený odkaz pro vkládání aktualizace tooyour webové aplikace pomocí hello URL Git (ukončuje v ".git"), který jste zkopírovali dříve.
   
        git remote add azure [URL for remote repository]
8. Nakonfigurujte Git toosave vaše přihlašovací údaje místně, takže budou automaticky připojením tooyour nabízené příkazy vygenerovat z VS Code.
   
        git config credential.helper store
9. Vaše změny tooAzure push tak, že zadáte následující příkaz hello. Po této tooAzure počáteční nabízené budete moct toodo všechny nabízené hello příkazy z kódu VS. 
   
        git push -u azure master
   
    Zobrazí se výzva k hello hesla, kterou jste vytvořili dříve v Azure. **Poznámka: Hesla nezobrazí.**
   
    výstup Hello hello výše příkaz končí zprávu, že nasazení je úspěšné.
   
        remote: Deployment successful.
        toohttps://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Pokud provedete změny tooyour aplikace, můžete znovu publikovat přímo v produktu VS Code pomocí integrované funkce Git hello výběrem hello **potvrdit všechny** následovaný hello **Push** možnost. Zjistíte, hello **Push** možnost k dispozici v další toohello hello rozevírací nabídky **potvrdit všechny** a **aktualizovat** tlačítka.
> 
> 

Pokud potřebujete toocollaborate na projektu, byste měli zvážit, když zavedete tooGitHub mezi vkládání tooAzure.

## <a name="run-hello-app-in-azure"></a>Spuštění aplikace hello v Azure
Teď, když jste nasadili vaší webové aplikace, umožňuje spuštění aplikace hello při hostované v Azure. 

Tento krok můžete provést dvěma způsoby:

* Otevřete prohlížeč a zadejte název webové aplikace hello následujícím způsobem.   
  
        http://SampleWebAppDemo.azurewebsites.net
* V hello portálu Azure, vyhledejte hello webové aplikace okno pro vaši webovou aplikaci a klikněte na tlačítko **Procházet** tooview vaší aplikace 
* ve výchozím prohlížeči.

![Webové aplikace Azure](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Souhrn
V tomto kurzu jste se naučili jak toocreate webové aplikace v produktu VS Code a nasaďte ho tooAzure. Další informace o VS Code, najdete v článku hello, [proč Visual Studio Code?](https://code.visualstudio.com/Docs/) Informace o webové aplikace služby App Service najdete v tématu [webových aplikací – přehled](app-service-web-overview.md). 

