---
title: "Postup řešení běžných problémů při vytváření virtuálního pevného disku | Microsoft Docs"
description: "Odpovědi na časté otázky řešení potíží a problémů při vytváření virtuálního pevného disku."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a><span data-ttu-id="a7d35-103">Postup řešení běžných problémů s došlo při vytváření virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="a7d35-103">How to troubleshoot common issues encountered during VHD creation</span></span>
<span data-ttu-id="a7d35-104">Tento článek je určena k Azure Marketplace vydavatele nebo spolusprávcem, který může zaznamenat problém nebo mít běžné otázky při publikování nebo správu jejich solution(s) virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7d35-104">This article is provided to help an Azure Marketplace publisher and/or co-administrator that may experience an issue or have common questions while publishing or managing their virtual machine solution(s).</span></span>

1. <span data-ttu-id="a7d35-105">Změna názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="a7d35-105">How do I change the name of the host?</span></span>
   
    <span data-ttu-id="a7d35-106">Po vytvoření virtuálního počítače uživatele nelze aktualizovat název hostitele.</span><span class="sxs-lookup"><span data-stu-id="a7d35-106">Once VM is created, users can’t update the name of the host.</span></span>
2. <span data-ttu-id="a7d35-107">Jak obnovit služby Vzdálená plocha nebo jeho heslo pro přihlášení?</span><span class="sxs-lookup"><span data-stu-id="a7d35-107">How to reset the Remote Desktop service or its login password?</span></span>
   
   * [<span data-ttu-id="a7d35-108">Referenční informace pro virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="a7d35-108">Reference for Windows VM</span></span>](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [<span data-ttu-id="a7d35-109">Referenční informace pro virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="a7d35-109">Reference for Linux VM</span></span>](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. <span data-ttu-id="a7d35-110">Jak vygenerovat nový ssh certifikáty?</span><span class="sxs-lookup"><span data-stu-id="a7d35-110">How to generate new ssh certificates?</span></span>
   
   <span data-ttu-id="a7d35-111">Podrobnosti najdete odkaz na: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)</span><span class="sxs-lookup"><span data-stu-id="a7d35-111">Please refer to the link: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)</span></span>
4. <span data-ttu-id="a7d35-112">Jak nakonfigurovat certifikát sítě VPN otevřené?</span><span class="sxs-lookup"><span data-stu-id="a7d35-112">How to configure an open VPN certificate?</span></span>
   
   <span data-ttu-id="a7d35-113">Podrobnosti najdete odkaz na: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)</span><span class="sxs-lookup"><span data-stu-id="a7d35-113">Please refer to the link: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)</span></span>
5. <span data-ttu-id="a7d35-114">Co je zásady podpory pro spuštění serverového softwaru společnosti Microsoft v prostředí virtuálního počítače Microsoft Azure (infrastruktura jako služba)?</span><span class="sxs-lookup"><span data-stu-id="a7d35-114">What is the support policy for running Microsoft server software in the Microsoft Azure virtual machine environment (infrastructure-as-a-service)?</span></span>
   
   <span data-ttu-id="a7d35-115">Podrobnosti najdete odkaz na: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span><span class="sxs-lookup"><span data-stu-id="a7d35-115">Please refer to the link: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span></span>
6. <span data-ttu-id="a7d35-116">Mají virtuální počítače žádné jedinečný identifikátor?</span><span class="sxs-lookup"><span data-stu-id="a7d35-116">Do Virtual Machines have any unique identifier?</span></span>
   
   <span data-ttu-id="a7d35-117">Azure kóduje jedinečné ID virtuálního počítače Azure v každé virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7d35-117">Azure encodes Azure VM Unique ID in every VM.</span></span> <span data-ttu-id="a7d35-118">Najdete v podrobnostech v tomto blogu a dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="a7d35-118">See details in this blog and documentation.</span></span>
7. <span data-ttu-id="a7d35-119">Do virtuálního počítače, jak lze spravovat rozšíření vlastních skriptů v úloze pro spuštění?</span><span class="sxs-lookup"><span data-stu-id="a7d35-119">In a VM how can I manage the custom script extension in the startup task?</span></span>
   
   <span data-ttu-id="a7d35-120">Podrobnosti najdete odkaz na: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)</span><span class="sxs-lookup"><span data-stu-id="a7d35-120">Please refer to the link: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)</span></span>
8. <span data-ttu-id="a7d35-121">Postup vytvoření virtuálního počítače z portálu Azure pomocí virtuálního pevného disku, který je nahrán do úložiště úrovně premium?</span><span class="sxs-lookup"><span data-stu-id="a7d35-121">How to create a VM from the Azure portal using the VHD that is uploaded to premium storage?</span></span>
   
   <span data-ttu-id="a7d35-122">Tato funkce ještě nepodporujeme.</span><span class="sxs-lookup"><span data-stu-id="a7d35-122">We do not support this feature yet.</span></span>
9. <span data-ttu-id="a7d35-123">Je podporováno 32bitovou aplikaci v Azure Marketplace?</span><span class="sxs-lookup"><span data-stu-id="a7d35-123">Is a 32-bit app supported in the Azure Marketplace?</span></span>
   
   <span data-ttu-id="a7d35-124">Naleznete odkaz podrobnosti o zásadách podpory: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span><span class="sxs-lookup"><span data-stu-id="a7d35-124">Please refer to the link for details on the support policy: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span></span>
10. <span data-ttu-id="a7d35-125">Pokaždé, když chcete vytvořit bitovou kopii z mé virtuálních pevných disků, zobrazí chyba ". Virtuální pevný disk je už registrovaný s úložištěm image jako prostředek "v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a7d35-125">Every time I am trying to create an image from my VHDs, I get the error “.VHD is already registered with image repository as the resource” in PowerShell.</span></span> <span data-ttu-id="a7d35-126">Nebyl vytvořen žádný obrázek před ani nebyl nalezen žádný obrázek s tímto názvem v Azure.</span><span class="sxs-lookup"><span data-stu-id="a7d35-126">I did not create any image before nor did I find any image with this name in Azure.</span></span> <span data-ttu-id="a7d35-127">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="a7d35-127">How do I resolve this?</span></span>
    
    <span data-ttu-id="a7d35-128">K tomu obvykle dojít, pokud uživatel zřízení virtuálního počítače z tento virtuální pevný disk a na tento virtuální pevný disk je uzamčen.</span><span class="sxs-lookup"><span data-stu-id="a7d35-128">This usually happen if the user provisioned a VM from this VHD and there is a lock on that VHD.</span></span> <span data-ttu-id="a7d35-129">Zkontrolujte, že neexistuje žádný virtuální počítač přidělené z tento virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="a7d35-129">Please check that there is no VM allocated from this VHD.</span></span> <span data-ttu-id="a7d35-130">Pokud chyba stále přetrvává, pak vyvolejte lístek podpory pomocí tohoto odkazu nebo z publikování portál ohledně to (podrobnosti jsou uvedeny v otázce 11 odpověď).</span><span class="sxs-lookup"><span data-stu-id="a7d35-130">If the error still persist , then please raise a support ticket using this link or from the Publishing portal regarding this (details are given in the answer of question 11).</span></span>