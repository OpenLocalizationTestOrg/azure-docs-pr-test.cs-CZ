---
title: "aaaCreate virtuální počítač s statickou veřejnou IP adresu - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální počítač s statickou veřejnou IP adresu pomocí portálu Azure."
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
ms.openlocfilehash: f74d2132785f06148757409ee0a44b98d1e4b98e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-portal"></a><span data-ttu-id="ce9f8-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ce9f8-103">Create a VM with a static public IP address using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ce9f8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ce9f8-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="ce9f8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce9f8-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="ce9f8-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ce9f8-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="ce9f8-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="ce9f8-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="ce9f8-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="ce9f8-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="ce9f8-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ce9f8-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ce9f8-110">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a><span data-ttu-id="ce9f8-111">Vytvoření virtuálního počítače se statickou veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="ce9f8-111">Create a VM with a static public IP</span></span>

<span data-ttu-id="ce9f8-112">toocreate virtuálního počítače se statickou veřejnou IP adresou v hello portál Azure, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce9f8-112">toocreate a VM with a static public IP address in hello Azure portal, complete hello following steps:</span></span>

1. <span data-ttu-id="ce9f8-113">V prohlížeči přejděte toohello [portál Azure](https://portal.azure.com) a v případě potřeby, přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-113">From a browser, navigate toohello [Azure portal](https://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="ce9f8-114">V levém horním rohu hello hello portálu, klikněte na tlačítko **nový**>>**výpočetní**>**Windows Server 2012 R2 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-114">On hello top left hand corner of hello portal, click **New**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span></span>
3. <span data-ttu-id="ce9f8-115">V hello **vybrat model nasazení** vyberte **Resource Manager** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-115">In hello **Select a deployment model** list, select **Resource Manager** and click **Create**.</span></span>
4. <span data-ttu-id="ce9f8-116">V hello **Základy** okno, zadejte informace o hello virtuálních počítačů, jak je uvedeno níže a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-116">In hello **Basics** blade, enter hello VM information as shown below, and then click **OK**.</span></span>
   
    ![Portál Azure – základy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. <span data-ttu-id="ce9f8-118">V hello **zvolte velikost** okně klikněte na tlačítko **A1 standardní** jak je uvedeno níže a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-118">In hello **Choose a size** blade, click **A1 Standard** as shown below, and then click **Select**.</span></span>
   
    ![Portál Azure – zvolte velikost](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. <span data-ttu-id="ce9f8-120">V hello **nastavení** okně klikněte na tlačítko **veřejnou IP adresu**, potom v hello **vytvoření veřejné IP adresy** okno, v části **přiřazení**, klikněte na **Statické** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-120">In hello **Settings** blade, click **Public IP address**, then in hello **Create public IP address** blade, under **Assignment**, click **Static** as shown below.</span></span> <span data-ttu-id="ce9f8-121">A pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-121">And then click **OK**.</span></span>
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. <span data-ttu-id="ce9f8-123">V hello **nastavení** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-123">In hello **Settings** blade, click **OK**.</span></span>
8. <span data-ttu-id="ce9f8-124">Zkontrolujte hello **Souhrn** okno, jak je uvedeno níže a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-124">Review hello **Summary** blade, as shown below, and then click **OK**.</span></span>
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. <span data-ttu-id="ce9f8-126">Všimněte si hello nové dlaždice na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="ce9f8-126">Notice hello new tile in your dashboard.</span></span>
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. <span data-ttu-id="ce9f8-128">Po vytvoření virtuálního počítače hello hello **nastavení** okno se zobrazí, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="ce9f8-128">Once hello VM is created, hello **Settings** blade will be displayed as shown below</span></span>
    
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

