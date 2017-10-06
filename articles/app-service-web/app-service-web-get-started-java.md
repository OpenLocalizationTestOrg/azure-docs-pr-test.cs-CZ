---
title: "aaaCreate první webové aplikace Java v Azure"
description: "Zjistěte, jak toorun webové aplikace ve službě App Service pomocí nasazení základní aplikace v jazyce Java."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.openlocfilehash: 81315c07b5aa84cbec50a17b2cb3914927b19c00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a>Vytvoření první webové Java aplikace ve službě Azure

Hello [webové aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) funkce [Azure App Service](../app-service/app-service-value-prop-what-is.md) nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento rychlý start ukazuje, jak toodeploy Java webové aplikace tooApp služby pomocí hello [IDE Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/).

![Hello Azure! Ukázková webová aplikace](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý start, nainstalovat:

* Hello volné [IDE Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/downloads/). Tento kurz Rychlý start používá Eclipse Neon.
* Hello [nástrojů Azure pro Eclipse](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Vytvoření dynamického webového projektu v Eclipse

V prostředí Eclipse vyberte **File** (Soubor) > **New** (Nový) > **Dynamic Web Project** (Dynamický webový projekt).

V hello **nové Dynamic Web Project** dialogové okno, název projektu hello **MyFirstJavaOnAzureWebApp**a vyberte **Dokončit**.
   
![Dialogové okno New Dynamic Web Project (Nový dynamický webový projekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>Přidání stránky JSP

Pokud se nezobrazí Project Explorer (Prohlížeč projektu), obnovte zobrazení.

![Pracovní prostor Java EE pro Eclipse](./media/app-service-web-get-started-java/pe.png)

V prohlížeči projektu rozbalte hello **MyFirstJavaOnAzureWebApp** projektu.
Klikněte pravým tlačítkem na **WebContent** (Webový obsah) a potom vyberte **New** (Nový) > **JSP File** (Soubor JSP).

![Nabídka pro nový soubor JSP v prohlížeči Project Explorer](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

V hello **nový soubor JSP** dialogové okno:

* Název souboru hello **index.jsp**.
* Vyberte **Finish** (Dokončit).

  ![Dialogové okno New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

V souboru index.jsp hello nahraďte hello `<body></body>` element s hello následující kód:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Uložte změny hello.

## <a name="publish-hello-web-app-tooazure"></a>Publikování hello webové aplikace tooAzure

V prohlížeči projektu klikněte pravým tlačítkem na projekt hello a pak vyberte **Azure** > **publikovat jako webové aplikace Azure**.

![Místní nabídka Publish as Azure Web App (Publikovat jako webovou aplikaci Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

V hello **přihlásit k Azure** dialogové okno, udržování hello **interaktivní** a pak vyberte možnost **přihlášení**.

Podle pokynů hello přihlášení.

### <a name="deploy-web-app-dialog-box"></a>Dialogové okno Deploy Web App (Nasazení webové aplikace)

Po přihlášení tooyour účet Azure hello **nasadit webovou aplikaci** zobrazí se dialogové okno.

Vyberte **Vytvořit**.

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>Dialogové okno Create App Service (Vytvoření služby App Service)

Hello **vytvořit službu App Service** dialogové okno s výchozími hodnotami. Hello číslo **170602185241** ukazuje hello následující bitové kopie se ve vašem dialogovém liší.

![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/cas1.png)

V hello **vytvořit službu App Service** dialogové okno:

* Zachovejte hello vygeneruje název hello webové aplikace. Tento název musí být v rámci služby Azure jedinečný. Název Hello je součástí hello adresu URL pro webovou aplikaci hello. Například: Pokud je název webové aplikace hello **MyJavaWebApp**, adresa URL je hello *myjavawebapp.azurewebsites.net*.
* Zachovat hello výchozí webový kontejner.
* Vyberte předplatné služby Azure.
* Na hello **plán služby App service** karty:

  * **Vytvořit nový**: zachovat hello výchozí, což je název hello hello plán služby App Service.
  * **Location** (Umístění): Vyberte **West Europe** (Západní Evropa) nebo umístění ve vaší blízkosti.
  * **Cenová úroveň**: Vyberte hello bezplatnou možností. Informace o funkcích najdete na stránce [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).

   ![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Karta Resource group (Skupina prostředků)

Vyberte hello **skupiny prostředků** kartě. Zachovat hello výchozí generovaný hodnotu pro skupinu prostředků hello.

![Karta Resource group (Skupina prostředků)](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Vyberte **Vytvořit**.

<!--
### hello JDK tab

Select hello **JDK** tab. Keep hello default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Hello nástrojů Azure vytvoří hello webovou aplikaci a zobrazí dialogové okno průběhu.

![Dialogové okno Create App Service Progress (Průběh vytváření plánu služby App Service)](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Dialogové okno Deploy Web App (Nasazení webové aplikace)

V hello **nasadit webovou aplikaci** dialogové okno, vyberte **nasazení tooroot**. Pokud máte aplikační službu v *wingtiptoys.azurewebsites.net* a nenasazujte toohello kořenová hello webovou aplikaci s názvem **MyFirstJavaOnAzureWebApp** nasazuje příliš *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Hello dialogové okno pole ukazuje hello Azure, JDK a výběr webového kontejneru.

Vyberte **nasadit** toopublish hello webové aplikace tooAzure.

Po dokončení publikování hello vyberte hello **publikováno** odkaz v hello **protokol činnosti Azure** dialogové okno.

![Dialogové okno Protokol aktivit v Azure](./media/app-service-web-get-started-java/aal.png)

Blahopřejeme! Úspěšně jste nasadili tooAzure vaší webové aplikace. 

![Hello Azure! Ukázková webová aplikace](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-hello-web-app"></a>Aktualizace hello webové aplikace

Změnit hello ukázka JSP kód tooa jiná zpráva.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Uložte změny hello.

V prohlížeči projektu klikněte pravým tlačítkem na projekt hello a pak vyberte **Azure** > **publikovat jako webové aplikace Azure**.

Hello **nasadit webovou aplikaci** se zobrazí dialogové okno a ukazuje hello služby app service, kterou jste vytvořili. 

> [!NOTE]
> Vyberte **nasazení tooroot** pokaždé, když publikujete.
>

Vyberte hello webovou aplikaci a vyberte **nasadit**, který publikuje hello změny.

Když hello **publikování** odkaz se zobrazí, vyberte ho toobrowse toohello webové aplikace a podívejte se změny hello.

## <a name="manage-hello-web-app"></a>Správa webové aplikace hello

Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toosee hello webovou aplikaci, kterou jste vytvořili.

Hello levé nabídce vyberte **skupiny prostředků**.

![Skupiny tooresource portálu](media/app-service-web-get-started-java/rg.png)

Vyberte skupinu prostředků hello. Hello stránka zobrazuje hello prostředky, které jste vytvořili v tento rychlý start.

![Skupina prostředků myResourceGroup](media/app-service-web-get-started-java/rg2.png)

Vyberte hello webové aplikace (**webapp 170602193915** v hello předcházející bitové kopie).

Hello **přehled** se zobrazí stránka. Tato stránka poskytuje zobrazení jak je to aplikace hello. Tady můžete provádět základní úkoly správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Hello karty na levé straně stránky hello hello zobrazit hello různé konfigurace, které lze otevřít. 

![Stránka služby App Service na webu Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Mapování vlastní domény](app-service-web-tutorial-custom-domain.md)
