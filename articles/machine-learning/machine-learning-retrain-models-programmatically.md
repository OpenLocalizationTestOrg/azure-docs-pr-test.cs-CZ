---
title: "modelů aaaRetrain Machine Learning | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically přeučování modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: edbb64c08f7d9edf3c76e23e0cc7e14c0125d697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a>Programové přeučení modelů Azure Machine Learning
V tomto návodu se dozvíte, jak tooprogrammatically přeučování Azure Machine Learning webové služby pomocí jazyka C# a hello Služba Machine Learning Batch Execution.

Jakmile mají retrained hello modelu, hello následující postupy ukazují, jak tooupdate hello model prediktivní webové služby:

* Pokud jste nasadili hello webové služby Machine Learning portálu Classic webové služby, přečtěte si téma [Přeučování webové služby Classic](machine-learning-retrain-a-classic-web-service.md). 
* Pokud jste nasadili novou webovou službu, najdete v části [Přeučování novou webovou službu pomocí rutin Machine Learning Management hello](machine-learning-retrain-new-web-service-using-powershell.md).

Přehled hello retraining procesu najdete v tématu [Přeučování Model strojového učení](machine-learning-retrain-machine-learning-model.md).

Pokud chcete, aby se stávající toostart nové Azure Resource Manager na základě webové služby, najdete v části [Přeučování existující prediktivní webovou službu](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Vytvoření experimentu školení
V tomto příkladu budete používat "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" z ukázky Microsoft Azure Machine Learning hello. 

toocreate hello experiment:

1. Přihlaste se k tooMicrosoft Azure Machine Learning Studio. 
2. Na hello pravém dolním rohu hello řídicí panel, klikněte na tlačítko **nový**.
3. Hello Microsoft Samples vyberte ukázkový 5.
4. experiment hello toorename hello horní části plátna experimentu hello, vyberte název experimentu hello "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu".
5. Typ modelu úplné zjišťování.
6. Hello dolní části plátna experimentu hello, klikněte na **spustit**.
7. Klikněte na tlačítko **nastavit webové služby** a vyberte **Retraining webové služby**. 

Následující Hello ukazuje počáteční experimentu hello.
   
   ![Počáteční experimentu.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Vytvořit prediktivní experiment a publikovat jako webovou službu
Dále vytvoříte Predicative experimentu.

1. Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **webové služby prediktivní**. To uloží hello model jako modulu Trained Model a přidá modulů vstup a výstup webové služby. 
2. Klikněte na **Run** (Spustit). 
3. Po dokončení běhu hello experimentu klikněte na tlačítko **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby**.

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

## <a name="deploy-hello-training-experiment-as-a-training-web-service"></a>Hello výukový experiment nasadit jako webovou službu školení
tooretrain hello trained model, je nutné nasadit hello výukový experiment, kterou jste vytvořili jako Retraining webovou službu. Potřebuje této webové služby *výstup webové služby* modulu připojené toohello  *[Train Model] [ train-model]*  modulu, toobe možné tooproduce nové trénované modely.

1. tooreturn toohello výukový experiment, klikněte v levém podokně hello hello experimenty ikonu a pak klikněte na hello experimentu s názvem úplné zjišťování modelu.  
2. Hello položek experimentu hledání vyhledávacího pole zadejte webovou službu. 
3. Přetáhněte *vstup webové služby* modulu do hello experimentovat plátno a připojte jeho výstup toohello *vyčištění chybějících dat* modulu.  To zajistí, že je zpracován retraining data hello stejný způsob jako původní data školení.
4. Přetáhněte dva *webovou službu výstup* modulů do hello experimentovat plátno. Připojit hello výstup hello *Train Model* modulu tooone a hello výstup hello *Evaluate Model* modulu toohello jiné. Hello výstup webové služby pro **Train Model** nám dává hello nový trained model. Hello výstup připojené příliš**Evaluate Model** vrátí výstupu Tenhle modul, který je hello výsledky.
5. Klikněte na **Run** (Spustit). 

Dále je nutné nasadit hello výukový experiment jako webová služba, která vytváří modulu trained model a výsledky vyhodnocení modelu. tooaccomplish to vaše další sadu akcí jsou závislé na tom, jestli pracujete s webovou službu Classic nebo novou webovou službu.  

**Classic webové služby**

Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **nasazení webové služby [Classic]**. Hello webové služby **řídicí panel** se zobrazí se stránka nápovědy hello klíč rozhraní API a hello rozhraní API pro dávkové zpracování. Pro vytvoření Trénované modely lze použít pouze hello Batch Execution metoda.

**Novou webovou službu**

Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **nasazení [nové] webové služby**. Hello webové služby Azure Machine Learning webové služby portál otevře stránku toohello nasazení webové služby. Zadejte název pro webovou službu a zvolte plán platby a potom klikněte na **nasadit**. Pro vytvoření Trénované modely lze použít pouze hello Batch Execution – metoda

V obou případech po experiment má dokončené spuštěná hello výsledné pracovního postupu by měl vypadat takto:

![Po spuštění výsledného workflow.][4]



## <a name="retrain-hello-model-with-new-data-using-bes"></a>Přeučování hello modelu s použitím BES nová data
V tomto příkladu použijete toocreate hello C# retraining aplikace. Můžete taky hello Python nebo R ukázkový kód tooaccomplish této úlohy.

toocall hello Retraining API:

1. Vytvořte konzolovou aplikaci C# v sadě Visual Studio: **nový** > **projektu** > **Visual C#** > **Windows Classic Desktop** > **konzolovou aplikaci (rozhraní .NET Framework)**.
2. Přihlaste se toohello portál webová služba Machine Learning.
3. Pokud pracujete s webovou službou Classic, klikněte na tlačítko **Classic webové služby**.
   1. Klikněte na tlačítko hello webové služby, kterou pracujete.
   2. Klikněte na tlačítko hello výchozí koncový bod.
   3. Klikněte na tlačítko **využívat**.
   4. Na konci hello hello **spotřebě** stránku hello **ukázkový kód** klikněte na tlačítko **Batch**.
   5. Pokračujte toostep 5 tohoto postupu.
4. Pokud pracujete s novou webovou službu, klikněte na tlačítko **webové služby**.
   1. Klikněte na tlačítko hello webové služby, kterou pracujete.
   2. Klikněte na tlačítko **využívat**.
   3. V dolní části hello hello spotřebě stránky, v hello **ukázkový kód** klikněte na tlačítko **Batch**.
5. Zkopírujte hello ukázkový kód C# pro spuštění dávky a vložte jej do souboru Program.cs hello, ujistěte se, že obor názvů hello zůstává beze změn.

Jak je uvedeno v komentářích hello přidáte hello balíček Nuget Microsoft.AspNet.WebApi.Client. tooadd hello odkaz tooMicrosoft.WindowsAzure.Storage.dll, bude pravděpodobně nutné nejprve tooinstall hello Klientská knihovna pro úložiště služby Microsoft Azure. Další informace najdete v tématu [služby úložiště systému Windows](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-hello-apikey-declaration"></a>Aktualizovat hello apikey deklarace
Vyhledejte hello **apikey** deklarace.

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

V hello **informace o základní spotřeby** části hello **spotřebě** stránky, vyhledejte hello primární klíč a zkopírujte jej toohello **apikey** deklarace.

### <a name="update-hello-azure-storage-information"></a>Aktualizace informací o úložišti Azure hello
Hello BES ukázkový kód se uloží soubor z místního disku (například "C:\temp\CensusIpnput.csv") tooAzure úložiště, procesy a zapíše zpět tooAzure výsledky hello úložiště.  

tooaccomplish tato úloha musí načíst hello název, klíč a kontejner informace o účtu úložiště pro váš účet úložiště z portálu Azure classic hello a aktualizace hello odpovídající hodnoty v kódu hello. 

1. Přihlaste se na portálu Azure classic toohello.
2. V levém navigačním sloupci hello, klikněte na tlačítko **úložiště**.
3. Vyberte jeden toostore hello hello seznamu účtů úložiště retrained modelu.
4. V dolní části hello hello stránky, klikněte na tlačítko **spravovat přístupové klíče**.
5. Zkopírujte a uložte hello **primární přístupový klíč** a dialogové okno zavřít hello. 
6. V horní části hello hello stránky, klikněte na tlačítko **kontejnery**.
7. Vyberte existující kontejner nebo vytvořte novou a uložte hello název.

Vyhledejte hello *StorageAccountName*, *StorageAccountKey*, a *StorageContainerName* deklarace a aktualizace hodnoty hello jste uložili z hello portálu Azure.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Můžete také zajistit, aby byl hello vstupní soubor k dispozici v hello umístění, které zadáte v kódu hello. 

### <a name="specify-hello-output-location"></a>Zadejte umístění výstupu hello
Při zadávání hello výstupní umístění do hello datová část požadavku, hello rozšíření hello soubor zadaný v *RelativeLocation* musí být zadány jako ilearner. 

Viz následující ukázka hello:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you would like toouse for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> názvy Hello výstupních umístění se může lišit od těch, které jsou v tomto návodu podle hello pořadí, ve které jste přidali modulů hello webové služby výstup hello. Vzhledem k tomu, že jste nastavili tento výukový experiment s dvěma výstupy, hello výsledky obsahovat informace o umístění úložiště pro oba dva.  
> 
> 

![Retraining výstup][6]

Obrázek 4: Retraining výstup.

## <a name="evaluate-hello-retraining-results"></a>Vyhodnoťte výsledky Retraining hello
Při spuštění aplikace hello výstup hello obsahuje adresu URL hello a SAS token nezbytné tooaccess hello výsledky hodnocení.

Zobrazí výsledky výkonu hello hello retrained modelu kombinací hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello pro *output2* (jak je uvedeno v předchozí retraining image výstup hello) a vkládání hello úplnou adresu URL v adresním řádku prohlížeče hello.  

Hello výsledky toodetermine zkontrolujte, zda nově trained model hello provede dobře dostatek tooreplace hello existující jeden.

Kopírování hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello, použijeme je během hello retraining procesu.

## <a name="next-steps"></a>Další kroky
Pokud jste nasadili hello prediktivní webové služby kliknutím **nasazení webové služby [Classic]**, najdete v části [Přeučování webové služby Classic](machine-learning-retrain-a-classic-web-service.md).

Pokud jste nasadili hello prediktivní webová služba kliknutím **nasazení [nové] webové služby**, najdete v části [Přeučování novou webovou službu pomocí rutin Machine Learning Management hello](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using hello Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
