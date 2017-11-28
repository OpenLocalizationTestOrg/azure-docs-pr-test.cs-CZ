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
# <a name="deploy-webjobs-using-visual-studio"></a><span data-ttu-id="0f088-103">Nasazení WebJobs přes Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f088-103">Deploy WebJobs using Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="0f088-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0f088-104">Overview</span></span>
<span data-ttu-id="0f088-105">Toto téma vysvětluje, jak toouse Visual Studio toodeploy konzolovou aplikaci projektu tooa webové aplikace ve [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) jako [webové úlohy Azure](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="0f088-105">This topic explains how toouse Visual Studio toodeploy a Console Application project tooa web app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) as an [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span> <span data-ttu-id="0f088-106">Informace o tom, jak toodeploy webové úlohy pomocí hello [portálu Azure](https://portal.azure.com), najdete v části [úlohy na pozadí spustit s WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="0f088-106">For information about how toodeploy WebJobs by using hello [Azure Portal](https://portal.azure.com), see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

<span data-ttu-id="0f088-107">Když Visual Studio nasadí projekt aplikace s povolenými webové konzoly, provádí dvě úlohy:</span><span class="sxs-lookup"><span data-stu-id="0f088-107">When Visual Studio deploys a WebJobs-enabled Console Application project, it performs two tasks:</span></span>

* <span data-ttu-id="0f088-108">Soubory modulu runtime kopie toohello odpovídající složku v hello webové aplikace (*App_Data/úlohy/průběžné* pro nepřetržité webové úlohy *App_Data, úlohy nebo aktivaci* pro webové úlohy naplánované i na vyžádání).</span><span class="sxs-lookup"><span data-stu-id="0f088-108">Copies runtime files toohello appropriate folder in hello web app (*App_Data/jobs/continuous* for continuous WebJobs, *App_Data/jobs/triggered* for scheduled and on-demand WebJobs).</span></span>
* <span data-ttu-id="0f088-109">Nastaví [úlohy Azure Scheduler](#scheduler) pro webové úlohy, které jsou naplánované toorun v určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="0f088-109">Sets up [Azure Scheduler jobs](#scheduler) for WebJobs that are scheduled toorun at particular times.</span></span> <span data-ttu-id="0f088-110">(To není nutné pro nepřetržité webové úlohy).</span><span class="sxs-lookup"><span data-stu-id="0f088-110">(This is not needed for continuous WebJobs.)</span></span>

<span data-ttu-id="0f088-111">Projekt webové úlohy povolené má hello následující tooit přidané položky:</span><span class="sxs-lookup"><span data-stu-id="0f088-111">A WebJobs-enabled project has hello following items added tooit:</span></span>

* <span data-ttu-id="0f088-112">Hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0f088-112">hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package.</span></span>
* <span data-ttu-id="0f088-113">A [webové úlohy publikovat settings.json](#publishsettings) soubor, který obsahuje nastavení nasazení a plánovače.</span><span class="sxs-lookup"><span data-stu-id="0f088-113">A [webjob-publish-settings.json](#publishsettings) file that contains deployment and scheduler settings.</span></span> 

![Diagram zobrazující tooa konzolovou aplikaci tooenable nasazení co je přidána jako webová](./media/websites-dotnet-deploy-webjobs/convert.png)

<span data-ttu-id="0f088-115">Můžete přidat tyto položky tooan existující projekt konzolové aplikace nebo použít šablonu toocreate nový projekt aplikace s povolenými webové úlohy konzoly.</span><span class="sxs-lookup"><span data-stu-id="0f088-115">You can add these items tooan existing Console Application project or use a template toocreate a new WebJobs-enabled Console Application project.</span></span> 

<span data-ttu-id="0f088-116">Můžete nasadit projekt jako webová samostatně nebo ho tak, aby automaticky nasadí vždy, když je nasazení webového projektu hello odkaz tooa webového projektu.</span><span class="sxs-lookup"><span data-stu-id="0f088-116">You can deploy a project as a WebJob by itself, or link it tooa web project so that it automatically deploys whenever you deploy hello web project.</span></span> <span data-ttu-id="0f088-117">toolink projektů sady Visual Studio obsahuje název hello hello povolené webové úlohy projektu v [webjobs list.json](#webjobslist) souboru v hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="0f088-117">toolink projects, Visual Studio includes hello name of hello WebJobs-enabled project in a [webjobs-list.json](#webjobslist) file in hello web project.</span></span>

![Diagram zobrazující projektu úlohy WebJob propojení projektu tooweb](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a><span data-ttu-id="0f088-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0f088-119">Prerequisites</span></span>
<span data-ttu-id="0f088-120">Funkce nasazení webové úlohy jsou k dispozici v sadě Visual Studio, když instalujete hello Azure SDK pro .NET:</span><span class="sxs-lookup"><span data-stu-id="0f088-120">WebJobs deployment features are available in Visual Studio when you install hello Azure SDK for .NET:</span></span>

* <span data-ttu-id="0f088-121">[Azure SDK pro .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0f088-121">[Azure SDK for .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span></span>

## <span data-ttu-id="0f088-122"><a id="convert"></a>Povolit nasazení webové úlohy pro existující projekt konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="0f088-122"><a id="convert"></a>Enable WebJobs deployment for an existing Console Application project</span></span>
<span data-ttu-id="0f088-123">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="0f088-123">You have two options:</span></span>

* <span data-ttu-id="0f088-124">[Povolit automatické nasazení webového projektu](#convertlink).</span><span class="sxs-lookup"><span data-stu-id="0f088-124">[Enable automatic deployment with a web project](#convertlink).</span></span>
  
    <span data-ttu-id="0f088-125">Existující projekt konzolové aplikace nakonfigurujte tak, aby se automaticky nasadí jako webová při nasazení webového projektu.</span><span class="sxs-lookup"><span data-stu-id="0f088-125">Configure an existing Console Application project so that it automatically deploys as a WebJob when you deploy a web project.</span></span> <span data-ttu-id="0f088-126">Tuto možnost použijte, pokud chcete, aby toorun vaše webová úloha v hello stejné webové aplikace, ve kterém můžete spouštět hello související webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f088-126">Use this option when you want toorun your WebJob in hello same web app in which you run hello related web application.</span></span>
* <span data-ttu-id="0f088-127">[Povolit nasazení bez webového projektu](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="0f088-127">[Enable deployment without a web project](#convertnolink).</span></span>
  
    <span data-ttu-id="0f088-128">Nakonfigurujte existující toodeploy projekt konzolové aplikace jako webová samostatně, žádný odkaz tooa webový projekt.</span><span class="sxs-lookup"><span data-stu-id="0f088-128">Configure an existing Console Application project toodeploy as a WebJob by itself, with no link tooa web project.</span></span> <span data-ttu-id="0f088-129">Tuto možnost použijte, pokud chcete toorun webová ve webové aplikaci samostatně, s žádná webová aplikace spuštěna v hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f088-129">Use this option when you want toorun a WebJob in a web app by itself, with no web application running in hello web app.</span></span> <span data-ttu-id="0f088-130">Můžete chtít toodo to v pořadí možné tooscale toobe prostředkům webové úlohy nezávisle na prostředkům webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f088-130">You might want toodo this in order toobe able tooscale your WebJob resources independently of your web application resources.</span></span>

### <span data-ttu-id="0f088-131"><a id="convertlink"></a>Povolit automatické nasazení webové úlohy s webovým projektem</span><span class="sxs-lookup"><span data-stu-id="0f088-131"><a id="convertlink"></a> Enable automatic WebJobs deployment with a web project</span></span>
1. <span data-ttu-id="0f088-132">Klikněte pravým tlačítkem na hello webového projektu v **Průzkumníku řešení**a potom klikněte na **přidat** > **stávající projekt jako webová úloha Azure**.</span><span class="sxs-lookup"><span data-stu-id="0f088-132">Right-click hello web project in **Solution Explorer**, and then click **Add** > **Existing Project as Azure WebJob**.</span></span>
   
    ![Stávající projekt jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    <span data-ttu-id="0f088-134">Hello [přidat webové úlohy Azure](#configure) zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0f088-134">hello [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="0f088-135">V hello **název projektu** rozevíracího seznamu, vyberte hello konzolové aplikace projektu tooadd jako webovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="0f088-135">In hello **Project name** drop-down list, select hello Console Application project tooadd as a WebJob.</span></span>
   
    ![Výběr projektu v dialogovém okně Přidat webové úlohy Azure](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. <span data-ttu-id="0f088-137">Dokončení hello [přidat webové úlohy Azure](#configure) dialogové okno a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f088-137">Complete hello [Add Azure WebJob](#configure) dialog, and then click **OK**.</span></span> 

### <span data-ttu-id="0f088-138"><a id="convertnolink"></a>Povolit nasazení webové úlohy bez webového projektu</span><span class="sxs-lookup"><span data-stu-id="0f088-138"><a id="convertnolink"></a> Enable WebJobs deployment without a web project</span></span>
1. <span data-ttu-id="0f088-139">Projekt konzolové aplikace hello klikněte pravým tlačítkem v **Průzkumníku řešení**a pak klikněte na tlačítko **publikovat jako webová úloha Azure...** .</span><span class="sxs-lookup"><span data-stu-id="0f088-139">Right-click hello Console Application project in **Solution Explorer**, and then click **Publish as Azure WebJob...**.</span></span> 
   
    ![Publikovat jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    <span data-ttu-id="0f088-141">Hello [přidat webové úlohy Azure](#configure) dialogové okno se zobrazí s hello projekt vybraný v hello **název projektu** pole.</span><span class="sxs-lookup"><span data-stu-id="0f088-141">hello [Add Azure WebJob](#configure) dialog box appears, with hello project selected in hello **Project name** box.</span></span>
2. <span data-ttu-id="0f088-142">Dokončení hello [přidat webové úlohy Azure](#configure) dialogové okno a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f088-142">Complete hello [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>
   
   <span data-ttu-id="0f088-143">Hello **Publikovat Web** zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="0f088-143">hello **Publish Web** wizard appears.</span></span>  <span data-ttu-id="0f088-144">Pokud nechcete, aby toopublish okamžitě, zavřete průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="0f088-144">If you do not want toopublish immediately, close hello wizard.</span></span> <span data-ttu-id="0f088-145">Hello nastavení, které jste zadali, se uloží pro být příliš[nasazení projektu hello](#deploy).</span><span class="sxs-lookup"><span data-stu-id="0f088-145">hello settings that you've entered are saved for when you do want too[deploy hello project](#deploy).</span></span>

## <span data-ttu-id="0f088-146"><a id="create"></a>Vytvořte nový projekt webové úlohy povolena</span><span class="sxs-lookup"><span data-stu-id="0f088-146"><a id="create"></a>Create a new WebJobs-enabled project</span></span>
<span data-ttu-id="0f088-147">toocreate nový projekt webové úlohy povolené, můžete použít hello konzolové aplikace projektu šablony a povolit webové úlohy nasazení jak je popsáno v [hello předchozí části](#convert).</span><span class="sxs-lookup"><span data-stu-id="0f088-147">toocreate a new WebJobs-enabled project, you can use hello Console Application project template and enable WebJobs deployment as explained in [hello previous section](#convert).</span></span> <span data-ttu-id="0f088-148">Jako alternativu můžete šablonu nový projekt webové úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="0f088-148">As an alternative, you can use hello WebJobs new-project template:</span></span>

* [<span data-ttu-id="0f088-149">Použít šablonu nový projekt webové úlohy hello pro nezávislé webové úlohy</span><span class="sxs-lookup"><span data-stu-id="0f088-149">Use hello WebJobs new-project template for an independent WebJob</span></span>](#createnolink)
  
    <span data-ttu-id="0f088-150">Vytvoření projektu a nakonfigurujte ho toodeploy samostatně jako webová úloha se žádný odkaz tooa webový projekt.</span><span class="sxs-lookup"><span data-stu-id="0f088-150">Create a project and configure it toodeploy by itself as a WebJob, with no link tooa web project.</span></span> <span data-ttu-id="0f088-151">Tuto možnost použijte, pokud chcete toorun webová ve webové aplikaci samostatně, s žádná webová aplikace spuštěna v hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f088-151">Use this option when you want toorun a WebJob in a web app by itself, with no web application running in hello web app.</span></span> <span data-ttu-id="0f088-152">Můžete chtít toodo to v pořadí možné tooscale toobe prostředkům webové úlohy nezávisle na prostředkům webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f088-152">You might want toodo this in order toobe able tooscale your WebJob resources independently of your web application resources.</span></span>
* [<span data-ttu-id="0f088-153">Použít šablonu nový projekt webové úlohy hello pro webové úlohy propojené tooa webového projektu</span><span class="sxs-lookup"><span data-stu-id="0f088-153">Use hello WebJobs new-project template for a WebJob linked tooa web project</span></span>](#createlink)
  
    <span data-ttu-id="0f088-154">Vytvořte projekt, který je nakonfigurovaný toodeploy automaticky jako webová úloha, když webového projektu v hello stejné řešení nasazeno.</span><span class="sxs-lookup"><span data-stu-id="0f088-154">Create a project that is configured toodeploy automatically as a WebJob when a web project in hello same solution is deployed.</span></span> <span data-ttu-id="0f088-155">Tuto možnost použijte, pokud chcete, aby toorun vaše webová úloha v hello stejné webové aplikace, ve kterém můžete spouštět hello související webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f088-155">Use this option when you want toorun your WebJob in hello same web app in which you run hello related web application.</span></span>

> [!NOTE]
> <span data-ttu-id="0f088-156">Šablona Nový projekt webové úlohy Hello automaticky nainstaluje balíčky NuGet a obsahuje kód v *Program.cs* pro hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span><span class="sxs-lookup"><span data-stu-id="0f088-156">hello WebJobs new-project template automatically installs NuGet packages and includes code in *Program.cs* for hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span></span> <span data-ttu-id="0f088-157">Pokud nechcete, aby toouse hello WebJobs SDK, odebrat ani změnit hello `host.RunAndBlock` příkaz v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="0f088-157">If you don't want toouse hello WebJobs SDK, remove or change hello `host.RunAndBlock` statement in *Program.cs*.</span></span>
> 
> 

### <span data-ttu-id="0f088-158"><a id="createnolink"></a>Použít šablonu nový projekt webové úlohy hello pro nezávislé webové úlohy</span><span class="sxs-lookup"><span data-stu-id="0f088-158"><a id="createnolink"></a> Use hello WebJobs new-project template for an independent WebJob</span></span>
1. <span data-ttu-id="0f088-159">Klikněte na tlačítko **soubor** > **nový projekt**a potom v hello **nový projekt** dialogovém okně **cloudu**  >   **Webová úloha Azure (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="0f088-159">Click **File** > **New Project**, and then in hello **New Project** dialog box click **Cloud** > **Azure WebJob (.NET Framework)**.</span></span>
   
    ![Dialogové okno Nový projekt zobrazující šablony webové úlohy](./media/websites-dotnet-deploy-webjobs/np.png)
2. <span data-ttu-id="0f088-161">Postupujte podle pokynů hello uvedena výše příliš[zkontrolujte hello projekt konzolové aplikace nezávislé projekt webové úlohy](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="0f088-161">Follow hello directions shown earlier too[make hello Console Application project an independent WebJobs project](#convertnolink).</span></span>

### <span data-ttu-id="0f088-162"><a id="createlink"></a>Použít šablonu nový projekt webové úlohy hello pro webové úlohy propojené tooa webového projektu</span><span class="sxs-lookup"><span data-stu-id="0f088-162"><a id="createlink"></a> Use hello WebJobs new-project template for a WebJob linked tooa web project</span></span>
1. <span data-ttu-id="0f088-163">Klikněte pravým tlačítkem na hello webového projektu v **Průzkumníku řešení**a potom klikněte na **přidat** > **nový projekt webové úlohy Azure**.</span><span class="sxs-lookup"><span data-stu-id="0f088-163">Right-click hello web project in **Solution Explorer**, and then click **Add** > **New Azure WebJob Project**.</span></span>
   
    ![Nová položka nabídky projektu webová úloha Azure](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    <span data-ttu-id="0f088-165">Hello [přidat webové úlohy Azure](#configure) zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0f088-165">hello [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="0f088-166">Dokončení hello [přidat webové úlohy Azure](#configure) dialogové okno a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f088-166">Complete hello [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>

## <span data-ttu-id="0f088-167"><a id="configure"></a>Dialogové okno Přidat webové úlohy Azure Hello</span><span class="sxs-lookup"><span data-stu-id="0f088-167"><a id="configure"></a>hello Add Azure WebJob dialog</span></span>
<span data-ttu-id="0f088-168">Hello **přidat webové úlohy Azure** dialogovém okně můžete zadat název webové úlohy hello a spusťte nastavení režimu pro vaše webová úloha.</span><span class="sxs-lookup"><span data-stu-id="0f088-168">hello **Add Azure WebJob** dialog lets you enter hello WebJob name and run mode setting for your WebJob.</span></span> 

![Přidat dialogové okno webová úloha Azure](./media/websites-dotnet-deploy-webjobs/aaw2.png)

<span data-ttu-id="0f088-170">Hello pole v tomto dialogovém okně odpovídají toofields na hello **nová úloha** dialogu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0f088-170">hello fields in this dialog correspond toofields on hello **New Job** dialog of hello Azure Portal.</span></span> <span data-ttu-id="0f088-171">Další informace najdete v tématu [úlohy na pozadí spustit s WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="0f088-171">For more information, see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

> [!NOTE]
> * <span data-ttu-id="0f088-172">Informace o nasazení příkazového řádku najdete v tématu [povolení příkazového řádku nebo průběžné doručování Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span><span class="sxs-lookup"><span data-stu-id="0f088-172">For information about command-line deployment, see [Enabling Command-line or Continuous Delivery of Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span></span>
> * <span data-ttu-id="0f088-173">Pokud nasadíte webovou úlohu a potom se rozhodnete, že chcete typ hello toochange webové úlohy a znovu ho zaveďte, budete potřebovat soubor webové úlohy publikovat settings.json toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="0f088-173">If you deploy a WebJob and then decide you want toochange hello type of WebJob and redeploy, you'll need toodelete hello webjobs-publish-settings.json file.</span></span> <span data-ttu-id="0f088-174">Bude Visual Studio zobrazit hello možnosti publikování znovu, abyste mohli změnit typ hello webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="0f088-174">This will make Visual Studio show hello publishing options again, so you can change hello type of WebJob.</span></span>
> * <span data-ttu-id="0f088-175">Pokud nasadíte webovou úlohu a později změníte hello režimu spuštění z průběžné toonon průběžné nebo naopak, Visual Studio vytvoří nové webové úlohy v Azure při opětovném nasazování.</span><span class="sxs-lookup"><span data-stu-id="0f088-175">If you deploy a WebJob and later change hello run mode from continuous toonon-continuous or vice versa, Visual Studio creates a new WebJob in Azure when you redeploy.</span></span> <span data-ttu-id="0f088-176">Pokud změníte další plánování nastavení, ale ponechte spustit režim hello stejné nebo přepínat mezi naplánovaná a na vyžádání, aktualizace Visual Studio hello existující úlohy a nikoli vytvořit novou.</span><span class="sxs-lookup"><span data-stu-id="0f088-176">If you change other scheduling settings but leave run mode hello same or switch between Scheduled and On Demand, Visual Studio updates hello existing job rather than create a new one.</span></span>
> 
> 

## <span data-ttu-id="0f088-177"><a id="publishsettings"></a>Webová úloha publikovat settings.json</span><span class="sxs-lookup"><span data-stu-id="0f088-177"><a id="publishsettings"></a>webjob-publish-settings.json</span></span>
<span data-ttu-id="0f088-178">Když konfigurujete konzolovou aplikaci pro webové úlohy nasazení, Visual Studio nainstaluje hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet balíčku a ukládá informace v o plánování *webové úlohy publikovat settings.json*  soubor v projektu hello *vlastnosti* složku projekt webové úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="0f088-178">When you configure a Console Application for WebJobs deployment, Visual Studio installs hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package and stores scheduling information in a *webjob-publish-settings.json* file in hello project *Properties* folder of hello WebJobs project.</span></span> <span data-ttu-id="0f088-179">Tady je příklad tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="0f088-179">Here is an example of that file:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

<span data-ttu-id="0f088-180">Tento soubor můžete upravit přímo a Visual Studio poskytuje technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="0f088-180">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="0f088-181">Schéma souboru Hello je uloženo na [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) a lze je zobrazit.</span><span class="sxs-lookup"><span data-stu-id="0f088-181">hello file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) and can be viewed there.</span></span>  

## <span data-ttu-id="0f088-182"><a id="webjobslist"></a>list.json webové úlohy</span><span class="sxs-lookup"><span data-stu-id="0f088-182"><a id="webjobslist"></a>webjobs-list.json</span></span>
<span data-ttu-id="0f088-183">Při propojení projektu webové úlohy povolené tooa webového projektu sady Visual Studio ukládá hello název hello projekt webové úlohy v *webjobs list.json* souboru v hello webového projektu *vlastnosti* složky.</span><span class="sxs-lookup"><span data-stu-id="0f088-183">When you link a WebJobs-enabled project tooa web project, Visual Studio stores hello name of hello WebJobs project in a *webjobs-list.json* file in hello web project's *Properties* folder.</span></span> <span data-ttu-id="0f088-184">Hello seznam může obsahovat více projektů webové úlohy, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0f088-184">hello list might contain multiple WebJobs projects, as shown in hello following example:</span></span>

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

<span data-ttu-id="0f088-185">Tento soubor můžete upravit přímo a Visual Studio poskytuje technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="0f088-185">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="0f088-186">Schéma souboru Hello je uloženo na [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) a lze je zobrazit.</span><span class="sxs-lookup"><span data-stu-id="0f088-186">hello file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) and can be viewed there.</span></span>

## <span data-ttu-id="0f088-187"><a id="deploy"></a>Nasazení projektu webové úlohy</span><span class="sxs-lookup"><span data-stu-id="0f088-187"><a id="deploy"></a>Deploy a WebJobs project</span></span>
<span data-ttu-id="0f088-188">Projekt webové úlohy propojení projektu webové tooa nasadí automaticky s hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="0f088-188">A WebJobs project that you have linked tooa web project deploys automatically with hello web project.</span></span> <span data-ttu-id="0f088-189">Informace o nasazení webového projektu najdete v tématu [jak toodeploy tooWeb aplikace](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="0f088-189">For information about web project deployment, see [How toodeploy tooWeb Apps](web-sites-deploy.md).</span></span>

<span data-ttu-id="0f088-190">toodeploy projekt webové úlohy samostatně, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a klikněte na tlačítko **publikovat jako webová úloha Azure...** .</span><span class="sxs-lookup"><span data-stu-id="0f088-190">toodeploy a WebJobs project by itself, right-click hello project in **Solution Explorer** and click **Publish as Azure WebJob...**.</span></span> 

![Publikovat jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/paw.png)

<span data-ttu-id="0f088-192">Pro nezávislé webové úlohy, hello stejné **Publikovat Web** průvodce, který se používá pro webové projekty, ale s méně toochange k dispozici nastavení.</span><span class="sxs-lookup"><span data-stu-id="0f088-192">For an independent WebJob, hello same **Publish Web** wizard that is used for web projects appears, but with fewer settings available toochange.</span></span>

## <span data-ttu-id="0f088-193"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f088-193"><a id="nextsteps"></a>Next Steps</span></span>
<span data-ttu-id="0f088-194">Tento článek obsahuje vysvětlení jak toodeploy webové úlohy pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f088-194">This article has explained how toodeploy WebJobs by using Visual Studio.</span></span> <span data-ttu-id="0f088-195">Další informace o tom, jak toodeploy Azure WebJobs, najdete v části [Azure WebJobs - doporučené prostředky - nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span><span class="sxs-lookup"><span data-stu-id="0f088-195">For more information about how toodeploy Azure WebJobs, see [Azure WebJobs - Recommended Resources - Deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span></span>

