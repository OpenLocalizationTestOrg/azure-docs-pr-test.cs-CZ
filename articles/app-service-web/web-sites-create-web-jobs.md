---
title: "úlohy na pozadí aaaRun s webové úlohy"
description: "Zjistěte, jak toorun úlohy na pozadí v Azure webové aplikace."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2016
ms.author: glenga
ms.openlocfilehash: 96a3d977a806e7192207f0f4da79dfd248694336
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-background-tasks-with-webjobs"></a><span data-ttu-id="d2901-103">Spouštění úloh na pozadí pomocí WebJobs</span><span class="sxs-lookup"><span data-stu-id="d2901-103">Run Background tasks with WebJobs</span></span>
## <a name="overview"></a><span data-ttu-id="d2901-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d2901-104">Overview</span></span>
<span data-ttu-id="d2901-105">Programy nebo skripty můžete spustit v WebJobs ve vaší [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webovou aplikaci třemi způsoby: na vyžádání, nepřetržitě, nebo podle plánu.</span><span class="sxs-lookup"><span data-stu-id="d2901-105">You can run programs or scripts in WebJobs in your [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app in three ways: on demand, continuously, or on a schedule.</span></span> <span data-ttu-id="d2901-106">Není k dispozici žádné další náklady toouse webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-106">There is no additional cost toouse WebJobs.</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="d2901-107">Tento článek ukazuje, jak toodeploy webové úlohy pomocí hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2901-107">This article shows how toodeploy WebJobs by using hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="d2901-108">Informace o tom najdete v části toodeploy pomocí sady Visual Studio nebo procesu nastavené průběžné doručování, [jak tooDeploy Azure WebJobs tooWeb aplikace](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="d2901-108">For information about how toodeploy by using Visual Studio or a continuous delivery process, see [How tooDeploy Azure WebJobs tooWeb Apps](websites-dotnet-deploy-webjobs.md).</span></span>

<span data-ttu-id="d2901-109">Hello Azure WebJobs SDK usnadňuje mnoho úloh programování webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-109">hello Azure WebJobs SDK simplifies many WebJobs programming tasks.</span></span> <span data-ttu-id="d2901-110">Další informace najdete v tématu [co je hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d2901-110">For more information, see [What is hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

 <span data-ttu-id="d2901-111">Azure Functions nabízí další způsob toorun programů a skriptů z prostředí bez serveru nebo z aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="d2901-111">Azure Functions provides another way toorun programs and scripts from either a serverless environment or from an App Service app.</span></span> <span data-ttu-id="d2901-112">Další informace najdete v tématu [přehled Azure Functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2901-112">For more information, see [Azure Functions overview](../azure-functions/functions-overview.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="d2901-113"><a name="acceptablefiles"></a>Typy souborů přijatelné pro skriptů nebo programů</span><span class="sxs-lookup"><span data-stu-id="d2901-113"><a name="acceptablefiles"></a>Acceptable file types for scripts or programs</span></span>
<span data-ttu-id="d2901-114">jsou podmínky přijaty Hello následující typy souborů:</span><span class="sxs-lookup"><span data-stu-id="d2901-114">hello following file types are accepted:</span></span>

* <span data-ttu-id="d2901-115">.bat, .cmd, .exe (pomocí windows cmd)</span><span class="sxs-lookup"><span data-stu-id="d2901-115">.cmd, .bat, .exe (using windows cmd)</span></span>
* <span data-ttu-id="d2901-116">.ps1 (pomocí prostředí powershell)</span><span class="sxs-lookup"><span data-stu-id="d2901-116">.ps1 (using powershell)</span></span>
* <span data-ttu-id="d2901-117">.SH (pomocí bash)</span><span class="sxs-lookup"><span data-stu-id="d2901-117">.sh (using bash)</span></span>
* <span data-ttu-id="d2901-118">.php (pomocí php)</span><span class="sxs-lookup"><span data-stu-id="d2901-118">.php (using php)</span></span>
* <span data-ttu-id="d2901-119">.PY (pomocí python)</span><span class="sxs-lookup"><span data-stu-id="d2901-119">.py (using python)</span></span>
* <span data-ttu-id="d2901-120">.js (pomocí uzlu)</span><span class="sxs-lookup"><span data-stu-id="d2901-120">.js (using node)</span></span>
* <span data-ttu-id="d2901-121">.JAR (využívající jazyk java)</span><span class="sxs-lookup"><span data-stu-id="d2901-121">.jar (using java)</span></span>

## <span data-ttu-id="d2901-122"><a name="CreateOnDemand"></a>Vytvoření webové úlohy na vyžádání hello portálu</span><span class="sxs-lookup"><span data-stu-id="d2901-122"><a name="CreateOnDemand"></a>Create an on demand WebJob in hello portal</span></span>
1. <span data-ttu-id="d2901-123">V hello **webové aplikace** okno hello [portálu Azure](https://portal.azure.com), klikněte na tlačítko **všechna nastavení > WebJobs** tooshow hello **WebJobs** okno.</span><span class="sxs-lookup"><span data-stu-id="d2901-123">In hello **Web App** blade of hello [Azure Portal](https://portal.azure.com), click **All settings > WebJobs** tooshow hello **WebJobs** blade.</span></span>
   
    ![Okno webové úlohy](./media/web-sites-create-web-jobs/wjblade.png)
2. <span data-ttu-id="d2901-125">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d2901-125">Click **Add**.</span></span> <span data-ttu-id="d2901-126">Hello **přidat webové úlohy** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2901-126">hello **Add WebJob** dialog appears.</span></span>
   
    ![Přidat webové úlohy okno](./media/web-sites-create-web-jobs/addwjblade.png)
3. <span data-ttu-id="d2901-128">V části **název**, zadejte název pro hello webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-128">Under **Name**, provide a name for hello WebJob.</span></span> <span data-ttu-id="d2901-129">Hello název musí začínat písmenem nebo číslem a nesmí obsahovat žádné speciální znaky jiné než "-" a "_".</span><span class="sxs-lookup"><span data-stu-id="d2901-129">hello name must start with a letter or a number and cannot contain any special characters other than "-" and "_".</span></span>
4. <span data-ttu-id="d2901-130">V hello **jak tooRun** vyberte **spouštět na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="d2901-130">In hello **How tooRun** box, choose **Run on Demand**.</span></span>
5. <span data-ttu-id="d2901-131">V hello **nahrát soubor** pole, klikněte na ikonu složky hello a procházet toohello soubor zip, který obsahuje skript.</span><span class="sxs-lookup"><span data-stu-id="d2901-131">In hello **File Upload** box, click hello folder icon and browse toohello zip file that contains your script.</span></span> <span data-ttu-id="d2901-132">soubor zip Hello by měla obsahovat vaše spustitelný soubor (.exe .cmd .bat .sh .php .py .js) a také všechny podpůrné soubory potřebné toorun hello programu nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="d2901-132">hello zip file should contain your executable (.exe .cmd .bat .sh .php .py .js) as well as any supporting files needed toorun hello program or script.</span></span>
6. <span data-ttu-id="d2901-133">Zkontrolujte **vytvořit** tooupload hello skriptu tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2901-133">Check **Create** tooupload hello script tooyour web app.</span></span> 
   
    <span data-ttu-id="d2901-134">Hello název zadaný pro hello webové úlohy se zobrazí v seznamu hello na hello **WebJobs** okno.</span><span class="sxs-lookup"><span data-stu-id="d2901-134">hello name you specified for hello WebJob appears in hello list on hello **WebJobs** blade.</span></span>
7. <span data-ttu-id="d2901-135">toorun hello webové úlohy, klikněte pravým tlačítkem na jeho název v seznamu hello a klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d2901-135">toorun hello WebJob, right-click its name in hello list and click **Run**.</span></span>
   
    ![Spuštění úlohy WebJob](./media/web-sites-create-web-jobs/runondemand.png)

## <span data-ttu-id="d2901-137"><a name="CreateContinuous"></a>Vytvoření nepřetržitě běží webové úlohy</span><span class="sxs-lookup"><span data-stu-id="d2901-137"><a name="CreateContinuous"></a>Create a continuously running WebJob</span></span>
1. <span data-ttu-id="d2901-138">toocreate nepřetržitě provádění webové úlohy, postupujte podle hello stejné kroky pro vytvoření webovou úlohu, která se spustí jednou, ale v hello **jak tooRun** vyberte **souvislý**.</span><span class="sxs-lookup"><span data-stu-id="d2901-138">toocreate a continuously executing WebJob, follow hello same steps for creating a WebJob that runs once, but in hello **How tooRun** box, choose **Continuous**.</span></span>
2. <span data-ttu-id="d2901-139">toostart nebo zastavení nepřetržitá webová úloha, klikněte pravým tlačítkem na hello webové úlohy v hello seznamu a klikněte na **spustit** nebo **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="d2901-139">toostart or stop a continuous WebJob, right-click hello WebJob in hello list and click **Start** or **Stop**.</span></span>

> [!NOTE]
> <span data-ttu-id="d2901-140">Pokud vaše webová aplikace běží na více než jednu instanci, nepřetržitě běží webové úlohy poběží na všech vaše instance.</span><span class="sxs-lookup"><span data-stu-id="d2901-140">If your web app runs on more than one instance, a continuously running WebJob will run on all of your instances.</span></span> <span data-ttu-id="d2901-141">Spustit na vyžádání a plánované webové úlohy do jedné instance vybrané pro Microsoft Azure pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d2901-141">On-demand and scheduled WebJobs run on a single instance selected for load balancing by Microsoft Azure.</span></span>
> 
> <span data-ttu-id="d2901-142">Nepřetržité webové úlohy toorun spolehlivé a na všech instancích povolit hello Always On * nastavení konfigurace pro hello webovou aplikaci jinak se může zastavit spuštěn, když hello SCM hostitele webu bylo nečinné příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="d2901-142">For Continuous WebJobs toorun reliably and on all instances, enable hello Always On* configuration setting for hello web app otherwise they can stop running when hello SCM host site has been idle for too long.</span></span>
> 
> 

## <span data-ttu-id="d2901-143"><a name="CreateScheduledCRON"></a>Vytvořte naplánovanou webovou úlohu pomocí výrazu CRON</span><span class="sxs-lookup"><span data-stu-id="d2901-143"><a name="CreateScheduledCRON"></a>Create a scheduled WebJob using a CRON expression</span></span>
<span data-ttu-id="d2901-144">Tento postup je k dispozici tooWeb aplikace spuštěna v režimu Basic, Standard nebo Premium a vyžaduje hello **Always On** nastavení toobe na hello aplikace povolena.</span><span class="sxs-lookup"><span data-stu-id="d2901-144">This technique is available tooWeb Apps running in Basic, Standard or Premium mode, and requires hello **Always On** setting toobe enabled on hello app.</span></span>

<span data-ttu-id="d2901-145">tooturn na vyžádání webové úlohy do plánované webové úlohy, jednoduše zahrnout `settings.job` souboru v kořenovém hello souboru zip webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-145">tooturn an On Demand WebJob into a scheduled WebJob, simply include a `settings.job` file at hello root of your WebJob zip file.</span></span> <span data-ttu-id="d2901-146">Tento soubor JSON by měla obsahovat `schedule` vlastnost s [výraz CRON](https://en.wikipedia.org/wiki/Cron), na následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="d2901-146">This JSON file should include a `schedule` property with a [CRON expression](https://en.wikipedia.org/wiki/Cron), per example below.</span></span>

<span data-ttu-id="d2901-147">Hello výraz CRON se skládá z pole 6: `{second} {minute} {hour} {day} {month} {day of hello week}`.</span><span class="sxs-lookup"><span data-stu-id="d2901-147">hello CRON expression is composed of 6 fields: `{second} {minute} {hour} {day} {month} {day of hello week}`.</span></span>

<span data-ttu-id="d2901-148">Například tootrigger vaše webová úloha každých 15 minut, vaše `settings.job` by měla mít:</span><span class="sxs-lookup"><span data-stu-id="d2901-148">For example, tootrigger your WebJob every 15 minutes, your `settings.job` would have:</span></span>

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

<span data-ttu-id="d2901-149">Další příklady plán CRON:</span><span class="sxs-lookup"><span data-stu-id="d2901-149">Other CRON schedule examples:</span></span>

* <span data-ttu-id="d2901-150">Každou hodinu (tj. pokaždé, když je 0 hello počet minut):`0 0 * * * *`</span><span class="sxs-lookup"><span data-stu-id="d2901-150">Every hour (i.e. whenever hello count of minutes is 0): `0 0 * * * *`</span></span> 
* <span data-ttu-id="d2901-151">Každou hodinu od too5 9: 00 PM:`0 0 9-17 * * *`</span><span class="sxs-lookup"><span data-stu-id="d2901-151">Every hour from 9 AM too5 PM: `0 0 9-17 * * *`</span></span> 
* <span data-ttu-id="d2901-152">V 9:30:00 každý den:`0 30 9 * * *`</span><span class="sxs-lookup"><span data-stu-id="d2901-152">At 9:30 AM every day: `0 30 9 * * *`</span></span>
* <span data-ttu-id="d2901-153">V 9:30:00 každý den v týdnu:`0 30 9 * * 1-5`</span><span class="sxs-lookup"><span data-stu-id="d2901-153">At 9:30 AM every week day: `0 30 9 * * 1-5`</span></span>

<span data-ttu-id="d2901-154">**Poznámka:**: Pokud nasazujete webovou úlohu ze sady Visual Studio, ujistěte se, že toomark vaše `settings.job` vlastnosti souboru jako kopii Pokud je novější.</span><span class="sxs-lookup"><span data-stu-id="d2901-154">**Note**: when deploying a WebJob from Visual Studio, make sure toomark your `settings.job` file properties as 'Copy if newer'.</span></span>

## <span data-ttu-id="d2901-155"><a name="CreateScheduled"></a>Vytvořte naplánovanou webovou úlohu pomocí hello Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="d2901-155"><a name="CreateScheduled"></a>Create a scheduled WebJob using hello Azure Scheduler</span></span>
<span data-ttu-id="d2901-156">Hello následující alternativní postup využívá hello Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="d2901-156">hello following alternate technique makes use of hello Azure Scheduler.</span></span> <span data-ttu-id="d2901-157">V takovém případě vaše webová úloha nemá žádné přímé znalostní báze hello plánu.</span><span class="sxs-lookup"><span data-stu-id="d2901-157">In this case, your WebJob does not have any direct knowledge of hello schedule.</span></span> <span data-ttu-id="d2901-158">Hello Azure Scheduler místo toho získá nakonfigurované tootrigger vaše webová úloha podle plánu.</span><span class="sxs-lookup"><span data-stu-id="d2901-158">Instead, hello Azure Scheduler gets configured tootrigger your WebJob on a schedule.</span></span> 

<span data-ttu-id="d2901-159">Hello portálu Azure ještě nemá hello možnost toocreate plánované webové úlohy, ale teprve po přidání této funkce můžete provést pomocí hello [portálu classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d2901-159">hello Azure Portal doesn't yet have hello ability toocreate a scheduled WebJob, but until that feature is added you can do it by using hello [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="d2901-160">V hello [portálu classic](http://manage.windowsazure.com) přejděte stránku toohello webovou úlohu a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d2901-160">In hello [classic portal](http://manage.windowsazure.com) go toohello WebJob page and click **Add**.</span></span>
2. <span data-ttu-id="d2901-161">V hello **jak tooRun** vyberte **spouštět podle plánu**.</span><span class="sxs-lookup"><span data-stu-id="d2901-161">In hello **How tooRun** box, choose **Run on a schedule**.</span></span>
   
    ![Novou naplánovanou úlohu][NewScheduledJob]
3. <span data-ttu-id="d2901-163">Zvolte hello **Scheduler oblast** pro úlohu a potom klikněte na šipku hello hello pravé dolní části hello dialogové okno tooproceed toohello další obrazovce.</span><span class="sxs-lookup"><span data-stu-id="d2901-163">Choose hello **Scheduler Region** for your job, and then click hello arrow on hello bottom right of hello dialog tooproceed toohello next screen.</span></span>
4. <span data-ttu-id="d2901-164">V hello **vytvoření úlohy** dialogovém okně, vyberte typ hello **opakování** chcete: **jednorázové úlohy** nebo **opakovaná úlohy**.</span><span class="sxs-lookup"><span data-stu-id="d2901-164">In hello **Create Job** dialog, choose hello type of **Recurrence** you want: **One-time job** or **Recurring job**.</span></span>
   
    ![Plán opakování][SchdRecurrence]
5. <span data-ttu-id="d2901-166">Také vyberte **počáteční** čas: **nyní** nebo **v určitém čase**.</span><span class="sxs-lookup"><span data-stu-id="d2901-166">Also choose a **Starting** time: **Now** or **At a specific time**.</span></span>
   
    ![Počáteční čas plánu][SchdStart]
6. <span data-ttu-id="d2901-168">Pokud chcete toostart v konkrétním čase, zvolte vaše počáteční čas hodnoty v části **spouštění na**.</span><span class="sxs-lookup"><span data-stu-id="d2901-168">If you want toostart at a specific time, choose your starting time values under **Starting On**.</span></span>
   
    ![Plán spuštění v určitém čase][SchdStartOn]
7. <span data-ttu-id="d2901-170">Pokud jste zvolili opakování úlohy, máte hello **opakování každých** možnost toospecify hello četnost výskytu a hello **končící na** toospecify možnost koncový čas.</span><span class="sxs-lookup"><span data-stu-id="d2901-170">If you chose a recurring job, you have hello **Recur Every** option toospecify hello frequency of occurrence and hello **Ending On** option toospecify an ending time.</span></span>
   
    ![Plán opakování][SchdRecurEvery]
8. <span data-ttu-id="d2901-172">Pokud se rozhodnete **týdny**, můžete vybrat hello **na konkrétní plán** a zadejte hello dny v týdnu hello, který chcete hello toorun úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-172">If you choose **Weeks**, you can select hello **On a Particular Schedule** box and specify hello days of hello week that you want hello job toorun.</span></span>
   
    ![Plán dny v týdnu hello][SchdWeeksOnParticular]
9. <span data-ttu-id="d2901-174">Pokud se rozhodnete **měsíců** a vyberte hello **na konkrétní plán** pole toorun hello úlohu můžete nastavit na konkrétní číslované **dní** v měsíci hello.</span><span class="sxs-lookup"><span data-stu-id="d2901-174">If you choose **Months** and select hello **On a Particular Schedule** box, you can set hello job toorun on particular numbered **Days** in hello month.</span></span> 
   
    ![Naplánovat konkrétní kalendářní data v hello měsíc][SchdMonthsOnPartDays]
10. <span data-ttu-id="d2901-176">Pokud se rozhodnete **dny v týdnu**, které den nebo dny v týdnu hello můžete vybrat v měsíci hello chcete hello toorun úlohy na.</span><span class="sxs-lookup"><span data-stu-id="d2901-176">If you choose **Week Days**, you can select which day or days of hello week in hello month you want hello job toorun on.</span></span>
    
     ![Naplánovat dny v týdnu konkrétní za měsíc][SchdMonthsOnPartWeekDays]
11. <span data-ttu-id="d2901-178">Nakonec můžete také použít hello **výskytů** možnost toochoose kterého týdne v měsíci hello (nejprve druhé, třetí atd.) chcete hello toorun úlohy na hello dny v týdnu jste zadali.</span><span class="sxs-lookup"><span data-stu-id="d2901-178">Finally, you can also use hello **Occurrences** option toochoose which week in hello month (first, second, third etc.) you want hello job toorun on hello week days you specified.</span></span>
    
    ![Naplánovat dny v týdnu konkrétní na konkrétní týdny v měsíci][SchdMonthsOnPartWeekDaysOccurences]
12. <span data-ttu-id="d2901-180">Po vytvoření jednoho nebo více úloh, jejich názvy se zobrazí na kartě WebJobs hello s jejich stav, typ plánu a další informace.</span><span class="sxs-lookup"><span data-stu-id="d2901-180">After you have created one or more jobs, their names will appear on hello WebJobs tab with their status, schedule type, and other information.</span></span> <span data-ttu-id="d2901-181">Historické informace se udržuje pro hello poslední 30 webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-181">Historical information for hello last 30  WebJobs is maintained.</span></span>
    
    ![Seznam úloh][WebJobsListWithSeveralJobs]

### <span data-ttu-id="d2901-183"><a name="Scheduler"></a>Naplánované úlohy a Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="d2901-183"><a name="Scheduler"></a>Scheduled jobs and Azure Scheduler</span></span>
<span data-ttu-id="d2901-184">Naplánované úlohy další konfiguraci hello Azure Scheduler stránkách hello [portálu classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d2901-184">Scheduled jobs can be further configured in hello Azure Scheduler pages of hello [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="d2901-185">Na stránce hello webové úlohy, klikněte na úlohu hello **plán** odkaz toonavigate toohello Azure Scheduler portálu stránky.</span><span class="sxs-lookup"><span data-stu-id="d2901-185">On hello WebJobs page, click hello job's **schedule** link toonavigate toohello Azure Scheduler portal page.</span></span> 
   
   ![Odkaz tooAzure plánovače][LinkToScheduler]
2. <span data-ttu-id="d2901-187">Na stránce Scheduler hello klikněte na úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="d2901-187">On hello Scheduler page, click hello job.</span></span>
   
    ![Úlohy na stránky portálu hello plánovače][SchedulerPortal]
3. <span data-ttu-id="d2901-189">Hello **akce úlohy** otevře, kde lze dále konfigurovat úlohy hello se stránka.</span><span class="sxs-lookup"><span data-stu-id="d2901-189">hello **Job Action** page opens, where you can further configure hello job.</span></span> 
   
    ![Akce úlohy PageInScheduler][JobActionPageInScheduler]

## <span data-ttu-id="d2901-191"><a name="ViewJobHistory"></a>Zobrazení historie úlohy hello</span><span class="sxs-lookup"><span data-stu-id="d2901-191"><a name="ViewJobHistory"></a>View hello job history</span></span>
1. <span data-ttu-id="d2901-192">tooview hello historie provádění úlohy, včetně úloh vytvořených s hello WebJobs SDK, klikněte na příslušný odkaz v části hello **protokoly** sloupec hello okna webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-192">tooview hello execution history of a job, including jobs created with hello WebJobs SDK, click  its corresponding link under hello **Logs** column of hello WebJobs blade.</span></span> <span data-ttu-id="d2901-193">(Hello schránky ikonu toocopy hello URL hello protokolu souboru stránky toohello schránky můžete použít, pokud chcete.)</span><span class="sxs-lookup"><span data-stu-id="d2901-193">(You can use hello clipboard icon toocopy hello URL of hello log file page toohello clipboard if you wish.)</span></span>
   
    ![Protokoly odkaz](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. <span data-ttu-id="d2901-195">Kliknutím na odkaz hello otevře stránka Podrobnosti hello hello webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-195">Clicking hello link opens hello details page for hello WebJob.</span></span> <span data-ttu-id="d2901-196">Tato stránka obsahuje hello název hello spustit příkaz, hello poslední kolikrát byl spuštěn a jeho úspěch nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="d2901-196">This page shows you hello name of hello command run, hello last times it ran, and its success or failure.</span></span> <span data-ttu-id="d2901-197">V části **poslední úloha spouští**, klikněte na tlačítko Další podrobnosti toosee čas.</span><span class="sxs-lookup"><span data-stu-id="d2901-197">Under **Recent job runs**, click a time toosee further details.</span></span>
   
    ![WebJobDetails][WebJobDetails]
3. <span data-ttu-id="d2901-199">Hello **podrobnosti o spuštění úlohy WebJob** se zobrazí stránka.</span><span class="sxs-lookup"><span data-stu-id="d2901-199">hello **WebJob Run Details** page appears.</span></span> <span data-ttu-id="d2901-200">Klikněte na tlačítko **přepnutí výstup** textu hello toosee obsah protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="d2901-200">Click **Toggle Output** toosee hello text of hello log contents.</span></span> <span data-ttu-id="d2901-201">protokol výstup Hello je ve formátu textu.</span><span class="sxs-lookup"><span data-stu-id="d2901-201">hello output log is in text format.</span></span> 
   
    ![Webovou úlohu podrobnosti o spuštění][WebJobRunDetails]
4. <span data-ttu-id="d2901-203">textový výstup hello toosee v samostatném okně prohlížeče, klikněte na tlačítko hello **Stáhnout** odkaz.</span><span class="sxs-lookup"><span data-stu-id="d2901-203">toosee hello output text in a separate browser window, click hello **download** link.</span></span> <span data-ttu-id="d2901-204">vlastní text hello toodownload klikněte pravým tlačítkem na odkaz hello a používat obsah souboru toosave hello váš prohlížeč možnosti.</span><span class="sxs-lookup"><span data-stu-id="d2901-204">toodownload hello text itself, right-click hello link and use your browser options toosave hello file contents.</span></span>
   
    ![Stažení výstupu protokolu][DownloadLogOutput]
5. <span data-ttu-id="d2901-206">Hello **WebJobs** odkaz hello horní části stránky hello poskytuje pohodlný způsob tooget tooa seznam webové úlohy na řídicím panelu historii hello.</span><span class="sxs-lookup"><span data-stu-id="d2901-206">hello **WebJobs** link at hello top of hello page provides a convenient way tooget tooa list of WebJobs on hello history dashboard.</span></span>
   
    ![Odkaz tooWebJobs seznamu][WebJobsLinkToDashboardList]
   
    ![Seznam webové úlohy na řídicím panelu historii][WebJobsListInJobsDashboard]
   
    <span data-ttu-id="d2901-209">Kliknutím na jedno z těchto odkazů přejdete stránce s podrobnostmi o webové úlohy toohello hello úlohy, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="d2901-209">Clicking one of these links takes you toohello WebJob Details page for hello job you selected.</span></span>

## <span data-ttu-id="d2901-210"><a name="WHPNotes"></a>Poznámky k</span><span class="sxs-lookup"><span data-stu-id="d2901-210"><a name="WHPNotes"></a>Notes</span></span>
* <span data-ttu-id="d2901-211">Webové aplikace v režimu volné můžete po 20 minutách vypršení časového limitu, pokud neexistují žádná lokalita požadavky toohello scm (nasazení) a portál hello webové aplikace není otevřen v Azure.</span><span class="sxs-lookup"><span data-stu-id="d2901-211">Web apps in Free mode can time out after 20 minutes if there are no requests toohello scm (deployment) site and hello web app's portal is not open in Azure.</span></span> <span data-ttu-id="d2901-212">Požadavky toohello lokality nebude resetovat to.</span><span class="sxs-lookup"><span data-stu-id="d2901-212">Requests toohello actual site will not reset this.</span></span>
* <span data-ttu-id="d2901-213">Kód pro úlohu průběžné musí toobe zapsána toorun v nekonečné smyčce.</span><span class="sxs-lookup"><span data-stu-id="d2901-213">Code for a continuous job needs toobe written toorun in an endless loop.</span></span>
* <span data-ttu-id="d2901-214">Nepřetržité úlohy spouštět nepřetržitě jenom v případě, že je hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2901-214">Continuous jobs run continuously only when hello web app is up.</span></span>
* <span data-ttu-id="d2901-215">Základní a standardní režimy nabídka hello vždy na funkce, které, když je povolené, zabraňuje webové aplikace stal nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="d2901-215">Basic and Standard modes offer hello Always On feature which, when enabled, prevents web apps from becoming idle.</span></span>
* <span data-ttu-id="d2901-216">Pouze můžete ladit průběžně běží webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2901-216">You can only debug continuously running WebJobs.</span></span> <span data-ttu-id="d2901-217">Ladění webové úlohy na vyžádání nebo naplánované není podporováno.</span><span class="sxs-lookup"><span data-stu-id="d2901-217">Debugging scheduled or on-demand WebJobs is not supported.</span></span>

## <span data-ttu-id="d2901-218"><a name="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2901-218"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="d2901-219">Další informace najdete v tématu [Azure WebJobs doporučené prostředky][WebJobsRecommendedResources].</span><span class="sxs-lookup"><span data-stu-id="d2901-219">For more information, see [Azure WebJobs Recommended Resources][WebJobsRecommendedResources].</span></span>

[PSonWebJobs]:http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx
[WebJobsRecommendedResources]:http://go.microsoft.com/fwlink/?LinkId=390226

[OnDemandWebJob]: ./media/web-sites-create-web-jobs/01aOnDemandWebJob.png
[WebJobsList]: ./media/web-sites-create-web-jobs/02aWebJobsList.png
[NewContinuousJob]: ./media/web-sites-create-web-jobs/03aNewContinuousJob.png
[NewScheduledJob]: ./media/web-sites-create-web-jobs/04aNewScheduledJob.png
[SchdRecurrence]: ./media/web-sites-create-web-jobs/05SchdRecurrence.png
[SchdStart]: ./media/web-sites-create-web-jobs/06SchdStart.png
[SchdStartOn]: ./media/web-sites-create-web-jobs/07SchdStartOn.png
[SchdRecurEvery]: ./media/web-sites-create-web-jobs/08SchdRecurEvery.png
[SchdWeeksOnParticular]: ./media/web-sites-create-web-jobs/09SchdWeeksOnParticular.png
[SchdMonthsOnPartDays]: ./media/web-sites-create-web-jobs/10SchdMonthsOnPartDays.png
[SchdMonthsOnPartWeekDays]: ./media/web-sites-create-web-jobs/11SchdMonthsOnPartWeekDays.png
[SchdMonthsOnPartWeekDaysOccurences]: ./media/web-sites-create-web-jobs/12SchdMonthsOnPartWeekDaysOccurences.png
[RunOnce]: ./media/web-sites-create-web-jobs/13RunOnce.png
[WebJobsListWithSeveralJobs]: ./media/web-sites-create-web-jobs/13WebJobsListWithSeveralJobs.png
[WebJobLogs]: ./media/web-sites-create-web-jobs/14WebJobLogs.png
[WebJobDetails]: ./media/web-sites-create-web-jobs/15WebJobDetails.png
[WebJobRunDetails]: ./media/web-sites-create-web-jobs/16WebJobRunDetails.png
[DownloadLogOutput]: ./media/web-sites-create-web-jobs/17DownloadLogOutput.png
[WebJobsLinkToDashboardList]: ./media/web-sites-create-web-jobs/18WebJobsLinkToDashboardList.png
[WebJobsListInJobsDashboard]: ./media/web-sites-create-web-jobs/19WebJobsListInJobsDashboard.png
[LinkToScheduler]: ./media/web-sites-create-web-jobs/31LinkToScheduler.png
[SchedulerPortal]: ./media/web-sites-create-web-jobs/32SchedulerPortal.png
[JobActionPageInScheduler]: ./media/web-sites-create-web-jobs/33JobActionPageInScheduler.png

