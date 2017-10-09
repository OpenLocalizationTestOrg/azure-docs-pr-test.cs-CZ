---
title: "virtuální počítač s Linuxem aaaTroubleshooting chyby v přidělení | Microsoft Docs"
description: "Řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálního počítače s Linuxem v Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cjiang
ms.openlocfilehash: 502fbb406b0b4acf086c2586795f69a44cc1a004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálních počítačů Linux v Azure
Při vytvoření virtuálního počítače, restartujte zastaveném (deallocated) virtuálních počítačů nebo změnit velikost virtuálního počítače Microsoft Azure přiřadí výpočetní prostředky tooyour předplatné. Příležitostně zobrazí chyby při provádění těchto operací – ještě před dosažením hello limity předplatného Azure. Tento článek vysvětluje hello příčiny některých běžných chyb přidělení hello a navrhne možné nápravu. Hello informace může být užitečné také při plánování nasazení hello vašich služeb. Můžete také [řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálních počítačů Windows v Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

