---
title: "Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Informace o instalaci MongoDB ve virtuálním počítači Azure vytvořené pomocí modelu nasazení classic systémem Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: 6b5af18d02fd508a21cdc21b38b1c16e79f07ecb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="ed73a-103">Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="ed73a-103">Install MongoDB on a Windows VM in Azure</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ed73a-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ed73a-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="ed73a-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="ed73a-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="ed73a-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ed73a-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="ed73a-107">Instalace a konfigurace MongoDB pomocí modelu nasazení Resource Manager, najdete v části [v tomto článku](../install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="ed73a-107">To install and configure MongoDB using the Resource Manager deployment model, see [this article](../install-mongodb.md).</span></span>

<span data-ttu-id="ed73a-108">[MongoDB] [ MongoDB] je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="ed73a-108">[MongoDB][MongoDB] is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="ed73a-109">Tento článek vás provede vytvořením virtuální počítač (VM) Windows serveru pomocí [portál Azure][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="ed73a-109">This article guides you through creating a Windows Server virtual machine (VM) using the [Azure portal][AzurePortal].</span></span> <span data-ttu-id="ed73a-110">Vytvořit a připojit datový disk k virtuálnímu počítači před instalací a konfigurací MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ed73a-110">You then create and attach a data disk to the VM before installing and configuring MongoDB.</span></span> <span data-ttu-id="ed73a-111">Pokud máte existující virtuální počítač v Azure, který chcete použít, můžete přejít rovnou na [instalace a konfigurace MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="ed73a-111">If you have an existing VM in Azure that you would like to use, you can jump straight to [installing and configuring MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span></span>

## <a name="create-a-virtual-machine-running-windows-server"></a><span data-ttu-id="ed73a-112">Vytvoření virtuálního počítače se systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="ed73a-112">Create a virtual machine running Windows Server</span></span>
<span data-ttu-id="ed73a-113">Postupujte podle těchto pokynů můžete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ed73a-113">Follow these instructions to create a virtual machine.</span></span>

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> <span data-ttu-id="ed73a-114">Přidání koncového bodu pro MongoDB při vytváření virtuálního počítače a nakonfigurovat následujícím způsobem: pojmenujte ji jako **Mongo**, použijte **TCP** jako protokol a nastavte veřejné a privátní porty na **27017**.</span><span class="sxs-lookup"><span data-stu-id="ed73a-114">You can add an endpoint for MongoDB while creating the virtual machine, and configure it as follows: name it as **Mongo**, use **TCP** as the protocol, and set both the public and private ports to **27017**.</span></span>
>
>

## <a name="attach-a-data-disk"></a><span data-ttu-id="ed73a-115">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="ed73a-115">Attach a data disk</span></span>
<span data-ttu-id="ed73a-116">K poskytování úložiště pro virtuální počítač, připojit datový disk a potom inicializujte tak, aby Windows můžete používat.</span><span class="sxs-lookup"><span data-stu-id="ed73a-116">To provide storage for the virtual machine, attach a data disk and then initialize it so that Windows can use it.</span></span> <span data-ttu-id="ed73a-117">Pokud již máte datový disk, můžete připojit tento stávající disk, nebo můžete připojte prázdný disk.</span><span class="sxs-lookup"><span data-stu-id="ed73a-117">If you already have a data disk, you can attach that existing disk, or you can attach an empty disk.</span></span>

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

<span data-ttu-id="ed73a-118">Pokyny pro inicializaci disku, v tématu "postup: Inicializace nový datový disk ve Windows serveru" v [jak připojit datový disk k virtuálnímu počítači Windows](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="ed73a-118">For instructions on initializing the disk, see "How to: Initialize a new data disk in Windows Server" in [How to attach a data disk to a Windows virtual machine](attach-disk.md).</span></span>

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a><span data-ttu-id="ed73a-119">Nainstalujte a spusťte MongoDB na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="ed73a-119">Install and run MongoDB on the virtual machine</span></span>
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a><span data-ttu-id="ed73a-120">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ed73a-120">Summary</span></span>
<span data-ttu-id="ed73a-121">V tomto kurzu jste zjistili, jak k vytvoření virtuálního počítače se systémem Windows Server, vzdáleně připojit k němu a připojit datový disk.</span><span class="sxs-lookup"><span data-stu-id="ed73a-121">In this tutorial, you learned how to create a virtual machine running Windows Server, remotely connect to it, and attach a data disk.</span></span>  <span data-ttu-id="ed73a-122">Také jste zjistili, jak nainstalovat a nakonfigurovat MongoDB na virtuálním počítači systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ed73a-122">You also learned how to install and configure MongoDB on the Windows-based virtual machine.</span></span> <span data-ttu-id="ed73a-123">Nyní můžete k MongoDB na virtuálním počítači systému Windows, pomocí následujících Pokročilá témata v [dokumentace k MongoDB][MongoDocs].</span><span class="sxs-lookup"><span data-stu-id="ed73a-123">You can now access MongoDB on the Windows-based virtual machine, by following the advanced topics in the [MongoDB documentation][MongoDocs].</span></span>

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

