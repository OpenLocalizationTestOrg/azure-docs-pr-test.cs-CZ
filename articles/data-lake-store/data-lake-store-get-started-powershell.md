---
title: "prostředí PowerShell tooget aaaUse začít s Azure Data Lake Store | Microsoft Docs"
description: "Použití Azure PowerShell toocreate účtu Data Lake Store a provádění základních operací"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 9c958bfd63e412ec0b0a4113a149f61aee026bc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Začínáme s Azure Data Lake Store pomocí Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Zjistěte, jak toouse prostředí Azure PowerShell toocreate Azure Data Lake ukládání účtu a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 nebo vyšší**. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="authentication"></a>Authentication
Tento článek používá jednodušší metodu ověřování s Data Lake Store, kde jsou výzvami tooenter přihlašovací údaje účtu Azure. Hello přístup úrovně tooData Lake Store účtu a systém souborů je pak řídí hello úroveň přístupu tohoto hello přihlášeného uživatele. Nicméně existují další postupy jako dobře tooauthenticate s Data Lake Store, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**. Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu Azure Data Lake Store
1. Z plochy otevřete nové okno prostředí Windows PowerShell, zadejte hello následující fragment kódu toolog v tooyour účet Azure, nastavte hello předplatné a zaregistrujte poskytovatele Data Lake Store hello. Když výzvami toolog, ujistěte se můžete přihlásit jako jeden ze správců/vlastník předplatného hello:

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Účet Azure Data Lake Store je přidružený ke skupině prostředků Azure. Začněte vytvořením skupiny prostředků Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Vytvoření skupiny prostředků Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Vytvoření skupiny prostředků Azure")
3. Vytvořte účet Azure Data Lake Store. Hello název, vámi zadaných musí obsahovat jenom malá písmena a číslice.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Vytvoření účtu Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Vytvoření účtu Azure Data Lake Store")
4. Ověřte, že hello účet je úspěšně vytvořen.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Výstup by měl proběhnout Hello **True**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Vytváření struktur adresářů v Azure Data Lake Store
Můžete vytvářet adresáře v rámci vaší toomanage účtu Azure Data Lake Store a ukládat data.

1. Zadejte kořenový adresář.

        $myrootdir = "/"
2. Vytvořte nový adresář s názvem **mynewdirectory** v kořenovém adresáři zadané hello.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Ověřte, že tento nový adresář hello je úspěšně vytvořen.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Měl by se zobrazit výstup jako hello následující:

    ![Ověření adresáře](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Ověření adresáře")

## <a name="upload-data-tooyour-azure-data-lake-store"></a>Nahrání dat tooyour Azure Data Lake Store
Můžete nahrát data Lake Store tooData přímo na hello kořenové úrovni nebo tooa adresář, který jste vytvořili v rámci účtu hello. Hello níže zobrazené fragmenty kódu ukazují, jak tooupload adresáře toohello některých ukázkových dat (**mynewdirectory**) jste vytvořili v předchozí části hello.

Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Stáhněte si soubor hello a uložte ho do místního adresáře v počítači, například C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Přejmenování, stažení a odstranění dat z Data Lake Store
toorename soubor, použijte následující příkaz hello:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

toodownload soubor, použijte následující příkaz hello:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

toodelete soubor, použijte následující příkaz hello:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po zobrazení výzvy zadejte **Y** toodelete hello položky. Pokud máte více než jeden soubor toodelete, můžete zadat všechny hello cesty oddělené čárkou.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Odstranění účtu Azure Data Lake Store
Použijte následující příkaz toodelete hello účtu Data Lake Store.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po zobrazení výzvy zadejte **Y** toodelete hello účtu.

## <a name="performance-guidance-while-using-powershell"></a>Průvodce výkonem při použití prostředí PowerShell

Níže jsou hello nejdůležitější nastavení, které mohou být ujít tooget hello nejlepší výkon při použití prostředí PowerShell toowork s Data Lake Store:

| Vlastnost            | Výchozí | Popis |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Tento parametr umožňuje toochoose hello počet paralelních vláken pro nahrávání nebo stahování každý soubor. Toto číslo představuje hello maximální počet vláken, které mohou být přiděleny na soubor, ale může získat méně vláken v závislosti na váš scénář (například pokud odesíláte soubor 1 KB, i když je požádat o 20 vláken obdržíte jedno vlákno).  |
| ConcurrentFileCount | 10      | Tento parametr je určený zejména pro nahrávání nebo stahování složek. Tento parametr určuje hello počet souběžných souborů, které můžete nahrát nebo stáhnout. Toto číslo představuje hello maximální počet souběžných souborů, které je možné nahrát nebo stáhnout najednou, ale může získat méně souběžnosti v závislosti na váš scénář (například pokud nahráváte dva soubory, zobrazí se dvě nahrávání souborů souběžných i v případě, že je požádat o 15). |

**Příklad**

Tento příkaz stáhne soubory z místního disku uživatele toohello Azure Data Lake Store pomocí 20 vláken na soubor a 100 souběžných soubory.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-hello-value-tooset-for-these-parameters"></a>Jak zjistím hello tooset hodnoty pro tyto parametry?

Tady je několik rad, kterými se můžete řídit.

* **Krok 1: Určení počet celkový počet vláken hello** -byste měli začít výpočtu toouse počet celkový počet vláken hello. Podle obecných pokynů byste měli použít 6 vláken na každé fyzické jádro.

        Total thread count = total physical cores * 6

    **Příklad**

    Za předpokladu, že používáte hello prostředí PowerShell příkazy z D14 virtuálního počítače, který má 16 jader

        Total thread count = 16 cores * 6 = 96 threads


* **Krok 2: Vypočítat PerFileThreadCount** -jsme vypočítat naše PerFileThreadCount na základě velikosti hello hello souborů. Pro soubory je menší než 2,5 GB není bez nutnosti toochange tento parametr protože stačí hello výchozí hodnotu 10. Pro soubory větší než 2,5 GB byste měli používat 10 vláken, hello základ pro hello první 2,5 GB a přidejte 1 vlákno pro každý další 256 MB nárůst velikosti souboru. Pokud kopírujete složku se soubory velmi rozdílných velikostí, zvažte jejich seskupení podle podobných velikostí. Velmi rozdílné velikosti souborů mohou způsobit, že výkon nebude optimální. Pokud není možné toogroup podobné velikosti souborů, měli byste nastavit PerFileThreadCount podle hello největší velikost souboru.

        PerFileThreadCount = 10 threads for hello first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Příklad**

    Za předpokladu, že máte 100 souborů od too10GB 1 GB, použijeme hello 10 GB, stejně jako hello největší velikost pro rovnice, který bude číst jako následující hello souboru.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **Krok 3: Vypočítat ConcurrentFilecount** -počet použití hello celkový počet vláken a PerFileThreadCount toocalculate ConcurrentFileCount podle následující rovnice hello.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Příklad**

    Podle hello ukázkové hodnoty, které jsme pomocí

        96 = 40 * ConcurrentFileCount

    Ano **ConcurrentFileCount** je **2.4**, který jsme můžete příliš zaokrouhlení**2**.

### <a name="further-tuning"></a>Další ladění

Může vyžadovat další ladění, protože je rozsah toowork velikosti souboru s. Hello výše výpočtu funguje dobře v případě, že všechny nebo většinu hello soubory jsou větší a blíže toohello rozsah 10GB. Pokud ale bude existovat mnoho různých velikostí souborů a mnoho jich bude menších, mohli byste hodnotu PerFileThreadCount snížit. Snížením hello PerFileThreadCount jsme může zvýšit ConcurrentFileCount. Ano Pokud předpokládáme, že většina našich soubory je menší v rozsahu 5GB hello, můžeme znovu proveďte naše výpočet:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

Ano **ConcurrentFileCount** bude nyní 96/20, což je 4.8, zaokrouhlen příliš**4**.

Můžete pokračovat tootune tato nastavení můžete změnit hello **PerFileThreadCount** nahoru a dolů v závislosti na hello distribuce vaší velikosti souborů.

### <a name="limitation"></a>Omezení

* **Počet souborů je menší než ConcurrentFileCount**: Pokud je menší než hello hello počet souborů, které odesíláte **ConcurrentFileCount** , jste vypočítali, pak by měl snížit  **ConcurrentFileCount** toobe rovna toohello počet souborů. Můžete použít všechny zbývající tooincrease vláken **PerFileThreadCount**.

* **Příliš mnoho vláken**: Pokud zvýšíte přístup z více vláken počet příliš mnoho bez zvýšit velikost vašeho clusteru, spusťte hello riziko snížení výkonu. Může být nutné vyřešit problémy při kontextu přechodu na hello procesoru.

* **Nedostatečná souběžnosti**: Pokud hello souběžnosti nestačí, pak clusteru může být příliš malá. Můžete zvýšit hello počet uzlů v clusteru, který vám poskytne další souběžnosti.

* **Chyby omezování**: Pokud je souběžnost příliš vysoká, může docházet k chybám omezování. Pokud vidíte chyby omezení, musí snižte hello souběžnosti nebo kontaktujte nás.

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

