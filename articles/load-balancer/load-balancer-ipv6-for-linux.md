---
title: "aaaConfiguring DHCPv6 pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Jak tooconfigure DHCPv6 pro virtuální počítače s Linuxem."
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
ms.openlocfilehash: abd5a98c3496b189946f59bab1d9c20dcd0aa2c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a><span data-ttu-id="4826c-104">Konfigurace protokolu DHCPv6 pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4826c-104">Configuring DHCPv6 for Linux VMs</span></span>

<span data-ttu-id="4826c-105">Některé bitové kopie virtuálních počítačů Linux hello v hello Azure Marketplace nemají ve výchozím nastavení nakonfigurované DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="4826c-105">Some of hello Linux virtual machine images in hello Azure Marketplace do not have DHCPv6 configured by default.</span></span> <span data-ttu-id="4826c-106">toosupport IPv6, DHCPv6 musí být konfigurované v v rámci distribučních hello operační systém Linux, který používáte.</span><span class="sxs-lookup"><span data-stu-id="4826c-106">toosupport IPv6, DHCPv6 must be configured in within hello Linux OS distribution that you are using.</span></span> <span data-ttu-id="4826c-107">Různých distribucí Linux mají různé způsoby konfigurace DHCPv6, protože používají různé balíčky.</span><span class="sxs-lookup"><span data-stu-id="4826c-107">Different Linux distributions have different ways of configuring DHCPv6 because they use different packages.</span></span>

> [!NOTE]
> <span data-ttu-id="4826c-108">Poslední SUSE Linux a CoreOS obrázků v hello Azure Marketplace se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="4826c-108">Recent SUSE Linux and CoreOS images in hello Azure Marketplace have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="4826c-109">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="4826c-109">No additional changes are required when using those images.</span></span>

<span data-ttu-id="4826c-110">Tento dokument popisuje, jak tooenable DHCPv6 tak, aby virtuální počítač Linux získá adresu IPv6.</span><span class="sxs-lookup"><span data-stu-id="4826c-110">This document describes how tooenable DHCPv6 so that your Linux virtual machine obtains an IPv6 address.</span></span>

> [!WARNING]
> <span data-ttu-id="4826c-111">Nesprávně úpravou konfiguračních souborů síť může způsobit, že jste toolose síťový přístup tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4826c-111">Improperly editing network configuration files can cause you toolose network access tooyour VM.</span></span> <span data-ttu-id="4826c-112">Doporučujeme, abyste otestovali změny konfigurace v systémech nevýrobní prostředí.</span><span class="sxs-lookup"><span data-stu-id="4826c-112">We recommended that you test your configuration changes on non-production systems.</span></span> <span data-ttu-id="4826c-113">Hello pokyny v tomto článku jsme otestovali na nejnovější verze hello hello Linux obrázků v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="4826c-113">hello instructions in this article have been tested on hello latest versions of hello Linux images in hello Azure Marketplace.</span></span> <span data-ttu-id="4826c-114">Pro konkrétní verzi systému Linux podrobnější pokyny naleznete v dokumentaci hello.</span><span class="sxs-lookup"><span data-stu-id="4826c-114">Consult hello documentation for your specific version of Linux for more detailed instructions.</span></span>

## <a name="ubuntu"></a><span data-ttu-id="4826c-115">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4826c-115">Ubuntu</span></span>

1. <span data-ttu-id="4826c-116">Upravte soubor hello `/etc/dhcp/dhclient6.conf` a přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="4826c-116">Edit hello file `/etc/dhcp/dhclient6.conf` and add hello following line:</span></span>

        timeout 10;

2. <span data-ttu-id="4826c-117">Upravte hello konfiguraci sítě pro položku eth0 rozhraní hello hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4826c-117">Edit hello network configuration for hello eth0 interface with hello following configuration:</span></span>

   * <span data-ttu-id="4826c-118">Na **Ubuntu 12.04 a 14.04**, upravte soubor hello`/etc/network/interfaces.d/eth0.cfg`</span><span class="sxs-lookup"><span data-stu-id="4826c-118">On **Ubuntu 12.04 and 14.04**, edit hello file `/etc/network/interfaces.d/eth0.cfg`</span></span>
   * <span data-ttu-id="4826c-119">Na **Ubuntu 16.04**, upravte soubor hello`/etc/network/interfaces.d/50-cloud-init.cfg`</span><span class="sxs-lookup"><span data-stu-id="4826c-119">On **Ubuntu 16.04**, edit hello file `/etc/network/interfaces.d/50-cloud-init.cfg`</span></span>

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="4826c-120">Obnovení IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="4826c-120">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a><span data-ttu-id="4826c-121">Debian</span><span class="sxs-lookup"><span data-stu-id="4826c-121">Debian</span></span>

1. <span data-ttu-id="4826c-122">Upravte soubor hello `/etc/dhcp/dhclient6.conf` a přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="4826c-122">Edit hello file `/etc/dhcp/dhclient6.conf` and add hello following line:</span></span>

        timeout 10;

2. <span data-ttu-id="4826c-123">Upravte soubor hello `/etc/network/interfaces` a přidejte hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4826c-123">Edit hello file `/etc/network/interfaces` and add hello following configuration:</span></span>

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="4826c-124">Obnovení IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="4826c-124">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a><span data-ttu-id="4826c-125">RHEL nebo CentOS nebo Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="4826c-125">RHEL / CentOS / Oracle Linux</span></span>

1. <span data-ttu-id="4826c-126">Upravte soubor hello `/etc/sysconfig/network` a přidejte hello následující parametr:</span><span class="sxs-lookup"><span data-stu-id="4826c-126">Edit hello file `/etc/sysconfig/network` and add hello following parameter:</span></span>

        NETWORKING_IPV6=yes

2. <span data-ttu-id="4826c-127">Upravte soubor hello `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte hello následující dva parametry:</span><span class="sxs-lookup"><span data-stu-id="4826c-127">Edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following two parameters:</span></span>

        IPV6INIT=yes
        DHCPV6C=yes

3. <span data-ttu-id="4826c-128">Obnovení IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="4826c-128">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a><span data-ttu-id="4826c-129">SLES 11 & openSUSE 13</span><span class="sxs-lookup"><span data-stu-id="4826c-129">SLES 11 & openSUSE 13</span></span>

<span data-ttu-id="4826c-130">Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="4826c-130">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="4826c-131">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="4826c-131">No additional changes are required when using those images.</span></span> <span data-ttu-id="4826c-132">Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4826c-132">If you have a VM based on an older or custom SUSE image, use hello following steps:</span></span>

1. <span data-ttu-id="4826c-133">Nainstalujte hello `dhcp-client` balíčku, v případě potřeby:</span><span class="sxs-lookup"><span data-stu-id="4826c-133">Install hello `dhcp-client` package, if needed:</span></span>

    ```bash
    sudo zypper install dhcp-client
    ```

2. <span data-ttu-id="4826c-134">Upravte soubor hello `/etc/sysconfig/network/ifcfg-eth0` a přidejte hello následující parametr:</span><span class="sxs-lookup"><span data-stu-id="4826c-134">Edit hello file `/etc/sysconfig/network/ifcfg-eth0` and add hello following parameter:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="4826c-135">Obnovení hello IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="4826c-135">Renew hello IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a><span data-ttu-id="4826c-136">SLES 12 a openSUSE přestupného</span><span class="sxs-lookup"><span data-stu-id="4826c-136">SLES 12 and openSUSE Leap</span></span>

<span data-ttu-id="4826c-137">Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="4826c-137">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="4826c-138">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="4826c-138">No additional changes are required when using those images.</span></span> <span data-ttu-id="4826c-139">Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4826c-139">If you have a VM based on an older or custom SUSE image, use hello following steps:</span></span>

1. <span data-ttu-id="4826c-140">Upravte soubor hello `/etc/sysconfig/network/ifcfg-eth0` a nahraďte tento parametr</span><span class="sxs-lookup"><span data-stu-id="4826c-140">Edit hello file `/etc/sysconfig/network/ifcfg-eth0` and replace this parameter</span></span>

        #BOOTPROTO='dhcp4'

    <span data-ttu-id="4826c-141">s hello následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="4826c-141">with hello following value:</span></span>

        BOOTPROTO='dhcp'

2. <span data-ttu-id="4826c-142">Přidejte následující parametr příliš hello`/etc/sysconfig/network/ifcfg-eth0`:</span><span class="sxs-lookup"><span data-stu-id="4826c-142">Add hello following parameter too`/etc/sysconfig/network/ifcfg-eth0`:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="4826c-143">Obnovení hello IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="4826c-143">Renew hello IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a><span data-ttu-id="4826c-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4826c-144">CoreOS</span></span>

<span data-ttu-id="4826c-145">Poslední CoreOS bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="4826c-145">Recent CoreOS images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="4826c-146">Při použití těchto imagí nejsou vyžadovány žádné další změny.</span><span class="sxs-lookup"><span data-stu-id="4826c-146">No additional changes are required when using those images.</span></span> <span data-ttu-id="4826c-147">Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní CoreOS, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4826c-147">If you have a VM based on an older or custom CoreOS image, use hello following steps:</span></span>

1. <span data-ttu-id="4826c-148">Upravte soubor hello`/etc/systemd/network/10_dhcp.network`</span><span class="sxs-lookup"><span data-stu-id="4826c-148">Edit hello file `/etc/systemd/network/10_dhcp.network`</span></span>

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. <span data-ttu-id="4826c-149">Obnovení hello IPv6 adresa:</span><span class="sxs-lookup"><span data-stu-id="4826c-149">Renew hello IPv6 address:</span></span>

    ```bash
    sudo systemctl restart systemd-networkd
    ```
