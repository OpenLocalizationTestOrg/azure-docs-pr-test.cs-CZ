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
# <a name="connect-tookafka-on-hdinsight-preview-through-an-azure-virtual-network"></a><span data-ttu-id="3a5e3-104">Připojení přes virtuální síť Azure tooKafka v HDInsight (preview)</span><span class="sxs-lookup"><span data-stu-id="3a5e3-104">Connect tooKafka on HDInsight (preview) through an Azure Virtual Network</span></span>

<span data-ttu-id="3a5e3-105">Zjistěte, jak připojit toodirectly tooKafka v HDInsight pomocí virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-105">Learn how toodirectly connect tooKafka on HDInsight using Azure Virtual Networks.</span></span> <span data-ttu-id="3a5e3-106">Tento dokument obsahuje informace o připojení tooKafka pomocí hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-106">This document provides information on connecting tooKafka using hello following configurations:</span></span>

* <span data-ttu-id="3a5e3-107">Z prostředků v místní síti.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-107">From resources in an on-premises network.</span></span> <span data-ttu-id="3a5e3-108">Tato připojení pomocí zařízení VPN (softwaru nebo hardwaru) ve vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-108">This connection is established by using a VPN device (software or hardware) on your local network.</span></span>
* <span data-ttu-id="3a5e3-109">Z prostředí pro vývoj pomocí softwaru klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-109">From a development environment using a VPN software client.</span></span>

## <a name="architecture-and-planning"></a><span data-ttu-id="3a5e3-110">Plánování a architektura</span><span class="sxs-lookup"><span data-stu-id="3a5e3-110">Architecture and planning</span></span>

<span data-ttu-id="3a5e3-111">HDInsight neumožňuje tooKafka přímé připojení přes hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-111">HDInsight does not allow direct connection tooKafka over hello public internet.</span></span> <span data-ttu-id="3a5e3-112">Místo toho Kafka klientů (producenti a spotřebitelé) musí používat jednu z hello následující metody připojení:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-112">Instead, Kafka clients (producers and consumers) must use one of hello following connection methods:</span></span>

* <span data-ttu-id="3a5e3-113">Spusťte klienta hello v hello stejné virtuální síti jako Kafka v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-113">Run hello client in hello same virtual network as Kafka on HDInsight.</span></span> <span data-ttu-id="3a5e3-114">Tato konfigurace se používá v hello [začínat Apache Kafka (preview) na HDInsight](hdinsight-apache-kafka-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-114">This configuration is used in hello [Start with Apache Kafka (preview) on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span> <span data-ttu-id="3a5e3-115">Hello klienta spustí přímo na hello HDInsight uzly clusteru nebo na jiný virtuální počítač v hello stejné síti.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-115">hello client runs directly on hello HDInsight cluster nodes or on another virtual machine in hello same network.</span></span>

* <span data-ttu-id="3a5e3-116">Připojte privátní síti, například v místní síti toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-116">Connect a private network, such as your on-premises network, toohello virtual network.</span></span> <span data-ttu-id="3a5e3-117">Tato konfigurace umožňuje klientům ve vaší místní síti toodirectly práce se Kafka.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-117">This configuration allows clients in your on-premises network toodirectly work with Kafka.</span></span> <span data-ttu-id="3a5e3-118">tooenable tuto konfiguraci provést hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-118">tooenable this configuration, perform hello following tasks:</span></span>

    1. <span data-ttu-id="3a5e3-119">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-119">Create a virtual network.</span></span>
    2. <span data-ttu-id="3a5e3-120">Vytvořte bránu VPN, který používá konfiguraci site-to-site.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-120">Create a VPN gateway that uses a site-to-site configuration.</span></span> <span data-ttu-id="3a5e3-121">Konfigurace Hello použitá v tomto dokumentu se připojí tooa zařízení brány sítě VPN v síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-121">hello configuration used in this document connects tooa VPN gateway device in your on-premises network.</span></span>
    3. <span data-ttu-id="3a5e3-122">Vytvoření serveru DNS ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-122">Create a DNS server in hello virtual network.</span></span>
    4. <span data-ttu-id="3a5e3-123">Konfigurace předávání mezi hello server DNS v každé sítě.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-123">Configure forwarding between hello DNS server in each network.</span></span>
    5. <span data-ttu-id="3a5e3-124">Nainstalujte Kafka v HDInsight do virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-124">Install Kafka on HDInsight into hello virtual network.</span></span>

    <span data-ttu-id="3a5e3-125">Další informace najdete v tématu hello [připojit tooKafka z místní sítě](#on-premises) části.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-125">For more information, see hello [Connect tooKafka from an on-premises network](#on-premises) section.</span></span> 

* <span data-ttu-id="3a5e3-126">Připojte virtuální sítě pro jednotlivé počítače toohello pomocí klienta VPN a brány VPN.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-126">Connect individual machines toohello virtual network using a VPN gateway and VPN client.</span></span> <span data-ttu-id="3a5e3-127">tooenable tuto konfiguraci provést hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-127">tooenable this configuration, perform hello following tasks:</span></span>

    1. <span data-ttu-id="3a5e3-128">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-128">Create a virtual network.</span></span>
    2. <span data-ttu-id="3a5e3-129">Vytvořte bránu VPN, který používá konfiguraci point-to-site.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-129">Create a VPN gateway that uses a point-to-site configuration.</span></span> <span data-ttu-id="3a5e3-130">Tato konfigurace poskytuje klienta VPN, který se dá nainstalovat na klienty se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-130">This configuration provides a VPN client that can be installed on Windows clients.</span></span>
    3. <span data-ttu-id="3a5e3-131">Nainstalujte Kafka v HDInsight do virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-131">Install Kafka on HDInsight into hello virtual network.</span></span>
    4. <span data-ttu-id="3a5e3-132">Nakonfigurujte Kafka pro inzerování IP.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-132">Configure Kafka for IP advertising.</span></span> <span data-ttu-id="3a5e3-133">Tato konfigurace umožňuje tooconnect hello klienta pomocí IP adres namísto názvů domény.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-133">This configuration allows hello client tooconnect using IP addressing instead of domain names.</span></span>
    5. <span data-ttu-id="3a5e3-134">Stáhnout a použít klienta VPN hello vývoj systému hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-134">Download and use hello VPN client on hello development system.</span></span>

    <span data-ttu-id="3a5e3-135">Další informace najdete v tématu hello [připojit tooKafka u klientů VPN](#vpnclient) části.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-135">For more information, see hello [Connect tooKafka with a VPN client](#vpnclient) section.</span></span>

    > [!WARNING]
    > <span data-ttu-id="3a5e3-136">Tato konfigurace se doporučuje jenom pro účely vývoje kvůli hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-136">This configuration is only recommended for development purposes because of hello following limitations:</span></span>
    >
    > * <span data-ttu-id="3a5e3-137">Každý klient musí připojit pomocí softwaru klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-137">Each client must connect using a VPN software client.</span></span> <span data-ttu-id="3a5e3-138">Azure poskytuje jenom klienta se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-138">Azure only provides a Windows-based client.</span></span>
    > * <span data-ttu-id="3a5e3-139">Hello klienta nepředává název řešení požadavky toohello virtuální sítě, takže je nutné použít IP adresách toocommunicate s Kafka.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-139">hello client does not pass name resolution requests toohello virtual network, so you must use IP addressing toocommunicate with Kafka.</span></span> <span data-ttu-id="3a5e3-140">Komunikaci IP vyžaduje další konfiguraci na hello Kafka clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-140">IP communication requires additional configuration on hello Kafka cluster.</span></span>

<span data-ttu-id="3a5e3-141">Další informace o používání HDInsight ve virtuální síti, najdete v části [rozšířit HDInsight pomocí virtuálních sítí Azure](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="3a5e3-141">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

## <span data-ttu-id="3a5e3-142"><a id="on-premises"></a>Připojit tooKafka z místní sítě</span><span class="sxs-lookup"><span data-stu-id="3a5e3-142"><a id="on-premises"></a> Connect tooKafka from an on-premises network</span></span>

<span data-ttu-id="3a5e3-143">toocreate Kafka clusteru, který komunikuje s vaší místní síti, postupujte podle kroků hello v hello [připojit HDInsight tooyour do místní sítě](./connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-143">toocreate a Kafka cluster that communicates with your on-premises network, follow hello steps in hello [Connect HDInsight tooyour on-premises network](./connect-on-premises-network.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a5e3-144">Při vytváření clusteru HDInsight hello, vyberte hello __Kafka__ clusteru typu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-144">When creating hello HDInsight cluster, select hello __Kafka__ cluster type.</span></span>

<span data-ttu-id="3a5e3-145">Pomocí těchto kroků vytvoříte hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-145">These steps create hello following configuration:</span></span>

* <span data-ttu-id="3a5e3-146">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="3a5e3-146">Azure Virtual Network</span></span>
* <span data-ttu-id="3a5e3-147">Brána Site-to-site VPN</span><span class="sxs-lookup"><span data-stu-id="3a5e3-147">Site-to-site VPN gateway</span></span>
* <span data-ttu-id="3a5e3-148">Účet služby Azure Storage (používá se v prostředí HDInsight)</span><span class="sxs-lookup"><span data-stu-id="3a5e3-148">Azure Storage account (used by HDInsight)</span></span>
* <span data-ttu-id="3a5e3-149">Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3a5e3-149">Kafka on HDInsight</span></span>

<span data-ttu-id="3a5e3-150">zda Kafka klienta můžete připojit toohello clusteru z místní, použijte kroky hello v hello tooverify [příklad: klienta Python](#python-client) části.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-150">tooverify that a Kafka client can connect toohello cluster from on-premises, use hello steps in hello [Example: Python client](#python-client) section.</span></span>

## <span data-ttu-id="3a5e3-151"><a id="vpnclient"></a>Připojit tooKafka u klientů VPN</span><span class="sxs-lookup"><span data-stu-id="3a5e3-151"><a id="vpnclient"></a> Connect tooKafka with a VPN client</span></span>

<span data-ttu-id="3a5e3-152">Použijte hello kroky v této části toocreate hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-152">Use hello steps in this section toocreate hello following configuration:</span></span>

* <span data-ttu-id="3a5e3-153">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="3a5e3-153">Azure Virtual Network</span></span>
* <span data-ttu-id="3a5e3-154">Brána sítě VPN Point-to-site</span><span class="sxs-lookup"><span data-stu-id="3a5e3-154">Point-to-site VPN gateway</span></span>
* <span data-ttu-id="3a5e3-155">Účet služby Azure Storage (používá se v prostředí HDInsight)</span><span class="sxs-lookup"><span data-stu-id="3a5e3-155">Azure Storage Account (used by HDInsight)</span></span>
* <span data-ttu-id="3a5e3-156">Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3a5e3-156">Kafka on HDInsight</span></span>

1. <span data-ttu-id="3a5e3-157">Postupujte podle kroků hello v hello [práce s certifikáty podepsané svým držitelem pro připojení Point-to-site](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-157">Follow hello steps in hello [Working with self-signed certificates for Point-to-site connections](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document.</span></span> <span data-ttu-id="3a5e3-158">Tento dokument vytvoří hello certifikáty potřebné pro bránu hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-158">This document creates hello certificates needed for hello gateway.</span></span>

2. <span data-ttu-id="3a5e3-159">Otevřete příkazovém řádku prostředí PowerShell a použít následující kód toolog v tooyour předplatného Azure hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-159">Open a PowerShell prompt and use hello following code toolog in tooyour Azure subscription:</span></span>

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment tooset hello subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. <span data-ttu-id="3a5e3-160">Použijte následující proměnné toocreate kódu, které obsahují informace o konfiguraci hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-160">Use hello following code toocreate variables that contain configuration information:</span></span>

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

4. <span data-ttu-id="3a5e3-161">Použití hello následující kód Skupina prostředků Azure hello toocreate a virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-161">Use hello following code toocreate hello Azure resource group and virtual network:</span></span>

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
    > <span data-ttu-id="3a5e3-162">Ho může trvat několik minut, než toocomplete tento proces.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-162">It can take several minutes for this process toocomplete.</span></span>

5. <span data-ttu-id="3a5e3-163">Použijte následující kód toocreate hello účet úložiště Azure a objektů blob kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-163">Use hello following code toocreate hello Azure Storage Account and blob container:</span></span>

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

6. <span data-ttu-id="3a5e3-164">Použijte následující clusteru HDInsight hello toocreate kód hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-164">Use hello following code toocreate hello HDInsight cluster:</span></span>

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
  > <span data-ttu-id="3a5e3-165">Tento proces trvá přibližně 20 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-165">This process takes around 20 minutes toocomplete.</span></span>

8. <span data-ttu-id="3a5e3-166">Použijte následující rutinu tooretrieve hello adresu URL pro klienta VPN ve Windows hello pro virtuální síť hello hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-166">Use hello following cmdlet tooretrieve hello URL for hello Windows VPN client for hello virtual network:</span></span>

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    <span data-ttu-id="3a5e3-167">klienta VPN ve Windows hello toodownload, použijte hello vrátil URI ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-167">toodownload hello Windows VPN client, use hello returned URI in your web browser.</span></span>

### <a name="configure-kafka-for-ip-advertising"></a><span data-ttu-id="3a5e3-168">Konfigurace Kafka pro inzerování IP</span><span class="sxs-lookup"><span data-stu-id="3a5e3-168">Configure Kafka for IP advertising</span></span>

<span data-ttu-id="3a5e3-169">Ve výchozím nastavení Zookeeper vrátí název domény hello hello Kafka zprostředkovatelé tooclients.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-169">By default, Zookeeper returns hello domain name of hello Kafka brokers tooclients.</span></span> <span data-ttu-id="3a5e3-170">Tato konfigurace nefunguje s hello softwaru klienta VPN, překlad nemůže použít pro entity ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-170">This configuration does not work with hello VPN software client, as it cannot use name resolution for entities in hello virtual network.</span></span> <span data-ttu-id="3a5e3-171">Pro tuto konfiguraci, použijte následující hello kroky tooconfigure Kafka tooadvertise IP adres místo názvy domén:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-171">For this configuration, use hello following steps tooconfigure Kafka tooadvertise IP addresses instead of domain names:</span></span>

1. <span data-ttu-id="3a5e3-172">Pomocí webového prohlížeče, přejděte toohttps://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-172">Using a web browser, go toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="3a5e3-173">Nahraďte __CLUSTERNAME__ s názvem hello hello Kafka na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-173">Replace __CLUSTERNAME__ with hello name of hello Kafka on HDInsight cluster.</span></span>

    <span data-ttu-id="3a5e3-174">Pokud budete vyzváni, použijte hello HTTPS uživatelské jméno a heslo pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-174">When prompted, use hello HTTPS user name and password for hello cluster.</span></span> <span data-ttu-id="3a5e3-175">Zobrazí se Hello webové uživatelské rozhraní Ambari pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-175">hello Ambari Web UI for hello cluster is displayed.</span></span>

2. <span data-ttu-id="3a5e3-176">Vyberte tooview informace o Kafka, __Kafka__ hello seznamu na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-176">tooview information on Kafka, select __Kafka__ from hello list on hello left.</span></span>

    ![Seznam služeb s Kafka zvýrazněná](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. <span data-ttu-id="3a5e3-178">Vyberte tooview konfigurace Kafka, __konfigurací__ z horní střední hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-178">tooview Kafka configuration, select __Configs__ from hello top middle.</span></span>

    ![Konfigurací odkazy pro Kafka](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. <span data-ttu-id="3a5e3-180">toofind hello __kafka env__ konfigurace, zadejte `kafka-env` v hello __filtru__ pole v horní pravé hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-180">toofind hello __kafka-env__ configuration, enter `kafka-env` in hello __Filter__ field on hello upper right.</span></span>

    ![Konfigurace Kafka, pro kafka env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. <span data-ttu-id="3a5e3-182">tooconfigure Kafka tooadvertise IP adres, přidejte následující text toohello dolní části hello hello __kafka. env šablony__ pole:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-182">tooconfigure Kafka tooadvertise IP addresses, add hello following text toohello bottom of hello __kafka-env-template__ field:</span></span>

    ```
    # Configure Kafka tooadvertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. <span data-ttu-id="3a5e3-183">tooconfigure hello rozhraní, které Kafka naslouchá na, zadejte `listeners` v hello __filtru__ pole v horní pravé hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-183">tooconfigure hello interface that Kafka listens on, enter `listeners` in hello __Filter__ field on hello upper right.</span></span>

7. <span data-ttu-id="3a5e3-184">tooconfigure Kafka toolisten na všech síťových rozhraní, hodnota hello změnit v hello __naslouchací procesy__ pole příliš`PLAINTEXT://0.0.0.0:9092`.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-184">tooconfigure Kafka toolisten on all network interfaces, change hello value in hello __listeners__ field too`PLAINTEXT://0.0.0.0:9092`.</span></span>

8. <span data-ttu-id="3a5e3-185">změny konfigurace hello toosave, použijte hello __Uložit__ tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-185">toosave hello configuration changes, use hello __Save__ button.</span></span> <span data-ttu-id="3a5e3-186">Zadejte textovou zprávu s popisem hello změny.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-186">Enter a text message describing hello changes.</span></span> <span data-ttu-id="3a5e3-187">Vyberte __OK__ po uložení změn hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-187">Select __OK__ once hello changes have been saved.</span></span>

    ![Konfigurace tlačítko Uložit](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. <span data-ttu-id="3a5e3-189">tooprevent chyby při restartování Kafka, použijte hello __služby akce__ tlačítko a vyberte __zapnout v režimu údržby__.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-189">tooprevent errors when restarting Kafka, use hello __Service Actions__ button and select __Turn On Maintenance Mode__.</span></span> <span data-ttu-id="3a5e3-190">Vyberte OK toocomplete tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-190">Select OK toocomplete this operation.</span></span>

    ![Akce služby s zapněte zvýrazněná údržby](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. <span data-ttu-id="3a5e3-192">toorestart Kafka, použijte hello __restartujte__ tlačítko a vyberte __restartujte všechny vliv__.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-192">toorestart Kafka, use hello __Restart__ button and select __Restart All Affected__.</span></span> <span data-ttu-id="3a5e3-193">Potvrďte hello restartování a pak použít hello __OK__ tlačítko po dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-193">Confirm hello restart, and then use hello __OK__ button after hello operation has completed.</span></span>

    ![Restartujte tlačítkem s restartem vliv](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. <span data-ttu-id="3a5e3-195">toodisable režimu údržby, použijte hello __služby akce__ tlačítko a vyberte __zapnout vypnout režimu údržby__.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-195">toodisable maintenance mode, use hello __Service Actions__ button and select __Turn Off Maintenance Mode__.</span></span> <span data-ttu-id="3a5e3-196">Vyberte **OK** toocomplete tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-196">Select **OK** toocomplete this operation.</span></span>

### <a name="connect-toohello-vpn-gateway"></a><span data-ttu-id="3a5e3-197">Připojení brány VPN toohello</span><span class="sxs-lookup"><span data-stu-id="3a5e3-197">Connect toohello VPN gateway</span></span>

<span data-ttu-id="3a5e3-198">Brána sítě VPN toohello tooconnect z __klienta Windows__, použijte hello __připojit tooAzure__ části hello [konfigurace připojení typu Point-to-Site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-198">tooconnect toohello VPN gateway from a __Windows client__, use hello __Connect tooAzure__ section of hello [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) document.</span></span>

## <span data-ttu-id="3a5e3-199"><a id="python-client"></a>Příklad: Python klienta</span><span class="sxs-lookup"><span data-stu-id="3a5e3-199"><a id="python-client"></a> Example: Python client</span></span>

<span data-ttu-id="3a5e3-200">toovalidate tooKafka připojení, použijte následující postup toocreate hello a spusťte producent Python a příjemce:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-200">toovalidate connectivity tooKafka, use hello following steps toocreate and run a Python producer and consumer:</span></span>

1. <span data-ttu-id="3a5e3-201">Použití jednoho hello následující metody tooretrieve hello plně kvalifikovaný název domény (FQDN) a IP adresy hello uzlů v clusteru Kafka hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-201">Use one of hello following methods tooretrieve hello fully qualified domain name (FQDN) and IP addresses of hello nodes in hello Kafka cluster:</span></span>

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

    <span data-ttu-id="3a5e3-202">Tento skript předpokládá, že `$resourceGroupName` je hello název skupiny prostředků Azure hello, která obsahuje virtuální síť hello.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-202">This script assumes that `$resourceGroupName` is hello name of hello Azure resource group that contains hello virtual network.</span></span>

    <span data-ttu-id="3a5e3-203">Uložte hello vrátí informace o pro použití v hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-203">Save hello returned information for use in hello next steps.</span></span>

2. <span data-ttu-id="3a5e3-204">Použití hello následující tooinstall hello [kafka python](http://kafka-python.readthedocs.io/) klienta:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-204">Use hello following tooinstall hello [kafka-python](http://kafka-python.readthedocs.io/) client:</span></span>

        pip install kafka-python

3. <span data-ttu-id="3a5e3-205">toosend data tooKafka hello použijte následující kód Python:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-205">toosend data tooKafka, use hello following Python code:</span></span>

  ```python
  from kafka import KafkaProducer
  # Replace hello `ip_address` entries with hello IP address of your worker nodes
  # NOTE: you don't need hello full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    <span data-ttu-id="3a5e3-206">Nahraďte hello `'kafka_broker'` položky s adresami hello vrácená z kroku 1 v této části:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-206">Replace hello `'kafka_broker'` entries with hello addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="3a5e3-207">Pokud používáte __klienta VPN softwaru__, nahraďte hello `kafka_broker` položky s IP adresou hello uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-207">If you are using a __Software VPN client__, replace hello `kafka_broker` entries with hello IP address of your worker nodes.</span></span>

    * <span data-ttu-id="3a5e3-208">Pokud máte __povolen překlad názvů pomocí vlastního serveru DNS__, nahraďte hello `kafka_broker` položky s hello plně kvalifikovaný název domény hello uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-208">If you have __enabled name resolution through a custom DNS server__, replace hello `kafka_broker` entries with hello FQDN of hello worker nodes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3a5e3-209">Tento kód odešle řetězec hello `test message` toohello tématu `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-209">This code sends hello string `test message` toohello topic `testtopic`.</span></span> <span data-ttu-id="3a5e3-210">Výchozí konfigurace Hello Kafka v HDInsight je toocreate hello tématu, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-210">hello default configuration of Kafka on HDInsight is toocreate hello topic if it does not exist.</span></span>

4. <span data-ttu-id="3a5e3-211">tooretrieve hello zprávy z Kafka, použijte následující kód Python hello:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-211">tooretrieve hello messages from Kafka, use hello following Python code:</span></span>

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

    <span data-ttu-id="3a5e3-212">Nahraďte hello `'kafka_broker'` položky s adresami hello vrácená z kroku 1 v této části:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-212">Replace hello `'kafka_broker'` entries with hello addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="3a5e3-213">Pokud používáte __klienta VPN softwaru__, nahraďte hello `kafka_broker` položky s IP adresou hello uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-213">If you are using a __Software VPN client__, replace hello `kafka_broker` entries with hello IP address of your worker nodes.</span></span>

    * <span data-ttu-id="3a5e3-214">Pokud máte __povolen překlad názvů pomocí vlastního serveru DNS__, nahraďte hello `kafka_broker` položky s hello plně kvalifikovaný název domény hello uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-214">If you have __enabled name resolution through a custom DNS server__, replace hello `kafka_broker` entries with hello FQDN of hello worker nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a5e3-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a5e3-215">Next steps</span></span>

<span data-ttu-id="3a5e3-216">Další informace o používání HDInsight s virtuální sítí, najdete v části hello [rozšíření Azure HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3a5e3-216">For more information on using HDInsight with a virtual network, see hello [Extend Azure HDInsight using an Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

<span data-ttu-id="3a5e3-217">Další informace o vytváření virtuální síť Azure s bránou sítě VPN Point-to-Site najdete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-217">For more information on creating an Azure Virtual Network with Point-to-Site VPN gateway, see hello following documents:</span></span>

* [<span data-ttu-id="3a5e3-218">Konfigurace připojení typu Point-to-Site pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3a5e3-218">Configure a Point-to-Site connection using hello Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [<span data-ttu-id="3a5e3-219">Konfigurace připojení typu Point-to-Site pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3a5e3-219">Configure a Point-to-Site connection using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

<span data-ttu-id="3a5e3-220">Další informace o práci s Kafka v HDInsight naleznete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="3a5e3-220">For more information on working with Kafka on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="3a5e3-221">Začínáme s Kafka ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="3a5e3-221">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="3a5e3-222">Použít zrcadlení s Kafka v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3a5e3-222">Use mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
