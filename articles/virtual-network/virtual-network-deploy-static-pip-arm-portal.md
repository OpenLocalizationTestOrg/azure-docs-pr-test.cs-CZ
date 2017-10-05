---
title: "Vytvoření virtuálního počítače se statickou veřejnou IP adresu - portálu Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač se statickou veřejnou IP adresu pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 233e4eea8439320c1c7446e2c2b2e9d379351a3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a><span data-ttu-id="6b509-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6b509-103">Create a VM with a static public IP address using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6b509-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6b509-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="6b509-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6b509-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="6b509-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6b509-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="6b509-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="6b509-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="6b509-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="6b509-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="6b509-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6b509-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6b509-110">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="6b509-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a><span data-ttu-id="6b509-111">Vytvoření virtuálního počítače se statickou veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="6b509-111">Create a VM with a static public IP</span></span>

<span data-ttu-id="6b509-112">Postup vytvoření virtuálního počítače se statickou veřejnou IP adresu na portálu Azure, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6b509-112">To create a VM with a static public IP address in the Azure portal, complete the following steps:</span></span>

1. <span data-ttu-id="6b509-113">V prohlížeči přejděte na portál [Azure Portal](https://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6b509-113">From a browser, navigate to the [Azure portal](https://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="6b509-114">V levém horním rohu portálu, klikněte na tlačítko **nový**>>**výpočetní**>**Windows Server 2012 R2 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="6b509-114">On the top left hand corner of the portal, click **New**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span></span>
3. <span data-ttu-id="6b509-115">V **vybrat model nasazení** vyberte **Resource Manager** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6b509-115">In the **Select a deployment model** list, select **Resource Manager** and click **Create**.</span></span>
4. <span data-ttu-id="6b509-116">V **Základy** okno, zadejte informace o virtuálních počítačů, jak je uvedeno níže a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b509-116">In the **Basics** blade, enter the VM information as shown below, and then click **OK**.</span></span>
   
    ![Portál Azure – základy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. <span data-ttu-id="6b509-118">V **zvolte velikost** okně klikněte na tlačítko **A1 standardní** jak je uvedeno níže a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="6b509-118">In the **Choose a size** blade, click **A1 Standard** as shown below, and then click **Select**.</span></span>
   
    ![Portál Azure – zvolte velikost](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. <span data-ttu-id="6b509-120">V **nastavení** okně klikněte na tlačítko **veřejnou IP adresu**, potom v **vytvoření veřejné IP adresy** okno, v části **přiřazení**, klikněte na tlačítko **statické** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6b509-120">In the **Settings** blade, click **Public IP address**, then in the **Create public IP address** blade, under **Assignment**, click **Static** as shown below.</span></span> <span data-ttu-id="6b509-121">A pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b509-121">And then click **OK**.</span></span>
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. <span data-ttu-id="6b509-123">V **nastavení** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b509-123">In the **Settings** blade, click **OK**.</span></span>
8. <span data-ttu-id="6b509-124">Zkontrolujte **Souhrn** okno, jak je uvedeno níže a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b509-124">Review the **Summary** blade, as shown below, and then click **OK**.</span></span>
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. <span data-ttu-id="6b509-126">Všimněte si nové dlaždice na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="6b509-126">Notice the new tile in your dashboard.</span></span>
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. <span data-ttu-id="6b509-128">Po vytvoření virtuálního počítače **nastavení** okno se zobrazí, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="6b509-128">Once the VM is created, the **Settings** blade will be displayed as shown below</span></span>
    
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

