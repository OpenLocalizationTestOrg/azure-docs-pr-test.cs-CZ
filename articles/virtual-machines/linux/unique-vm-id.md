---
title: "aaaGet ID virtuálních počítačů Linux Azure | Microsoft Docs"
description: "Popisuje, jak tooget a používání Azure Linux virtuálních počítačů Jedinečný ID."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/23/2017
ms.author: kmouss
ms.openlocfilehash: 4c8ddfc2e892824581e77649285ee8adbccd5def
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a><span data-ttu-id="532fb-103">Přístupu a použití jedinečné ID virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="532fb-103">Accessing and Using Azure VM Unique ID</span></span>
<span data-ttu-id="532fb-104">Jedinečné ID virtuální počítač Azure je identifikátor 128 mi kódování a uložené v SMBIOS všechny Azure IaaS Virtuálního počítače a lze je aktuálně číst příkazy platformy systému BIOS.</span><span class="sxs-lookup"><span data-stu-id="532fb-104">Azure VM unique ID is a 128bits identifier encoded and stored in all Azure IaaS VM’s SMBIOS and can currently be read using platform BIOS commands.</span></span>

<span data-ttu-id="532fb-105">Jedinečné ID virtuální počítač Azure je vlastnost jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="532fb-105">Azure VM unique ID is a Read-only property.</span></span> <span data-ttu-id="532fb-106">Při vypnutí restartování počítače nedojde ke změně Azure jedinečné ID virtuálního počítače (buď plánované pro neplánované), spuštění a zastavení zrušit přidělení, služby, opravy nebo obnovení na místě.</span><span class="sxs-lookup"><span data-stu-id="532fb-106">Azure Unique VM ID won’t change upon reboot shutdown (either planned for unplanned), Start/Stop de-allocate, service healing or restore in place.</span></span> <span data-ttu-id="532fb-107">Pokud hello virtuálního počítače je snímek a zkopírovaný toocreate novou instanci, je nakonfigurován nové ID virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="532fb-107">However, if hello VM is a snapshot and copied toocreate a new instance, new Azure VM ID is configured.</span></span>

> [!NOTE]
> <span data-ttu-id="532fb-108">Pokud máte starší virtuálních počítačů vytvořena a spuštěna, protože tato nová funkce získali nasazuje (18. září 2014), restartujte prosím váš počítač tooautomatically získat Azure jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="532fb-108">If you have older VMs created and running since this new feature got rolled out (September 18, 2014), please restart your VM tooautomatically get an Azure unique ID.</span></span>
> 
> 

<span data-ttu-id="532fb-109">Jedinečné ID virtuálního počítače Azure z tooaccess v rámci hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="532fb-109">tooaccess Azure Unique VM ID from within hello VM:</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="532fb-110">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="532fb-110">Create a VM</span></span>
<span data-ttu-id="532fb-111">Další informace najdete v tématu [vytvoření virtuálního počítače](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="532fb-111">For more information, see [Create a Virtual Machine](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="connect-toohello-vm"></a><span data-ttu-id="532fb-112">Připojit toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="532fb-112">Connect toohello VM</span></span>
<span data-ttu-id="532fb-113">Další informace najdete v tématu [SSH z Linuxu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="532fb-113">For more information, see [SSH from Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="query-vm-unique-id"></a><span data-ttu-id="532fb-114">Jedinečné ID dotazu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="532fb-114">Query VM Unique ID</span></span>
<span data-ttu-id="532fb-115">Příkaz (příklad používá **Ubuntu**):</span><span class="sxs-lookup"><span data-stu-id="532fb-115">Command (example uses **Ubuntu**):</span></span>

```bash
sudo dmidecode | grep UUID
```

<span data-ttu-id="532fb-116">Příklad očekávané výsledky:</span><span class="sxs-lookup"><span data-stu-id="532fb-116">Example Expected Results:</span></span>

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

<span data-ttu-id="532fb-117">Z důvodu tooBig Endian bit řazení hello skutečné jedinečné ID virtuálního počítače v tomto případě bude:</span><span class="sxs-lookup"><span data-stu-id="532fb-117">Due tooBig Endian bit ordering, hello actual Unique VM ID in this case will be:</span></span>

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

<span data-ttu-id="532fb-118">Azure jedinečné ID virtuálního počítače můžete použít v různých scénářích, zda hello virtuální počítač běží v Azure nebo místní a může pomoct vaší licencování, vytváření sestav a obecné požadavky sledování, které může být v nasazeních Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="532fb-118">Azure VM unique ID can be used in different scenarios whether hello VM is running on Azure or on-premises and can help your licensing, reporting or general tracking requirements you may have on your Azure IaaS deployments.</span></span> <span data-ttu-id="532fb-119">Mnoho nezávislí dodavatelé softwaru vytváření aplikací a jejich certifikuje v Azure může vyžadovat tooidentify virtuálního počítače Azure v rámci tootell a životního cyklu, pokud hello virtuální počítač běží na Azure, místně nebo na jiných poskytovatelů cloudu.</span><span class="sxs-lookup"><span data-stu-id="532fb-119">Many independent software vendors building applications and certifying them on Azure may require tooidentify an Azure VM throughout its lifecycle and tootell if hello VM is running on Azure, on-Premises or on other cloud providers.</span></span> <span data-ttu-id="532fb-120">Tento identifikátor platformy může pomoci například zjistit, pokud je řádně licencované hello softwaru nebo pomoci toocorrelate libovolný zdroj dat tooits virtuálního počítače jako je například tooassist na nastavení hello správné metrik pro hello správné platformy a tootrack a korelovat tyto metriky mezi jiné účely.</span><span class="sxs-lookup"><span data-stu-id="532fb-120">This platform identifier can for example help detect if hello software is properly licensed or help toocorrelate any VM data tooits source such as tooassist on setting hello right metrics for hello right platform and tootrack and correlate these metrics amongst other uses.</span></span>

