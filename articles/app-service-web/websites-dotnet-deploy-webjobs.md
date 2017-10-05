---
title: "Nasazení WebJobs přes Visual Studio"
description: "Zjistěte, jak nasadit Azure WebJobs do Azure App Service Web Apps pomocí sady Visual Studio."
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
ms.openlocfilehash: 5b0808afdadcf4d86a9a2d07ee6fc63b80b22993
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a><span data-ttu-id="9cf30-103">Nasazení WebJobs přes Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cf30-103">Deploy WebJobs using Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="9cf30-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9cf30-104">Overview</span></span>
<span data-ttu-id="9cf30-105">Toto téma vysvětluje, jak používat Visual Studio k nasazení projektu konzolové aplikace do webové aplikace ve [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) jako [webové úlohy Azure](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="9cf30-105">This topic explains how to use Visual Studio to deploy a Console Application project to a web app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) as an [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span> <span data-ttu-id="9cf30-106">Informace o tom, jak nasadit webové úlohy pomocí [portálu Azure](https://portal.azure.com), najdete v části [úlohy na pozadí spustit s WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="9cf30-106">For information about how to deploy WebJobs by using the [Azure Portal](https://portal.azure.com), see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

<span data-ttu-id="9cf30-107">Když Visual Studio nasadí projekt aplikace s povolenými webové konzoly, provádí dvě úlohy:</span><span class="sxs-lookup"><span data-stu-id="9cf30-107">When Visual Studio deploys a WebJobs-enabled Console Application project, it performs two tasks:</span></span>

* <span data-ttu-id="9cf30-108">Kopíruje soubory modulu runtime do příslušné složky ve webové aplikaci (*App_Data/úlohy/průběžné* pro nepřetržité webové úlohy *App_Data, úlohy nebo aktivaci* pro webové úlohy naplánované i na vyžádání).</span><span class="sxs-lookup"><span data-stu-id="9cf30-108">Copies runtime files to the appropriate folder in the web app (*App_Data/jobs/continuous* for continuous WebJobs, *App_Data/jobs/triggered* for scheduled and on-demand WebJobs).</span></span>
* <span data-ttu-id="9cf30-109">Nastaví [úlohy Azure Scheduler](#scheduler) pro webové úlohy, které mají naplánované spuštění v určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="9cf30-109">Sets up [Azure Scheduler jobs](#scheduler) for WebJobs that are scheduled to run at particular times.</span></span> <span data-ttu-id="9cf30-110">(To není nutné pro nepřetržité webové úlohy).</span><span class="sxs-lookup"><span data-stu-id="9cf30-110">(This is not needed for continuous WebJobs.)</span></span>

<span data-ttu-id="9cf30-111">Projekt webové úlohy povolené má přidána následující položky:</span><span class="sxs-lookup"><span data-stu-id="9cf30-111">A WebJobs-enabled project has the following items added to it:</span></span>

* <span data-ttu-id="9cf30-112">[Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="9cf30-112">The [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package.</span></span>
* <span data-ttu-id="9cf30-113">A [webové úlohy publikovat settings.json](#publishsettings) soubor, který obsahuje nastavení nasazení a plánovače.</span><span class="sxs-lookup"><span data-stu-id="9cf30-113">A [webjob-publish-settings.json](#publishsettings) file that contains deployment and scheduler settings.</span></span> 

![Diagram znázorňující, co se přidá do konzoly aplikaci, která umožní nasazení jako webová](./media/websites-dotnet-deploy-webjobs/convert.png)

<span data-ttu-id="9cf30-115">Můžete přidat tyto položky do existujícího projektu konzolové aplikace nebo použijte šablonu pro vytvoření nového projektu aplikace s povolenými webové úlohy konzoly.</span><span class="sxs-lookup"><span data-stu-id="9cf30-115">You can add these items to an existing Console Application project or use a template to create a new WebJobs-enabled Console Application project.</span></span> 

<span data-ttu-id="9cf30-116">Můžete nasadit na projekt jako webová samostatně, nebo odkaz na projekt webové, tak, aby automaticky nasadí vždy, když je nasazení webového projektu.</span><span class="sxs-lookup"><span data-stu-id="9cf30-116">You can deploy a project as a WebJob by itself, or link it to a web project so that it automatically deploys whenever you deploy the web project.</span></span> <span data-ttu-id="9cf30-117">Odkaz projekty, Visual Studio obsahuje název projektu webové úlohy povolené v [webjobs list.json](#webjobslist) souboru webového projektu.</span><span class="sxs-lookup"><span data-stu-id="9cf30-117">To link projects, Visual Studio includes the name of the WebJobs-enabled project in a [webjobs-list.json](#webjobslist) file in the web project.</span></span>

![Diagram zobrazující projektu úlohy WebJob připojení k webovému projektu](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a><span data-ttu-id="9cf30-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9cf30-119">Prerequisites</span></span>
<span data-ttu-id="9cf30-120">Funkce nasazení webové úlohy jsou k dispozici v sadě Visual Studio, při instalaci sady Azure SDK pro .NET:</span><span class="sxs-lookup"><span data-stu-id="9cf30-120">WebJobs deployment features are available in Visual Studio when you install the Azure SDK for .NET:</span></span>

* <span data-ttu-id="9cf30-121">[Azure SDK pro .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="9cf30-121">[Azure SDK for .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span></span>

## <span data-ttu-id="9cf30-122"><a id="convert"></a>Povolit nasazení webové úlohy pro existující projekt konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="9cf30-122"><a id="convert"></a>Enable WebJobs deployment for an existing Console Application project</span></span>
<span data-ttu-id="9cf30-123">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="9cf30-123">You have two options:</span></span>

* <span data-ttu-id="9cf30-124">[Povolit automatické nasazení webového projektu](#convertlink).</span><span class="sxs-lookup"><span data-stu-id="9cf30-124">[Enable automatic deployment with a web project](#convertlink).</span></span>
  
    <span data-ttu-id="9cf30-125">Existující projekt konzolové aplikace nakonfigurujte tak, aby se automaticky nasadí jako webová při nasazení webového projektu.</span><span class="sxs-lookup"><span data-stu-id="9cf30-125">Configure an existing Console Application project so that it automatically deploys as a WebJob when you deploy a web project.</span></span> <span data-ttu-id="9cf30-126">Tuto možnost použijte, pokud chcete spustit vaše webová úloha ve stejné webové aplikaci, ve kterém můžete spouštět související webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cf30-126">Use this option when you want to run your WebJob in the same web app in which you run the related web application.</span></span>
* <span data-ttu-id="9cf30-127">[Povolit nasazení bez webového projektu](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="9cf30-127">[Enable deployment without a web project](#convertnolink).</span></span>
  
    <span data-ttu-id="9cf30-128">Existující projekt konzolové aplikace nasadit jako webovou úlohu samostatně, se žádný odkaz na projekt webové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9cf30-128">Configure an existing Console Application project to deploy as a WebJob by itself, with no link to a web project.</span></span> <span data-ttu-id="9cf30-129">Tuto možnost použijte, pokud chcete spustit webovou úlohu ve webové aplikaci samostatně, s žádná webová aplikace spuštěna ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9cf30-129">Use this option when you want to run a WebJob in a web app by itself, with no web application running in the web app.</span></span> <span data-ttu-id="9cf30-130">Můžete k tomu, aby bylo možné škálovat vaše prostředky webové úlohy nezávisle na prostředkům webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cf30-130">You might want to do this in order to be able to scale your WebJob resources independently of your web application resources.</span></span>

### <span data-ttu-id="9cf30-131"><a id="convertlink"></a>Povolit automatické nasazení webové úlohy s webovým projektem</span><span class="sxs-lookup"><span data-stu-id="9cf30-131"><a id="convertlink"></a> Enable automatic WebJobs deployment with a web project</span></span>
1. <span data-ttu-id="9cf30-132">Klikněte pravým tlačítkem na projekt webové v **Průzkumníku řešení**a potom klikněte na **přidat** > **stávající projekt jako webová úloha Azure**.</span><span class="sxs-lookup"><span data-stu-id="9cf30-132">Right-click the web project in **Solution Explorer**, and then click **Add** > **Existing Project as Azure WebJob**.</span></span>
   
    ![Stávající projekt jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    <span data-ttu-id="9cf30-134">[Přidat webové úlohy Azure](#configure) zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9cf30-134">The [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="9cf30-135">V **název projektu** rozevíracím seznamu vyberte projektu konzolové aplikace přidejte jako webovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="9cf30-135">In the **Project name** drop-down list, select the Console Application project to add as a WebJob.</span></span>
   
    ![Výběr projektu v dialogovém okně Přidat webové úlohy Azure](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. <span data-ttu-id="9cf30-137">Dokončení [přidat webové úlohy Azure](#configure) dialogové okno a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9cf30-137">Complete the [Add Azure WebJob](#configure) dialog, and then click **OK**.</span></span> 

### <span data-ttu-id="9cf30-138"><a id="convertnolink"></a>Povolit nasazení webové úlohy bez webového projektu</span><span class="sxs-lookup"><span data-stu-id="9cf30-138"><a id="convertnolink"></a> Enable WebJobs deployment without a web project</span></span>
1. <span data-ttu-id="9cf30-139">Klikněte pravým tlačítkem na projekt konzolové aplikace v **Průzkumníku řešení**a pak klikněte na tlačítko **publikovat jako webová úloha Azure...** .</span><span class="sxs-lookup"><span data-stu-id="9cf30-139">Right-click the Console Application project in **Solution Explorer**, and then click **Publish as Azure WebJob...**.</span></span> 
   
    ![Publikovat jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    <span data-ttu-id="9cf30-141">[Přidat webové úlohy Azure](#configure) dialogové okno se zobrazí s projekt vybraný v **název projektu** pole.</span><span class="sxs-lookup"><span data-stu-id="9cf30-141">The [Add Azure WebJob](#configure) dialog box appears, with the project selected in the **Project name** box.</span></span>
2. <span data-ttu-id="9cf30-142">Dokončení [přidat webové úlohy Azure](#configure) dialogové okno a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9cf30-142">Complete the [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>
   
   <span data-ttu-id="9cf30-143">**Publikovat Web** zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="9cf30-143">The **Publish Web** wizard appears.</span></span>  <span data-ttu-id="9cf30-144">Pokud nechcete publikovat okamžitě, zavřete průvodce.</span><span class="sxs-lookup"><span data-stu-id="9cf30-144">If you do not want to publish immediately, close the wizard.</span></span> <span data-ttu-id="9cf30-145">Nastavení, které jste zadali se ukládají pro, pokud chcete [nasazení projektu](#deploy).</span><span class="sxs-lookup"><span data-stu-id="9cf30-145">The settings that you've entered are saved for when you do want to [deploy the project](#deploy).</span></span>

## <span data-ttu-id="9cf30-146"><a id="create"></a>Vytvořte nový projekt webové úlohy povolena</span><span class="sxs-lookup"><span data-stu-id="9cf30-146"><a id="create"></a>Create a new WebJobs-enabled project</span></span>
<span data-ttu-id="9cf30-147">Pokud chcete vytvořit nový projekt webové úlohy povolené, můžete pomocí šablony projektu konzolové aplikace a povolit nasazení webové úlohy, jak je popsáno v [v předchozí části](#convert).</span><span class="sxs-lookup"><span data-stu-id="9cf30-147">To create a new WebJobs-enabled project, you can use the Console Application project template and enable WebJobs deployment as explained in [the previous section](#convert).</span></span> <span data-ttu-id="9cf30-148">Případně můžete šablonu nový projekt webové úlohy:</span><span class="sxs-lookup"><span data-stu-id="9cf30-148">As an alternative, you can use the WebJobs new-project template:</span></span>

* [<span data-ttu-id="9cf30-149">Použít šablonu nový projekt webové úlohy pro nezávislé webové úlohy</span><span class="sxs-lookup"><span data-stu-id="9cf30-149">Use the WebJobs new-project template for an independent WebJob</span></span>](#createnolink)
  
    <span data-ttu-id="9cf30-150">Vytvoření projektu a nakonfigurujte ho nasadit jako webová úloha se žádný odkaz na projekt webové samostatně.</span><span class="sxs-lookup"><span data-stu-id="9cf30-150">Create a project and configure it to deploy by itself as a WebJob, with no link to a web project.</span></span> <span data-ttu-id="9cf30-151">Tuto možnost použijte, pokud chcete spustit webovou úlohu ve webové aplikaci samostatně, s žádná webová aplikace spuštěna ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9cf30-151">Use this option when you want to run a WebJob in a web app by itself, with no web application running in the web app.</span></span> <span data-ttu-id="9cf30-152">Můžete k tomu, aby bylo možné škálovat vaše prostředky webové úlohy nezávisle na prostředkům webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cf30-152">You might want to do this in order to be able to scale your WebJob resources independently of your web application resources.</span></span>
* [<span data-ttu-id="9cf30-153">Použít šablonu nový projekt webové úlohy pro webovou úlohu propojené s webového projektu</span><span class="sxs-lookup"><span data-stu-id="9cf30-153">Use the WebJobs new-project template for a WebJob linked to a web project</span></span>](#createlink)
  
    <span data-ttu-id="9cf30-154">Vytvoření projektu nakonfigurovaný tak, aby automaticky jako webová nasazení při nasazení webového projektu ve stejném řešení.</span><span class="sxs-lookup"><span data-stu-id="9cf30-154">Create a project that is configured to deploy automatically as a WebJob when a web project in the same solution is deployed.</span></span> <span data-ttu-id="9cf30-155">Tuto možnost použijte, pokud chcete spustit vaše webová úloha ve stejné webové aplikaci, ve kterém můžete spouštět související webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cf30-155">Use this option when you want to run your WebJob in the same web app in which you run the related web application.</span></span>

> [!NOTE]
> <span data-ttu-id="9cf30-156">Automaticky nainstaluje balíčky NuGet a obsahuje kód v šabloně nový projekt webové úlohy *Program.cs* pro [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span><span class="sxs-lookup"><span data-stu-id="9cf30-156">The WebJobs new-project template automatically installs NuGet packages and includes code in *Program.cs* for the [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span></span> <span data-ttu-id="9cf30-157">Pokud nechcete, aby se používat sadu WebJobs SDK, odebrat ani změnit `host.RunAndBlock` příkaz v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="9cf30-157">If you don't want to use the WebJobs SDK, remove or change the `host.RunAndBlock` statement in *Program.cs*.</span></span>
> 
> 

### <span data-ttu-id="9cf30-158"><a id="createnolink"></a>Použít šablonu nový projekt webové úlohy pro nezávislé webové úlohy</span><span class="sxs-lookup"><span data-stu-id="9cf30-158"><a id="createnolink"></a> Use the WebJobs new-project template for an independent WebJob</span></span>
1. <span data-ttu-id="9cf30-159">Klikněte na tlačítko **soubor** > **nový projekt**a potom v **nový projekt** dialogovém okně **cloudu** > **webové úlohy Azure (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="9cf30-159">Click **File** > **New Project**, and then in the **New Project** dialog box click **Cloud** > **Azure WebJob (.NET Framework)**.</span></span>
   
    ![Dialogové okno Nový projekt zobrazující šablony webové úlohy](./media/websites-dotnet-deploy-webjobs/np.png)
2. <span data-ttu-id="9cf30-161">Postupujte podle pokynů, zobrazí dříve do [zpřístupnění aplikace konzoly projektu nezávislé projekt webové úlohy](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="9cf30-161">Follow the directions shown earlier to [make the Console Application project an independent WebJobs project](#convertnolink).</span></span>

### <span data-ttu-id="9cf30-162"><a id="createlink"></a>Použít šablonu nový projekt webové úlohy pro webovou úlohu propojené s webového projektu</span><span class="sxs-lookup"><span data-stu-id="9cf30-162"><a id="createlink"></a> Use the WebJobs new-project template for a WebJob linked to a web project</span></span>
1. <span data-ttu-id="9cf30-163">Klikněte pravým tlačítkem na projekt webové v **Průzkumníku řešení**a potom klikněte na **přidat** > **nový projekt webové úlohy Azure**.</span><span class="sxs-lookup"><span data-stu-id="9cf30-163">Right-click the web project in **Solution Explorer**, and then click **Add** > **New Azure WebJob Project**.</span></span>
   
    ![Nová položka nabídky projektu webová úloha Azure](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    <span data-ttu-id="9cf30-165">[Přidat webové úlohy Azure](#configure) zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9cf30-165">The [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="9cf30-166">Dokončení [přidat webové úlohy Azure](#configure) dialogové okno a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9cf30-166">Complete the [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>

## <span data-ttu-id="9cf30-167"><a id="configure"></a>Dialogové okno Přidat webové úlohy Azure</span><span class="sxs-lookup"><span data-stu-id="9cf30-167"><a id="configure"></a>The Add Azure WebJob dialog</span></span>
<span data-ttu-id="9cf30-168">**Přidat webové úlohy Azure** dialogovém okně můžete zadat název webové úlohy a spusťte nastavení režimu pro vaše webová úloha.</span><span class="sxs-lookup"><span data-stu-id="9cf30-168">The **Add Azure WebJob** dialog lets you enter the WebJob name and run mode setting for your WebJob.</span></span> 

![Přidat dialogové okno webová úloha Azure](./media/websites-dotnet-deploy-webjobs/aaw2.png)

<span data-ttu-id="9cf30-170">Pole v tomto dialogovém okně odpovídají pole na **nová úloha** dialogové okno portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9cf30-170">The fields in this dialog correspond to fields on the **New Job** dialog of the Azure Portal.</span></span> <span data-ttu-id="9cf30-171">Další informace najdete v tématu [úlohy na pozadí spustit s WebJobs](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="9cf30-171">For more information, see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

> [!NOTE]
> * <span data-ttu-id="9cf30-172">Informace o nasazení příkazového řádku najdete v tématu [povolení příkazového řádku nebo průběžné doručování Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span><span class="sxs-lookup"><span data-stu-id="9cf30-172">For information about command-line deployment, see [Enabling Command-line or Continuous Delivery of Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span></span>
> * <span data-ttu-id="9cf30-173">Pokud nasadíte webovou úlohu a potom se rozhodnete, že chcete změnit typ webové úlohy a znovu ho zaveďte, budete muset odstranit soubor webové úlohy publikovat settings.json.</span><span class="sxs-lookup"><span data-stu-id="9cf30-173">If you deploy a WebJob and then decide you want to change the type of WebJob and redeploy, you'll need to delete the webjobs-publish-settings.json file.</span></span> <span data-ttu-id="9cf30-174">Abyste mohli změnit typ webová úloha bude nezobrazovat možnosti publikování, sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9cf30-174">This will make Visual Studio show the publishing options again, so you can change the type of WebJob.</span></span>
> * <span data-ttu-id="9cf30-175">Pokud nasadíte webovou úlohu a později změníte režimu spuštění z průběžné nesouvislé nebo naopak, Visual Studio vytvoří nové webové úlohy v Azure při opětovném nasazování.</span><span class="sxs-lookup"><span data-stu-id="9cf30-175">If you deploy a WebJob and later change the run mode from continuous to non-continuous or vice versa, Visual Studio creates a new WebJob in Azure when you redeploy.</span></span> <span data-ttu-id="9cf30-176">Pokud změníte další plánování nastavení, ale ponechte režim spuštění stejné nebo přepínat mezi naplánovaná a na vyžádání, Visual Studio aktualizuje existující úlohy a nikoli vytvořit novou.</span><span class="sxs-lookup"><span data-stu-id="9cf30-176">If you change other scheduling settings but leave run mode the same or switch between Scheduled and On Demand, Visual Studio updates the existing job rather than create a new one.</span></span>
> 
> 

## <span data-ttu-id="9cf30-177"><a id="publishsettings"></a>Webová úloha publikovat settings.json</span><span class="sxs-lookup"><span data-stu-id="9cf30-177"><a id="publishsettings"></a>webjob-publish-settings.json</span></span>
<span data-ttu-id="9cf30-178">Při konfiguraci konzolovou aplikaci pro webové úlohy nasazení nainstaluje Visual Studio [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet balíčku a ukládá informace v o plánování *webové úlohy publikovat settings.json* v projektu *vlastnosti* složku projekt webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="9cf30-178">When you configure a Console Application for WebJobs deployment, Visual Studio installs the [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package and stores scheduling information in a *webjob-publish-settings.json* file in the project *Properties* folder of the WebJobs project.</span></span> <span data-ttu-id="9cf30-179">Tady je příklad tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="9cf30-179">Here is an example of that file:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

<span data-ttu-id="9cf30-180">Tento soubor můžete upravit přímo a Visual Studio poskytuje technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="9cf30-180">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="9cf30-181">Schéma souboru je uložené v [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) a lze je zobrazit.</span><span class="sxs-lookup"><span data-stu-id="9cf30-181">The file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) and can be viewed there.</span></span>  

## <span data-ttu-id="9cf30-182"><a id="webjobslist"></a>list.json webové úlohy</span><span class="sxs-lookup"><span data-stu-id="9cf30-182"><a id="webjobslist"></a>webjobs-list.json</span></span>
<span data-ttu-id="9cf30-183">Když propojíte projekt webové úlohy povolené webového projektu, Visual Studio uloží název projektu webové úlohy v *webjobs list.json* souboru webového projektu *vlastnosti* složky.</span><span class="sxs-lookup"><span data-stu-id="9cf30-183">When you link a WebJobs-enabled project to a web project, Visual Studio stores the name of the WebJobs project in a *webjobs-list.json* file in the web project's *Properties* folder.</span></span> <span data-ttu-id="9cf30-184">Seznam může obsahovat více projektů webové úlohy, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9cf30-184">The list might contain multiple WebJobs projects, as shown in the following example:</span></span>

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

<span data-ttu-id="9cf30-185">Tento soubor můžete upravit přímo a Visual Studio poskytuje technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="9cf30-185">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="9cf30-186">Schéma souboru je uložené v [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) a lze je zobrazit.</span><span class="sxs-lookup"><span data-stu-id="9cf30-186">The file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) and can be viewed there.</span></span>

## <span data-ttu-id="9cf30-187"><a id="deploy"></a>Nasazení projektu webové úlohy</span><span class="sxs-lookup"><span data-stu-id="9cf30-187"><a id="deploy"></a>Deploy a WebJobs project</span></span>
<span data-ttu-id="9cf30-188">Projekt webové úlohy, který propojení projektu webové nasadí automaticky s webového projektu.</span><span class="sxs-lookup"><span data-stu-id="9cf30-188">A WebJobs project that you have linked to a web project deploys automatically with the web project.</span></span> <span data-ttu-id="9cf30-189">Informace o nasazení webového projektu najdete v tématu [jak nasadit do webové aplikace](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="9cf30-189">For information about web project deployment, see [How to deploy to Web Apps](web-sites-deploy.md).</span></span>

<span data-ttu-id="9cf30-190">Chcete-li nasadit projekt webové úlohy sám o sobě, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a klikněte na tlačítko **publikovat jako webová úloha Azure...** .</span><span class="sxs-lookup"><span data-stu-id="9cf30-190">To deploy a WebJobs project by itself, right-click the project in **Solution Explorer** and click **Publish as Azure WebJob...**.</span></span> 

![Publikovat jako webová úloha Azure](./media/websites-dotnet-deploy-webjobs/paw.png)

<span data-ttu-id="9cf30-192">Pro nezávislé webové úlohy, stejné **Publikovat Web** průvodce, který se používá pro webové projekty, ale s méně nastavení, které jsou k dispozici pro změnit.</span><span class="sxs-lookup"><span data-stu-id="9cf30-192">For an independent WebJob, the same **Publish Web** wizard that is used for web projects appears, but with fewer settings available to change.</span></span>

## <span data-ttu-id="9cf30-193"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="9cf30-193"><a id="nextsteps"></a>Next Steps</span></span>
<span data-ttu-id="9cf30-194">Tento článek obsahuje vysvětlení najdete postup nasazení webové úlohy pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9cf30-194">This article has explained how to deploy WebJobs by using Visual Studio.</span></span> <span data-ttu-id="9cf30-195">Další informace o tom, jak nasadit Azure WebJobs najdete v tématu [Azure WebJobs - doporučené prostředky - nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span><span class="sxs-lookup"><span data-stu-id="9cf30-195">For more information about how to deploy Azure WebJobs, see [Azure WebJobs - Recommended Resources - Deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span></span>

