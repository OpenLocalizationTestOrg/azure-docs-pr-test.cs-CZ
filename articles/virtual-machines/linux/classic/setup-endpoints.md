---
title: "aaaSet až koncových bodů na klasické virtuální počítač s Linuxem | Microsoft Docs"
description: "Přečtěte si informace tooset až koncové body pro virtuální počítač s Linuxem v hello Azure classic portálu tooallow komunikaci s virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 1c959d10dd1e20228fa4a20e1cc0205c1d12f185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Jak tooset až koncových bodů na klasické virtuální počítač s Linuxem v Azure
Všechny virtuální počítače Linux, které vytvoříte v Azure pomocí modelu nasazení classic hello můžete automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači v hello že stejné cloudové služby nebo virtuální sítě. Počítače na hello internetové nebo jiné virtuální sítě však vyžadují koncové body toodirect hello příchozích síťových přenosů tooa virtuálního počítače. Tento článek je také k dispozici pro [virtuální počítače s Windows](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

V hello **Resource Manager** modelu nasazení koncové body jsou konfigurováni pomocí **skupiny zabezpečení sítě (Nsg)**. Další informace najdete v tématu [otevírání portů a koncové body](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Když vytvoříte virtuální počítač s Linuxem v hello portálu Azure, koncový bod pro Secure Shell (SSH) je obvykle vám vytvoří automaticky. Při vytváření hello virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Další kroky
* Můžete také vytvořit koncový bod virtuálního počítače pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Spustit hello **vytvořit koncový bod virtuálního počítače azure** příkaz.
* Pokud jste vytvořili virtuální počítač v modelu nasazení Resource Manager hello, můžete použít hello rozhraní příkazového řádku Azure v režimu Resource Manager příliš[vytvoření skupin zabezpečení sítě](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toohello toocontrol přenosy virtuálních počítačů.
