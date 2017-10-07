---
title: "aaaRestrict přístup pomocí sdílené přístupové podpisy - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak přistupovat k toouse sdílené přístupové podpisy toorestrict HDInsight, toodata ukládají do objektů BLOB úložiště Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: a34a2f8e52e47a15b09f09bc1fc67fc6159ec75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-shared-access-signatures-toorestrict-access-toodata-in-hdinsight"></a>Použití Azure úložiště sdílené přístupové podpisy toorestrict přístup toodata v HDInsight

HDInsight má úplný přístup toodata v účtech úložiště Azure hello přidruženého k hello clusteru. Můžete vytvořit sdílené přístupové podpisy hello blob kontejneru toorestrict přístup toohello data. Například tooprovide oprávnění jen pro čtení toohello data. Podpisy sdíleného přístupu (SAS) jsou funkce účtů úložiště Azure, která vám umožní toodata toolimit přístup. Například poskytuje toodata oprávnění jen pro čtení.

> [!IMPORTANT]
> Řešení pomocí Apache škálu zvažte použití HDInsight připojený k doméně. Další informace najdete v tématu hello [konfigurace připojený k doméně HDInsight](hdinsight-domain-joined-configure.md) dokumentu.

> [!WARNING]
> HDInsight musí mít plný přístup toohello výchozí úložiště pro hello cluster.

## <a name="requirements"></a>Požadavky

* Předplatné Azure
* C# nebo Python. Příklad kódu C# je k dispozici jako řešení sady Visual Studio.

  * Visual Studio musí být ve verzi 2013, 2015 nebo 2017
  * Python musí být verze 2.7 nebo vyšší

* Cluster HDInsight se systémem Linux nebo [prostředí Azure PowerShell] [ powershell] – Pokud máte existující cluster založený na Linuxu, můžete použít Ambari tooadd cluster toohello podpis sdíleného přístupu. Pokud ne, můžete použít Azure PowerShell toocreate cluster a přidání sdíleného přístupového podpisu během vytváření clusteru.

    > [!IMPORTANT]
    > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Hello příklad soubory z [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Toto úložiště obsahuje hello následující položky:

  * Projekt sady Visual Studio, který můžete vytvořit kontejner úložiště, uložené zásady a SAS pro použití s HDInsight
  * Skript v jazyce Python, můžete vytvořit kontejner úložiště, uložené zásady a SAS pro použití s HDInsight
  * Skript prostředí PowerShell, který může vytvářet HDInsight clusteru a nakonfigurujte ji toouse hello SAS.

## <a name="shared-access-signatures"></a>Sdílené přístupové podpisy

Existují dvě formy sdílené přístupové podpisy:

* Ad hoc: hello času spuštění, čas vypršení platnosti a oprávnění pro hello SAS nejsou zadány na hello identifikátor URI pro SAS.

* Uložené zásady přístupu: zásady uložené přístupu je definován na kontejner prostředků, jako je například kontejner objektů blob. Zásadu lze použít toomanage omezení pro jeden nebo více sdílených přístupových podpisů. Pokud přidružíte SAS se zásadami přístupu uložené, hello SAS dědí omezení hello – hello času spuštění, čas vypršení platnosti a - definována pro zásady přístupu hello uložené oprávnění.

Hello rozdíl mezi formuláři hello dva je důležité pro jeden klíč scénář: odvolaných certifikátů. SAS je adresu URL, takže každý, kdo získává hello SAS je můžete použít, bez ohledu na to, kdo ho požadovaný toobegin s. Pokud veřejně publikována SAS, můžete použít každý, vítáme. SAS, který je distribuován je platný, dokud jednu ze čtyř akcí se stane:

1. čas vypršení platnosti Hello zadaný na hello dosaženo SAS.

2. čas vypršení platnosti Hello zadaný v zásadách přístupu hello uložené odkazuje hello dosaženo SAS. Hello následující scénáře způsobit čas vypršení platnosti hello toobe dosaženo:

    * časový interval Hello uplynul.
    * zásady přístupu Hello uložené je upravený toohave čas vypršení platnosti v posledních hello. Změna hello čas vypršení platnosti je jedním ze způsobů toorevoke hello SAS.

3. Hello uložené zásady přístupu odkazuje hello odstranění SAS, který je jiný způsob toorevoke hello SAS. Pokud je znovu vytvořit zásady přístupu hello uložené s hello stejný název, všechny tokeny SAS pro předchozí zásady hello jsou platné (Pokud hello čas vypršení platnosti na hello neuplynul SAS). Pokud hodláte toorevoke hello SAS, mít jistotu toouse jiný název znovu hello zásady přístupu, doba vypršení platnosti v budoucnu hello.

4. Hello klíč účtu, který byl použité toocreate hello SAS vygenerován znovu. Opakované generování klíče hello způsobí, že všechny aplikace, které používají hello předchozí klíče toofail ověřování. Aktualizujte všechny součásti toohello nový klíč.

> [!IMPORTANT]
> Sdílený přístupový podpis identifikátor URI je přidružen hello účet klíče používané toocreate hello podpisu a hello související zásady uložené přístupu (pokud existuje). Pokud je zadána žádná zásada uložené přístup, hello pouze způsob toorevoke sdílený přístupový podpis je klíč účtu toochange hello.

Doporučujeme vždy použít zásady uložené přístupu. Pokud používáte uložené zásady, můžete odvolat podpisy nebo prodloužit datum vypršení platnosti hello podle potřeby. Hello kroky v tomto dokumentu používají uložené přístup zásady toogenerate SAS.

Další informace o sdílených přístupových podpisů najdete v tématu [vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Vytvoření uložené zásady a SAS pomocí jazyka C\#

1. Otevřete hello řešení v sadě Visual Studio.

2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **SASToken** projektu a vyberte **vlastnosti**.

3. Vyberte **nastavení** a přidejte hodnoty pro hello následující položky:

   * StorageConnectionString: hello připojovací řetězec pro hello účet úložiště, které chcete toocreate uložené zásady a SAS pro. Hello formát by měl být `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` kde `myaccount` je hello název účtu úložiště a `mykey` je hello klíč pro účet úložiště hello.

   * ContainerName: hello kontejneru v účtu úložiště hello, který chcete toorestrict přístup k.

   * SASPolicyName: hello název toouse pro hello uložené toocreate zásad.

   * FileToUpload: hello cesta tooa soubor, který je nahraný toohello kontejner.

4. Spusťte projekt hello. Po vygenerování hello SAS, zobrazí se informace podobné toohello následující text:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Uložte tokenu zásad hello SAS, název účtu úložiště a název kontejneru. Tyto hodnoty se používají při přidružení účtu úložiště hello k vašemu clusteru HDInsight.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Vytvoření uložené zásady a SAS používá Python

1. Otevřete soubor SASToken.py hello a změnit hello následující hodnoty:

   * zásady\_název: hello název toouse pro hello uložené toocreate zásad.

   * úložiště\_účet\_název: hello název účtu úložiště.

   * úložiště\_účet\_klíč: hello klíč pro účet úložiště hello.

   * úložiště\_kontejneru\_název: hello kontejneru v účtu úložiště hello, který chcete toorestrict přístup k.

   * Příklad\_souboru\_cesta: hello cesta tooa soubor, který je nahraný toohello kontejneru.

2. Spusťte skript hello. Zobrazuje hello SAS token podobné toohello po dokončení skriptu hello následující text:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Uložte tokenu zásad hello SAS, název účtu úložiště a název kontejneru. Tyto hodnoty se používají při přidružení účtu úložiště hello k vašemu clusteru HDInsight.

## <a name="use-hello-sas-with-hdinsight"></a>Použijte hello SAS s HDInsight

Při vytváření clusteru služby HDInsight, musíte zadat účet primárního úložiště a volitelně můžete zadat další účty úložiště. Obě tyto metody přidávání úložiště vyžadují účty úložiště toohello plný přístup a kontejnerů, které se používají.

toouse kontejner sdíleného přístupového podpisu toolimit přístup tooa přidat vlastní položku toohello **core-site** konfiguraci pro hello cluster.

* Pro **založené na Windows** nebo **systémem Linux** clusterů HDInsight, můžete přidat položku hello při vytváření clusteru pomocí prostředí PowerShell.
* Pro **systémem Linux** clusterů HDInsight, změnit po vytvoření clusteru pomocí nástroje Ambari konfiguraci hello.

### <a name="create-a-cluster-that-uses-hello-sas"></a>Vytvoření clusteru, který používá hello SAS

Příklad vytvoření clusteru HDInsight této hello používá SAS je součástí hello `CreateCluster` adresář hello úložiště. toouse, které ho hello použijte následující kroky:

1. Otevřete hello `CreateCluster\HDInsightSAS.ps1` soubor v textovém editoru a upravte hello následující hodnoty od začátku hello hello dokumentu.

    ```powershell
    # Replace 'mycluster' with hello name of hello cluster toobe created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with hello name of hello group toobe created
    $resourceGroupName = 'myresourcegroup'
    # Replace with hello Azure data center you want toohello cluster toolive in
    $location = 'North Europe'
    # Replace with hello name of hello default storage account toobe created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with hello name of hello SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with hello name of hello SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with hello SAS token generated earlier
    $SASToken = 'sastoken'
    # Set hello number of worker nodes in hello cluster
    $clusterSizeInNodes = 3
    ```

    Můžete například změnit `'mycluster'` toohello název clusteru hello chcete toocreate. hodnoty Hello SAS by měl odpovídat hello hodnoty z předchozích kroků hello při vytváření účtu úložiště a tokenu SAS.

    Po změně hodnoty hello se hello soubor uložte.

2. Otevření nového příkazového řádku Azure PowerShell. Pokud nejste obeznámeni s prostředím Azure PowerShell nebo ho nenainstalovali, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell][powershell].

1. Z řádku hello použijte následující příkaz tooauthenticate tooyour předplatného Azure hello:

    ```powershell
    Login-AzureRmAccount
    ```

    Pokud budete vyzváni, přihlaste se pomocí hello účet pro vaše předplatné Azure.

    Pokud váš účet je spojen s několika předplatných Azure, může být nutné toouse `Select-AzureRmSubscription` tooselect hello předplatné chcete toouse.

4. Z příkazového řádku hello změnit adresáře toohello `CreateCluster` adresář, který obsahuje soubor HDInsightSAS.ps1 hello. Pak použijte následující příkaz toorun hello skriptu hello

    ```powershell
    .\HDInsightSAS.ps1
    ```

    Jako hello skript se spustí zaprotokoluje příkazovém řádku prostředí PowerShell toohello výstup jako vytvoří hello prostředků skupiny a úložiště účtů. Jste uživatelem výzvami tooenter hello HTTP pro hello clusteru HDInsight. Tento účet je clusteru toohello přístupu použité toosecure HTTP/s.

    Při vytváření clusteru se systémem Linux, zobrazí se výzva pro název účtu uživatele SSH a heslo. Tento účet je přihlášení použité tooremotely toohello clusteru.

   > [!IMPORTANT]
   > Po zobrazení výzvy k hello HTTP/s nebo SSH uživatelské jméno a heslo, je nutné zadat heslo, které splňuje hello následující kritéria:
   >
   > * Musí být minimálně 10 znaků.
   > * Musí obsahovat nejméně jednu číslici
   > * Musí obsahovat alespoň jeden jiný než alfanumerický znak
   > * Musí obsahovat aspoň jedno velké nebo malé písmeno.

Jak dlouho trvá chvíli toocomplete tento skript obvykle přibližně 15 minut. Po dokončení skriptu hello bez chyb, byl vytvořen hello cluster.

### <a name="use-hello-sas-with-an-existing-cluster"></a>Hello SAS pomocí stávajícího clusteru

Pokud máte existující cluster založený na Linuxu, můžete přidat hello SAS toohello **core-site** konfigurace pomocí hello následující kroky:

1. Otevřete hello Ambari webového uživatelského rozhraní pro váš cluster. Hello adresa pro tuto stránku je https://YOURCLUSTERNAME.azurehdinsight.net. Po zobrazení výzvy ověřování clusteru toohello pomocí hello jméno správce (správce) a heslo použité při vytváření clusteru hello.

2. Hello levé straně hello webovému uživatelskému rozhraní Ambari, vyberte **HDFS** a pak vyberte hello **konfigurací** kartě uprostřed hello stránku hello.

3. Vyberte hello **Upřesnit** kartě a potom přejděte najde hello **vlastní základní site** části.

4. Rozbalte hello **vlastní základní site** části potom posuňte toohello end a vyberte hello **přidat vlastnost...**  odkaz. Použití hello následující hodnoty pro hello **klíč** a **hodnotu** pole:

   * **Klíč**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Hodnota**: hello SAS vrácený hello jazyka C# nebo Python aplikace byla spuštěna dříve

     Nahraďte **CONTAINERNAME** názvem hello kontejneru, který jste použili s aplikací hello jazyka C# nebo SAS. Nahraďte **STORAGEACCOUNTNAME** s názvem účtu úložiště hello jste použili.

5. Klikněte na tlačítko hello **přidat** tlačítko toosave tento klíč a hodnotu a pak klikněte na hello **Uložit** tlačítko změny konfigurace toosave hello. Po zobrazení výzvy, přidejte popis změny hello ("Přidat přístup k úložišti SAS" třeba) a potom klikněte na **Uložit**.

    Klikněte na tlačítko **OK** když byly dokončeny hello změny.

   > [!IMPORTANT]
   > Hello změna se projeví až po restartování několik služeb.

6. V hello webovému uživatelskému rozhraní Ambari, vyberte **HDFS** hello seznamu na levé hello a potom vyberte **restartujte všechny** z hello **služby akce** rozevíracím seznamu na pravé hello. Po zobrazení výzvy vyberte **zapnout režim údržby** a pak vyberte __Conform restartujte všechny ".

    Tento postup opakujte pro MapReduce2 a YARN.

7. Po restartování služby hello, vyberte každé z nich a zakázat režimu údržby od hello **služby akce** rozevírací nabídku.

## <a name="test-restricted-access"></a>Testování s omezeným přístupem

tooverify, mají omezený přístup, použijte hello následující metody:

* Pro **založené na Windows** clusterů HDInsight pomocí vzdálené plochy tooconnect toohello clusteru. Další informace najdete v tématu [připojit pomocí protokolu RDP tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    Po připojení používat hello **Hadoop příkazového řádku** ikony na ploše tooopen hello příkazový řádek.

* Pro **systémem Linux** clusterů HDInsight pomocí SSH tooconnect toohello clusteru. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Po připojení toohello clusteru, použijte následující kroky tooverify, můžete pouze pro čtení a seznam položek v účtu úložiště SAS hello hello:

1. obsah hello toolist hello kontejneru, použijte následující příkaz z řádku hello hello: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Nahraďte **SASCONTAINER** s názvem hello hello kontejneru vytvořené pro hello účet úložiště SAS. Nahraďte **SASACCOUNTNAME** hello název účtu úložiště hello používá pro hello SAS.

    Hello seznamu zahrnuje nahrát hello kontejneru a SAS čas vytvoření souboru hello.

2. Použijte následující příkaz tooverify, abyste si přečetli hello obsah souboru hello hello. Nahraďte hello **SASCONTAINER** a **SASACCOUNTNAME** jako v předchozím kroku hello. Nahraďte **FILENAME** s názvem hello hello souboru se zobrazí v předchozí příkaz hello:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Tento příkaz vypíše hello obsah souboru hello.

3. Použijte následující příkaz toodownload hello toohello místní systém souborů hello:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Tento příkaz stáhne hello souboru tooa místního souboru s názvem **testfile.txt**.

4. Použití hello následující příkaz tooupload hello místního souboru tooa nový soubor s názvem **testupload.txt** na hello úložiště SAS:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Zobrazí se zpráva podobné toohello následující text:

        put: java.io.IOException

    K této chybě dojde, protože umístění úložiště hello je čtení + pouze seznam. Použijte následující příkaz tooput hello data na hello výchozí úložiště pro cluster hello, který lze zapisovat hello:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Tentokrát hello operace by měl úspěšně dokončit.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="a-task-was-canceled"></a>Úloha byla zrušena.

**Příznaky**: při vytváření clusteru pomocí skriptu prostředí PowerShell text hello, se může zobrazit hello následující chybová zpráva:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Příčina**: k této chybě může dojít, pokud použijete heslo pro uživatele správce nebo HTTP hello hello clusteru nebo (pro clustery se systémem Linux) uživatel SSH hello.

**Řešení**: použít heslo, které splňuje hello následující kritéria:

* Musí být minimálně 10 znaků.
* Musí obsahovat nejméně jednu číslici
* Musí obsahovat alespoň jeden jiný než alfanumerický znak
* Musí obsahovat aspoň jedno velké nebo malé písmeno.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak tooyour tooadd omezený přístup úložiště clusteru HDInsight, zjistěte další způsoby toowork s daty v clusteru:

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
