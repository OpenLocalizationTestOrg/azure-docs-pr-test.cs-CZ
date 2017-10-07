---
Title: aaa "prostředí PowerShell: cluster Azure HDInsight s Data Lake Store jako přídavná služba storage | Služby Microsoft Docs": data lake store, hdinsight documentationcenter:" Autor: nitinme správce: jhubbard editor: cgronlun

MS.AssetID: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: úložiště dat lake ms.devlang: na ms.topic: článek ms.tgt_pltfrm: na ms.workload: velkých objemů dat ms.date: ms.author 06/08/2017: nitinme

---
# <a name="use-azure-powershell-toocreate-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a>Použití Azure PowerShell toocreate clusteru HDInsight s Data Lake Store (jako další úložiště)
> [!div class="op_single_selector"]
> * [Pomocí portálu](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Pomocí prostředí PowerShell (pro výchozí úložiště)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Pomocí prostředí PowerShell (pro další úložiště)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Pomocí Správce prostředků](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Zjistěte, jak tooconfigure toouse prostředí Azure PowerShell HDInsight cluster s Azure Data Lake Store, **jako další úložiště**. Pokyny jak toocreate HDInsight, cluster s Azure Data Lake Store jako výchozí úložiště, najdete v části [vytvoření clusteru HDInsight s Data Lake Store jako výchozí úložiště](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> Pokud chcete toouse Azure Data Lake Store jako další úložiště pro HDInsight cluster, důrazně doporučujeme, abyste to provedli při vytváření clusteru hello, jak je popsáno v tomto článku. Přidání Azure Data Lake Store jako další úložiště tooan stávající cluster HDInsight je složitý proces a tooerrors náchylné k chybám.
>

Pro typy podporované clusteru slouží jako výchozí úložiště nebo účtu další úložiště Data Lake Store. Pokud se použije jako další úložiště Data Lake Store, hello výchozí účet úložiště pro clustery hello budou mít pořád Azure úložiště objektů BLOB (WASB) a hello clusteru související soubory (například protokoly atd.) se zapisují stále toohello výchozí úložiště, při hello dat, které chtít tooprocess můžou být uložené v účtu Data Lake Store. Pomocí Data Lake Store jako další úložiště účet nemá negativní vliv na výkon nebo hello možnost tooread a zápis toohello úložiště z clusteru hello.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Pro úložiště clusteru HDInsight pomocí Data Lake Store

Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:

* Clustery HDInsight možnost toocreate s přístupem k tooData Lake Store jako další úložiště je k dispozici pro HDInsight verze 3.2, 3,4, 3.5 a 3.6.

Konfigurace HDInsight s Data Lake Store pomocí prostředí PowerShell toowork zahrnuje hello následující kroky:

* Vytvoření Azure Data Lake Store
* Nastavení ověřování pro založené na rolích přístup tooData Lake Store
* Vytvoření clusteru HDInsight s ověřování tooData Lake Store
* Spustit úlohu testu v clusteru hello

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 nebo vyšší**. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* **Windows SDK**. Můžete nainstalovat z [zde](https://dev.windows.com/en-us/downloads). Můžete použít tento toocreate certifikát zabezpečení.
* **Azure Active Directory instanční objekt**. Kroky v tomto kurzu poskytují pokyny o tom, toocreate objektu služby ve službě Azure AD. Ale musí být webu možné toocreate pro Azure AD správce toobe hlavní název služby. Pokud jste správce Azure AD, můžete přeskočit tento požadavek a pokračovat v kurzu hello.

    **Pokud si nejste správce Azure AD**, nebudete moct tooperform hello kroky požadované toocreate hlavní název služby. V takovém případě musíte vám správce Azure AD nejdřív vytvořit objekt služby, před vytvořením clusteru HDInsight s Data Lake Store. Navíc hello instanční objekt musí být vytvořen pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-azure-data-lake-store"></a>Vytvoření Azure Data Lake Store
Postupujte podle těchto kroků toocreate Data Lake Store.

1. Z plochy otevřete nové okno Azure PowerShell a zadejte hello následující fragment kódu. Když výzvami toolog, ujistěte se můžete přihlásit jako jeden hello správce/vlastník předplatného:

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Pokud narazíte na chyby, které jsou podobné příliš`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` při registraci zprostředkovatele prostředků hello Data Lake Store, je možné, že vaše předplatné není povolený pro Azure Data Lake Store. Ujistěte se, aktivujte předplatné Azure pro verzi public preview služby Data Lake Store pomocí následujících tyto [pokyny](data-lake-store-get-started-portal.md).
   >
   >
2. Účet Azure Data Lake Store je přidružený ke skupině prostředků Azure. Začněte vytvořením skupiny prostředků Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Měli byste vidět výstup takto:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Vytvořte účet Azure Data Lake Store. účet Hello název, který zadáte, musí obsahovat jenom malá písmena a číslice.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    Měli byste vidět výstup jako hello následující:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

5. Nahrajte několik ukázkových dat tooAzure Data Lake. Použijeme ho později v tooverify tohoto článku, hello data jsou přístupná z clusteru služby HDInsight. Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path toodata>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a>Nastavení ověřování pro založené na rolích přístup tooData Lake Store
Každé předplatné služby Azure souvisí s Azure Active Directory. Uživatelů a služeb, která přistupují k prostředkům pomocí hello portálu Azure Classic nebo rozhraní API služby Azure Resource Manager předplatného hello musí nejprve ověřit pomocí této služby Azure Active Directory. Přístup uděluje přiřazením příslušné role hello na prostředek služby Azure tooAzure odběry a služby.  Objekt služby pro služby, identifikuje hello služby v hello Azure Active Directory (AAD). Tato část ukazuje, jak toogrant aplikace služby, stejně jako HDInsight, tooan přístup prostředků Azure (hello účet Azure Data Lake Store, které jste vytvořili dříve) vytvořením objekt služby pro aplikaci hello a přiřadit role toothat přes Azure Prostředí PowerShell.

tooset až ověřování služby Active Directory pro Azure Data Lake, je nutné provést hello následující úlohy.

* Vytvořit certifikát podepsaný svým držitelem
* Vytvoření aplikace v Azure Active Directory a objektu služby

### <a name="create-a-self-signed-certificate"></a>Vytvořit certifikát podepsaný svým držitelem
Zajistěte, aby byla [Windows SDK](https://dev.windows.com/en-us/downloads) nainstalovala ještě před pokračováním hello kroky v této části. Musíte také vytvořit adresář, jako například **C:\mycertdir**, kde bude vytvořen hello certifikátu.

1. V okně Powershellu hello přejděte toohello umístění, kam jste nainstalovali Windows SDK (obvykle `C:\Program Files (x86)\Windows Kits\10\bin\x86` a používat hello [MakeCert] [ makecert] nástroj toocreate certifikát podepsaný svým držitelem a privátní klíč. Použijte následující příkazy hello.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Bude výzvami tooenter hello heslo soukromého klíče. Po hello příkaz úspěšně provede, měli byste vidět **CertFile.cer** a **mykey.pvk** v adresáři certifikát hello jste zadali.
2. Použití hello [Pvk2Pfx] [ pvk2pfx] nástroj tooconvert hello pvk a .cer soubory tento soubor .pfx vytvořený tooa MakeCert. Spusťte následující příkaz hello.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Po zobrazení výzvy zadejte heslo hello privátní klíče jste dřív zadali. Hodnota zadaná pro hello Hello **-SP** parametr je hello heslo, které souvisí s hello soubor .pfx. Po úspěšném dokončení příkazu hello, měli byste taky vidět CertFile.pfx v určeném adresáři hello certifikátu.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Vytvoření služby Azure Active Directory a objektu služby
V této části provádět kroky toocreate hello objekt služby pro aplikaci Azure Active Directory, přiřaďte objekt role toohello služby a ověření jako hello instanční objekt tím, že poskytuje certifikát. Spusťte následující příkazy toocreate hello aplikace v Azure Active Directory.

1. Vložte následující rutiny v okně konzoly prostředí PowerShell hello hello. Ujistěte se, zda text hello hodnota zadaná pro hello **– DisplayName** vlastnost je jedinečný. Navíc hello hodnoty pro **– Domovská stránka** a **- IdentiferUris** jsou zástupné hodnoty a nejsou ověřené.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter hello password" # This is hello password you specified for hello .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Vytvořit objekt služby pomocí ID hello aplikace.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Udělit hello služby hlavní přístup toohello Data Lake Store složku a soubor hello, kterému bude přístup z clusteru HDInsight hello. Následující fragment Hello poskytuje přístup toohello kořenovém hello Data Lake Store účtu (které jste zkopírovali hello ukázkový datový soubor) a hello samotném souboru.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a>Vytvoření clusteru služby HDInsight Linux s Data Lake Store jako další úložiště

V této části vytvoříme cluster služby HDInsight Hadoop Linux s Data Lake Store jako další úložiště. Pro tuto verzi clusteru HDInsight hello a hello Data Lake Store musí být v hello stejné umístění.

1. Začněte s načítání ID hello předplatného klienta. Budete potřebovat, který později.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. Pro tuto verzi pro Hadoop cluster, Data Lake Store se použít jenom jako další úložiště pro hello cluster. Hello výchozí úložiště budou mít pořád hello objektů BLOB Azure storage (WASB). Proto nejdřív vytvoříme hello účet a úložiště kontejnery úložiště potřebné pro hello cluster.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Vytvoření clusteru HDInsight se hello. Pomocí následující rutiny hello.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have hello same name for hello cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Po úspěšném dokončení hello rutiny, měli byste vidět výstup hello clusteru podrobnosti výpisu.


## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a>Spustit testovací úlohy na hello HDInsight clusteru toouse hello Data Lake Store
Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na hello clusteru tootest této hello HDInsight clusteru můžete získat přístup k Data Lake Store. toodo tedy jsme se spustí úlohy Hive ukázka, která vytvoří tabulku pomocí hello ukázková data, který jste nahráli starší tooyour Data Lake Store.

V této části se SSH do clusteru HDInsight Linux můžete vytvořit a spustit ukázkový dotaz Hive hello hello.

* Pokud používáte tooSSH klienta Windows do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Pokud používáte klienta tooSSH Linux do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Po připojení, spusťte hello Hive rozhraní příkazového řádku pomocí hello následující příkaz:

        hive
2. Pomocí hello rozhraní příkazového řádku, zadejte následující příkazy toocreate novou tabulku s názvem hello **vozidel** pomocí hello ukázkových dat v Data Lake Store hello:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Měli byste vidět výstup podobný toohello následující:

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

## <a name="access-data-lake-store-using-hdfs-commands"></a>Přístup k Data Lake Store pomocí příkazů HDFS
Po nakonfigurování hello HDInsight clusteru toouse Data Lake Store můžete hello HDFS prostředí příkazy tooaccess hello úložiště.

V této části se SSH do clusteru HDInsight Linux můžete vytvořit a spustit příkazy HDFS hello hello.

* Pokud používáte tooSSH klienta Windows do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Pokud používáte klienta tooSSH Linux do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Po připojení použijte hello následující soubory o hello toolist příkazů systému souborů HDFS v hello Data Lake Store.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

To by měl seznamu hello souboru, který jste nahráli starší toohello Data Lake Store.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Můžete taky hello `hdfs dfs -put` příkaz tooupload některé soubory toohello Data Lake Store a pak použijte `hdfs dfs -ls` tooverify jestli hello soubory úspěšně se nahrál.

## <a name="see-also"></a>Viz také
* [Portál: Vytvoření toouse clusteru HDInsight Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
