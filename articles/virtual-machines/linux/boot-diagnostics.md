---
title: "aaaBoot diagnostiky pro virtuální počítače s Linuxem v Azure | Microsoft dokumentů"
description: "Přehled hello dvě funkce ladění pro virtuální počítače s Linuxem v Azure"
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
ms.openlocfilehash: d355d512de09d2f1d7a2718e3db3fb99c9dd9e24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-boot-diagnostics-tootroubleshoot-linux-virtual-machines-in-azure"></a><span data-ttu-id="a1459-103">Jak toouse spouštění diagnostiky tootroubleshoot Linuxové virtuální počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="a1459-103">How toouse boot diagnostics tootroubleshoot Linux virtual machines in Azure</span></span>

<span data-ttu-id="a1459-104">V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a1459-104">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="a1459-105">Při zpětném přepnutí vlastní image tooAzure nebo i spouštění jeden z Image platformy hello, může být mnoha důvodů, proč se virtuální počítač získá do jiných spouštěcího stavu.</span><span class="sxs-lookup"><span data-stu-id="a1459-105">When bringing your own image tooAzure or even booting one of hello platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="a1459-106">Tyto funkce povolit tooeasily jste diagnostiku a obnovení virtuálních počítačů v případě selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="a1459-106">These features enable you tooeasily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="a1459-107">Pro virtuální počítače s Linuxem, můžete snadno zobrazit výstup hello protokolu konzoly z hello portálu:</span><span class="sxs-lookup"><span data-stu-id="a1459-107">For Linux Virtual Machines, you can easily view hello output of your console log from hello Portal:</span></span>

![portál Azure](./media/boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="a1459-109">Pro systém Windows a virtuální počítače s Linuxem, je však Azure také umožňuje toosee snímek hello virtuálních počítačů ze hypervisoru hello:</span><span class="sxs-lookup"><span data-stu-id="a1459-109">However, for both Windows and Linux Virtual Machines, Azure also enables you toosee a screenshot of hello VM from hello hypervisor:</span></span>

![Chyba](./media/boot-diagnostics/screenshot2.png)

<span data-ttu-id="a1459-111">Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="a1459-111">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="a1459-112">Všimněte si, snímky a výstup může trvat až minut tooappear too10 ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a1459-112">Note, screenshots, and output can take up too10 minutes tooappear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="a1459-113">Běžné chyby spuštění</span><span class="sxs-lookup"><span data-stu-id="a1459-113">Common boot errors</span></span>

- [<span data-ttu-id="a1459-114">Problémy systému souborů</span><span class="sxs-lookup"><span data-stu-id="a1459-114">File system issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [<span data-ttu-id="a1459-115">Problémy jádra</span><span class="sxs-lookup"><span data-stu-id="a1459-115">Kernel Issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [<span data-ttu-id="a1459-116">FSTAB chyby</span><span class="sxs-lookup"><span data-stu-id="a1459-116">FSTAB errors</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="a1459-117">Povolení diagnostiky na novém virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="a1459-117">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="a1459-118">Při vytváření nového virtuálního počítače z hello portál Preview, vyberte hello **Azure Resource Manager** z rozevíracího seznamu model nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="a1459-118">When creating a new Virtual Machine from hello Preview Portal, select hello **Azure Resource Manager** from hello deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="a1459-120">Nakonfigurujte hello monitorování možnost tooselect hello účet úložiště kam chcete tooplace tyto diagnostické soubory.</span><span class="sxs-lookup"><span data-stu-id="a1459-120">Configure hello Monitoring option tooselect hello storage account where you would like tooplace these diagnostic files.</span></span>
 
    ![Vytvoření virtuálního počítače](./media/boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="a1459-122">Pokud nasazujete z šablony Azure Resource Manager, přejděte tooyour prostředek virtuálního počítače a připojte oddíl profilu hello diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="a1459-122">If you are deploying from an Azure Resource Manager template, navigate tooyour Virtual Machine resource and append hello diagnostics profile section.</span></span> <span data-ttu-id="a1459-123">Mějte na paměti, hlavičkou verze rozhraní API "2015-06-15" hello toouse.</span><span class="sxs-lookup"><span data-stu-id="a1459-123">Remember toouse hello “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="a1459-124">Hello diagnostického profilu můžete účet úložiště hello tooselect místo tooput tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="a1459-124">hello diagnostics profile enables you tooselect hello storage account where you want tooput these logs.</span></span>

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

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="a1459-125">Aktualizace stávajícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a1459-125">Update an existing virtual machine</span></span>

<span data-ttu-id="a1459-126">Diagnostika spouštění tooenable prostřednictvím hello portálu, můžete také aktualizovat existující virtuální počítač prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="a1459-126">tooenable boot diagnostics through hello portal, you can also update an existing virtual machine through hello portal.</span></span> <span data-ttu-id="a1459-127">Diagnostika spouštění vyberte hello možnost a uložte.</span><span class="sxs-lookup"><span data-stu-id="a1459-127">Select hello Boot Diagnostics option and Save.</span></span> <span data-ttu-id="a1459-128">Restartujte vliv tootake hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a1459-128">Restart hello VM tootake effect.</span></span>

![Aktualizace stávajícího virtuálního počítače](./media/boot-diagnostics/screenshot5.png)