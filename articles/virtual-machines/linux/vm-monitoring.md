---
title: "Povolení nebo zákaz monitorování virtuálních počítačů Azure"
description: "Popisuje, jak povolit nebo zakázat monitorování virtuálních počítačů Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 9b2fe579113d6ca6bfd27d82eb9d4657657d44ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a>Povolte nebo zakažte monitorování virtuálních počítačů Azure

Tato část popisuje, jak povolit nebo zakázat monitorování na virtuální počítače běžící v Azure. Můžete povolit nebo zakázat monitorování pomocí portálu nebo rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI).

> [!IMPORTANT]
> Tento dokument popisuje 2.3 verzi rozšíření diagnostiky Linux, která se už nepoužívá. Verze 2.3 bude až do 30. června 2018 podporována.
>
> Místo toho lze povolit rozšíření diagnostiky Linux verze 3.0. Další informace najdete v tématu [dokumentaci](./diagnostic-extension.md).

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Povolit / zakázat monitorování prostřednictvím portálu Azure

Můžete povolit monitorování vašeho virtuálního počítače Azure, který poskytuje data o vaší instance v období 1 minutu. (použít změny úložiště). Podrobné diagnostická data, pak je k dispozici pro virtuální počítač v portálu grafy nebo přes rozhraní API. Ve výchozím nastavení portál Azure umožňuje monitorování na základě hostitele omezenou sadu metriky. Můžete povolit sledování metrik z v rámci virtuálního počítače spuštěného virtuálního počítače nebo v zastaveném stavu.

* Otevřete portál Azure na adrese [https://portal.azure.com](https://portal.azure.com).
* V levém navigačním panelu klikněte na virtuální počítače.
* V seznamu virtuálních počítačů vyberte instanci spuštěná nebo zastavená. Otevře se okno "Virtuální počítač".
* Klikněte na tlačítko všechna nastavení.
* Klikněte na tlačítko diagnostiky.
* Změnit stav zapnuto nebo vypnuto. Můžete také vybrat v tomto okně úroveň monitorování podrobnosti, které chcete povolit pro virtuální počítač.

![Povolit nebo zakázat monitorování prostřednictvím portálu Azure.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a>Povolit / zakázat monitorování pomocí rozhraní příkazového řádku Azure

Chcete-li povolit monitorování pro virtuální počítač Azure.

* Vytvořte soubor (s názvem například PrivateConfig.json):

```json
{
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account"
}
```

* Povolte rozšíření prostřednictvím rozhraní příkazového řádku Azure.

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

Další informace najdete v tématu [pomocí Linux diagnostiky rozšíření výkonu monitorování Linux Virtuálního počítače a diagnostických dat](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
