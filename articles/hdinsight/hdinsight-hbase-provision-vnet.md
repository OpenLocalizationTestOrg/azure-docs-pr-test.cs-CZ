---
title: "aaaCreate HBase clusterů ve virtuální síti - Azure | Microsoft Docs"
description: "Začínáme používat HBase v Azure HDInsight. Zjistěte, jak toocreate HDInsight HBase clusterů ve virtuální síti Azure."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 097338a5a650bb607a9f6f9ddb59bb88d098b56f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Vytvořit clustery HBase v HDInsight v Azure Virtual Network
Zjistěte, jak toocreate Azure HDInsight HBase clusterů v [Azure Virtual Network][1].

Clustery HBase integrace virtuální sítě, může být nasazený toohello stejné virtuální sítě jako aplikace tak, že aplikace může komunikovat přímo s HBase. Hello výhody patří:

* Přímé připojení hello webové aplikace toohello uzlů tohoto clusteru hello HBase, který umožňuje komunikaci prostřednictvím vzdálené procedury HBase Java volání rozhraní API (RPC).
* Zvýšení výkonu tak, že není provozu přejděte přes více bran a nástroje pro vyrovnávání zatížení.
* Hello možnost tooprocess citlivé informace bezpečnější způsobem bez vystavení veřejný koncový bod.

### <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít hello následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Pracovní stanice s prostředím Azure PowerShell**. V tématu [instalace a použití prostředí Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-hbase-cluster-into-virtual-network"></a>Vytvoření clusteru HBase do virtuální sítě
V této části vytvoříte clusteru s podporou systémem Linux HBase s hello závislý účet Azure Storage virtuální síti Azure pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Další metody vytváření clusterů a principy hello nastavení, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Další informace o použití šablony toocreate Hadoop clusterů v HDInsight, najdete v části [vytvoření Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [!NOTE]
> Některé vlastnosti jsou pevně zakódovaná do šablony hello. Například:
>
> * **Umístění**: východní USA 2
> * **Verze clusteru:** 3.5
> * **Počet uzlů pracovního procesu clusteru**: 2
> * **Výchozí účet úložiště**: jedinečné řetězce
> * **Název virtuální sítě**: &lt;název clusteru >-virtuální síť
> * **Adresní prostor virtuální sítě**: 10.0.0.0/16
> * **Název podsítě**: subnet1
> * **Rozsah adres podsítě**: 10.0.0.0/24
>
> &lt;Název clusteru > se nahradí název clusteru hello poskytují při použití šablony hello.
>
>

1. Klikněte na tlačítko hello následující šablony hello tooopen bitové kopie v hello portálu Azure. Šablona Hello se nachází v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Z hello **vlastní nasazení** okno, zadejte hello následující vlastnosti:

   * **Předplatné**: Vyberte předplatné Azure použité clusteru HDInsight hello toocreate, hello závislý účet úložiště a hello virtuální síť Azure.
   * **Skupina prostředků**: vyberte **vytvořit nový**a zadejte nový název skupiny prostředků.
   * **Umístění**: Vyberte umístění pro skupinu prostředků hello.
   * **Název clusteru**: Zadejte název toobe clusteru Hadoop hello vytvořili.
   * **Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je **správce**.
   * **SSH uživatelské jméno a heslo**: výchozí uživatelské jméno hello **sshuser**.  Můžete ho změnit.
   * **Souhlasím toohello podmínky a ujednání hello výše uvedených**: (vyberte)
3. Klikněte na **Koupit**. Trvá přibližně 20 minut toocreate cluster. Po vytvoření clusteru hello můžete kliknout na okno hello clusteru v portálu tooopen hello ho.

Po dokončení kurzu hello můžete chtít toodelete hello clusteru. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána. Pokyny hello odstranění clusteru najdete v tématu [hello spravovat Hadoop clusterů v HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

toobegin práce s nového clusteru HBase, můžete použít postupy hello najít v [Začínáme používat HBase s Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-toohello-hbase-cluster-using-hbase-java-rpc-apis"></a>Připojte toohello cluster HBase pomocí rozhraní API RPC Java HBase
1. Vytváření infrastruktury jako služby (IaaS) virtuální počítač do hello stejnou virtuální síť Azure a hello stejné podsíti. Pokyny pro vytvoření nového virtuálního počítače IaaS, najdete v části [vytvořit virtuální počítač spuštěný Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Pokud následující hello kroky v tomto dokumentu, je nutné použít následující hodnoty pro konfiguraci sítě hello hello:

   * **Virtuální síť**: &lt;název clusteru >-virtuální síť
   * **Podsíť**: subnet1

   > [!IMPORTANT]
   > Nahraďte &lt;název clusteru > s názvem hello jste použili při vytváření clusteru HDInsight hello v předchozích krocích.
   >
   >

   Pomocí těchto hodnot, hello je umístěn virtuální počítač v hello stejné virtuální síť a podsíť jako hello clusteru HDInsight. Tato konfigurace umožňuje jejich toodirectly vzájemně komunikovat. Neexistuje způsob, jak toocreate clusteru HDInsight bez prázdný hraniční uzel. Hello hraniční uzel může být použité toomanage hello clusteru.  Další informace najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).

2. Při vzdáleném použití tooHBase tooconnect aplikace Java, musíte použít hello plně kvalifikovaný název domény (FQDN). toodetermine, musíte získat hello příponu DNS specifickou pro připojení clusteru HBase hello. toodo, můžete použít jednu z následujících metod hello:

   * Použití webové prohlížeče toomake volání rozhraní Ambari:

     Procházet toohttps: / /&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hostitelem? minimal_response = true. Se změní soubor JSON s hello přípony DNS.
   * Použití webu Ambari hello:

     1. Procházet příliš https://&lt;ClusterName >. azurehdinsight.net.
     2. Klikněte na tlačítko **hostitele** hello hlavní nabídce.
   * Použijte volání REST toomake Curl:

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     V hello JavaScript Object Notation (JSON) data vrácená vyhledejte položku "název_hostitele" hello. Obsahuje hello plně kvalifikovaný název domény pro hello uzly v clusteru hello. Například:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     část Hello hello domény název začíná název clusteru hello je hello příponu DNS. Například mycluster.b1.cloudapp.net.
   * Použití Azure Powershell

     Použití hello následující hello tooregister skriptu prostředí Azure PowerShell **Get-ClusterDetail** funkce, které lze použít tooreturn příponu DNS hello:

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information toofacilitate an HDInsight cluster-to-cluster scenario within hello same virtual network.
            .Description
            This command shows hello following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run hello HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows hello FQDN suffix of hosts in hello cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows hello value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run hello HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows hello FQDN suffix of hosts in hello cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     Po spouštění skriptu prostředí Azure PowerShell text hello, použijte hello následující příkaz příponu DNS hello tooreturn pomocí hello **Get-ClusterDetail** funkce. Při použití tohoto příkazu, zadejte název clusteru HDInsight HBase, správce jméno a heslo správce.

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     Tento příkaz vrátí hello příponu DNS. Například **yourclustername.b4.internal.cloudapp.net**.


<!--
3.    Change hello primary DNS suffix configuration of hello virtual machine. This enables hello virtual machine tooautomatically resolve hello host name of hello HBase cluster without explicit specification of hello suffix. For example, hello *workernode0* host name will be correctly resolved tooworkernode0 of hello HBase cluster.

    toomake hello configuration change:

    1. RDP into hello virtual machine.
    2. Open **Local Group Policy Editor**. hello executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** toohello value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot hello virtual machine.
-->

tooverify, který hello virtuální počítač může komunikovat s hello HBase cluster, použijte příkaz hello `ping headnode0.<dns suffix>` z hello virtuálního počítače. Například headnode0.mycluster.b1.cloudapp.net příkazu ping.

toouse tyto informace v aplikaci Java, můžete provést kroky hello v [aplikací Java toobuild použít Maven, které používají HBase s HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate aplikace. aplikace hello toohave připojení vzdáleného serveru HBase tooa, změňte hello **hbase-site.xml** soubor v této hello toouse příklad plně kvalifikovaný název domény pro Zookeeper. Například:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> Další informace o překladu názvů v Azure virtuální sítě včetně jak toouse vlastního serveru DNS, najdete v části [rozlišení DNS (Name)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
>
>

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak toocreate HBase cluster. toolearn více, najdete v části:

* [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md)
* [Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Začínáme používat HBase s Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started.md)
* [Analýza sentimentu Twitter s HBase v HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
* [Přehled virtuálních sítí][vnet-overview]

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Podrobnosti o zřizování pro nový cluster HBase hello"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Pomocí akce skriptu toocustomize HBase cluster"

[azure-preview-portal]: https://portal.azure.com
