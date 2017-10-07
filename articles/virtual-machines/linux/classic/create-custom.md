---
title: "aaaCreate klasické virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač s Linuxem pomocí Azure CLI 1.0 hello hello model nasazení Classic"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 5c3c54e5d1444a79e8e609c76d04a3a621c8d03f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-classic-linux-vm-with-hello-azure-cli-10"></a>Jak hello tooCreate a klasické virtuální počítač s Linuxem pomocí Azure CLI 1.0
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Verze hello Resource Manager, najdete v části [zde](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Toto téma popisuje, jak toocreate Linux virtuálního počítače (VM) s použitím hello Azure CLI 1.0 hello model nasazení Classic. Používáme image systému Linux z hello k dispozici **bitové kopie** v Azure. příkazy Hello Azure CLI 1.0 poskytnout hello následující možnosti konfigurace, mimo jiné:

* Propojení virtuální sítě tooa hello virtuálních počítačů
* Přidávání virtuálních počítačů hello tooan stávající cloudovou službu
* Přidání hello virtuálních počítačů tooan stávající účet úložiště
* Přidání nastavení dostupnosti tooan Virtuálního hello nebo umístění

> [!IMPORTANT]
> Pokud chcete, aby váš počítač toouse virtuální sítě, aby se mohli připojit tooit přímo podle názvu hostitele nebo nastavte připojení mezi různými místy, ujistěte se, když vytvoříte hello virtuálního počítače zadejte hello virtuální sítě. Virtuální počítač může být nakonfigurované toojoin virtuální sítě, pouze když vytváříte hello virtuálních počítačů. Podrobnosti virtuální sítě najdete v tématu [Přehled virtuálních sítí Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-toocreate-a-linux-vm-using-hello-classic-deployment-model"></a>Jak hello toocreate a virtuální počítač s Linuxem pomocí modelu nasazení Classic
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

