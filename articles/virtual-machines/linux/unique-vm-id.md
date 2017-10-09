---
title: "aaaGet ID virtuálních počítačů Linux Azure | Microsoft Docs"
description: "Popisuje, jak tooget a používání Azure Linux virtuálních počítačů Jedinečný ID."
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
ms.openlocfilehash: 4c8ddfc2e892824581e77649285ee8adbccd5def
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a>Přístupu a použití jedinečné ID virtuálního počítače Azure
Jedinečné ID virtuální počítač Azure je identifikátor 128 mi kódování a uložené v SMBIOS všechny Azure IaaS Virtuálního počítače a lze je aktuálně číst příkazy platformy systému BIOS.

Jedinečné ID virtuální počítač Azure je vlastnost jen pro čtení. Při vypnutí restartování počítače nedojde ke změně Azure jedinečné ID virtuálního počítače (buď plánované pro neplánované), spuštění a zastavení zrušit přidělení, služby, opravy nebo obnovení na místě. Pokud hello virtuálního počítače je snímek a zkopírovaný toocreate novou instanci, je nakonfigurován nové ID virtuálního počítače Azure.

> [!NOTE]
> Pokud máte starší virtuálních počítačů vytvořena a spuštěna, protože tato nová funkce získali nasazuje (18. září 2014), restartujte prosím váš počítač tooautomatically získat Azure jedinečný identifikátor.
> 
> 

Jedinečné ID virtuálního počítače Azure z tooaccess v rámci hello virtuálních počítačů:

## <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Další informace najdete v tématu [vytvoření virtuálního počítače](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="connect-toohello-vm"></a>Připojit toohello virtuálních počítačů
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

Z důvodu tooBig Endian bit řazení hello skutečné jedinečné ID virtuálního počítače v tomto případě bude:

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

Azure jedinečné ID virtuálního počítače můžete použít v různých scénářích, zda hello virtuální počítač běží v Azure nebo místní a může pomoct vaší licencování, vytváření sestav a obecné požadavky sledování, které může být v nasazeních Azure IaaS. Mnoho nezávislí dodavatelé softwaru vytváření aplikací a jejich certifikuje v Azure může vyžadovat tooidentify virtuálního počítače Azure v rámci tootell a životního cyklu, pokud hello virtuální počítač běží na Azure, místně nebo na jiných poskytovatelů cloudu. Tento identifikátor platformy může pomoci například zjistit, pokud je řádně licencované hello softwaru nebo pomoci toocorrelate libovolný zdroj dat tooits virtuálního počítače jako je například tooassist na nastavení hello správné metrik pro hello správné platformy a tootrack a korelovat tyto metriky mezi jiné účely.

