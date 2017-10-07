---
title: "aaaEnable nebo zakázání monitorování virtuálních počítačů Azure"
description: "Popisuje, jak tooEnable nebo zakázat monitorování virtuálních počítačů Azure"
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
ms.openlocfilehash: 39c2211e4e5f3ad364513b52c5acc70c00b432fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a>Povolte nebo zakažte monitorování virtuálních počítačů Azure

Tato část popisuje, jak se tooenable nebo zakažte monitorování na virtuální počítače, spuštěné v Azure. Můžete povolit nebo zakázat monitorování pomocí portálu hello nebo rozhraní příkazového řádku Azure pro Mac, Linux a Windows (hello příkazového řádku Azure CLI).

> [!IMPORTANT]
> Tento dokument popisuje 2.3 verzi hello rozšíření diagnostiky Linux, která se už nepoužívá. Verze 2.3 bude až do 30. června 2018 podporována.
>
> Místo toho lze povolit verze 3.0 hello rozšíření diagnostiky Linux. Další informace najdete v tématu [hello dokumentaci](./diagnostic-extension.md).

## <a name="enable--disable-monitoring-through-hello-azure-portal"></a>Povolit / zakázat monitorování hello na portálu Azure

Můžete povolit monitorování vašeho virtuálního počítače Azure, který poskytuje data o vaší instance v období 1 minutu. (použít změny úložiště). Podrobné diagnostická data bude k dispozici pro hello virtuálních počítačů v portálu grafy hello nebo prostřednictvím hello rozhraní API. Ve výchozím nastavení portál Azure umožňuje monitorování na základě hostitele omezenou sadu metriky. Můžete povolit sledování metrik z ve virtuálním počítači při hello, který je spuštěný virtuální počítač nebo v zastaveném stavu.

* Otevřete hello Azure portálu na [https://portal.azure.com](https://portal.azure.com).
* V levé navigační hello klikněte na virtuální počítače.
* Ve virtuálních počítačích hello seznamu vyberte instanci spuštěná nebo zastavená. Otevře se okno Hello "Virtuální počítač".
* Klikněte na tlačítko všechna nastavení.
* Klikněte na tlačítko diagnostiky.
* Změnit stav tooOn nebo vypnutí. Můžete také vybrat v této úrovni hello okno monitorování podrobnosti, které byste chtěli tooenable pro virtuální počítač.

![Povolit nebo zakázat monitorování prostřednictvím hello portálu Azure.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a>Povolit / zakázat monitorování pomocí rozhraní příkazového řádku Azure

tooenable monitorování pro virtuální počítač Azure.

* Vytvořte soubor (s názvem například PrivateConfig.json):

```json
{
        "storageAccountName":"hello storage account tooreceive data",
        "storageAccountKey":"hello key of hello account"
}
```

* Povolte rozšíření hello prostřednictvím rozhraní příkazového řádku Azure.

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

Další informace najdete v tématu [výkonu pomocí rozšíření diagnostiky Linux tooMonitor Linux Virtuálního počítače a diagnostických dat](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
