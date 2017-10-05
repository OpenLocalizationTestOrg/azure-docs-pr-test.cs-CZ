---
title: "Připojení k Kafka pomocí virtuální sítě - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak k přímému připojení k Kafka v HDInsight prostřednictvím virtuální síti Azure. Zjistěte, jak se připojit k Kafka od klientů vývoj pomocí brány sítě VPN nebo z klientů ve vaší místní síti pomocí zařízení brány VPN."
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
ms.openlocfilehash: 245bee7c1dbb0236afdc2506e7ab84b5573cbc85
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-kafka-on-hdinsight-preview-through-an-azure-virtual-network"></a><span data-ttu-id="265b5-104">Připojení k Kafka v HDInsight (preview) přes virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="265b5-104">Connect to Kafka on HDInsight (preview) through an Azure Virtual Network</span></span>

<span data-ttu-id="265b5-105">Zjistěte, jak k přímému připojení k Kafka v HDInsight pomocí virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="265b5-105">Learn how to directly connect to Kafka on HDInsight using Azure Virtual Networks.</span></span> <span data-ttu-id="265b5-106">Tento dokument obsahuje informace o připojení k Kafka pomocí následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="265b5-106">This document provides information on connecting to Kafka using the following configurations:</span></span>

* <span data-ttu-id="265b5-107">Z prostředků v místní síti.</span><span class="sxs-lookup"><span data-stu-id="265b5-107">From resources in an on-premises network.</span></span> <span data-ttu-id="265b5-108">Tato připojení pomocí zařízení VPN (softwaru nebo hardwaru) ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="265b5-108">This connection is established by using a VPN device (software or hardware) on your local network.</span></span>
* <span data-ttu-id="265b5-109">Z prostředí pro vývoj pomocí softwaru klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="265b5-109">From a development environment using a VPN software client.</span></span>

## <a name="architecture-and-planning"></a><span data-ttu-id="265b5-110">Plánování a architektura</span><span class="sxs-lookup"><span data-stu-id="265b5-110">Architecture and planning</span></span>

<span data-ttu-id="265b5-111">HDInsight neumožňuje přímé připojení k Kafka prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="265b5-111">HDInsight does not allow direct connection to Kafka over the public internet.</span></span> <span data-ttu-id="265b5-112">Místo toho Kafka klientů (producenti a spotřebitelé) musí používat jednu z následujících metod připojení:</span><span class="sxs-lookup"><span data-stu-id="265b5-112">Instead, Kafka clients (producers and consumers) must use one of the following connection methods:</span></span>

* <span data-ttu-id="265b5-113">Spustíte klienta ve stejné virtuální síti jako Kafka v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="265b5-113">Run the client in the same virtual network as Kafka on HDInsight.</span></span> <span data-ttu-id="265b5-114">Tato konfigurace se používá v [začínat Apache Kafka (preview) na HDInsight](hdinsight-apache-kafka-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="265b5-114">This configuration is used in the [Start with Apache Kafka (preview) on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span> <span data-ttu-id="265b5-115">Klient spouští přímo na uzlech clusteru HDInsight nebo na jiný virtuální počítač ve stejné síti.</span><span class="sxs-lookup"><span data-stu-id="265b5-115">The client runs directly on the HDInsight cluster nodes or on another virtual machine in the same network.</span></span>

* <span data-ttu-id="265b5-116">Privátní síti, například v místní síti připojte k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="265b5-116">Connect a private network, such as your on-premises network, to the virtual network.</span></span> <span data-ttu-id="265b5-117">Tato konfigurace umožňuje klientům ve vaší místní síti pracovat přímo s Kafka.</span><span class="sxs-lookup"><span data-stu-id="265b5-117">This configuration allows clients in your on-premises network to directly work with Kafka.</span></span> <span data-ttu-id="265b5-118">Chcete-li tuto konfiguraci, proveďte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="265b5-118">To enable this configuration, perform the following tasks:</span></span>

    1. <span data-ttu-id="265b5-119">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="265b5-119">Create a virtual network.</span></span>
    2. <span data-ttu-id="265b5-120">Vytvořte bránu VPN, který používá konfiguraci site-to-site.</span><span class="sxs-lookup"><span data-stu-id="265b5-120">Create a VPN gateway that uses a site-to-site configuration.</span></span> <span data-ttu-id="265b5-121">Konfigurace použitá v tomto dokumentu se připojí k zařízení brány sítě VPN v síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="265b5-121">The configuration used in this document connects to a VPN gateway device in your on-premises network.</span></span>
    3. <span data-ttu-id="265b5-122">Vytvoření serveru DNS ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="265b5-122">Create a DNS server in the virtual network.</span></span>
    4. <span data-ttu-id="265b5-123">Konfigurace předávání mezi serverem DNS v každé sítě.</span><span class="sxs-lookup"><span data-stu-id="265b5-123">Configure forwarding between the DNS server in each network.</span></span>
    5. <span data-ttu-id="265b5-124">Nainstalujte Kafka v HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="265b5-124">Install Kafka on HDInsight into the virtual network.</span></span>

    <span data-ttu-id="265b5-125">Další informace najdete v tématu [připojení k Kafka z místní sítě](#on-premises) části.</span><span class="sxs-lookup"><span data-stu-id="265b5-125">For more information, see the [Connect to Kafka from an on-premises network](#on-premises) section.</span></span> 

* <span data-ttu-id="265b5-126">Jednotlivé počítače připojte k virtuální síti pomocí klienta VPN a brány VPN.</span><span class="sxs-lookup"><span data-stu-id="265b5-126">Connect individual machines to the virtual network using a VPN gateway and VPN client.</span></span> <span data-ttu-id="265b5-127">Chcete-li tuto konfiguraci, proveďte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="265b5-127">To enable this configuration, perform the following tasks:</span></span>

    1. <span data-ttu-id="265b5-128">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="265b5-128">Create a virtual network.</span></span>
    2. <span data-ttu-id="265b5-129">Vytvořte bránu VPN, který používá konfiguraci point-to-site.</span><span class="sxs-lookup"><span data-stu-id="265b5-129">Create a VPN gateway that uses a point-to-site configuration.</span></span> <span data-ttu-id="265b5-130">Tato konfigurace poskytuje klienta VPN, který se dá nainstalovat na klienty se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="265b5-130">This configuration provides a VPN client that can be installed on Windows clients.</span></span>
    3. <span data-ttu-id="265b5-131">Nainstalujte Kafka v HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="265b5-131">Install Kafka on HDInsight into the virtual network.</span></span>
    4. <span data-ttu-id="265b5-132">Nakonfigurujte Kafka pro inzerování IP.</span><span class="sxs-lookup"><span data-stu-id="265b5-132">Configure Kafka for IP advertising.</span></span> <span data-ttu-id="265b5-133">Tato konfigurace umožňuje klientům připojení pomocí IP adresy namísto názvů domény.</span><span class="sxs-lookup"><span data-stu-id="265b5-133">This configuration allows the client to connect using IP addressing instead of domain names.</span></span>
    5. <span data-ttu-id="265b5-134">Stáhnout a použít klienta VPN na vývojovém systému.</span><span class="sxs-lookup"><span data-stu-id="265b5-134">Download and use the VPN client on the development system.</span></span>

    <span data-ttu-id="265b5-135">Další informace najdete v tématu [připojit k Kafka u klientů VPN](#vpnclient) části.</span><span class="sxs-lookup"><span data-stu-id="265b5-135">For more information, see the [Connect to Kafka with a VPN client](#vpnclient) section.</span></span>

    > [!WARNING]
    > <span data-ttu-id="265b5-136">Tato konfigurace se doporučuje jenom pro účely vývoje z důvodu následující omezení:</span><span class="sxs-lookup"><span data-stu-id="265b5-136">This configuration is only recommended for development purposes because of the following limitations:</span></span>
    >
    > * <span data-ttu-id="265b5-137">Každý klient musí připojit pomocí softwaru klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="265b5-137">Each client must connect using a VPN software client.</span></span> <span data-ttu-id="265b5-138">Azure poskytuje jenom klienta se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="265b5-138">Azure only provides a Windows-based client.</span></span>
    > * <span data-ttu-id="265b5-139">Klient nesplňuje požadavky na rozlišení názvů k virtuální síti, je nutné použít ke komunikaci s Kafka adresování IP adres.</span><span class="sxs-lookup"><span data-stu-id="265b5-139">The client does not pass name resolution requests to the virtual network, so you must use IP addressing to communicate with Kafka.</span></span> <span data-ttu-id="265b5-140">Komunikaci IP vyžaduje další konfiguraci v clusteru Kafka.</span><span class="sxs-lookup"><span data-stu-id="265b5-140">IP communication requires additional configuration on the Kafka cluster.</span></span>

<span data-ttu-id="265b5-141">Další informace o používání HDInsight ve virtuální síti, najdete v části [rozšířit HDInsight pomocí virtuálních sítí Azure](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="265b5-141">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

## <span data-ttu-id="265b5-142"><a id="on-premises"></a>Připojit k Kafka z místní sítě</span><span class="sxs-lookup"><span data-stu-id="265b5-142"><a id="on-premises"></a> Connect to Kafka from an on-premises network</span></span>

<span data-ttu-id="265b5-143">Pokud chcete vytvořit cluster Kafka, který komunikuje s vaší místní síti, postupujte podle kroků v [HDInsight připojit k místní síti](./connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="265b5-143">To create a Kafka cluster that communicates with your on-premises network, follow the steps in the [Connect HDInsight to your on-premises network](./connect-on-premises-network.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="265b5-144">Při vytváření clusteru HDInsight, vyberte __Kafka__ clusteru typu.</span><span class="sxs-lookup"><span data-stu-id="265b5-144">When creating the HDInsight cluster, select the __Kafka__ cluster type.</span></span>

<span data-ttu-id="265b5-145">Pomocí těchto kroků vytvoříte následující konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="265b5-145">These steps create the following configuration:</span></span>

* <span data-ttu-id="265b5-146">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="265b5-146">Azure Virtual Network</span></span>
* <span data-ttu-id="265b5-147">Brána Site-to-site VPN</span><span class="sxs-lookup"><span data-stu-id="265b5-147">Site-to-site VPN gateway</span></span>
* <span data-ttu-id="265b5-148">Účet služby Azure Storage (používá se v prostředí HDInsight)</span><span class="sxs-lookup"><span data-stu-id="265b5-148">Azure Storage account (used by HDInsight)</span></span>
* <span data-ttu-id="265b5-149">Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="265b5-149">Kafka on HDInsight</span></span>

<span data-ttu-id="265b5-150">Pokud chcete ověřit, že klient Kafka se může připojit ke clusteru z místního, použijte kroky v [příklad: klienta Python](#python-client) části.</span><span class="sxs-lookup"><span data-stu-id="265b5-150">To verify that a Kafka client can connect to the cluster from on-premises, use the steps in the [Example: Python client](#python-client) section.</span></span>

## <span data-ttu-id="265b5-151"><a id="vpnclient"></a>Připojení k Kafka pomocí klienta VPN</span><span class="sxs-lookup"><span data-stu-id="265b5-151"><a id="vpnclient"></a> Connect to Kafka with a VPN client</span></span>

<span data-ttu-id="265b5-152">Postupujte podle kroků v této části vytvořit následující konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="265b5-152">Use the steps in this section to create the following configuration:</span></span>

* <span data-ttu-id="265b5-153">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="265b5-153">Azure Virtual Network</span></span>
* <span data-ttu-id="265b5-154">Brána sítě VPN Point-to-site</span><span class="sxs-lookup"><span data-stu-id="265b5-154">Point-to-site VPN gateway</span></span>
* <span data-ttu-id="265b5-155">Účet služby Azure Storage (používá se v prostředí HDInsight)</span><span class="sxs-lookup"><span data-stu-id="265b5-155">Azure Storage Account (used by HDInsight)</span></span>
* <span data-ttu-id="265b5-156">Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="265b5-156">Kafka on HDInsight</span></span>

1. <span data-ttu-id="265b5-157">Postupujte podle kroků v [práce s certifikáty podepsané svým držitelem pro připojení Point-to-site](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="265b5-157">Follow the steps in the [Working with self-signed certificates for Point-to-site connections](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document.</span></span> <span data-ttu-id="265b5-158">Tento dokument vytvoří certifikáty potřebné pro bránu.</span><span class="sxs-lookup"><span data-stu-id="265b5-158">This document creates the certificates needed for the gateway.</span></span>

2. <span data-ttu-id="265b5-159">Otevřete příkazovém řádku prostředí PowerShell a použít následující kód k přihlášení k předplatnému Azure:</span><span class="sxs-lookup"><span data-stu-id="265b5-159">Open a PowerShell prompt and use the following code to log in to your Azure subscription:</span></span>

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. <span data-ttu-id="265b5-160">Použijte následující kód k vytvoření proměnné, které obsahují informace o konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="265b5-160">Use the following code to create variables that contain configuration information:</span></span>

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is the resource group name?"
    $baseName = Read-Host "What is the base name? It is used to create names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want to create the resources in?"
    $rootCert = Read-Host "What is the file path to the root certificate? It is used to secure the VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter the HTTPS user name and password for the HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter the SSH user name and password for the HDInsight cluster" -UserName "sshuser"

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

4. <span data-ttu-id="265b5-161">Použijte následující kód k vytvoření skupiny prostředků Azure a virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="265b5-161">Use the following code to create the Azure resource group and virtual network:</span></span>

    ```powershell
    # Create the resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
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
    > <span data-ttu-id="265b5-162">To může trvat několik minut na dokončení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="265b5-162">It can take several minutes for this process to complete.</span></span>

5. <span data-ttu-id="265b5-163">Použijte následující kód k vytvoření účtu úložiště Azure a objektů blob kontejneru:</span><span class="sxs-lookup"><span data-stu-id="265b5-163">Use the following code to create the Azure Storage Account and blob container:</span></span>

    ```powershell
    # Create the storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. <span data-ttu-id="265b5-164">Použijte následující kód k vytvoření clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="265b5-164">Use the following code to create the HDInsight cluster:</span></span>

    ```powershell
    # Create the HDInsight cluster
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
  > <span data-ttu-id="265b5-165">Tento proces trvá přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="265b5-165">This process takes around 20 minutes to complete.</span></span>

8. <span data-ttu-id="265b5-166">K načtení adresu URL pro klienta VPN ve Windows pro virtuální síť, použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="265b5-166">Use the following cmdlet to retrieve the URL for the Windows VPN client for the virtual network:</span></span>

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    <span data-ttu-id="265b5-167">Pokud chcete stáhnout klienta VPN ve Windows, použijte identifikátor URI vrácené ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="265b5-167">To download the Windows VPN client, use the returned URI in your web browser.</span></span>

### <a name="configure-kafka-for-ip-advertising"></a><span data-ttu-id="265b5-168">Konfigurace Kafka pro inzerování IP</span><span class="sxs-lookup"><span data-stu-id="265b5-168">Configure Kafka for IP advertising</span></span>

<span data-ttu-id="265b5-169">Ve výchozím nastavení Zookeeper vrátí název domény zprostředkovatelé Kafka klientům.</span><span class="sxs-lookup"><span data-stu-id="265b5-169">By default, Zookeeper returns the domain name of the Kafka brokers to clients.</span></span> <span data-ttu-id="265b5-170">Tato konfigurace nefunguje s softwaru klienta VPN, překlad nemůže použít pro entity ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="265b5-170">This configuration does not work with the VPN software client, as it cannot use name resolution for entities in the virtual network.</span></span> <span data-ttu-id="265b5-171">Pro tuto konfiguraci použijte ke konfiguraci Kafka inzerovat IP adresy namísto názvů domény následující kroky:</span><span class="sxs-lookup"><span data-stu-id="265b5-171">For this configuration, use the following steps to configure Kafka to advertise IP addresses instead of domain names:</span></span>

1. <span data-ttu-id="265b5-172">Pomocí webového prohlížeče, přejděte na https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="265b5-172">Using a web browser, go to https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="265b5-173">Nahraďte __CLUSTERNAME__ s názvem Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="265b5-173">Replace __CLUSTERNAME__ with the name of the Kafka on HDInsight cluster.</span></span>

    <span data-ttu-id="265b5-174">Pokud budete vyzváni, použijte HTTPS uživatelské jméno a heslo pro cluster.</span><span class="sxs-lookup"><span data-stu-id="265b5-174">When prompted, use the HTTPS user name and password for the cluster.</span></span> <span data-ttu-id="265b5-175">Webové uživatelské rozhraní Ambari pro cluster se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="265b5-175">The Ambari Web UI for the cluster is displayed.</span></span>

2. <span data-ttu-id="265b5-176">Chcete-li zobrazit informace o Kafka, vyberte __Kafka__ ze seznamu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="265b5-176">To view information on Kafka, select __Kafka__ from the list on the left.</span></span>

    ![Seznam služeb s Kafka zvýrazněná](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. <span data-ttu-id="265b5-178">Chcete-li zobrazit informace o konfiguraci Kafka, vyberte __konfigurací__ nejvyšší uprostřed.</span><span class="sxs-lookup"><span data-stu-id="265b5-178">To view Kafka configuration, select __Configs__ from the top middle.</span></span>

    ![Konfigurací odkazy pro Kafka](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. <span data-ttu-id="265b5-180">Najít __kafka env__ konfigurace, zadejte `kafka-env` v __filtru__ pole v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="265b5-180">To find the __kafka-env__ configuration, enter `kafka-env` in the __Filter__ field on the upper right.</span></span>

    ![Konfigurace Kafka, pro kafka env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. <span data-ttu-id="265b5-182">Ke konfiguraci Kafka inzerovat IP adres, přidejte následující text k dolní části __kafka. env šablony__ pole:</span><span class="sxs-lookup"><span data-stu-id="265b5-182">To configure Kafka to advertise IP addresses, add the following text to the bottom of the __kafka-env-template__ field:</span></span>

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. <span data-ttu-id="265b5-183">Pokud chcete konfigurovat rozhraní, které Kafka naslouchá na, zadejte `listeners` v __filtru__ pole v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="265b5-183">To configure the interface that Kafka listens on, enter `listeners` in the __Filter__ field on the upper right.</span></span>

7. <span data-ttu-id="265b5-184">Ke konfiguraci Kafka naslouchat na všech síťových rozhraní, změňte hodnotu v __naslouchací procesy__ do `PLAINTEXT://0.0.0.0:9092`.</span><span class="sxs-lookup"><span data-stu-id="265b5-184">To configure Kafka to listen on all network interfaces, change the value in the __listeners__ field to `PLAINTEXT://0.0.0.0:9092`.</span></span>

8. <span data-ttu-id="265b5-185">Chcete-li uložit změny konfigurace, použijte __Uložit__ tlačítko.</span><span class="sxs-lookup"><span data-stu-id="265b5-185">To save the configuration changes, use the __Save__ button.</span></span> <span data-ttu-id="265b5-186">Zadejte textovou zprávu s popisem změny.</span><span class="sxs-lookup"><span data-stu-id="265b5-186">Enter a text message describing the changes.</span></span> <span data-ttu-id="265b5-187">Vyberte __OK__ po změny byly uloženy.</span><span class="sxs-lookup"><span data-stu-id="265b5-187">Select __OK__ once the changes have been saved.</span></span>

    ![Konfigurace tlačítko Uložit](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. <span data-ttu-id="265b5-189">Chcete-li zabránit chybám při restartování Kafka, použijte __služby akce__ tlačítko a vyberte __zapnout v režimu údržby__.</span><span class="sxs-lookup"><span data-stu-id="265b5-189">To prevent errors when restarting Kafka, use the __Service Actions__ button and select __Turn On Maintenance Mode__.</span></span> <span data-ttu-id="265b5-190">Klepněte na tlačítko OK pro dokončení této operace.</span><span class="sxs-lookup"><span data-stu-id="265b5-190">Select OK to complete this operation.</span></span>

    ![Akce služby s zapněte zvýrazněná údržby](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. <span data-ttu-id="265b5-192">Chcete-li restartovat Kafka, použijte __restartujte__ tlačítko a vyberte __restartujte všechny vliv__.</span><span class="sxs-lookup"><span data-stu-id="265b5-192">To restart Kafka, use the __Restart__ button and select __Restart All Affected__.</span></span> <span data-ttu-id="265b5-193">Potvrďte restartování a potom pomocí __OK__ tlačítko po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="265b5-193">Confirm the restart, and then use the __OK__ button after the operation has completed.</span></span>

    ![Restartujte tlačítkem s restartem vliv](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. <span data-ttu-id="265b5-195">Zakázat režim údržby, použijte __služby akce__ tlačítko a vyberte __zapnout vypnout režimu údržby__.</span><span class="sxs-lookup"><span data-stu-id="265b5-195">To disable maintenance mode, use the __Service Actions__ button and select __Turn Off Maintenance Mode__.</span></span> <span data-ttu-id="265b5-196">Vyberte **OK** pro dokončení této operace.</span><span class="sxs-lookup"><span data-stu-id="265b5-196">Select **OK** to complete this operation.</span></span>

### <a name="connect-to-the-vpn-gateway"></a><span data-ttu-id="265b5-197">Připojení ke službě VPN gateway</span><span class="sxs-lookup"><span data-stu-id="265b5-197">Connect to the VPN gateway</span></span>

<span data-ttu-id="265b5-198">Pro připojení k bráně VPN z __klienta Windows__, použijte __připojit k Azure__ části [konfigurace připojení typu Point-to-Site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="265b5-198">To connect to the VPN gateway from a __Windows client__, use the __Connect to Azure__ section of the [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) document.</span></span>

## <span data-ttu-id="265b5-199"><a id="python-client"></a>Příklad: Python klienta</span><span class="sxs-lookup"><span data-stu-id="265b5-199"><a id="python-client"></a> Example: Python client</span></span>

<span data-ttu-id="265b5-200">K ověření připojení k Kafka, použijte následující postup k vytvoření a spuštění producent Python a příjemce:</span><span class="sxs-lookup"><span data-stu-id="265b5-200">To validate connectivity to Kafka, use the following steps to create and run a Python producer and consumer:</span></span>

1. <span data-ttu-id="265b5-201">K načtení plně kvalifikovaný název domény (FQDN) a IP adresy z uzlů v clusteru Kafka, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="265b5-201">Use one of the following methods to retrieve the fully qualified domain name (FQDN) and IP addresses of the nodes in the Kafka cluster:</span></span>

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

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

    <span data-ttu-id="265b5-202">Tento skript předpokládá, že `$resourceGroupName` je název skupiny prostředků Azure, která obsahuje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="265b5-202">This script assumes that `$resourceGroupName` is the name of the Azure resource group that contains the virtual network.</span></span>

    <span data-ttu-id="265b5-203">Uložte vrácené informace pro použití v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="265b5-203">Save the returned information for use in the next steps.</span></span>

2. <span data-ttu-id="265b5-204">Následující informace vám pomůžou nainstalovat [kafka python](http://kafka-python.readthedocs.io/) klienta:</span><span class="sxs-lookup"><span data-stu-id="265b5-204">Use the following to install the [kafka-python](http://kafka-python.readthedocs.io/) client:</span></span>

        pip install kafka-python

3. <span data-ttu-id="265b5-205">K odesílání dat do Kafka, použijte následující kód Python:</span><span class="sxs-lookup"><span data-stu-id="265b5-205">To send data to Kafka, use the following Python code:</span></span>

  ```python
  from kafka import KafkaProducer
  # Replace the `ip_address` entries with the IP address of your worker nodes
  # NOTE: you don't need the full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    <span data-ttu-id="265b5-206">Nahraďte `'kafka_broker'` položky s adresami vrácená z kroku 1 v této části:</span><span class="sxs-lookup"><span data-stu-id="265b5-206">Replace the `'kafka_broker'` entries with the addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="265b5-207">Pokud používáte __klienta VPN softwaru__, nahraďte `kafka_broker` položky s IP adresou uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="265b5-207">If you are using a __Software VPN client__, replace the `kafka_broker` entries with the IP address of your worker nodes.</span></span>

    * <span data-ttu-id="265b5-208">Pokud máte __povolen překlad názvů pomocí vlastního serveru DNS__, nahraďte `kafka_broker` položek s uzly pracovního procesu plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="265b5-208">If you have __enabled name resolution through a custom DNS server__, replace the `kafka_broker` entries with the FQDN of the worker nodes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="265b5-209">Tento kód odešle řetězec `test message` do tématu `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="265b5-209">This code sends the string `test message` to the topic `testtopic`.</span></span> <span data-ttu-id="265b5-210">Výchozí konfigurace Kafka v HDInsight se má vytvořit téma, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="265b5-210">The default configuration of Kafka on HDInsight is to create the topic if it does not exist.</span></span>

4. <span data-ttu-id="265b5-211">Pro načtení zprávy z Kafka, použijte následující kód Python:</span><span class="sxs-lookup"><span data-stu-id="265b5-211">To retrieve the messages from Kafka, use the following Python code:</span></span>

   ```python
   from kafka import KafkaConsumer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # Again, you only need one or two, not the full list.
   # Note: auto_offset_reset='earliest' resets the starting offset to the beginning
   #       of the topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    <span data-ttu-id="265b5-212">Nahraďte `'kafka_broker'` položky s adresami vrácená z kroku 1 v této části:</span><span class="sxs-lookup"><span data-stu-id="265b5-212">Replace the `'kafka_broker'` entries with the addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="265b5-213">Pokud používáte __klienta VPN softwaru__, nahraďte `kafka_broker` položky s IP adresou uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="265b5-213">If you are using a __Software VPN client__, replace the `kafka_broker` entries with the IP address of your worker nodes.</span></span>

    * <span data-ttu-id="265b5-214">Pokud máte __povolen překlad názvů pomocí vlastního serveru DNS__, nahraďte `kafka_broker` položek s uzly pracovního procesu plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="265b5-214">If you have __enabled name resolution through a custom DNS server__, replace the `kafka_broker` entries with the FQDN of the worker nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="265b5-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="265b5-215">Next steps</span></span>

<span data-ttu-id="265b5-216">Další informace o používání HDInsight s virtuální sítě najdete v tématu [rozšíření Azure HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="265b5-216">For more information on using HDInsight with a virtual network, see the [Extend Azure HDInsight using an Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

<span data-ttu-id="265b5-217">Další informace o vytváření virtuální síť Azure s bránou sítě VPN Point-to-Site najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="265b5-217">For more information on creating an Azure Virtual Network with Point-to-Site VPN gateway, see the following documents:</span></span>

* [<span data-ttu-id="265b5-218">Konfigurace připojení typu Point-to-Site pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="265b5-218">Configure a Point-to-Site connection using the Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [<span data-ttu-id="265b5-219">Konfigurace připojení typu Point-to-Site pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="265b5-219">Configure a Point-to-Site connection using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

<span data-ttu-id="265b5-220">Další informace o práci se systémem Kafka v prostředí HDInsight najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="265b5-220">For more information on working with Kafka on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="265b5-221">Začínáme s Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="265b5-221">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="265b5-222">Použít zrcadlení s Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="265b5-222">Use mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
