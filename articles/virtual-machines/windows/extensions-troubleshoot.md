---
title: "chyby rozšíření virtuálního počítače s Windows aaaTroubleshooting | Microsoft Docs"
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
ms.openlocfilehash: d24544743d9e0cb1a68ec9ab7718716f918054f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a><span data-ttu-id="6067a-103">Řešení potíží se přes rozšíření virtuálního počítače Windows Azure</span><span class="sxs-lookup"><span data-stu-id="6067a-103">Troubleshooting Azure Windows VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="6067a-104">Zobrazení stavu rozšíření</span><span class="sxs-lookup"><span data-stu-id="6067a-104">Viewing extension status</span></span>
<span data-ttu-id="6067a-105">Šablony Azure Resource Manager lze spustit z prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6067a-105">Azure Resource Manager templates can be executed from Azure PowerShell.</span></span> <span data-ttu-id="6067a-106">Jakmile se spustí hello šablony, lze zobrazit stav rozšíření hello z Průzkumníka prostředků Azure nebo hello nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6067a-106">Once hello template is executed, hello extension status can be viewed from Azure Resource Explorer or hello command line tools.</span></span>

<span data-ttu-id="6067a-107">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="6067a-107">Here is an example:</span></span>

<span data-ttu-id="6067a-108">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6067a-108">Azure PowerShell:</span></span>

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

<span data-ttu-id="6067a-109">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6067a-109">Here is hello sample output:</span></span>

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
  <span data-ttu-id="6067a-110">]</span><span class="sxs-lookup"><span data-stu-id="6067a-110">]</span></span>

## <a name="troubleshooting-extension-failures"></a><span data-ttu-id="6067a-111">Řešení potíží s chyby rozšíření</span><span class="sxs-lookup"><span data-stu-id="6067a-111">Troubleshooting extension failures</span></span>
### <a name="re-running-hello-extension-on-hello-vm"></a><span data-ttu-id="6067a-112">Opětovné spuštění rozšíření hello na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6067a-112">Re-running hello extension on hello VM</span></span>
<span data-ttu-id="6067a-113">Pokud používáte skripty na hello virtuálního počítače pomocí rozšíření vlastních skriptů, může někdy spustit došlo k chybě, kde virtuálního počítače byla úspěšně vytvořena, ale došlo k selhání skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="6067a-113">If you are running scripts on hello VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but hello script has failed.</span></span> <span data-ttu-id="6067a-114">Za těchto podmínek hello doporučená toorecover způsob, jak z této chyby je tooremove hello rozšíření a znovu spustit šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="6067a-114">Under these conditons, hello recommended way toorecover from this error is tooremove hello extension and rerun hello template again.</span></span>
<span data-ttu-id="6067a-115">Poznámka: V budoucnosti, tato funkce bude rozšířené tooremove hello potřebu odinstalace hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6067a-115">Note: In future, this functionality would be enhanced tooremove hello need for uninstalling hello extension.</span></span>

#### <a name="remove-hello-extension-from-azure-powershell"></a><span data-ttu-id="6067a-116">Odebrat rozšíření hello z prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6067a-116">Remove hello extension from Azure PowerShell</span></span>
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

<span data-ttu-id="6067a-117">Po odebrání rozšíření hello hello šablonou může být znovu spustit toorun hello skripty na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6067a-117">Once hello extension has been removed, hello template can be re-executed toorun hello scripts on hello VM.</span></span>

