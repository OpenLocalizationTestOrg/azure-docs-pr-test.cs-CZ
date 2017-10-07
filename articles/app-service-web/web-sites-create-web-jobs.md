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
# <a name="run-background-tasks-with-webjobs"></a>Spouštění úloh na pozadí pomocí WebJobs
## <a name="overview"></a>Přehled
Programy nebo skripty můžete spustit v WebJobs ve vaší [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webovou aplikaci třemi způsoby: na vyžádání, nepřetržitě, nebo podle plánu. Není k dispozici žádné další náklady toouse webové úlohy.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Tento článek ukazuje, jak toodeploy webové úlohy pomocí hello [portálu Azure](https://portal.azure.com). Informace o tom najdete v části toodeploy pomocí sady Visual Studio nebo procesu nastavené průběžné doručování, [jak tooDeploy Azure WebJobs tooWeb aplikace](websites-dotnet-deploy-webjobs.md).

Hello Azure WebJobs SDK usnadňuje mnoho úloh programování webové úlohy. Další informace najdete v tématu [co je hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).

 Azure Functions nabízí další způsob toorun programů a skriptů z prostředí bez serveru nebo z aplikace služby App Service. Další informace najdete v tématu [přehled Azure Functions](../azure-functions/functions-overview.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="acceptablefiles"></a>Typy souborů přijatelné pro skriptů nebo programů
jsou podmínky přijaty Hello následující typy souborů:

* .bat, .cmd, .exe (pomocí windows cmd)
* .ps1 (pomocí prostředí powershell)
* .SH (pomocí bash)
* .php (pomocí php)
* .PY (pomocí python)
* .js (pomocí uzlu)
* .JAR (využívající jazyk java)

## <a name="CreateOnDemand"></a>Vytvoření webové úlohy na vyžádání hello portálu
1. V hello **webové aplikace** okno hello [portálu Azure](https://portal.azure.com), klikněte na tlačítko **všechna nastavení > WebJobs** tooshow hello **WebJobs** okno.
   
    ![Okno webové úlohy](./media/web-sites-create-web-jobs/wjblade.png)
2. Klikněte na tlačítko **Přidat**. Hello **přidat webové úlohy** otevře se dialogové okno.
   
    ![Přidat webové úlohy okno](./media/web-sites-create-web-jobs/addwjblade.png)
3. V části **název**, zadejte název pro hello webové úlohy. Hello název musí začínat písmenem nebo číslem a nesmí obsahovat žádné speciální znaky jiné než "-" a "_".
4. V hello **jak tooRun** vyberte **spouštět na vyžádání**.
5. V hello **nahrát soubor** pole, klikněte na ikonu složky hello a procházet toohello soubor zip, který obsahuje skript. soubor zip Hello by měla obsahovat vaše spustitelný soubor (.exe .cmd .bat .sh .php .py .js) a také všechny podpůrné soubory potřebné toorun hello programu nebo skriptu.
6. Zkontrolujte **vytvořit** tooupload hello skriptu tooyour webové aplikace. 
   
    Hello název zadaný pro hello webové úlohy se zobrazí v seznamu hello na hello **WebJobs** okno.
7. toorun hello webové úlohy, klikněte pravým tlačítkem na jeho název v seznamu hello a klikněte na **spustit**.
   
    ![Spuštění úlohy WebJob](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>Vytvoření nepřetržitě běží webové úlohy
1. toocreate nepřetržitě provádění webové úlohy, postupujte podle hello stejné kroky pro vytvoření webovou úlohu, která se spustí jednou, ale v hello **jak tooRun** vyberte **souvislý**.
2. toostart nebo zastavení nepřetržitá webová úloha, klikněte pravým tlačítkem na hello webové úlohy v hello seznamu a klikněte na **spustit** nebo **Zastavit**.

> [!NOTE]
> Pokud vaše webová aplikace běží na více než jednu instanci, nepřetržitě běží webové úlohy poběží na všech vaše instance. Spustit na vyžádání a plánované webové úlohy do jedné instance vybrané pro Microsoft Azure pro vyrovnávání zatížení.
> 
> Nepřetržité webové úlohy toorun spolehlivé a na všech instancích povolit hello Always On * nastavení konfigurace pro hello webovou aplikaci jinak se může zastavit spuštěn, když hello SCM hostitele webu bylo nečinné příliš dlouho.
> 
> 

## <a name="CreateScheduledCRON"></a>Vytvořte naplánovanou webovou úlohu pomocí výrazu CRON
Tento postup je k dispozici tooWeb aplikace spuštěna v režimu Basic, Standard nebo Premium a vyžaduje hello **Always On** nastavení toobe na hello aplikace povolena.

tooturn na vyžádání webové úlohy do plánované webové úlohy, jednoduše zahrnout `settings.job` souboru v kořenovém hello souboru zip webové úlohy. Tento soubor JSON by měla obsahovat `schedule` vlastnost s [výraz CRON](https://en.wikipedia.org/wiki/Cron), na následujícím příkladu.

Hello výraz CRON se skládá z pole 6: `{second} {minute} {hour} {day} {month} {day of hello week}`.

Například tootrigger vaše webová úloha každých 15 minut, vaše `settings.job` by měla mít:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

Další příklady plán CRON:

* Každou hodinu (tj. pokaždé, když je 0 hello počet minut):`0 0 * * * *` 
* Každou hodinu od too5 9: 00 PM:`0 0 9-17 * * *` 
* V 9:30:00 každý den:`0 30 9 * * *`
* V 9:30:00 každý den v týdnu:`0 30 9 * * 1-5`

**Poznámka:**: Pokud nasazujete webovou úlohu ze sady Visual Studio, ujistěte se, že toomark vaše `settings.job` vlastnosti souboru jako kopii Pokud je novější.

## <a name="CreateScheduled"></a>Vytvořte naplánovanou webovou úlohu pomocí hello Azure Scheduler
Hello následující alternativní postup využívá hello Azure Scheduler. V takovém případě vaše webová úloha nemá žádné přímé znalostní báze hello plánu. Hello Azure Scheduler místo toho získá nakonfigurované tootrigger vaše webová úloha podle plánu. 

Hello portálu Azure ještě nemá hello možnost toocreate plánované webové úlohy, ale teprve po přidání této funkce můžete provést pomocí hello [portálu classic](http://manage.windowsazure.com).

1. V hello [portálu classic](http://manage.windowsazure.com) přejděte stránku toohello webovou úlohu a klikněte na tlačítko **přidat**.
2. V hello **jak tooRun** vyberte **spouštět podle plánu**.
   
    ![Novou naplánovanou úlohu][NewScheduledJob]
3. Zvolte hello **Scheduler oblast** pro úlohu a potom klikněte na šipku hello hello pravé dolní části hello dialogové okno tooproceed toohello další obrazovce.
4. V hello **vytvoření úlohy** dialogovém okně, vyberte typ hello **opakování** chcete: **jednorázové úlohy** nebo **opakovaná úlohy**.
   
    ![Plán opakování][SchdRecurrence]
5. Také vyberte **počáteční** čas: **nyní** nebo **v určitém čase**.
   
    ![Počáteční čas plánu][SchdStart]
6. Pokud chcete toostart v konkrétním čase, zvolte vaše počáteční čas hodnoty v části **spouštění na**.
   
    ![Plán spuštění v určitém čase][SchdStartOn]
7. Pokud jste zvolili opakování úlohy, máte hello **opakování každých** možnost toospecify hello četnost výskytu a hello **končící na** toospecify možnost koncový čas.
   
    ![Plán opakování][SchdRecurEvery]
8. Pokud se rozhodnete **týdny**, můžete vybrat hello **na konkrétní plán** a zadejte hello dny v týdnu hello, který chcete hello toorun úlohy.
   
    ![Plán dny v týdnu hello][SchdWeeksOnParticular]
9. Pokud se rozhodnete **měsíců** a vyberte hello **na konkrétní plán** pole toorun hello úlohu můžete nastavit na konkrétní číslované **dní** v měsíci hello. 
   
    ![Naplánovat konkrétní kalendářní data v hello měsíc][SchdMonthsOnPartDays]
10. Pokud se rozhodnete **dny v týdnu**, které den nebo dny v týdnu hello můžete vybrat v měsíci hello chcete hello toorun úlohy na.
    
     ![Naplánovat dny v týdnu konkrétní za měsíc][SchdMonthsOnPartWeekDays]
11. Nakonec můžete také použít hello **výskytů** možnost toochoose kterého týdne v měsíci hello (nejprve druhé, třetí atd.) chcete hello toorun úlohy na hello dny v týdnu jste zadali.
    
    ![Naplánovat dny v týdnu konkrétní na konkrétní týdny v měsíci][SchdMonthsOnPartWeekDaysOccurences]
12. Po vytvoření jednoho nebo více úloh, jejich názvy se zobrazí na kartě WebJobs hello s jejich stav, typ plánu a další informace. Historické informace se udržuje pro hello poslední 30 webové úlohy.
    
    ![Seznam úloh][WebJobsListWithSeveralJobs]

### <a name="Scheduler"></a>Naplánované úlohy a Azure Scheduler
Naplánované úlohy další konfiguraci hello Azure Scheduler stránkách hello [portálu classic](http://manage.windowsazure.com).

1. Na stránce hello webové úlohy, klikněte na úlohu hello **plán** odkaz toonavigate toohello Azure Scheduler portálu stránky. 
   
   ![Odkaz tooAzure plánovače][LinkToScheduler]
2. Na stránce Scheduler hello klikněte na úlohu hello.
   
    ![Úlohy na stránky portálu hello plánovače][SchedulerPortal]
3. Hello **akce úlohy** otevře, kde lze dále konfigurovat úlohy hello se stránka. 
   
    ![Akce úlohy PageInScheduler][JobActionPageInScheduler]

## <a name="ViewJobHistory"></a>Zobrazení historie úlohy hello
1. tooview hello historie provádění úlohy, včetně úloh vytvořených s hello WebJobs SDK, klikněte na příslušný odkaz v části hello **protokoly** sloupec hello okna webové úlohy. (Hello schránky ikonu toocopy hello URL hello protokolu souboru stránky toohello schránky můžete použít, pokud chcete.)
   
    ![Protokoly odkaz](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. Kliknutím na odkaz hello otevře stránka Podrobnosti hello hello webové úlohy. Tato stránka obsahuje hello název hello spustit příkaz, hello poslední kolikrát byl spuštěn a jeho úspěch nebo selhání. V části **poslední úloha spouští**, klikněte na tlačítko Další podrobnosti toosee čas.
   
    ![WebJobDetails][WebJobDetails]
3. Hello **podrobnosti o spuštění úlohy WebJob** se zobrazí stránka. Klikněte na tlačítko **přepnutí výstup** textu hello toosee obsah protokolu hello. protokol výstup Hello je ve formátu textu. 
   
    ![Webovou úlohu podrobnosti o spuštění][WebJobRunDetails]
4. textový výstup hello toosee v samostatném okně prohlížeče, klikněte na tlačítko hello **Stáhnout** odkaz. vlastní text hello toodownload klikněte pravým tlačítkem na odkaz hello a používat obsah souboru toosave hello váš prohlížeč možnosti.
   
    ![Stažení výstupu protokolu][DownloadLogOutput]
5. Hello **WebJobs** odkaz hello horní části stránky hello poskytuje pohodlný způsob tooget tooa seznam webové úlohy na řídicím panelu historii hello.
   
    ![Odkaz tooWebJobs seznamu][WebJobsLinkToDashboardList]
   
    ![Seznam webové úlohy na řídicím panelu historii][WebJobsListInJobsDashboard]
   
    Kliknutím na jedno z těchto odkazů přejdete stránce s podrobnostmi o webové úlohy toohello hello úlohy, které jste vybrali.

## <a name="WHPNotes"></a>Poznámky k
* Webové aplikace v režimu volné můžete po 20 minutách vypršení časového limitu, pokud neexistují žádná lokalita požadavky toohello scm (nasazení) a portál hello webové aplikace není otevřen v Azure. Požadavky toohello lokality nebude resetovat to.
* Kód pro úlohu průběžné musí toobe zapsána toorun v nekonečné smyčce.
* Nepřetržité úlohy spouštět nepřetržitě jenom v případě, že je hello webové aplikace.
* Základní a standardní režimy nabídka hello vždy na funkce, které, když je povolené, zabraňuje webové aplikace stal nečinnosti.
* Pouze můžete ladit průběžně běží webové úlohy. Ladění webové úlohy na vyžádání nebo naplánované není podporováno.

## <a name="NextSteps"></a>Další kroky
Další informace najdete v tématu [Azure WebJobs doporučené prostředky][WebJobsRecommendedResources].

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

