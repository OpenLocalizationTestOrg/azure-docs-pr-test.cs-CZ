---
title: "Pochopení sdílené IP adresy v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak Azure DevTest Labs využívá sdílené IP adresy, chcete-li minimalizovat veřejné IP adresy, které jsou nutné pro přístup k testovacího prostředí virtuálních počítačů."
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
ms.openlocfilehash: 9f6e1980bf5ea5b41da98a135d89f1c5159921a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a><span data-ttu-id="7f1e0-103">Pochopení sdílené IP adresy v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7f1e0-103">Understand shared IP addresses in Azure DevTest Labs</span></span>

<span data-ttu-id="7f1e0-104">Azure DevTest Labs umožňuje testovacího prostředí virtuálních počítačů sdílet stejnou veřejnou IP adresu minimalizovat počet veřejných IP adres, které jsou nutné pro přístup k testovacího prostředí jednotlivé virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-104">Azure DevTest Labs lets lab VMs share the same public IP address to minimize the number of public IP addresses required to access your individual lab VMs.</span></span>  <span data-ttu-id="7f1e0-105">Tento článek popisuje, jak sdílené pracovní IP adresy a jejich související možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-105">This article describes how shared IPs work and their related configuration options.</span></span>

## <a name="shared-ip-setting"></a><span data-ttu-id="7f1e0-106">Sdílené nastavení protokolu IP</span><span class="sxs-lookup"><span data-stu-id="7f1e0-106">Shared IP setting</span></span>

<span data-ttu-id="7f1e0-107">Při vytváření testovacího prostředí se nachází v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-107">When you create a lab, it resides in a subnet of a virtual network.</span></span>  <span data-ttu-id="7f1e0-108">Ve výchozím nastavení, tato podsíť je vytvořena s **povolení sdíleného veřejnou IP adresu** nastavena na *Ano*.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-108">By default, this subnet is created with **Enable shared public IP** set to *Yes*.</span></span>  <span data-ttu-id="7f1e0-109">Tato konfigurace vytvoří jednu veřejnou IP adresu pro celou podsíť.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-109">This configuration creates one public IP address for the entire subnet.</span></span>  <span data-ttu-id="7f1e0-110">Další informace o konfiguraci virtuální sítě a podsítě, najdete v části [konfigurace virtuální sítě v Azure DevTest Labs](devtest-lab-configure-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="7f1e0-110">For more information about configuring virtual networks and subnets, see [Configure a virtual network in Azure DevTest Labs](devtest-lab-configure-vnet.md).</span></span>

![Novou podsíť testovacího prostředí](media/devtest-lab-shared-ip/lab-subnet.png)

<span data-ttu-id="7f1e0-112">Pro existující labs, můžete tuto možnost můžete povolit výběrem **konfiguraci a zásady > virtuální sítě**.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-112">For existing labs, you can enable this option by selecting **Configuration and policies > Virtual Networks**.</span></span> <span data-ttu-id="7f1e0-113">Potom vyberte virtuální síť ze seznamu a zvolte **povolit SDÍLENÉ VEŘEJNOU IP adresu** pro vybranou podsíť.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-113">Then, select a virtual network from the list and choose **ENABLE SHARED PUBLIC IP** for a selected subnet.</span></span> <span data-ttu-id="7f1e0-114">Tato možnost v jakékoli prostředí lze zakázat i pokud nechcete sdílet veřejnou IP adresu v rámci prostředí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-114">You can also disable this option in any lab if you don't want to share a public IP address across lab VMs.</span></span>

<span data-ttu-id="7f1e0-115">Všechny virtuální počítače vytvořené v toto výchozí nastavení testovacího prostředí pro použití sdílené IP.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-115">Any VMs created in this lab default to using a shared IP.</span></span>  <span data-ttu-id="7f1e0-116">Při vytváření virtuálního počítače, toto nastavení může být dodržen v **upřesňující nastavení** okno pod **konfiguraci IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-116">When creating the VM, this setting can be observed in the **Advanced settings** blade under **IP address configuration**.</span></span>

![Nový virtuální počítač](media/devtest-lab-shared-ip/new-vm.png)

- <span data-ttu-id="7f1e0-118">**Sdílené:** všechny virtuální počítače vytvořené jako **sdílené** se umístí do jedné skupiny prostředků (RG).</span><span class="sxs-lookup"><span data-stu-id="7f1e0-118">**Shared:** All VMs created as **Shared** are placed into one resource group (RG).</span></span> <span data-ttu-id="7f1e0-119">Jedna IP adresa je přiřazen, pro který RG a všechny virtuální počítače ve RG použije tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-119">A single IP address is assigned for that RG and all VMs in the RG will use that IP address.</span></span>
- <span data-ttu-id="7f1e0-120">**Veřejná:** každý virtuální počítač vytvoříte má svou vlastní IP adresu a je vytvořen v jeho vlastní skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-120">**Public:** Every VM you create has its own IP address and is created in its own resource group.</span></span>
- <span data-ttu-id="7f1e0-121">**Privátní:** každý virtuální počítač vytvoříte používá privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-121">**Private:** Every VM you create uses a private IP address.</span></span> <span data-ttu-id="7f1e0-122">Nebudete moci připojit k tomuto virtuálnímu počítači přímo z Internetu pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-122">You will not be able to connect to this VM directly from the internet with Remote Desktop.</span></span>

<span data-ttu-id="7f1e0-123">Vždy, když je virtuální počítač s sdílené IP protokol povolen přidány k podsíti, DevTest Labs automaticky přidá virtuální počítač ke službě Vyrovnávání zatížení a přiřadí číslo portu TCP na veřejnou IP adresu, předávání k portu RDP na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-123">Whenever a VM with shared IP enabled is added to the subnet, DevTest Labs automatically adds the VM to a load balancer and assigns a TCP port number on the public IP address, forwarding to the RDP port on the VM.</span></span>  

## <a name="using-the-shared-ip"></a><span data-ttu-id="7f1e0-124">Použití sdíleného IP</span><span class="sxs-lookup"><span data-stu-id="7f1e0-124">Using the shared IP</span></span>

- <span data-ttu-id="7f1e0-125">**Uživatelé Linux:** SSH k virtuálnímu počítači pomocí IP adresy nebo plně kvalifikovaný název domény, následovaným dvojtečkou, za nímž následuje port.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-125">**Linux users:** SSH to the VM by using the IP address or fully qualified domain name, followed by a colon, followed by the port.</span></span> <span data-ttu-id="7f1e0-126">Například následující obrázek je adresu protokolu RDP pro připojení k virtuálnímu počítači `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-126">For example, in the image below, the RDP address to connect to the VM is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span></span>

  ![Příklad virtuálních počítačů](media/devtest-lab-shared-ip/vm-info.png)

- <span data-ttu-id="7f1e0-128">**Uživatelé systému Windows:** vyberte **Connect** tlačítko na portálu Azure ke stažení předem nakonfigurovaný soubor RDP a přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="7f1e0-128">**Windows users:** Select the **Connect** button on the Azure portal to download a pre-configured RDP file and access the VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f1e0-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f1e0-129">Next steps</span></span>

* [<span data-ttu-id="7f1e0-130">Definovat zásady testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7f1e0-130">Define lab policies in Azure DevTest Labs</span></span>](devtest-lab-set-lab-policy.md)
* [<span data-ttu-id="7f1e0-131">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7f1e0-131">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md)





