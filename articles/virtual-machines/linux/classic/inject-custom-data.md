---
title: "aaaInject data do virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Toto téma popisuje, jak tooinject vlastních dat do Azure virtuální počítače, když je vytvořena hello instance a jak toolocate hello vlastních dat v systému Windows nebo Linux."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: d376093c-e01d-4ee3-a826-2b5a35caaa7e
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/23/2016
ms.author: rasquill
ms.openlocfilehash: a3197e06a8d367eab6336577e5cfb6d2d6858441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="injecting-custom-data-into-an-azure-virtual-machine"></a><span data-ttu-id="b1840-103">Vložení vlastní data do virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="b1840-103">Injecting custom data into an Azure virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="b1840-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b1840-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b1840-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="b1840-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="b1840-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="b1840-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="b1840-107">Informace o pomocí modelu Resource Manager hello hello rozšíření vlastních skriptů najdete v tématu [zde](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b1840-107">For information about using hello Custom Script Extension with hello Resource Manager model, see [here](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-inject-custom-data](../../../../includes/virtual-machines-common-classic-inject-custom-data.md)]
