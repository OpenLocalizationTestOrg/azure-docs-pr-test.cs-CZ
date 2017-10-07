---
title: "aaaInstall MySQL na virtuální počítač OpenSUSE | Microsoft Docs"
description: "Přečtěte si tooinstall MySQL na počítači OpenSUSE Linux VMirtual v Azure."
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
ms.openlocfilehash: 8f96d29f29cb9c466dd7fdaf92b378783fdaacd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a><span data-ttu-id="7e2c4-103">Instalace MySQL do virtuálního počítače se spuštěným OpenSUSE Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="7e2c4-103">Install MySQL on a virtual machine running OpenSUSE Linux in Azure</span></span>
<span data-ttu-id="7e2c4-104">[MySQL] [ MySQL] je Oblíbené databáze SQL open source.</span><span class="sxs-lookup"><span data-stu-id="7e2c4-104">[MySQL][MySQL] is a popular, open-source SQL database.</span></span> <span data-ttu-id="7e2c4-105">Tento kurz ukazuje, jak toocreate virtuálního počítače se systémem OpenSUSE Linux, pak nainstalujte MySQL.</span><span class="sxs-lookup"><span data-stu-id="7e2c4-105">This tutorial shows you how toocreate a virtual machine running OpenSUSE Linux, then install MySQL.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="7e2c4-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7e2c4-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7e2c4-107">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="7e2c4-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="7e2c4-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="7e2c4-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a><span data-ttu-id="7e2c4-109">Vytvoření virtuálního počítače se systémem OpenSUSE Linux</span><span class="sxs-lookup"><span data-stu-id="7e2c4-109">Create a virtual machine running OpenSUSE Linux</span></span>
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-hello-virtual-machine"></a><span data-ttu-id="7e2c4-110">Nainstalujte a spusťte MySQL na virtuálním počítači hello</span><span class="sxs-lookup"><span data-stu-id="7e2c4-110">Install and run MySQL on hello virtual machine</span></span>
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="7e2c4-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e2c4-111">Next steps</span></span>
<span data-ttu-id="7e2c4-112">Podrobnosti o MySQL najdete v tématu hello [MySQL dokumentaci][MySQLDocs].</span><span class="sxs-lookup"><span data-stu-id="7e2c4-112">For details about MySQL, see hello [MySQL Documentation][MySQLDocs].</span></span>

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

