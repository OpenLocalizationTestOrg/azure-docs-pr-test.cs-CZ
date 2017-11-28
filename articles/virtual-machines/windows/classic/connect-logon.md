---
title: "aaaLog na tooa classic virtuálního počítače Azure | Microsoft Docs"
description: "Použijte hello Azure portálu toolog na virtuální počítač Windows tooa vytvořené pomocí modelu nasazení classic hello."
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
ms.openlocfilehash: 2e32b7036c2538e73b46580e0f5f8f4979e8a685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-on-tooa-windows-virtual-machine-using-hello-azure-portal"></a><span data-ttu-id="78f26-103">Přihlaste se tooa Windows virtuálního počítače pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="78f26-103">Log on tooa Windows virtual machine using hello Azure portal</span></span>
<span data-ttu-id="78f26-104">V hello portál Azure používáte hello **Connect** tlačítko toostart relaci vzdálené plochy a přihlaste se tooa virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="78f26-104">In hello Azure portal, you use hello **Connect** button toostart a Remote Desktop session and log on tooa Windows VM.</span></span>

<span data-ttu-id="78f26-105">Chcete, aby tooconnect tooa virtuálního počítače s Linuxem?</span><span class="sxs-lookup"><span data-stu-id="78f26-105">Do you want tooconnect tooa Linux VM?</span></span> <span data-ttu-id="78f26-106">V tématu [jak toolog na tooa virtuální počítač se systémem Linux](../../linux/mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="78f26-106">See [How toolog on tooa virtual machine running Linux](../../linux/mac-create-ssh-keys.md).</span></span>

<!--
Deleting, but not 100% sure
Learn how too[perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> <span data-ttu-id="78f26-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="78f26-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="78f26-108">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="78f26-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="78f26-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="78f26-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="78f26-110">Informace o tom, jak toolog na virtuální počítač pomocí tooa hello Resource Manager modelu najdete v tématu [zde](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="78f26-110">For information about how toolog on tooa VM using hello Resource Manager model, see [here](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="78f26-111">Připojit toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="78f26-111">Connect toohello virtual machine</span></span>
1. <span data-ttu-id="78f26-112">Přihlaste se toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="78f26-112">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="78f26-113">Klikněte na hello virtuálního počítače, které chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="78f26-113">Click on hello virtual machine that you want tooaccess.</span></span> <span data-ttu-id="78f26-114">Název Hello je uvedený v hello **všechny prostředky** podokně.</span><span class="sxs-lookup"><span data-stu-id="78f26-114">hello name is listed in hello **All resources** pane.</span></span>

    ![Virtuální počítač – umístění](./media/connect-logon/azureportaldashboard.png)

3. <span data-ttu-id="78f26-116">Klikněte na tlačítko **Connect** na panelu příkazů hello na řídicí panel hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="78f26-116">Click **Connect** on hello command bar atop hello virtual machine dashboard.</span></span>

    ![Ikona pro virtuální počítač hello připojení](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If hello **Connect** button isn't available, see hello troubleshooting tips at hello end of this article.
>
>
-->

## <a name="log-on-toohello-virtual-machine"></a><span data-ttu-id="78f26-118">Přihlaste se toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="78f26-118">Log on toohello virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="78f26-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78f26-119">Next steps</span></span>
* <span data-ttu-id="78f26-120">Pokud hello **Connect** tlačítko je neaktivní nebo máte další problémy s hello připojení vzdálené plochy, opakujte pokus o obnovení konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="78f26-120">If hello **Connect** button is inactive or you are having other problems with hello Remote Desktop connection, try resetting hello configuration.</span></span> <span data-ttu-id="78f26-121">Klikněte na tlačítko **obnovte vzdálený přístup** z řídicího panelu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="78f26-121">click **Reset remote access** from hello virtual machine dashboard.</span></span>

    ![Resetování vzdálený přístup](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* <span data-ttu-id="78f26-123">Řešení problémů s heslo zkuste resetovat ho.</span><span class="sxs-lookup"><span data-stu-id="78f26-123">For problems with your password, try resetting it.</span></span> <span data-ttu-id="78f26-124">Klikněte na tlačítko **resetovat heslo** podél hello levé hrany řídicího panelu virtuální počítač, v části **podporu + Poradce při potížích s**.</span><span class="sxs-lookup"><span data-stu-id="78f26-124">Click **Reset password** along hello left edge of virtual machine dashboard, under **Support + Troubleshooting**.</span></span>

    ![Resetování hesla](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

<span data-ttu-id="78f26-126">Pokud tyto tipy nefungují nebo nejsou, co potřebujete, najdete v části [tooa připojení řešení Vzdálená plocha systému Windows virtuálního počítače Azure](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="78f26-126">If those tips don't work or aren't what you need, see [Troubleshoot Remote Desktop connections tooa Windows-based Azure Virtual Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="78f26-127">Tento článek vás provede diagnostikou a řešením běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="78f26-127">This article walks you through diagnosing and resolving common problems.</span></span>
