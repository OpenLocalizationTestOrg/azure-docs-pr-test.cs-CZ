---
title: "Konfigurace DHCPv6 pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Postup konfigurace DHCPv6 pro virtuální počítače s Linuxem."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5c591e7f1838c86ca74caea9dd3a5e8f874fd8a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a><span data-ttu-id="e0c76-104">Konfigurace protokolu DHCPv6 pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e0c76-104">Configuring DHCPv6 for Linux VMs</span></span>

<span data-ttu-id="e0c76-105">Některé bitové kopie virtuálních počítačů Linux v Azure Marketplace nemají ve výchozím nastavení nakonfigurované DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e0c76-105">Some of the Linux virtual machine images in the Azure Marketplace do not have DHCPv6 configured by default.</span></span> <span data-ttu-id="e0c76-106">Chcete-li podporovat protokol IPv6, musí být nakonfigurován DHCPv6 v v rámci operačního systému Linux distribuce, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="e0c76-106">To support IPv6, DHCPv6 must be configured in within the Linux OS distribution that you are using.</span></span> <span data-ttu-id="e0c76-107">Různých distribucí Linux mají různé způsoby konfigurace DHCPv6, protože používají různé balíčky.</span><span class="sxs-lookup"><span data-stu-id="e0c76-107">Different Linux distributions have different ways of configuring DHCPv6 because they use different packages.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c76-108">Poslední SUSE Linux a CoreOS bitové kopie v Azure Marketplace se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e0c76-108">Recent SUSE Linux and CoreOS images in the Azure Marketplace have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e0c76-109">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="e0c76-109">No additional changes are required when using those images.</span></span>

<span data-ttu-id="e0c76-110">Tento dokument popisuje, jak povolit DHCPv6 tak, aby virtuální počítač Linux získá adresu IPv6.</span><span class="sxs-lookup"><span data-stu-id="e0c76-110">This document describes how to enable DHCPv6 so that your Linux virtual machine obtains an IPv6 address.</span></span>

> [!WARNING]
> <span data-ttu-id="e0c76-111">Nesprávně úpravou konfiguračních souborů síť může způsobit ztrátu přístupu k síti k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e0c76-111">Improperly editing network configuration files can cause you to lose network access to your VM.</span></span> <span data-ttu-id="e0c76-112">Doporučujeme, abyste otestovali změny konfigurace v systémech nevýrobní prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0c76-112">We recommended that you test your configuration changes on non-production systems.</span></span> <span data-ttu-id="e0c76-113">Podle pokynů v tomto článku jsme otestovali na nejnovější verze Linux obrázků v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e0c76-113">The instructions in this article have been tested on the latest versions of the Linux images in the Azure Marketplace.</span></span> <span data-ttu-id="e0c76-114">V dokumentaci ke konkrétní verzi systému Linux podrobnější pokyny.</span><span class="sxs-lookup"><span data-stu-id="e0c76-114">Consult the documentation for your specific version of Linux for more detailed instructions.</span></span>

## <a name="ubuntu"></a><span data-ttu-id="e0c76-115">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e0c76-115">Ubuntu</span></span>

1. <span data-ttu-id="e0c76-116">Upravte soubor `/etc/dhcp/dhclient6.conf` a přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="e0c76-116">Edit the file `/etc/dhcp/dhclient6.conf` and add the following line:</span></span>

        timeout 10;

2. <span data-ttu-id="e0c76-117">Upravte konfiguraci sítě pro položku eth0 rozhraní s následující konfigurací:</span><span class="sxs-lookup"><span data-stu-id="e0c76-117">Edit the network configuration for the eth0 interface with the following configuration:</span></span>

   * <span data-ttu-id="e0c76-118">Na **Ubuntu 12.04 a 14.04**, upravte soubor`/etc/network/interfaces.d/eth0.cfg`</span><span class="sxs-lookup"><span data-stu-id="e0c76-118">On **Ubuntu 12.04 and 14.04**, edit the file `/etc/network/interfaces.d/eth0.cfg`</span></span>
   * <span data-ttu-id="e0c76-119">Na **Ubuntu 16.04**, upravte soubor`/etc/network/interfaces.d/50-cloud-init.cfg`</span><span class="sxs-lookup"><span data-stu-id="e0c76-119">On **Ubuntu 16.04**, edit the file `/etc/network/interfaces.d/50-cloud-init.cfg`</span></span>

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="e0c76-120">Obnovení IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="e0c76-120">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a><span data-ttu-id="e0c76-121">Debian</span><span class="sxs-lookup"><span data-stu-id="e0c76-121">Debian</span></span>

1. <span data-ttu-id="e0c76-122">Upravte soubor `/etc/dhcp/dhclient6.conf` a přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="e0c76-122">Edit the file `/etc/dhcp/dhclient6.conf` and add the following line:</span></span>

        timeout 10;

2. <span data-ttu-id="e0c76-123">Upravte soubor `/etc/network/interfaces` a přidejte následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="e0c76-123">Edit the file `/etc/network/interfaces` and add the following configuration:</span></span>

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="e0c76-124">Obnovení IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="e0c76-124">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a><span data-ttu-id="e0c76-125">RHEL nebo CentOS nebo Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="e0c76-125">RHEL / CentOS / Oracle Linux</span></span>

1. <span data-ttu-id="e0c76-126">Upravte soubor `/etc/sysconfig/network` a přidejte následující parametr:</span><span class="sxs-lookup"><span data-stu-id="e0c76-126">Edit the file `/etc/sysconfig/network` and add the following parameter:</span></span>

        NETWORKING_IPV6=yes

2. <span data-ttu-id="e0c76-127">Upravte soubor `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte následující dva parametry:</span><span class="sxs-lookup"><span data-stu-id="e0c76-127">Edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following two parameters:</span></span>

        IPV6INIT=yes
        DHCPV6C=yes

3. <span data-ttu-id="e0c76-128">Obnovení IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="e0c76-128">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a><span data-ttu-id="e0c76-129">SLES 11 & openSUSE 13</span><span class="sxs-lookup"><span data-stu-id="e0c76-129">SLES 11 & openSUSE 13</span></span>

<span data-ttu-id="e0c76-130">Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e0c76-130">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e0c76-131">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="e0c76-131">No additional changes are required when using those images.</span></span> <span data-ttu-id="e0c76-132">Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0c76-132">If you have a VM based on an older or custom SUSE image, use the following steps:</span></span>

1. <span data-ttu-id="e0c76-133">Nainstalujte `dhcp-client` balíčku, v případě potřeby:</span><span class="sxs-lookup"><span data-stu-id="e0c76-133">Install the `dhcp-client` package, if needed:</span></span>

    ```bash
    sudo zypper install dhcp-client
    ```

2. <span data-ttu-id="e0c76-134">Upravte soubor `/etc/sysconfig/network/ifcfg-eth0` a přidejte následující parametr:</span><span class="sxs-lookup"><span data-stu-id="e0c76-134">Edit the file `/etc/sysconfig/network/ifcfg-eth0` and add the following parameter:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="e0c76-135">Obnovení na adresu IPv6:</span><span class="sxs-lookup"><span data-stu-id="e0c76-135">Renew the IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a><span data-ttu-id="e0c76-136">SLES 12 a openSUSE přestupného</span><span class="sxs-lookup"><span data-stu-id="e0c76-136">SLES 12 and openSUSE Leap</span></span>

<span data-ttu-id="e0c76-137">Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e0c76-137">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e0c76-138">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="e0c76-138">No additional changes are required when using those images.</span></span> <span data-ttu-id="e0c76-139">Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0c76-139">If you have a VM based on an older or custom SUSE image, use the following steps:</span></span>

1. <span data-ttu-id="e0c76-140">Upravte soubor `/etc/sysconfig/network/ifcfg-eth0` a nahraďte tento parametr</span><span class="sxs-lookup"><span data-stu-id="e0c76-140">Edit the file `/etc/sysconfig/network/ifcfg-eth0` and replace this parameter</span></span>

        #BOOTPROTO='dhcp4'

    <span data-ttu-id="e0c76-141">s následující hodnotou:</span><span class="sxs-lookup"><span data-stu-id="e0c76-141">with the following value:</span></span>

        BOOTPROTO='dhcp'

2. <span data-ttu-id="e0c76-142">Přidejte následující parametr k `/etc/sysconfig/network/ifcfg-eth0`:</span><span class="sxs-lookup"><span data-stu-id="e0c76-142">Add the following parameter to `/etc/sysconfig/network/ifcfg-eth0`:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="e0c76-143">Obnovení na adresu IPv6:</span><span class="sxs-lookup"><span data-stu-id="e0c76-143">Renew the IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a><span data-ttu-id="e0c76-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="e0c76-144">CoreOS</span></span>

<span data-ttu-id="e0c76-145">Poslední CoreOS bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e0c76-145">Recent CoreOS images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e0c76-146">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="e0c76-146">No additional changes are required when using those images.</span></span> <span data-ttu-id="e0c76-147">Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní CoreOS, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0c76-147">If you have a VM based on an older or custom CoreOS image, use the following steps:</span></span>

1. <span data-ttu-id="e0c76-148">Upravte soubor`/etc/systemd/network/10_dhcp.network`</span><span class="sxs-lookup"><span data-stu-id="e0c76-148">Edit the file `/etc/systemd/network/10_dhcp.network`</span></span>

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. <span data-ttu-id="e0c76-149">Obnovení na adresu IPv6:</span><span class="sxs-lookup"><span data-stu-id="e0c76-149">Renew the IPv6 address:</span></span>

    ```bash
    sudo systemctl restart systemd-networkd
    ```
