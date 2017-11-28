---
title: "místní síť aaaConnect HDInsight tooyour – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate HDInsight cluster v Azure Virtual Network a připojte ho tooyour do místní sítě. Zjistěte, jak tooconfigure překlad mezi HDInsight a místní sítí pomocí vlastního serveru DNS."
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
ms.openlocfilehash: 8a3adf0e3df7726d8e6566d723700506baaf89a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-hdinsight-tooyour-on-premise-network"></a><span data-ttu-id="e83bb-104">Připojení HDInsight tooyour místní sítě</span><span class="sxs-lookup"><span data-stu-id="e83bb-104">Connect HDInsight tooyour on-premise network</span></span>

<span data-ttu-id="e83bb-105">Zjistěte, jak tooconnect HDInsight tooyour místní sítě pomocí virtuální sítě Azure a brány VPN.</span><span class="sxs-lookup"><span data-stu-id="e83bb-105">Learn how tooconnect HDInsight tooyour on-premises network by using Azure Virtual Networks and a VPN gateway.</span></span> <span data-ttu-id="e83bb-106">Tento dokument obsahuje informace o plánování na:</span><span class="sxs-lookup"><span data-stu-id="e83bb-106">This document provides planning information on:</span></span>

* <span data-ttu-id="e83bb-107">Pomocí HDInsight ve virtuální síti Azure, která se připojuje tooyour místní sítě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-107">Using HDInsight in an Azure Virtual Network that connects tooyour on-premises network.</span></span>

* <span data-ttu-id="e83bb-108">Konfigurace překladu názvů DNS mezi hello virtuální sítě a v místní síti.</span><span class="sxs-lookup"><span data-stu-id="e83bb-108">Configuring DNS name resolution between hello virtual network and your on-premises network.</span></span>

* <span data-ttu-id="e83bb-109">Konfigurace sítě zabezpečení skupiny toorestrict internet přístup tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="e83bb-109">Configuring network security groups toorestrict internet access tooHDInsight.</span></span>

* <span data-ttu-id="e83bb-110">Porty poskytovaných v HDInsight ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-110">Ports provided by HDInsight on hello virtual network.</span></span>

## <a name="create-hello-virtual-network-configuration"></a><span data-ttu-id="e83bb-111">Vytvoření konfigurace virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="e83bb-111">Create hello Virtual network configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e83bb-112">Pokud hledáte pokyny krok za krokem k připojování HDInsight tooyour místní sítě pomocí virtuální síť Azure, naleznete v tématu hello [připojit HDInsight tooyour místní sítě](connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-112">If you are looking for step by step guidance on connecting HDInsight tooyour on-premises network using an Azure Virtual Network, see hello [Connect HDInsight tooyour on-premise network](connect-on-premises-network.md) document.</span></span>

<span data-ttu-id="e83bb-113">Použití hello následující dokumenty toolearn jak toocreate virtuální síť Azure, který je připojený tooyour místní sítě:</span><span class="sxs-lookup"><span data-stu-id="e83bb-113">Use hello following documents toolearn how toocreate an Azure Virtual Network that is connected tooyour on-premises network:</span></span>
    
* [<span data-ttu-id="e83bb-114">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e83bb-114">Using hello Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [<span data-ttu-id="e83bb-115">Použití Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="e83bb-115">Using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [<span data-ttu-id="e83bb-116">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e83bb-116">Using Azure CLI</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a><span data-ttu-id="e83bb-117">Konfigurování překladu</span><span class="sxs-lookup"><span data-stu-id="e83bb-117">Configure name resolution</span></span>

<span data-ttu-id="e83bb-118">tooallow HDInsight a prostředky v toocommunicate hello připojený k síti podle názvu, je třeba provést hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="e83bb-118">tooallow HDInsight and resources in hello joined network toocommunicate by name, you must perform hello following actions:</span></span>

* <span data-ttu-id="e83bb-119">Vytvoření vlastního serveru DNS v hello Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="e83bb-119">Create a custom DNS server in hello Azure Virtual Network.</span></span>

* <span data-ttu-id="e83bb-120">Nakonfigurujte hello virtuální sítě toouse hello vlastního serveru DNS místo výchozí hello Azure rekurzivní překladač.</span><span class="sxs-lookup"><span data-stu-id="e83bb-120">Configure hello virtual network toouse hello custom DNS server instead of hello default Azure Recursive Resolver.</span></span>

* <span data-ttu-id="e83bb-121">Konfigurace předávání mezi hello vlastního serveru DNS a serveru DNS na místě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-121">Configure forwarding between hello custom DNS server and your on-premises DNS server.</span></span>

<span data-ttu-id="e83bb-122">Tato konfigurace umožňuje hello následující chování:</span><span class="sxs-lookup"><span data-stu-id="e83bb-122">This configuration enables hello following behavior:</span></span>

* <span data-ttu-id="e83bb-123">Požadavky pro plně kvalifikované názvy domény které mají příponu DNS hello __pro virtuální síť hello__ se předávají toohello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-123">Requests for fully qualified domain names that have hello DNS suffix __for hello virtual network__ are forwarded toohello custom DNS server.</span></span> <span data-ttu-id="e83bb-124">Hello vlastního serveru DNS potom předává tyto požadavky toohello rekurzivní překladač Azure, která vrátí hodnotu hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-124">hello custom DNS server then forwards these requests toohello Azure Recursive Resolver, which returns hello IP address.</span></span>

* <span data-ttu-id="e83bb-125">Všechny ostatní požadavky jsou předávány toohello místní DNS server.</span><span class="sxs-lookup"><span data-stu-id="e83bb-125">All other requests are forwarded toohello on-premises DNS server.</span></span> <span data-ttu-id="e83bb-126">I požadavky na veřejné internetové prostředky, jako je například microsoft.com jsou předávány toohello na místním serveru DNS pro rozlišení názvu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-126">Even requests for public internet resources such as microsoft.com are forwarded toohello on-premises DNS server for name resolution.</span></span>

<span data-ttu-id="e83bb-127">Zelená řádky v následujícím diagramu hello, jsou požadavky na prostředky, které končí příponu DNS hello hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-127">In hello following diagram, green lines are requests for resources that end in hello DNS suffix of hello virtual network.</span></span> <span data-ttu-id="e83bb-128">Modré řádky jsou požadavky na prostředky v místní síti hello nebo na hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-128">Blue lines are requests for resources in hello on-premises network or on hello public internet.</span></span>

![Diagram způsob řešení požadavky na DNS v hello konfigurace použitá v tomto dokumentu](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a><span data-ttu-id="e83bb-130">Vytvoření vlastního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="e83bb-130">Create a custom DNS server</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e83bb-131">Musíte vytvořit a nakonfigurovat hello DNS server před instalací HDInsight do virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-131">You must create and configure hello DNS server before installing HDInsight into hello virtual network.</span></span>

<span data-ttu-id="e83bb-132">toocreate Linux virtuálního počítače, který používá hello [vazby](https://www.isc.org/downloads/bind/) DNS softwaru, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e83bb-132">toocreate a Linux VM that uses hello [Bind](https://www.isc.org/downloads/bind/) DNS software, use hello following steps:</span></span>

> [!NOTE]
> <span data-ttu-id="e83bb-133">Hello následující postup použijte hello [portál Azure](https://portal.azure.com) toocreate virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="e83bb-133">hello following steps use hello [Azure portal](https://portal.azure.com) toocreate an Azure Virtual Machine.</span></span> <span data-ttu-id="e83bb-134">Jiné způsoby toocreate virtuálního počítače najdete v části hello [vytvoření virtuálního počítače – rozhraní příkazového řádku Azure](../virtual-machines/linux/quick-create-cli.md) a [vytvoření virtuálního počítače - prostředí Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokumenty.</span><span class="sxs-lookup"><span data-stu-id="e83bb-134">For other ways toocreate a virtual machine, see hello [Create VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) and [Create VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) documents.</span></span>

1. <span data-ttu-id="e83bb-135">Z hello [portál Azure](https://portal.azure.com), vyberte  __+__ , __výpočetní__, a __Ubuntu Server 16.04 LTS__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-135">From hello [Azure portal](https://portal.azure.com), select __+__, __Compute__, and __Ubuntu Server 16.04 LTS__.</span></span>

    ![Vytvoření virtuálního počítače Ubuntu](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. <span data-ttu-id="e83bb-137">Z hello __Základy__ zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e83bb-137">From hello __Basics__ section, enter hello following information:</span></span>

    * <span data-ttu-id="e83bb-138">__Název__: popisný název, který identifikuje tento virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e83bb-138">__Name__: A friendly name that identifies this virtual machine.</span></span> <span data-ttu-id="e83bb-139">Například __DNSProxy__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-139">For example, __DNSProxy__.</span></span>
    * <span data-ttu-id="e83bb-140">__Uživatelské jméno__: název hello hello účtu SSH.</span><span class="sxs-lookup"><span data-stu-id="e83bb-140">__User name__: hello name of hello SSH account.</span></span>
    * <span data-ttu-id="e83bb-141">__Veřejný klíč SSH__ nebo __heslo__: hello metody ověřování pro hello účtu SSH.</span><span class="sxs-lookup"><span data-stu-id="e83bb-141">__SSH public key__ or __Password__: hello authentication method for hello SSH account.</span></span> <span data-ttu-id="e83bb-142">Doporučujeme používat veřejných klíčů, protože se jedná o bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="e83bb-142">We recommend using public keys, as they are more secure.</span></span> <span data-ttu-id="e83bb-143">Další informace najdete v tématu hello [vytvoření a použití klíče SSH pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-143">For more information, see hello [Create and use SSH keys for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md) document.</span></span>
    * <span data-ttu-id="e83bb-144">__Skupina prostředků__: vyberte __použít existující__a pak vyberte skupinu prostředků hello, který obsahuje hello virtuální sítě vytvořené dříve.</span><span class="sxs-lookup"><span data-stu-id="e83bb-144">__Resource group__: Select __Use existing__, and then select hello resource group that contains hello virtual network created earlier.</span></span>
    * <span data-ttu-id="e83bb-145">__Umístění__: Vyberte hello stejné umístění jako virtuální síť hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-145">__Location__: Select hello same location as hello virtual network.</span></span>

    ![Základní konfigurace virtuálního počítače](./media/connect-on-premises-network/vm-basics.png)

    <span data-ttu-id="e83bb-147">Jiné položky v hello nechte výchozí hodnoty a potom vyberte __OK__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-147">Leave other entries at hello default values and then select __OK__.</span></span>

3. <span data-ttu-id="e83bb-148">Z hello __zvolte velikost__ část, vyberte hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e83bb-148">From hello __Choose a size__ section, select hello VM size.</span></span> <span data-ttu-id="e83bb-149">V tomto kurzu vyberte hello nejmenší a nejnižší náklady možnost.</span><span class="sxs-lookup"><span data-stu-id="e83bb-149">For this tutorial, select hello smallest and lowest cost option.</span></span> <span data-ttu-id="e83bb-150">toocontinue, použijte hello __vyberte__ tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e83bb-150">toocontinue, use hello __Select__ button.</span></span>

4. <span data-ttu-id="e83bb-151">Z hello __nastavení__ zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e83bb-151">From hello __Settings__ section, enter hello following information:</span></span>

    * <span data-ttu-id="e83bb-152">__Virtuální síť__: Vyberte hello virtuální síť, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e83bb-152">__Virtual network__: Select hello virtual network that you created earlier.</span></span>

    * <span data-ttu-id="e83bb-153">__Podsíť__: Vyberte výchozí hello podsíť pro virtuální síť hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-153">__Subnet__: Select hello default subnet for hello virtual network.</span></span> <span data-ttu-id="e83bb-154">Proveďte __není__ vyberte hello podsítě používané hello VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="e83bb-154">Do __not__ select hello subnet used by hello VPN gateway.</span></span>

    * <span data-ttu-id="e83bb-155">__Účet úložiště diagnostiky__: Vyberte existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="e83bb-155">__Diagnostics storage account__: Either select an existing storage account or create a new one.</span></span>

    ![Nastavení virtuální sítě](./media/connect-on-premises-network/virtual-network-settings.png)

    <span data-ttu-id="e83bb-157">Nechat hello jiné položky v hello výchozí hodnotu a pak vyberte __OK__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="e83bb-157">Leave hello other entries at hello default value, then select __OK__ toocontinue.</span></span>

5. <span data-ttu-id="e83bb-158">Z hello __nákupu__ části, vyberte hello __nákupu__ tlačítko toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e83bb-158">From hello __Purchase__ section, select hello __Purchase__ button toocreate hello virtual machine.</span></span>

6. <span data-ttu-id="e83bb-159">Po vytvoření virtuálního počítače hello jeho __přehled__ části se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e83bb-159">Once hello virtual machine has been created, its __Overview__ section is displayed.</span></span> <span data-ttu-id="e83bb-160">Hello seznamu na levé straně hello vyberte __vlastnosti__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-160">From hello list on hello left, select __Properties__.</span></span> <span data-ttu-id="e83bb-161">Uložit hello __veřejnou IP adresu__ a __privátní IP adresa__ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e83bb-161">Save hello __Public IP address__ and __Private IP address__ values.</span></span> <span data-ttu-id="e83bb-162">Použije se v další části hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-162">It will be used in hello next section.</span></span>

    ![Veřejné a privátní IP adresy](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a><span data-ttu-id="e83bb-164">Instalace a konfigurace vazby (DNS software)</span><span class="sxs-lookup"><span data-stu-id="e83bb-164">Install and configure Bind (DNS software)</span></span>

1. <span data-ttu-id="e83bb-165">Použití SSH tooconnect toohello __veřejnou IP adresu__ hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e83bb-165">Use SSH tooconnect toohello __public IP address__ of hello virtual machine.</span></span> <span data-ttu-id="e83bb-166">Následující ukázka Hello připojí tooa virtuálního počítače v 40.68.254.142:</span><span class="sxs-lookup"><span data-stu-id="e83bb-166">hello following example connects tooa virtual machine at 40.68.254.142:</span></span>

    ```bash
    ssh sshuser@40.68.254.142
    ```

    <span data-ttu-id="e83bb-167">Nahraďte `sshuser` s hello SSH uživatelský účet, jste zadali při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-167">Replace `sshuser` with hello SSH user account you specified when creating hello cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e83bb-168">Existují různé způsoby tooobtain hello `ssh` nástroj.</span><span class="sxs-lookup"><span data-stu-id="e83bb-168">There are a variety of ways tooobtain hello `ssh` utility.</span></span> <span data-ttu-id="e83bb-169">Na systému Linux, Unix a systému macOS je poskytována jako součást hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e83bb-169">On Linux, Unix, and macOS, it is provided as part of hello operating system.</span></span> <span data-ttu-id="e83bb-170">Pokud používáte systém Windows, zvažte jednu hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="e83bb-170">If you are using Windows, consider one of hello following options:</span></span>
    >
    > * [<span data-ttu-id="e83bb-171">Prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="e83bb-171">Azure Cloud Shell</span></span>](../cloud-shell/quickstart.md)
    > * [<span data-ttu-id="e83bb-172">Bash na Ubuntu na Windows 10</span><span class="sxs-lookup"><span data-stu-id="e83bb-172">Bash on Ubuntu on Windows 10</span></span>](https://msdn.microsoft.com/commandline/wsl/about)
    > * [<span data-ttu-id="e83bb-173">Git (https://git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="e83bb-173">Git (https://git-scm.com/)</span></span>](https://git-scm.com/)
    > * [<span data-ttu-id="e83bb-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span><span class="sxs-lookup"><span data-stu-id="e83bb-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span></span>](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. <span data-ttu-id="e83bb-175">tooinstall vazby, použijte následující příkazy z relace SSH hello hello:</span><span class="sxs-lookup"><span data-stu-id="e83bb-175">tooinstall Bind, use hello following commands from hello SSH session:</span></span>

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. <span data-ttu-id="e83bb-176">tooconfigure vazby tooforward název řešení požadavky tooyour místní server DNS, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.options` souboru:</span><span class="sxs-lookup"><span data-stu-id="e83bb-176">tooconfigure Bind tooforward name resolution requests tooyour on-prem DNS server, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

        acl goodclients {
            10.0.0.0/16; # Replace with hello IP address range of hello virtual network
            10.1.0.0/16; # Replace with hello IP address range of hello on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with hello IP address of hello on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform tooRFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > <span data-ttu-id="e83bb-177">Nahraďte hodnoty hello v hello `goodclients` oddíl s rozsah IP adres hello hello virtuální sítě a místní sítě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-177">Replace hello values in hello `goodclients` section with hello IP address range of hello virtual network and on-premises network.</span></span> <span data-ttu-id="e83bb-178">Tento oddíl definuje hello adresy, které tento server DNS přijímá požadavky od.</span><span class="sxs-lookup"><span data-stu-id="e83bb-178">This section defines hello addresses that this DNS server accepts requests from.</span></span>
    >
    > <span data-ttu-id="e83bb-179">Nahraďte hello `192.168.0.1` položku v hello `forwarders` oddíl s hello IP adresu serveru DNS na místě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-179">Replace hello `192.168.0.1` entry in hello `forwarders` section with hello IP address of your on-premises DNS server.</span></span> <span data-ttu-id="e83bb-180">Tato položka trasy DNS požadavky tooyour místní server DNS pro rozlišení.</span><span class="sxs-lookup"><span data-stu-id="e83bb-180">This entry routes DNS requests tooyour on-premises DNS server for resolution.</span></span>

    <span data-ttu-id="e83bb-181">tooedit tento soubor hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e83bb-181">tooedit this file, use hello following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    <span data-ttu-id="e83bb-182">toosave hello soubor, použijte __Ctrl + X__, __Y__a potom __Enter__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-182">toosave hello file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

4. <span data-ttu-id="e83bb-183">Z relace SSH hello použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e83bb-183">From hello SSH session, use hello following command:</span></span>

    ```bash
    hostname -f
    ```

    <span data-ttu-id="e83bb-184">Tento příkaz vrátí hodnotu podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="e83bb-184">This command returns a value similar toohello following text:</span></span>

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    <span data-ttu-id="e83bb-185">Hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text je hello __příponu DNS__ pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e83bb-185">hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text is hello __DNS suffix__ for this virtual network.</span></span> <span data-ttu-id="e83bb-186">Tato hodnota, uložte, protože se později používá.</span><span class="sxs-lookup"><span data-stu-id="e83bb-186">Save this value, as it is used later.</span></span>

5. <span data-ttu-id="e83bb-187">tooconfigure vazby tooresolve názvy DNS pro prostředky v rámci hello virtuální sítě, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.local` souboru:</span><span class="sxs-lookup"><span data-stu-id="e83bb-187">tooconfigure Bind tooresolve DNS names for resources within hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.local` file:</span></span>

        // Replace hello following with hello DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # hello Azure recursive resolver
        };

    > [!IMPORTANT]
    > <span data-ttu-id="e83bb-188">Je třeba nahradit hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` s příponou hello DNS, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="e83bb-188">You must replace hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` with hello DNS suffix you retrieved earlier.</span></span>

    <span data-ttu-id="e83bb-189">tooedit tento soubor hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e83bb-189">tooedit this file, use hello following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    <span data-ttu-id="e83bb-190">toosave hello soubor, použijte __Ctrl + X__, __Y__a potom __Enter__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-190">toosave hello file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

6. <span data-ttu-id="e83bb-191">toostart vazby, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e83bb-191">toostart Bind, use hello following command:</span></span>

    ```bash
    sudo service bind9 restart
    ```

7. <span data-ttu-id="e83bb-192">tooverify, který vazbu můžete vyřešit hello názvy zdrojů v síti na pracovišti, hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e83bb-192">tooverify that bind can resolve hello names of resources in your on-premises network, use hello following commands:</span></span>

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="e83bb-193">Nahraďte `dns.mynetwork.net` s hello plně kvalifikovaný název domény (FQDN) prostředku v síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="e83bb-193">Replace `dns.mynetwork.net` with hello fully qualified domain name (FQDN) of a resource in your on-premises network.</span></span>
    >
    > <span data-ttu-id="e83bb-194">Nahraďte `10.0.0.4` s hello __interní IP adresu__ vašeho vlastního serveru DNS ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-194">Replace `10.0.0.4` with hello __internal IP address__ of your custom DNS server in hello virtual network.</span></span>

    <span data-ttu-id="e83bb-195">Hello odpovědi, zobrazí se podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="e83bb-195">hello response appears similar toohello following text:</span></span>

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-hello-virtual-network-toouse-hello-custom-dns-server"></a><span data-ttu-id="e83bb-196">Konfigurace hello virtuální sítě toouse hello vlastního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="e83bb-196">Configure hello virtual network toouse hello custom DNS server</span></span>

<span data-ttu-id="e83bb-197">tooconfigure hello virtuální sítě toouse hello vlastního serveru DNS místo hello Azure rekurzivní překladač použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e83bb-197">tooconfigure hello virtual network toouse hello custom DNS server instead of hello Azure recursive resolver, use hello following steps:</span></span>

1. <span data-ttu-id="e83bb-198">V hello [portál Azure](https://portal.azure.com), vyberte hello virtuální sítě a pak vyberte __servery DNS__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-198">In hello [Azure portal](https://portal.azure.com), select hello virtual network, and then select __DNS Servers__.</span></span>

2. <span data-ttu-id="e83bb-199">Vyberte __vlastní__a zadejte hello __interní IP adresu__ hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-199">Select __Custom__, and enter hello __internal IP address__ of hello custom DNS server.</span></span> <span data-ttu-id="e83bb-200">Nakonec vyberte __Uložit__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-200">Finally, select __Save__.</span></span>

    ![Nastavit hello vlastního serveru DNS pro síť hello](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-hello-on-premises-dns-server"></a><span data-ttu-id="e83bb-202">Nakonfigurujte server DNS místní hello</span><span class="sxs-lookup"><span data-stu-id="e83bb-202">Configure hello on-premises DNS server</span></span>

<span data-ttu-id="e83bb-203">V předchozí části hello že jste nakonfigurovali hello vlastní DNS server tooforward požadavky toohello místní server DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-203">In hello previous section, you configured hello custom DNS server tooforward requests toohello on-premises DNS server.</span></span> <span data-ttu-id="e83bb-204">Dále je nutné nakonfigurovat hello místní DNS server tooforward požadavky toohello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-204">Next, you must configure hello on-premises DNS server tooforward requests toohello custom DNS server.</span></span>

<span data-ttu-id="e83bb-205">Postup pro konkrétní tooconfigure vašeho serveru DNS, najdete v dokumentaci hello software serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-205">For specific steps on how tooconfigure your DNS server, consult hello documentation for your DNS server software.</span></span> <span data-ttu-id="e83bb-206">Vyhledejte hello postup tooconfigure __pro podmíněné předávání__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-206">Look for hello steps on how tooconfigure a __conditional forwarder__.</span></span>

<span data-ttu-id="e83bb-207">Podmíněné dopředného pouze předá požadavky pro konkrétní příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-207">A conditional forward only forwards requests for a specific DNS suffix.</span></span> <span data-ttu-id="e83bb-208">V takovém případě musíte nakonfigurovat server pro předávání pro příponu DNS hello hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-208">In this case, you must configure a forwarder for hello DNS suffix of hello virtual network.</span></span> <span data-ttu-id="e83bb-209">Požadavky pro tuto příponu mají předávat toohello IP adresu hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-209">Requests for this suffix should be forwarded toohello IP address of hello custom DNS server.</span></span> 

<span data-ttu-id="e83bb-210">Hello následující text je příklad konfigurace pro podmíněné předávání pro hello **vazby** DNS softwaru:</span><span class="sxs-lookup"><span data-stu-id="e83bb-210">hello following text is an example of a conditional forwarder configuration for hello **Bind** DNS software:</span></span>

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello custom DNS server's internal IP address
    };

<span data-ttu-id="e83bb-211">Informace o použití DNS na **systému Windows Server 2016**, najdete v části hello [přidat DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentace...</span><span class="sxs-lookup"><span data-stu-id="e83bb-211">For information on using DNS on **Windows Server 2016**, see hello [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentation...</span></span>

<span data-ttu-id="e83bb-212">Jakmile jste nakonfigurovali server DNS místní hello, můžete použít `nslookup` z hello místní sítě tooverify, abyste mohli vyřešit názvy ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-212">Once you have configured hello on-premises DNS server, you can use `nslookup` from hello on-premises network tooverify that you can resolve names in hello virtual network.</span></span> <span data-ttu-id="e83bb-213">Následující ukázka Hello</span><span class="sxs-lookup"><span data-stu-id="e83bb-213">hello following example</span></span> 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

<span data-ttu-id="e83bb-214">Tento příklad používá hello na místním serveru DNS na 196.168.0.4 tooresolve hello název hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-214">This example uses hello on-premises DNS server at 196.168.0.4 tooresolve hello name of hello custom DNS server.</span></span> <span data-ttu-id="e83bb-215">Nahraďte text hello, jeden pro server DNS místní hello hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-215">Replace hello IP address with hello one for hello on-premises DNS server.</span></span> <span data-ttu-id="e83bb-216">Nahraďte hello `dnsproxy` adresu s hello plně kvalifikovaný název domény hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-216">Replace hello `dnsproxy` address with hello fully qualified domain name of hello custom DNS server.</span></span>

## <a name="optional-control-network-traffic"></a><span data-ttu-id="e83bb-217">Volitelné: Řízení síťového provozu</span><span class="sxs-lookup"><span data-stu-id="e83bb-217">Optional: Control network traffic</span></span>

<span data-ttu-id="e83bb-218">Můžete použít skupiny zabezpečení sítě (NSG) nebo trasy definované uživatelem (UDR) toocontrol síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="e83bb-218">You can use network security groups (NSG) or user-defined routes (UDR) toocontrol network traffic.</span></span> <span data-ttu-id="e83bb-219">Skupiny Nsg umožňují toofilter příchozí a odchozí přenos dat a povolí nebo zakážou provoz hello.</span><span class="sxs-lookup"><span data-stu-id="e83bb-219">NSGs allow you toofilter inbound and outbound traffic, and allow or deny hello traffic.</span></span> <span data-ttu-id="e83bb-220">Udr umožňují toocontrol tok přenosů mezi prostředky ve virtuální síti hello hello internet a hello do místní sítě.</span><span class="sxs-lookup"><span data-stu-id="e83bb-220">UDRs allow you toocontrol how traffic flows between resources in hello virtual network, hello internet, and hello on-premises network.</span></span>

> [!WARNING]
> <span data-ttu-id="e83bb-221">HDInsight vyžaduje příchozí přístup z konkrétní IP adresy v hello cloudu Azure a neomezený přístup pro odchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="e83bb-221">HDInsight requires inbound access from specific IP addresses in hello Azure cloud, and unrestricted outbound access.</span></span> <span data-ttu-id="e83bb-222">Pokud používáte skupiny Nsg nebo udr toocontrol provoz, je třeba provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e83bb-222">When using NSGs or UDRs toocontrol traffic, you must perform hello following steps:</span></span>
>
> 1. <span data-ttu-id="e83bb-223">Najde hello IP adresy pro hello umístění, která obsahuje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e83bb-223">Find hello IP addresses for hello location that contains your virtual network.</span></span> <span data-ttu-id="e83bb-224">Seznam požadované IP adresy podle umístění najdete v tématu [požadované IP adresy](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span><span class="sxs-lookup"><span data-stu-id="e83bb-224">For a list of required IPs by location, see [Required IP addresses](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span></span>
>
> 2. <span data-ttu-id="e83bb-225">Povolí příchozí provoz z hello IP adres.</span><span class="sxs-lookup"><span data-stu-id="e83bb-225">Allow inbound traffic from hello IP addresses.</span></span>
>
>    * <span data-ttu-id="e83bb-226">__Skupina NSG__: Povolit __příchozí__ přenosy na portu __443__ z hello __Internet__.</span><span class="sxs-lookup"><span data-stu-id="e83bb-226">__NSG__: Allow __inbound__ traffic on port __443__ from hello __Internet__.</span></span>
>    * <span data-ttu-id="e83bb-227">__UDR__: Sada hello __další směrování__ typ too__Internet__ hello trasy.</span><span class="sxs-lookup"><span data-stu-id="e83bb-227">__UDR__: Set hello __Next Hop__ type of hello route too__Internet__.</span></span>

<span data-ttu-id="e83bb-228">Příklad použití Azure PowerShell nebo rozhraní příkazového řádku Azure toocreate hello skupin Nsg, naleznete v části hello [rozšířit HDInsight s virtuálními sítěmi Azure](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-228">For an example of using Azure PowerShell or hello Azure CLI toocreate NSGs, see hello [Extend HDInsight with Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.</span></span>

## <a name="create-hello-hdinsight-cluster"></a><span data-ttu-id="e83bb-229">Vytvoření clusteru HDInsight se hello</span><span class="sxs-lookup"><span data-stu-id="e83bb-229">Create hello HDInsight cluster</span></span>

> [!WARNING]
> <span data-ttu-id="e83bb-230">Před instalací HDInsight ve virtuální síti hello je nutné nakonfigurovat hello vlastního serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e83bb-230">You must configure hello custom DNS server before installing HDInsight in hello virtual network.</span></span>

<span data-ttu-id="e83bb-231">Hello použijte kroky v hello [vytvoření clusteru HDInsight pomocí portálu Azure hello](./hdinsight-hadoop-create-linux-clusters-portal.md) dokumentu toocreate clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e83bb-231">Use hello steps in hello [Create an HDInsight cluster using hello Azure portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document toocreate an HDInsight cluster.</span></span>

> [!WARNING]
> * <span data-ttu-id="e83bb-232">Při vytváření clusteru musíte zvolit hello umístění, která obsahuje virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="e83bb-232">During cluster creation, you must choose hello location that contains your virtual network.</span></span>
>
> * <span data-ttu-id="e83bb-233">V hello __upřesňující nastavení__ část konfigurace, je nutné vybrat hello virtuální síť a podsíť, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e83bb-233">In hello __Advanced settings__ part of configuration, you must select hello virtual network and subnet that you created earlier.</span></span>

## <a name="connecting-toohdinsight"></a><span data-ttu-id="e83bb-234">Připojení tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="e83bb-234">Connecting tooHDInsight</span></span>

<span data-ttu-id="e83bb-235">Většina dokumentace v HDInsight předpokládá, že máte přístup toohello clusteru přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-235">Most documentation on HDInsight assumes that you have access toohello cluster over hello internet.</span></span> <span data-ttu-id="e83bb-236">Například, že se můžete připojit toohello clusteru https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="e83bb-236">For example, that you can connect toohello cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="e83bb-237">Tuto adresu používá hello veřejné bránu, která není k dispozici, pokud jste použili skupiny Nsg nebo hello udr toorestrict přístupu z Internetu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-237">This address uses hello public gateway, which is not available if you have used NSGs or UDRs toorestrict access from hello internet.</span></span>

<span data-ttu-id="e83bb-238">toodirectly připojení tooHDInsight přes hello virtuální síť, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e83bb-238">toodirectly connect tooHDInsight through hello virtual network, use hello following steps:</span></span>

1. <span data-ttu-id="e83bb-239">toodiscover hello interní plně kvalifikované názvy domény hello uzly clusteru HDInsight, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="e83bb-239">toodiscover hello internal fully qualified domain names of hello HDInsight cluster nodes, use one of hello following methods:</span></span>

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

2. <span data-ttu-id="e83bb-240">toodetermine hello port, který služba je k dispozici, najdete v části hello [porty používané služby Hadoop v HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-240">toodetermine hello port that a service is available on, see hello [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e83bb-241">Některé služby hostované v uzlech head hello aktivní pouze na jednom uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="e83bb-241">Some services hosted on hello head nodes are only active on one node at a time.</span></span> <span data-ttu-id="e83bb-242">Pokud se pokusíte přístup k službě jeden hlavního uzlu a ona selže, přepínače toohello jiných hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-242">If you try accessing a service on one head node and it fails, switch toohello other head node.</span></span>
    >
    > <span data-ttu-id="e83bb-243">Například Ambari je aktivní pouze jeden hlavního uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="e83bb-243">For example, Ambari is only active on one head node at a time.</span></span> <span data-ttu-id="e83bb-244">Pokud se pokusíte přístup k Ambari na jeden hlavní uzel a vrátí chybu 404, pak je spuštěn na hello jiných hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="e83bb-244">If you try accessing Ambari on one head node and it returns a 404 error, then it is running on hello other head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e83bb-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e83bb-245">Next steps</span></span>

* <span data-ttu-id="e83bb-246">Další informace o používání HDInsight ve virtuální síti, najdete v části [rozšířit HDInsight pomocí virtuálních sítí Azure](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="e83bb-246">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

* <span data-ttu-id="e83bb-247">Další informace o virtuálních sítí Azure, najdete v části hello [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e83bb-247">For more information on Azure virtual networks, see hello [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="e83bb-248">Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="e83bb-248">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="e83bb-249">Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e83bb-249">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>
