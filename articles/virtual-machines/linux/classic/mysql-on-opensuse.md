---
title: "Instalace databáze MySQL na virtuální počítač OpenSUSE | Microsoft Docs"
description: "Další informace pro instalaci databáze MySQL na počítači OpenSUSE Linux VMirtual v Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 01b798a25575b66f89057315ce80d6cc0cde53b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a><span data-ttu-id="4904a-103">Instalace MySQL do virtuálního počítače se spuštěným OpenSUSE Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="4904a-103">Install MySQL on a virtual machine running OpenSUSE Linux in Azure</span></span>
<span data-ttu-id="4904a-104">[MySQL] [ MySQL] je Oblíbené databáze SQL open source.</span><span class="sxs-lookup"><span data-stu-id="4904a-104">[MySQL][MySQL] is a popular, open-source SQL database.</span></span> <span data-ttu-id="4904a-105">V tomto kurzu se dozvíte, jak vytvořit virtuální počítač s Linuxem OpenSUSE a pak nainstalujte MySQL.</span><span class="sxs-lookup"><span data-stu-id="4904a-105">This tutorial shows you how to create a virtual machine running OpenSUSE Linux, then install MySQL.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="4904a-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4904a-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4904a-107">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="4904a-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="4904a-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4904a-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a><span data-ttu-id="4904a-109">Vytvoření virtuálního počítače se systémem OpenSUSE Linux</span><span class="sxs-lookup"><span data-stu-id="4904a-109">Create a virtual machine running OpenSUSE Linux</span></span>
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a><span data-ttu-id="4904a-110">Nainstalujte a spusťte MySQL na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="4904a-110">Install and run MySQL on the virtual machine</span></span>
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="4904a-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4904a-111">Next steps</span></span>
<span data-ttu-id="4904a-112">Podrobnosti o MySQL najdete v tématu [MySQL dokumentaci][MySQLDocs].</span><span class="sxs-lookup"><span data-stu-id="4904a-112">For details about MySQL, see the [MySQL Documentation][MySQLDocs].</span></span>

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

