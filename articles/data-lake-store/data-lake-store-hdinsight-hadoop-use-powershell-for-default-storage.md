---
title: "aaaCreate HDInsight clustery s Data Lake Store jako výchozí úložiště pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí Azure PowerShell toocreate a clusterů HDInsight pomocí Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/08/2017
ms.author: nitinme
ms.openlocfilehash: a5c0ad416da6ad9bd07204af2ebb6b7470916085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>Vytvoření clusterů HDInsight s Data Lake Store jako výchozí úložiště pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Hello použití portálu Azure](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Pomocí prostředí PowerShell (pro výchozí úložiště)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Pomocí prostředí PowerShell (pro další úložiště)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Pomocí Správce prostředků](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Zjistěte, jak toouse prostředí Azure PowerShell tooconfigure Azure HDInsight clustery s Azure Data Lake Store, jako výchozí úložiště. Pokyny týkající se vytvoření clusteru HDInsight s Data Lake Store jako další úložiště najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store jako další úložiště](data-lake-store-hdinsight-hadoop-use-powershell.md).

Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:

* clustery HDInsight toocreate Hello možnost s přístupem k tooData Lake Store jako výchozí úložiště je k dispozici pro HDInsight verze 3.5 a 3.6.

* Hello možnost toocreate clusterů HDInsight pomocí přístup tooData Lake Store jako výchozí úložiště je *není k dispozici* u clusterů HDInsight Premium.

tooconfigure toowork HDInsight s Data Lake Store pomocí prostředí PowerShell, postupujte podle pokynů hello v části Další pět hello.

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, ujistěte se, že splňujete hello následující požadavky:

* **Předplatné Azure**: přejděte příliš[získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Prostředí Azure PowerShell 1.0 nebo vyšší**: najdete v části [jak tooinstall a konfigurace prostředí PowerShell](/powershell/azure/overview).
* **Windows Software Development Kit (SDK)**: tooinstall Windows SDK a přejděte příliš[stáhne a nástroje pro Windows 10](https://dev.windows.com/en-us/downloads). Hello SDK je použité toocreate certifikát zabezpečení.
* **Objekt služby Azure Active Directory**: Tento kurz popisuje, jak toocreate objektu služby ve službě Azure Active Directory (Azure AD). Ale toocreate hlavní název služby, musíte být správce Azure AD. Pokud jste správce, můžete přeskočit tento požadavek a pokračovat v kurzu hello.

    >[!NOTE]
    >Pouze v případě, že jste správce Azure AD, vytvořte službu objektu zabezpečení. Správce služby Azure AD musí vytvořte službu objektu zabezpečení před vytvořením clusteru HDInsight s Data Lake Store. Hello instanční objekt musí být vytvořeny pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-store-account"></a>Vytvoření účtu Data Lake Store
toocreate účet Data Lake Store hello následující:

1. Z plochy otevřete okno prostředí PowerShell a potom zadejte níže zobrazené fragmenty kódu hello. Pokud jste výzvami toosign v přihlášení jako správci předplatného hello nebo vlastníky. 

        # Sign in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Pokud registrace poskytovatele prostředků hello Data Lake Store a došlo k chybě, podobně jako příliš`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, vaše předplatné nemusí být seznam povolených adres pro Data Lake Store. tooenable vašeho předplatného Azure pro hello Data Lake Store verzi public preview, postupujte podle pokynů hello v [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).
    >

2. Účet Data Lake Store je přidruženo ke skupině prostředků Azure. Začněte vytvořením skupiny prostředků.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Měli byste vidět výstup takto:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Vytvoření účtu Data Lake Store. účet Hello název, který zadáte, musí obsahovat jenom malá písmena a číslice.

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

4. Použití Data Lake Store jako výchozí úložiště vyžaduje jste toospecify specifických pro cluster souborů kořenovou cestu toowhich hello zkopírované při vytváření clusteru. toocreate kořenovou cestu, která je **/clustery/hdiadlcluster** ve fragmentu kódu hello pomocí hello následující rutiny:

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a>Nastavení ověřování pro založené na rolích přístup tooData Lake Store
Každé předplatné služby Azure souvisí s entitou, Azure AD. Uživatelů a služeb, přístup k prostředkům předplatné pomocí hello portál Azure nebo hello rozhraní API služby Azure Resource Manager, musí nejprve ověřit pomocí služby Azure AD. Přístup uděluje přiřazením příslušné role hello na prostředek služby Azure tooAzure odběry a služby. Objekt služby pro služby, identifikuje hello služby ve službě Azure AD.

Tato část ukazuje, jak toogrant aplikace služby, jako je například HDInsight, tooan přístup prostředků Azure (hello účtu Data Lake Store, který jste vytvořili dříve). To uděláte tak, že vytvoření služby hlavní aplikace hello a přiřazování rolí tooit pomocí prostředí PowerShell.

tooset až ověřování služby Active Directory pro Azure Data Lake, provádějí úlohy hello v hello následující dvě části.

### <a name="create-a-self-signed-certificate"></a>Vytvořit certifikát podepsaný svým držitelem
Zajistěte, aby byla [Windows SDK](https://dev.windows.com/en-us/downloads) nainstalovala ještě před pokračováním hello kroky v této části. Musíte také vytvořit adresář, jako například *C:\mycertdir*, kde můžete vytvořit certifikát hello.

1. Z okna PowerShell text hello, přejděte toohello umístění, kam jste nainstalovali Windows SDK (obvykle *C:\Program Files (x86) \Windows Kits\10\bin\x86*) a použít hello [MakeCert] [ makecert] toocreate nástroj certifikát podepsaný svým držitelem a privátní klíč. Hello použijte následující příkazy:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Bude výzvami tooenter hello heslo soukromého klíče. Po hello příkaz je úspěšně provést, měli byste vidět **CertFile.cer** a **mykey.pvk** v adresáři hello certifikát, který jste zadali.
2. Použití hello [Pvk2Pfx] [ pvk2pfx] nástroj tooconvert hello pvk a .cer soubory tento soubor .pfx vytvořený tooa MakeCert. Spusťte následující příkaz hello:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Po zobrazení výzvy zadejte hello heslo soukromého klíče dříve zadaný. Hodnota zadaná pro hello Hello **-SP** parametr je hello heslo, které jsou přidružené soubor .pfx hello. Po příkazu hello byla úspěšně dokončena, měli byste taky vidět **CertFile.pfx** v adresáři hello certifikát, který jste zadali.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Vytvoření Azure AD a instanční objekt
V této části vytvořit objekt služby pro aplikaci Azure AD, přiřaďte objekt role toohello služby a ověření jako hello instanční objekt tím, že poskytuje certifikát. toocreate aplikace ve službě Azure AD, spusťte následující příkazy hello:

1. Vložte následující rutiny v okně konzoly prostředí PowerShell hello hello. Ujistěte se, že hello hodnota zadaná pro hello **– DisplayName** vlastnost je jedinečný. Hello hodnoty pro **– Domovská stránka** a **- IdentiferUris** jsou zástupné hodnoty a nejsou ověřené.

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
2. Vytvořit objekt služby s použitím ID hello aplikace.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Udělit hello kořenovém adresáři Data Lake Store toohello služby hlavní přístupu a všechny složky hello v hello kořenovou cestu, která jste zadali dříve. Použijte hello následující rutiny:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-hello-default-storage"></a>Vytvoření clusteru služby HDInsight Linux s Data Lake Store jako hello výchozí úložiště

V této části vytvoříte clusteru služby HDInsight Hadoop Linux s Data Lake Store jako hello výchozí úložiště. Pro tuto verzi hello HDInsight cluster a Data Lake Store musí být ve hello stejné umístění.

1. Načtení ID hello předplatné klienta a uloží jej později toouse.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Vytvoření clusteru HDInsight se hello pomocí hello následující rutiny:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Po úspěšném dokončení hello rutinu byste měli vidět výstup obsahující podrobnosti o clusteru hello.

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-data-lake-store"></a>Spustit testovací úlohy na toouse clusteru HDInsight hello Data Lake Store
Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na něm tooensure, že Data Lake Store můžete získat přístup. toodo tedy spustit toocreate úlohy Hive ukázkové tabulku, která používá hello ukázková data, která je již k dispozici v Data Lake Store v  *<cluster root>/example/data/sample.log*.

V této části vytvoříte připojení Secure Shell (SSH) do hello cluster HDInsight Linux, který jste vytvořili, a spusťte ukázkový dotaz Hive.

* Pokud používáte toomake klienta Windows připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Pokud používáte klienta toomake Linux připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Po provedení hello připojení, spusťte hello Hive rozhraní příkazového řádku (CLI) pomocí hello následující příkaz:

        hive
2. Použití hello rozhraní příkazového řádku tooenter hello následující příkazy toocreate novou tabulku s názvem **vozidel** pomocí hello ukázkových dat v Data Lake Store:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    V konzole hello SSH, byste měli vidět výstup hello dotazu.

    >[!NOTE]
    >Hello cesta toohello ukázková data v předchozím příkazu CREATE TABLE hello jsou `adl:///example/data/`, kde `adl:///` je kořenový cluster hello. Následující příklad hello kořenové hello clusteru, který je uveden v tomto kurzu, příkaz hello je `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Můžete buď použít kratší alternativní hello nebo zadejte toohello hello úplnou cestu ke kořenovému adresáři clusteru.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>Přístup k Data Lake Store pomocí příkazů HDFS
Po nakonfigurování hello HDInsight clusteru toouse Data Lake Store, můžete použít Hadoop Distributed File System (HDFS) prostředí příkazy tooaccess hello úložiště.

V této části provedete připojení SSH do hello cluster HDInsight Linux, který jste vytvořili, a spusťte příkazů HDFS hello.

* Pokud používáte toomake klienta Windows připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Pokud používáte klienta toomake Linux připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Po provedení hello připojení, seznam souborů hello v Data Lake Store pomocí následujících příkazů systému souborů HDFS hello.

    hdfs dfs -ls adl:///

Můžete taky hello `hdfs dfs -put` příkaz tooupload některé soubory tooData Lake Store a pak použijte `hdfs dfs -ls` tooverify jestli hello soubory úspěšně se nahrál.

## <a name="see-also"></a>Viz také
* [Portál Azure: vytvoření toouse clusteru HDInsight Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
