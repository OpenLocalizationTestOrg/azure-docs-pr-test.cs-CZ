---
title: "clustery aaaUse hello Azure toocreate portálu Azure HDInsight s Data Lake Store | Microsoft Docs"
description: "Použijte hello Azure portálu toocreate a clusterů HDInsight pomocí Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: nitinme
ms.openlocfilehash: f23113d444a3c5a01894dba29f75f3621b2d16bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-by-using-hello-azure-portal"></a>Vytvoření clusterů HDInsight s Data Lake Store pomocí portálu Azure hello
> [!div class="op_single_selector"]
> * [Hello použití portálu Azure](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Pomocí prostředí PowerShell (pro výchozí úložiště)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Pomocí prostředí PowerShell (pro další úložiště)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Pomocí Správce prostředků](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Zjistěte, jak toouse hello Azure portálu toocreate clusteru HDInsight pomocí účtu Azure Data Lake Store jako hello výchozí úložiště nebo další úložiště. I když je dodatečné úložiště pro HDInsight cluster volitelné, je doporučeno toostore obchodní data v hello další účty úložiště.

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, ujistěte se, že jste splnili hello následující požadavky:

* **Předplatné Azure**. Přejděte příliš[získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Postupujte podle pokynů hello z [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md). Musíte taky vytvořit kořenové složky na účtu hello.  V tomto kurzu kořenovou složku s názvem __/clusterů__ se používá.
* **Objektu služby Azure Active Directory**. Tento kurz obsahuje pokyny, jak toocreate objektu služby ve službě Azure Active Directory (Azure AD). Ale toocreate hlavní název služby, musíte být správce Azure AD. Pokud jste správce, můžete přeskočit tento požadavek a pokračovat v kurzu hello.

    >[!NOTE]
    >Pouze v případě, že jste správce Azure AD, vytvořte službu objektu zabezpečení. Správce služby Azure AD musí vytvořte službu objektu zabezpečení před vytvořením clusteru HDInsight s Data Lake Store. Také musí být hello instanční objekt vytvořen pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>Vytvoření clusteru HDInsight

V této části vytvoříte clusteru HDInsight s účty Data Lake Store jako výchozí hello nebo hello další úložiště. Tento článek se týká pouze hello součástí konfigurace účtů Data Lake Store.  Informace o vytvoření hello obecné clusteru a postupy najdete v tématu [vytvoření Hadoop clusterů v HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-store-as-default-storage"></a>Vytvoření clusteru s Data Lake Store jako výchozí úložiště

**cluster toocreate HDInsight s Data Lake Store jako výchozí účet úložiště hello**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Postupujte podle [vytvořit clustery se](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) hello obecné informace o vytváření clusterů HDInsight.
3. Na hello **úložiště** okno, v části **primárního úložiště typu**, vyberte **Data Lake Store**a pak zadejte hello následující informace:

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "přidat služby hlavní tooHDInsight clusteru")

    - **Vyberte Data Lake Store účtu**: Vyberte existující účet Data Lake Store. Existující účet Data Lake Store je povinný.  V tématu [požadavky](#prereuisites).
    - **Kořenová cesta**: Zadejte cestu, kde jsou uložené toobe hello specifických pro cluster soubory. Na snímku obrazovky hello je __/clusterů/myhdiadlcluster/__, ve které hello __/clusterů__ složka musí existovat a vytvoří technologie hello portál *myhdicluster* složky.  Hello *myhdicluster* je název clusteru hello.
    - **Data Lake Store přístup**: Konfigurace přístupu mezi hello účtu Data Lake Store a HDInsight cluster. Pokyny najdete v tématu [přístup konfigurovat Data Lake Store](#configure-data-lake-store-access).
    - **Další účty úložiště**: účtů přidat účty úložiště Azure jako dodatečné úložiště pro hello cluster. tooadd další Data Lake úložiště provádí tím, že při konfiguraci účtu Data Lake Store jako typ primárního úložiště hello hello clusteru oprávnění na data v další účty Data Lake Store. Viz [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).

4. Na hello **Data Lake Store přístup**, klikněte na tlačítko **vyberte**a poté pokračovat ve vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-store-as-additional-storage"></a>Vytvoření clusteru s Data Lake Store jako další úložiště

Hello následující pokyny vytvoření clusteru HDInsight se pomocí účtu Azure Storage jako hello výchozí úložiště a účet Data Lake Store jako další úložiště.
**cluster toocreate HDInsight s Data Lake Store jako výchozí účet úložiště hello**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Postupujte podle [vytvořit clustery se](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) hello obecné informace o vytváření clusterů HDInsight.
3. Na hello **úložiště** okno, v části **primárního úložiště typu**, vyberte **Azure Storage**a pak zadejte hello následující informace:

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "přidat služby hlavní tooHDInsight clusteru")

    - **Metody výběru**: použijte jednu z hello následující možnosti:

        * Vyberte účet úložiště, který je součástí vašeho předplatného Azure toospecify **mé odběry**a potom vyberte účet úložiště hello.
        * toospecify účet úložiště, který je mimo předplatného Azure, vyberte **přístupový klíč**a pak zadejte hello informace pro hello mimo účet úložiště.

    - **Výchozí kontejner**: použít výchozí hodnotu buď hello nebo zadat vlastní název.

    - Další účty úložiště: přidat další účty Azure Storage jako další úložiště hello.
    - Data Lake Store přístup: Konfigurace přístupu mezi hello účtu Data Lake Store a HDInsight cluster. Pokyny naleznete v části [přístup konfigurovat Data Lake Store](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Konfigurace přístupu Data Lake Store 

V této části nakonfigurujete Data Lake Store přístup z clusterů HDInsight pomocí Azure Active Directory instanční objekt. 

### <a name="specify-a-service-principal"></a>Zadejte hlavní název služby

Z hello portálu Azure můžete použít stávající instanční objekt nebo vytvořte novou.

**toocreate hlavní název služby z hello portálu Azure**

1. Klikněte na tlačítko **Data Lake Store přístup** v okně hello úložiště.
2. Na hello **Data Lake Store přístup** okně klikněte na tlačítko **vytvořit nový**.
3. Klikněte na tlačítko **instanční objekt**a pak postupujte podle pokynů toocreate hello hlavní název služby.
4. Stáhněte si certifikát hello, pokud se rozhodnete toouse ho znovu v budoucnu hello. Při stahování hello certifikát je užitečné, že pokud chcete, aby toouse hello stejné služby objekt zabezpečení, když vytvoříte další clustery HDInsight.

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "přidat služby hlavní tooHDInsight clusteru")

4. Klikněte na tlačítko **přístup** tooconfigure hello složce přístup.  V tématu [konfigurace oprávnění k souboru](#configure-file-permissions).


**toouse existující službu hlavní z hello portálu Azure**

1. Klikněte na tlačítko **Data Lake Store přístup**.
1. Na hello **Data Lake Store přístup** okně klikněte na tlačítko **použít existující**.
2. Klikněte na tlačítko **instanční objekt**a potom vyberte objekt služby. 
3. Nahrajte certifikát hello (soubor .pfx), který je spojen s vybranou instanční objekt a potom zadejte heslo certifikátu hello.

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "přidat služby hlavní tooHDInsight clusteru")

4. Klikněte na tlačítko **přístup** tooconfigure hello složce přístup.  V tématu [konfigurace oprávnění k souboru](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Nakonfigurujte oprávnění souborů

Nakonfiguruje Hello se liší v závislosti na tom, jestli hello účet se používá jako výchozí úložiště hello nebo účet další úložiště:

- Používat jako výchozí úložiště

    - oprávnění na úrovni kořenového hello hello účtu Data Lake Store
    - oprávnění na úrovni kořenového hello hello úložiště clusteru HDInsight. Například hello __/clusterů__ složku v kurzu hello.
- Použít jako další úložiště

    - Oprávnění hello složek, které potřebují přístup k souborům.

**tooassign oprávnění na úrovni kořenového účtu Data Lake Store hello**

1. Na hello **Data Lake Store přístup** okně klikněte na tlačítko **přístup**. Hello **vyberte oprávnění k souboru** otevře okno. Zobrazí seznam všech účtů Data Lake Store hello ve vašem předplatném.
2. Pozastavte ukazatel myši (neklikejte na) hello myši nad hello název hello toomake hello políčko viditelná, pak vyberte hello políčko účtu Data Lake Store.

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "přidat služby hlavní tooHDInsight clusteru")

  Ve výchozím nastavení __ČÍST__, __zápisu__, a __EXECUTE__ jsou vybrané všechny.

3. Klikněte na tlačítko **vyberte** na hello dolní části stránky hello.
4. Klikněte na tlačítko **spustit** tooassign oprávnění.
5. Klikněte na **Done** (Hotovo).

**tooassign oprávnění na úrovni kořenového clusteru HDInsight hello**

1. Na hello **Data Lake Store přístup** okně klikněte na tlačítko **přístup**. Hello **vyberte oprávnění k souboru** otevře okno. Zobrazí seznam všech účtů Data Lake Store hello ve vašem předplatném.
1. Z hello **vyberte oprávnění k souboru** okně klikněte na tlačítko tooshow název Data Lake Store hello jeho obsah.
2. Vyberte kořenové úložiště clusteru HDInsight hello hello zaškrtávací políčko je na levé straně hello hello složky. Snímek obrazovky toohello podle dříve, kořenová úložiště clusteru hello je __/clusterů__ složky, kterou jste zadali, že při výběru hello Data Lake Store jako výchozí úložiště.
3. Hello složku nastavit oprávnění hello.  Ve výchozím nastavení, číst, zapisovat a spouštět jsou vybrané všechny.
4. Klikněte na tlačítko **vyberte** na hello dolní části stránky hello.
5. Klikněte na **Run** (Spustit).
6. Klikněte na **Done** (Hotovo).

Pokud používáte Data Lake Store jako další úložiště, je nutné přiřadit oprávnění pouze pro hello složky, které chcete tooaccess z clusteru HDInsight hello. Například hello následující snímek obrazovky, poskytnete přístup pouze příliš**hdiaddonstorage** složek v účtu Data Lake Store.

![Přiřadit clusteru HDInsight toohello hlavní oprávnění služby](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "clusteru HDInsight toohello hlavní oprávnění služby přiřazení")


## <a name="verify-cluster-set-up"></a>Ověřte nastavení clusteru

Po dokončení instalace clusteru hello v okně clusteru hello, ověřte výsledky provedením jedné nebo obou hello následující kroky:

* tooverify, který hello přidruženého úložiště pro hello cluster je hello účtu Data Lake Store, který jste zadali, klikněte na tlačítko **účty úložiště** v levém podokně hello.

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "přidat služby hlavní tooHDInsight clusteru")

* tooverify, který hello instanční objekt správně souvisí s clusterem HDInsight hello, klikněte na tlačítko **Data Lake Store přístup** v levém podokně hello.

    ![Přidat službu hlavní tooHDInsight clusteru](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "přidat služby hlavní tooHDInsight clusteru")


## <a name="examples"></a>Příklady

Poté, co jste nastavili jako úložiště clusteru hello s Data Lake Store, naleznete v nápovědě toothese příklady jak toouse HDInsight clusteru tooanalyze hello data uložená v Data Lake Store.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-primary-storage"></a>Spouštění dotazů Hive proti dat v Data Lake Store (jako primární úložiště)

toorun Hive dotaz, použít hello Hive zobrazení rozhraní Ambari portálu hello. Pokyny jak toouse Ambari Hive zobrazení, najdete v části [použití hello zobrazení Hive se systémem Hadoop v HDInsight](../hdinsight/hdinsight-hadoop-use-hive-ambari-view.md).

Při práci s daty v Data Lake Store, existuje několik toochange řetězce.

Pokud chcete použít, například hello cluster, který jste vytvořili s Data Lake Store jako primární úložiště, data toohello cesty hello je: *adl: / / < data_lake_store_account_name > /azuredatalakestore.net/path/to/file*. Toocreate dotaz Hive tabulku ze ukázková data, která je uložená v účtu Data Lake Store hello vypadá hello následující příkaz:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Popis:
* `adl://hdiadlstorage.azuredatalakestore.net/`je kořenový adresář hello hello účtu Data Lake Store.
* `/clusters/myhdiadlcluster`je kořenový adresář hello hello dat clusteru, který jste zadali při vytváření clusteru hello.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/`je hello umístění hello ukázkového souboru, který jste použili v dotazu hello.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-additional-storage"></a>Spouštění dotazů Hive proti dat v Data Lake Store (jako další úložiště)

Pokud hello cluster, který jste vytvořili používá jako výchozí úložiště Blob storage, hello ukázková data není obsažen v hello účtu Azure Data Lake Store, který se používá jako další úložiště. V takovém případě nejprve přenosu hello dat z objektu Blob úložiště toohello Data Lake Store a pak spusťte hello dotazy, jak je uvedeno v předchozím příkladu hello.

Informace o tom, jak toocopy úložiště dat, z úložiště objektů Blob tooa Data Lake najdete v tématu hello následující články:

* [Použití Distcp toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store](data-lake-store-copy-data-wasb-distcp.md)
* [Použijte AdlCopy toocopy data ze služby Azure Storage objekty BLOB tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-store-with-a-spark-cluster"></a>Použití Data Lake Store s clusterem Spark
Můžete vytvořit úloh Spark toorun clusteru Spark na data, která je uložená v Data Lake Store. Další informace najdete v tématu [použití HDInsight Spark clusteru tooanalyze dat v Data Lake Store](../hdinsight/hdinsight-apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Použití Data Lake Store v topologii Storm
Hello Data Lake Store toowrite dat můžete použít z topologie Storm. Návod, jak tooachieve v tomto scénáři najdete v části [pomocí Azure Data Lake Store s Apache Storm v prostředí HDInsight](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## <a name="see-also"></a>Viz také
* [Prostředí PowerShell: Vytvoření toouse clusteru HDInsight Data Lake Store](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
