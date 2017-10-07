---
title: "aaaCreate plně kvalifikovaný název domény pro virtuální počítač s Windows v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény pro správce prostředků na základě virtuálního počítače v hello portálu Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 67c817ec97073803e513bc41ebde67b75ced565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-windows-vm"></a>Vytvoření platný plně kvalifikovaný název domény v hello portál Azure pro virtuální počítač s Windows

Když vytvoříte virtuální počítač (VM) v hello [portál Azure](https://portal.azure.com), se automaticky vytvoří prostředek veřejné IP pro virtuální počítač hello. Můžete použít tuto IP adresu tooremotely přístup hello virtuálních počítačů. Přestože portál hello nevytvoří [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, můžete vytvořit jeden po hello virtuální počítač je vytvořen. Tento článek ukazuje hello kroky toocreate název DNS nebo plně kvalifikovaný název domény.

## <a name="create-a-fqdn"></a>Vytvoření plně kvalifikovaný název domény
Tento článek předpokládá, že jste již vytvořili virtuální počítač. V případě potřeby můžete [vytvoření virtuálního počítače na portálu hello](quick-create-portal.md) nebo [s prostředím Azure PowerShell](quick-create-powershell.md). Po nastavení a spuštění virtuálního počítače, postupujte takto:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Nyní můžete přihlásit vzdáleně toohello virtuálního počítače pomocí tento název DNS, jako pro protokol RDP (Remote Desktop).

## <a name="next-steps"></a>Další kroky
Nyní, když virtuální počítač veřejný název IP a DNS, můžete nasadit společné architektury aplikace nebo služby, například služby IIS, SQL nebo SharePoint.

Také další informace o [pomocí Resource Manager](../../azure-resource-manager/resource-group-overview.md) tipy k sestavení Azure nasazení.

