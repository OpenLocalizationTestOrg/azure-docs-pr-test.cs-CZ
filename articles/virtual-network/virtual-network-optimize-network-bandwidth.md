---
title: "propustnost sítě virtuálních počítačů aaaOptimize | Microsoft Docs"
description: "Zjistěte, jak toooptimize virtuální počítač Azure sítě propustnost."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: a5cff2d0ab6e3553c3f90d99629521a431477de0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Optimalizovat propustnost sítě pro virtuální počítače Azure

Azure virtuální počítače (VM) mají výchozí nastavení sítě, které lze dále optimalizovat pro propustnost sítě. Tento článek popisuje, jak toooptimize propustnost sítě pro Microsoft Azure Windows a virtuální počítače s Linuxem, včetně hlavních distribuce například Ubuntu a CentOS Red Hat.

## <a name="windows-vm"></a>Virtuální počítač s Windows

Pokud vaše virtuální počítač s Windows je podporovaná s [Accelerated sítě](virtual-network-create-vm-accelerated-networking.md), povolení této funkce by být hello optimální konfigurace pro propustnost. Pro všechny ostatní virtuální počítače Windows škálování na straně příjmu (RSS) pomocí dosáhnout vyšší maximální propustnost než virtuální počítač bez RSS. RSS může být nepovolený ve výchozím nastavení v systému Windows virtuálního počítače. Dokončete následující kroky toodetermine, zda byla povolená technologie RSS hello a tooenable ji, pokud je zakázána.

1. Zadejte hello `Get-NetAdapterRss` toosee příkaz prostředí PowerShell, pokud byla povolená technologie RSS pro síťový adaptér. Hello následující příklad výstupu vrácených hello `Get-NetAdapterRss`, RSS není povoleno.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Zadejte následující příkaz tooenable RSS hello:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    předchozí příkaz Hello nemá výstup. příkaz Hello změnit nastavení síťový adaptér, což by způsobilo ztrátě dočasné připojení přibližně jednu minutu. Při ztrátě připojení hello se zobrazí dialogové okno Reconnecting. Obvykle se obnoví připojení k po pokusu o třetí hello.
3. Zkontrolujte, zda je RSS povoleno v hello virtuálních počítačů tak, že zadáte hello `Get-NetAdapterRss` příkaz znovu. V případě úspěchu se vrátí hello následující příklad výstupu:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Virtuální počítač s Linuxem

RSS je vždy povolena ve výchozím nastavení v systému Linux virtuálního počítače Azure. Linux jádra vydané od ledna 2017 zahrnují nové možnosti optimalizace sítě, které umožňují virtuální počítač s Linuxem tooachieve vyšší propustnost sítě.

### <a name="ubuntu"></a>Ubuntu

V modulu snap-in optimalizace hello tooget pořadí aktualizace toohello podporované nejnovější verzi, od června 2017, což je:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Po dokončení aktualizace hello zadejte následující příkazy tooget hello nejnovější jádra hello:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Volitelný příkaz:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure Preview jádra
> [!WARNING]
> Tato verze Preview Linux Azure nemusí mít jádra hello stejnou úroveň dostupnost a spolehlivost jako obrázky Marketplace a jádra, která jsou obecně dostupnosti verze. Hello funkce nepodporuje, může mít omezené možnosti a nemusí být stejně spolehlivá jako hello výchozí jádra. Nepoužívejte tuto jádra pro úlohy v produkčním prostředí.

Nainstalováním hello navrhované jádra Azure Linux jde dosáhnout výrazné propustnost. tootry tento jádra, přidejte tento řádek too/etc/apt/sources.list

```bash
#add this toohello end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Potom spuštěním těchto příkazů jako kořenového příkazu.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

V pořadí tooget hello optimalizace aktualizace verze toohello nejnovější podporované od července 2017, což je:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Po dokončení aktualizace hello hello instalovat nejnovější Linux integrační služby (LIS).
Optimalizace propustnosti Hello se LIS, od 4.2.2-2. Zadejte následující příkazy tooinstall LIS hello:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

V pořadí tooget hello optimalizace aktualizace verze toohello nejnovější podporované od července 2017, což je:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Po dokončení aktualizace hello hello instalovat nejnovější Linux integrační služby (LIS).
Optimalizace propustnosti Hello se LIS, od 4.2. Zadejte následující příkazy toodownload hello a instalovat:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Další informace o Linux integrační služby verzi 4.2 pro Hyper-V zobrazením hello [stránce pro stažení](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Další kroky
* Teď, když hello virtuálního počítače je optimalizovaná, najdete v části hello výsledek [šířky pásma nebo propustnost testování virtuální počítač Azure](virtual-network-bandwidth-testing.md) pro váš scénář.
* Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)
