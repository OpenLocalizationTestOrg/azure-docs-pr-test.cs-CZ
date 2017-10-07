---
title: "aaaCreate plně kvalifikovaný název domény pro virtuální počítač s Linuxem v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény pro správce prostředků na základě virtuálního počítače v hello portálu Azure."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1494a0cb1caa62069c72096a739aee111ac8b383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-linux-vm"></a>Vytvoření platný plně kvalifikovaný název domény v hello portál Azure pro virtuální počítač s Linuxem

Když vytvoříte virtuální počítač (VM) v hello [portál Azure](https://portal.azure.com), se automaticky vytvoří prostředek veřejné IP pro virtuální počítač hello. Můžete použít tuto IP adresu tooremotely přístup hello virtuálních počítačů. Přestože portál hello nevytvoří [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), nebo plně kvalifikovaný název domény, můžete přidat až hello virtuální počítač je vytvořen. Tento článek ukazuje hello kroky toocreate název DNS nebo plně kvalifikovaný název domény.

## <a name="create-a-fqdn"></a>Vytvoření plně kvalifikovaný název domény
Tento článek předpokládá, že jste již vytvořili virtuální počítač. V případě potřeby můžete [vytvoření virtuálního počítače na portálu hello](quick-create-portal.md) nebo [s hello rozhraní příkazového řádku Azure](quick-create-cli.md). Po nastavení a spuštění virtuálního počítače, postupujte takto:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Nyní můžete přihlásit vzdáleně toohello název virtuálního počítače pomocí této služby DNS, jako s `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Další kroky
Nyní, když virtuální počítač veřejný název IP a DNS, můžete nasadit společné architektury aplikace nebo služby, jako je například nginx, MongoDB, Docker, atd.

Také další informace o [pomocí Resource Manager](../../azure-resource-manager/resource-group-overview.md) tipy k sestavení Azure nasazení.

