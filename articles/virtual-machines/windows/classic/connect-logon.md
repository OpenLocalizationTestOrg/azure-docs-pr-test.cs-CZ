---
title: "Přihlaste se k classic virtuálního počítače Azure | Microsoft Docs"
description: "Použití portálu Azure k přihlášení k Windows virtuální počítač vytvořený s modelem nasazení classic."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 43d54de7e875de9212c23c49ad0539bf2272a312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a><span data-ttu-id="44dd9-103">Přihlášení k virtuálnímu počítači s Windows pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="44dd9-103">Log on to a Windows virtual machine using the Azure portal</span></span>
<span data-ttu-id="44dd9-104">Na portálu Azure můžete použít **Connect** tlačítko spuštění relace vzdálené plochy a přihlaste se k virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="44dd9-104">In the Azure portal, you use the **Connect** button to start a Remote Desktop session and log on to a Windows VM.</span></span>

<span data-ttu-id="44dd9-105">Chcete se připojit k virtuální počítač s Linuxem?</span><span class="sxs-lookup"><span data-stu-id="44dd9-105">Do you want to connect to a Linux VM?</span></span> <span data-ttu-id="44dd9-106">V tématu [přihlášení do virtuálního počítače se systémem Linux](../../linux/mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="44dd9-106">See [How to log on to a virtual machine running Linux](../../linux/mac-create-ssh-keys.md).</span></span>

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> <span data-ttu-id="44dd9-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="44dd9-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="44dd9-108">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="44dd9-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="44dd9-109">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="44dd9-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="44dd9-110">Informace o tom, jak se přihlásit k virtuálnímu počítači pomocí modelu Resource Manager najdete v tématu [zde](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="44dd9-110">For information about how to log on to a VM using the Resource Manager model, see [here](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="44dd9-111">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="44dd9-111">Connect to the virtual machine</span></span>
1. <span data-ttu-id="44dd9-112">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44dd9-112">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="44dd9-113">Klikněte na virtuální počítač, který chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="44dd9-113">Click on the virtual machine that you want to access.</span></span> <span data-ttu-id="44dd9-114">Název je uveden v **všechny prostředky** podokně.</span><span class="sxs-lookup"><span data-stu-id="44dd9-114">The name is listed in the **All resources** pane.</span></span>

    ![Virtuální počítač – umístění](./media/connect-logon/azureportaldashboard.png)

3. <span data-ttu-id="44dd9-116">Klikněte na tlačítko **Connect** na panelu příkazů na řídicím panelu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="44dd9-116">Click **Connect** on the command bar atop the virtual machine dashboard.</span></span>

    ![Připojit ikonu pro virtuální počítač](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If the **Connect** button isn't available, see the troubleshooting tips at the end of this article.
>
>
-->

## <a name="log-on-to-the-virtual-machine"></a><span data-ttu-id="44dd9-118">Přihlášení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="44dd9-118">Log on to the virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="44dd9-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44dd9-119">Next steps</span></span>
* <span data-ttu-id="44dd9-120">Pokud **Connect** tlačítko je neaktivní nebo máte další problémy s připojením ke vzdálené ploše, zkuste resetuje se konfigurace.</span><span class="sxs-lookup"><span data-stu-id="44dd9-120">If the **Connect** button is inactive or you are having other problems with the Remote Desktop connection, try resetting the configuration.</span></span> <span data-ttu-id="44dd9-121">Klikněte na tlačítko **obnovte vzdálený přístup** na řídicím panelu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="44dd9-121">click **Reset remote access** from the virtual machine dashboard.</span></span>

    ![Resetování vzdálený přístup](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* <span data-ttu-id="44dd9-123">Řešení problémů s heslo zkuste resetovat ho.</span><span class="sxs-lookup"><span data-stu-id="44dd9-123">For problems with your password, try resetting it.</span></span> <span data-ttu-id="44dd9-124">Klikněte na tlačítko **resetovat heslo** podél levého okraje virtuálního počítače v části řídicího panelu, **podporu + Poradce při potížích s**.</span><span class="sxs-lookup"><span data-stu-id="44dd9-124">Click **Reset password** along the left edge of virtual machine dashboard, under **Support + Troubleshooting**.</span></span>

    ![Resetování hesla](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

<span data-ttu-id="44dd9-126">Pokud tyto tipy nefungují nebo nejsou, co potřebujete, najdete v části [řešení potíží s vzdálené plochy připojení k systému Windows Azure virtuální počítač](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="44dd9-126">If those tips don't work or aren't what you need, see [Troubleshoot Remote Desktop connections to a Windows-based Azure Virtual Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="44dd9-127">Tento článek vás provede diagnostikou a řešením běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="44dd9-127">This article walks you through diagnosing and resolving common problems.</span></span>
