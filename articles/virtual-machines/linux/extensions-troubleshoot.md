---
title: "virtuální počítač s Linuxem aaaTroubleshooting rozšíření selhání | Microsoft Docs"
description: "Další informace o řešení potíží se přes rozšíření virtuálního počítače Azure s Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: f05d93f3-42fc-4a09-9798-d92f7929c417
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 29a0ca34207421e0014380000a313d3c44e7e594
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a><span data-ttu-id="c88d2-103">Řešení potíží se přes rozšíření virtuálního počítače Azure s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c88d2-103">Troubleshooting Azure Linux VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="c88d2-104">Zobrazení stavu rozšíření</span><span class="sxs-lookup"><span data-stu-id="c88d2-104">Viewing extension status</span></span>
<span data-ttu-id="c88d2-105">Šablony Azure Resource Manager lze spustit z příkazového řádku Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="c88d2-105">Azure Resource Manager templates can be executed from hello  Azure CLI.</span></span> <span data-ttu-id="c88d2-106">Jakmile se spustí hello šablony, lze zobrazit stav rozšíření hello z Průzkumníka prostředků Azure nebo hello nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c88d2-106">Once hello template is executed, hello extension status can be viewed from Azure Resource Explorer or hello command line tools.</span></span>

<span data-ttu-id="c88d2-107">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="c88d2-107">Here is an example:</span></span>

<span data-ttu-id="c88d2-108">Rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="c88d2-108">Azure CLI:</span></span>

      azure vm get-instance-view


<span data-ttu-id="c88d2-109">Tady je ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="c88d2-109">Here is hello sample output:</span></span>

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
  <span data-ttu-id="c88d2-110">]</span><span class="sxs-lookup"><span data-stu-id="c88d2-110">]</span></span>

## <a name="troubleshooting-extenson-failures"></a><span data-ttu-id="c88d2-111">Řešení potíží s Extenson selhání:</span><span class="sxs-lookup"><span data-stu-id="c88d2-111">Troubleshooting Extenson failures:</span></span>
### <a name="re-running-hello-extension-on-hello-vm"></a><span data-ttu-id="c88d2-112">Opětovné spuštění rozšíření hello na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c88d2-112">Re-running hello extension on hello VM</span></span>
<span data-ttu-id="c88d2-113">Pokud používáte skripty na hello virtuálního počítače pomocí rozšíření vlastních skriptů, může někdy spustit došlo k chybě, kde virtuálního počítače byla úspěšně vytvořena, ale došlo k selhání skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="c88d2-113">If you are running scripts on hello VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but hello script has failed.</span></span> <span data-ttu-id="c88d2-114">Za těchto podmínek hello doporučená toorecover způsob, jak z této chyby je tooremove hello rozšíření a znovu spustit šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="c88d2-114">Under these conditons, hello recommended way toorecover from this error is tooremove hello extension and rerun hello template again.</span></span>
<span data-ttu-id="c88d2-115">Poznámka: V budoucnosti, tato funkce bude rozšířené tooremove hello potřebu odinstalace hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c88d2-115">Note: In future, this functionality would be enhanced tooremove hello need for uninstalling hello extension.</span></span>

#### <a name="remove-hello-extension-from-azure-cli"></a><span data-ttu-id="c88d2-116">Odebrat rozšíření hello z příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c88d2-116">Remove hello extension from Azure CLI</span></span>
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

<span data-ttu-id="c88d2-117">Kde "publsher-name" odpovídá typu rozšíření toohello z výstupu hello "virtuální počítač azure get-instance zobrazení" a název je název hello hello rozšíření prostředků z šablony hello</span><span class="sxs-lookup"><span data-stu-id="c88d2-117">Where "publsher-name" corresponds toohello extension type from hello output of "azure vm get-instance-view" and name is hello name of hello extension resource from hello template</span></span>

<span data-ttu-id="c88d2-118">Po odebrání rozšíření hello hello šablonou může být znovu spustit toorun hello skripty na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c88d2-118">Once hello extension has been removed, hello template can be re-executed toorun hello scripts on hello VM.</span></span>

