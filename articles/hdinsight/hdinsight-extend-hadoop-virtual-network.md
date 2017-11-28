---
title: "Rozšíření prostředí HDInsight pomocí virtuální sítě - Azure | Microsoft Docs"
description: "Naučte se používat pro připojení HDInsight k jiným cloudovým prostředkům nebo prostředkům ve vašem datovém centru Azure Virtual Network"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: 380423ec42ad4905c73fcd57501102e9f7062e81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a><span data-ttu-id="a438f-103">Rozšíření Azure HDInsight pomocí virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="a438f-103">Extend Azure HDInsight using an Azure Virtual Network</span></span>

<span data-ttu-id="a438f-104">Další informace o použití prostředí HDInsight pomocí [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a438f-104">Learn how to use HDInsight with an [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="a438f-105">Použití virtuální sítě Azure umožňuje následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="a438f-105">Using an Azure Virtual Network enables the following scenarios:</span></span>

* <span data-ttu-id="a438f-106">Připojení k HDInsight přímo z místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-106">Connecting to HDInsight directly from an on-premises network.</span></span>

* <span data-ttu-id="a438f-107">Připojování k datům HDInsight ukládá v Azure virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-107">Connecting HDInsight to data stores in an Azure Virtual network.</span></span>

* <span data-ttu-id="a438f-108">Přímý přístup k služby Hadoop, které nejsou k dispozici veřejně přes internet.</span><span class="sxs-lookup"><span data-stu-id="a438f-108">Directly accessing Hadoop services that are not available publicly over the internet.</span></span> <span data-ttu-id="a438f-109">Například Kafka rozhraní API nebo rozhraní API HBase Java.</span><span class="sxs-lookup"><span data-stu-id="a438f-109">For example, Kafka APIs or the HBase Java API.</span></span>

> [!WARNING]
> <span data-ttu-id="a438f-110">Informace v tomto dokumentu vyžaduje znalosti o protokolu TCP/IP v síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-110">The information in this document requires an understanding of TCP/IP networking.</span></span> <span data-ttu-id="a438f-111">Pokud nejste obeznámeni s prací v síti TCP/IP, by měla spolupracovat s uživatelem, který je před provedením změny produkční sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-111">If you are not familiar with TCP/IP networking, you should partner with someone who is before making modifications to production networks.</span></span>

## <a name="planning"></a><span data-ttu-id="a438f-112">Plánování</span><span class="sxs-lookup"><span data-stu-id="a438f-112">Planning</span></span>

<span data-ttu-id="a438f-113">Tady jsou otázky, které je nutné zodpovědět při plánování instalace HDInsight ve virtuální síti:</span><span class="sxs-lookup"><span data-stu-id="a438f-113">The following are the questions that you must answer when planning to install HDInsight in a virtual network:</span></span>

* <span data-ttu-id="a438f-114">Potřebujete k instalaci HDInsight do existující virtuální síť?</span><span class="sxs-lookup"><span data-stu-id="a438f-114">Do you need to install HDInsight into an existing virtual network?</span></span> <span data-ttu-id="a438f-115">Nebo můžete vytvořit novou síť?</span><span class="sxs-lookup"><span data-stu-id="a438f-115">Or are you creating a new network?</span></span>

    <span data-ttu-id="a438f-116">Pokud používáte stávající virtuální síť, musíte změnit síťovou konfiguraci, před instalací HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-116">If you are using an existing virtual network, you may need to modify the network configuration before you can install HDInsight.</span></span> <span data-ttu-id="a438f-117">Další informace najdete v tématu [přidat HDInsight k existující virtuální síti](#existingvnet) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-117">For more information, see the [add HDInsight to an existing virtual network](#existingvnet) section.</span></span>

* <span data-ttu-id="a438f-118">Opravdu chcete připojit virtuální síť obsahující HDInsight k jiné virtuální síti nebo v místní síti?</span><span class="sxs-lookup"><span data-stu-id="a438f-118">Do you want to connect the virtual network containing HDInsight to another virtual network or your on-premises network?</span></span>

    <span data-ttu-id="a438f-119">Snadno pracovat s prostředky v sítích, můžete vytvořit vlastní DNS a nakonfigurujte předávání DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-119">To easily work with resources across networks, you may need to create a custom DNS and configure DNS forwarding.</span></span> <span data-ttu-id="a438f-120">Další informace najdete v tématu [připojení více sítí](#multinet) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-120">For more information, see the [connecting multiple networks](#multinet) section.</span></span>

* <span data-ttu-id="a438f-121">Chcete omezit či přesměrování příchozích a odchozích přenosů do HDInsight</span><span class="sxs-lookup"><span data-stu-id="a438f-121">Do you want to restrict/redirect inbound or outbound traffic to HDInsight?</span></span>

    <span data-ttu-id="a438f-122">HDInsight musí mít neomezený komunikace s konkrétní IP adresy v datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-122">HDInsight must have unrestricted communication with specific IP addresses in the Azure data center.</span></span> <span data-ttu-id="a438f-123">Existují také několik portů, které musí být povoleno přes brány firewall pro komunikaci klienta.</span><span class="sxs-lookup"><span data-stu-id="a438f-123">There are also several ports that must be allowed through firewalls for client communication.</span></span> <span data-ttu-id="a438f-124">Další informace najdete v tématu [řízení síťového provozu](#networktraffic) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-124">For more information, see the [controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="a438f-125"><a id="existingvnet"></a>Přidání HDInsight do existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="a438f-125"><a id="existingvnet"></a>Add HDInsight to an existing virtual network</span></span>

<span data-ttu-id="a438f-126">Pokud chcete zjistit, jak přidat nové HDInsight do existující virtuální síť Azure, postupujte podle kroků v této části.</span><span class="sxs-lookup"><span data-stu-id="a438f-126">Use the steps in this section to discover how to add a new HDInsight to an existing Azure Virtual Network.</span></span>

> [!NOTE]
> <span data-ttu-id="a438f-127">Nelze přidat stávající cluster HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-127">You cannot add an existing HDInsight cluster into a virtual network.</span></span>

1. <span data-ttu-id="a438f-128">Používáte pro virtuální síť klasický nebo modelu nasazení Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="a438f-128">Are you using a classic or Resource Manager deployment model for the virtual network?</span></span>

    <span data-ttu-id="a438f-129">HDInsight 3.4 a větší vyžaduje virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a438f-129">HDInsight 3.4 and greater requires a Resource Manager virtual network.</span></span> <span data-ttu-id="a438f-130">Dřívějších verzích HDInsight vyžaduje klasickou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="a438f-130">Earlier versions of HDInsight required a classic virtual network.</span></span>

    <span data-ttu-id="a438f-131">Pokud vaší stávající sítě klasickou virtuální síť, musíte vytvořit virtuální síť Resource Manager a potom připojení dvě.</span><span class="sxs-lookup"><span data-stu-id="a438f-131">If your existing network is a classic virtual network, then you must create a Resource Manager virtual network and then connect the two.</span></span> <span data-ttu-id="a438f-132">[Připojení virtuální sítě classic k nové virtuální sítě](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a438f-132">[Connecting classic VNets to new VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span></span>

    <span data-ttu-id="a438f-133">Jakmile připojený, HDInsight v síti Resource Manager nainstalovaný mohou komunikovat s prostředky v síti classic.</span><span class="sxs-lookup"><span data-stu-id="a438f-133">Once joined, HDInsight installed in the Resource Manager network can interact with resources in the classic network.</span></span>

2. <span data-ttu-id="a438f-134">Používáte vynucené tunelování?</span><span class="sxs-lookup"><span data-stu-id="a438f-134">Do you use forced tunneling?</span></span> <span data-ttu-id="a438f-135">Vynucené tunelování je nastavení podsítě, které vynutí odchozí přenosy z Internetu do zařízení pro kontroly a protokolování.</span><span class="sxs-lookup"><span data-stu-id="a438f-135">Forced tunneling is a subnet setting that forces outbound Internet traffic to a device for inspection and logging.</span></span> <span data-ttu-id="a438f-136">HDInsight nepodporuje vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="a438f-136">HDInsight does not support forced tunneling.</span></span> <span data-ttu-id="a438f-137">Buď odeberte vynucené tunelování před instalací HDInsight do podsítě, nebo vytvořit novou podsíť pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-137">Either remove forced tunneling before installing HDInsight into a subnet, or create a new subnet for HDInsight.</span></span>

3. <span data-ttu-id="a438f-138">Používáte skupiny zabezpečení sítě, trasy definované uživatelem nebo virtuální síťové zařízení k omezení přenosu do nebo z virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="a438f-138">Do you use network security groups, user-defined routes, or Virtual Network Appliances to restrict traffic into or out of the virtual network?</span></span>

    <span data-ttu-id="a438f-139">Jako spravovanou službu vyžaduje HDInsight neomezený přístup k několika IP adresy v datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-139">As a managed service, HDInsight requires unrestricted access to several IP addresses in the Azure data center.</span></span> <span data-ttu-id="a438f-140">Povolit komunikaci se tyto IP adresy, aktualizujte všechny existující skupiny zabezpečení sítě nebo trasy definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="a438f-140">To allow communication with these IP addresses, update any existing network security groups or user-defined routes.</span></span>

    <span data-ttu-id="a438f-141">HDInsight je hostitelem více služeb, které používají různé porty.</span><span class="sxs-lookup"><span data-stu-id="a438f-141">HDInsight  hosts multiple services, which use a variety of ports.</span></span> <span data-ttu-id="a438f-142">Nejsou blokovány přenosy na těchto portech.</span><span class="sxs-lookup"><span data-stu-id="a438f-142">Do not block traffic to these ports.</span></span> <span data-ttu-id="a438f-143">Seznam portů pro tvorbu přes virtuální zařízení brány firewall, naleznete v části [zabezpečení](#security) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-143">For a list of ports to allow through virtual appliance firewalls, see the [Security](#security) section.</span></span>

    <span data-ttu-id="a438f-144">Pokud chcete vyhledat existující konfiguraci zabezpečení, použijte následující příkazy prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="a438f-144">To find your existing security configuration, use the following Azure PowerShell or Azure CLI commands:</span></span>

    * <span data-ttu-id="a438f-145">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="a438f-145">Network security groups</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="a438f-146">Další informace najdete v tématu [odstraňování skupin zabezpečení sítě](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-146">For more information, see the [Troubleshoot network security groups](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) document.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="a438f-147">Pravidla skupiny zabezpečení sítě jsou použity v pořadí podle priority pravidel.</span><span class="sxs-lookup"><span data-stu-id="a438f-147">Network security group rules are applied in order based on rule priority.</span></span> <span data-ttu-id="a438f-148">První pravidlo, který odpovídá vzorku provoz se použije a žádné jiné se použijí pro tento přenos.</span><span class="sxs-lookup"><span data-stu-id="a438f-148">The first rule that matches the traffic pattern is applied, and no others are applied for that traffic.</span></span> <span data-ttu-id="a438f-149">Pravidla pořadí od nejvíce projektovou na omezenou.</span><span class="sxs-lookup"><span data-stu-id="a438f-149">Order rules from most permissive to least permissive.</span></span> <span data-ttu-id="a438f-150">Další informace najdete v tématu [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-150">For more information, see the [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    * <span data-ttu-id="a438f-151">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="a438f-151">User-defined routes</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="a438f-152">Další informace najdete v tématu [řešení potíží s trasy](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-152">For more information, see the [Troubleshoot routes](../virtual-network/virtual-network-routes-troubleshoot-portal.md) document.</span></span>

4. <span data-ttu-id="a438f-153">Vytvoření clusteru HDInsight a vyberte virtuální síť Azure během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a438f-153">Create an HDInsight cluster and select the Azure Virtual Network during configuration.</span></span> <span data-ttu-id="a438f-154">Porozumět procesu vytváření clusteru, postupujte podle kroků v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="a438f-154">Use the steps in the following documents to understand the cluster creation process:</span></span>

    * [<span data-ttu-id="a438f-155">Vytvoření HDInsight pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a438f-155">Create HDInsight using the Azure portal</span></span>](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [<span data-ttu-id="a438f-156">Vytvoření HDInsight pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="a438f-156">Create HDInsight using Azure PowerShell</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [<span data-ttu-id="a438f-157">Vytvoření HDInsight pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a438f-157">Create HDInsight using Azure CLI 1.0</span></span>](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [<span data-ttu-id="a438f-158">Vytvoření HDInsight pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a438f-158">Create HDInsight using an Azure Resource Manager template</span></span>](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > <span data-ttu-id="a438f-159">Přidání HDInsight k virtuální síti je krok volitelné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a438f-159">Adding HDInsight to a virtual network is an optional configuration step.</span></span> <span data-ttu-id="a438f-160">Je nutné vybrat virtuální síť, při konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="a438f-160">Be sure to select the virtual network when configuring the cluster.</span></span>

## <span data-ttu-id="a438f-161"><a id="multinet"></a>Připojení více sítí</span><span class="sxs-lookup"><span data-stu-id="a438f-161"><a id="multinet"></a>Connecting multiple networks</span></span>

<span data-ttu-id="a438f-162">Největší výzvou s konfigurací s více sítě je překlad mezi sítěmi.</span><span class="sxs-lookup"><span data-stu-id="a438f-162">The biggest challenge with a multi-network configuration is name resolution between the networks.</span></span>

<span data-ttu-id="a438f-163">Azure poskytuje překlad názvů pro služby Azure, které jsou nainstalovány ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-163">Azure provides name resolution for Azure services that are installed in a virtual network.</span></span> <span data-ttu-id="a438f-164">Toto řešení předdefinovaným názvem umožňuje HDInsight pro připojení v následujících zdrojích informací s použitím platný plně kvalifikovaný název domény (FQDN):</span><span class="sxs-lookup"><span data-stu-id="a438f-164">This built-in name resolution allows HDInsight to connect to the following resources by using a fully qualified domain name (FQDN):</span></span>

* <span data-ttu-id="a438f-165">Jakémukoli prostředku, který je dostupný na Internetu.</span><span class="sxs-lookup"><span data-stu-id="a438f-165">Any resource that is available on the internet.</span></span> <span data-ttu-id="a438f-166">Například microsoft.com, google.com.</span><span class="sxs-lookup"><span data-stu-id="a438f-166">For example, microsoft.com, google.com.</span></span>

* <span data-ttu-id="a438f-167">Jakémukoli prostředku, který je ve stejné virtuální síti Azure, pomocí __interní název DNS__ prostředku.</span><span class="sxs-lookup"><span data-stu-id="a438f-167">Any resource that is in the same Azure Virtual Network, by using the __internal DNS name__ of the resource.</span></span> <span data-ttu-id="a438f-168">Například pokud používáte překlad výchozí, následuje příklad interní DNS názvy přiřazené k pracovním uzlům HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a438f-168">For example, when using the default name resolution, the following are example internal DNS names assigned to HDInsight worker nodes:</span></span>

    * <span data-ttu-id="a438f-169">wn0 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="a438f-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>
    * <span data-ttu-id="a438f-170">wn2 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="a438f-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>

    <span data-ttu-id="a438f-171">Obě tyto uzly může komunikovat přímo s a jiné uzly v HDInsight pomocí interní názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-171">Both these nodes can communicate directly with each other, and other nodes in HDInsight, by using internal DNS names.</span></span>

<span data-ttu-id="a438f-172">Rozlišení názvů výchozí nemá __není__ povolit HDInsight překládat názvy prostředků v sítích, které jsou připojeny k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-172">The default name resolution does __not__ allow HDInsight to resolve the names of resources in networks that are joined to the virtual network.</span></span> <span data-ttu-id="a438f-173">Například je společné pro připojení k místní síti do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-173">For example, it is common to join your on-premises network to the virtual network.</span></span> <span data-ttu-id="a438f-174">S pouze výchozí název řešení HDInsight nemají přístup k prostředkům v místní síti podle názvu.</span><span class="sxs-lookup"><span data-stu-id="a438f-174">With only the default name resolution, HDInsight cannot access resources in the on-premises network by name.</span></span> <span data-ttu-id="a438f-175">Naopak je také nastavena hodnota true, prostředky ve vaší místní síti nemají přístup k prostředkům ve virtuální síti podle názvu.</span><span class="sxs-lookup"><span data-stu-id="a438f-175">The opposite is also true, resources in your on-premises network cannot access resources in the virtual network by name.</span></span>

> [!WARNING]
> <span data-ttu-id="a438f-176">Musíte vytvořit vlastního serveru DNS a konfigurovat virtuální sítě, abyste ho použít před vytvořením clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-176">You must create the custom DNS server and configure the virtual network to use it before creating the HDInsight cluster.</span></span>

<span data-ttu-id="a438f-177">Chcete-li povolit překlad mezi virtuální sítě a prostředky v připojené k sítím, musíte provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="a438f-177">To enable name resolution between the virtual network and resources in joined networks, you must perform the following actions:</span></span>

1. <span data-ttu-id="a438f-178">Vytvoření vlastního serveru DNS ve virtuální síti Azure, kam chcete nainstalovat HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-178">Create a custom DNS server in the Azure Virtual Network where you plan to install HDInsight.</span></span>

2. <span data-ttu-id="a438f-179">Konfigurace virtuální sítě pro použití vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-179">Configure the virtual network to use the custom DNS server.</span></span>

3. <span data-ttu-id="a438f-180">Najít že přiřazené přípona DNS pro vaši virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-180">Find the Azure assigned DNS suffix for your virtual network.</span></span> <span data-ttu-id="a438f-181">Tato hodnota je podobná `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="a438f-181">This value is similar to `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span></span> <span data-ttu-id="a438f-182">Informace o hledání přípon DNS, najdete v článku [příklad: vlastní DNS](#example-dns) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-182">For information on finding the DNS suffix, see the [Example: Custom DNS](#example-dns) section.</span></span>

4. <span data-ttu-id="a438f-183">Konfigurace předávání mezi servery DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-183">Configure forwarding between the DNS servers.</span></span> <span data-ttu-id="a438f-184">Konfigurace závisí na typu vzdálené sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-184">The configuration depends on the type of remote network.</span></span>

    * <span data-ttu-id="a438f-185">Pokud vzdálené sítě do místní sítě, nakonfigurujte DNS takto:</span><span class="sxs-lookup"><span data-stu-id="a438f-185">If the remote network is an on-premises network, configure DNS as follows:</span></span>
        
        * <span data-ttu-id="a438f-186">__Vlastní DNS__ (ve virtuální síti):</span><span class="sxs-lookup"><span data-stu-id="a438f-186">__Custom DNS__ (in the virtual network):</span></span>

            * <span data-ttu-id="a438f-187">Předat dál požadavků pro přípony DNS virtuální sítě do Azure rekurzivní překladač (168.63.129.16).</span><span class="sxs-lookup"><span data-stu-id="a438f-187">Forward requests for the DNS suffix of the virtual network to the Azure recursive resolver (168.63.129.16).</span></span> <span data-ttu-id="a438f-188">Zpracovává požadavky na prostředky ve virtuální síti Azure</span><span class="sxs-lookup"><span data-stu-id="a438f-188">Azure handles requests for resources in the virtual network</span></span>

            * <span data-ttu-id="a438f-189">Předávání všech ostatních požadavků na místní server DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-189">Forward all other requests to the on-premises DNS server.</span></span> <span data-ttu-id="a438f-190">Místní DNS zpracovává všechny ostatní požadavky na rozlišení názvů, i požadavky na internetové prostředky, jako je například Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="a438f-190">The on-premises DNS handles all other name resolution requests, even requests for internet resources such as Microsoft.com.</span></span>

        * <span data-ttu-id="a438f-191">__Místní DNS__: předávat požadavky pro příponu DNS virtuální sítě do vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-191">__On-premises DNS__: Forward requests for the virtual network DNS suffix to the custom DNS server.</span></span> <span data-ttu-id="a438f-192">Vlastního serveru DNS se potom předá do překladače Azure rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="a438f-192">The custom DNS server then forwards to the Azure recursive resolver.</span></span>

        <span data-ttu-id="a438f-193">Tato požadavky na konfiguraci tras pro plně kvalifikované názvy domény, které obsahují příponu DNS virtuální sítě do vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-193">This configuration routes requests for fully qualified domain names that contain the DNS suffix of the virtual network to the custom DNS server.</span></span> <span data-ttu-id="a438f-194">Zpracovává všechny požadavky (i pro veřejné internetové adresy) na místním serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-194">All other requests (even for public internet addresses) are handled by the on-premises DNS server.</span></span>

    * <span data-ttu-id="a438f-195">Pokud je vzdálené síť jinou virtuální sítí Azure, nakonfigurujte DNS následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a438f-195">If the remote network is another Azure Virtual Network, configure DNS as follows:</span></span>

        * <span data-ttu-id="a438f-196">__Vlastní DNS__ (v každé virtuální sítě):</span><span class="sxs-lookup"><span data-stu-id="a438f-196">__Custom DNS__ (in each virtual network):</span></span>

            * <span data-ttu-id="a438f-197">Požadavky pro příponu DNS virtuální sítě se předávají do vlastní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-197">Requests for the DNS suffix of the virtual networks are forwarded to the custom DNS servers.</span></span> <span data-ttu-id="a438f-198">Služba DNS v každé virtuální sítě je zodpovědná za řešení prostředky ve své síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-198">The DNS in each virtual network is responsible for resolving resources within its network.</span></span>

            * <span data-ttu-id="a438f-199">Předávání všech ostatních požadavků na Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="a438f-199">Forward all other requests to the Azure recursive resolver.</span></span> <span data-ttu-id="a438f-200">Rekurzivní překladač zodpovídá za řešení místní a prostředků z Internetu.</span><span class="sxs-lookup"><span data-stu-id="a438f-200">The recursive resolver is responsible for resolving local and internet resources.</span></span>

        <span data-ttu-id="a438f-201">DNS server pro každou síť předá požadavky na druhý, na základě přípony DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-201">The DNS server for each network forwards requests to the other, based on DNS suffix.</span></span> <span data-ttu-id="a438f-202">Ostatní požadavky jsou vyřešeny pomocí Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="a438f-202">Other requests are resolved using the Azure recursive resolver.</span></span>

    <span data-ttu-id="a438f-203">Příklad každé konfiguraci, naleznete v části [příklad: vlastní DNS](#example-dns) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-203">For an example of each configuration, see the [Example: Custom DNS](#example-dns) section.</span></span>

<span data-ttu-id="a438f-204">Další informace najdete v tématu [překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-204">For more information, see the [Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) document.</span></span>

## <a name="directly-connect-to-hadoop-services"></a><span data-ttu-id="a438f-205">Připojovat přímo k služby Hadoop</span><span class="sxs-lookup"><span data-stu-id="a438f-205">Directly connect to Hadoop services</span></span>

<span data-ttu-id="a438f-206">Většina dokumentace v HDInsight předpokládá, že máte přístup ke clusteru přes internet.</span><span class="sxs-lookup"><span data-stu-id="a438f-206">Most documentation on HDInsight assumes that you have access to the cluster over the internet.</span></span> <span data-ttu-id="a438f-207">Pro příklad, který můžete připojit ke clusteru v https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="a438f-207">For example, that you can connect to the cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="a438f-208">Tato adresa se používá veřejný brány, která není k dispozici, pokud jste použili skupiny Nsg nebo udr k omezení přístupu z Internetu.</span><span class="sxs-lookup"><span data-stu-id="a438f-208">This address uses the public gateway, which is not available if you have used NSGs or UDRs to restrict access from the internet.</span></span>

<span data-ttu-id="a438f-209">Pro připojení k Ambari a další webové stránky prostřednictvím virtuální sítě, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a438f-209">To connect to Ambari and other web pages through the virtual network, use the following steps:</span></span>

1. <span data-ttu-id="a438f-210">Pokud chcete zjistit, interní plně kvalifikované názvy domény (FQDN) uzlů clusteru HDInsight, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="a438f-210">To discover the internal fully qualified domain names (FQDN) of the HDInsight cluster nodes, use one of the following methods:</span></span>

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

    <span data-ttu-id="a438f-211">V seznamu uzlů vrátil najít plně kvalifikovaný název domény pro hlavních uzlech a použít plně kvalifikované názvy domény pro připojení k Ambari a dalších webových služeb.</span><span class="sxs-lookup"><span data-stu-id="a438f-211">In the list of nodes returned, find the FQDN for the head nodes and use the FQDNs to connect to Ambari and other web services.</span></span> <span data-ttu-id="a438f-212">Například použít `http://<headnode-fqdn>:8080` pro přístup k Ambari.</span><span class="sxs-lookup"><span data-stu-id="a438f-212">For example, use `http://<headnode-fqdn>:8080` to access Ambari.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a438f-213">Některé služby hostované o hlavních uzlech aktivní pouze na jednom uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="a438f-213">Some services hosted on the head nodes are only active on one node at a time.</span></span> <span data-ttu-id="a438f-214">Pokud se pokusíte přístup k službě jeden hlavního uzlu a vrátí chybu 404, přepněte do jiného hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="a438f-214">If you try accessing a service on one head node and it returns a 404 error, switch to the other head node.</span></span>

2. <span data-ttu-id="a438f-215">Zjistit uzel a port, který je k dispozici na služby, najdete v článku [porty používané služby Hadoop v HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-215">To determine the node and port that a service is available on, see the [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

## <span data-ttu-id="a438f-216"><a id="networktraffic"></a>Řízení síťového provozu</span><span class="sxs-lookup"><span data-stu-id="a438f-216"><a id="networktraffic"></a> Controlling network traffic</span></span>

<span data-ttu-id="a438f-217">Síťový provoz v Azure Virtual Network se dá řídit pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="a438f-217">Network traffic in an Azure Virtual Networks can be controlled using the following methods:</span></span>

* <span data-ttu-id="a438f-218">**Skupin zabezpečení sítě** (NSG) vám umožní filtrovat příchozí a odchozí přenosy v síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-218">**Network security groups** (NSG) allow you to filter inbound and outbound traffic to the network.</span></span> <span data-ttu-id="a438f-219">Další informace najdete v tématu [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-219">For more information, see the [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    > [!WARNING]
    > <span data-ttu-id="a438f-220">HDInsight nepodporuje omezení odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="a438f-220">HDInsight does not support restricting outbound traffic.</span></span>

* <span data-ttu-id="a438f-221">**Trasy definované uživatelem** (UDR) definovat tok přenosů mezi prostředky v síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-221">**User-defined routes** (UDR) define how traffic flows between resources in the network.</span></span> <span data-ttu-id="a438f-222">Další informace najdete v tématu [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-222">For more information, see the [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md) document.</span></span>

* <span data-ttu-id="a438f-223">**Síťových virtuálních zařízení** replikace funkce zařízení, jako jsou brány firewall a směrovače.</span><span class="sxs-lookup"><span data-stu-id="a438f-223">**Network virtual appliances** replicate the functionality of devices such as firewalls and routers.</span></span> <span data-ttu-id="a438f-224">Další informace najdete v tématu [síťových zařízení](https://azure.microsoft.com/solutions/network-appliances) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-224">For more information, see the [Network Appliances](https://azure.microsoft.com/solutions/network-appliances) document.</span></span>

<span data-ttu-id="a438f-225">Jako spravovanou službu vyžaduje HDInsight neomezený přístup ke službám Azure stavu a správu v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-225">As a managed service, HDInsight requires unrestricted access to Azure health and management services in the Azure cloud.</span></span> <span data-ttu-id="a438f-226">Pokud používáte skupiny Nsg a udr, je nutné zajistit, že HDInsight tyto služby můžete stále komunikovat s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-226">When using NSGs and UDRs, you must ensure that HDInsight these services can still communicate with HDInsight.</span></span>

<span data-ttu-id="a438f-227">HDInsight poskytuje služby na několika portech.</span><span class="sxs-lookup"><span data-stu-id="a438f-227">HDInsight exposes services on several ports.</span></span> <span data-ttu-id="a438f-228">Při použití brány firewall virtuální zařízení, musí umožňovat komunikaci na portech používaných pro tyto služby.</span><span class="sxs-lookup"><span data-stu-id="a438f-228">When using a virtual appliance firewall, you must allow traffic on the ports used for these services.</span></span> <span data-ttu-id="a438f-229">Další informace najdete v části [požadované porty].</span><span class="sxs-lookup"><span data-stu-id="a438f-229">For more information, see the [Required ports] section.</span></span>

### <span data-ttu-id="a438f-230"><a id="hdinsight-ip"></a>Prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="a438f-230"><a id="hdinsight-ip"></a> HDInsight with network security groups and user-defined routes</span></span>

<span data-ttu-id="a438f-231">Pokud máte v úmyslu používat **skupin zabezpečení sítě** nebo **trasy definované uživatelem** k řízení síťových přenosů, proveďte následující akce před instalací HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a438f-231">If you plan on using **network security groups** or **user-defined routes** to control network traffic, perform the following actions before installing HDInsight:</span></span>

1. <span data-ttu-id="a438f-232">Určete oblast Azure, který chcete použít pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-232">Identify the Azure region that you plan to use for HDInsight.</span></span>

2. <span data-ttu-id="a438f-233">Určete IP adresy, které vyžadují HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-233">Identify the IP addresses required by HDInsight.</span></span> <span data-ttu-id="a438f-234">Další informace najdete v tématu [IP adresy, které vyžadují HDInsight](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-234">For more information, see the [IP Addresses required by HDInsight](#hdinsight-ip) section.</span></span>

3. <span data-ttu-id="a438f-235">Vytvoření nebo úprava skupiny zabezpečení sítě nebo trasy definované uživatelem, které chcete nainstalovat HDInsight do podsítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-235">Create or modify the network security groups or user-defined routes for the subnet that you plan to install HDInsight into.</span></span>

    * <span data-ttu-id="a438f-236">__Skupin zabezpečení sítě__: Povolit __příchozí__ přenosy na portu __443__ z IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a438f-236">__Network security groups__: allow __inbound__ traffic on port __443__ from the IP addresses.</span></span>
    * <span data-ttu-id="a438f-237">__Trasy definované uživatelem__: vytvoření směrování pro každou IP adresu a nastavení __typ dalšího směrování__ k __Internet__.</span><span class="sxs-lookup"><span data-stu-id="a438f-237">__User-defined routes__: create a route to each IP address and set the __Next hop type__ to __Internet__.</span></span>

<span data-ttu-id="a438f-238">Další informace o skupinách zabezpečení sítě nebo trasy definované uživatelem naleznete v následující dokumentaci:</span><span class="sxs-lookup"><span data-stu-id="a438f-238">For more information on network security groups or user-defined routes, see the following documentation:</span></span>

* [<span data-ttu-id="a438f-239">Skupina zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="a438f-239">Network security group</span></span>](../virtual-network/virtual-networks-nsg.md)

* [<span data-ttu-id="a438f-240">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="a438f-240">User-defined routes</span></span>](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a><span data-ttu-id="a438f-241">Vynucené tunelování</span><span class="sxs-lookup"><span data-stu-id="a438f-241">Forced tunneling</span></span>

<span data-ttu-id="a438f-242">Vynucené tunelování je uživatelem definované konfigurace směrování, kde veškerý provoz z podsítě musí v určité síti nebo umístění, jako například do místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-242">Forced tunneling is a user-defined routing configuration where all traffic from a subnet is forced to a specific network or location, such as your on-premises network.</span></span> <span data-ttu-id="a438f-243">HDInsight nemá __není__ podporu vynuceného tunelování.</span><span class="sxs-lookup"><span data-stu-id="a438f-243">HDInsight does __not__ support forced tunneling.</span></span>

## <span data-ttu-id="a438f-244"><a id="hdinsight-ip"></a>Požadované IP adresy</span><span class="sxs-lookup"><span data-stu-id="a438f-244"><a id="hdinsight-ip"></a> Required IP addresses</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a438f-245">Stav a správu služeb Azure musí být schopen komunikovat s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-245">The Azure health and management services must be able to communicate with HDInsight.</span></span> <span data-ttu-id="a438f-246">Pokud používáte skupiny zabezpečení sítě nebo trasy definované uživatelem, povolí provoz z IP adres pro tyto služby k dosažení HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-246">If you use network security groups or user-defined routes, allow traffic from the IP addresses for these services to reach HDInsight.</span></span>
>
> <span data-ttu-id="a438f-247">Pokud nepoužijete skupin zabezpečení sítě nebo trasy definované uživatelem pro řízení provozu, můžete ignorovat v této části.</span><span class="sxs-lookup"><span data-stu-id="a438f-247">If you do not use network security groups or user-defined routes to control traffic, you can ignore this section.</span></span>

<span data-ttu-id="a438f-248">Pokud používáte skupiny zabezpečení sítě nebo trasy definované uživatelem, musíte povolit přenosy z Azure stavu a správu služeb k dosažení HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-248">If you use network security groups or user-defined routes, you must allow traffic from the Azure health and management services to reach HDInsight.</span></span> <span data-ttu-id="a438f-249">Použijte následující postup k vyhledání IP adresy, které musí být povoleno:</span><span class="sxs-lookup"><span data-stu-id="a438f-249">Use the following steps to find the IP addresses that must be allowed:</span></span>

1. <span data-ttu-id="a438f-250">Vždy musí povolit provoz z následujících IP adres:</span><span class="sxs-lookup"><span data-stu-id="a438f-250">You must always allow traffic from the following IP addresses:</span></span>

    | <span data-ttu-id="a438f-251">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a438f-251">IP address</span></span> | <span data-ttu-id="a438f-252">Povolené portu</span><span class="sxs-lookup"><span data-stu-id="a438f-252">Allowed port</span></span> | <span data-ttu-id="a438f-253">Směr</span><span class="sxs-lookup"><span data-stu-id="a438f-253">Direction</span></span> |
    | ---- | ----- | ----- |
    | <span data-ttu-id="a438f-254">168.61.49.99</span><span class="sxs-lookup"><span data-stu-id="a438f-254">168.61.49.99</span></span> | <span data-ttu-id="a438f-255">443</span><span class="sxs-lookup"><span data-stu-id="a438f-255">443</span></span> | <span data-ttu-id="a438f-256">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-256">Inbound</span></span> |
    | <span data-ttu-id="a438f-257">23.99.5.239</span><span class="sxs-lookup"><span data-stu-id="a438f-257">23.99.5.239</span></span> | <span data-ttu-id="a438f-258">443</span><span class="sxs-lookup"><span data-stu-id="a438f-258">443</span></span> | <span data-ttu-id="a438f-259">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-259">Inbound</span></span> |
    | <span data-ttu-id="a438f-260">168.61.48.131</span><span class="sxs-lookup"><span data-stu-id="a438f-260">168.61.48.131</span></span> | <span data-ttu-id="a438f-261">443</span><span class="sxs-lookup"><span data-stu-id="a438f-261">443</span></span> | <span data-ttu-id="a438f-262">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-262">Inbound</span></span> |
    | <span data-ttu-id="a438f-263">138.91.141.162</span><span class="sxs-lookup"><span data-stu-id="a438f-263">138.91.141.162</span></span> | <span data-ttu-id="a438f-264">443</span><span class="sxs-lookup"><span data-stu-id="a438f-264">443</span></span> | <span data-ttu-id="a438f-265">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-265">Inbound</span></span> |

2. <span data-ttu-id="a438f-266">Pokud váš cluster HDInsight v jednom z následujících oblastí, musíte povolit přenosy z IP adresy uvedené pro oblast:</span><span class="sxs-lookup"><span data-stu-id="a438f-266">If your HDInsight cluster is in one of the following regions, then you must allow traffic from the IP addresses listed for the region:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a438f-267">Pokud není uvedené oblast Azure, kterou používáte, pak použijte pouze čtyři IP adresy z kroku 1.</span><span class="sxs-lookup"><span data-stu-id="a438f-267">If the Azure region you are using is not listed, then only use the four IP addresses from step 1.</span></span>

    | <span data-ttu-id="a438f-268">Země</span><span class="sxs-lookup"><span data-stu-id="a438f-268">Country</span></span> | <span data-ttu-id="a438f-269">Oblast</span><span class="sxs-lookup"><span data-stu-id="a438f-269">Region</span></span> | <span data-ttu-id="a438f-270">Povolené IP adresy</span><span class="sxs-lookup"><span data-stu-id="a438f-270">Allowed IP addresses</span></span> | <span data-ttu-id="a438f-271">Povolené portu</span><span class="sxs-lookup"><span data-stu-id="a438f-271">Allowed port</span></span> | <span data-ttu-id="a438f-272">Směr</span><span class="sxs-lookup"><span data-stu-id="a438f-272">Direction</span></span> |
    | ---- | ---- | ---- | ---- | ----- |
    | <span data-ttu-id="a438f-273">Asie</span><span class="sxs-lookup"><span data-stu-id="a438f-273">Asia</span></span> | <span data-ttu-id="a438f-274">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="a438f-274">East Asia</span></span> | <span data-ttu-id="a438f-275">23.102.235.122</span><span class="sxs-lookup"><span data-stu-id="a438f-275">23.102.235.122</span></span></br><span data-ttu-id="a438f-276">52.175.38.134</span><span class="sxs-lookup"><span data-stu-id="a438f-276">52.175.38.134</span></span> | <span data-ttu-id="a438f-277">443</span><span class="sxs-lookup"><span data-stu-id="a438f-277">443</span></span> | <span data-ttu-id="a438f-278">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-278">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-279">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="a438f-279">Southeast Asia</span></span> | <span data-ttu-id="a438f-280">13.76.245.160</span><span class="sxs-lookup"><span data-stu-id="a438f-280">13.76.245.160</span></span></br><span data-ttu-id="a438f-281">13.76.136.249</span><span class="sxs-lookup"><span data-stu-id="a438f-281">13.76.136.249</span></span> | <span data-ttu-id="a438f-282">443</span><span class="sxs-lookup"><span data-stu-id="a438f-282">443</span></span> | <span data-ttu-id="a438f-283">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-283">Inbound</span></span> |
    | <span data-ttu-id="a438f-284">Austrálie</span><span class="sxs-lookup"><span data-stu-id="a438f-284">Australia</span></span> | <span data-ttu-id="a438f-285">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="a438f-285">Australia East</span></span> | <span data-ttu-id="a438f-286">104.210.84.115</span><span class="sxs-lookup"><span data-stu-id="a438f-286">104.210.84.115</span></span></br><span data-ttu-id="a438f-287">13.75.152.195</span><span class="sxs-lookup"><span data-stu-id="a438f-287">13.75.152.195</span></span> | <span data-ttu-id="a438f-288">443</span><span class="sxs-lookup"><span data-stu-id="a438f-288">443</span></span> | <span data-ttu-id="a438f-289">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-289">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-290">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="a438f-290">Australia Southeast</span></span> | <span data-ttu-id="a438f-291">13.77.2.56</span><span class="sxs-lookup"><span data-stu-id="a438f-291">13.77.2.56</span></span></br><span data-ttu-id="a438f-292">13.77.2.94</span><span class="sxs-lookup"><span data-stu-id="a438f-292">13.77.2.94</span></span> | <span data-ttu-id="a438f-293">443</span><span class="sxs-lookup"><span data-stu-id="a438f-293">443</span></span> | <span data-ttu-id="a438f-294">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-294">Inbound</span></span> |
    | <span data-ttu-id="a438f-295">Brazílie</span><span class="sxs-lookup"><span data-stu-id="a438f-295">Brazil</span></span> | <span data-ttu-id="a438f-296">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="a438f-296">Brazil South</span></span> | <span data-ttu-id="a438f-297">191.235.84.104</span><span class="sxs-lookup"><span data-stu-id="a438f-297">191.235.84.104</span></span></br><span data-ttu-id="a438f-298">191.235.87.113</span><span class="sxs-lookup"><span data-stu-id="a438f-298">191.235.87.113</span></span> | <span data-ttu-id="a438f-299">443</span><span class="sxs-lookup"><span data-stu-id="a438f-299">443</span></span> | <span data-ttu-id="a438f-300">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-300">Inbound</span></span> |
    | <span data-ttu-id="a438f-301">Kanada</span><span class="sxs-lookup"><span data-stu-id="a438f-301">Canada</span></span> | <span data-ttu-id="a438f-302">Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="a438f-302">Canada East</span></span> | <span data-ttu-id="a438f-303">52.229.127.96</span><span class="sxs-lookup"><span data-stu-id="a438f-303">52.229.127.96</span></span></br><span data-ttu-id="a438f-304">52.229.123.172</span><span class="sxs-lookup"><span data-stu-id="a438f-304">52.229.123.172</span></span> | <span data-ttu-id="a438f-305">443</span><span class="sxs-lookup"><span data-stu-id="a438f-305">443</span></span> | <span data-ttu-id="a438f-306">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-306">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-307">Střední Kanada</span><span class="sxs-lookup"><span data-stu-id="a438f-307">Canada Central</span></span> | <span data-ttu-id="a438f-308">52.228.37.66</span><span class="sxs-lookup"><span data-stu-id="a438f-308">52.228.37.66</span></span></br><span data-ttu-id="a438f-309">52.228.45.222</span><span class="sxs-lookup"><span data-stu-id="a438f-309">52.228.45.222</span></span> | <span data-ttu-id="a438f-310">443</span><span class="sxs-lookup"><span data-stu-id="a438f-310">443</span></span> | <span data-ttu-id="a438f-311">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-311">Inbound</span></span> |
    | <span data-ttu-id="a438f-312">Čína</span><span class="sxs-lookup"><span data-stu-id="a438f-312">China</span></span> | <span data-ttu-id="a438f-313">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="a438f-313">China North</span></span> | <span data-ttu-id="a438f-314">42.159.96.170</span><span class="sxs-lookup"><span data-stu-id="a438f-314">42.159.96.170</span></span></br><span data-ttu-id="a438f-315">139.217.2.219</span><span class="sxs-lookup"><span data-stu-id="a438f-315">139.217.2.219</span></span> | <span data-ttu-id="a438f-316">443</span><span class="sxs-lookup"><span data-stu-id="a438f-316">443</span></span> | <span data-ttu-id="a438f-317">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-317">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-318">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="a438f-318">China East</span></span> | <span data-ttu-id="a438f-319">42.159.198.178</span><span class="sxs-lookup"><span data-stu-id="a438f-319">42.159.198.178</span></span></br><span data-ttu-id="a438f-320">42.159.234.157</span><span class="sxs-lookup"><span data-stu-id="a438f-320">42.159.234.157</span></span> | <span data-ttu-id="a438f-321">443</span><span class="sxs-lookup"><span data-stu-id="a438f-321">443</span></span> | <span data-ttu-id="a438f-322">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-322">Inbound</span></span> |
    | <span data-ttu-id="a438f-323">Evropa</span><span class="sxs-lookup"><span data-stu-id="a438f-323">Europe</span></span> | <span data-ttu-id="a438f-324">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="a438f-324">North Europe</span></span> | <span data-ttu-id="a438f-325">52.164.210.96</span><span class="sxs-lookup"><span data-stu-id="a438f-325">52.164.210.96</span></span></br><span data-ttu-id="a438f-326">13.74.153.132</span><span class="sxs-lookup"><span data-stu-id="a438f-326">13.74.153.132</span></span> | <span data-ttu-id="a438f-327">443</span><span class="sxs-lookup"><span data-stu-id="a438f-327">443</span></span> | <span data-ttu-id="a438f-328">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-328">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-329">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="a438f-329">West Europe</span></span>| <span data-ttu-id="a438f-330">52.166.243.90</span><span class="sxs-lookup"><span data-stu-id="a438f-330">52.166.243.90</span></span></br><span data-ttu-id="a438f-331">52.174.36.244</span><span class="sxs-lookup"><span data-stu-id="a438f-331">52.174.36.244</span></span> | <span data-ttu-id="a438f-332">443</span><span class="sxs-lookup"><span data-stu-id="a438f-332">443</span></span> | <span data-ttu-id="a438f-333">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-333">Inbound</span></span> |
    | <span data-ttu-id="a438f-334">Německo</span><span class="sxs-lookup"><span data-stu-id="a438f-334">Germany</span></span> | <span data-ttu-id="a438f-335">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="a438f-335">Germany Central</span></span> | <span data-ttu-id="a438f-336">51.4.146.68</span><span class="sxs-lookup"><span data-stu-id="a438f-336">51.4.146.68</span></span></br><span data-ttu-id="a438f-337">51.4.146.80</span><span class="sxs-lookup"><span data-stu-id="a438f-337">51.4.146.80</span></span> | <span data-ttu-id="a438f-338">443</span><span class="sxs-lookup"><span data-stu-id="a438f-338">443</span></span> | <span data-ttu-id="a438f-339">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-339">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-340">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="a438f-340">Germany Northeast</span></span> | <span data-ttu-id="a438f-341">51.5.150.132</span><span class="sxs-lookup"><span data-stu-id="a438f-341">51.5.150.132</span></span></br><span data-ttu-id="a438f-342">51.5.144.101</span><span class="sxs-lookup"><span data-stu-id="a438f-342">51.5.144.101</span></span> | <span data-ttu-id="a438f-343">443</span><span class="sxs-lookup"><span data-stu-id="a438f-343">443</span></span> | <span data-ttu-id="a438f-344">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-344">Inbound</span></span> |
    | <span data-ttu-id="a438f-345">Indie</span><span class="sxs-lookup"><span data-stu-id="a438f-345">India</span></span> | <span data-ttu-id="a438f-346">Střed Indie</span><span class="sxs-lookup"><span data-stu-id="a438f-346">Central India</span></span> | <span data-ttu-id="a438f-347">52.172.153.209</span><span class="sxs-lookup"><span data-stu-id="a438f-347">52.172.153.209</span></span></br><span data-ttu-id="a438f-348">52.172.152.49</span><span class="sxs-lookup"><span data-stu-id="a438f-348">52.172.152.49</span></span> | <span data-ttu-id="a438f-349">443</span><span class="sxs-lookup"><span data-stu-id="a438f-349">443</span></span> | <span data-ttu-id="a438f-350">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-350">Inbound</span></span> |
    | <span data-ttu-id="a438f-351">Japonsko</span><span class="sxs-lookup"><span data-stu-id="a438f-351">Japan</span></span> | <span data-ttu-id="a438f-352">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="a438f-352">Japan East</span></span> | <span data-ttu-id="a438f-353">13.78.125.90</span><span class="sxs-lookup"><span data-stu-id="a438f-353">13.78.125.90</span></span></br><span data-ttu-id="a438f-354">13.78.89.60</span><span class="sxs-lookup"><span data-stu-id="a438f-354">13.78.89.60</span></span> | <span data-ttu-id="a438f-355">443</span><span class="sxs-lookup"><span data-stu-id="a438f-355">443</span></span> | <span data-ttu-id="a438f-356">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-356">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-357">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="a438f-357">Japan West</span></span> | <span data-ttu-id="a438f-358">40.74.125.69</span><span class="sxs-lookup"><span data-stu-id="a438f-358">40.74.125.69</span></span></br><span data-ttu-id="a438f-359">138.91.29.150</span><span class="sxs-lookup"><span data-stu-id="a438f-359">138.91.29.150</span></span> | <span data-ttu-id="a438f-360">443</span><span class="sxs-lookup"><span data-stu-id="a438f-360">443</span></span> | <span data-ttu-id="a438f-361">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-361">Inbound</span></span> |
    | <span data-ttu-id="a438f-362">Korea</span><span class="sxs-lookup"><span data-stu-id="a438f-362">Korea</span></span> | <span data-ttu-id="a438f-363">Korea – střed</span><span class="sxs-lookup"><span data-stu-id="a438f-363">Korea Central</span></span> | <span data-ttu-id="a438f-364">52.231.39.142</span><span class="sxs-lookup"><span data-stu-id="a438f-364">52.231.39.142</span></span></br><span data-ttu-id="a438f-365">52.231.36.209</span><span class="sxs-lookup"><span data-stu-id="a438f-365">52.231.36.209</span></span> | <span data-ttu-id="a438f-366">433</span><span class="sxs-lookup"><span data-stu-id="a438f-366">433</span></span> | <span data-ttu-id="a438f-367">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-367">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-368">Korea – jih</span><span class="sxs-lookup"><span data-stu-id="a438f-368">Korea South</span></span> | <span data-ttu-id="a438f-369">52.231.203.16</span><span class="sxs-lookup"><span data-stu-id="a438f-369">52.231.203.16</span></span></br><span data-ttu-id="a438f-370">52.231.205.214</span><span class="sxs-lookup"><span data-stu-id="a438f-370">52.231.205.214</span></span> | <span data-ttu-id="a438f-371">443</span><span class="sxs-lookup"><span data-stu-id="a438f-371">443</span></span> | <span data-ttu-id="a438f-372">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-372">Inbound</span></span>
    | <span data-ttu-id="a438f-373">Spojené království</span><span class="sxs-lookup"><span data-stu-id="a438f-373">United Kingdom</span></span> | <span data-ttu-id="a438f-374">Spojené království – západ</span><span class="sxs-lookup"><span data-stu-id="a438f-374">UK West</span></span> | <span data-ttu-id="a438f-375">51.141.13.110</span><span class="sxs-lookup"><span data-stu-id="a438f-375">51.141.13.110</span></span></br><span data-ttu-id="a438f-376">51.141.7.20</span><span class="sxs-lookup"><span data-stu-id="a438f-376">51.141.7.20</span></span> | <span data-ttu-id="a438f-377">443</span><span class="sxs-lookup"><span data-stu-id="a438f-377">443</span></span> | <span data-ttu-id="a438f-378">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-378">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-379">Spojené království – jih</span><span class="sxs-lookup"><span data-stu-id="a438f-379">UK South</span></span> | <span data-ttu-id="a438f-380">51.140.47.39</span><span class="sxs-lookup"><span data-stu-id="a438f-380">51.140.47.39</span></span></br><span data-ttu-id="a438f-381">51.140.52.16</span><span class="sxs-lookup"><span data-stu-id="a438f-381">51.140.52.16</span></span> | <span data-ttu-id="a438f-382">443</span><span class="sxs-lookup"><span data-stu-id="a438f-382">443</span></span> | <span data-ttu-id="a438f-383">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-383">Inbound</span></span> |
    | <span data-ttu-id="a438f-384">Spojené státy</span><span class="sxs-lookup"><span data-stu-id="a438f-384">United States</span></span> | <span data-ttu-id="a438f-385">Střed USA</span><span class="sxs-lookup"><span data-stu-id="a438f-385">Central US</span></span> | <span data-ttu-id="a438f-386">13.67.223.215</span><span class="sxs-lookup"><span data-stu-id="a438f-386">13.67.223.215</span></span></br><span data-ttu-id="a438f-387">40.86.83.253</span><span class="sxs-lookup"><span data-stu-id="a438f-387">40.86.83.253</span></span> | <span data-ttu-id="a438f-388">443</span><span class="sxs-lookup"><span data-stu-id="a438f-388">443</span></span> | <span data-ttu-id="a438f-389">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-389">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-390">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="a438f-390">North Central US</span></span> | <span data-ttu-id="a438f-391">157.56.8.38</span><span class="sxs-lookup"><span data-stu-id="a438f-391">157.56.8.38</span></span></br><span data-ttu-id="a438f-392">157.55.213.99</span><span class="sxs-lookup"><span data-stu-id="a438f-392">157.55.213.99</span></span> | <span data-ttu-id="a438f-393">443</span><span class="sxs-lookup"><span data-stu-id="a438f-393">443</span></span> | <span data-ttu-id="a438f-394">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-394">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-395">Západní střed USA</span><span class="sxs-lookup"><span data-stu-id="a438f-395">West Central US</span></span> | <span data-ttu-id="a438f-396">52.161.23.15</span><span class="sxs-lookup"><span data-stu-id="a438f-396">52.161.23.15</span></span></br><span data-ttu-id="a438f-397">52.161.10.167</span><span class="sxs-lookup"><span data-stu-id="a438f-397">52.161.10.167</span></span> | <span data-ttu-id="a438f-398">443</span><span class="sxs-lookup"><span data-stu-id="a438f-398">443</span></span> | <span data-ttu-id="a438f-399">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-399">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="a438f-400">Západní USA 2</span><span class="sxs-lookup"><span data-stu-id="a438f-400">West US 2</span></span> | <span data-ttu-id="a438f-401">52.175.211.210</span><span class="sxs-lookup"><span data-stu-id="a438f-401">52.175.211.210</span></span></br><span data-ttu-id="a438f-402">52.175.222.222</span><span class="sxs-lookup"><span data-stu-id="a438f-402">52.175.222.222</span></span> | <span data-ttu-id="a438f-403">443</span><span class="sxs-lookup"><span data-stu-id="a438f-403">443</span></span> | <span data-ttu-id="a438f-404">Příchozí</span><span class="sxs-lookup"><span data-stu-id="a438f-404">Inbound</span></span> |

    <span data-ttu-id="a438f-405">Informace o IP adresy pro Azure Government, najdete v článku [Azure Government Intelligence + analýzy](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-405">For information on the IP addresses to use for Azure Government, see the [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.</span></span>

3. <span data-ttu-id="a438f-406">Pokud používáte vlastního serveru DNS s vaší virtuální síti, musíte také povolit přístup z __168.63.129.16__.</span><span class="sxs-lookup"><span data-stu-id="a438f-406">If you use a custom DNS server with your virtual network, you must also allow access from __168.63.129.16__.</span></span> <span data-ttu-id="a438f-407">Tato adresa se Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="a438f-407">This address is Azure's recursive resolver.</span></span> <span data-ttu-id="a438f-408">Další informace najdete v tématu [překlad názvů pro virtuální počítače a Role instance](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-408">For more information, see the [Name resolution for VMs and Role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.</span></span>

<span data-ttu-id="a438f-409">Další informace najdete v tématu [řízení síťového provozu](#networktraffic) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-409">For more information, see the [Controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="a438f-410"><a id="hdinsight-ports"></a>Požadované porty</span><span class="sxs-lookup"><span data-stu-id="a438f-410"><a id="hdinsight-ports"></a> Required ports</span></span>

<span data-ttu-id="a438f-411">Pokud máte v úmyslu používat síť **virtuální zařízení brány firewall** zabezpečit virtuální síť, musíte povolit odchozí přenosy na následující porty:</span><span class="sxs-lookup"><span data-stu-id="a438f-411">If you plan on using a network **virtual appliance firewall** to secure the virtual network, you must allow outbound traffic on the following ports:</span></span>

* <span data-ttu-id="a438f-412">53</span><span class="sxs-lookup"><span data-stu-id="a438f-412">53</span></span>
* <span data-ttu-id="a438f-413">443</span><span class="sxs-lookup"><span data-stu-id="a438f-413">443</span></span>
* <span data-ttu-id="a438f-414">1433</span><span class="sxs-lookup"><span data-stu-id="a438f-414">1433</span></span>
* <span data-ttu-id="a438f-415">11000-11999</span><span class="sxs-lookup"><span data-stu-id="a438f-415">11000-11999</span></span>
* <span data-ttu-id="a438f-416">14000-14999</span><span class="sxs-lookup"><span data-stu-id="a438f-416">14000-14999</span></span>

<span data-ttu-id="a438f-417">Seznam portů pro určité služby, najdete v článku [porty používané služby Hadoop v HDInsight](hdinsight-hadoop-port-settings-for-services.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-417">For a list of ports for specific services, see the [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

<span data-ttu-id="a438f-418">Další informace o pravidlech brány firewall pro virtuální zařízení, najdete v článku [virtuální zařízení scénář](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a438f-418">For more information on firewall rules for virtual appliances, see the [virtual appliance scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.</span></span>

## <span data-ttu-id="a438f-419"><a id="hdinsight-nsg"></a>Příklad: skupin zabezpečení sítě s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a438f-419"><a id="hdinsight-nsg"></a>Example: network security groups with HDInsight</span></span>

<span data-ttu-id="a438f-420">Příklady v této části ukazují, jak vytvořit skupiny pravidla, která umožňují HDInsight ke komunikaci se službami Azure správu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-420">The examples in this section demonstrate how to create network security group rules that allow HDInsight to communicate with the Azure management services.</span></span> <span data-ttu-id="a438f-421">Před použitím v příkladech, upravte IP adresy tak, aby odpovídala těm, které jsou pro oblast Azure, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="a438f-421">Before using the examples, adjust the IP addresses to match the ones for the Azure region you are using.</span></span> <span data-ttu-id="a438f-422">Tyto informace můžete najít [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-422">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-resource-management-template"></a><span data-ttu-id="a438f-423">Šablony Azure Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="a438f-423">Azure Resource Management template</span></span>

<span data-ttu-id="a438f-424">Následující šablony správy prostředků vytvoří virtuální síť, která omezuje příchozí provoz, ale povoluje provoz z IP adresy, které vyžadují HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-424">The following Resource Management template creates a virtual network that restricts inbound traffic, but allows traffic from the IP addresses required by HDInsight.</span></span> <span data-ttu-id="a438f-425">Tato šablona vytvoří HDInsight cluster také ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-425">This template also creates an HDInsight cluster in the virtual network.</span></span>

* [<span data-ttu-id="a438f-426">Nasazení zabezpečených virtuální síť Azure a cluster systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="a438f-426">Deploy a secured Azure Virtual Network and an HDInsight Hadoop cluster</span></span>](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> <span data-ttu-id="a438f-427">Změna IP adresy použité v tomto příkladu tak, aby odpovídaly oblasti Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="a438f-427">Change the IP addresses used in this example to match the Azure region you are using.</span></span> <span data-ttu-id="a438f-428">Tyto informace můžete najít [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-428">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="a438f-429">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a438f-429">Azure PowerShell</span></span>

<span data-ttu-id="a438f-430">Pomocí následujícího skriptu prostředí PowerShell k vytvoření virtuální sítě, která omezuje příchozí provoz a umožňuje přenos z IP adres pro oblast Severní Evropa.</span><span class="sxs-lookup"><span data-stu-id="a438f-430">Use the following PowerShell script to create a virtual network that restricts inbound traffic and allows traffic from the IP addresses for the North Europe region.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a438f-431">Změna IP adresy použité v tomto příkladu tak, aby odpovídaly oblasti Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="a438f-431">Change the IP addresses used in this example to match the Azure region you are using.</span></span> <span data-ttu-id="a438f-432">Tyto informace můžete najít [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-432">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set the changes to the security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> <span data-ttu-id="a438f-433">Tento příklad ukazuje, jak přidat pravidla, která povolí příchozí komunikaci na požadované IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a438f-433">This example demonstrates how to add rules to allow inbound traffic on the required IP addresses.</span></span> <span data-ttu-id="a438f-434">Neobsahuje na pravidlo můžete omezit příchozí přístup z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="a438f-434">It does not contain a rule to restrict inbound access from other sources.</span></span>
>
> <span data-ttu-id="a438f-435">Následující příklad ukazuje, jak povolit přístup SSH z Internetu:</span><span class="sxs-lookup"><span data-stu-id="a438f-435">The following example demonstrates how to enable SSH access from the Internet:</span></span>
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a><span data-ttu-id="a438f-436">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a438f-436">Azure CLI</span></span>

<span data-ttu-id="a438f-437">Pomocí následujících kroků vytvořte virtuální síť, která omezuje příchozí provoz, ale povoluje provoz z IP adresy, které vyžadují HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a438f-437">Use the following steps to create a virtual network that restricts inbound traffic, but allows traffic from the IP addresses required by HDInsight.</span></span>

1. <span data-ttu-id="a438f-438">Použijte následující příkaz k vytvoření nové skupiny zabezpečení sítě s názvem `hdisecure`.</span><span class="sxs-lookup"><span data-stu-id="a438f-438">Use the following command to create a new network security group named `hdisecure`.</span></span> <span data-ttu-id="a438f-439">Nahraďte **RESOURCEGROUPNAME** ke skupině prostředků, který obsahuje virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-439">Replace **RESOURCEGROUPNAME** with the resource group that contains the Azure Virtual Network.</span></span> <span data-ttu-id="a438f-440">Nahraďte **umístění** s umístěním (oblastí), která byla skupina vytvořena v.</span><span class="sxs-lookup"><span data-stu-id="a438f-440">Replace **LOCATION** with the location (region) that the group was created in.</span></span>

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    <span data-ttu-id="a438f-441">Po vytvoření skupiny, zobrazí informace o nové skupiny.</span><span class="sxs-lookup"><span data-stu-id="a438f-441">Once the group has been created, you receive information on the new group.</span></span>

2. <span data-ttu-id="a438f-442">Následující informace vám pomůžou přidat pravidla pro novou skupinu zabezpečení sítě, které umožní příchozí komunikaci na portu 443 ze služby Azure HDInsight stavu a správu.</span><span class="sxs-lookup"><span data-stu-id="a438f-442">Use the following to add rules to the new network security group that allow inbound communication on port 443 from the Azure HDInsight health and management service.</span></span> <span data-ttu-id="a438f-443">Nahraďte **RESOURCEGROUPNAME** s názvem skupiny prostředků, který obsahuje virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-443">Replace **RESOURCEGROUPNAME** with the name of the resource group that contains the Azure Virtual Network.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a438f-444">Změna IP adresy použité v tomto příkladu tak, aby odpovídaly oblasti Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="a438f-444">Change the IP addresses used in this example to match the Azure region you are using.</span></span> <span data-ttu-id="a438f-445">Tyto informace můžete najít [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="a438f-445">You can find this information in the [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. <span data-ttu-id="a438f-446">Pro načtení jedinečný identifikátor pro tuto skupinu zabezpečení sítě, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a438f-446">To retrieve the unique identifier for this network security group, use the following command:</span></span>

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    <span data-ttu-id="a438f-447">Tento příkaz vrátí hodnotu podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="a438f-447">This command returns a value similar to the following text:</span></span>

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    <span data-ttu-id="a438f-448">Pokud nejste očekávané výsledky, použijte dvojité uvozovky kolem id v příkazu.</span><span class="sxs-lookup"><span data-stu-id="a438f-448">Use double-quotes around id in the command if you don't get the expected results.</span></span>

4. <span data-ttu-id="a438f-449">Chcete-li použít skupinu zabezpečení sítě k podsíti, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="a438f-449">Use the following command to apply the network security group to a subnet.</span></span> <span data-ttu-id="a438f-450">Nahraďte __GUID__ a __RESOURCEGROUPNAME__ hodnoty s těmi vrácené z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="a438f-450">Replace the __GUID__ and __RESOURCEGROUPNAME__ values with the ones returned from the previous step.</span></span> <span data-ttu-id="a438f-451">Nahraďte __VNETNAME__ a __SUBNETNAME__ s název virtuální sítě a název podsítě, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a438f-451">Replace __VNETNAME__ and __SUBNETNAME__ with the virtual network name and subnet name that you want to create.</span></span>

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    <span data-ttu-id="a438f-452">Po dokončení tohoto příkazu můžete nainstalovat HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-452">Once this command completes, you can install HDInsight into the Virtual Network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a438f-453">Tyto kroky otevíraly jenom přístup k službě HDInsight stavu a správu v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="a438f-453">These steps only open access to the HDInsight health and management service on the Azure cloud.</span></span> <span data-ttu-id="a438f-454">Jiný přístup ke clusteru HDInsight z mimo virtuální síť je blokována.</span><span class="sxs-lookup"><span data-stu-id="a438f-454">Any other access to the HDInsight cluster from outside the Virtual Network is blocked.</span></span> <span data-ttu-id="a438f-455">Pokud chcete povolit přístup mimo virtuální síť, musíte přidat další pravidla skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-455">To enable access from outside the virtual network, you must add additional Network Security Group rules.</span></span>
>
> <span data-ttu-id="a438f-456">Následující příklad ukazuje, jak povolit přístup SSH z Internetu:</span><span class="sxs-lookup"><span data-stu-id="a438f-456">The following example demonstrates how to enable SSH access from the Internet:</span></span>
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <span data-ttu-id="a438f-457"><a id="example-dns"></a>Příklad: Konfigurace DNS</span><span class="sxs-lookup"><span data-stu-id="a438f-457"><a id="example-dns"></a> Example: DNS configuration</span></span>

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a><span data-ttu-id="a438f-458">Překlad mezi virtuální sítí a připojené místní sítě</span><span class="sxs-lookup"><span data-stu-id="a438f-458">Name resolution between a virtual network and a connected on-premises network</span></span>

<span data-ttu-id="a438f-459">Tento příklad vytvoří následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="a438f-459">This example makes the following assumptions:</span></span>

* <span data-ttu-id="a438f-460">Máte virtuální síť Azure, která je připojena k místní síti pomocí brány VPN.</span><span class="sxs-lookup"><span data-stu-id="a438f-460">You have an Azure Virtual Network that is connected to an on-premises network using a VPN gateway.</span></span>

* <span data-ttu-id="a438f-461">Vlastního serveru DNS ve virtuální síti běží jako operační systém Linux nebo Unix.</span><span class="sxs-lookup"><span data-stu-id="a438f-461">The custom DNS server in the virtual network is running Linux or Unix as the operating system.</span></span>

* <span data-ttu-id="a438f-462">[Vytvoření vazby](https://www.isc.org/downloads/bind/) je nainstalován na vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-462">[Bind](https://www.isc.org/downloads/bind/) is installed on the custom DNS server.</span></span>

<span data-ttu-id="a438f-463">Na vlastního serveru DNS ve virtuální síti:</span><span class="sxs-lookup"><span data-stu-id="a438f-463">On the custom DNS server in the virtual network:</span></span>

1. <span data-ttu-id="a438f-464">Použití Azure PowerShell nebo rozhraní příkazového řádku Azure k nalezení přípona DNS virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="a438f-464">Use either Azure PowerShell or Azure CLI to find the DNS suffix of the virtual network:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="a438f-465">Na vlastního serveru DNS pro virtuální síť, použijte následující text jako obsah `/etc/bind/named.conf.local` souboru:</span><span class="sxs-lookup"><span data-stu-id="a438f-465">On the custom DNS server for the virtual network, use the following text as the contents of the `/etc/bind/named.conf.local` file:</span></span>

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    <span data-ttu-id="a438f-466">Nahraďte `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hodnotu s příponou DNS virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-466">Replace the `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with the DNS suffix of your virtual network.</span></span>

    <span data-ttu-id="a438f-467">Tato konfigurace směrovat všechny požadavky na DNS pro příponu DNS virtuální sítě na Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="a438f-467">This configuration routes all DNS requests for the DNS suffix of the virtual network to the Azure recursive resolver.</span></span>

2. <span data-ttu-id="a438f-468">Na vlastního serveru DNS pro virtuální síť, použijte následující text jako obsah `/etc/bind/named.conf.options` souboru:</span><span class="sxs-lookup"><span data-stu-id="a438f-468">On the custom DNS server for the virtual network, use the following text as the contents of the `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="a438f-469">Nahraďte `10.0.0.0/16` hodnotu s rozsah IP adres vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-469">Replace the `10.0.0.0/16` value with the IP address range of your virtual network.</span></span> <span data-ttu-id="a438f-470">Tato položka umožňuje název řešení požadavků adresy v tomto rozsahu.</span><span class="sxs-lookup"><span data-stu-id="a438f-470">This entry allows name resolution requests addresses within this range.</span></span>

    * <span data-ttu-id="a438f-471">Přidat rozsah IP adres místní sítě, aby `acl goodclients { ... }` oddílu.</span><span class="sxs-lookup"><span data-stu-id="a438f-471">Add the IP address range of the on-premises network to the `acl goodclients { ... }` section.</span></span>  <span data-ttu-id="a438f-472">položka umožňuje požadavky na rozlišení názvů z prostředků v místní síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-472">entry allows name resolution requests from resources in the on-premises network.</span></span>
    
    * <span data-ttu-id="a438f-473">Nahraďte hodnotu `192.168.0.1` s IP adresou serveru DNS na místě.</span><span class="sxs-lookup"><span data-stu-id="a438f-473">Replace the value `192.168.0.1` with the IP address of your on-premises DNS server.</span></span> <span data-ttu-id="a438f-474">Tato položka směrování všech ostatních požadavků na DNS na místní server DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-474">This entry routes all other DNS requests to the on-premises DNS server.</span></span>

3. <span data-ttu-id="a438f-475">Pokud chcete používat konfiguraci, restartujte vazby.</span><span class="sxs-lookup"><span data-stu-id="a438f-475">To use the configuration, restart Bind.</span></span> <span data-ttu-id="a438f-476">Například, `sudo service bind9 restart`.</span><span class="sxs-lookup"><span data-stu-id="a438f-476">For example, `sudo service bind9 restart`.</span></span>

4. <span data-ttu-id="a438f-477">Přidejte do místní server DNS pro podmíněné předávání.</span><span class="sxs-lookup"><span data-stu-id="a438f-477">Add a conditional forwarder to the on-premises DNS server.</span></span> <span data-ttu-id="a438f-478">Nakonfigurujte pro podmíněné předávání na odesílání žádostí pro příponu DNS z kroku 1 na vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-478">Configure the conditional forwarder to send requests for the DNS suffix from step 1 to the custom DNS server.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a438f-479">V dokumentaci pro váš software DNS pro konkrétní o tom, jak přidat server pro podmíněné předávání.</span><span class="sxs-lookup"><span data-stu-id="a438f-479">Consult the documentation for your DNS software for specifics on how to add a conditional forwarder.</span></span>

<span data-ttu-id="a438f-480">Po dokončení těchto kroků se můžete připojit k prostředkům v síti, buď pomocí plně kvalifikovaných názvů domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="a438f-480">After completing these steps, you can connect to resources in either network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="a438f-481">HDInsight můžete nainstalovat do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-481">You can now install HDInsight into the virtual network.</span></span>

### <a name="name-resolution-between-two-connected-virtual-networks"></a><span data-ttu-id="a438f-482">Překlad mezi dvěma připojené virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="a438f-482">Name resolution between two connected virtual networks</span></span>

<span data-ttu-id="a438f-483">Tento příklad vytvoří následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="a438f-483">This example makes the following assumptions:</span></span>

* <span data-ttu-id="a438f-484">Máte dvě virtuální sítě Azure, které jsou připojené pomocí brány VPN nebo partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="a438f-484">You have two Azure Virtual Networks that are connected using either a VPN gateway or peering.</span></span>

* <span data-ttu-id="a438f-485">Vlastní server DNS v obě sítě běží jako operační systém Linux nebo Unix.</span><span class="sxs-lookup"><span data-stu-id="a438f-485">The custom DNS server in both networks is running Linux or Unix as the operating system.</span></span>

* <span data-ttu-id="a438f-486">[Vytvoření vazby](https://www.isc.org/downloads/bind/) je nainstalován na vlastních serverech DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-486">[Bind](https://www.isc.org/downloads/bind/) is installed on the custom DNS servers.</span></span>

1. <span data-ttu-id="a438f-487">Použití Azure PowerShell nebo rozhraní příkazového řádku Azure k nalezení příponu DNS obě virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="a438f-487">Use either Azure PowerShell or Azure CLI to find the DNS suffix of both virtual networks:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="a438f-488">Použít následující text jako obsah `/etc/bind/named.config.local` souboru na vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-488">Use the following text as the contents of the `/etc/bind/named.config.local` file on the custom DNS server.</span></span> <span data-ttu-id="a438f-489">Tuto změnu proveďte na serveru DNS vlastní obě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-489">Make this change on the custom DNS server in both virtual networks.</span></span>

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    <span data-ttu-id="a438f-490">Nahraďte `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hodnotu s příponu DNS __jiných__ virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-490">Replace the `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with the DNS suffix of the __other__ virtual network.</span></span> <span data-ttu-id="a438f-491">Tato položka směruje požadavky pro příponu DNS vzdálené sítě na vlastní DNS v síti.</span><span class="sxs-lookup"><span data-stu-id="a438f-491">This entry routes requests for the DNS suffix of the remote network to the custom DNS in that network.</span></span>

3. <span data-ttu-id="a438f-492">Na vlastní servery DNS v obě virtuální sítě, použijte následující text jako obsah `/etc/bind/named.conf.options` souboru:</span><span class="sxs-lookup"><span data-stu-id="a438f-492">On the custom DNS servers in both virtual networks, use the following text as the contents of the `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="a438f-493">Nahraďte `10.0.0.0/16` a `10.1.0.0/16` hodnot pomocí IP adresy rozsahů virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="a438f-493">Replace the `10.0.0.0/16` and `10.1.0.0/16` values with the IP address ranges of your virtual networks.</span></span> <span data-ttu-id="a438f-494">Tato položka umožňuje prostředky v každé sítě odesílá požadavky na servery DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-494">This entry allows resources in each network to make requests of the DNS servers.</span></span>

    <span data-ttu-id="a438f-495">Překladač Azure rekurzivní zpracovává všechny žádosti, které nejsou pro přípony DNS virtuální sítě (například microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a438f-495">Any requests that are not for the DNS suffixes of the virtual networks (for example, microsoft.com) is handled by the Azure recursive resolver.</span></span>

4. <span data-ttu-id="a438f-496">Pokud chcete používat konfiguraci, restartujte vazby.</span><span class="sxs-lookup"><span data-stu-id="a438f-496">To use the configuration, restart Bind.</span></span> <span data-ttu-id="a438f-497">Například `sudo service bind9 restart` na obou serverech DNS.</span><span class="sxs-lookup"><span data-stu-id="a438f-497">For example, `sudo service bind9 restart` on both DNS servers.</span></span>

<span data-ttu-id="a438f-498">Po dokončení těchto kroků se můžete připojit k prostředkům ve virtuální síti pomocí plně kvalifikovaných názvů domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="a438f-498">After completing these steps, you can connect to resources in the virtual network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="a438f-499">HDInsight můžete nainstalovat do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a438f-499">You can now install HDInsight into the virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a438f-500">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a438f-500">Next steps</span></span>

* <span data-ttu-id="a438f-501">Příklad začátku do konce konfigurace HDInsight pro připojení k místní síti, najdete v části [HDInsight připojit k místní síti](./connect-on-premises-network.md).</span><span class="sxs-lookup"><span data-stu-id="a438f-501">For an end-to-end example of configuring HDInsight to connect to an on-premises network, see [Connect HDInsight to an on-premises network](./connect-on-premises-network.md).</span></span>

* <span data-ttu-id="a438f-502">Další informace o virtuálních sítí Azure, najdete v článku [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a438f-502">For more information on Azure virtual networks, see the [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="a438f-503">Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="a438f-503">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="a438f-504">Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a438f-504">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>