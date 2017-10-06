---
title: "aaaDeploy webové úlohy pomocí sady Visual Studio"
description: "Zjistěte, jak Azure WebJobs tooAzure toodeploy App Service Web Apps pomocí sady Visual Studio."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2016
ms.author: glenga
ms.openlocfilehash: 5fc5d9562e8836348f5ab6844fb6c23ff40a321c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a>Nasazení WebJobs přes Visual Studio
## <a name="overview"></a>Přehled
Toto téma vysvětluje, jak toouse Visual Studio toodeploy konzolovou aplikaci projektu tooa webové aplikace ve [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) jako [webové úlohy Azure](http://go.microsoft.com/fwlink/?LinkId=390226). Informace o tom, jak toodeploy webové úlohy pomocí hello [portálu Azure](https://portal.azure.com), najdete v části [úlohy na pozadí spustit s WebJobs](web-sites-create-web-jobs.md).

Když Visual Studio nasadí projekt aplikace s povolenými webové konzoly, provádí dvě úlohy:

* Soubory modulu runtime kopie toohello odpovídající složku v hello webové aplikace (*App_Data/úlohy/průběžné* pro nepřetržité webové úlohy *App_Data, úlohy nebo aktivaci* pro webové úlohy naplánované i na vyžádání).
* Nastaví [úlohy Azure Scheduler](#scheduler) pro webové úlohy, které jsou naplánované toorun v určitou dobu. (To není nutné pro nepřetržité webové úlohy).

Projekt webové úlohy povolené má hello následující tooit přidané položky:

* Hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) balíček NuGet.
* A [webové úlohy publikovat settings.json](#publishsettings) soubor, který obsahuje nastavení nasazení a plánovače. 

![Diagram zobrazující tooa konzolovou aplikaci tooenable nasazení co je přidána jako webová](./media/websites-dotnet-deploy-webjobs/convert.png)

Můžete přidat tyto položky tooan existující projekt konzolové aplikace nebo použít šablonu toocreate nový projekt aplikace s povolenými webové úlohy konzoly. 

Můžete nasadit projekt jako webová samostatně nebo ho tak, aby automaticky nasadí vždy, když je nasazení webového projektu hello odkaz tooa webového projektu. toolink projektů sady Visual Studio obsahuje název hello hello povolené webové úlohy projektu v [webjobs list.json](#webjobslist) souboru v hello webového projektu.

![Diagram zobrazující projektu úlohy WebJob propojení projektu tooweb](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Požadavky
Funkce nasazení webové úlohy jsou k dispozici v sadě Visual Studio, když instalujete hello Azure SDK pro .NET:

* [Azure SDK pro .NET (Visual Studio)](https://azure.microsoft.com/downloads/).

## <a id="convert"></a>Povolit nasazení webové úlohy pro existující projekt konzolové aplikace
Máte dvě možnosti:

* [Povolit automatické nasazení webového projektu](#convertlink).
  
    Existující projekt konzolové aplikace nakonfigurujte tak, aby se automaticky nasadí jako webová při nasazení webového projektu. Tuto možnost použijte, pokud chcete, aby toorun vaše webová úloha v hello stejné webové aplikace, ve kterém můžete spouštět hello související webové aplikace.
* [Povolit nasazení bez webového projektu](#convertnolink).
  
    Nakonfigurujte existující toodeploy projekt konzolové aplikace jako webová samostatně, žádný odkaz tooa webový projekt. Tuto možnost použijte, pokud chcete toorun webová ve webové aplikaci samostatně, s žádná webová aplikace spuštěna v hello webové aplikace. Můžete chtít toodo to v pořadí možné tooscale toobe prostředkům webové úlohy nezávisle na prostředkům webové aplikace.

### <a id="convertlink"></a>Povolit automatické nasazení webové úlohy s webovým projektem
1. Klikněte pravým tlačítkem na hello webového projektu v **Průzkumníku řešení**a potom klikněte na **přidat** > **stávající projekt jako webová úloha Azure**.
   
    ![Stávající projekt jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    Hello [přidat webové úlohy Azure](#configure) zobrazí se dialogové okno.
2. V hello **název projektu** rozevíracího seznamu, vyberte hello konzolové aplikace projektu tooadd jako webovou úlohu.
   
    ![Výběr projektu v dialogovém okně Přidat webové úlohy Azure](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Dokončení hello [přidat webové úlohy Azure](#configure) dialogové okno a potom klikněte na **OK**. 

### <a id="convertnolink"></a>Povolit nasazení webové úlohy bez webového projektu
1. Projekt konzolové aplikace hello klikněte pravým tlačítkem v **Průzkumníku řešení**a pak klikněte na tlačítko **publikovat jako webová úloha Azure...** . 
   
    ![Publikovat jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    Hello [přidat webové úlohy Azure](#configure) dialogové okno se zobrazí s hello projekt vybraný v hello **název projektu** pole.
2. Dokončení hello [přidat webové úlohy Azure](#configure) dialogové okno a pak klikněte na tlačítko **OK**.
   
   Hello **Publikovat Web** zobrazí se průvodce.  Pokud nechcete, aby toopublish okamžitě, zavřete průvodce hello. Hello nastavení, které jste zadali, se uloží pro být příliš[nasazení projektu hello](#deploy).

## <a id="create"></a>Vytvořte nový projekt webové úlohy povolena
toocreate nový projekt webové úlohy povolené, můžete použít hello konzolové aplikace projektu šablony a povolit webové úlohy nasazení jak je popsáno v [hello předchozí části](#convert). Jako alternativu můžete šablonu nový projekt webové úlohy hello:

* [Použít šablonu nový projekt webové úlohy hello pro nezávislé webové úlohy](#createnolink)
  
    Vytvoření projektu a nakonfigurujte ho toodeploy samostatně jako webová úloha se žádný odkaz tooa webový projekt. Tuto možnost použijte, pokud chcete toorun webová ve webové aplikaci samostatně, s žádná webová aplikace spuštěna v hello webové aplikace. Můžete chtít toodo to v pořadí možné tooscale toobe prostředkům webové úlohy nezávisle na prostředkům webové aplikace.
* [Použít šablonu nový projekt webové úlohy hello pro webové úlohy propojené tooa webového projektu](#createlink)
  
    Vytvořte projekt, který je nakonfigurovaný toodeploy automaticky jako webová úloha, když webového projektu v hello stejné řešení nasazeno. Tuto možnost použijte, pokud chcete, aby toorun vaše webová úloha v hello stejné webové aplikace, ve kterém můžete spouštět hello související webové aplikace.

> [!NOTE]
> Šablona Nový projekt webové úlohy Hello automaticky nainstaluje balíčky NuGet a obsahuje kód v *Program.cs* pro hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Pokud nechcete, aby toouse hello WebJobs SDK, odebrat ani změnit hello `host.RunAndBlock` příkaz v *Program.cs*.
> 
> 

### <a id="createnolink"></a>Použít šablonu nový projekt webové úlohy hello pro nezávislé webové úlohy
1. Klikněte na tlačítko **soubor** > **nový projekt**a potom v hello **nový projekt** dialogovém okně **cloudu**  >   **Webová úloha Azure (rozhraní .NET Framework)**.
   
    ![Dialogové okno Nový projekt zobrazující šablony webové úlohy](./media/websites-dotnet-deploy-webjobs/np.png)
2. Postupujte podle pokynů hello uvedena výše příliš[zkontrolujte hello projekt konzolové aplikace nezávislé projekt webové úlohy](#convertnolink).

### <a id="createlink"></a>Použít šablonu nový projekt webové úlohy hello pro webové úlohy propojené tooa webového projektu
1. Klikněte pravým tlačítkem na hello webového projektu v **Průzkumníku řešení**a potom klikněte na **přidat** > **nový projekt webové úlohy Azure**.
   
    ![Nová položka nabídky projektu webová úloha Azure](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    Hello [přidat webové úlohy Azure](#configure) zobrazí se dialogové okno.
2. Dokončení hello [přidat webové úlohy Azure](#configure) dialogové okno a pak klikněte na tlačítko **OK**.

## <a id="configure"></a>Dialogové okno Přidat webové úlohy Azure Hello
Hello **přidat webové úlohy Azure** dialogovém okně můžete zadat název webové úlohy hello a spusťte nastavení režimu pro vaše webová úloha. 

![Přidat dialogové okno webová úloha Azure](./media/websites-dotnet-deploy-webjobs/aaw2.png)

Hello pole v tomto dialogovém okně odpovídají toofields na hello **nová úloha** dialogu hello portálu Azure. Další informace najdete v tématu [úlohy na pozadí spustit s WebJobs](web-sites-create-web-jobs.md).

> [!NOTE]
> * Informace o nasazení příkazového řádku najdete v tématu [povolení příkazového řádku nebo průběžné doručování Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Pokud nasadíte webovou úlohu a potom se rozhodnete, že chcete typ hello toochange webové úlohy a znovu ho zaveďte, budete potřebovat soubor webové úlohy publikovat settings.json toodelete hello. Bude Visual Studio zobrazit hello možnosti publikování znovu, abyste mohli změnit typ hello webové úlohy.
> * Pokud nasadíte webovou úlohu a později změníte hello režimu spuštění z průběžné toonon průběžné nebo naopak, Visual Studio vytvoří nové webové úlohy v Azure při opětovném nasazování. Pokud změníte další plánování nastavení, ale ponechte spustit režim hello stejné nebo přepínat mezi naplánovaná a na vyžádání, aktualizace Visual Studio hello existující úlohy a nikoli vytvořit novou.
> 
> 

## <a id="publishsettings"></a>Webová úloha publikovat settings.json
Když konfigurujete konzolovou aplikaci pro webové úlohy nasazení, Visual Studio nainstaluje hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet balíčku a ukládá informace v o plánování *webové úlohy publikovat settings.json*  soubor v projektu hello *vlastnosti* složku projekt webové úlohy hello. Tady je příklad tohoto souboru:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Tento soubor můžete upravit přímo a Visual Studio poskytuje technologii IntelliSense. Schéma souboru Hello je uloženo na [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) a lze je zobrazit.  

## <a id="webjobslist"></a>list.json webové úlohy
Při propojení projektu webové úlohy povolené tooa webového projektu sady Visual Studio ukládá hello název hello projekt webové úlohy v *webjobs list.json* souboru v hello webového projektu *vlastnosti* složky. Hello seznam může obsahovat více projektů webové úlohy, jak je znázorněno v hello následující ukázka:

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

Tento soubor můžete upravit přímo a Visual Studio poskytuje technologii IntelliSense. Schéma souboru Hello je uloženo na [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) a lze je zobrazit.

## <a id="deploy"></a>Nasazení projektu webové úlohy
Projekt webové úlohy propojení projektu webové tooa nasadí automaticky s hello webového projektu. Informace o nasazení webového projektu najdete v tématu [jak toodeploy tooWeb aplikace](web-sites-deploy.md).

toodeploy projekt webové úlohy samostatně, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a klikněte na tlačítko **publikovat jako webová úloha Azure...** . 

![Publikovat jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/paw.png)

Pro nezávislé webové úlohy, hello stejné **Publikovat Web** průvodce, který se používá pro webové projekty, ale s méně toochange k dispozici nastavení.

## <a id="nextsteps"></a>Další kroky
Tento článek obsahuje vysvětlení jak toodeploy webové úlohy pomocí sady Visual Studio. Další informace o tom, jak toodeploy Azure WebJobs, najdete v části [Azure WebJobs - doporučené prostředky - nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).

