---
title: "aaaComparing vlastních bitových kopií a vzorce v DevTest Labs | Microsoft Docs"
description: "Další informace o hello rozdíly mezi vlastních bitových kopií a vzorce jako virtuální počítač základny, abyste se mohli rozhodnout, které z nich nejlépe vyhovuje prostředí."
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
ms.openlocfilehash: 3c1d88dfe0ff94b8e825bb7a0b4aca3341c9330d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a><span data-ttu-id="2098f-103">Porovnání vlastních bitových kopií a vzorce v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="2098f-103">Comparing custom images and formulas in DevTest Labs</span></span>
<span data-ttu-id="2098f-104">Obě [vlastních bitových kopií](devtest-lab-create-template.md) a [vzorce](devtest-lab-manage-formulas.md) lze použít jako základ pro [vytvořit nové virtuální počítače](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="2098f-104">Both [custom images](devtest-lab-create-template.md) and [formulas](devtest-lab-manage-formulas.md) can be used as bases for [created new VMs](devtest-lab-add-vm-with-artifacts.md).</span></span> <span data-ttu-id="2098f-105">Ale hello mezi vlastních bitových kopií a vzorce klíče rozdíl je, že vlastní image je jednoduše image podle virtuální pevný disk, když vzorec je obrázek, který založené na virtuální pevný disk *kromě* předkonfigurované nastavení – například velikost virtuálního počítače, virtuální sítě podsíť a artefakty.</span><span class="sxs-lookup"><span data-stu-id="2098f-105">However, hello key distinction between custom images and formulas is that a custom image is simply an image based on a VHD, while a formula is an image based on a VHD *in addition to* preconfigured settings - such as VM Size, virtual network, subnet, and artifacts.</span></span> <span data-ttu-id="2098f-106">Tyto předem nakonfigurovaných nastavení se nastaví se výchozí hodnoty, které bude možné přepsat na hello čas vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2098f-106">These preconfigured settings are set up with default values that can be overridden at hello time of VM creation.</span></span> <span data-ttu-id="2098f-107">Tento článek vysvětluje některé výhody hello (specialisté) a nevýhody (cons) toousing vlastních bitových kopií a kdy vzorce.</span><span class="sxs-lookup"><span data-stu-id="2098f-107">This article explains some of hello advantages (pros) and disadvantages (cons) toousing custom images versus using formulas.</span></span>

## <a name="custom-image-pros-and-cons"></a><span data-ttu-id="2098f-108">Vlastní image výhody a nevýhody</span><span class="sxs-lookup"><span data-stu-id="2098f-108">Custom image pros and cons</span></span>
<span data-ttu-id="2098f-109">Vlastní Image poskytují toocreate statické a neměnné způsob, jak virtuální počítače z požadované prostředí.</span><span class="sxs-lookup"><span data-stu-id="2098f-109">Custom images provide a static, immutable way toocreate VMs from a desired environment.</span></span> 

<span data-ttu-id="2098f-110">**Odborníci na**</span><span class="sxs-lookup"><span data-stu-id="2098f-110">**Pros**</span></span>

* <span data-ttu-id="2098f-111">Zřizování z vlastní image virtuálních počítačů je rychlé, protože nic změny po hello, které se spouštějí, virtuální počítač z bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="2098f-111">VM provisioning from a custom image is fast as nothing changes after hello VM is spun up from hello image.</span></span> <span data-ttu-id="2098f-112">Jinými slovy nejsou žádné nastavení tooapply jako vlastní image hello je právě obrázek bez nastavení.</span><span class="sxs-lookup"><span data-stu-id="2098f-112">In other words, there are no settings tooapply as hello custom image is just an image without settings.</span></span> 
* <span data-ttu-id="2098f-113">Virtuální počítače vytvořené z jedné vlastní image jsou identické.</span><span class="sxs-lookup"><span data-stu-id="2098f-113">VMs created from a single custom image are identical.</span></span>

<span data-ttu-id="2098f-114">**Cons**</span><span class="sxs-lookup"><span data-stu-id="2098f-114">**Cons**</span></span>

* <span data-ttu-id="2098f-115">Pokud potřebujete tooupdate některých aspektů hello vlastní image, je nutné znovu vytvořit bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="2098f-115">If you need tooupdate some aspect of hello custom image, hello image must be recreated.</span></span>  

## <a name="formula-pros-and-cons"></a><span data-ttu-id="2098f-116">Vzorce výhody a nevýhody</span><span class="sxs-lookup"><span data-stu-id="2098f-116">Formula pros and cons</span></span>
<span data-ttu-id="2098f-117">Vzorce zadejte dynamické způsob toocreate virtuální počítače z hello požadovaného nastavení.</span><span class="sxs-lookup"><span data-stu-id="2098f-117">Formulas provide a dynamic way toocreate VMs from hello desired configuration/settings.</span></span>

<span data-ttu-id="2098f-118">**Odborníci na**</span><span class="sxs-lookup"><span data-stu-id="2098f-118">**Pros**</span></span>

* <span data-ttu-id="2098f-119">Změny v prostředí hello se dají zachytit v chodu hello prostřednictvím artefakty.</span><span class="sxs-lookup"><span data-stu-id="2098f-119">Changes in hello environment can be captured on hello fly via artifacts.</span></span> <span data-ttu-id="2098f-120">Například pokud chcete nainstalovat s nejnovější bity hello z vaší verze kanálu virtuálního počítače nebo zařazení hello nejnovější kód z úložiště, můžete jednoduše zadat artefakt nasadí nejnovější bits hello nebo využívá hello nejnovější kód ve vzorci hello společně se službou cíl základní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="2098f-120">For example, if you want a VM installed with hello latest bits from your release pipeline or enlist hello latest code from your repository, you can simply specify an artifact that deploys hello latest bits or enlists hello latest code in hello formula together with a target base image.</span></span> <span data-ttu-id="2098f-121">Vždy, když tento vzorec je použité toocreate virtuální počítače, kód nejnovější bits hello jsou nasazené zařazen toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2098f-121">Whenever this formula is used toocreate VMs, hello latest bits/code are deployed/enlisted toohello VM.</span></span> 
* <span data-ttu-id="2098f-122">Vzorce můžete definovat výchozí nastavení, které nemůžete zadat vlastní Image – například velikosti virtuálních počítačů a nastavení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2098f-122">Formulas can define default settings that custom images cannot provide - such as VM sizes and virtual network settings.</span></span> 
* <span data-ttu-id="2098f-123">Hello nastavení uložená v vzorec se zobrazují jako výchozí hodnoty, ale můžete upravovat, když je vytvořena hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2098f-123">hello settings saved in a formula are shown as default values, but can be modified when hello VM is created.</span></span> 

<span data-ttu-id="2098f-124">**Cons**</span><span class="sxs-lookup"><span data-stu-id="2098f-124">**Cons**</span></span>

* <span data-ttu-id="2098f-125">Vytvoření virtuálního počítače ze vzorce může trvat delší dobu než vytvoření virtuálního počítače z vlastní image.</span><span class="sxs-lookup"><span data-stu-id="2098f-125">Creating a VM from a formula can take more time than creating a VM from a custom image.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="2098f-126">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="2098f-126">Related blog posts</span></span>
* [<span data-ttu-id="2098f-127">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="2098f-127">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="2098f-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2098f-128">Next steps</span></span>
- [<span data-ttu-id="2098f-129">DevTest Labs – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2098f-129">DevTest Labs FAQ</span></span>](devtest-lab-faq.md)