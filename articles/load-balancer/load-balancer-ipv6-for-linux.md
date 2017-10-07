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
# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurace protokolu DHCPv6 pro virtuální počítače s Linuxem

Některé bitové kopie virtuálních počítačů Linux hello v hello Azure Marketplace nemají ve výchozím nastavení nakonfigurované DHCPv6. toosupport IPv6, DHCPv6 musí být konfigurované v v rámci distribučních hello operační systém Linux, který používáte. Různých distribucí Linux mají různé způsoby konfigurace DHCPv6, protože používají různé balíčky.

> [!NOTE]
> Poslední SUSE Linux a CoreOS obrázků v hello Azure Marketplace se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny.

Tento dokument popisuje, jak tooenable DHCPv6 tak, aby virtuální počítač Linux získá adresu IPv6.

> [!WARNING]
> Nesprávně úpravou konfiguračních souborů síť může způsobit, že jste toolose síťový přístup tooyour virtuálních počítačů. Doporučujeme, abyste otestovali změny konfigurace v systémech nevýrobní prostředí. Hello pokyny v tomto článku jsme otestovali na nejnovější verze hello hello Linux obrázků v hello Azure Marketplace. Pro konkrétní verzi systému Linux podrobnější pokyny naleznete v dokumentaci hello.

## <a name="ubuntu"></a>Ubuntu

1. Upravte soubor hello `/etc/dhcp/dhclient6.conf` a přidejte následující řádek hello:

        timeout 10;

2. Upravte hello konfiguraci sítě pro položku eth0 rozhraní hello hello následující konfigurace:

   * Na **Ubuntu 12.04 a 14.04**, upravte soubor hello`/etc/network/interfaces.d/eth0.cfg`
   * Na **Ubuntu 16.04**, upravte soubor hello`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Obnovení IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Upravte soubor hello `/etc/dhcp/dhclient6.conf` a přidejte následující řádek hello:

        timeout 10;

2. Upravte soubor hello `/etc/network/interfaces` a přidejte hello následující konfigurace:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Obnovení IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL nebo CentOS nebo Oracle Linux

1. Upravte soubor hello `/etc/sysconfig/network` a přidejte hello následující parametr:

        NETWORKING_IPV6=yes

2. Upravte soubor hello `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte hello následující dva parametry:

        IPV6INIT=yes
        DHCPV6C=yes

3. Obnovení IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny. Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte hello následující kroky:

1. Nainstalujte hello `dhcp-client` balíčku, v případě potřeby:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Upravte soubor hello `/etc/sysconfig/network/ifcfg-eth0` a přidejte hello následující parametr:

        DHCLIENT6_MODE='managed'

3. Obnovení hello IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 a openSUSE přestupného

Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny. Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte hello následující kroky:

1. Upravte soubor hello `/etc/sysconfig/network/ifcfg-eth0` a nahraďte tento parametr

        #BOOTPROTO='dhcp4'

    s hello následující hodnotu:

        BOOTPROTO='dhcp'

2. Přidejte následující parametr příliš hello`/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Obnovení hello IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Poslední CoreOS bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny. Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní CoreOS, použijte hello následující kroky:

1. Upravte soubor hello`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Obnovení hello IPv6 adresa:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
