---
title: "Připojit k místní síti - Azure HDInsight HDInsight | Microsoft Docs"
description: "Zjistěte, jak vytvořit cluster služby HDInsight ve virtuální síti Azure a připojte ho k síti na pracovišti. Zjistěte, jak nakonfigurovat překlad mezi HDInsight a místní sítí pomocí vlastního serveru DNS."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 6fc863010cc59e20e7d86ea9344489e574be75f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-hdinsight-to-your-on-premise-network"></a><span data-ttu-id="f5ce2-104">Připojit k místní síti HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5ce2-104">Connect HDInsight to your on-premise network</span></span>

<span data-ttu-id="f5ce2-105">Zjistěte, jak připojit HDInsight k místní síti pomocí virtuální sítě Azure a brány VPN.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-105">Learn how to connect HDInsight to your on-premises network by using Azure Virtual Networks and a VPN gateway.</span></span> <span data-ttu-id="f5ce2-106">Tento dokument obsahuje informace o plánování na:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-106">This document provides planning information on:</span></span>

* <span data-ttu-id="f5ce2-107">Pomocí HDInsight ve virtuální síti Azure, která se připojuje k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-107">Using HDInsight in an Azure Virtual Network that connects to your on-premises network.</span></span>

* <span data-ttu-id="f5ce2-108">Konfigurace překlad názvu DNS mezi virtuální sítí a v místní síti.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-108">Configuring DNS name resolution between the virtual network and your on-premises network.</span></span>

* <span data-ttu-id="f5ce2-109">Konfigurace skupin zabezpečení sítě pro omezení přístupu k Internetu do HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-109">Configuring network security groups to restrict internet access to HDInsight.</span></span>

* <span data-ttu-id="f5ce2-110">Porty poskytovaných v HDInsight ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-110">Ports provided by HDInsight on the virtual network.</span></span>

## <a name="create-the-virtual-network-configuration"></a><span data-ttu-id="f5ce2-111">Vytvoření konfigurace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f5ce2-111">Create the Virtual network configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f5ce2-112">Pokud hledáte pokyny krok za krokem k připojení HDInsight do místní sítě pomocí virtuální síť Azure, najdete v článku [HDInsight připojit k místní síti](connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-112">If you are looking for step by step guidance on connecting HDInsight to your on-premises network using an Azure Virtual Network, see the [Connect HDInsight to your on-premise network](connect-on-premises-network.md) document.</span></span>

<span data-ttu-id="f5ce2-113">Naučte se vytvořit virtuální síť Azure, která je připojena k místní síti pomocí v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-113">Use the following documents to learn how to create an Azure Virtual Network that is connected to your on-premises network:</span></span>
    
* [<span data-ttu-id="f5ce2-114">Pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5ce2-114">Using the Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [<span data-ttu-id="f5ce2-115">Použití Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="f5ce2-115">Using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [<span data-ttu-id="f5ce2-116">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f5ce2-116">Using Azure CLI</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a><span data-ttu-id="f5ce2-117">Konfigurování překladu</span><span class="sxs-lookup"><span data-stu-id="f5ce2-117">Configure name resolution</span></span>

<span data-ttu-id="f5ce2-118">Chcete-li povolit HDInsight a prostředky v připojené k síti komunikovat podle názvu, musíte provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-118">To allow HDInsight and resources in the joined network to communicate by name, you must perform the following actions:</span></span>

* <span data-ttu-id="f5ce2-119">Vytvoření vlastního serveru DNS ve virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-119">Create a custom DNS server in the Azure Virtual Network.</span></span>

* <span data-ttu-id="f5ce2-120">Konfigurace virtuální sítě pro použití vlastního serveru DNS místo výchozího Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-120">Configure the virtual network to use the custom DNS server instead of the default Azure Recursive Resolver.</span></span>

* <span data-ttu-id="f5ce2-121">Konfigurace předávání mezi vlastního serveru DNS a serveru DNS na místě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-121">Configure forwarding between the custom DNS server and your on-premises DNS server.</span></span>

<span data-ttu-id="f5ce2-122">Tato konfigurace umožňuje následující chování:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-122">This configuration enables the following behavior:</span></span>

* <span data-ttu-id="f5ce2-123">Požadavky pro plně kvalifikované názvy domény které mají příponu DNS __pro virtuální síť__ se předávají do vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-123">Requests for fully qualified domain names that have the DNS suffix __for the virtual network__ are forwarded to the custom DNS server.</span></span> <span data-ttu-id="f5ce2-124">Tyto požadavky vlastního serveru DNS potom předává do překladače rekurzivní Azure, která vrátí hodnotu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-124">The custom DNS server then forwards these requests to the Azure Recursive Resolver, which returns the IP address.</span></span>

* <span data-ttu-id="f5ce2-125">Všechny ostatní požadavky jsou předávány na místní server DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-125">All other requests are forwarded to the on-premises DNS server.</span></span> <span data-ttu-id="f5ce2-126">I požadavky na veřejné internetové prostředky, jako je například microsoft.com jsou předány na místním serveru DNS pro rozlišení názvu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-126">Even requests for public internet resources such as microsoft.com are forwarded to the on-premises DNS server for name resolution.</span></span>

<span data-ttu-id="f5ce2-127">Zelená řádky v následujícím diagramu jsou požadavky na prostředky, které končí příponou DNS virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-127">In the following diagram, green lines are requests for resources that end in the DNS suffix of the virtual network.</span></span> <span data-ttu-id="f5ce2-128">Modré řádky jsou požadavky na prostředky v místní síti nebo na veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-128">Blue lines are requests for resources in the on-premises network or on the public internet.</span></span>

![Diagram způsob řešení požadavky na DNS v konfiguraci v tomto dokumentu](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a><span data-ttu-id="f5ce2-130">Vytvoření vlastního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="f5ce2-130">Create a custom DNS server</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f5ce2-131">Musíte vytvořit a nakonfigurovat DNS server před instalací HDInsight do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-131">You must create and configure the DNS server before installing HDInsight into the virtual network.</span></span>

<span data-ttu-id="f5ce2-132">Chcete-li vytvořit virtuální počítač Linux, který používá [vazby](https://www.isc.org/downloads/bind/) DNS software, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-132">To create a Linux VM that uses the [Bind](https://www.isc.org/downloads/bind/) DNS software, use the following steps:</span></span>

> [!NOTE]
> <span data-ttu-id="f5ce2-133">Následující postup použijte [portál Azure](https://portal.azure.com) vytvořit virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-133">The following steps use the [Azure portal](https://portal.azure.com) to create an Azure Virtual Machine.</span></span> <span data-ttu-id="f5ce2-134">Další způsoby vytvoření virtuálního počítače, najdete v článku [vytvoření virtuálního počítače – rozhraní příkazového řádku Azure](../virtual-machines/linux/quick-create-cli.md) a [vytvoření virtuálního počítače - prostředí Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokumenty.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-134">For other ways to create a virtual machine, see the [Create VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) and [Create VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) documents.</span></span>

1. <span data-ttu-id="f5ce2-135">Z [portál Azure](https://portal.azure.com), vyberte  __+__ , __výpočetní__, a __Ubuntu Server 16.04 LTS__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-135">From the [Azure portal](https://portal.azure.com), select __+__, __Compute__, and __Ubuntu Server 16.04 LTS__.</span></span>

    ![Vytvoření virtuálního počítače Ubuntu](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. <span data-ttu-id="f5ce2-137">Z __Základy__ části, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-137">From the __Basics__ section, enter the following information:</span></span>

    * <span data-ttu-id="f5ce2-138">__Název__: popisný název, který identifikuje tento virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-138">__Name__: A friendly name that identifies this virtual machine.</span></span> <span data-ttu-id="f5ce2-139">Například __DNSProxy__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-139">For example, __DNSProxy__.</span></span>
    * <span data-ttu-id="f5ce2-140">__Uživatelské jméno__: název účtu SSH.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-140">__User name__: The name of the SSH account.</span></span>
    * <span data-ttu-id="f5ce2-141">__Veřejný klíč SSH__ nebo __heslo__: metody ověřování pro účet SSH.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-141">__SSH public key__ or __Password__: The authentication method for the SSH account.</span></span> <span data-ttu-id="f5ce2-142">Doporučujeme používat veřejných klíčů, protože se jedná o bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-142">We recommend using public keys, as they are more secure.</span></span> <span data-ttu-id="f5ce2-143">Další informace najdete v tématu [vytvoření a použití klíče SSH pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-143">For more information, see the [Create and use SSH keys for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md) document.</span></span>
    * <span data-ttu-id="f5ce2-144">__Skupina prostředků__: vyberte __použít existující__a pak vyberte skupinu prostředků, která obsahuje virtuální síť vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-144">__Resource group__: Select __Use existing__, and then select the resource group that contains the virtual network created earlier.</span></span>
    * <span data-ttu-id="f5ce2-145">__Umístění__: Vybrat stejné umístění jako virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-145">__Location__: Select the same location as the virtual network.</span></span>

    ![Základní konfigurace virtuálního počítače](./media/connect-on-premises-network/vm-basics.png)

    <span data-ttu-id="f5ce2-147">Nechte ostatní položky na výchozí hodnoty a potom vyberte __OK__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-147">Leave other entries at the default values and then select __OK__.</span></span>

3. <span data-ttu-id="f5ce2-148">Z __zvolte velikost__ vyberte velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-148">From the __Choose a size__ section, select the VM size.</span></span> <span data-ttu-id="f5ce2-149">V tomto kurzu vyberte možnost nejmenší a nejnižší náklady.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-149">For this tutorial, select the smallest and lowest cost option.</span></span> <span data-ttu-id="f5ce2-150">Chcete-li pokračovat, použijte __vyberte__ tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-150">To continue, use the __Select__ button.</span></span>

4. <span data-ttu-id="f5ce2-151">Z __nastavení__ části, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-151">From the __Settings__ section, enter the following information:</span></span>

    * <span data-ttu-id="f5ce2-152">__Virtuální síť__: vyberte virtuální síť, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-152">__Virtual network__: Select the virtual network that you created earlier.</span></span>

    * <span data-ttu-id="f5ce2-153">__Podsíť__: Vyberte výchozí podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-153">__Subnet__: Select the default subnet for the virtual network.</span></span> <span data-ttu-id="f5ce2-154">Proveďte __není__ vyberte podsíť používá bránu sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-154">Do __not__ select the subnet used by the VPN gateway.</span></span>

    * <span data-ttu-id="f5ce2-155">__Účet úložiště diagnostiky__: Vyberte existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-155">__Diagnostics storage account__: Either select an existing storage account or create a new one.</span></span>

    ![Nastavení virtuální sítě](./media/connect-on-premises-network/virtual-network-settings.png)

    <span data-ttu-id="f5ce2-157">Nechte ostatní položky na výchozí hodnotu a pak vyberte __OK__ pokračujte.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-157">Leave the other entries at the default value, then select __OK__ to continue.</span></span>

5. <span data-ttu-id="f5ce2-158">Z __nákupu__ vyberte __nákupu__ tlačítko pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-158">From the __Purchase__ section, select the __Purchase__ button to create the virtual machine.</span></span>

6. <span data-ttu-id="f5ce2-159">Po vytvoření virtuálního počítače, jeho __přehled__ části se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-159">Once the virtual machine has been created, its __Overview__ section is displayed.</span></span> <span data-ttu-id="f5ce2-160">V seznamu na levé straně vyberte __vlastnosti__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-160">From the list on the left, select __Properties__.</span></span> <span data-ttu-id="f5ce2-161">Uložit __veřejnou IP adresu__ a __privátní IP adresa__ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-161">Save the __Public IP address__ and __Private IP address__ values.</span></span> <span data-ttu-id="f5ce2-162">Použije se v další části.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-162">It will be used in the next section.</span></span>

    ![Veřejné a privátní IP adresy](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a><span data-ttu-id="f5ce2-164">Instalace a konfigurace vazby (DNS software)</span><span class="sxs-lookup"><span data-stu-id="f5ce2-164">Install and configure Bind (DNS software)</span></span>

1. <span data-ttu-id="f5ce2-165">Použití SSH se připojit k __veřejnou IP adresu__ virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-165">Use SSH to connect to the __public IP address__ of the virtual machine.</span></span> <span data-ttu-id="f5ce2-166">Následující příklad se připojí k virtuálnímu počítači na 40.68.254.142:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-166">The following example connects to a virtual machine at 40.68.254.142:</span></span>

    ```bash
    ssh sshuser@40.68.254.142
    ```

    <span data-ttu-id="f5ce2-167">Nahraďte `sshuser` k uživatelskému účtu SSH, který jste zadali při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-167">Replace `sshuser` with the SSH user account you specified when creating the cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5ce2-168">Existuje mnoho různých způsobů, jak získat `ssh` nástroj.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-168">There are a variety of ways to obtain the `ssh` utility.</span></span> <span data-ttu-id="f5ce2-169">Na systému Linux, Unix a systému macOS je poskytována jako součást operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-169">On Linux, Unix, and macOS, it is provided as part of the operating system.</span></span> <span data-ttu-id="f5ce2-170">Pokud používáte systém Windows, zvažte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-170">If you are using Windows, consider one of the following options:</span></span>
    >
    > * [<span data-ttu-id="f5ce2-171">Prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="f5ce2-171">Azure Cloud Shell</span></span>](../cloud-shell/quickstart.md)
    > * [<span data-ttu-id="f5ce2-172">Bash na Ubuntu na Windows 10</span><span class="sxs-lookup"><span data-stu-id="f5ce2-172">Bash on Ubuntu on Windows 10</span></span>](https://msdn.microsoft.com/commandline/wsl/about)
    > * [<span data-ttu-id="f5ce2-173">Git (https://git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="f5ce2-173">Git (https://git-scm.com/)</span></span>](https://git-scm.com/)
    > * [<span data-ttu-id="f5ce2-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span><span class="sxs-lookup"><span data-stu-id="f5ce2-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span></span>](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. <span data-ttu-id="f5ce2-175">Chcete-li nainstalovat vazby, použijte následující příkazy z relace SSH:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-175">To install Bind, use the following commands from the SSH session:</span></span>

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. <span data-ttu-id="f5ce2-176">Ke konfiguraci vazby k předávání žádostí o překlad názvu na místní server DNS, použijte následující text jako obsah `/etc/bind/named.conf.options` souboru:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-176">To configure Bind to forward name resolution requests to your on-prem DNS server, use the following text as the contents of the `/etc/bind/named.conf.options` file:</span></span>

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > <span data-ttu-id="f5ce2-177">Nahraďte hodnoty v `goodclients` části s použitým rozsahem IP adres virtuální sítě i místní sítě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-177">Replace the values in the `goodclients` section with the IP address range of the virtual network and on-premises network.</span></span> <span data-ttu-id="f5ce2-178">Tento oddíl definuje adresy, které tento server DNS přijímá požadavky od.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-178">This section defines the addresses that this DNS server accepts requests from.</span></span>
    >
    > <span data-ttu-id="f5ce2-179">Nahraďte `192.168.0.1` položku v `forwarders` část s IP adresou serveru DNS na místě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-179">Replace the `192.168.0.1` entry in the `forwarders` section with the IP address of your on-premises DNS server.</span></span> <span data-ttu-id="f5ce2-180">Tato položka směruje požadavky DNS na serveru místní DNS pro rozlišení.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-180">This entry routes DNS requests to your on-premises DNS server for resolution.</span></span>

    <span data-ttu-id="f5ce2-181">K úpravě tohoto souboru, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-181">To edit this file, use the following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    <span data-ttu-id="f5ce2-182">Chcete-li uložit soubor, použijte __Ctrl + X__, __Y__a potom __Enter__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-182">To save the file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

4. <span data-ttu-id="f5ce2-183">Z relace SSH použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-183">From the SSH session, use the following command:</span></span>

    ```bash
    hostname -f
    ```

    <span data-ttu-id="f5ce2-184">Tento příkaz vrátí hodnotu podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-184">This command returns a value similar to the following text:</span></span>

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    <span data-ttu-id="f5ce2-185">`icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` Text je __příponu DNS__ pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-185">The `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text is the __DNS suffix__ for this virtual network.</span></span> <span data-ttu-id="f5ce2-186">Tato hodnota, uložte, protože se později používá.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-186">Save this value, as it is used later.</span></span>

5. <span data-ttu-id="f5ce2-187">Ke konfiguraci vazby k překladu názvů DNS pro prostředky v rámci virtuální sítě, použijte následující text jako obsah `/etc/bind/named.conf.local` souboru:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-187">To configure Bind to resolve DNS names for resources within the virtual network, use the following text as the contents of the `/etc/bind/named.conf.local` file:</span></span>

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > <span data-ttu-id="f5ce2-188">Musíte `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` s příponou DNS, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-188">You must replace the `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` with the DNS suffix you retrieved earlier.</span></span>

    <span data-ttu-id="f5ce2-189">K úpravě tohoto souboru, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-189">To edit this file, use the following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    <span data-ttu-id="f5ce2-190">Chcete-li uložit soubor, použijte __Ctrl + X__, __Y__a potom __Enter__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-190">To save the file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

6. <span data-ttu-id="f5ce2-191">Pokud chcete spustit vazby, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-191">To start Bind, use the following command:</span></span>

    ```bash
    sudo service bind9 restart
    ```

7. <span data-ttu-id="f5ce2-192">Pokud chcete ověřit, že vazby může překládat názvy zdrojů v síti na pracovišti, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-192">To verify that bind can resolve the names of resources in your on-premises network, use the following commands:</span></span>

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="f5ce2-193">Nahraďte `dns.mynetwork.net` s plně kvalifikovaný název domény (FQDN) prostředku v síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-193">Replace `dns.mynetwork.net` with the fully qualified domain name (FQDN) of a resource in your on-premises network.</span></span>
    >
    > <span data-ttu-id="f5ce2-194">Nahraďte `10.0.0.4` s __interní IP adresu__ vašeho vlastního serveru DNS ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-194">Replace `10.0.0.4` with the __internal IP address__ of your custom DNS server in the virtual network.</span></span>

    <span data-ttu-id="f5ce2-195">Odpověď se zobrazí podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-195">The response appears similar to the following text:</span></span>

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a><span data-ttu-id="f5ce2-196">Konfigurace virtuální sítě pro použití vlastního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="f5ce2-196">Configure the virtual network to use the custom DNS server</span></span>

<span data-ttu-id="f5ce2-197">Ke konfiguraci virtuální sítě pro použití vlastního serveru DNS místo Azure rekurzivní překladač, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-197">To configure the virtual network to use the custom DNS server instead of the Azure recursive resolver, use the following steps:</span></span>

1. <span data-ttu-id="f5ce2-198">V [portál Azure](https://portal.azure.com), vyberte virtuální síť a potom vyberte __servery DNS__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-198">In the [Azure portal](https://portal.azure.com), select the virtual network, and then select __DNS Servers__.</span></span>

2. <span data-ttu-id="f5ce2-199">Vyberte __vlastní__a zadejte __interní IP adresu__ vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-199">Select __Custom__, and enter the __internal IP address__ of the custom DNS server.</span></span> <span data-ttu-id="f5ce2-200">Nakonec vyberte __Uložit__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-200">Finally, select __Save__.</span></span>

    ![Nastavení vlastního serveru DNS pro síť](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a><span data-ttu-id="f5ce2-202">Konfigurace serveru DNS na místě</span><span class="sxs-lookup"><span data-stu-id="f5ce2-202">Configure the on-premises DNS server</span></span>

<span data-ttu-id="f5ce2-203">V předchozí části že jste nakonfigurovali vlastního serveru DNS pro předávání požadavků na místním serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-203">In the previous section, you configured the custom DNS server to forward requests to the on-premises DNS server.</span></span> <span data-ttu-id="f5ce2-204">Dále musíte nakonfigurovat na místním serveru DNS pro předávání požadavků na vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-204">Next, you must configure the on-premises DNS server to forward requests to the custom DNS server.</span></span>

<span data-ttu-id="f5ce2-205">Konkrétní kroky pro konfiguraci serveru DNS najdete v dokumentaci pro software serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-205">For specific steps on how to configure your DNS server, consult the documentation for your DNS server software.</span></span> <span data-ttu-id="f5ce2-206">Vyhledejte kroky pro konfiguraci __pro podmíněné předávání__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-206">Look for the steps on how to configure a __conditional forwarder__.</span></span>

<span data-ttu-id="f5ce2-207">Podmíněné dopředného pouze předá požadavky pro konkrétní příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-207">A conditional forward only forwards requests for a specific DNS suffix.</span></span> <span data-ttu-id="f5ce2-208">V takovém případě musíte nakonfigurovat server pro předávání pro příponu DNS virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-208">In this case, you must configure a forwarder for the DNS suffix of the virtual network.</span></span> <span data-ttu-id="f5ce2-209">Požadavky pro tuto příponu předáte vlastního serveru DNS na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-209">Requests for this suffix should be forwarded to the IP address of the custom DNS server.</span></span> 

<span data-ttu-id="f5ce2-210">Tento text je příklad konfigurace pro podmíněné předávání pro **vazby** DNS softwaru:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-210">The following text is an example of a conditional forwarder configuration for the **Bind** DNS software:</span></span>

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

<span data-ttu-id="f5ce2-211">Informace o použití DNS na **systému Windows Server 2016**, najdete v článku [přidat DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentace...</span><span class="sxs-lookup"><span data-stu-id="f5ce2-211">For information on using DNS on **Windows Server 2016**, see the [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentation...</span></span>

<span data-ttu-id="f5ce2-212">Jakmile jste nakonfigurovali na místním serveru DNS, můžete použít `nslookup` z místní sítě chcete-li ověřit, zda je možné přeložit názvy ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-212">Once you have configured the on-premises DNS server, you can use `nslookup` from the on-premises network to verify that you can resolve names in the virtual network.</span></span> <span data-ttu-id="f5ce2-213">Následující příklad</span><span class="sxs-lookup"><span data-stu-id="f5ce2-213">The following example</span></span> 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

<span data-ttu-id="f5ce2-214">Tento příklad používá místní server DNS v 196.168.0.4 pro překlad názvu vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-214">This example uses the on-premises DNS server at 196.168.0.4 to resolve the name of the custom DNS server.</span></span> <span data-ttu-id="f5ce2-215">Nahraďte IP adresu pro místní server DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-215">Replace the IP address with the one for the on-premises DNS server.</span></span> <span data-ttu-id="f5ce2-216">Nahraďte `dnsproxy` adresa se plně kvalifikovaný název domény vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-216">Replace the `dnsproxy` address with the fully qualified domain name of the custom DNS server.</span></span>

## <a name="optional-control-network-traffic"></a><span data-ttu-id="f5ce2-217">Volitelné: Řízení síťového provozu</span><span class="sxs-lookup"><span data-stu-id="f5ce2-217">Optional: Control network traffic</span></span>

<span data-ttu-id="f5ce2-218">Skupiny zabezpečení sítě (NSG) nebo trasy definované uživatelem (UDR) můžete použít k řízení síťových přenosů.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-218">You can use network security groups (NSG) or user-defined routes (UDR) to control network traffic.</span></span> <span data-ttu-id="f5ce2-219">Skupiny Nsg umožňují filtrovat příchozí a odchozí přenosy a povolit nebo odepřít provoz.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-219">NSGs allow you to filter inbound and outbound traffic, and allow or deny the traffic.</span></span> <span data-ttu-id="f5ce2-220">Udr umožňují řídit tok přenosů mezi prostředky ve virtuální síti, internet a místní sítě.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-220">UDRs allow you to control how traffic flows between resources in the virtual network, the internet, and the on-premises network.</span></span>

> [!WARNING]
> <span data-ttu-id="f5ce2-221">HDInsight vyžaduje příchozí přístup z konkrétní IP adresy v cloudu Azure a neomezený přístup pro odchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-221">HDInsight requires inbound access from specific IP addresses in the Azure cloud, and unrestricted outbound access.</span></span> <span data-ttu-id="f5ce2-222">Pokud používáte skupiny Nsg nebo udr k řízení provozu, je třeba provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-222">When using NSGs or UDRs to control traffic, you must perform the following steps:</span></span>
>
> 1. <span data-ttu-id="f5ce2-223">Najít IP adresy pro umístění, které obsahuje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-223">Find the IP addresses for the location that contains your virtual network.</span></span> <span data-ttu-id="f5ce2-224">Seznam požadované IP adresy podle umístění najdete v tématu [požadované IP adresy](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span><span class="sxs-lookup"><span data-stu-id="f5ce2-224">For a list of required IPs by location, see [Required IP addresses](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span></span>
>
> 2. <span data-ttu-id="f5ce2-225">Povolí příchozí provoz z IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-225">Allow inbound traffic from the IP addresses.</span></span>
>
>    * <span data-ttu-id="f5ce2-226">__Skupina NSG__: Povolit __příchozí__ přenosy na portu __443__ z __Internet__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-226">__NSG__: Allow __inbound__ traffic on port __443__ from the __Internet__.</span></span>
>    * <span data-ttu-id="f5ce2-227">__UDR__: nastavte __další směrování__ typ trasy k __Internet__.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-227">__UDR__: Set the __Next Hop__ type of the route to __Internet__.</span></span>

<span data-ttu-id="f5ce2-228">Příklad použití Azure PowerShell nebo rozhraní příkazového řádku Azure k vytvoření skupin Nsg, naleznete v části [rozšířit HDInsight s virtuálními sítěmi Azure](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-228">For an example of using Azure PowerShell or the Azure CLI to create NSGs, see the [Extend HDInsight with Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.</span></span>

## <a name="create-the-hdinsight-cluster"></a><span data-ttu-id="f5ce2-229">Vytvoření clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5ce2-229">Create the HDInsight cluster</span></span>

> [!WARNING]
> <span data-ttu-id="f5ce2-230">Před instalací HDInsight ve virtuální síti musíte nakonfigurovat vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-230">You must configure the custom DNS server before installing HDInsight in the virtual network.</span></span>

<span data-ttu-id="f5ce2-231">Postupujte podle kroků v [vytvoření clusteru HDInsight pomocí portálu Azure](./hdinsight-hadoop-create-linux-clusters-portal.md) dokument k vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-231">Use the steps in the [Create an HDInsight cluster using the Azure portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document to create an HDInsight cluster.</span></span>

> [!WARNING]
> * <span data-ttu-id="f5ce2-232">Při vytváření clusteru musíte zvolit umístění, které obsahuje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-232">During cluster creation, you must choose the location that contains your virtual network.</span></span>
>
> * <span data-ttu-id="f5ce2-233">V __upřesňující nastavení__ část konfigurace, musíte vybrat virtuální síť a podsíť, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-233">In the __Advanced settings__ part of configuration, you must select the virtual network and subnet that you created earlier.</span></span>

## <a name="connecting-to-hdinsight"></a><span data-ttu-id="f5ce2-234">Připojení k HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5ce2-234">Connecting to HDInsight</span></span>

<span data-ttu-id="f5ce2-235">Většina dokumentace v HDInsight předpokládá, že máte přístup ke clusteru přes internet.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-235">Most documentation on HDInsight assumes that you have access to the cluster over the internet.</span></span> <span data-ttu-id="f5ce2-236">Pro příklad, který můžete připojit ke clusteru v https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-236">For example, that you can connect to the cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="f5ce2-237">Tato adresa se používá veřejný brány, která není k dispozici, pokud jste použili skupiny Nsg nebo udr k omezení přístupu z Internetu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-237">This address uses the public gateway, which is not available if you have used NSGs or UDRs to restrict access from the internet.</span></span>

<span data-ttu-id="f5ce2-238">K přímému připojení k HDInsight prostřednictvím virtuální sítě, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-238">To directly connect to HDInsight through the virtual network, use the following steps:</span></span>

1. <span data-ttu-id="f5ce2-239">Pokud chcete zjistit, interní plně kvalifikované názvy domény uzlů clusteru HDInsight, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="f5ce2-239">To discover the internal fully qualified domain names of the HDInsight cluster nodes, use one of the following methods:</span></span>

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

2. <span data-ttu-id="f5ce2-240">Ke zjištění portu, která je dostupná na služby, najdete v článku [porty používané služby Hadoop v HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-240">To determine the port that a service is available on, see the [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f5ce2-241">Některé služby hostované o hlavních uzlech aktivní pouze na jednom uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-241">Some services hosted on the head nodes are only active on one node at a time.</span></span> <span data-ttu-id="f5ce2-242">Pokud se pokusíte přístup k službě jeden hlavního uzlu a ona selže, přepněte do jiného hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-242">If you try accessing a service on one head node and it fails, switch to the other head node.</span></span>
    >
    > <span data-ttu-id="f5ce2-243">Například Ambari je aktivní pouze jeden hlavního uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-243">For example, Ambari is only active on one head node at a time.</span></span> <span data-ttu-id="f5ce2-244">Pokud se pokusíte přístup k Ambari na jeden hlavní uzel a vrátí chybu 404, je spuštěna z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="f5ce2-244">If you try accessing Ambari on one head node and it returns a 404 error, then it is running on the other head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5ce2-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5ce2-245">Next steps</span></span>

* <span data-ttu-id="f5ce2-246">Další informace o používání HDInsight ve virtuální síti, najdete v části [rozšířit HDInsight pomocí virtuálních sítí Azure](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="f5ce2-246">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

* <span data-ttu-id="f5ce2-247">Další informace o virtuálních sítí Azure, najdete v článku [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5ce2-247">For more information on Azure virtual networks, see the [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="f5ce2-248">Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="f5ce2-248">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="f5ce2-249">Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5ce2-249">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>
