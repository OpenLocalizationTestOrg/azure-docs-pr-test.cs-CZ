---
title: "O bitové kopie virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o použití bitové kopie systému Linux s virtuálními počítači v Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: e6ea8adc-4e7a-467a-9394-cd05e67898b7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/21/2016
ms.author: cynthn
ms.openlocfilehash: 187642db18806f4034dcecf4c25b5c71028fdfe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-images-for-linux-virtual-machines"></a><span data-ttu-id="45747-103">O bitových kopií pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="45747-103">About images for Linux virtual machines</span></span>
> [!IMPORTANT]
> <span data-ttu-id="45747-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="45747-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="45747-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="45747-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="45747-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="45747-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="45747-107">Informace o bitových kopiích pomocí modelu Resource Manager najdete v tématu [zde](../cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45747-107">For information about images using the Resource Manager model, see [here](../cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a><span data-ttu-id="45747-108">Práce s obrázky</span><span class="sxs-lookup"><span data-stu-id="45747-108">Working with images</span></span>
<span data-ttu-id="45747-109">Azure rozhraní příkazového řádku (CLI) pro Mac, Linux a Windows můžete použít ke správě bitových kopií, která je k dispozici k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="45747-109">You can use the Azure Command-Line Interface (CLI) for Mac, Linux, and Windows to manage the images available to your Azure subscription.</span></span> <span data-ttu-id="45747-110">Také můžete použít portál Azure pro některé úlohy bitové kopie, ale příkazového řádku získáte další možnosti.</span><span class="sxs-lookup"><span data-stu-id="45747-110">You also can use the Azure portal for some image tasks, but the command line gives you more options.</span></span>

<span data-ttu-id="45747-111">Příklady použití nástroje najdete v tématu [společné rozhraní příkazového řádku Azure na Linuxu a Macu](../cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45747-111">For examples of using the tools, see [Common Azure CLI commands on Linux and Mac](../cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="45747-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45747-112">Next steps</span></span>
<span data-ttu-id="45747-113">Můžete také [nahrát vlastní image](create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="45747-113">You can also [upload your own image](create-upload-vhd.md).</span></span>
