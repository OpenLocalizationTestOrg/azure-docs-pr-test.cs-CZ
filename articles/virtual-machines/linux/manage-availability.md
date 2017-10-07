---
title: "dostupnost hello aaaManage virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse více virtuálních počítačů tooensure vysoká dostupnost pro Linux aplikaci v Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 891c852a-84c0-4940-a61e-ada6e185bf37
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 523d45a6c65b6b3255d82c96defb8e7302f14fc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-availability-of-linux-virtual-machines"></a><span data-ttu-id="75e8d-103">Spravovat hello dostupnosti virtuálních počítačů Linux</span><span class="sxs-lookup"><span data-stu-id="75e8d-103">Manage hello availability of Linux virtual machines</span></span>

<span data-ttu-id="75e8d-104">Další způsoby tooset nahoru a spravovat více virtuálních počítačů tooensure vysoká dostupnost pro Linux aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="75e8d-104">Learn ways tooset up and manage multiple virtual machines tooensure high availability for your Linux application in Azure.</span></span> <span data-ttu-id="75e8d-105">Můžete také [spravovat virtuální počítače s Windows hello dostupnost](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="75e8d-105">You can also [manage hello availability of Windows virtual machines](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="75e8d-106">Pokyny pro vytvoření sadu pomocí rozhraní příkazového řádku v modelu nasazení Resource Manager hello dostupnosti, najdete v části [azure availset: Nastaví vaší dostupnosti příkazy toomanage](../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets).</span><span class="sxs-lookup"><span data-stu-id="75e8d-106">For instructions on creating an availability set using CLI in hello Resource Manager deployment model, see [azure availset: commands toomanage your availability sets](../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets).</span></span>

[!INCLUDE [virtual-machines-common-manage-availability](../../../includes/virtual-machines-common-manage-availability.md)]

## <a name="next-steps"></a><span data-ttu-id="75e8d-107">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75e8d-107">Next steps</span></span>
<span data-ttu-id="75e8d-108">najdete v části Další informace o virtuálních počítačích, Vyrovnávání zatížení toolearn [Vyrovnávání zatížení virtuálních počítačů](../virtual-machines-linux-load-balance.md).</span><span class="sxs-lookup"><span data-stu-id="75e8d-108">toolearn more about load balancing your virtual machines, see [Load Balancing virtual machines](../virtual-machines-linux-load-balance.md).</span></span>

