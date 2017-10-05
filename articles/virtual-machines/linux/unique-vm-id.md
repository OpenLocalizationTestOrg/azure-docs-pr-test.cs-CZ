---
title: "Získání ID virtuálního počítače Azure s Linuxem | Microsoft Docs"
description: "Popisuje, jak získat a použít Azure Linux virtuálních počítačů jedinečný identifikátor."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/23/2017
ms.author: kmouss
ms.openlocfilehash: 258ce425d5692730011cf2f4468dc0ba77f4cb79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a>Přístupu a použití jedinečné ID virtuálního počítače Azure
Jedinečné ID virtuální počítač Azure je identifikátor 128 mi kódování a uložené v SMBIOS všechny Azure IaaS Virtuálního počítače a lze je aktuálně číst příkazy platformy systému BIOS.

Jedinečné ID virtuální počítač Azure je vlastnost jen pro čtení. Při vypnutí restartování počítače nedojde ke změně Azure jedinečné ID virtuálního počítače (buď plánované pro neplánované), spuštění a zastavení zrušit přidělení, služby, opravy nebo obnovení na místě. Pokud virtuální počítač je snímek a zkopírují se na vytvoření nové instance, je nakonfigurován nové ID virtuálního počítače Azure.

> [!NOTE]
> Pokud máte starší virtuálních počítačů vytvořena a spuštěna, protože tato nová funkce získali nasazuje (18. září 2014), prosím restartování virtuálního počítače se automaticky získat Azure Jedinečný ID.
> 
> 

Pro přístup k Azure jedinečné ID virtuálního počítače z virtuálního počítače:

## <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Další informace najdete v tématu [vytvoření virtuálního počítače](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="connect-to-the-vm"></a>Připojení k virtuálnímu počítači
Další informace najdete v tématu [SSH z Linuxu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="query-vm-unique-id"></a>Jedinečné ID dotazu virtuálního počítače
Příkaz (příklad používá **Ubuntu**):

```bash
sudo dmidecode | grep UUID
```

Příklad očekávané výsledky:

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

Z důvodu Big Endian bit řazení skutečný jedinečné ID virtuálního počítače v tomto případě bude:

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

Azure jedinečné ID virtuálního počítače můžete použít v různých scénářích, zda virtuální počítač běží v Azure nebo místní a může pomoct vaší licencování, vytváření sestav a obecné požadavky sledování, které může být v nasazeních Azure IaaS. Může vyžadovat mnoho nezávislí dodavatelé softwaru vytváření aplikací a jejich certifikuje v Azure k identifikaci virtuálního počítače Azure v průběhu životního cyklu a říct, pokud je virtuální počítač spuštěný v Azure, místně nebo na jiných poskytovatelů cloudu. Tento identifikátor platformy může například pomoci zjistit, pokud je řádně licencované softwaru a v nápovědě ke korelaci žádná data virtuálních počítačů, jako svůj zdroj pomůže na nastavení správné metriky pro správné platformu a ke sledování a korelovat tyto metriky mimo jiné používá.

