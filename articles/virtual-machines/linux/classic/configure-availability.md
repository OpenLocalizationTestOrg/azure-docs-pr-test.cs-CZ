---
title: "Sady dostupnosti pro klasické virtuální počítače Linux | Microsoft Docs"
description: "Nakonfigurujte sadu dostupnosti pro nové nebo existující virtuální počítač s Linuxem v modelu nasazení classic pomocí portálu Azure a prostředí Azure PowerShell."
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
ms.openlocfilehash: 41d427862150d17e1ad726afc51114d6f62f5a8e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-an-availability-set-for-linux-virtual-machines-in-the-classic-deployment-model"></a><span data-ttu-id="51f3a-103">Jak nakonfigurovat sadu dostupnosti pro virtuální počítače s Linuxem v modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="51f3a-103">How to configure an availability set for Linux virtual machines in the classic deployment model</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="51f3a-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="51f3a-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="51f3a-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="51f3a-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="51f3a-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="51f3a-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="51f3a-107">Můžete také [konfigurace skupiny dostupnosti](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) v nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="51f3a-107">You can also [configure availability sets](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) in Resource Manager deployments.</span></span>

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]