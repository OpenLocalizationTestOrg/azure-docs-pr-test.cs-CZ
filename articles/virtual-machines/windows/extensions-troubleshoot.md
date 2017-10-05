---
title: "Řešení potíží se přes rozšíření virtuálního počítače s Windows | Microsoft Docs"
description: "Další informace o řešení potíží se přes rozšíření virtuálního počítače Windows Azure"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 4dba196e1b838f2092b30972fb070514bd2a4b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a><span data-ttu-id="db8cb-103">Řešení potíží se přes rozšíření virtuálního počítače Windows Azure</span><span class="sxs-lookup"><span data-stu-id="db8cb-103">Troubleshooting Azure Windows VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="db8cb-104">Zobrazení stavu rozšíření</span><span class="sxs-lookup"><span data-stu-id="db8cb-104">Viewing extension status</span></span>
<span data-ttu-id="db8cb-105">Šablony Azure Resource Manager lze spustit z prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db8cb-105">Azure Resource Manager templates can be executed from Azure PowerShell.</span></span> <span data-ttu-id="db8cb-106">Jakmile se spustí šablony, lze zobrazit stav rozšíření z Průzkumníka prostředků Azure nebo nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="db8cb-106">Once the template is executed, the extension status can be viewed from Azure Resource Explorer or the command line tools.</span></span>

<span data-ttu-id="db8cb-107">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="db8cb-107">Here is an example:</span></span>

<span data-ttu-id="db8cb-108">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="db8cb-108">Azure PowerShell:</span></span>

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

<span data-ttu-id="db8cb-109">Zde je ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="db8cb-109">Here is the sample output:</span></span>

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  <span data-ttu-id="db8cb-110">]</span><span class="sxs-lookup"><span data-stu-id="db8cb-110">]</span></span>

## <a name="troubleshooting-extension-failures"></a><span data-ttu-id="db8cb-111">Řešení potíží s chyby rozšíření</span><span class="sxs-lookup"><span data-stu-id="db8cb-111">Troubleshooting extension failures</span></span>
### <a name="re-running-the-extension-on-the-vm"></a><span data-ttu-id="db8cb-112">Opětovné spuštění rozšíření ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="db8cb-112">Re-running the extension on the VM</span></span>
<span data-ttu-id="db8cb-113">Pokud se spouštění skriptů na virtuálním počítači pomocí rozšíření vlastních skriptů, může někdy spustit došlo k chybě, kde byl virtuální počítač úspěšně vytvořen, ale došlo k selhání skriptu.</span><span class="sxs-lookup"><span data-stu-id="db8cb-113">If you are running scripts on the VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but the script has failed.</span></span> <span data-ttu-id="db8cb-114">Doporučený způsob obnovení z této chyby je v rámci těchto podmínek, odeberte rozšíření a znovu spustit šablonu.</span><span class="sxs-lookup"><span data-stu-id="db8cb-114">Under these conditons, the recommended way to recover from this error is to remove the extension and rerun the template again.</span></span>
<span data-ttu-id="db8cb-115">Poznámka: V budoucnosti, tato funkce by vylepšit odebrat potřeba odinstalovat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="db8cb-115">Note: In future, this functionality would be enhanced to remove the need for uninstalling the extension.</span></span>

#### <a name="remove-the-extension-from-azure-powershell"></a><span data-ttu-id="db8cb-116">Odeberte rozšíření z prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="db8cb-116">Remove the extension from Azure PowerShell</span></span>
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

<span data-ttu-id="db8cb-117">Po odebrání rozšíření šablony lze spouštět skripty ve virtuálním počítači znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="db8cb-117">Once the extension has been removed, the template can be re-executed to run the scripts on the VM.</span></span>

