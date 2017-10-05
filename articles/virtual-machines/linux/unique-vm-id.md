---
title: "Získání ID virtuálního počítače Azure s Linuxem | Microsoft Docs"
description: "Popisuje, jak získat a použít Azure Linux virtuálních počítačů jedinečný identifikátor."
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
ms.openlocfilehash: 258ce425d5692730011cf2f4468dc0ba77f4cb79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a><span data-ttu-id="2091d-103">Přístupu a použití jedinečné ID virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="2091d-103">Accessing and Using Azure VM Unique ID</span></span>
<span data-ttu-id="2091d-104">Jedinečné ID virtuální počítač Azure je identifikátor 128 mi kódování a uložené v SMBIOS všechny Azure IaaS Virtuálního počítače a lze je aktuálně číst příkazy platformy systému BIOS.</span><span class="sxs-lookup"><span data-stu-id="2091d-104">Azure VM unique ID is a 128bits identifier encoded and stored in all Azure IaaS VM’s SMBIOS and can currently be read using platform BIOS commands.</span></span>

<span data-ttu-id="2091d-105">Jedinečné ID virtuální počítač Azure je vlastnost jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="2091d-105">Azure VM unique ID is a Read-only property.</span></span> <span data-ttu-id="2091d-106">Při vypnutí restartování počítače nedojde ke změně Azure jedinečné ID virtuálního počítače (buď plánované pro neplánované), spuštění a zastavení zrušit přidělení, služby, opravy nebo obnovení na místě.</span><span class="sxs-lookup"><span data-stu-id="2091d-106">Azure Unique VM ID won’t change upon reboot shutdown (either planned for unplanned), Start/Stop de-allocate, service healing or restore in place.</span></span> <span data-ttu-id="2091d-107">Pokud virtuální počítač je snímek a zkopírují se na vytvoření nové instance, je nakonfigurován nové ID virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="2091d-107">However, if the VM is a snapshot and copied to create a new instance, new Azure VM ID is configured.</span></span>

> [!NOTE]
> <span data-ttu-id="2091d-108">Pokud máte starší virtuálních počítačů vytvořena a spuštěna, protože tato nová funkce získali nasazuje (18. září 2014), prosím restartování virtuálního počítače se automaticky získat Azure Jedinečný ID.</span><span class="sxs-lookup"><span data-stu-id="2091d-108">If you have older VMs created and running since this new feature got rolled out (September 18, 2014), please restart your VM to automatically get an Azure unique ID.</span></span>
> 
> 

<span data-ttu-id="2091d-109">Pro přístup k Azure jedinečné ID virtuálního počítače z virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="2091d-109">To access Azure Unique VM ID from within the VM:</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="2091d-110">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2091d-110">Create a VM</span></span>
<span data-ttu-id="2091d-111">Další informace najdete v tématu [vytvoření virtuálního počítače](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2091d-111">For more information, see [Create a Virtual Machine](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="2091d-112">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="2091d-112">Connect to the VM</span></span>
<span data-ttu-id="2091d-113">Další informace najdete v tématu [SSH z Linuxu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2091d-113">For more information, see [SSH from Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="query-vm-unique-id"></a><span data-ttu-id="2091d-114">Jedinečné ID dotazu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2091d-114">Query VM Unique ID</span></span>
<span data-ttu-id="2091d-115">Příkaz (příklad používá **Ubuntu**):</span><span class="sxs-lookup"><span data-stu-id="2091d-115">Command (example uses **Ubuntu**):</span></span>

```bash
sudo dmidecode | grep UUID
```

<span data-ttu-id="2091d-116">Příklad očekávané výsledky:</span><span class="sxs-lookup"><span data-stu-id="2091d-116">Example Expected Results:</span></span>

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

<span data-ttu-id="2091d-117">Z důvodu Big Endian bit řazení skutečný jedinečné ID virtuálního počítače v tomto případě bude:</span><span class="sxs-lookup"><span data-stu-id="2091d-117">Due to Big Endian bit ordering, the actual Unique VM ID in this case will be:</span></span>

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

<span data-ttu-id="2091d-118">Azure jedinečné ID virtuálního počítače můžete použít v různých scénářích, zda virtuální počítač běží v Azure nebo místní a může pomoct vaší licencování, vytváření sestav a obecné požadavky sledování, které může být v nasazeních Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="2091d-118">Azure VM unique ID can be used in different scenarios whether the VM is running on Azure or on-premises and can help your licensing, reporting or general tracking requirements you may have on your Azure IaaS deployments.</span></span> <span data-ttu-id="2091d-119">Může vyžadovat mnoho nezávislí dodavatelé softwaru vytváření aplikací a jejich certifikuje v Azure k identifikaci virtuálního počítače Azure v průběhu životního cyklu a říct, pokud je virtuální počítač spuštěný v Azure, místně nebo na jiných poskytovatelů cloudu.</span><span class="sxs-lookup"><span data-stu-id="2091d-119">Many independent software vendors building applications and certifying them on Azure may require to identify an Azure VM throughout its lifecycle and to tell if the VM is running on Azure, on-Premises or on other cloud providers.</span></span> <span data-ttu-id="2091d-120">Tento identifikátor platformy může například pomoci zjistit, pokud je řádně licencované softwaru a v nápovědě ke korelaci žádná data virtuálních počítačů, jako svůj zdroj pomůže na nastavení správné metriky pro správné platformu a ke sledování a korelovat tyto metriky mimo jiné používá.</span><span class="sxs-lookup"><span data-stu-id="2091d-120">This platform identifier can for example help detect if the software is properly licensed or help to correlate any VM data to its source such as to assist on setting the right metrics for the right platform and to track and correlate these metrics amongst other uses.</span></span>

