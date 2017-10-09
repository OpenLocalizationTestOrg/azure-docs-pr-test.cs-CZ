---
title: "aaaExtend prostředí HDInsight pomocí virtuální sítě - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Virtual Network tooconnect HDInsight tooother cloudové prostředky nebo prostředky ve vašem datovém centru"
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
ms.openlocfilehash: ba80be4d9f280c6c62fa8acc996ef5f921acdbbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a><span data-ttu-id="9cfae-103">Rozšíření Azure HDInsight pomocí virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="9cfae-103">Extend Azure HDInsight using an Azure Virtual Network</span></span>

<span data-ttu-id="9cfae-104">Zjistěte, jak toouse HDInsight s [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9cfae-104">Learn how toouse HDInsight with an [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="9cfae-105">Použití virtuální sítě Azure umožňuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="9cfae-105">Using an Azure Virtual Network enables hello following scenarios:</span></span>

* <span data-ttu-id="9cfae-106">Připojení tooHDInsight přímo z místní sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-106">Connecting tooHDInsight directly from an on-premises network.</span></span>

* <span data-ttu-id="9cfae-107">Připojení HDInsight toodata ukládá v Azure virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-107">Connecting HDInsight toodata stores in an Azure Virtual network.</span></span>

* <span data-ttu-id="9cfae-108">Přímo hello přístupem služby Hadoop, které nejsou k dispozici veřejně přes internet.</span><span class="sxs-lookup"><span data-stu-id="9cfae-108">Directly accessing Hadoop services that are not available publicly over hello internet.</span></span> <span data-ttu-id="9cfae-109">Například Kafka rozhraní API nebo hello HBase Java API.</span><span class="sxs-lookup"><span data-stu-id="9cfae-109">For example, Kafka APIs or hello HBase Java API.</span></span>

> [!WARNING]
> <span data-ttu-id="9cfae-110">Hello informace v tomto dokumentu vyžaduje znalosti o protokolu TCP/IP v síti.</span><span class="sxs-lookup"><span data-stu-id="9cfae-110">hello information in this document requires an understanding of TCP/IP networking.</span></span> <span data-ttu-id="9cfae-111">Pokud nejste obeznámeni s prací v síti TCP/IP, by měla spolupracovat s uživatelem, který je před provedením změny tooproduction sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-111">If you are not familiar with TCP/IP networking, you should partner with someone who is before making modifications tooproduction networks.</span></span>

## <a name="planning"></a><span data-ttu-id="9cfae-112">Plánování</span><span class="sxs-lookup"><span data-stu-id="9cfae-112">Planning</span></span>

<span data-ttu-id="9cfae-113">Hello následují hello otázky, které je nutné zodpovědět při plánování tooinstall HDInsight ve virtuální síti:</span><span class="sxs-lookup"><span data-stu-id="9cfae-113">hello following are hello questions that you must answer when planning tooinstall HDInsight in a virtual network:</span></span>

* <span data-ttu-id="9cfae-114">Potřebujete tooinstall HDInsight do existující virtuální síť?</span><span class="sxs-lookup"><span data-stu-id="9cfae-114">Do you need tooinstall HDInsight into an existing virtual network?</span></span> <span data-ttu-id="9cfae-115">Nebo můžete vytvořit novou síť?</span><span class="sxs-lookup"><span data-stu-id="9cfae-115">Or are you creating a new network?</span></span>

    <span data-ttu-id="9cfae-116">Pokud používáte stávající virtuální síť, musíte konfigurace sítě hello toomodify před instalací HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-116">If you are using an existing virtual network, you may need toomodify hello network configuration before you can install HDInsight.</span></span> <span data-ttu-id="9cfae-117">Další informace najdete v tématu hello [přidat HDInsight tooan existující virtuální síť](#existingvnet) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-117">For more information, see hello [add HDInsight tooan existing virtual network](#existingvnet) section.</span></span>

* <span data-ttu-id="9cfae-118">Chcete tooconnect hello virtuální sítě obsahující HDInsight tooanother virtuální sítě nebo v místní síti?</span><span class="sxs-lookup"><span data-stu-id="9cfae-118">Do you want tooconnect hello virtual network containing HDInsight tooanother virtual network or your on-premises network?</span></span>

    <span data-ttu-id="9cfae-119">tooeasily práci s prostředky v sítích, můžete potřebovat toocreate vlastní DNS a nakonfigurujte předávání DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-119">tooeasily work with resources across networks, you may need toocreate a custom DNS and configure DNS forwarding.</span></span> <span data-ttu-id="9cfae-120">Další informace najdete v tématu hello [připojení více sítí](#multinet) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-120">For more information, see hello [connecting multiple networks](#multinet) section.</span></span>

* <span data-ttu-id="9cfae-121">Chcete, aby toorestrict či přesměrování příchozích a odchozích přenosů tooHDInsight?</span><span class="sxs-lookup"><span data-stu-id="9cfae-121">Do you want toorestrict/redirect inbound or outbound traffic tooHDInsight?</span></span>

    <span data-ttu-id="9cfae-122">HDInsight musí mít neomezený komunikace s konkrétní IP adresy v hello datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfae-122">HDInsight must have unrestricted communication with specific IP addresses in hello Azure data center.</span></span> <span data-ttu-id="9cfae-123">Existují také několik portů, které musí být povoleno přes brány firewall pro komunikaci klienta.</span><span class="sxs-lookup"><span data-stu-id="9cfae-123">There are also several ports that must be allowed through firewalls for client communication.</span></span> <span data-ttu-id="9cfae-124">Další informace najdete v tématu hello [řízení síťového provozu](#networktraffic) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-124">For more information, see hello [controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="9cfae-125"><a id="existingvnet"></a>Přidat HDInsight tooan existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="9cfae-125"><a id="existingvnet"></a>Add HDInsight tooan existing virtual network</span></span>

<span data-ttu-id="9cfae-126">Jak používat hello kroky v této části toodiscover tooadd nové tooan HDInsight existující virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfae-126">Use hello steps in this section toodiscover how tooadd a new HDInsight tooan existing Azure Virtual Network.</span></span>

> [!NOTE]
> <span data-ttu-id="9cfae-127">Nelze přidat stávající cluster HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-127">You cannot add an existing HDInsight cluster into a virtual network.</span></span>

1. <span data-ttu-id="9cfae-128">Používáte pro virtuální síť hello klasický nebo modelu nasazení Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="9cfae-128">Are you using a classic or Resource Manager deployment model for hello virtual network?</span></span>

    <span data-ttu-id="9cfae-129">HDInsight 3.4 a větší vyžaduje virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9cfae-129">HDInsight 3.4 and greater requires a Resource Manager virtual network.</span></span> <span data-ttu-id="9cfae-130">Dřívějších verzích HDInsight vyžaduje klasickou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="9cfae-130">Earlier versions of HDInsight required a classic virtual network.</span></span>

    <span data-ttu-id="9cfae-131">Pokud vaší stávající sítě klasickou virtuální síť, musíte vytvořit virtuální sítě Resource Manager a potom se připojte hello dva.</span><span class="sxs-lookup"><span data-stu-id="9cfae-131">If your existing network is a classic virtual network, then you must create a Resource Manager virtual network and then connect hello two.</span></span> <span data-ttu-id="9cfae-132">[Připojení klasické virtuální sítě toonew virtuálních sítí](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9cfae-132">[Connecting classic VNets toonew VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span></span>

    <span data-ttu-id="9cfae-133">Jakmile připojený, HDInsight v síti Resource Manager hello nainstalovaný mohou komunikovat s prostředky v síti classic hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-133">Once joined, HDInsight installed in hello Resource Manager network can interact with resources in hello classic network.</span></span>

2. <span data-ttu-id="9cfae-134">Používáte vynucené tunelování?</span><span class="sxs-lookup"><span data-stu-id="9cfae-134">Do you use forced tunneling?</span></span> <span data-ttu-id="9cfae-135">Vynucené tunelování je nastavení podsítě, které vynutí odchozí internetové přenosy tooa zařízení pro kontroly a protokolování.</span><span class="sxs-lookup"><span data-stu-id="9cfae-135">Forced tunneling is a subnet setting that forces outbound Internet traffic tooa device for inspection and logging.</span></span> <span data-ttu-id="9cfae-136">HDInsight nepodporuje vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="9cfae-136">HDInsight does not support forced tunneling.</span></span> <span data-ttu-id="9cfae-137">Buď odeberte vynucené tunelování před instalací HDInsight do podsítě, nebo vytvořit novou podsíť pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-137">Either remove forced tunneling before installing HDInsight into a subnet, or create a new subnet for HDInsight.</span></span>

3. <span data-ttu-id="9cfae-138">Používáte skupiny zabezpečení sítě, trasy definované uživatelem nebo virtuální síťové zařízení toorestrict provoz do nebo z hello virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="9cfae-138">Do you use network security groups, user-defined routes, or Virtual Network Appliances toorestrict traffic into or out of hello virtual network?</span></span>

    <span data-ttu-id="9cfae-139">Jako spravovanou službu vyžaduje HDInsight neomezený přístup tooseveral IP adresy v hello datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfae-139">As a managed service, HDInsight requires unrestricted access tooseveral IP addresses in hello Azure data center.</span></span> <span data-ttu-id="9cfae-140">tooallow komunikace se tyto IP adresy, aktualizovat všechny existující skupiny zabezpečení sítě nebo trasy definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9cfae-140">tooallow communication with these IP addresses, update any existing network security groups or user-defined routes.</span></span>

    <span data-ttu-id="9cfae-141">HDInsight je hostitelem více služeb, které používají různé porty.</span><span class="sxs-lookup"><span data-stu-id="9cfae-141">HDInsight  hosts multiple services, which use a variety of ports.</span></span> <span data-ttu-id="9cfae-142">Provoz toothese porty nejsou blokovány.</span><span class="sxs-lookup"><span data-stu-id="9cfae-142">Do not block traffic toothese ports.</span></span> <span data-ttu-id="9cfae-143">Seznam portů tooallow přes virtuální zařízení brány firewall, naleznete v části hello [zabezpečení](#security) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-143">For a list of ports tooallow through virtual appliance firewalls, see hello [Security](#security) section.</span></span>

    <span data-ttu-id="9cfae-144">toofind existující konfiguraci zabezpečení, hello použijte následující příkazy prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="9cfae-144">toofind your existing security configuration, use hello following Azure PowerShell or Azure CLI commands:</span></span>

    * <span data-ttu-id="9cfae-145">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="9cfae-145">Network security groups</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="9cfae-146">Další informace najdete v tématu hello [odstraňování skupin zabezpečení sítě](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-146">For more information, see hello [Troubleshoot network security groups](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) document.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="9cfae-147">Pravidla skupiny zabezpečení sítě jsou použity v pořadí podle priority pravidel.</span><span class="sxs-lookup"><span data-stu-id="9cfae-147">Network security group rules are applied in order based on rule priority.</span></span> <span data-ttu-id="9cfae-148">Hello první pravidlo, které odpovídá vzoru provoz hello platí a žádné jiné se použijí pro tento přenos.</span><span class="sxs-lookup"><span data-stu-id="9cfae-148">hello first rule that matches hello traffic pattern is applied, and no others are applied for that traffic.</span></span> <span data-ttu-id="9cfae-149">Pravidla pořadí od nejvíce projektovou tooleast projektovou.</span><span class="sxs-lookup"><span data-stu-id="9cfae-149">Order rules from most permissive tooleast permissive.</span></span> <span data-ttu-id="9cfae-150">Další informace najdete v tématu hello [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-150">For more information, see hello [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    * <span data-ttu-id="9cfae-151">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="9cfae-151">User-defined routes</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="9cfae-152">Další informace najdete v tématu hello [řešení potíží s trasy](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-152">For more information, see hello [Troubleshoot routes](../virtual-network/virtual-network-routes-troubleshoot-portal.md) document.</span></span>

4. <span data-ttu-id="9cfae-153">Vytvoření clusteru HDInsight a vyberte hello Azure Virtual Network během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9cfae-153">Create an HDInsight cluster and select hello Azure Virtual Network during configuration.</span></span> <span data-ttu-id="9cfae-154">Použijte hello kroky v následujícím procesu vytváření clusteru hello toounderstand dokumenty hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-154">Use hello steps in hello following documents toounderstand hello cluster creation process:</span></span>

    * [<span data-ttu-id="9cfae-155">Vytvoření HDInsight pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9cfae-155">Create HDInsight using hello Azure portal</span></span>](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [<span data-ttu-id="9cfae-156">Vytvoření HDInsight pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="9cfae-156">Create HDInsight using Azure PowerShell</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [<span data-ttu-id="9cfae-157">Vytvoření HDInsight pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9cfae-157">Create HDInsight using Azure CLI 1.0</span></span>](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [<span data-ttu-id="9cfae-158">Vytvoření HDInsight pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9cfae-158">Create HDInsight using an Azure Resource Manager template</span></span>](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > <span data-ttu-id="9cfae-159">Přidání HDInsight tooa virtuální síť se na krok volitelné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9cfae-159">Adding HDInsight tooa virtual network is an optional configuration step.</span></span> <span data-ttu-id="9cfae-160">Zda tooselect hello virtuální sítě se při konfiguraci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-160">Be sure tooselect hello virtual network when configuring hello cluster.</span></span>

## <span data-ttu-id="9cfae-161"><a id="multinet"></a>Připojení více sítí</span><span class="sxs-lookup"><span data-stu-id="9cfae-161"><a id="multinet"></a>Connecting multiple networks</span></span>

<span data-ttu-id="9cfae-162">Hello největších problém s konfigurací s více sítě je překlad mezi sítěmi hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-162">hello biggest challenge with a multi-network configuration is name resolution between hello networks.</span></span>

<span data-ttu-id="9cfae-163">Azure poskytuje překlad názvů pro služby Azure, které jsou nainstalovány ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="9cfae-163">Azure provides name resolution for Azure services that are installed in a virtual network.</span></span> <span data-ttu-id="9cfae-164">Toto řešení předdefinovaným názvem umožňuje toohello tooconnect HDInsight následující prostředky pomocí platný plně kvalifikovaný název domény (FQDN):</span><span class="sxs-lookup"><span data-stu-id="9cfae-164">This built-in name resolution allows HDInsight tooconnect toohello following resources by using a fully qualified domain name (FQDN):</span></span>

* <span data-ttu-id="9cfae-165">Hello jakémukoli prostředku, který je dostupný na Internetu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-165">Any resource that is available on hello internet.</span></span> <span data-ttu-id="9cfae-166">Například microsoft.com, google.com.</span><span class="sxs-lookup"><span data-stu-id="9cfae-166">For example, microsoft.com, google.com.</span></span>

* <span data-ttu-id="9cfae-167">Jakémukoli prostředku, který je v hello stejnou virtuální síť Azure, pomocí hello __interní název DNS__ hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="9cfae-167">Any resource that is in hello same Azure Virtual Network, by using hello __internal DNS name__ of hello resource.</span></span> <span data-ttu-id="9cfae-168">Například při použití hello výchozí název řešení, jsou hello následující příklad interní DNS názvy přiřazené tooHDInsight pracovní uzly:</span><span class="sxs-lookup"><span data-stu-id="9cfae-168">For example, when using hello default name resolution, hello following are example internal DNS names assigned tooHDInsight worker nodes:</span></span>

    * <span data-ttu-id="9cfae-169">wn0 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="9cfae-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>
    * <span data-ttu-id="9cfae-170">wn2 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="9cfae-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>

    <span data-ttu-id="9cfae-171">Obě tyto uzly může komunikovat přímo s a jiné uzly v HDInsight pomocí interní názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-171">Both these nodes can communicate directly with each other, and other nodes in HDInsight, by using internal DNS names.</span></span>

<span data-ttu-id="9cfae-172">rozlišení názvů výchozí Hello nemá __není__ povolit HDInsight tooresolve hello názvy prostředků v sítích, které jsou připojené k toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-172">hello default name resolution does __not__ allow HDInsight tooresolve hello names of resources in networks that are joined toohello virtual network.</span></span> <span data-ttu-id="9cfae-173">Například je běžné toojoin místní sítě toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-173">For example, it is common toojoin your on-premises network toohello virtual network.</span></span> <span data-ttu-id="9cfae-174">S pouze hello výchozí překlad IP adres HDInsight nemají přístup k prostředkům v místní síti hello podle názvu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-174">With only hello default name resolution, HDInsight cannot access resources in hello on-premises network by name.</span></span> <span data-ttu-id="9cfae-175">Hello opačné je také nastavena hodnota true, prostředky ve vaší místní síti nemají přístup k prostředkům ve virtuální síti hello podle názvu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-175">hello opposite is also true, resources in your on-premises network cannot access resources in hello virtual network by name.</span></span>

> [!WARNING]
> <span data-ttu-id="9cfae-176">Musíte vytvořit hello vlastního serveru DNS a konfigurovat virtuální sítě toouse hello jej před vytvořením hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-176">You must create hello custom DNS server and configure hello virtual network toouse it before creating hello HDInsight cluster.</span></span>

<span data-ttu-id="9cfae-177">tooenable překlad mezi hello virtuální sítě a prostředky v připojené k sítím, je třeba provést hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="9cfae-177">tooenable name resolution between hello virtual network and resources in joined networks, you must perform hello following actions:</span></span>

1. <span data-ttu-id="9cfae-178">Vytvoření vlastního serveru DNS v hello virtuální sítě Azure, kam budete tooinstall HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-178">Create a custom DNS server in hello Azure Virtual Network where you plan tooinstall HDInsight.</span></span>

2. <span data-ttu-id="9cfae-179">Nakonfigurujte hello virtuální sítě toouse hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-179">Configure hello virtual network toouse hello custom DNS server.</span></span>

3. <span data-ttu-id="9cfae-180">Najde hello přiřazené přípona DNS pro vaši virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfae-180">Find hello Azure assigned DNS suffix for your virtual network.</span></span> <span data-ttu-id="9cfae-181">Tato hodnota je příliš podobné`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="9cfae-181">This value is similar too`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span></span> <span data-ttu-id="9cfae-182">Informace o zjištění hello příponu DNS, najdete v části hello [příklad: vlastní DNS](#example-dns) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-182">For information on finding hello DNS suffix, see hello [Example: Custom DNS](#example-dns) section.</span></span>

4. <span data-ttu-id="9cfae-183">Konfigurace předávání mezi servery DNS hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-183">Configure forwarding between hello DNS servers.</span></span> <span data-ttu-id="9cfae-184">Konfigurace Hello závisí na typu hello vzdálené sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-184">hello configuration depends on hello type of remote network.</span></span>

    * <span data-ttu-id="9cfae-185">Pokud hello vzdálené sítě do místní sítě, nakonfigurujte DNS takto:</span><span class="sxs-lookup"><span data-stu-id="9cfae-185">If hello remote network is an on-premises network, configure DNS as follows:</span></span>
        
        * <span data-ttu-id="9cfae-186">__Vlastní DNS__ (ve virtuální síti hello):</span><span class="sxs-lookup"><span data-stu-id="9cfae-186">__Custom DNS__ (in hello virtual network):</span></span>

            * <span data-ttu-id="9cfae-187">Předat dál požadavků pro přípony DNS hello hello virtuální sítě toohello Azure rekurzivní překladač (168.63.129.16).</span><span class="sxs-lookup"><span data-stu-id="9cfae-187">Forward requests for hello DNS suffix of hello virtual network toohello Azure recursive resolver (168.63.129.16).</span></span> <span data-ttu-id="9cfae-188">Azure zpracovává požadavky na prostředky ve virtuální síti hello</span><span class="sxs-lookup"><span data-stu-id="9cfae-188">Azure handles requests for resources in hello virtual network</span></span>

            * <span data-ttu-id="9cfae-189">Předat dál všechny ostatní požadavky toohello místní server DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-189">Forward all other requests toohello on-premises DNS server.</span></span> <span data-ttu-id="9cfae-190">Hello místního DNS zpracovává všechny ostatní požadavky na rozlišení názvů, i požadavky na internetové prostředky, jako je například Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="9cfae-190">hello on-premises DNS handles all other name resolution requests, even requests for internet resources such as Microsoft.com.</span></span>

        * <span data-ttu-id="9cfae-191">__Místní DNS__: předávat požadavky pro hello virtuální sítě DNS přípona toohello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-191">__On-premises DNS__: Forward requests for hello virtual network DNS suffix toohello custom DNS server.</span></span> <span data-ttu-id="9cfae-192">Hello vlastního serveru DNS potom předává toohello Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="9cfae-192">hello custom DNS server then forwards toohello Azure recursive resolver.</span></span>

        <span data-ttu-id="9cfae-193">Tato požadavky na konfiguraci tras pro plně kvalifikované názvy domény, které obsahují příponu DNS hello hello virtuální sítě toohello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-193">This configuration routes requests for fully qualified domain names that contain hello DNS suffix of hello virtual network toohello custom DNS server.</span></span> <span data-ttu-id="9cfae-194">Server DNS místní hello zpracovává všechny požadavky (i pro veřejné internetové adresy).</span><span class="sxs-lookup"><span data-stu-id="9cfae-194">All other requests (even for public internet addresses) are handled by hello on-premises DNS server.</span></span>

    * <span data-ttu-id="9cfae-195">Pokud vzdálené sítě hello jinou virtuální sítí Azure, nakonfigurujte DNS následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9cfae-195">If hello remote network is another Azure Virtual Network, configure DNS as follows:</span></span>

        * <span data-ttu-id="9cfae-196">__Vlastní DNS__ (v každé virtuální sítě):</span><span class="sxs-lookup"><span data-stu-id="9cfae-196">__Custom DNS__ (in each virtual network):</span></span>

            * <span data-ttu-id="9cfae-197">Požadavky pro příponu DNS hello hello virtuální sítě jsou předávány toohello vlastní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-197">Requests for hello DNS suffix of hello virtual networks are forwarded toohello custom DNS servers.</span></span> <span data-ttu-id="9cfae-198">Hello DNS v každé virtuální sítě je zodpovědná za řešení prostředky ve své síti.</span><span class="sxs-lookup"><span data-stu-id="9cfae-198">hello DNS in each virtual network is responsible for resolving resources within its network.</span></span>

            * <span data-ttu-id="9cfae-199">Předávání všech dalších překladač Azure rekurzivní toohello požadavky.</span><span class="sxs-lookup"><span data-stu-id="9cfae-199">Forward all other requests toohello Azure recursive resolver.</span></span> <span data-ttu-id="9cfae-200">rekurzivní překladač Hello je zodpovědná za řešení místní a prostředků z Internetu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-200">hello recursive resolver is responsible for resolving local and internet resources.</span></span>

        <span data-ttu-id="9cfae-201">server DNS Hello pro každou síť, předává žádosti o toohello jiných, podle přípony DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-201">hello DNS server for each network forwards requests toohello other, based on DNS suffix.</span></span> <span data-ttu-id="9cfae-202">Další požadavky se přeloží pomocí překladače Azure rekurzivní hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-202">Other requests are resolved using hello Azure recursive resolver.</span></span>

    <span data-ttu-id="9cfae-203">Příklad každé konfiguraci, naleznete v části hello [příklad: vlastní DNS](#example-dns) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-203">For an example of each configuration, see hello [Example: Custom DNS](#example-dns) section.</span></span>

<span data-ttu-id="9cfae-204">Další informace najdete v tématu hello [překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-204">For more information, see hello [Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) document.</span></span>

## <a name="directly-connect-toohadoop-services"></a><span data-ttu-id="9cfae-205">Připojovat přímo tooHadoop služby</span><span class="sxs-lookup"><span data-stu-id="9cfae-205">Directly connect tooHadoop services</span></span>

<span data-ttu-id="9cfae-206">Většina dokumentace v HDInsight předpokládá, že máte přístup toohello clusteru přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-206">Most documentation on HDInsight assumes that you have access toohello cluster over hello internet.</span></span> <span data-ttu-id="9cfae-207">Například, že se můžete připojit toohello clusteru https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="9cfae-207">For example, that you can connect toohello cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="9cfae-208">Tuto adresu používá hello veřejné bránu, která není k dispozici, pokud jste použili skupiny Nsg nebo hello udr toorestrict přístupu z Internetu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-208">This address uses hello public gateway, which is not available if you have used NSGs or UDRs toorestrict access from hello internet.</span></span>

<span data-ttu-id="9cfae-209">tooconnect tooAmbari a jiné webové stránky prostřednictvím hello virtuální sítě, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9cfae-209">tooconnect tooAmbari and other web pages through hello virtual network, use hello following steps:</span></span>

1. <span data-ttu-id="9cfae-210">toodiscover hello interní plně kvalifikované názvy domény (FQDN) hello uzly clusteru HDInsight, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-210">toodiscover hello internal fully qualified domain names (FQDN) of hello HDInsight cluster nodes, use one of hello following methods:</span></span>

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

    <span data-ttu-id="9cfae-211">V seznamu hello uzlů vrátil najít hello plně kvalifikovaný název domény pro hello hlavních uzlech a použitím tooAmbari tooconnect hello plně kvalifikované názvy domény a dalších webových služeb.</span><span class="sxs-lookup"><span data-stu-id="9cfae-211">In hello list of nodes returned, find hello FQDN for hello head nodes and use hello FQDNs tooconnect tooAmbari and other web services.</span></span> <span data-ttu-id="9cfae-212">Například použít `http://<headnode-fqdn>:8080` tooaccess Ambari.</span><span class="sxs-lookup"><span data-stu-id="9cfae-212">For example, use `http://<headnode-fqdn>:8080` tooaccess Ambari.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9cfae-213">Některé služby hostované v uzlech head hello aktivní pouze na jednom uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="9cfae-213">Some services hosted on hello head nodes are only active on one node at a time.</span></span> <span data-ttu-id="9cfae-214">Pokud se pokusíte přístup k službě na jeden hlavní uzel a vrátí chybu 404, přepínač toohello jiných hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-214">If you try accessing a service on one head node and it returns a 404 error, switch toohello other head node.</span></span>

2. <span data-ttu-id="9cfae-215">uzel hello toodetermine a port, který služba je k dispozici, najdete v části hello [porty používané služby Hadoop v HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-215">toodetermine hello node and port that a service is available on, see hello [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

## <span data-ttu-id="9cfae-216"><a id="networktraffic"></a>Řízení síťového provozu</span><span class="sxs-lookup"><span data-stu-id="9cfae-216"><a id="networktraffic"></a> Controlling network traffic</span></span>

<span data-ttu-id="9cfae-217">Síťový provoz v Azure Virtual Network se dá řídit pomocí hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="9cfae-217">Network traffic in an Azure Virtual Networks can be controlled using hello following methods:</span></span>

* <span data-ttu-id="9cfae-218">**Skupin zabezpečení sítě** (NSG) umožňují toofilter příchozí a odchozí přenosy toohello sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-218">**Network security groups** (NSG) allow you toofilter inbound and outbound traffic toohello network.</span></span> <span data-ttu-id="9cfae-219">Další informace najdete v tématu hello [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-219">For more information, see hello [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    > [!WARNING]
    > <span data-ttu-id="9cfae-220">HDInsight nepodporuje omezení odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="9cfae-220">HDInsight does not support restricting outbound traffic.</span></span>

* <span data-ttu-id="9cfae-221">**Trasy definované uživatelem** (UDR) definovat tok přenosů mezi prostředky v síti hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-221">**User-defined routes** (UDR) define how traffic flows between resources in hello network.</span></span> <span data-ttu-id="9cfae-222">Další informace najdete v tématu hello [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-222">For more information, see hello [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md) document.</span></span>

* <span data-ttu-id="9cfae-223">**Síťových virtuálních zařízení** replikovat hello funkce zařízení, jako jsou brány firewall a směrovače.</span><span class="sxs-lookup"><span data-stu-id="9cfae-223">**Network virtual appliances** replicate hello functionality of devices such as firewalls and routers.</span></span> <span data-ttu-id="9cfae-224">Další informace najdete v tématu hello [síťových zařízení](https://azure.microsoft.com/solutions/network-appliances) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-224">For more information, see hello [Network Appliances](https://azure.microsoft.com/solutions/network-appliances) document.</span></span>

<span data-ttu-id="9cfae-225">Jako spravované služby HDInsight vyžaduje služby stavu a správu tooAzure neomezený přístup v hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfae-225">As a managed service, HDInsight requires unrestricted access tooAzure health and management services in hello Azure cloud.</span></span> <span data-ttu-id="9cfae-226">Pokud používáte skupiny Nsg a udr, je nutné zajistit, že HDInsight tyto služby můžete stále komunikovat s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-226">When using NSGs and UDRs, you must ensure that HDInsight these services can still communicate with HDInsight.</span></span>

<span data-ttu-id="9cfae-227">HDInsight poskytuje služby na několika portech.</span><span class="sxs-lookup"><span data-stu-id="9cfae-227">HDInsight exposes services on several ports.</span></span> <span data-ttu-id="9cfae-228">Pokud používáte virtuální zařízení brány firewall, musíte povolit přenosy na hello porty používané pro tyto služby.</span><span class="sxs-lookup"><span data-stu-id="9cfae-228">When using a virtual appliance firewall, you must allow traffic on hello ports used for these services.</span></span> <span data-ttu-id="9cfae-229">Další informace najdete v části hello [požadované porty].</span><span class="sxs-lookup"><span data-stu-id="9cfae-229">For more information, see hello [Required ports] section.</span></span>

### <span data-ttu-id="9cfae-230"><a id="hdinsight-ip"></a>Prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="9cfae-230"><a id="hdinsight-ip"></a> HDInsight with network security groups and user-defined routes</span></span>

<span data-ttu-id="9cfae-231">Pokud máte v úmyslu používat **skupin zabezpečení sítě** nebo **trasy definované uživatelem** toocontrol přenos v síti, proveďte následující akce před instalací HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-231">If you plan on using **network security groups** or **user-defined routes** toocontrol network traffic, perform hello following actions before installing HDInsight:</span></span>

1. <span data-ttu-id="9cfae-232">Identifikujte hello oblast Azure, abyste naplánovali toouse pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-232">Identify hello Azure region that you plan toouse for HDInsight.</span></span>

2. <span data-ttu-id="9cfae-233">Identifikujte hello IP adresy potřeba v prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-233">Identify hello IP addresses required by HDInsight.</span></span> <span data-ttu-id="9cfae-234">Další informace najdete v tématu hello [IP adresy, které vyžadují HDInsight](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-234">For more information, see hello [IP Addresses required by HDInsight](#hdinsight-ip) section.</span></span>

3. <span data-ttu-id="9cfae-235">Vytvoření nebo úprava skupiny zabezpečení sítě hello nebo trasy definované uživatelem hello podsítě, abyste naplánovali tooinstall HDInsight do.</span><span class="sxs-lookup"><span data-stu-id="9cfae-235">Create or modify hello network security groups or user-defined routes for hello subnet that you plan tooinstall HDInsight into.</span></span>

    * <span data-ttu-id="9cfae-236">__Skupin zabezpečení sítě__: Povolit __příchozí__ přenosy na portu __443__ z hello IP adres.</span><span class="sxs-lookup"><span data-stu-id="9cfae-236">__Network security groups__: allow __inbound__ traffic on port __443__ from hello IP addresses.</span></span>
    * <span data-ttu-id="9cfae-237">__Trasy definované uživatelem__: Vytvoření trasy tooeach IP adresy a nastavte hello __typ dalšího směrování__ too__Internet__.</span><span class="sxs-lookup"><span data-stu-id="9cfae-237">__User-defined routes__: create a route tooeach IP address and set hello __Next hop type__ too__Internet__.</span></span>

<span data-ttu-id="9cfae-238">Další informace o skupinách zabezpečení sítě nebo trasy definované uživatelem najdete v části hello následující dokumentaci:</span><span class="sxs-lookup"><span data-stu-id="9cfae-238">For more information on network security groups or user-defined routes, see hello following documentation:</span></span>

* [<span data-ttu-id="9cfae-239">Skupina zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="9cfae-239">Network security group</span></span>](../virtual-network/virtual-networks-nsg.md)

* [<span data-ttu-id="9cfae-240">Trasy definované uživatelem</span><span class="sxs-lookup"><span data-stu-id="9cfae-240">User-defined routes</span></span>](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a><span data-ttu-id="9cfae-241">Vynucené tunelování</span><span class="sxs-lookup"><span data-stu-id="9cfae-241">Forced tunneling</span></span>

<span data-ttu-id="9cfae-242">Vynucené tunelování je konfigurace směrování definovaný uživatelem, kde veškerý provoz z podsítě je vynucené tooa určitého síťového umístění nebo umístění, jako například do místní sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-242">Forced tunneling is a user-defined routing configuration where all traffic from a subnet is forced tooa specific network or location, such as your on-premises network.</span></span> <span data-ttu-id="9cfae-243">HDInsight nemá __není__ podporu vynuceného tunelování.</span><span class="sxs-lookup"><span data-stu-id="9cfae-243">HDInsight does __not__ support forced tunneling.</span></span>

## <span data-ttu-id="9cfae-244"><a id="hdinsight-ip"></a>Požadované IP adresy</span><span class="sxs-lookup"><span data-stu-id="9cfae-244"><a id="hdinsight-ip"></a> Required IP addresses</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9cfae-245">Hello Azure stavu a služby pro správu musí být schopný toocommunicate s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-245">hello Azure health and management services must be able toocommunicate with HDInsight.</span></span> <span data-ttu-id="9cfae-246">Pokud používáte skupiny zabezpečení sítě nebo trasy definované uživatelem, povolí provoz z hello IP adres pro tyto tooreach služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-246">If you use network security groups or user-defined routes, allow traffic from hello IP addresses for these services tooreach HDInsight.</span></span>
>
> <span data-ttu-id="9cfae-247">Pokud nepoužijete skupin zabezpečení sítě nebo trasy definované uživatelem toocontrol přenosů, můžete ignorovat v této části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-247">If you do not use network security groups or user-defined routes toocontrol traffic, you can ignore this section.</span></span>

<span data-ttu-id="9cfae-248">Pokud používáte skupiny zabezpečení sítě nebo trasy definované uživatelem, musíte povolit přenosy z hello Azure stavu a správu služeb tooreach HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-248">If you use network security groups or user-defined routes, you must allow traffic from hello Azure health and management services tooreach HDInsight.</span></span> <span data-ttu-id="9cfae-249">Použijte následující postup toofind hello IP adresy, které musí být povoleno hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-249">Use hello following steps toofind hello IP addresses that must be allowed:</span></span>

1. <span data-ttu-id="9cfae-250">Vždy musí povolit provoz z hello následující IP adresy:</span><span class="sxs-lookup"><span data-stu-id="9cfae-250">You must always allow traffic from hello following IP addresses:</span></span>

    | <span data-ttu-id="9cfae-251">IP adresa</span><span class="sxs-lookup"><span data-stu-id="9cfae-251">IP address</span></span> | <span data-ttu-id="9cfae-252">Povolené portu</span><span class="sxs-lookup"><span data-stu-id="9cfae-252">Allowed port</span></span> | <span data-ttu-id="9cfae-253">Směr</span><span class="sxs-lookup"><span data-stu-id="9cfae-253">Direction</span></span> |
    | ---- | ----- | ----- |
    | <span data-ttu-id="9cfae-254">168.61.49.99</span><span class="sxs-lookup"><span data-stu-id="9cfae-254">168.61.49.99</span></span> | <span data-ttu-id="9cfae-255">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-255">443</span></span> | <span data-ttu-id="9cfae-256">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-256">Inbound</span></span> |
    | <span data-ttu-id="9cfae-257">23.99.5.239</span><span class="sxs-lookup"><span data-stu-id="9cfae-257">23.99.5.239</span></span> | <span data-ttu-id="9cfae-258">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-258">443</span></span> | <span data-ttu-id="9cfae-259">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-259">Inbound</span></span> |
    | <span data-ttu-id="9cfae-260">168.61.48.131</span><span class="sxs-lookup"><span data-stu-id="9cfae-260">168.61.48.131</span></span> | <span data-ttu-id="9cfae-261">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-261">443</span></span> | <span data-ttu-id="9cfae-262">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-262">Inbound</span></span> |
    | <span data-ttu-id="9cfae-263">138.91.141.162</span><span class="sxs-lookup"><span data-stu-id="9cfae-263">138.91.141.162</span></span> | <span data-ttu-id="9cfae-264">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-264">443</span></span> | <span data-ttu-id="9cfae-265">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-265">Inbound</span></span> |

2. <span data-ttu-id="9cfae-266">Pokud váš cluster HDInsight v jednom z následujících oblastí hello, musíte povolit přenosy z hello IP adresy uvedené pro oblast hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-266">If your HDInsight cluster is in one of hello following regions, then you must allow traffic from hello IP addresses listed for hello region:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9cfae-267">Pokud není uvedené hello oblast Azure, který používáte, pak použijte jenom hello čtyři IP adresy z kroku 1.</span><span class="sxs-lookup"><span data-stu-id="9cfae-267">If hello Azure region you are using is not listed, then only use hello four IP addresses from step 1.</span></span>

    | <span data-ttu-id="9cfae-268">Země</span><span class="sxs-lookup"><span data-stu-id="9cfae-268">Country</span></span> | <span data-ttu-id="9cfae-269">Oblast</span><span class="sxs-lookup"><span data-stu-id="9cfae-269">Region</span></span> | <span data-ttu-id="9cfae-270">Povolené IP adresy</span><span class="sxs-lookup"><span data-stu-id="9cfae-270">Allowed IP addresses</span></span> | <span data-ttu-id="9cfae-271">Povolené portu</span><span class="sxs-lookup"><span data-stu-id="9cfae-271">Allowed port</span></span> | <span data-ttu-id="9cfae-272">Směr</span><span class="sxs-lookup"><span data-stu-id="9cfae-272">Direction</span></span> |
    | ---- | ---- | ---- | ---- | ----- |
    | <span data-ttu-id="9cfae-273">Asie</span><span class="sxs-lookup"><span data-stu-id="9cfae-273">Asia</span></span> | <span data-ttu-id="9cfae-274">Východní Asie</span><span class="sxs-lookup"><span data-stu-id="9cfae-274">East Asia</span></span> | <span data-ttu-id="9cfae-275">23.102.235.122</span><span class="sxs-lookup"><span data-stu-id="9cfae-275">23.102.235.122</span></span></br><span data-ttu-id="9cfae-276">52.175.38.134</span><span class="sxs-lookup"><span data-stu-id="9cfae-276">52.175.38.134</span></span> | <span data-ttu-id="9cfae-277">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-277">443</span></span> | <span data-ttu-id="9cfae-278">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-278">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-279">Jihovýchodní Asie</span><span class="sxs-lookup"><span data-stu-id="9cfae-279">Southeast Asia</span></span> | <span data-ttu-id="9cfae-280">13.76.245.160</span><span class="sxs-lookup"><span data-stu-id="9cfae-280">13.76.245.160</span></span></br><span data-ttu-id="9cfae-281">13.76.136.249</span><span class="sxs-lookup"><span data-stu-id="9cfae-281">13.76.136.249</span></span> | <span data-ttu-id="9cfae-282">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-282">443</span></span> | <span data-ttu-id="9cfae-283">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-283">Inbound</span></span> |
    | <span data-ttu-id="9cfae-284">Austrálie</span><span class="sxs-lookup"><span data-stu-id="9cfae-284">Australia</span></span> | <span data-ttu-id="9cfae-285">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="9cfae-285">Australia East</span></span> | <span data-ttu-id="9cfae-286">104.210.84.115</span><span class="sxs-lookup"><span data-stu-id="9cfae-286">104.210.84.115</span></span></br><span data-ttu-id="9cfae-287">13.75.152.195</span><span class="sxs-lookup"><span data-stu-id="9cfae-287">13.75.152.195</span></span> | <span data-ttu-id="9cfae-288">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-288">443</span></span> | <span data-ttu-id="9cfae-289">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-289">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-290">Austrálie – jihovýchod</span><span class="sxs-lookup"><span data-stu-id="9cfae-290">Australia Southeast</span></span> | <span data-ttu-id="9cfae-291">13.77.2.56</span><span class="sxs-lookup"><span data-stu-id="9cfae-291">13.77.2.56</span></span></br><span data-ttu-id="9cfae-292">13.77.2.94</span><span class="sxs-lookup"><span data-stu-id="9cfae-292">13.77.2.94</span></span> | <span data-ttu-id="9cfae-293">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-293">443</span></span> | <span data-ttu-id="9cfae-294">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-294">Inbound</span></span> |
    | <span data-ttu-id="9cfae-295">Brazílie</span><span class="sxs-lookup"><span data-stu-id="9cfae-295">Brazil</span></span> | <span data-ttu-id="9cfae-296">Brazílie – jih</span><span class="sxs-lookup"><span data-stu-id="9cfae-296">Brazil South</span></span> | <span data-ttu-id="9cfae-297">191.235.84.104</span><span class="sxs-lookup"><span data-stu-id="9cfae-297">191.235.84.104</span></span></br><span data-ttu-id="9cfae-298">191.235.87.113</span><span class="sxs-lookup"><span data-stu-id="9cfae-298">191.235.87.113</span></span> | <span data-ttu-id="9cfae-299">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-299">443</span></span> | <span data-ttu-id="9cfae-300">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-300">Inbound</span></span> |
    | <span data-ttu-id="9cfae-301">Kanada</span><span class="sxs-lookup"><span data-stu-id="9cfae-301">Canada</span></span> | <span data-ttu-id="9cfae-302">Východní Kanada</span><span class="sxs-lookup"><span data-stu-id="9cfae-302">Canada East</span></span> | <span data-ttu-id="9cfae-303">52.229.127.96</span><span class="sxs-lookup"><span data-stu-id="9cfae-303">52.229.127.96</span></span></br><span data-ttu-id="9cfae-304">52.229.123.172</span><span class="sxs-lookup"><span data-stu-id="9cfae-304">52.229.123.172</span></span> | <span data-ttu-id="9cfae-305">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-305">443</span></span> | <span data-ttu-id="9cfae-306">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-306">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-307">Střední Kanada</span><span class="sxs-lookup"><span data-stu-id="9cfae-307">Canada Central</span></span> | <span data-ttu-id="9cfae-308">52.228.37.66</span><span class="sxs-lookup"><span data-stu-id="9cfae-308">52.228.37.66</span></span></br><span data-ttu-id="9cfae-309">52.228.45.222</span><span class="sxs-lookup"><span data-stu-id="9cfae-309">52.228.45.222</span></span> | <span data-ttu-id="9cfae-310">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-310">443</span></span> | <span data-ttu-id="9cfae-311">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-311">Inbound</span></span> |
    | <span data-ttu-id="9cfae-312">Čína</span><span class="sxs-lookup"><span data-stu-id="9cfae-312">China</span></span> | <span data-ttu-id="9cfae-313">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="9cfae-313">China North</span></span> | <span data-ttu-id="9cfae-314">42.159.96.170</span><span class="sxs-lookup"><span data-stu-id="9cfae-314">42.159.96.170</span></span></br><span data-ttu-id="9cfae-315">139.217.2.219</span><span class="sxs-lookup"><span data-stu-id="9cfae-315">139.217.2.219</span></span> | <span data-ttu-id="9cfae-316">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-316">443</span></span> | <span data-ttu-id="9cfae-317">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-317">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-318">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="9cfae-318">China East</span></span> | <span data-ttu-id="9cfae-319">42.159.198.178</span><span class="sxs-lookup"><span data-stu-id="9cfae-319">42.159.198.178</span></span></br><span data-ttu-id="9cfae-320">42.159.234.157</span><span class="sxs-lookup"><span data-stu-id="9cfae-320">42.159.234.157</span></span> | <span data-ttu-id="9cfae-321">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-321">443</span></span> | <span data-ttu-id="9cfae-322">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-322">Inbound</span></span> |
    | <span data-ttu-id="9cfae-323">Evropa</span><span class="sxs-lookup"><span data-stu-id="9cfae-323">Europe</span></span> | <span data-ttu-id="9cfae-324">Severní Evropa</span><span class="sxs-lookup"><span data-stu-id="9cfae-324">North Europe</span></span> | <span data-ttu-id="9cfae-325">52.164.210.96</span><span class="sxs-lookup"><span data-stu-id="9cfae-325">52.164.210.96</span></span></br><span data-ttu-id="9cfae-326">13.74.153.132</span><span class="sxs-lookup"><span data-stu-id="9cfae-326">13.74.153.132</span></span> | <span data-ttu-id="9cfae-327">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-327">443</span></span> | <span data-ttu-id="9cfae-328">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-328">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-329">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="9cfae-329">West Europe</span></span>| <span data-ttu-id="9cfae-330">52.166.243.90</span><span class="sxs-lookup"><span data-stu-id="9cfae-330">52.166.243.90</span></span></br><span data-ttu-id="9cfae-331">52.174.36.244</span><span class="sxs-lookup"><span data-stu-id="9cfae-331">52.174.36.244</span></span> | <span data-ttu-id="9cfae-332">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-332">443</span></span> | <span data-ttu-id="9cfae-333">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-333">Inbound</span></span> |
    | <span data-ttu-id="9cfae-334">Německo</span><span class="sxs-lookup"><span data-stu-id="9cfae-334">Germany</span></span> | <span data-ttu-id="9cfae-335">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="9cfae-335">Germany Central</span></span> | <span data-ttu-id="9cfae-336">51.4.146.68</span><span class="sxs-lookup"><span data-stu-id="9cfae-336">51.4.146.68</span></span></br><span data-ttu-id="9cfae-337">51.4.146.80</span><span class="sxs-lookup"><span data-stu-id="9cfae-337">51.4.146.80</span></span> | <span data-ttu-id="9cfae-338">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-338">443</span></span> | <span data-ttu-id="9cfae-339">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-339">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-340">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="9cfae-340">Germany Northeast</span></span> | <span data-ttu-id="9cfae-341">51.5.150.132</span><span class="sxs-lookup"><span data-stu-id="9cfae-341">51.5.150.132</span></span></br><span data-ttu-id="9cfae-342">51.5.144.101</span><span class="sxs-lookup"><span data-stu-id="9cfae-342">51.5.144.101</span></span> | <span data-ttu-id="9cfae-343">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-343">443</span></span> | <span data-ttu-id="9cfae-344">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-344">Inbound</span></span> |
    | <span data-ttu-id="9cfae-345">Indie</span><span class="sxs-lookup"><span data-stu-id="9cfae-345">India</span></span> | <span data-ttu-id="9cfae-346">Střed Indie</span><span class="sxs-lookup"><span data-stu-id="9cfae-346">Central India</span></span> | <span data-ttu-id="9cfae-347">52.172.153.209</span><span class="sxs-lookup"><span data-stu-id="9cfae-347">52.172.153.209</span></span></br><span data-ttu-id="9cfae-348">52.172.152.49</span><span class="sxs-lookup"><span data-stu-id="9cfae-348">52.172.152.49</span></span> | <span data-ttu-id="9cfae-349">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-349">443</span></span> | <span data-ttu-id="9cfae-350">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-350">Inbound</span></span> |
    | <span data-ttu-id="9cfae-351">Japonsko</span><span class="sxs-lookup"><span data-stu-id="9cfae-351">Japan</span></span> | <span data-ttu-id="9cfae-352">Japonsko – východ</span><span class="sxs-lookup"><span data-stu-id="9cfae-352">Japan East</span></span> | <span data-ttu-id="9cfae-353">13.78.125.90</span><span class="sxs-lookup"><span data-stu-id="9cfae-353">13.78.125.90</span></span></br><span data-ttu-id="9cfae-354">13.78.89.60</span><span class="sxs-lookup"><span data-stu-id="9cfae-354">13.78.89.60</span></span> | <span data-ttu-id="9cfae-355">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-355">443</span></span> | <span data-ttu-id="9cfae-356">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-356">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-357">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="9cfae-357">Japan West</span></span> | <span data-ttu-id="9cfae-358">40.74.125.69</span><span class="sxs-lookup"><span data-stu-id="9cfae-358">40.74.125.69</span></span></br><span data-ttu-id="9cfae-359">138.91.29.150</span><span class="sxs-lookup"><span data-stu-id="9cfae-359">138.91.29.150</span></span> | <span data-ttu-id="9cfae-360">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-360">443</span></span> | <span data-ttu-id="9cfae-361">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-361">Inbound</span></span> |
    | <span data-ttu-id="9cfae-362">Korea</span><span class="sxs-lookup"><span data-stu-id="9cfae-362">Korea</span></span> | <span data-ttu-id="9cfae-363">Korea – střed</span><span class="sxs-lookup"><span data-stu-id="9cfae-363">Korea Central</span></span> | <span data-ttu-id="9cfae-364">52.231.39.142</span><span class="sxs-lookup"><span data-stu-id="9cfae-364">52.231.39.142</span></span></br><span data-ttu-id="9cfae-365">52.231.36.209</span><span class="sxs-lookup"><span data-stu-id="9cfae-365">52.231.36.209</span></span> | <span data-ttu-id="9cfae-366">433</span><span class="sxs-lookup"><span data-stu-id="9cfae-366">433</span></span> | <span data-ttu-id="9cfae-367">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-367">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-368">Korea – jih</span><span class="sxs-lookup"><span data-stu-id="9cfae-368">Korea South</span></span> | <span data-ttu-id="9cfae-369">52.231.203.16</span><span class="sxs-lookup"><span data-stu-id="9cfae-369">52.231.203.16</span></span></br><span data-ttu-id="9cfae-370">52.231.205.214</span><span class="sxs-lookup"><span data-stu-id="9cfae-370">52.231.205.214</span></span> | <span data-ttu-id="9cfae-371">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-371">443</span></span> | <span data-ttu-id="9cfae-372">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-372">Inbound</span></span>
    | <span data-ttu-id="9cfae-373">Spojené království</span><span class="sxs-lookup"><span data-stu-id="9cfae-373">United Kingdom</span></span> | <span data-ttu-id="9cfae-374">Spojené království – západ</span><span class="sxs-lookup"><span data-stu-id="9cfae-374">UK West</span></span> | <span data-ttu-id="9cfae-375">51.141.13.110</span><span class="sxs-lookup"><span data-stu-id="9cfae-375">51.141.13.110</span></span></br><span data-ttu-id="9cfae-376">51.141.7.20</span><span class="sxs-lookup"><span data-stu-id="9cfae-376">51.141.7.20</span></span> | <span data-ttu-id="9cfae-377">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-377">443</span></span> | <span data-ttu-id="9cfae-378">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-378">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-379">Spojené království – jih</span><span class="sxs-lookup"><span data-stu-id="9cfae-379">UK South</span></span> | <span data-ttu-id="9cfae-380">51.140.47.39</span><span class="sxs-lookup"><span data-stu-id="9cfae-380">51.140.47.39</span></span></br><span data-ttu-id="9cfae-381">51.140.52.16</span><span class="sxs-lookup"><span data-stu-id="9cfae-381">51.140.52.16</span></span> | <span data-ttu-id="9cfae-382">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-382">443</span></span> | <span data-ttu-id="9cfae-383">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-383">Inbound</span></span> |
    | <span data-ttu-id="9cfae-384">Spojené státy</span><span class="sxs-lookup"><span data-stu-id="9cfae-384">United States</span></span> | <span data-ttu-id="9cfae-385">Střed USA</span><span class="sxs-lookup"><span data-stu-id="9cfae-385">Central US</span></span> | <span data-ttu-id="9cfae-386">13.67.223.215</span><span class="sxs-lookup"><span data-stu-id="9cfae-386">13.67.223.215</span></span></br><span data-ttu-id="9cfae-387">40.86.83.253</span><span class="sxs-lookup"><span data-stu-id="9cfae-387">40.86.83.253</span></span> | <span data-ttu-id="9cfae-388">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-388">443</span></span> | <span data-ttu-id="9cfae-389">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-389">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-390">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="9cfae-390">North Central US</span></span> | <span data-ttu-id="9cfae-391">157.56.8.38</span><span class="sxs-lookup"><span data-stu-id="9cfae-391">157.56.8.38</span></span></br><span data-ttu-id="9cfae-392">157.55.213.99</span><span class="sxs-lookup"><span data-stu-id="9cfae-392">157.55.213.99</span></span> | <span data-ttu-id="9cfae-393">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-393">443</span></span> | <span data-ttu-id="9cfae-394">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-394">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-395">Západní střed USA</span><span class="sxs-lookup"><span data-stu-id="9cfae-395">West Central US</span></span> | <span data-ttu-id="9cfae-396">52.161.23.15</span><span class="sxs-lookup"><span data-stu-id="9cfae-396">52.161.23.15</span></span></br><span data-ttu-id="9cfae-397">52.161.10.167</span><span class="sxs-lookup"><span data-stu-id="9cfae-397">52.161.10.167</span></span> | <span data-ttu-id="9cfae-398">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-398">443</span></span> | <span data-ttu-id="9cfae-399">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-399">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="9cfae-400">Západní USA 2</span><span class="sxs-lookup"><span data-stu-id="9cfae-400">West US 2</span></span> | <span data-ttu-id="9cfae-401">52.175.211.210</span><span class="sxs-lookup"><span data-stu-id="9cfae-401">52.175.211.210</span></span></br><span data-ttu-id="9cfae-402">52.175.222.222</span><span class="sxs-lookup"><span data-stu-id="9cfae-402">52.175.222.222</span></span> | <span data-ttu-id="9cfae-403">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-403">443</span></span> | <span data-ttu-id="9cfae-404">Příchozí</span><span class="sxs-lookup"><span data-stu-id="9cfae-404">Inbound</span></span> |

    <span data-ttu-id="9cfae-405">Informace o hello IP adresy toouse pro Azure Government, najdete v části hello [Azure Government Intelligence + analýzy](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-405">For information on hello IP addresses toouse for Azure Government, see hello [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.</span></span>

3. <span data-ttu-id="9cfae-406">Pokud používáte vlastního serveru DNS s vaší virtuální síti, musíte také povolit přístup z __168.63.129.16__.</span><span class="sxs-lookup"><span data-stu-id="9cfae-406">If you use a custom DNS server with your virtual network, you must also allow access from __168.63.129.16__.</span></span> <span data-ttu-id="9cfae-407">Tato adresa se Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="9cfae-407">This address is Azure's recursive resolver.</span></span> <span data-ttu-id="9cfae-408">Další informace najdete v tématu hello [překlad názvů pro virtuální počítače a Role instance](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-408">For more information, see hello [Name resolution for VMs and Role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.</span></span>

<span data-ttu-id="9cfae-409">Další informace najdete v tématu hello [řízení síťového provozu](#networktraffic) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-409">For more information, see hello [Controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="9cfae-410"><a id="hdinsight-ports"></a>Požadované porty</span><span class="sxs-lookup"><span data-stu-id="9cfae-410"><a id="hdinsight-ports"></a> Required ports</span></span>

<span data-ttu-id="9cfae-411">Pokud máte v úmyslu používat síť **virtuální zařízení brány firewall** toosecure hello virtuální síť, musíte povolit odchozí přenosy na hello následující porty:</span><span class="sxs-lookup"><span data-stu-id="9cfae-411">If you plan on using a network **virtual appliance firewall** toosecure hello virtual network, you must allow outbound traffic on hello following ports:</span></span>

* <span data-ttu-id="9cfae-412">53</span><span class="sxs-lookup"><span data-stu-id="9cfae-412">53</span></span>
* <span data-ttu-id="9cfae-413">443</span><span class="sxs-lookup"><span data-stu-id="9cfae-413">443</span></span>
* <span data-ttu-id="9cfae-414">1433</span><span class="sxs-lookup"><span data-stu-id="9cfae-414">1433</span></span>
* <span data-ttu-id="9cfae-415">11000-11999</span><span class="sxs-lookup"><span data-stu-id="9cfae-415">11000-11999</span></span>
* <span data-ttu-id="9cfae-416">14000-14999</span><span class="sxs-lookup"><span data-stu-id="9cfae-416">14000-14999</span></span>

<span data-ttu-id="9cfae-417">Seznam portů pro určité služby, najdete v části hello [porty používané služby Hadoop v HDInsight](hdinsight-hadoop-port-settings-for-services.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-417">For a list of ports for specific services, see hello [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

<span data-ttu-id="9cfae-418">Další informace o pravidlech brány firewall pro virtuální zařízení, najdete v části hello [virtuální zařízení scénář](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-418">For more information on firewall rules for virtual appliances, see hello [virtual appliance scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.</span></span>

## <span data-ttu-id="9cfae-419"><a id="hdinsight-nsg"></a>Příklad: skupin zabezpečení sítě s HDInsight</span><span class="sxs-lookup"><span data-stu-id="9cfae-419"><a id="hdinsight-nsg"></a>Example: network security groups with HDInsight</span></span>

<span data-ttu-id="9cfae-420">Příklady Hello v této části ukazují, jak skupinu zabezpečení sítě toocreate pravidel, které umožňují služby Azure pro HDInsight toocommunicate s hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-420">hello examples in this section demonstrate how toocreate network security group rules that allow HDInsight toocommunicate with hello Azure management services.</span></span> <span data-ttu-id="9cfae-421">Před použitím hello příklady, upravte hello IP adresy toomatch hello ty, které jsou pro hello oblast Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="9cfae-421">Before using hello examples, adjust hello IP addresses toomatch hello ones for hello Azure region you are using.</span></span> <span data-ttu-id="9cfae-422">Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-422">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-resource-management-template"></a><span data-ttu-id="9cfae-423">Šablony Azure Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="9cfae-423">Azure Resource Management template</span></span>

<span data-ttu-id="9cfae-424">Hello následující Správa prostředků šablona vytvoří virtuální síť, která omezuje příchozí provoz, ale umožňuje přenos z hello IP adres vyžadují HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9cfae-424">hello following Resource Management template creates a virtual network that restricts inbound traffic, but allows traffic from hello IP addresses required by HDInsight.</span></span> <span data-ttu-id="9cfae-425">Tato šablona vytvoří cluster služby HDInsight také ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-425">This template also creates an HDInsight cluster in hello virtual network.</span></span>

* [<span data-ttu-id="9cfae-426">Nasazení zabezpečených virtuální síť Azure a cluster systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="9cfae-426">Deploy a secured Azure Virtual Network and an HDInsight Hadoop cluster</span></span>](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> <span data-ttu-id="9cfae-427">Změnit hello IP adresy použité v této příklad toomatch hello oblast Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="9cfae-427">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="9cfae-428">Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-428">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="9cfae-429">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9cfae-429">Azure PowerShell</span></span>

<span data-ttu-id="9cfae-430">Použijte následující toocreate skript prostředí PowerShell virtuální síť, která omezuje příchozí provoz a umožňuje provoz z hello IP adres pro oblast Severní Evropa hello hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-430">Use hello following PowerShell script toocreate a virtual network that restricts inbound traffic and allows traffic from hello IP addresses for hello North Europe region.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9cfae-431">Změnit hello IP adresy použité v této příklad toomatch hello oblast Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="9cfae-431">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="9cfae-432">Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-432">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with hello resource group hello virtual network is in"
$subnetName = "Replace with hello name of hello subnet that you plan toouse for HDInsight"
# Get hello Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get hello region hello Virtual network is in.
$location = $vnet.Location
# Get hello subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for hello HDInsight health and management services.
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
# Set hello changes toohello security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply hello NSG toohello subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> <span data-ttu-id="9cfae-433">Tento příklad ukazuje, jak tooadd pravidla tooallow příchozí přenosy na hello požadované IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9cfae-433">This example demonstrates how tooadd rules tooallow inbound traffic on hello required IP addresses.</span></span> <span data-ttu-id="9cfae-434">Neobsahuje toorestrict pravidlo příchozí přístup z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="9cfae-434">It does not contain a rule toorestrict inbound access from other sources.</span></span>
>
> <span data-ttu-id="9cfae-435">Hello následující příklad ukazuje, jak přistupovat k tooenable SSH ze hello Internetu:</span><span class="sxs-lookup"><span data-stu-id="9cfae-435">hello following example demonstrates how tooenable SSH access from hello Internet:</span></span>
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a><span data-ttu-id="9cfae-436">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9cfae-436">Azure CLI</span></span>

<span data-ttu-id="9cfae-437">Pomocí následujících kroků toocreate virtuální síť, která omezuje příchozí provoz, ale umožňuje přenos z hello IP adres vyžadují HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-437">Use hello following steps toocreate a virtual network that restricts inbound traffic, but allows traffic from hello IP addresses required by HDInsight.</span></span>

1. <span data-ttu-id="9cfae-438">Hello použijte následující příkaz toocreate novou skupinu zabezpečení sítě s názvem `hdisecure`.</span><span class="sxs-lookup"><span data-stu-id="9cfae-438">Use hello following command toocreate a new network security group named `hdisecure`.</span></span> <span data-ttu-id="9cfae-439">Nahraďte **RESOURCEGROUPNAME** s hello skupinu prostředků, který obsahuje hello Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="9cfae-439">Replace **RESOURCEGROUPNAME** with hello resource group that contains hello Azure Virtual Network.</span></span> <span data-ttu-id="9cfae-440">Nahraďte **umístění** s umístěním (oblastí), hello této skupiny hello byl vytvořen v.</span><span class="sxs-lookup"><span data-stu-id="9cfae-440">Replace **LOCATION** with hello location (region) that hello group was created in.</span></span>

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    <span data-ttu-id="9cfae-441">Po vytvoření skupiny hello zobrazí informace o nové skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-441">Once hello group has been created, you receive information on hello new group.</span></span>

2. <span data-ttu-id="9cfae-442">Použijte následující tooadd pravidla toohello novou skupinu zabezpečení sítě, povolit příchozí komunikaci na portu 443 z hello stavu a správu služby Azure HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-442">Use hello following tooadd rules toohello new network security group that allow inbound communication on port 443 from hello Azure HDInsight health and management service.</span></span> <span data-ttu-id="9cfae-443">Nahraďte **RESOURCEGROUPNAME** s názvem hello hello skupiny prostředků, která obsahuje hello Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="9cfae-443">Replace **RESOURCEGROUPNAME** with hello name of hello resource group that contains hello Azure Virtual Network.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9cfae-444">Změnit hello IP adresy použité v této příklad toomatch hello oblast Azure, který používáte.</span><span class="sxs-lookup"><span data-stu-id="9cfae-444">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="9cfae-445">Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-445">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. <span data-ttu-id="9cfae-446">tooretrieve hello jedinečný identifikátor pro tuto skupinu zabezpečení sítě, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-446">tooretrieve hello unique identifier for this network security group, use hello following command:</span></span>

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    <span data-ttu-id="9cfae-447">Tento příkaz vrátí hodnotu podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="9cfae-447">This command returns a value similar toohello following text:</span></span>

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    <span data-ttu-id="9cfae-448">Pokud neobdržíte hello očekávané výsledky, použijte dvojité uvozovky kolem id v příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-448">Use double-quotes around id in hello command if you don't get hello expected results.</span></span>

4. <span data-ttu-id="9cfae-449">Použijte následující příkaz tooapply hello zabezpečení skupiny tooa podsíti hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-449">Use hello following command tooapply hello network security group tooa subnet.</span></span> <span data-ttu-id="9cfae-450">Nahraďte hello __GUID__ a __RESOURCEGROUPNAME__ hodnoty s hello ty, které jsou vráceny z předchozího kroku hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-450">Replace hello __GUID__ and __RESOURCEGROUPNAME__ values with hello ones returned from hello previous step.</span></span> <span data-ttu-id="9cfae-451">Nahraďte __VNETNAME__ a __SUBNETNAME__ s hello název virtuální sítě a název podsítě, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="9cfae-451">Replace __VNETNAME__ and __SUBNETNAME__ with hello virtual network name and subnet name that you want toocreate.</span></span>

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    <span data-ttu-id="9cfae-452">Po dokončení tohoto příkazu můžete nainstalovat HDInsight do hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-452">Once this command completes, you can install HDInsight into hello Virtual Network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9cfae-453">Tyto kroky pouze otevřete přístup toohello stavu a správu Služba HDInsight na hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="9cfae-453">These steps only open access toohello HDInsight health and management service on hello Azure cloud.</span></span> <span data-ttu-id="9cfae-454">Všechny ostatní přístup toohello clusteru HDInsight z hello mimo virtuální síť je blokována.</span><span class="sxs-lookup"><span data-stu-id="9cfae-454">Any other access toohello HDInsight cluster from outside hello Virtual Network is blocked.</span></span> <span data-ttu-id="9cfae-455">tooenable přístup mimo hello virtuální sítě, musíte přidat další pravidla skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-455">tooenable access from outside hello virtual network, you must add additional Network Security Group rules.</span></span>
>
> <span data-ttu-id="9cfae-456">Hello následující příklad ukazuje, jak přistupovat k tooenable SSH ze hello Internetu:</span><span class="sxs-lookup"><span data-stu-id="9cfae-456">hello following example demonstrates how tooenable SSH access from hello Internet:</span></span>
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <span data-ttu-id="9cfae-457"><a id="example-dns"></a>Příklad: Konfigurace DNS</span><span class="sxs-lookup"><span data-stu-id="9cfae-457"><a id="example-dns"></a> Example: DNS configuration</span></span>

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a><span data-ttu-id="9cfae-458">Překlad mezi virtuální sítí a připojené místní sítě</span><span class="sxs-lookup"><span data-stu-id="9cfae-458">Name resolution between a virtual network and a connected on-premises network</span></span>

<span data-ttu-id="9cfae-459">Tento příklad vytvoří hello následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="9cfae-459">This example makes hello following assumptions:</span></span>

* <span data-ttu-id="9cfae-460">Máte virtuální síť Azure, který je připojený tooan místní síti pomocí brány VPN.</span><span class="sxs-lookup"><span data-stu-id="9cfae-460">You have an Azure Virtual Network that is connected tooan on-premises network using a VPN gateway.</span></span>

* <span data-ttu-id="9cfae-461">Hello vlastního serveru DNS ve virtuální síti hello běží jako hello operačního systému Linux nebo Unix.</span><span class="sxs-lookup"><span data-stu-id="9cfae-461">hello custom DNS server in hello virtual network is running Linux or Unix as hello operating system.</span></span>

* <span data-ttu-id="9cfae-462">[Vytvoření vazby](https://www.isc.org/downloads/bind/) je nainstalován na serveru DNS pro vlastní hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-462">[Bind](https://www.isc.org/downloads/bind/) is installed on hello custom DNS server.</span></span>

<span data-ttu-id="9cfae-463">Na hello vlastního serveru DNS ve virtuální síti hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-463">On hello custom DNS server in hello virtual network:</span></span>

1. <span data-ttu-id="9cfae-464">Použití Azure PowerShell nebo rozhraní příkazového řádku Azure toofind hello příponu DNS virtuální sítě hello:</span><span class="sxs-lookup"><span data-stu-id="9cfae-464">Use either Azure PowerShell or Azure CLI toofind hello DNS suffix of hello virtual network:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="9cfae-465">Na hello vlastního serveru DNS pro virtuální síť hello, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.local` souboru:</span><span class="sxs-lookup"><span data-stu-id="9cfae-465">On hello custom DNS server for hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.local` file:</span></span>

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    <span data-ttu-id="9cfae-466">Nahraďte hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hodnotu s hello příponu DNS virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-466">Replace hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with hello DNS suffix of your virtual network.</span></span>

    <span data-ttu-id="9cfae-467">Tato konfigurace směrovat všechny požadavky na DNS pro příponu DNS hello hello virtuální sítě toohello Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="9cfae-467">This configuration routes all DNS requests for hello DNS suffix of hello virtual network toohello Azure recursive resolver.</span></span>

2. <span data-ttu-id="9cfae-468">Na hello vlastního serveru DNS pro virtuální síť hello, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.options` souboru:</span><span class="sxs-lookup"><span data-stu-id="9cfae-468">On hello custom DNS server for hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients tooaccept requests from
    // TODO: Add hello IP range of hello joined network toothis list
    acl goodclients {
        10.0.0.0/16; # IP address range of hello virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent toohello following
            forwarders {
                192.168.0.1; # Replace with hello IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="9cfae-469">Nahraďte hello `10.0.0.0/16` hodnotu s hello rozsah IP adres vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-469">Replace hello `10.0.0.0/16` value with hello IP address range of your virtual network.</span></span> <span data-ttu-id="9cfae-470">Tato položka umožňuje název řešení požadavků adresy v tomto rozsahu.</span><span class="sxs-lookup"><span data-stu-id="9cfae-470">This entry allows name resolution requests addresses within this range.</span></span>

    * <span data-ttu-id="9cfae-471">Přidat rozsah IP adres hello místní sítě toohello hello `acl goodclients { ... }` části.</span><span class="sxs-lookup"><span data-stu-id="9cfae-471">Add hello IP address range of hello on-premises network toohello `acl goodclients { ... }` section.</span></span>  <span data-ttu-id="9cfae-472">položka umožňuje požadavky na rozlišení názvů z prostředků v hello do místní sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-472">entry allows name resolution requests from resources in hello on-premises network.</span></span>
    
    * <span data-ttu-id="9cfae-473">Nahradí hodnotu hello `192.168.0.1` hello IP adresu serveru DNS na místě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-473">Replace hello value `192.168.0.1` with hello IP address of your on-premises DNS server.</span></span> <span data-ttu-id="9cfae-474">Tato položka směrovat všechny požadavky toohello místní DNS servery DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-474">This entry routes all other DNS requests toohello on-premises DNS server.</span></span>

3. <span data-ttu-id="9cfae-475">Konfigurace hello toouse, restartujte vazby.</span><span class="sxs-lookup"><span data-stu-id="9cfae-475">toouse hello configuration, restart Bind.</span></span> <span data-ttu-id="9cfae-476">Například, `sudo service bind9 restart`.</span><span class="sxs-lookup"><span data-stu-id="9cfae-476">For example, `sudo service bind9 restart`.</span></span>

4. <span data-ttu-id="9cfae-477">Přidání serveru DNS pro podmíněné předávání toohello místně.</span><span class="sxs-lookup"><span data-stu-id="9cfae-477">Add a conditional forwarder toohello on-premises DNS server.</span></span> <span data-ttu-id="9cfae-478">Nakonfigurujte požadavky toosend pro podmíněné předávání hello hello přípony DNS z kroku 1 toohello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-478">Configure hello conditional forwarder toosend requests for hello DNS suffix from step 1 toohello custom DNS server.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9cfae-479">Dokumentaci hello softwaru DNS pro konkrétní na postupy pro tooadd pro podmíněné předávání.</span><span class="sxs-lookup"><span data-stu-id="9cfae-479">Consult hello documentation for your DNS software for specifics on how tooadd a conditional forwarder.</span></span>

<span data-ttu-id="9cfae-480">Po dokončení těchto kroků, se můžete připojit tooresources v síti, buď pomocí plně kvalifikovaných názvů domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="9cfae-480">After completing these steps, you can connect tooresources in either network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="9cfae-481">HDInsight můžete nainstalovat do virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-481">You can now install HDInsight into hello virtual network.</span></span>

### <a name="name-resolution-between-two-connected-virtual-networks"></a><span data-ttu-id="9cfae-482">Překlad mezi dvěma připojené virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="9cfae-482">Name resolution between two connected virtual networks</span></span>

<span data-ttu-id="9cfae-483">Tento příklad vytvoří hello následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="9cfae-483">This example makes hello following assumptions:</span></span>

* <span data-ttu-id="9cfae-484">Máte dvě virtuální sítě Azure, které jsou připojené pomocí brány VPN nebo partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="9cfae-484">You have two Azure Virtual Networks that are connected using either a VPN gateway or peering.</span></span>

* <span data-ttu-id="9cfae-485">Hello vlastního serveru DNS v obě sítě běží jako hello operačního systému Linux nebo Unix.</span><span class="sxs-lookup"><span data-stu-id="9cfae-485">hello custom DNS server in both networks is running Linux or Unix as hello operating system.</span></span>

* <span data-ttu-id="9cfae-486">[Vytvoření vazby](https://www.isc.org/downloads/bind/) je nainstalován na hello vlastní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-486">[Bind](https://www.isc.org/downloads/bind/) is installed on hello custom DNS servers.</span></span>

1. <span data-ttu-id="9cfae-487">Použití Azure PowerShell nebo rozhraní příkazového řádku Azure toofind hello příponu DNS obě virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="9cfae-487">Use either Azure PowerShell or Azure CLI toofind hello DNS suffix of both virtual networks:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="9cfae-488">Použití hello následující text jako hello obsah hello `/etc/bind/named.config.local` souboru na hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-488">Use hello following text as hello contents of hello `/etc/bind/named.config.local` file on hello custom DNS server.</span></span> <span data-ttu-id="9cfae-489">Tuto změnu proveďte na hello vlastního serveru DNS v obě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-489">Make this change on hello custom DNS server in both virtual networks.</span></span>

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello IP address of hello DNS server in hello other virtual network
    };
    ```

    <span data-ttu-id="9cfae-490">Nahraďte hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hodnotu s příponu DNS hello hello __jiných__ virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9cfae-490">Replace hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with hello DNS suffix of hello __other__ virtual network.</span></span> <span data-ttu-id="9cfae-491">Tato položka směruje požadavky pro příponu DNS hello hello vzdálených síťových toohello vlastní DNS v síti.</span><span class="sxs-lookup"><span data-stu-id="9cfae-491">This entry routes requests for hello DNS suffix of hello remote network toohello custom DNS in that network.</span></span>

3. <span data-ttu-id="9cfae-492">Na hello vlastní servery DNS v obě virtuální sítě, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.options` souboru:</span><span class="sxs-lookup"><span data-stu-id="9cfae-492">On hello custom DNS servers in both virtual networks, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients tooaccept requests from
    acl goodclients {
        10.1.0.0/16; # hello IP address range of one virtual network
        10.0.0.0/16; # hello IP address range of hello other virtual network
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

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="9cfae-493">Nahraďte hello `10.0.0.0/16` a `10.1.0.0/16` hodnoty s hello IP adres, rozsahy virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="9cfae-493">Replace hello `10.0.0.0/16` and `10.1.0.0/16` values with hello IP address ranges of your virtual networks.</span></span> <span data-ttu-id="9cfae-494">Tato položka umožňuje prostředky v každé sítě toomake požadavky hello serverů DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-494">This entry allows resources in each network toomake requests of hello DNS servers.</span></span>

    <span data-ttu-id="9cfae-495">Všechny žádosti, které nejsou pro přípony DNS hello hello virtuálních sítí (například microsoft.com) se zpracovává souborem hello Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="9cfae-495">Any requests that are not for hello DNS suffixes of hello virtual networks (for example, microsoft.com) is handled by hello Azure recursive resolver.</span></span>

4. <span data-ttu-id="9cfae-496">Konfigurace hello toouse, restartujte vazby.</span><span class="sxs-lookup"><span data-stu-id="9cfae-496">toouse hello configuration, restart Bind.</span></span> <span data-ttu-id="9cfae-497">Například `sudo service bind9 restart` na obou serverech DNS.</span><span class="sxs-lookup"><span data-stu-id="9cfae-497">For example, `sudo service bind9 restart` on both DNS servers.</span></span>

<span data-ttu-id="9cfae-498">Po dokončení těchto kroků, se můžete připojit tooresources ve virtuální síti hello pomocí plně kvalifikovaných názvů domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="9cfae-498">After completing these steps, you can connect tooresources in hello virtual network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="9cfae-499">HDInsight můžete nainstalovat do virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="9cfae-499">You can now install HDInsight into hello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cfae-500">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9cfae-500">Next steps</span></span>

* <span data-ttu-id="9cfae-501">Příklad začátku do konce konfigurace HDInsight tooconnect tooan do místní sítě, naleznete v části [připojit HDInsight tooan do místní sítě](./connect-on-premises-network.md).</span><span class="sxs-lookup"><span data-stu-id="9cfae-501">For an end-to-end example of configuring HDInsight tooconnect tooan on-premises network, see [Connect HDInsight tooan on-premises network](./connect-on-premises-network.md).</span></span>

* <span data-ttu-id="9cfae-502">Další informace o virtuálních sítí Azure, najdete v části hello [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9cfae-502">For more information on Azure virtual networks, see hello [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="9cfae-503">Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="9cfae-503">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="9cfae-504">Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9cfae-504">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>