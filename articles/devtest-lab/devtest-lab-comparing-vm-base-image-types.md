---
title: "Porovnání vlastních bitových kopií a vzorce v DevTest Labs | Microsoft Docs"
description: "Další informace o rozdílech mezi vlastních bitových kopií a vzorce jako virtuální počítač základny, abyste se mohli rozhodnout, které z nich nejlépe vyhovuje prostředí."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: ff771abc26c08f0adb977c29739d2f5c91924b21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a><span data-ttu-id="6d745-103">Porovnání vlastních bitových kopií a vzorce v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="6d745-103">Comparing custom images and formulas in DevTest Labs</span></span>
<span data-ttu-id="6d745-104">Obě [vlastních bitových kopií](devtest-lab-create-template.md) a [vzorce](devtest-lab-manage-formulas.md) lze použít jako základ pro [vytvořit nové virtuální počítače](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="6d745-104">Both [custom images](devtest-lab-create-template.md) and [formulas](devtest-lab-manage-formulas.md) can be used as bases for [created new VMs](devtest-lab-add-vm-with-artifacts.md).</span></span> <span data-ttu-id="6d745-105">Však mezi vlastních bitových kopií a vzorce klíče rozdíl je, že vlastní image je jednoduše image podle virtuální pevný disk, když vzorec je obrázek, který založené na virtuální pevný disk *kromě* předkonfigurované nastavení – například velikost virtuálního počítače, virtuální sítě, podsítě a artefakty.</span><span class="sxs-lookup"><span data-stu-id="6d745-105">However, the key distinction between custom images and formulas is that a custom image is simply an image based on a VHD, while a formula is an image based on a VHD *in addition to* preconfigured settings - such as VM Size, virtual network, subnet, and artifacts.</span></span> <span data-ttu-id="6d745-106">Tyto předem nakonfigurovaných nastavení se nastaví se výchozí hodnoty, které je možné přepsat v době vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d745-106">These preconfigured settings are set up with default values that can be overridden at the time of VM creation.</span></span> <span data-ttu-id="6d745-107">Tento článek vysvětluje některé (specialisté) výhody a nevýhody (cons) pomocí vlastních bitových kopií a kdy vzorce.</span><span class="sxs-lookup"><span data-stu-id="6d745-107">This article explains some of the advantages (pros) and disadvantages (cons) to using custom images versus using formulas.</span></span>

## <a name="custom-image-pros-and-cons"></a><span data-ttu-id="6d745-108">Vlastní image výhody a nevýhody</span><span class="sxs-lookup"><span data-stu-id="6d745-108">Custom image pros and cons</span></span>
<span data-ttu-id="6d745-109">Vlastní Image poskytují statické, neměnné způsob, jak vytvořit virtuální počítače z požadované prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d745-109">Custom images provide a static, immutable way to create VMs from a desired environment.</span></span> 

<span data-ttu-id="6d745-110">**Odborníci na**</span><span class="sxs-lookup"><span data-stu-id="6d745-110">**Pros**</span></span>

* <span data-ttu-id="6d745-111">Zřizování virtuálních počítačů z vlastní image je rychlé, protože nic změny po je spuštěné virtuální počítač z bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6d745-111">VM provisioning from a custom image is fast as nothing changes after the VM is spun up from the image.</span></span> <span data-ttu-id="6d745-112">Jinými slovy nejsou žádné nastavení, které se použijí jako vlastní image je právě obrázek bez nastavení.</span><span class="sxs-lookup"><span data-stu-id="6d745-112">In other words, there are no settings to apply as the custom image is just an image without settings.</span></span> 
* <span data-ttu-id="6d745-113">Virtuální počítače vytvořené z jedné vlastní image jsou identické.</span><span class="sxs-lookup"><span data-stu-id="6d745-113">VMs created from a single custom image are identical.</span></span>

<span data-ttu-id="6d745-114">**Cons**</span><span class="sxs-lookup"><span data-stu-id="6d745-114">**Cons**</span></span>

* <span data-ttu-id="6d745-115">Pokud budete potřebovat aktualizace některých aspektů vlastní image, je nutné znovu vytvořit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="6d745-115">If you need to update some aspect of the custom image, the image must be recreated.</span></span>  

## <a name="formula-pros-and-cons"></a><span data-ttu-id="6d745-116">Vzorce výhody a nevýhody</span><span class="sxs-lookup"><span data-stu-id="6d745-116">Formula pros and cons</span></span>
<span data-ttu-id="6d745-117">Vzorce zadejte dynamické způsob, jak vytvořit virtuální počítače z požadované nastavení nebo konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6d745-117">Formulas provide a dynamic way to create VMs from the desired configuration/settings.</span></span>

<span data-ttu-id="6d745-118">**Odborníci na**</span><span class="sxs-lookup"><span data-stu-id="6d745-118">**Pros**</span></span>

* <span data-ttu-id="6d745-119">Změny v prostředí se dají zachytit za chodu prostřednictvím artefakty.</span><span class="sxs-lookup"><span data-stu-id="6d745-119">Changes in the environment can be captured on the fly via artifacts.</span></span> <span data-ttu-id="6d745-120">Například pokud chcete nainstalovat s nejnovější bity z vaší verze kanálu virtuálního počítače nebo zařazení nejnovější kód z úložiště, je jednoduše zadat artefakt nasadí nejnovější bits nebo využívá nejnovější kód ve vzorci společně s cíl základní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="6d745-120">For example, if you want a VM installed with the latest bits from your release pipeline or enlist the latest code from your repository, you can simply specify an artifact that deploys the latest bits or enlists the latest code in the formula together with a target base image.</span></span> <span data-ttu-id="6d745-121">Vždy, když tento vzorec se používá k vytvoření virtuálních počítačů, nejnovější bits nebo kód jsou zařazeny do virtuálního počítače jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="6d745-121">Whenever this formula is used to create VMs, the latest bits/code are deployed/enlisted to the VM.</span></span> 
* <span data-ttu-id="6d745-122">Vzorce můžete definovat výchozí nastavení, které nemůžete zadat vlastní Image – například velikosti virtuálních počítačů a nastavení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6d745-122">Formulas can define default settings that custom images cannot provide - such as VM sizes and virtual network settings.</span></span> 
* <span data-ttu-id="6d745-123">Nastavení uložená v vzorec se zobrazují jako výchozí hodnoty, ale můžete upravovat, když je virtuální počítač vytvořený.</span><span class="sxs-lookup"><span data-stu-id="6d745-123">The settings saved in a formula are shown as default values, but can be modified when the VM is created.</span></span> 

<span data-ttu-id="6d745-124">**Cons**</span><span class="sxs-lookup"><span data-stu-id="6d745-124">**Cons**</span></span>

* <span data-ttu-id="6d745-125">Vytvoření virtuálního počítače ze vzorce může trvat delší dobu než vytvoření virtuálního počítače z vlastní image.</span><span class="sxs-lookup"><span data-stu-id="6d745-125">Creating a VM from a formula can take more time than creating a VM from a custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="6d745-126">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="6d745-126">Related blog posts</span></span>
* [<span data-ttu-id="6d745-127">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="6d745-127">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="6d745-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d745-128">Next steps</span></span>
- [<span data-ttu-id="6d745-129">DevTest Labs – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="6d745-129">DevTest Labs FAQ</span></span>](devtest-lab-faq.md)