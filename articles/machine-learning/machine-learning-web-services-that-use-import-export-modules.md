---
title: "aaaUsing importu a exportu dat v Azure Machine Learning webové služby | Microsoft Docs"
description: "Zjistěte, jak toouse hello toosend moduly Data importovat a exportovat Data a přijímat data z webové služby."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 176380259b15cb338ede61c7f28ba2296b35dd52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Nasazování webových služeb Azure ML používajících moduly Import dat a Export dat

Když vytvoříte prediktivní experiment, obvykle přidat vstup webové služby a výstup. Když nasadíte hello experiment, příjemci můžete odesílat a přijímat data z hello webové služby prostřednictvím hello vstupy a výstupy. Některé aplikace může být k dispozici z datového kanálu uživatelských dat nebo se již nacházejí v zdroj externích dat, jako je například úložiště objektů Blob v Azure. V těchto případech nemusí čtení a zápisu dat pomocí webové služby vstupy a výstupy. Mohou, místo toho použít hello spuštění služby Batch (BES) tooread data ze zdroje dat hello pomocí modulu importu dat a zápis hello vyhodnocování výsledky tooa různých datových umístění pomocí modul exportovat Data.

Hello importovat Data a Export dat moduly, může číst a zapisovat data toovarious zadejte umístění, jako je například adresa URL webového prostřednictvím protokolu HTTP, dotaz Hive, databázi Azure SQL, Azure Table storage, Azure Blob storage, datového kanálu nebo místní databázi SQL.

Toto téma používá hello "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" ukázkové a předpokládá datovou sadu hello již byla načtena do tabulky Azure SQL s názvem censusdata.

## <a name="create-hello-training-experiment"></a>Vytvoření experimentu školení hello
Při otevření hello "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" ukázkové ji používá hello ukázkovou pro dospělé úplné zjišťování příjem binární klasifikace datovou sadu. A hello experimentu v hello plátno bude vypadat podobně jako toohello následující bitové kopie:

![Počáteční konfigurace hello experimentu.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

tooread hello data z tabulky Azure SQL hello:

1. Odstraňte modul hello datovou sadu.
2. Hello součásti vyhledávacího pole zadejte importu.
3. Ze seznamu výsledků hello, přidejte *importovat Data* modulu toohello experimentovat plátno.
4. Připojit výstup hello *importovat Data* vstup hello modulu Dobrý den *vyčištění chybějících dat* modulu.
5. V podokně vlastnosti, vyberte **Azure SQL Database** v hello **zdroj dat** rozevíracího seznamu.
6. V hello **název databázového serveru**, **název databáze**, **uživatelské jméno**, a **heslo** pole, zadejte příslušné informace o hello pro vaše databáze.
7. V poli dotazu databáze hello zadejte následující dotaz hello.
   
     Vyberte [stáří]
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     z dbo.censusdata;
8. Hello dolní části plátna experimentu hello, klikněte na **spustit**.

## <a name="create-hello-predictive-experiment"></a>Vytvořit prediktivní experiment hello
Další nastavíte hello prediktivní experiment, ze kterého nasadíte webovou službu.

1. Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **prediktivní webové služby (doporučeno)**.
2. Odebrat hello *vstup webové služby* a *webové služby výstupní moduly* z prediktivní experiment hello. 
3. Hello součásti vyhledávacího pole zadejte export.
4. Ze seznamu výsledků hello, přidejte *Export dat* modulu toohello experimentovat plátno.
5. Připojit výstup hello *Score Model* vstup hello modulu Dobrý den *Export dat* modulu. 
6. V podokně vlastnosti, vyberte **Azure SQL Database** v rozevírací cílové hello data.
7. V hello **název databázového serveru**, **název databáze**, **název uživatelského účtu serveru**, a **heslo uživatelského účtu serveru** pole, zadejte Hello příslušné informace pro vaši databázi.
8. V hello **čárkou oddělený seznam sloupců toobe Uložit** zadejte popisky vyhodnocení.
9. V hello **pole název tabulky dat**, zadejte dbo. ScoredLabels. Pokud hello tabulka neexistuje, vytvoří se při spuštění experimentu hello nebo se nazývá hello webové služby.
10. V hello **čárkami oddělený seznam sloupců datatable** pole, zadejte ScoredLabels.

Při psaní aplikace, volání hello konečné webové služby, můžete chtít toospecify jiné vstupní dotaz nebo cílové tabulky v době běhu. tooconfigure tyto vstupy a výstupy, použijte hello parametry webové služby funkce tooset hello *importovat Data* modulu *zdroj dat* vlastnost a hello *Export dat* režimu Vlastnost cílové data.  Další informace o parametry webové služby, najdete v části hello [vstupní parametry webové služby AzureML](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) na Machine Learning Blog a hello Cortana Intelligence.

tooconfigure hello parametry webové služby pro hello cílové tabulky a dotazu import hello:

1. V podokně Vlastnosti hello hello *importovat Data* modulu, klikněte na ikonu hello v hello top napravo od hello **databázový dotaz** pole a vyberte **nastavit jako parametr webové služby**.
2. V podokně Vlastnosti hello hello *Export dat* modulu, klikněte na ikonu hello v hello top napravo od hello **název tabulky dat** pole a vyberte **nastavit jako parametr webové služby**.
3. Na konci hello hello *Export dat* podokno vlastností modulu, v hello **parametry webové služby** části, klikněte na databázový dotaz a přejmenujte ji dotazu.
4. Klikněte na tlačítko **název tabulky dat** a přejmenujte ji **tabulky**.

Až skončíte, experimentu by měl vypadat podobně jako toohello následující bitové kopie:

![Poslední vzhled experimentu.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Hello experiment teď můžete nasadit jako webovou službu.

## <a name="deploy-hello-web-service"></a>Nasazení webové služby hello
Můžete nasadit tooeither Classic nebo nové webové služby.

### <a name="deploy-a-classic-web-service"></a>Nasazení Classic webové služby
toodeploy jako webovou službu, Classic a vytvoření tooconsume aplikace ho:

1. V hello dolní části plátna experimentu hello klikněte na tlačítko spustit.
2. Po dokončení hello spustit, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]**.
3. Na panelu webové služby hello vyhledejte klíč rozhraní API. Zkopírujte a uložte ho později toouse.
4. V hello **výchozí koncový bod** tabulky, klikněte na tlačítko hello **Batch Execution** odkaz tooopen hello stránce nápovědy k rozhraní API.
5. Ve Visual Studiu Vytvořte konzolovou aplikaci C#: **nový** > **projektu** > **Visual C#** > **Windows Klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.
6. Na stránce nápovědy k rozhraní API hello, najít hello **ukázkový kód** oddíl hello dolní části stránky hello.
7. Zkopírujte a vložte do souboru Program.cs hello ukázkový kód C# a odebrat všechny odkazy na toohello objektu blob úložiště.
8. Aktualizujte hodnotu hello hello *apiKey* proměnné s klíčem rozhraní API hello předtím uložili.
9. Vyhledejte hello žádosti o deklaraci a aktualizace hello hodnoty parametry webové služby, které se předávají toohello *importovat Data* a *Export dat* moduly. V takovém případě použijte původní dotaz hello ale definovat nový název tabulky.
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Spusťte aplikaci hello. 

Při dokončení hello spuštění se přidá novou tabulku toohello databáze obsahující hello vyhodnocování výsledky.

### <a name="deploy-a-new-web-service"></a>Nasadit novou webovou službu

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

toodeploy jako novou webovou službu a vytvořit tooconsume aplikace ho:

1. Hello dolní části plátna experimentu hello, klikněte na **spustit**.
2. Po dokončení hello spustit, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení [nové] webové služby**.
3. Na stránce experimentu nasazení hello, zadejte název vaší webové služby a vybrat cenový plán, a pak klikněte na **nasadit**.
4. Na hello **rychlý Start** klikněte na tlačítko **spotřebě**.
5. V hello **ukázkový kód** klikněte na tlačítko **Batch**.
6. Ve Visual Studiu Vytvořte konzolovou aplikaci C#: **nový** > **projektu** > **Visual C#** > **Windows Klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.
7. Zkopírujte a vložte hello C# ukázkový kód do souboru Program.cs.
8. Aktualizujte hodnotu hello hello *apiKey* proměnné s hello **primární klíč** umístěný v hello **informace o základní spotřeby** části.
9. Vyhledejte hello *scoreRequest* deklarace a aktualizace hodnoty hello parametry webové služby, které se předávají toohello *importovat Data* a *Export dat* moduly. V takovém případě použijte původní dotaz hello ale definovat nový název tabulky.
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. Spusťte aplikaci hello. 

