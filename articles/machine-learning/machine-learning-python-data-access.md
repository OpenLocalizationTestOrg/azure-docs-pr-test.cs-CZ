---
title: "datové sady aaaAccess s knihovnou klienta Machine Learning Python | Microsoft Docs"
description: "Nainstalovat a používat hello tooaccess knihovny klienta Python a bezpečně spravovat Azure Machine Learning data z místního prostředí Python."
services: machine-learning
documentationcenter: python
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: huvalo;bradsev
ms.openlocfilehash: f55067118f13c52bf677930a20836ce6989f8187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-datasets-with-python-using-hello-azure-machine-learning-python-client-library"></a>Přístup k datové sady s Pythonem pomocí klientské knihovny Azure Machine Learning Python hello
Hello preview Klientská knihovna pro Microsoft Azure Machine Learning Python můžete povolit zabezpečený přístup tooyour Azure Machine Learning datové sady z místního prostředí Python a umožňuje hello vytváření a správu datových sad v pracovním prostoru.

Toto téma obsahuje pokyny o tom, jak:

* Nainstalujte klientské knihovny pro Machine Learning Python hello 
* přístup a datové sady, včetně pokyny, jak nahrát tooget autorizace tooaccess Azure Machine Learning datové sady z vaší místní prostředí Python
* přístup k zprostředkující datové sady z experimentů
* používat hello Python klienta knihovny tooenumerate datové sady, získat přístup k metadatům, přečíst obsah hello datové sady, vytvořte nové datové sady a aktualizovat existující datové sady

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Požadavky
Klientská knihovna pro Python Hello otestovala pod hello následující prostředí:

* Windows, Mac a Linux
* Python 2.7, 3.3 a 3.4

Má závislost na hello následující balíčky:

* požadavky
* Python dateutil
* pandas

Doporučujeme použít distribuci jazyka Python, jako [Anaconda](http://continuum.io/downloads#all) nebo [zápoje](https://store.enthought.com/downloads/), které jsou součástí Python, IPython a nainstalované hello tři balíčky uvedené výše. I když IPython není nezbytně nutné, je skvělé prostředí pro manipulace a vizualizace dat interaktivně.

### <a name="installation"></a>Jak tooinstall hello Klientská knihovna pro Azure Machine Learning Python
Klientská knihovna pro Azure Machine Learning Python Hello také musí být nainstalované toocomplete hello úlohy uvedené v tomto tématu. Je k dispozici z hello [indexu balíčků Pythonu](https://pypi.python.org/pypi/azureml). tooinstall ji ve vašem prostředí Python, spusťte následující příkaz z vaší místní prostředí Python hello:

    pip install azureml

Alternativně můžete stáhnout a nainstalovat z hello zdrojů na [githubu](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Pokud máte nainstalovaný na počítači git, můžete přímo z úložiště git hello pip tooinstall:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Použít Studio Code fragmenty tooaccess datové sady
Klientská knihovna pro Hello Python poskytuje existující datové sady tooyour programový přístup z experimenty, které byly spuštěny.

Z hello Studio webové rozhraní můžete vygenerovat fragmenty kódu, které zahrnují všechny potřebné informace toodownload hello a deserializaci datové sady jako objekty Pandas DataFrame na počítači pro umístění.

### <a name="security"></a>Zabezpečení pro přístup k datům
Hello fragmenty kódu poskytované Studio pro použití s hello Python Klientská knihovna obsahuje id pracovního prostoru a autorizační token. Tyto poskytnout úplný přístup tooyour prostoru a musí být chráněny, jako je heslo.

Z bezpečnostních důvodů hello funkce fragmentu kódu je pouze k dispozici toousers, které mají své role nastavit jako **vlastníka** pro pracovní prostor hello. Vaše role se zobrazí v Azure Machine Learning Studio na hello **uživatelé** v části **nastavení**.

![Zabezpečení][security]

Pokud vaše role není nastaven jako **vlastníka**, můžete požádat o toobe nového pozvání jako vlastníka, nebo požádejte vlastníka hello hello prostoru tooprovide vám hello fragmentu kódu.

tooobtain hello autorizační token, můžete provést jednu z následujících akcí hello:

* Požádejte o token z vlastníka. Přístup k nim vlastníci jejich autorizace tokeny ze stránky nastavení hello jejich pracovního prostoru v Studio. Vyberte **nastavení** z hello levé podokno a klikněte na tlačítko **AUTORIZACE TOKENY** toosee hello primární a sekundární tokeny.  Hello primární nebo sekundární autorizace tokeny hello lze ve fragmentu kódu hello, ale doporučujeme vlastníky sdílet jenom tokeny hello sekundární ověřování.

![Tokeny ověřování](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* Požádejte toobe povýší toorole vlastníka.  toodo to aktuálního vlastníka hello prostoru potřebám toofirst můžete odebrat z pracovního prostoru hello pak znovu vás pozval tooit jako vlastníka.

Po získání vývojáři id pracovního prostoru hello a autorizace token, jsou možné tooaccess hello prostoru pomocí fragmentu kódu hello bez ohledu na jejich role.

Tokeny ověřování jsou spravovány v hello **AUTORIZACE TOKENY** v části **nastavení**. Můžete je obnovit, ale tento postup odvolá přístup toohello předchozí token.

### <a name="accessingDatasets"></a>Datové sady přístup z místních aplikací Python
1. V nástroji Machine Learning Studio, klikněte na tlačítko **datové sady** hello navigačním panelu na levé straně hello.
2. Vyberte datovou sadu hello chcete tooaccess. Vyberte některé z datové sady hello z hello **Moje datové sady** seznamu nebo z hello **UKÁZKY** seznamu.
3. Z hello dolním panelu nástrojů, klikněte na tlačítko **generování dat přístupového kódu**. Je-li hello data ve formátu, který je kompatibilní s hello Python klientské knihovny, toto tlačítko k dispozici.
   
    ![Datové sady][datasets]
4. Vyberte hello fragmentu kódu zobrazeném okně hello a zkopírujte ho tooyour schránky.
   
    ![Přístupový kód][dataset-access-code]
5. Hello kód vložte do poznámkového bloku hello místní aplikace Python.
   
    ![Poznámkový blok][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Zprostředkující datové sady přístup z experimentů Machine Learning
Po spuštění experimentu v hello Machine Learning Studio, je možné tooaccess hello zprostředkující datové sady z uzlů výstup hello modulů. Zprostředkující datové sady jsou data, která byla vytvořena a používá se pro zprostředkující kroky při byl spuštěn nástroj modelu.

Zprostředkující datové sady je přístupný, dokud hello formát dat je kompatibilní s knihovnou klienta Python hello.

Hello jsou podporovány následující formáty (konstanty pro tyto jsou v hello `azureml.DataTypeIds` třída):

* Ve formátu prostého textu
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Můžete určit formát hello podržením ukazatele nad uzlem výstup modulu. Zobrazí se společně s hello název uzlu, v popisu tlačítka.

Některé moduly hello, například hello [rozdělení] [ split] modulu, výstupní formát tooa s názvem `Dataset`, která není podporována hello Python klientské knihovny.

![Formát datové sady][dataset-format]

Je třeba toouse převodu modulu, například [převést tooCSV][convert-to-csv], tooget výstup do podporovaného formátu.

![Formát GenericCSV][csv-format]

Hello následující postup příklad, který vytvoří experimentu, ji spustí a přistupuje k hello zprostředkující datovou sadu.

1. Vytvoření nového experimentu.
2. Vložit **datovou sadu pro dospělé úplné zjišťování příjem binární klasifikace** modulu.
3. Vložení [rozdělení] [ split] modul a připojte její výstup modulu toohello vstupní datovou sadu.
4. Vložení [převést tooCSV] [ convert-to-csv] modulu a připojte jeho vstupní tooone hello [rozdělení] [ split] výstupy modulu.
5. Uložit hello experiment, spouštět a počkejte na její toofinish systémem.
6. Klikněte na uzel výstup hello na hello [převést tooCSV] [ convert-to-csv] modulu.
7. Jakmile se zobrazí hello kontextové nabídky, vyberte **generování dat přístupového kódu**.
   
    ![Kontextové nabídky][experiment]
8. Fragment kódu hello vyberte a zkopírujte ho tooyour schránky z okna hello, který se zobrazí.
   
    ![Přístupový kód][intermediate-dataset-access-code]
9. Vložte kód hello v poznámkovém bloku.
   
    ![Poznámkový blok][ipython-intermediate-dataset]
10. Můžete vizualizovat data hello pomocí matplotlib. Zobrazí se v histogram pro sloupec stáří hello:
    
    ![Histogram][ipython-histogram]

## <a name="clientApis"></a>Použít hello Machine Learning Python klienta knihovny tooaccess, číst, vytvářet a spravovat datové sady
### <a name="workspace"></a>Pracovní prostor
pracovní prostor Hello je hello vstupní bod pro hello Python klientské knihovny. Zadejte hello `Workspace` třídy s vašeho pracovního prostoru id a autorizační token toocreate instance:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Zobrazení výčtu datových sad
tooenumerate všechny datové sady v daném prostoru:

    for ds in ws.datasets:
        print(ds.name)

tooenumerate jenom hello uživatelem vytvořené datové sady:

    for ds in ws.user_datasets:
        print(ds.name)

tooenumerate jenom hello příklad datové sady:

    for ds in ws.example_datasets:
        print(ds.name)

Datové sady se můžete dostat pomocí názvu (což je malá a velká písmena):

    ds = ws.datasets['my dataset name']

Nebo můžete k němu přístup podle indexu:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metadata
Datové sady mít metadata, přidání toocontent. (Zprostředkující datové sady jsou toothis pravidlo výjimky a nemají žádné metadata.)

Některé hodnoty metadata jsou přiřazeny uživatelem hello v okamžiku vytvoření:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Jiné jsou hodnoty přiřadila Azure ML:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

V tématu hello `SourceDataset` třídu pro další na hello k dispozici metadata.

### <a name="read-contents"></a>Načíst obsah
fragmenty kódu Hello poskytované Machine Learning Studio automaticky stáhnout a deserializovat objekt Pandas DataFrame tooa datovou sadu hello. To lze provést pomocí hello `to_dataframe` metoda:

    frame = ds.to_dataframe()

Pokud dáváte přednost toodownload hello nezpracovaná data a provést deserializaci hello sami, který je možnost. Momentálně hello Toto je jediná možnost hello formátů, jako je například "ARFF', které hello Python klientské knihovny nelze deserializovat.

obsah hello tooread jako text:

    text_data = ds.read_as_text()

obsah hello tooread jako binární:

    binary_data = ds.read_as_binary()

Můžete otevřít také jenom datový proud toohello obsah:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Vytvořit novou sadu dat
Klientská knihovna pro Python Hello umožňuje tooupload datové sady z programu Python. Tyto datové sady jsou pak k dispozici pro použití v pracovním prostoru.

Pokud máte v Pandas DataFrame vaše data, použijte hello následující kód:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Pokud už je serializováno vaše data, můžete použít:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Hello Python klientské knihovny je možné tooserialize Pandas DataFrame toohello následující formáty (konstanty pro tyto jsou v hello `azureml.DataTypeIds` třída):

* Ve formátu prostého textu
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Aktualizovat existující datovou sadu
Pokud se pokusíte tooupload novou datovou sadu s názvem, který odpovídá existující datovou sadu, měli byste obdržet chybu konfliktu.

tooupdate existující datovou sadu, musíte nejdřív tooget odkaz na datovou sadu existující toohello:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Potom pomocí `update_from_dataframe` tooserialize a nahraďte obsah hello hello datovou sadu v Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Pokud chcete tooserialize hello data tooa jiný formát, zadejte hodnotu pro hello volitelné `data_type_id` parametr.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Volitelně můžete nastavit nový popis zadáním hodnoty pro hello `description` parametr.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toofeb 2015'

Volitelně můžete nastavit nový název a zadat hodnotu pro hello `name` parametr. Od této chvíle budete načíst datové sady hello pomocí hello pouze nový název. Hello následující kód aktualizuje hello dat, název a popis.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up toofeb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

Hello `data_type_id`, `name` a `description` parametry jsou volitelné a výchozí tootheir předchozí hodnotu. Hello `dataframe` vždy parametr je povinný.

Pokud už je serializováno vaše data, použijte `update_from_raw_data` místo `update_from_dataframe`. Pokud předáte právě v `raw_data` místo `dataframe`, funguje podobným způsobem.

<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

