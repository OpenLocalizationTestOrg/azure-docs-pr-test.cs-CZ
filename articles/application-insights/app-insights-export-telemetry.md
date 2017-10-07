---
title: aaaContinuous export telemetrie z Application Insights | Microsoft Docs
description: "Export dat toostorage diagnostiky a použití ve službě Microsoft Azure a stažení z ní."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: bwren
ms.openlocfilehash: be9ed7e05922c1c8186df9ca4e642862adaa5fd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-telemetry-from-application-insights"></a>Export telemetrie z Application Insights
Chcete tookeep telemetrie po dobu delší než doba uchování standardní hello? Nebo ji zpracovat nějakým způsobem specializované? Průběžné Export je ideální pro tento. Hello události, které vidíte na portálu služby Application Insights hello může být exportovaný toostorage v Microsoft Azure ve formátu JSON. Odtud můžete stáhnout vaše data a zápis, ať kódu můžete potřebovat tooprocess ho.  

Pomocí průběžné exportovat mohou být účtovány dalších poplatků. Zkontrolujte vaše [cenový model](http://azure.microsoft.com/pricing/details/application-insights/).

Před nastavením průběžné export neexistují nějaké alternativy, můžete chtít tooconsider:

* tlačítko Exportovat Hello hello horní části okna metriky nebo vyhledávání umožňuje přenos tabulku aplikace Excel tooan tabulky a grafy.

* [Analýza](app-insights-analytics.md) poskytuje účinný dotazovací jazyk pro telemetrie. Je také možné exportovat výsledky.
* Pokud se díváte příliš[zkoumat data v Power BI](app-insights-export-power-bi.md), můžete to udělat bez použití průběžné exportovat.
* Hello [REST API přístup k datům](https://dev.applicationinsights.io/) vám umožní přístup k vaší telemetrie prostřednictvím kódu programu.

Po průběžné exportovat kopie vaše data toostorage (kde může zůstat pro, dokud se vám líbí), bude stále k dispozici ve službě Application Insights hello obvyklé [dobu uchování](app-insights-data-retention-privacy.md).

## <a name="setup"></a>Vytvoření průběžné exportu
1. V hello prostředek Application Insights pro aplikace, otevřete průběžné exportovat a zvolte **přidat**:

    ![Posuňte se dolů a klikněte na tlačítko průběžné Export](./media/app-insights-export-telemetry/01-export.png)

2. Zvolte hello telemetrie datové typy chcete tooexport.

3. Vytvořte nebo vyberte [účtu úložiště Azure](../storage/common/storage-introduction.md) místo toostore hello data.

    > [!Warning]
    > Ve výchozím nastavení, bude umístění úložiště hello nastavena toohello stejné zeměpisné oblasti jako prostředek Application Insights. Pokud uchováváte v jiné oblasti, mohou být účtovány poplatky za přenos.

    ![Klikněte na tlačítko Přidat, Export cílový účet úložiště a vytvořit nové úložiště nebo vyberte stávající úložiště](./media/app-insights-export-telemetry/02-add.png)

4. Vytvořte nebo vyberte kontejner v úložišti hello:

    ![Klikněte na tlačítko Vybrat typy událostí](./media/app-insights-export-telemetry/create-container.png)

Po vytvoření export, začne přejdete. Zobrazí jenom data, která dorazí po vytvoření hello export.

Může být zpoždění o jednu hodinu, než se data zobrazí v úložišti hello.

### <a name="tooedit-continuous-export"></a>průběžné export tooedit

Pokud chcete typů událostí toochange hello později, upravte hello export:

![Klikněte na tlačítko Vybrat typy událostí](./media/app-insights-export-telemetry/05-edit.png)

### <a name="toostop-continuous-export"></a>průběžné export toostop

toostop hello export, klikněte na tlačítko zakázat. Když kliknete znovu povolit, hello export restartuje se nová data. Nebude získat hello data, které byly přijaty portálu hello exportu bylo zakázáno.

toostop hello export trvale, odstraňte jej. Díky tomu nedojde k odstranění dat z úložiště.

### <a name="cant-add-or-change-an-export"></a>Nelze přidat nebo změnit exportu?
* Exportuje tooadd nebo změnit, je třeba vlastník, Přispěvatel nebo Application Insights Přispěvatel přístupová práva. [Další informace o rolích][roles].

## <a name="analyze"></a>Jaké události získáte?
Hello exportovaná data je nezpracovaná telemetrická data hello, které obdržíme z vaší aplikace, s tím rozdílem, že přidáme data o umístění, které jsme vypočítat z IP adresy klienta hello.

Data, která byla zahozena podle [vzorkování](app-insights-sampling.md) není součástí hello exportovat data.

Další počítané metriky nejsou zahrnuty. Například nemáte exportu průměrné využití procesoru, ale hello nezpracovaná telemetrická data, ze kterého hello průměr se vypočítává exportu.

Hello data také obsahují hello výsledky všech [testy dostupnosti webu](app-insights-monitor-web-app-availability.md) který jste nastavili.

> [!NOTE]
> **Vzorkování.** Pokud vaše aplikace odešle velké množství dat, hello vzorkování funkce může pracovat a odesílat pouze část hello generované telemetrie. [Přečtěte si další informace o vzorkování.](app-insights-sampling.md)
>
>

## <a name="get"></a>Zkontrolujte hello data
Si můžete prohlédnout hello úložiště přímo na portálu hello. Klikněte na tlačítko **Procházet**, vyberte svůj účet úložiště a pak otevřete **kontejnery**.

tooinspect úložiště Azure v sadě Visual Studio otevřete **zobrazení**, **Průzkumník cloudu**. (Pokud nemáte tohoto příkazu v nabídce, je třeba tooinstall hello Azure SDK: Otevřete hello **nový projekt** dialogové okno, rozbalte položku Visual C# / cloudu a zvolte **získat Microsoft Azure SDK for .NET**.)

Když otevřete váš úložišti objektů blob, uvidíte kontejner s určitou sadu souborů objektů blob. Hello URI každého souboru odvozená od vašeho název prostředku Application Insights, jeho klíč instrumentace, telemetrie – typ nebo datum a čas. (název prostředku hello je psaný malými písmeny a klíč instrumentace hello vynechá pomlčky.)

![Zkontrolujte hello úložišti objektů blob pomocí vhodného nástroje](./media/app-insights-export-telemetry/04-data.png)

Hello datum a čas UTC se a jsou při hello telemetrie byl uložen v úložišti hello – není hello čas byl vygenerován. Takže pokud napíšete kód toodownload hello data, můžete lineárně přesunout prostřednictvím hello data.

Tady je hello formu cesty hello:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

kde

* `blobCreationTimeUtc`je čas, kdy objektu blob byla vytvořena v hello interní pracovní úložiště
* `blobDeliveryTimeUtc`je čas hello po zkopírovaný toohello export cílového úložiště objektů blob

## <a name="format"></a>Formát dat
* Každý objekt blob je textový soubor, který obsahuje více ' \n'-separated řádků. Obsahuje telemetrii hello zpracovaných za časové období zhruba poloviční minuta.
* Každý řádek představuje bod telemetrická data například zobrazení požadavku nebo stránky.
* Každý řádek je neformátovaný dokument JSON. Pokud chcete toosit a stare na to, otevřete v sadě Visual Studio a zvolte upravte, Upřesnit, formát souboru:

![Zobrazení telemetrie hello pomocí vhodného nástroje](./media/app-insights-export-telemetry/06-json.png)

Dobách trvání jsou v rysky, kde 10 000 značek = 1ms. Například tyto hodnoty zobrazit čas 1ms toosend žádost od hello prohlížeče, 3ms tooreceive a 1.8s tooprocess hello stránku v prohlížeči hello:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.](app-insights-export-data-model.md)

## <a name="processing-hello-data"></a>Zpracování dat hello
V malém měřítku můžete napsat některé toopull kódu od sebe vaše data, přečtěte si ho do tabulky a tak dále. Například:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

Větší ukázky kódu, najdete v části [pomocí rolí pracovního procesu][exportasa].

## <a name="delete"></a>Odstranit stará data
Upozorňujeme, že jste zodpovědní za správu vaše kapacita úložiště a odstraňování starých dat hello v případě potřeby.

## <a name="if-you-regenerate-your-storage-key"></a>Pokud byste znovu generovali klíče účtu úložiště...
Pokud změníte hello klíče tooyour úložiště, průběžné export přestanou fungovat. Uvidíte oznámení v účtu Azure.

Otevřete okno hello průběžné exportovat a upravovat export. Upravit hello exportovat cílové, ale nechte hello vybrané stejné úložiště. Klikněte na tlačítko OK tooconfirm.

![Upravit hello průběžné exportovat, otevírají a zavírají thí cíl exportu.](./media/app-insights-export-telemetry/07-resetstore.png)

průběžné export Hello se restartuje.

## <a name="export-samples"></a>Export – ukázky

* [Export tooSQL pomocí služby Stream Analytics][exportasa]
* [Stream Analytics ukázka 2](app-insights-export-stream-analytics.md)

Na větších měřítek, vezměte v úvahu [HDInsight](https://azure.microsoft.com/services/hdinsight/) -clusterů systému Hadoop v cloudu hello. HDInsight nabízí celou řadu technologií pro správu a analýze velkých objemů dat, a můžete ji použít tooprocess data, která byla exportována z Application Insights.

## <a name="q--a"></a>Dotazy a odpovědi
* *Ale všechny, které mají být je jednorázové stažení grafu.*  

    Ano, můžete to udělat. V horní části hello hello okna, klikněte na tlačítko **Export dat**.
* *Mám nastavit exportu, ale nejsou žádná data v tomto úložišti.*

    Application Insights zobrazila všechny telemetrie z vaší aplikace. vzhledem k tomu, že nastavíte hello exportu? Obdržíte jenom nová data.
* *I pokusili tooset nahoru exportu, ale byl odepřen přístup*

    Pokud vaše organizace vlastní účet hello, máte toobe členem skupiny hello vlastníky nebo přispěvatele.
* *Můžete exportovat přímých toomy vlastní místní úložiště?*

    Ne, je nám líto. Naše export modul v současné době funkční s Azure storage v tuto chvíli.  
* *Je k dispozici žádné omezení toohello množství dat, která jste uložili v tomto úložišti?*

    Ne. Jsme budete zachovat předání dat dokud neodstraníte hello export. Pokud jsme dosáhl hello vnější limity pro úložiště objektů blob, ale který je poměrně velký budete zastavení. Je to tooyou toocontrol jak velké úložiště používáte.  
* *Kolik objekty BLOB může zobrazit v úložišti hello?*

  * Pro každý datový typ vybraného tooexport, nový objekt blob se vytvoří každou minutu (pokud jsou k dispozici data).
  * Pro aplikace s intenzivní provoz, navíc další oddíl jednotky jsou přiděleny. V takovém případě jednotlivých jednotek vytvoří objekt blob každou minutu.
* *I znovu vygenerovat hello klíče toomy úložiště, nebo změnit název hello hello kontejneru a nyní hello export nefunguje.*

    Upravit hello exportu a otevřete okno cílové export hello. Nechte hello stejné úložiště jako předtím vybrali a klikněte na OK tooconfirm. Export se restartuje. Pokud hello změnu byla v rámci hello po několik dní, abyste neztratili data.
* *Můžete pozastavit hello exportu?*

    Ano. Klikněte na tlačítko zakázat.

## <a name="code-samples"></a>Ukázky kódů

* [Ukázka Stream Analytics](app-insights-export-stream-analytics.md)
* [Export tooSQL pomocí služby Stream Analytics][exportasa]
* [Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
