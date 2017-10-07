---
title: "aaaCreate více modely z jednoho experimentu | Microsoft Docs"
description: "Pomocí prostředí PowerShell toocreate více modelů Machine Learning a webové služby koncové body pomocí hello stejný algoritmus ale jiné školení datové sady."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 4a258a8ab26395d4169a058520151c860e16e169
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Vytvoření mnoha modelů Machine Learning a koncových bodů webové služby z jednoho experimentu pomocí prostředí PowerShell
Zde problém je běžný, machine learning: Chcete toocreate mnoho modely, které mají hello stejný pracovní postup školení a použití hello stejný algoritmus, ale mají různé školení datové sady jako vstup. Tento článek ukazuje, jak toodo to škálované v Azure Machine Learning Studio pomocí právě jeden experimentu.

Řekněme například, že vlastníte firma franšíza pronájem globální kolo. Chcete toobuild regresní model toopredict hello pronájem požadavek na základě historických dat. Máte 1000 pronájem umístění napříč hello, world a jste shromažďují datovou sadu pro každé umístění, která obsahuje důležité funkce, jako je například datum, čas, počasí a provoz, které jsou specifické tooeach umístění.

Může natrénování modelu jednou pomocí sloučené verze všech datových sad hello ve všech umístěních. Ale protože každé z vašich lokalit má jedinečné prostředí, je vhodnější by být tootrain regresní model samostatně pomocí hello datovou sadu pro každé umístění. Tímto způsobem do velikosti jiném úložišti hello účtů, svazku, geography, plnění, kolo friendly provoz prostředí, může trvat každý trained model *atd*.

Který může být nejlepším postupem hello, ale nechcete, aby toocreate 1000 školení experimenty v Azure Machine Learning s každé z nich představující jedinečné umístění. Kromě toho se čtenáře úloh, je také zdá se, že poměrně neefektivní vzhledem k tomu, že každý experiment by měla mít všechny hello stejné komponenty s výjimkou hello školení datovou sadu.

Naštěstí jsme se dá dosáhnout pomocí hello [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) a automatizace úkolů hello s [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).

> [!NOTE]
> toomake naše ukázka rychleji probíhají rychleji, jsme budete snížit hello počet umístění z 1 000 too10. Ale hello stejné zásady a postupy platí too1 000 umístění. Hello jediným rozdílem je, že pokud chcete, aby tootrain z 1 000 datových sad, budete ho zřejmě chtít toothink spuštěných hello následujících skriptů prostředí PowerShell paralelně. Jak toodo, který je mimo rozsah hello v tomto článku, ale najdete příklady prostředí PowerShell více vláken na hello Internetu.  
> 
> 

## <a name="set-up-hello-training-experiment"></a>Nastavit výukový experiment hello
Vytvoříme toouse příklad [výukový experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , jsme již jste vytvořili v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Otevřete tento experiment v vaše [Azure Machine Learning Studio](https://studio.azureml.net) pracovního prostoru.

> [!NOTE]
> V pořadí toofollow spolu se v tomto příkladu můžete chtít toouse standardní pracovní prostor než volného prostoru. Jsme budete vytváření jeden koncový bod pro každého zákazníka – celkem 10 koncové body – a vzhledem k tomu, že volného prostoru je omezená too3 koncové body, které se vyžadují standardní pracovní prostor. Pokud máte pouze volného prostoru, stačí upravte skripty hello níže tooallow pro jenom 3 umístění.
> 
> 

Hello experiment používá **importovat Data** modulu tooimport hello školení datovou sadu *customer001.csv* z účtu úložiště Azure. Předpokládejme, jsme shromážděných z všech kolo pronájem umístění datové sady školení a je uložen v hello stejné umístění úložiště blob s názvy souborů od *rentalloc001.csv* příliš*rentalloc10.csv* .

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Všimněte si, že **výstup webové služby** modulu přidala toohello **Train Model** modulu.
Při nasazení tohoto experimentu jako webové služby, koncový bod hello přidružený tento výstup hello formát souboru .ilearner, vrátí hello trained model.

Všimněte si, že jsme nastavit parametr webové služby pro adresu URL hello této hello **importovat Data** používá modul. To umožňuje nám toouse hello parametr toospecify jednotlivých školení datové sady tootrain hello modelu pro každé umístění.
Existují jiné způsoby jsme může to provedli, například pomocí příkazu jazyka SQL webové služby parametr tooget daty z databáze SQL Azure nebo jednoduše pomocí **vstup webové služby** toopass modulu v datové sadě toohello webové služby.

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Teď umožňuje spustit tento výukový experiment s výchozí hodnotou hello *rental001.csv* jako hello školení datovou sadu. Při zobrazení hello výstup hello **Evaluate** modulu (klikněte na výstup hello a vyberte **vizualizovat**), najdete v části se nám získat dostatečnou výkon *AUC* = 0.91. V tomto okamžiku nám připravené toodeploy webová služba mimo tento výukový experiment.

## <a name="deploy-hello-training-and-scoring-web-services"></a>Nasazení hello školení a vyhodnocování webové služby
toodeploy hello cvičení webové služby, klikněte na tlačítko hello **nastavit webové služby** tlačítko pod plátnem experimentu hello a vyberte **nasazení webové služby**. Volání této webové služby "" kolo pronájem školení".

Nyní potřebujeme toodeploy hello vyhodnocování webové služby.
toodo toho jsme klikněte na tlačítko **nastavit webové služby** níže hello plátno a vyberte **webové služby prediktivní**. Tím se vytvoří vyhodnocování experimentu.
Toomake potřebujeme pár menší úpravy toomake fungovat jako webovou službu, například odebráním hello popisek sloupce "cnt" hello vstupní data a omezení id instance hello tooonly výstup hello a odpovídající hello předpovědět hodnotu.

toosave sami, které pracují, můžete jednoduše otevřít hello [prediktivní experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) v hello galerie, která je již připravena.

toodeploy hello webové služby, spustit experiment prediktivní hello, pak klikněte na hello **nasazení webové služby** tlačítko pod plátnem hello. Název hello vyhodnocování webové služby "Kolo pronájem vyhodnocování" ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Vytvoření 10 koncových bodů identické webové služby pomocí prostředí PowerShell
Této webové služby se dodává s výchozí koncový bod. Ale ještě nejsme zajímat hello výchozí koncový bod vzhledem k tomu, že se nezdařila. Co potřebujeme toodo je toocreate 10 další koncové body, jeden pro každé umístění. Provedeme to pomocí prostředí PowerShell.

Nejprve nastavíme naše prostředí PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and is properly set toopoint toohello valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Potom spusťte následující příkaz prostředí PowerShell hello:

    # Create 10 endpoints on hello scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nyní vytvořili jsme 10 koncových bodů a všechny obsahují hello stejný trained model trénink na *customer001.csv*. Můžete je zobrazit v hello portálu pro správu Azure.

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-hello-endpoints-toouse-separate-training-datasets-using-powershell"></a>Aktualizovat hello koncové body toouse samostatné školení datové sady pomocí prostředí PowerShell
dalším krokem Hello je tooupdate hello koncových bodů s modely, které jednoznačně trénink na jednotlivé data každého zákazníka. Ale nejdřív potřebujeme tooproduce, tyto modely z hello **kolo pronájem školení** webové služby. Přejděte zpět toohello **kolo pronájem školení** webové služby. Potřebujeme toocall svůj koncový bod BES 10krát s 10 různých školení datové sady v různých modelech tooproduce 10 pořadí. Použijeme hello **InovkeAmlWebServiceBESEndpoint** toodo rutiny prostředí PowerShell to.

Budete také potřebovat tooprovide pověření pro účet úložiště objektů blob do `$configContent`, a to, v polích hello `AccountName`, `AccountKey` a `RelativeLocation`. Hello `AccountName` může být jedna z vaší názvy účtů, jak je vidět v hello **portálu pro správu Azure Classic** (*úložiště* karta). Když kliknete na účet úložiště jeho `AccountKey` získáte stisknutím hello **spravovat přístupové klíče** tlačítko v dolní části hello a kopírování hello *primární přístupový klíč*. Hello `RelativeLocation` je hello cesta relativní tooyour úložiště, kde bude uložena nový model. Například cesta hello `hai/retrain/bike_rental/` ve skriptu hello pod body tooa kontejner s názvem `hai`, a `/retrain/bike_rental/` obsahuje podsložky. V současné době nelze vytvořit podsložky prostřednictvím portálu hello uživatelského rozhraní, ale existují [několik Průzkumníci úložiště Azure](../storage/common/storage-explorers.md) , umožňují toodo tak. Doporučuje se vytvořit nový kontejner v vašeho úložiště toostore hello nové trénované modely (soubory .ilearner) následujícím způsobem: ze stránky úložiště, klikněte na hello **přidat** tlačítko dole v hello a pojmenujte ji `retrain`. Souhrnně níže hello potřebné změny toohello skriptu týkají příliš`AccountName`, `AccountKey` a `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke hello retraining API 10 times
    # This is hello default (and hello only) endpoint on hello training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> Hello koncový bod BES je hello režimu podporovány pouze pro tuto operaci. Záznamy o prostředku nelze použít pro vytvoření trénované modely.
> 
> 

Jak je uvedeno výše, namísto vytváření 10 různých BES úlohy konfigurace soubory json, dynamicky místo vytvoření hello konfigurační řetězec a informačního kanálu toohello *jobConfigString* parametr hello  **InvokeAmlWebServceBESEndpoint** rutiny, protože je skutečně bez nutnosti tookeep kopii na disku.

Pokud všechno proběhne správně, po chvíli se měl zobrazit 10 .ilearner souborů z *model001.ilearner* příliš*model010.ilearner*, v účtu úložiště Azure. Teď máme připraven tooupdate naše 10 vyhodnocování webové koncové body služby pomocí těchto modelů pomocí hello **oprava AmlWebServiceEndpoint** rutiny prostředí PowerShell. Znovu si pamatujte, že jsme pouze oprava koncové body hello jiné než výchozí, které jsme prostřednictvím kódu programu vytvořili předtím.

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

To měly být spuštěny docela rychle. Po dokončení provádění hello jsme budete úspěšně jste vytvořili 10 koncových bodů prediktivní webové služby, každá obsahuje modulu trained model jednoznačně trénink na hello datovou sadu konkrétní tooa pronájem umístění, z jednoho výukový experiment. tooverify, můžete zkusit volání tyto koncové body pomocí hello **InvokeAmlWebServiceRRSEndpoint** rutinu a poskytovat jim hello stejné vstupní data a vzhledem k tomu, že jsou hello modely, které byste měli očekávat toosee různých předpovědi výsledky cvičení s jinou školení sad.

## <a name="full-powershell-script"></a>Úplné skript prostředí PowerShell
Tady je seznam hello hello úplný zdrojový kód:

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and properly set toopoint toohello valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on hello scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke hello retraining API 10 times tooproduce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
