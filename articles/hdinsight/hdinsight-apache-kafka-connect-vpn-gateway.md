---
title: "aaaConnect tooKafka pomocí virtuální sítě - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toodirectly připojení tooKafka v HDInsight přes virtuální síť Azure. Zjistěte, jak tooconnect tooKafka od klientů vývoj pomocí brány sítě VPN nebo z klientů ve vaší místní sítě pomocí zařízení brány VPN."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.custom: hdinsightactive
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/01/2017
ms.author: larryfr
ms.openlocfilehash: 03542fe14b9a1d010dffa22a8f8d96b098a1576e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tookafka-on-hdinsight-preview-through-an-azure-virtual-network"></a>Připojení přes virtuální síť Azure tooKafka v HDInsight (preview)

Zjistěte, jak připojit toodirectly tooKafka v HDInsight pomocí virtuálních sítí Azure. Tento dokument obsahuje informace o připojení tooKafka pomocí hello následující konfigurace:

* Z prostředků v místní síti. Tato připojení pomocí zařízení VPN (softwaru nebo hardwaru) ve vaší místní síti.
* Z prostředí pro vývoj pomocí softwaru klienta VPN.

## <a name="architecture-and-planning"></a>Plánování a architektura

HDInsight neumožňuje tooKafka přímé připojení přes hello veřejného Internetu. Místo toho Kafka klientů (producenti a spotřebitelé) musí používat jednu z hello následující metody připojení:

* Spusťte klienta hello v hello stejné virtuální síti jako Kafka v HDInsight. Tato konfigurace se používá v hello [začínat Apache Kafka (preview) na HDInsight](hdinsight-apache-kafka-get-started.md) dokumentu. Hello klienta spustí přímo na hello HDInsight uzly clusteru nebo na jiný virtuální počítač v hello stejné síti.

* Připojte privátní síti, například v místní síti toohello virtuální sítě. Tato konfigurace umožňuje klientům ve vaší místní síti toodirectly práce se Kafka. tooenable tuto konfiguraci provést hello následující úlohy:

    1. Vytvoření virtuální sítě.
    2. Vytvořte bránu VPN, který používá konfiguraci site-to-site. Konfigurace Hello použitá v tomto dokumentu se připojí tooa zařízení brány sítě VPN v síti na pracovišti.
    3. Vytvoření serveru DNS ve virtuální síti hello.
    4. Konfigurace předávání mezi hello server DNS v každé sítě.
    5. Nainstalujte Kafka v HDInsight do virtuální sítě hello.

    Další informace najdete v tématu hello [připojit tooKafka z místní sítě](#on-premises) části. 

* Připojte virtuální sítě pro jednotlivé počítače toohello pomocí klienta VPN a brány VPN. tooenable tuto konfiguraci provést hello následující úlohy:

    1. Vytvoření virtuální sítě.
    2. Vytvořte bránu VPN, který používá konfiguraci point-to-site. Tato konfigurace poskytuje klienta VPN, který se dá nainstalovat na klienty se systémem Windows.
    3. Nainstalujte Kafka v HDInsight do virtuální sítě hello.
    4. Nakonfigurujte Kafka pro inzerování IP. Tato konfigurace umožňuje tooconnect hello klienta pomocí IP adres namísto názvů domény.
    5. Stáhnout a použít klienta VPN hello vývoj systému hello.

    Další informace najdete v tématu hello [připojit tooKafka u klientů VPN](#vpnclient) části.

    > [!WARNING]
    > Tato konfigurace se doporučuje jenom pro účely vývoje kvůli hello následující omezení:
    >
    > * Každý klient musí připojit pomocí softwaru klienta VPN. Azure poskytuje jenom klienta se systémem Windows.
    > * Hello klienta nepředává název řešení požadavky toohello virtuální sítě, takže je nutné použít IP adresách toocommunicate s Kafka. Komunikaci IP vyžaduje další konfiguraci na hello Kafka clusteru.

Další informace o používání HDInsight ve virtuální síti, najdete v části [rozšířit HDInsight pomocí virtuálních sítí Azure](./hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a>Připojit tooKafka z místní sítě

toocreate Kafka clusteru, který komunikuje s vaší místní síti, postupujte podle kroků hello v hello [připojit HDInsight tooyour do místní sítě](./connect-on-premises-network.md) dokumentu.

> [!IMPORTANT]
> Při vytváření clusteru HDInsight hello, vyberte hello __Kafka__ clusteru typu.

Pomocí těchto kroků vytvoříte hello následující konfigurace:

* Azure Virtual Network
* Brána Site-to-site VPN
* Účet služby Azure Storage (používá se v prostředí HDInsight)
* Kafka v HDInsight

zda Kafka klienta můžete připojit toohello clusteru z místní, použijte kroky hello v hello tooverify [příklad: klienta Python](#python-client) části.

## <a id="vpnclient"></a>Připojit tooKafka u klientů VPN

Použijte hello kroky v této části toocreate hello následující konfigurace:

* Azure Virtual Network
* Brána sítě VPN Point-to-site
* Účet služby Azure Storage (používá se v prostředí HDInsight)
* Kafka v HDInsight

1. Postupujte podle kroků hello v hello [práce s certifikáty podepsané svým držitelem pro připojení Point-to-site](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentu. Tento dokument vytvoří hello certifikáty potřebné pro bránu hello.

2. Otevřete příkazovém řádku prostředí PowerShell a použít následující kód toolog v tooyour předplatného Azure hello:

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment tooset hello subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. Použijte následující proměnné toocreate kódu, které obsahují informace o konfiguraci hello:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is hello resource group name?"
    $baseName = Read-Host "What is hello base name? It is used toocreate names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want toocreate hello resources in?"
    $rootCert = Read-Host "What is hello file path toohello root certificate? It is used toosecure hello VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter hello HTTPS user name and password for hello HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter hello SSH user name and password for hello HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.5"
    $hdiType = "Kafka"
    ```

4. Použití hello následující kód Skupina prostředků Azure hello toocreate a virtuální sítě:

    ```powershell
    # Create hello resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create hello subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get hello network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for hello gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get hello certificate info
    # Get hello full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create hello VPN gateway
    New-AzureRmVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]
    > Ho může trvat několik minut, než toocomplete tento proces.

5. Použijte následující kód toocreate hello účet úložiště Azure a objektů blob kontejneru hello:

    ```powershell
    # Create hello storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get hello storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create hello default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. Použijte následující clusteru HDInsight hello toocreate kód hello:

    ```powershell
    # Create hello HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

  > [!WARNING]
  > Tento proces trvá přibližně 20 minut toocomplete.

8. Použijte následující rutinu tooretrieve hello adresu URL pro klienta VPN ve Windows hello pro virtuální síť hello hello:

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    klienta VPN ve Windows hello toodownload, použijte hello vrátil URI ve webovém prohlížeči.

### <a name="configure-kafka-for-ip-advertising"></a>Konfigurace Kafka pro inzerování IP

Ve výchozím nastavení Zookeeper vrátí název domény hello hello Kafka zprostředkovatelé tooclients. Tato konfigurace nefunguje s hello softwaru klienta VPN, překlad nemůže použít pro entity ve virtuální síti hello. Pro tuto konfiguraci, použijte následující hello kroky tooconfigure Kafka tooadvertise IP adres místo názvy domén:

1. Pomocí webového prohlížeče, přejděte toohttps://CLUSTERNAME.azurehdinsight.net. Nahraďte __CLUSTERNAME__ s názvem hello hello Kafka na clusteru HDInsight.

    Pokud budete vyzváni, použijte hello HTTPS uživatelské jméno a heslo pro hello cluster. Zobrazí se Hello webové uživatelské rozhraní Ambari pro hello cluster.

2. Vyberte tooview informace o Kafka, __Kafka__ hello seznamu na levé straně hello.

    ![Seznam služeb s Kafka zvýrazněná](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Vyberte tooview konfigurace Kafka, __konfigurací__ z horní střední hello.

    ![Konfigurací odkazy pro Kafka](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. toofind hello __kafka env__ konfigurace, zadejte `kafka-env` v hello __filtru__ pole v horní pravé hello.

    ![Konfigurace Kafka, pro kafka env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. tooconfigure Kafka tooadvertise IP adres, přidejte následující text toohello dolní části hello hello __kafka. env šablony__ pole:

    ```
    # Configure Kafka tooadvertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. tooconfigure hello rozhraní, které Kafka naslouchá na, zadejte `listeners` v hello __filtru__ pole v horní pravé hello.

7. tooconfigure Kafka toolisten na všech síťových rozhraní, hodnota hello změnit v hello __naslouchací procesy__ pole příliš`PLAINTEXT://0.0.0.0:9092`.

8. změny konfigurace hello toosave, použijte hello __Uložit__ tlačítko. Zadejte textovou zprávu s popisem hello změny. Vyberte __OK__ po uložení změn hello.

    ![Konfigurace tlačítko Uložit](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. tooprevent chyby při restartování Kafka, použijte hello __služby akce__ tlačítko a vyberte __zapnout v režimu údržby__. Vyberte OK toocomplete tuto operaci.

    ![Akce služby s zapněte zvýrazněná údržby](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. toorestart Kafka, použijte hello __restartujte__ tlačítko a vyberte __restartujte všechny vliv__. Potvrďte hello restartování a pak použít hello __OK__ tlačítko po dokončení operace hello.

    ![Restartujte tlačítkem s restartem vliv](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. toodisable režimu údržby, použijte hello __služby akce__ tlačítko a vyberte __zapnout vypnout režimu údržby__. Vyberte **OK** toocomplete tuto operaci.

### <a name="connect-toohello-vpn-gateway"></a>Připojení brány VPN toohello

Brána sítě VPN toohello tooconnect z __klienta Windows__, použijte hello __připojit tooAzure__ části hello [konfigurace připojení typu Point-to-Site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentu.

## <a id="python-client"></a>Příklad: Python klienta

toovalidate tooKafka připojení, použijte následující postup toocreate hello a spusťte producent Python a příjemce:

1. Použití jednoho hello následující metody tooretrieve hello plně kvalifikovaný název domény (FQDN) a IP adresy hello uzlů v clusteru Kafka hello:

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Tento skript předpokládá, že `$resourceGroupName` je hello název skupiny prostředků Azure hello, která obsahuje virtuální síť hello.

    Uložte hello vrátí informace o pro použití v hello další kroky.

2. Použití hello následující tooinstall hello [kafka python](http://kafka-python.readthedocs.io/) klienta:

        pip install kafka-python

3. toosend data tooKafka hello použijte následující kód Python:

  ```python
  from kafka import KafkaProducer
  # Replace hello `ip_address` entries with hello IP address of your worker nodes
  # NOTE: you don't need hello full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    Nahraďte hello `'kafka_broker'` položky s adresami hello vrácená z kroku 1 v této části:

    * Pokud používáte __klienta VPN softwaru__, nahraďte hello `kafka_broker` položky s IP adresou hello uzly pracovního procesu.

    * Pokud máte __povolen překlad názvů pomocí vlastního serveru DNS__, nahraďte hello `kafka_broker` položky s hello plně kvalifikovaný název domény hello uzlů pracovního procesu.

    > [!NOTE]
    > Tento kód odešle řetězec hello `test message` toohello tématu `testtopic`. Výchozí konfigurace Hello Kafka v HDInsight je toocreate hello tématu, pokud neexistuje.

4. tooretrieve hello zprávy z Kafka, použijte následující kód Python hello:

   ```python
   from kafka import KafkaConsumer
   # Replace hello `ip_address` entries with hello IP address of your worker nodes
   # Again, you only need one or two, not hello full list.
   # Note: auto_offset_reset='earliest' resets hello starting offset toohello beginning
   #       of hello topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    Nahraďte hello `'kafka_broker'` položky s adresami hello vrácená z kroku 1 v této části:

    * Pokud používáte __klienta VPN softwaru__, nahraďte hello `kafka_broker` položky s IP adresou hello uzly pracovního procesu.

    * Pokud máte __povolen překlad názvů pomocí vlastního serveru DNS__, nahraďte hello `kafka_broker` položky s hello plně kvalifikovaný název domény hello uzlů pracovního procesu.

## <a name="next-steps"></a>Další kroky

Další informace o používání HDInsight s virtuální sítí, najdete v části hello [rozšíření Azure HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.

Další informace o vytváření virtuální síť Azure s bránou sítě VPN Point-to-Site najdete v tématu hello následující dokumenty:

* [Konfigurace připojení typu Point-to-Site pomocí hello portálu Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Konfigurace připojení typu Point-to-Site pomocí Azure PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Další informace o práci s Kafka v HDInsight naleznete v tématu hello následující dokumenty:

* [Začínáme s Kafka ve službě HDInsight](hdinsight-apache-kafka-get-started.md)
* [Použít zrcadlení s Kafka v HDInsight](hdinsight-apache-kafka-mirroring.md)
