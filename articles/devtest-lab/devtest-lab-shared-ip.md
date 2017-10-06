---
title: "aaaUnderstand sdílet IP adresy v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak Azure DevTest Labs využívá sdílený IP adresy toominimize hello veřejné IP adresy požadované tooaccess testovacího prostředí virtuálních počítačů."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 8756410117a9d550d567d372174bf1ea96703c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a><span data-ttu-id="633a3-103">Pochopení sdílené IP adresy v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="633a3-103">Understand shared IP addresses in Azure DevTest Labs</span></span>

<span data-ttu-id="633a3-104">Umožňuje prostředí Azure DevTest Labs sdílenou složku virtuální počítače hello stejný veřejnou IP adresu toominimize hello počet veřejných IP adres vyžaduje tooaccess testovacího prostředí jednotlivé virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="633a3-104">Azure DevTest Labs lets lab VMs share hello same public IP address toominimize hello number of public IP addresses required tooaccess your individual lab VMs.</span></span>  <span data-ttu-id="633a3-105">Tento článek popisuje, jak sdílené pracovní IP adresy a jejich související možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="633a3-105">This article describes how shared IPs work and their related configuration options.</span></span>

## <a name="shared-ip-setting"></a><span data-ttu-id="633a3-106">Sdílené nastavení protokolu IP</span><span class="sxs-lookup"><span data-stu-id="633a3-106">Shared IP setting</span></span>

<span data-ttu-id="633a3-107">Při vytváření testovacího prostředí se nachází v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="633a3-107">When you create a lab, it resides in a subnet of a virtual network.</span></span>  <span data-ttu-id="633a3-108">Ve výchozím nastavení, tato podsíť je vytvořena s **povolení sdíleného veřejnou IP adresu** nastavit příliš*Ano*.</span><span class="sxs-lookup"><span data-stu-id="633a3-108">By default, this subnet is created with **Enable shared public IP** set too*Yes*.</span></span>  <span data-ttu-id="633a3-109">Tato konfigurace vytvoří jednu veřejnou IP adresu pro celou podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="633a3-109">This configuration creates one public IP address for hello entire subnet.</span></span>  <span data-ttu-id="633a3-110">Další informace o konfiguraci virtuální sítě a podsítě, najdete v části [konfigurace virtuální sítě v Azure DevTest Labs](devtest-lab-configure-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="633a3-110">For more information about configuring virtual networks and subnets, see [Configure a virtual network in Azure DevTest Labs](devtest-lab-configure-vnet.md).</span></span>

![Novou podsíť testovacího prostředí](media/devtest-lab-shared-ip/lab-subnet.png)

<span data-ttu-id="633a3-112">Pro existující labs, můžete tuto možnost můžete povolit výběrem **konfiguraci a zásady > virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="633a3-112">For existing labs, you can enable this option by selecting **Configuration and policies > Virtual Networks**.</span></span> <span data-ttu-id="633a3-113">Potom vyberte virtuální síť ze seznamu hello a zvolte **povolit SDÍLENÉ VEŘEJNOU IP adresu** pro vybranou podsíť.</span><span class="sxs-lookup"><span data-stu-id="633a3-113">Then, select a virtual network from hello list and choose **ENABLE SHARED PUBLIC IP** for a selected subnet.</span></span> <span data-ttu-id="633a3-114">Tato možnost v jakékoli prostředí lze zakázat i pokud nechcete, aby tooshare veřejnou IP adresu v prostředí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="633a3-114">You can also disable this option in any lab if you don't want tooshare a public IP address across lab VMs.</span></span>

<span data-ttu-id="633a3-115">Všechny virtuální počítače vytvořené v této laboratoře výchozí toousing sdílené IP.</span><span class="sxs-lookup"><span data-stu-id="633a3-115">Any VMs created in this lab default toousing a shared IP.</span></span>  <span data-ttu-id="633a3-116">Při vytváření hello virtuálních počítačů, toto nastavení může být dodržen v hello **upřesňující nastavení** okno pod **konfiguraci IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="633a3-116">When creating hello VM, this setting can be observed in hello **Advanced settings** blade under **IP address configuration**.</span></span>

![Nový virtuální počítač](media/devtest-lab-shared-ip/new-vm.png)

- <span data-ttu-id="633a3-118">**Sdílené:** všechny virtuální počítače vytvořené jako **sdílené** se umístí do jedné skupiny prostředků (RG).</span><span class="sxs-lookup"><span data-stu-id="633a3-118">**Shared:** All VMs created as **Shared** are placed into one resource group (RG).</span></span> <span data-ttu-id="633a3-119">Jedna IP adresa je přiřazen, pro který RG a všechny virtuální počítače v hello RG použije tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="633a3-119">A single IP address is assigned for that RG and all VMs in hello RG will use that IP address.</span></span>
- <span data-ttu-id="633a3-120">**Veřejná:** každý virtuální počítač vytvoříte má svou vlastní IP adresu a je vytvořen v jeho vlastní skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="633a3-120">**Public:** Every VM you create has its own IP address and is created in its own resource group.</span></span>
- <span data-ttu-id="633a3-121">**Privátní:** každý virtuální počítač vytvoříte používá privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="633a3-121">**Private:** Every VM you create uses a private IP address.</span></span> <span data-ttu-id="633a3-122">Nebudete moct tooconnect toothis virtuálních počítačů přímo z hello Internetu pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="633a3-122">You will not be able tooconnect toothis VM directly from hello internet with Remote Desktop.</span></span>

<span data-ttu-id="633a3-123">Při každém přidání toohello podsíť virtuálního počítače pomocí sdíleného IP protokol povolen DevTest Labs automaticky přidá Vyrovnávání zatížení tooa hello virtuálních počítačů a přiřadí číslo portu TCP na hello veřejnou IP adresu předávání toohello portu RDP na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="633a3-123">Whenever a VM with shared IP enabled is added toohello subnet, DevTest Labs automatically adds hello VM tooa load balancer and assigns a TCP port number on hello public IP address, forwarding toohello RDP port on hello VM.</span></span>  

## <a name="using-hello-shared-ip"></a><span data-ttu-id="633a3-124">Používání hello sdíleného IP</span><span class="sxs-lookup"><span data-stu-id="633a3-124">Using hello shared IP</span></span>

- <span data-ttu-id="633a3-125">**Uživatelé Linux:** SSH toohello virtuálních počítačů pomocí hello IP adresu nebo plně kvalifikovaný název domény, následovaným dvojtečkou, za nímž následuje hello portu.</span><span class="sxs-lookup"><span data-stu-id="633a3-125">**Linux users:** SSH toohello VM by using hello IP address or fully qualified domain name, followed by a colon, followed by hello port.</span></span> <span data-ttu-id="633a3-126">Například v bitové kopii hello níže, hello RDP adres tooconnect toohello virtuálního počítače je `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span><span class="sxs-lookup"><span data-stu-id="633a3-126">For example, in hello image below, hello RDP address tooconnect toohello VM is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span></span>

  ![Příklad virtuálních počítačů](media/devtest-lab-shared-ip/vm-info.png)

- <span data-ttu-id="633a3-128">**Uživatelé systému Windows:** vyberte hello **Connect** tlačítko hello Azure portálu toodownload předem nakonfigurovaný soubor RDP a přístup k hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="633a3-128">**Windows users:** Select hello **Connect** button on hello Azure portal toodownload a pre-configured RDP file and access hello VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="633a3-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="633a3-129">Next steps</span></span>

* [<span data-ttu-id="633a3-130">Definovat zásady testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="633a3-130">Define lab policies in Azure DevTest Labs</span></span>](devtest-lab-set-lab-policy.md)
* [<span data-ttu-id="633a3-131">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="633a3-131">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md)





