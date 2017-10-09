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
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a><span data-ttu-id="b7730-103">Řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálních počítačů Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="b7730-103">Troubleshoot allocation failures when you create, restart, or resize Linux VMs in Azure</span></span>
<span data-ttu-id="b7730-104">Při vytvoření virtuálního počítače, restartujte zastaveném (deallocated) virtuálních počítačů nebo změnit velikost virtuálního počítače Microsoft Azure přiřadí výpočetní prostředky tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="b7730-104">When you create a VM, restart stopped (deallocated) VMs, or resize a VM, Microsoft Azure allocates compute resources tooyour subscription.</span></span> <span data-ttu-id="b7730-105">Příležitostně zobrazí chyby při provádění těchto operací – ještě před dosažením hello limity předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="b7730-105">You may occasionally receive errors when performing these operations -- even before you reach hello Azure subscription limits.</span></span> <span data-ttu-id="b7730-106">Tento článek vysvětluje hello příčiny některých běžných chyb přidělení hello a navrhne možné nápravu.</span><span class="sxs-lookup"><span data-stu-id="b7730-106">This article explains hello causes of some of hello common allocation failures and suggests possible remediation.</span></span> <span data-ttu-id="b7730-107">Hello informace může být užitečné také při plánování nasazení hello vašich služeb.</span><span class="sxs-lookup"><span data-stu-id="b7730-107">hello information may also be useful when you plan hello deployment of your services.</span></span> <span data-ttu-id="b7730-108">Můžete také [řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálních počítačů Windows v Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7730-108">You can also [troubleshoot allocation failures when you create, restart, or resize Windows VMs in Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

