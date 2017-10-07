---
title: "toocreate aaaUse šablony Azure HDInsight a Data Lake Store | Microsoft Docs"
description: "Použijte toocreate šablony Azure Resource Manager a clusterů HDInsight pomocí Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: nitinme
ms.openlocfilehash: eb88a626f2837dcc29295f3f73a91757059c3bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a>Vytvoření clusteru HDInsight s Data Lake Store pomocí šablony Azure Resource Manageru
> [!div class="op_single_selector"]
> * [Pomocí portálu](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Pomocí prostředí PowerShell (pro výchozí úložiště)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Pomocí prostředí PowerShell (pro další úložiště)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Pomocí Správce prostředků](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Zjistěte, jak tooconfigure toouse prostředí Azure PowerShell HDInsight cluster s Azure Data Lake Store, **jako další úložiště**.

Pro typy podporované clusteru Data Lake Store použít jako výchozí úložiště nebo účtu další úložiště. Pokud se použije jako další úložiště Data Lake Store, hello výchozí účet úložiště pro clustery hello budou mít pořád Azure úložiště objektů BLOB (WASB) a hello clusteru související soubory (například protokoly atd.) se zapisují stále toohello výchozí úložiště, při hello dat, které chtít tooprocess můžou být uložené v účtu Data Lake Store. Pomocí Data Lake Store jako další úložiště účet nemá negativní vliv na výkon nebo hello možnost tooread a zápis toohello úložiště z clusteru hello.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Pro úložiště clusteru HDInsight pomocí Data Lake Store

Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:

* Clustery HDInsight možnost toocreate s přístupem k tooData Lake Store jako výchozí úložiště je k dispozici pro HDInsight verze 3.5 a 3.6.

* Clustery HDInsight možnost toocreate s přístupem k tooData Lake Store jako další úložiště je k dispozici pro HDInsight verze 3.2, 3,4, 3.5 a 3.6.

V tomto článku jsme zřízení clusteru Hadoop s Data Lake Store jako další úložiště. Pokyny jak toocreate Hadoop cluster s Data Lake Store jako výchozí úložiště, najdete v části [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 nebo vyšší**. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* **Azure Active Directory instanční objekt**. Kroky v tomto kurzu poskytují pokyny o tom, toocreate objektu služby ve službě Azure AD. Ale musí být webu možné toocreate pro Azure AD správce toobe hlavní název služby. Pokud jste správce Azure AD, můžete přeskočit tento požadavek a pokračovat v kurzu hello.

    **Pokud si nejste správce Azure AD**, nebudete moct tooperform hello kroky požadované toocreate hlavní název služby. V takovém případě musíte vám správce Azure AD nejdřív vytvořit objekt služby, před vytvořením clusteru HDInsight s Data Lake Store. Navíc hello instanční objekt musí být vytvořen pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a>Vytvoření clusteru HDInsight s Azure Data Lake Store
Hello šablony Resource Manageru a hello požadavků na používání hello šablony jsou dostupné na webu GitHub na [nasadit cluster HDInsight Linux s novou Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Postupujte podle pokynů hello na tento odkaz toocreate zadaný cluster služby HDInsight s Azure Data Lake Store jako další úložiště hello.

odkaz hello výše uvedených pokynů Hello vyžadují prostředí PowerShell. Než začnete pomocí těchto pokynů, zkontrolujte, zda že přihlásit tooyour účet Azure. Z plochy otevřete nové okno Azure PowerShell a zadejte následující fragmenty hello. Když výzvami toolog, ujistěte se můžete přihlásit jako jeden ze správců/vlastník předplatného hello:

```
# Log in tooyour Azure account
Login-AzureRmAccount

# List all hello subscriptions associated tooyour account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-toohello-azure-data-lake-store"></a>Nahrát ukázková data toohello Azure Data Lake Store
šablony Resource Manageru Hello vytvoří nový účet Data Lake Store a přidruží ji s clusterem HDInsight hello. Teď musíte nahrát některých ukázkových dat toohello Data Lake Store. Budete potřebovat později v hello úlohy kurz toorun z clusteru služby HDInsight, které přístup k datům v Data Lake Store hello tato data. Pokyny najdete v části tooupload data [nahrát soubor tooyour Data Lake Store](data-lake-store-get-started-portal.md#uploaddata). Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).

## <a name="set-relevant-acls-on-hello-sample-data"></a>Nastavit příslušné seznamy ACL na hello ukázková data
toomake se, že je dostupný z clusteru HDInsight hello hello ukázková data, který nahrajete, musíte zajistit, že aplikace hello Azure AD, která je použité tooestablish identity mezi hello HDInsight cluster a Data Lake Store má přístup toohello soubor nebo složku, která se Probíhá tooaccess. toodo, proveďte následující kroky hello.

1. Najít název hello hello aplikaci Azure AD, který je přidružen HDInsight cluster a hello Data Lake Store. Jedním ze způsobů toolook pro název hello je tooopen hello HDInsight clusteru okno, které jste vytvořili pomocí šablony Resource Manageru hello, klikněte na tlačítko hello **identita AAD clusteru** kartě a vyhledejte hodnotu hello **instančního objektu Zobrazovaný název**.
2. Nyní poskytují přístup toothis aplikaci Azure AD na hello soubor nebo složku, které chcete tooaccess z clusteru HDInsight hello. tooset hello vpravo seznamy ACL na hello soubor nebo složku v Data Lake Store najdete v části [zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a>Spustit testovací úlohy na hello HDInsight clusteru toouse hello Data Lake Store
Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na hello clusteru tootest této hello HDInsight clusteru můžete získat přístup k Data Lake Store. toodo tedy jsme se spustí úlohy Hive ukázka, která vytvoří tabulku pomocí hello ukázková data, který jste nahráli starší tooyour Data Lake Store.

V této části budete SSH do služby clusteru HDInsight Linux a spuštění hello ukázkový dotaz Hive. Pokud používáte klienta se systémem Windows, doporučujeme používat **PuTTY**, který si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití klienta PuTTY, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Po připojení, spusťte hello Hive rozhraní příkazového řádku pomocí hello následující příkaz:

   ```
   hive
   ```
2. Pomocí hello rozhraní příkazového řádku, zadejte následující příkazy toocreate novou tabulku s názvem hello **vozidel** pomocí hello ukázkových dat v Data Lake Store hello:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Měli byste vidět výstup podobný toohello následující:

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a>Přístup k Data Lake Store pomocí příkazů HDFS
Po nakonfigurování hello HDInsight clusteru toouse Data Lake Store můžete hello HDFS prostředí příkazy tooaccess hello úložiště.

V této části budete SSH do služby clusteru HDInsight Linux a spuštění hello příkazů HDFS. Pokud používáte klienta se systémem Windows, doporučujeme používat **PuTTY**, který si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití klienta PuTTY, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Po připojení použijte hello následující soubory o hello toolist příkazů systému souborů HDFS v hello Data Lake Store.

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

To by měl seznamu hello souboru, který jste nahráli starší toohello Data Lake Store.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

Můžete taky hello `hdfs dfs -put` příkaz tooupload některé soubory toohello Data Lake Store a pak použijte `hdfs dfs -ls` tooverify jestli hello soubory úspěšně se nahrál.


## <a name="next-steps"></a>Další kroky
* [Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store](data-lake-store-copy-data-wasb-distcp.md)
