---
title: "Nastaví aaaAvailability pro klasické virtuální počítače Linux | Microsoft Docs"
description: "Nakonfigurujte sadu dostupnosti pro nové nebo existující virtuální počítač s Linuxem v modelu nasazení classic hello pomocí hello portál Azure a prostředí Azure PowerShell."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b8624315-beca-4ec7-8441-2e98b166b548
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: cynthn
ms.openlocfilehash: 8d8d041e3540e42a1921f5665469a2fdcaa30a29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-availability-set-for-linux-virtual-machines-in-hello-classic-deployment-model"></a><span data-ttu-id="a0544-103">Jak tooconfigure sadu dostupnosti pro virtuální počítače s Linuxem v modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="a0544-103">How tooconfigure an availability set for Linux virtual machines in hello classic deployment model</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="a0544-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a0544-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a0544-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="a0544-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="a0544-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="a0544-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="a0544-107">Můžete také [konfigurace skupiny dostupnosti](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) v nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0544-107">You can also [configure availability sets](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) in Resource Manager deployments.</span></span>

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]