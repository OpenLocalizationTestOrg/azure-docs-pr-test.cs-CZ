---
title: "aaaCreate technologie ASP.NET webové aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak toorun webové aplikace v Azure App Service nasazením hello výchozí ASP.NET webové aplikace."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: eec916b3c32b6c8b68083177938c5c822a9782b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a>Vytvoření webové aplikace ASP.NET v Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento rychlý start ukazuje, jak toodeploy vaše první ASP.NET webové aplikace tooAzure webové aplikace. Po dokončení kurzu budete mít skupinu prostředků, která se bude skládat z plánu služby App Service a webové aplikace Azure s nasazenou webovou aplikací.

Podívejte se na hello video toosee tento rychlý start v akci a potom postupujte podle hello kroky sami toopublish vaší první aplikace .NET v Azure.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

* Nainstalujte [Visual Studio 2017](https://www.visualstudio.com/downloads/) s hello následující úlohy:
    - **Vývoj pro ASP.NET a web**
    - **Azure – vývoj**

    ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Vytvoření webové aplikace ASP.NET

Ve Visual Studiu vytvořte projekt tak, že vyberete **Soubor > Nový > Projekt**. 

V hello **nový projekt** dialogovém okně, vyberte **Visual C# > Web > webové aplikace ASP.NET (rozhraní .NET Framework)**.

Název aplikace hello _myFirstAzureWebApp_a potom vyberte **OK**.
   
![Dialogové okno Nový projekt](./media/app-service-web-get-started-dotnet/new-project.png)

Můžete nasadit libovolného typu tooAzure aplikace ASP.NET web. Pro tento rychlý start, vyberte hello **MVC** šablony a zajistěte, aby ověřování je nastaven příliš**bez ověřování**.
      
Vyberte **OK**.

![Dialogové okno Nový projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

Hello nabídce vyberte **ladění > Spustit bez ladění** toorun hello webovou aplikaci místně.

![Místní spuštění aplikace](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-tooazure"></a>Publikování tooAzure

V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **myFirstAzureWebApp** projektu a vyberte **publikovat**.

![Publikování z Průzkumníka řešení](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Zkontrolujte, že je vybrána možnost **Microsoft Azure App Service** a vyberte **Publikovat**.

![Publikování ze stránky přehledu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Tím se otevře hello **vytvořit službu App Service** dialog, který vám pomůže vytvořit všechny hello potřebné prostředky Azure toorun hello webové aplikace ASP.NET v Azure.

## <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

V hello **vytvořit službu App Service** dialogovém okně, vyberte **přidat účet**a přihlaste se tooyour předplatného Azure. Pokud už jste přihlášení, vyberte hello účet obsahující hello požadované předplatné z rozevíracího seznamu hello.

> [!NOTE]
> Pokud už jste přihlášení, nevybírejte zatím možnost **Vytvořit**.
>
>
   
![Přihlaste se tooAzure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Další příliš**skupiny prostředků**, vyberte **nový**.

Název skupiny prostředků hello **myResourceGroup** a vyberte **OK**.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Další příliš**plán služby App Service**, vyberte **nový**. 

V hello **nakonfigurovat plán služby App Service** dialogové okno, použít nastavení hello v tabulce hello následující snímek obrazovky hello.

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Nastavení | Navrhovaná hodnota | Popis |
|-|-|-|
|Plán služby App Service| myAppServicePlan | Název hello plán služby App Service. |
| Umístění | Západní Evropa | datacentrum Hello je hostitelem hello webové aplikace. |
| Velikost | Free | [Cenová úroveň](https://azure.microsoft.com/pricing/details/app-service/) určuje funkce hostování. |

Vyberte **OK**.

## <a name="create-and-publish-hello-web-app"></a>Vytvoření a publikování webové aplikace hello

V **název webové aplikace**, zadejte název jedinečné aplikace (platnými znaky jsou `a-z`, `0-9`, a `-`), nebo přijměte hello automaticky generuje jedinečný název. Adresa URL Hello hello webové aplikace je `http://<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace.

Vyberte **vytvořit** toostart hello vytváření prostředků Azure.

![Nastavení názvu webové aplikace](./media/app-service-web-get-started-dotnet/web-app-name.png)

Po dokončení Průvodce hello, tato možnost publikuje hello ASP.NET webové aplikace tooAzure a pak spustí hello aplikace v hello výchozí prohlížeč.

![Publikovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

Název webové aplikace Hello zadaný v hello [vytvoření a publikování krok](#create-and-publish-the-web-app) slouží jako hello předponu adresy URL ve formátu hello `http://<app_name>.azurewebsites.net`.

Blahopřejeme, vaše webová aplikace ASP.NET je veřejně spuštěná ve službě Azure App Service.

## <a name="update-hello-app-and-redeploy"></a>Aktualizace aplikace hello a znovu ho zaveďte

Z hello **Průzkumníku řešení**, otevřete _Views\Home\Index.cshtml_.

Najde hello `<div class="jumbotron">` HTML značky v horní hello a nahraďte celý elementu hello hello následující kód:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how toodeploy a .NET app tooAzure App Service.</p>
</div>
```

tooredeploy tooAzure, klikněte pravým tlačítkem na hello **myFirstAzureWebApp** projektu v **Průzkumníku řešení** a vyberte **publikovat**.

V hello stránky publikování, vyberte **publikovat**.

Po dokončení publikování aplikace Visual Studio spustí adresu URL prohlížeče toohello hello webové aplikace.

![Aktualizovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-hello-azure-web-app"></a>Spravovat hello webové aplikace Azure

Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webové aplikace.

Hello levé nabídce vyberte **App Services**a pak vyberte název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-dotnet/access-portal.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [ASP.NET s databází SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)
