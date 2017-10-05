---
title: "Vytvoření webové aplikace ASP.NET Core v kódu Visual Studio"
description: "Tento kurz ukazuje, jak vytvořit webovou aplikaci ASP.NET Core pomocí Visual Studio Code."
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
ms.openlocfilehash: 46e3852dc84265de41bb358f482dec06608e7efa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Vytvoření webové aplikace ASP.NET Core v kódu Visual Studio
## <a name="overview"></a>Přehled
V tomto kurzu se dozvíte, jak vytvořit ASP.NET Core webovou aplikaci pomocí [Visual Studio Code (kód VS)](http://code.visualstudio.com//Docs/whyvscode) a nasadit ji [Azure App Service](../app-service/app-service-value-prop-what-is.md). 

> [!NOTE]
> I když se tento článek týká webových aplikací, platí také pro aplikace API a mobilní aplikace. 
> 
> 

ASP.NET Core je důležité změnit některé technologie ASP.NET. ASP.NET Core je nové open source a napříč platformami architektura pro vytváření webových moderní cloudové aplikace pomocí rozhraní .NET. Další informace najdete v tématu [Úvod do ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html). Informace o službě Azure App Service webových aplikací najdete v tématu [webových aplikací – přehled](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Požadavky
* Nainstalujte [VS Code](http://code.visualstudio.com/Docs/setup).
* Nainstalovat Git – můžete ho nainstalovat pomocí kteréhokoli z těchto umístění: [Chocolatey](https://chocolatey.org/packages/git) nebo [git scm.com](http://git-scm.com/downloads). Pokud jste ještě Git, vyberte možnost [git scm.com](http://git-scm.com/downloads) a vyberte možnost **použití Git z příkazového řádku Windows**. Po instalaci Git také budete muset nastavit Git uživatelské jméno a e-mailu, to je nutné později v tomto kurzu (při provádění potvrzení z VS Code).  

## <a name="install-aspnet-core"></a>Instalace technologie ASP.NET Core
ASP.NET Core je Štíhlá zásobníku .NET pro tvorbu moderní cloudové a webové aplikace, které běží na OS X, Linux a Windows. Byla postavena od základů zajistit představuje optimalizované vývoj rozhraní pro aplikace, které jsou nasazeny v cloudu nebo spustit místně. Se skládá z modulární komponenty s minimální režií, takže zachovat flexibilitu při vytváření řešení.

V tomto kurzu je navržená tak, abyste mohli začít vytváření aplikací s nejnovější verze vývoj ASP.NET Core. Následující pokyny jsou specifické pro systém Windows. Pokyny k instalaci na OS X, Linux a Windows, najdete v části [Začínáme s ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Podrobné pokyny k instalaci pro OS X, Linux a Windows, najdete v části [instalaci ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx). 
> 
> 

## <a name="create-the-web-app"></a>Vytvoření webové aplikace
V této části se dozvíte, jak chcete vygenerovat novou aplikaci ASP.NET webovou aplikaci pomocí nástroje příkazového řádku .NET. 

1. Zadejte na příkazovém řádku k vytvoření složky projektu a vygenerovat uživatelské rozhraní aplikace.
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![rozhraní příkazového řádku - ASP.NET Core generátor DotNet.](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. Chcete-li obnovit potřebné balíčky NuGet, spusťte následující příkaz:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a>Místní spuštění webové aplikace
Teď, když jste vytvořili webovou aplikaci a načíst všechny balíčky NuGet pro aplikaci, můžete spustit webovou aplikaci místně.

1. Spuštění aplikace ( `dotnet run` příkaz bude sestavení aplikace, když je zastaralý):
    ```terminal
    dotnet run
    ```
2. Otevřete prohlížeč a přejděte na následující adresu URL.
   
    **http://localhost: 5000**
   
    Takto se zobrazí stránka výchozí webové aplikace.
   
    ![Místní webové aplikace v prohlížeči](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Zavřete webový prohlížeč. V **příkazové okno**, stiskněte klávesu **Ctrl + C** vypnutí aplikace a zavřít **příkazové okno**. 

## <a name="create-a-web-app-in-the-azure-portal"></a>Vytvořit webovou aplikaci na portálu Azure
Následující postup vás provede vytvořením webové aplikace na portálu Azure.

1. Přihlaste se k [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **nový** levé v horní části portálu.
3. Klikněte na tlačítko **webové aplikace > Webová aplikace**.
   
    ![Azure nové webové aplikace](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Zadejte hodnotu pro **název**, jako například **SampleWebAppDemo**. Všimněte si, že tento název musí být jedinečný a portálu vynutí, při pokusu o zadání názvu. Proto pokud vyberete, zadejte jinou hodnotu, budete muset nahraďte tuto hodnotu pro každý výskyt **SampleWebAppDemo** , které vidíte v tomto kurzu. 
5. Vyberte existující **plán služby App Service** nebo vytvořte novou. Pokud vytvoříte nový plán, vyberte cenovou úroveň, umístění a další možnosti. Další informace o plánech služby App Service najdete v článku [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Azure nové okno webové aplikace](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Klikněte na možnost **Vytvořit**.
   
    ![okně webové aplikace](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Povolení publikování Git pro nové webové aplikace
Git je systém správy distribuovaných verzí, který můžete použít k nasazení webové aplikace služby Azure App Service. Kód napsaný pro webovou aplikaci uložíte do místního úložiště Git a kód budete nasazovat do Azure nuceným doručením (push) do vzdáleného úložiště.   

1. Přihlaste se [portál Azure](https://portal.azure.com).
2. Klikněte na **Browse** (Procházet).
3. Klikněte na tlačítko **webové aplikace** zobrazíte seznam webové aplikace přidružené k předplatnému Azure.
4. Vyberte webovou aplikaci, kterou jste vytvořili v tomto kurzu.
5. V okně webové aplikace klikněte na **nastavení** > **průběžné nasazování**. 
   
    ![Službě Azure web app hostitele](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Klikněte na tlačítko **zvolit zdroj > místní úložiště Git**.
7. Klikněte na **OK**.
   
    ![Azure místní úložiště Git](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Pokud jste dříve vytvořili přihlašovací údaje nasazení pro publikování webové aplikace nebo jiná aplikace služby App Service, nastavte je nyní:
   
   * Klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení**. **Nastavit přihlašovací údaje nasazení** zobrazí se okno.
   * Vytvořte uživatelské jméno a heslo.  Toto heslo budete potřebovat později při nastavování Git.
   * Klikněte na **Uložit**.
9. V okně vaší webové aplikace, klikněte na tlačítko **Nastavení > vlastnosti**. Vzdálené úložiště Git, které budete nasazovat na adresu URL se zobrazí v části **adresy URL pro GIT**.
10. Kopírování **adresy URL pro GIT** hodnoty pro pozdější použití v tomto kurzu.
    
    ![Adresa URL Azure Git](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Publikování webové aplikace do služby Azure App Service
V této části vytvoříte místní úložiště Git a posílejte nabízená oznámení z tohoto úložiště do Azure k nasazení vaší webové aplikace do Azure.

1. V produktu VS Code, vyberte **Git** možnost v levém navigačním panelu.
   
    ![Ikona Git v produktu VS Code](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Vyberte **úložiště git inicializovat** a ujistěte se, pracovní prostor je pod git zdrojového kódu. 
   
    ![Inicializace Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Otevřete příkazové okno a přejděte do adresáře vaší webové aplikace. Potom zadejte následující příkaz:
   
        git config core.autocrlf false
   
    Tento příkaz zabrání problém o textu, které se podílejí Line FEED zakončení a LF zakončení.
4. V produktu VS Code přidat zprávu o potvrzení a klikněte na **potvrzení všechny** ikona zaškrtnutí.
   
    ![Git potvrdit všechny](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. Po dokončení zpracování Git, se zobrazí, že neexistují žádné soubory uvedené v okně Git v části **změny**. 
   
    ![Git žádné změny](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Změnit zpátky na příkazové okno kde příkazovém řádku odkazuje na adresář, kde se nachází vaší webové aplikace.
7. Vytvořte vzdálený odkaz pro vkládání aktualizací do webové aplikace pomocí adresy URL Git (ukončuje v ".git"), který jste zkopírovali dříve.
   
        git remote add azure [URL for remote repository]
8. Nakonfigurujte si Git svoje přihlašovací údaje uložit místně, tak, aby se automaticky připojí se k vaší nabízené příkazy vygenerovat z VS Code.
   
        git config credential.helper store
9. Doručte změny do Azure tak, že zadáte následující příkaz. Po této úvodní nabízená instalace Azure budete moci provádět všechny příkazy nabízené z VS Code. 
   
        git push -u azure master
   
    Zobrazí se výzva k zadání hesla, které jste vytvořili dříve v Azure. **Poznámka: Hesla nezobrazí.**
   
    Výstup z výše uvedeném příkazu končí zprávu, že nasazení je úspěšné.
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Pokud provedete změny aplikace, můžete znovu publikovat přímo v produktu VS Code pomocí integrované funkce Git tak, že vyberete **potvrdit všechny** následovaný **Push** možnost. Zjistíte, **Push** možnost k dispozici v rozevírací nabídce vedle položky **potvrdit všechny** a **aktualizovat** tlačítka.
> 
> 

Pokud budete potřebovat spolupracovat na projektu, měli byste zvážit, když zavedete Githubu mezi vkládání do Azure.

## <a name="run-the-app-in-azure"></a>Spusťte aplikaci v Azure
Teď, když jste nasadili vaší webové aplikace, umožňuje spuštění aplikace při hostované v Azure. 

Tento krok můžete provést dvěma způsoby:

* Otevřete prohlížeč a zadejte název webové aplikace následujícím způsobem.   
  
        http://SampleWebAppDemo.azurewebsites.net
* Na portálu Azure, vyhledejte okně webové aplikace pro vaši webovou aplikaci a klikněte na tlačítko **Procházet** zobrazení vaší aplikace 
* ve výchozím prohlížeči.

![Webové aplikace Azure](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Souhrn
V tomto kurzu jste zjistili, jak vytvořit webovou aplikaci v produktu VS Code a nasadíte ho do Azure. Další informace o VS Code, najdete v článku [proč Visual Studio Code?](https://code.visualstudio.com/Docs/) Informace o webové aplikace služby App Service najdete v tématu [webových aplikací – přehled](app-service-web-overview.md). 

