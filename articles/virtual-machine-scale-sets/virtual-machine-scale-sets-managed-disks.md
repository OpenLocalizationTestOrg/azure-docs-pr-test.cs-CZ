---
title: "aaaUsing spravované disky s Azure sady škálování virtuálního počítače | Microsoft Docs"
description: "Zjistěte, proč a jak spravovat toouse disky s sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 0e2a21e9f8b114ae1c8b81e1e6124621366f5643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Škálovací sady virtuálních počítačů Azure a spravované disky

[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure podporují virtuální počítače se spravovanými disky. Použití spravovaných disků se škálovacími sadami má několik výhod:

* Již nepotřebujete toopre – vytvoření a Správa disků hello OS toostore účty úložiště pro virtuální počítače sady škálování hello.

* Spravované datové disky toohello škálovací sadu můžete připojit.

* Při použití spravovaného disku může mít škálovací sada kapacitu až 1 000 virtuálních počítačů, pokud je založená na imagi platformy, nebo 100 virtuálních počítačů, pokud je založená na vlastní imagi.

## <a name="get-started"></a>Začínáme

Jednoduchý způsob tooget začít s sady škálování spravovaného disku je toodeploy jeden z hello portálu Azure. Další informace najdete v [tomto článku](./virtual-machine-scale-sets-portal-create.md). Jiné jednoduchý způsob tooget spuštění je toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy sady škálování virtuálního počítače. Hello následující příklad ukazuje, jak toocreate Ubuntu na základě škálování s 10 virtuálních počítačů, každý s 50 GB a 100 GB datový disk:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Alternativně může hledat v hello [úložiště GitHub šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) pro složky, které obsahují `vmss` toosee předem vytvořené Příklady šablon, které nasadit sady škálování. tootell šablony, které už používáte spravovaných disků, naleznete v části příliš[tento seznam](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## <a name="next-steps"></a>Další kroky

Další obecné informace o spravovaných discích najdete v [tomto článku](../virtual-machines/windows/managed-disks-overview.md).

toosee způsob tooconvert nastaví škálování tooprovision šablony Resource Manageru pomocí správy disků, najdete v části [v tomto článku](./virtual-machine-scale-sets-convert-template-to-md.md). Hello stejné šablony Resource Manageru toohello změny použít také toohello rozhraní REST API Azure.

toolearn Další informace o spravovaných datových disků pomocí sad škálování, najdete v části [v tomto článku](./virtual-machine-scale-sets-attached-disks.md).

Práce se sadami velkém měřítku, toobegin odkazovat příliš[v tomto článku](./virtual-machine-scale-sets-placement-groups.md).


