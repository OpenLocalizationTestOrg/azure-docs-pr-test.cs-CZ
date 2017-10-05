---
title: "Spuštění diagnostiky pro virtuální počítače s Linuxem v Azure | Microsoft dokumentů"
description: "Přehled ladění dvou funkcí pro virtuální počítače s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: 70254d39b5c6326166f7e29fdfc99533835502f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a><span data-ttu-id="cf44a-103">Jak používat Diagnostika spouštění k řešení potíží s virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="cf44a-103">How to use boot diagnostics to troubleshoot Linux virtual machines in Azure</span></span>

<span data-ttu-id="cf44a-104">V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cf44a-104">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="cf44a-105">Při přenosu vlastní image do Azure nebo spouštění některé z imagí platformy může virtuální počítač přejít do nespustitelného stavu z mnoha důvodů.</span><span class="sxs-lookup"><span data-stu-id="cf44a-105">When bringing your own image to Azure or even booting one of the platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="cf44a-106">Tyto funkce umožňují snadnou diagnostiku a obnovení virtuálních počítačů v případě chyb při spuštění.</span><span class="sxs-lookup"><span data-stu-id="cf44a-106">These features enable you to easily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="cf44a-107">U virtuálních počítačů s Linuxem můžete snadno zobrazit výstup protokolu konzoly z portálu:</span><span class="sxs-lookup"><span data-stu-id="cf44a-107">For Linux Virtual Machines, you can easily view the output of your console log from the Portal:</span></span>

![portál Azure](./media/boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="cf44a-109">U virtuálních počítačů s Windows i Linuxem však Azure umožňuje zobrazit také snímek obrazovky virtuálního počítače z hypervisoru:</span><span class="sxs-lookup"><span data-stu-id="cf44a-109">However, for both Windows and Linux Virtual Machines, Azure also enables you to see a screenshot of the VM from the hypervisor:</span></span>

![Chyba](./media/boot-diagnostics/screenshot2.png)

<span data-ttu-id="cf44a-111">Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="cf44a-111">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="cf44a-112">Mějte na paměti, že může trvat až 10 minut, než se snímky obrazovky a výstup zobrazí v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cf44a-112">Note, screenshots, and output can take up to 10 minutes to appear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="cf44a-113">Běžné chyby spuštění</span><span class="sxs-lookup"><span data-stu-id="cf44a-113">Common boot errors</span></span>

- [<span data-ttu-id="cf44a-114">Problémy systému souborů</span><span class="sxs-lookup"><span data-stu-id="cf44a-114">File system issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [<span data-ttu-id="cf44a-115">Problémy jádra</span><span class="sxs-lookup"><span data-stu-id="cf44a-115">Kernel Issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [<span data-ttu-id="cf44a-116">FSTAB chyby</span><span class="sxs-lookup"><span data-stu-id="cf44a-116">FSTAB errors</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="cf44a-117">Povolení diagnostiky na novém virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="cf44a-117">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="cf44a-118">Při vytváření nového virtuálního počítače na portálu Preview vyberte z rozevíracího seznamu modelu nasazení **Azure Resource Manager**:</span><span class="sxs-lookup"><span data-stu-id="cf44a-118">When creating a new Virtual Machine from the Preview Portal, select the **Azure Resource Manager** from the deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="cf44a-120">Nakonfigurujte možnost Monitorování pro výběr účtu úložiště, do kterého chcete tyto diagnostické soubory ukládat.</span><span class="sxs-lookup"><span data-stu-id="cf44a-120">Configure the Monitoring option to select the storage account where you would like to place these diagnostic files.</span></span>
 
    ![Vytvoření virtuálního počítače](./media/boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="cf44a-122">Pokud provádíte nasazení z šablony Azure Resource Manageru, přejděte do prostředku vašeho virtuálního počítače a připojte část s diagnostickým profilem.</span><span class="sxs-lookup"><span data-stu-id="cf44a-122">If you are deploying from an Azure Resource Manager template, navigate to your Virtual Machine resource and append the diagnostics profile section.</span></span> <span data-ttu-id="cf44a-123">Nezapomeňte použít hlavičku verze rozhraní API s hodnotou 2015-06-15.</span><span class="sxs-lookup"><span data-stu-id="cf44a-123">Remember to use the “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="cf44a-124">Diagnostický profil umožňuje vybrat účet úložiště pro ukládání protokolů.</span><span class="sxs-lookup"><span data-stu-id="cf44a-124">The diagnostics profile enables you to select the storage account where you want to put these logs.</span></span>

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="cf44a-125">Aktualizace stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cf44a-125">Update an existing virtual machine</span></span>

<span data-ttu-id="cf44a-126">Pokud chcete povolit Diagnostika spouštění prostřednictvím portálu, můžete také aktualizovat existujícího virtuálního počítače přes portál.</span><span class="sxs-lookup"><span data-stu-id="cf44a-126">To enable boot diagnostics through the portal, you can also update an existing virtual machine through the portal.</span></span> <span data-ttu-id="cf44a-127">Vyberte možnost Diagnostika spuštění a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="cf44a-127">Select the Boot Diagnostics option and Save.</span></span> <span data-ttu-id="cf44a-128">Restartujte virtuální počítač, aby se projevily změny.</span><span class="sxs-lookup"><span data-stu-id="cf44a-128">Restart the VM to take effect.</span></span>

![Aktualizace stávajícího virtuálního počítače](./media/boot-diagnostics/screenshot5.png)