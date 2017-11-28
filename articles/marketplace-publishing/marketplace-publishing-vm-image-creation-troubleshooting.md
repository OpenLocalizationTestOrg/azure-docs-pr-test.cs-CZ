---
title: "aaaHow tootroubleshoot běžné problémy při vytváření virtuálního pevného disku | Microsoft Docs"
description: "Řešení potíží s odpovědi toocommon otázky a problémy při vytváření virtuálního pevného disku."
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
ms.openlocfilehash: e4ff09a979bdf575badff2d33f2299abb17c947d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-common-issues-encountered-during-vhd-creation"></a><span data-ttu-id="1d150-103">Jak běžné problémy tootroubleshoot došlo při vytváření virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="1d150-103">How tootroubleshoot common issues encountered during VHD creation</span></span>
<span data-ttu-id="1d150-104">Tento článek je zadal toohelp Azure Marketplace vydavatele nebo spolusprávcem, který může zaznamenat problém nebo mít běžné otázky při publikování nebo správu jejich solution(s) virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1d150-104">This article is provided toohelp an Azure Marketplace publisher and/or co-administrator that may experience an issue or have common questions while publishing or managing their virtual machine solution(s).</span></span>

1. <span data-ttu-id="1d150-105">Změna hello název hostitele hello</span><span class="sxs-lookup"><span data-stu-id="1d150-105">How do I change hello name of hello host?</span></span>
   
    <span data-ttu-id="1d150-106">Po vytvoření virtuálního počítače nelze aktualizovat uživatele hello název hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="1d150-106">Once VM is created, users can’t update hello name of hello host.</span></span>
2. <span data-ttu-id="1d150-107">Jak tooreset hello služby Vzdálená plocha nebo jeho heslo přihlášení?</span><span class="sxs-lookup"><span data-stu-id="1d150-107">How tooreset hello Remote Desktop service or its login password?</span></span>
   
   * [<span data-ttu-id="1d150-108">Referenční informace pro virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="1d150-108">Reference for Windows VM</span></span>](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [<span data-ttu-id="1d150-109">Referenční informace pro virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="1d150-109">Reference for Linux VM</span></span>](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. <span data-ttu-id="1d150-110">Jak toogenerate nové ssh certifikáty?</span><span class="sxs-lookup"><span data-stu-id="1d150-110">How toogenerate new ssh certificates?</span></span>
   
   <span data-ttu-id="1d150-111">Naleznete odkaz toohello: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)</span><span class="sxs-lookup"><span data-stu-id="1d150-111">Please refer toohello link: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)</span></span>
4. <span data-ttu-id="1d150-112">Jak tooconfigure certifikát sítě VPN otevřené?</span><span class="sxs-lookup"><span data-stu-id="1d150-112">How tooconfigure an open VPN certificate?</span></span>
   
   <span data-ttu-id="1d150-113">Naleznete odkaz toohello: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)</span><span class="sxs-lookup"><span data-stu-id="1d150-113">Please refer toohello link: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)</span></span>
5. <span data-ttu-id="1d150-114">Co je hello zásady podpory pro spuštění serverového softwaru společnosti Microsoft v prostředí virtuálního počítače Microsoft Azure hello (infrastruktura jako služba)?</span><span class="sxs-lookup"><span data-stu-id="1d150-114">What is hello support policy for running Microsoft server software in hello Microsoft Azure virtual machine environment (infrastructure-as-a-service)?</span></span>
   
   <span data-ttu-id="1d150-115">Naleznete odkaz toohello: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span><span class="sxs-lookup"><span data-stu-id="1d150-115">Please refer toohello link: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span></span>
6. <span data-ttu-id="1d150-116">Mají virtuální počítače žádné jedinečný identifikátor?</span><span class="sxs-lookup"><span data-stu-id="1d150-116">Do Virtual Machines have any unique identifier?</span></span>
   
   <span data-ttu-id="1d150-117">Azure kóduje jedinečné ID virtuálního počítače Azure v každé virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1d150-117">Azure encodes Azure VM Unique ID in every VM.</span></span> <span data-ttu-id="1d150-118">Najdete v podrobnostech v tomto blogu a dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1d150-118">See details in this blog and documentation.</span></span>
7. <span data-ttu-id="1d150-119">Do virtuálního počítače, jak lze spravovat rozšíření vlastních skriptů hello hello spuštění úlohy?</span><span class="sxs-lookup"><span data-stu-id="1d150-119">In a VM how can I manage hello custom script extension in hello startup task?</span></span>
   
   <span data-ttu-id="1d150-120">Naleznete odkaz toohello: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)</span><span class="sxs-lookup"><span data-stu-id="1d150-120">Please refer toohello link: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)</span></span>
8. <span data-ttu-id="1d150-121">Jak toocreate virtuální počítač z Azure pomocí portálu hello hello virtuálního pevného disku, který je odeslán toopremium úložiště?</span><span class="sxs-lookup"><span data-stu-id="1d150-121">How toocreate a VM from hello Azure portal using hello VHD that is uploaded toopremium storage?</span></span>
   
   <span data-ttu-id="1d150-122">Tato funkce ještě nepodporujeme.</span><span class="sxs-lookup"><span data-stu-id="1d150-122">We do not support this feature yet.</span></span>
9. <span data-ttu-id="1d150-123">Je podporováno 32bitovou aplikaci v Azure Marketplace hello?</span><span class="sxs-lookup"><span data-stu-id="1d150-123">Is a 32-bit app supported in hello Azure Marketplace?</span></span>
   
   <span data-ttu-id="1d150-124">Informace o zásadách podpory hello naleznete odkaz toohello: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span><span class="sxs-lookup"><span data-stu-id="1d150-124">Please refer toohello link for details on hello support policy: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span></span>
10. <span data-ttu-id="1d150-125">Pokaždé, když se pokouším toocreate bitovou kopii z mé virtuálních pevných disků, zobrazí chybová zpráva hello ". Virtuální pevný disk je už registrovaný s úložištěm image jako prostředek hello "v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1d150-125">Every time I am trying toocreate an image from my VHDs, I get hello error “.VHD is already registered with image repository as hello resource” in PowerShell.</span></span> <span data-ttu-id="1d150-126">Nebyl vytvořen žádný obrázek před ani nebyl nalezen žádný obrázek s tímto názvem v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d150-126">I did not create any image before nor did I find any image with this name in Azure.</span></span> <span data-ttu-id="1d150-127">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="1d150-127">How do I resolve this?</span></span>
    
    <span data-ttu-id="1d150-128">K tomu obvykle dojít, pokud uživatel hello zřízení virtuálního počítače z tento virtuální pevný disk a na tento virtuální pevný disk je uzamčen.</span><span class="sxs-lookup"><span data-stu-id="1d150-128">This usually happen if hello user provisioned a VM from this VHD and there is a lock on that VHD.</span></span> <span data-ttu-id="1d150-129">Zkontrolujte, že neexistuje žádný virtuální počítač přidělené z tento virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="1d150-129">Please check that there is no VM allocated from this VHD.</span></span> <span data-ttu-id="1d150-130">Pokud chyba hello zůstane zachován a pak vyvolejte lístek podpory pomocí tohoto odkazu nebo z hello publikování portál týkající se to (podrobnosti jsou uvedeny v hello odpovědí otázky 11).</span><span class="sxs-lookup"><span data-stu-id="1d150-130">If hello error still persist , then please raise a support ticket using this link or from hello Publishing portal regarding this (details are given in hello answer of question 11).</span></span>
