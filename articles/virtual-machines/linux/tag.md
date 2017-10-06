---
title: "aaaHow tootag se virtuální počítač Azure Linux | Microsoft Docs"
description: "Informace o označování se virtuální počítač Azure Linux vytvořené v Azure pomocí modelu nasazení Resource Manager hello."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 456b226af4495c3b446cb79c99cf9494dde9fca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a>Jak tootag virtuální počítač s Linuxem v Azure
Tento článek popisuje různé způsoby tootag virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager hello. Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů. Azure aktuálně podporuje až too15 značky na prostředků a skupina prostředků. Značky mohou být uvedeny na prostředku v době vytvoření hello nebo přidat tooan existující prostředek. Poznámka: značky jsou podporovány pro vytvořené prostřednictvím hello modelu nasazení Resource Manager pouze prostředky.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Označování pomocí rozhraní příkazového řádku Azure
toobegin, budete potřebovat hello nejnovější [2.0 rozhraní příkazového řádku Azure (Preview)](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Můžete také provést tyto kroky hello [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Můžete zobrazit všechny vlastnosti pro daný virtuální počítač, včetně hello značky, použití tohoto příkazu:

        az vm show --resource-group MyResourceGroup --name MyTestVM

tooadd novou značku virtuálních počítačů prostřednictvím hello rozhraní příkazového řádku Azure, můžete použít hello `azure vm update` příkazu společně s hello značky parametr **– nastavte**:

        az vm update --resource-group MyResourceGroup --name MyTestVM –-set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2

tooremove značky, můžete použít hello **– odebrat** parametr v hello `azure vm update` příkaz.

        az vm update –-resource-group MyResourceGroup –-name MyTestVM --remove tags.myNewTagName1


Teď, když jsme provedli značky tooour prostředky Azure CLI a hello portál, Podívejme se na toosee podrobnosti o využití hello hello značky hello fakturace portálu.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [tooorganize pomocí značky vašich prostředků Azure] [ Using Tags tooorganize your Azure Resources].
* toosee způsobu značky vám může pomoci spravovat používání prostředky Azure, najdete v [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure] [Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
