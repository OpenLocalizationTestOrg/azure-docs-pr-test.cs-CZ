---
title: "aaaConnect virtuálních počítačů Windows v cloudové službě | Microsoft Docs"
description: "Připojte virtuální počítače s Windows, které jsou vytvořené pomocí tooan modelu nasazení classic hello cloudové služby Azure nebo virtuální sítě."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: d19dc555694eab8a7e790c970cfb5e6a53aa7a7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-virtual-machines-created-with-hello-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Připojit virtuální počítače s Windows, které jsou vytvořené pomocí modelu nasazení classic hello pomocí virtuální sítě nebo cloudové služby
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Virtuální počítače s Windows, které jsou vytvořené pomocí modelu nasazení classic hello jsou vždy umístěny v cloudové službě. Cloudová služba Hello slouží jako kontejner a poskytuje jedinečný veřejný název DNS, veřejnou IP adresu a sada koncových bodů tooaccess hello virtuálního počítače přes hello Internet. Hello cloudové služby může být ve virtuální síti, ale není povinné. Můžete také [připojit virtuální počítače s Linuxem pomocí virtuální sítě nebo cloudové služby](../../linux/classic/connect-vms.md).

Pokud cloudové služby není ve virtuální síti, se nazývá *samostatné* Cloudová služba. Hello virtuální počítače v cloudové službě samostatné komunikovat s jinými virtuálními počítači pomocí hello veřejné názvy DNS jiné virtuální počítače a provoz hello přenášena přes hello Internetu. Pokud cloudové služby ve virtuální síti, hello virtuální počítače v tomto cloudové služby může komunikovat s všechny ostatní virtuální počítače ve virtuální síti hello bez odeslání veškerou komunikaci přes hello Internet.

Zadáte-li virtuální počítače v hello stejné samostatnou cloudovou službu, můžete nadále používat, Vyrovnávání zatížení a sady dostupnosti. Podrobnosti najdete v tématu [virtuální počítače Vyrovnávání zatížení](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [spravovat hello dostupnosti virtuálních počítačů](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Nelze však uspořádání hello virtuální počítače v podsítích nebo připojit samostatné cloudové služby tooyour místní síť. Tady je příklad:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření virtuálního počítače, je vhodné příliš[přidat datový disk](attach-disk.md) aby dat toostore umístění služby a úlohy.
