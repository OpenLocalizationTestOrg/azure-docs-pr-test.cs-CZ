---
title: "aaaInstall MongoDB na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit tooinstall MongoDB ve virtuálním počítači Azure s modelem nasazení classic hello systémem Windows Server."
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
ms.openlocfilehash: aafd86b4c49c6a97ae8a1a2191ae375b6c47483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="3ee28-103">Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="3ee28-103">Install MongoDB on a Windows VM in Azure</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3ee28-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3ee28-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="3ee28-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="3ee28-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="3ee28-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="3ee28-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="3ee28-107">tooinstall a nakonfigurovat pomocí modelu nasazení Resource Manager hello MongoDB, najdete v části [v tomto článku](../install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="3ee28-107">tooinstall and configure MongoDB using hello Resource Manager deployment model, see [this article](../install-mongodb.md).</span></span>

<span data-ttu-id="3ee28-108">[MongoDB] [ MongoDB] je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="3ee28-108">[MongoDB][MongoDB] is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="3ee28-109">Tento článek vás provede vytvořením virtuálního počítače (VM) Windows serveru pomocí hello [portál Azure][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="3ee28-109">This article guides you through creating a Windows Server virtual machine (VM) using hello [Azure portal][AzurePortal].</span></span> <span data-ttu-id="3ee28-110">Pak vytvořte a připojte disk toohello data virtuálních počítačů před instalací a konfigurací MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3ee28-110">You then create and attach a data disk toohello VM before installing and configuring MongoDB.</span></span> <span data-ttu-id="3ee28-111">Pokud máte existující virtuální počítač v Azure, které chcete toouse, můžete přejít rovnou příliš[instalace a konfigurace MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="3ee28-111">If you have an existing VM in Azure that you would like toouse, you can jump straight too[installing and configuring MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span></span>

## <a name="create-a-virtual-machine-running-windows-server"></a><span data-ttu-id="3ee28-112">Vytvoření virtuálního počítače se systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="3ee28-112">Create a virtual machine running Windows Server</span></span>
<span data-ttu-id="3ee28-113">Postupujte podle těchto pokynů toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3ee28-113">Follow these instructions toocreate a virtual machine.</span></span>

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> <span data-ttu-id="3ee28-114">Přidání koncového bodu pro MongoDB při vytváření hello virtuálního počítače a nakonfigurovat následujícím způsobem: pojmenujte ji jako **Mongo**, použijte **TCP** jako hello protokolu a nastavte oba hello veřejné a privátní porty příliš**27017**.</span><span class="sxs-lookup"><span data-stu-id="3ee28-114">You can add an endpoint for MongoDB while creating hello virtual machine, and configure it as follows: name it as **Mongo**, use **TCP** as hello protocol, and set both hello public and private ports too**27017**.</span></span>
>
>

## <a name="attach-a-data-disk"></a><span data-ttu-id="3ee28-115">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="3ee28-115">Attach a data disk</span></span>
<span data-ttu-id="3ee28-116">tooprovide úložiště pro virtuální počítač hello, připojit datový disk a potom inicializujte tak, aby Windows můžete používat.</span><span class="sxs-lookup"><span data-stu-id="3ee28-116">tooprovide storage for hello virtual machine, attach a data disk and then initialize it so that Windows can use it.</span></span> <span data-ttu-id="3ee28-117">Pokud již máte datový disk, můžete připojit tento stávající disk, nebo můžete připojte prázdný disk.</span><span class="sxs-lookup"><span data-stu-id="3ee28-117">If you already have a data disk, you can attach that existing disk, or you can attach an empty disk.</span></span>

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

<span data-ttu-id="3ee28-118">Pokyny k inicializaci hello disku, najdete v části "postup: inicializovat nový datový disk ve Windows serveru" v [jak tooattach datový disk virtuálního počítače Windows tooa](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="3ee28-118">For instructions on initializing hello disk, see "How to: Initialize a new data disk in Windows Server" in [How tooattach a data disk tooa Windows virtual machine](attach-disk.md).</span></span>

## <a name="install-and-run-mongodb-on-hello-virtual-machine"></a><span data-ttu-id="3ee28-119">Nainstalujte a spusťte MongoDB hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3ee28-119">Install and run MongoDB on hello virtual machine</span></span>
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a><span data-ttu-id="3ee28-120">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3ee28-120">Summary</span></span>
<span data-ttu-id="3ee28-121">V tomto kurzu jste se dozvěděli, jak toocreate virtuálního počítače se systémem Windows Server vzdáleně připojit tooit a připojit datový disk.</span><span class="sxs-lookup"><span data-stu-id="3ee28-121">In this tutorial, you learned how toocreate a virtual machine running Windows Server, remotely connect tooit, and attach a data disk.</span></span>  <span data-ttu-id="3ee28-122">Také jste se naučili jak tooinstall a konfigurace virtuálního počítače založené na Windows hello MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3ee28-122">You also learned how tooinstall and configure MongoDB on hello Windows-based virtual machine.</span></span> <span data-ttu-id="3ee28-123">Nyní můžete k MongoDB hello systému Windows virtuálního počítače pomocí následující hello advanced témata v hello [dokumentace k MongoDB][MongoDocs].</span><span class="sxs-lookup"><span data-stu-id="3ee28-123">You can now access MongoDB on hello Windows-based virtual machine, by following hello advanced topics in hello [MongoDB documentation][MongoDocs].</span></span>

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

