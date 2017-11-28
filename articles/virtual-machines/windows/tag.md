---
title: "aaaHow tootag prostředek virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o označování Windows virtuální počítač vytvořený v Azure pomocí modelu nasazení Resource Manager hello"
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 160416ddc35998b3c98c6e579668a6a5eb6de6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="2612c-103">Jak tootag virtuálního počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="2612c-103">How tootag a Windows virtual machine in Azure</span></span>
<span data-ttu-id="2612c-104">Tento článek popisuje různé způsoby tootag virtuálního počítače s Windows v Azure pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="2612c-104">This article describes different ways tootag a Windows virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="2612c-105">Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů.</span><span class="sxs-lookup"><span data-stu-id="2612c-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="2612c-106">Azure aktuálně podporuje až too15 značky na prostředků a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="2612c-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="2612c-107">Značky mohou být uvedeny na prostředku v době vytvoření hello nebo přidat tooan existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="2612c-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="2612c-108">Upozorňujeme, že značky jsou podporovány pro vytvořené prostřednictvím hello modelu nasazení Resource Manager pouze prostředky.</span><span class="sxs-lookup"><span data-stu-id="2612c-108">Please note that tags are supported for resources created via hello Resource Manager deployment model only.</span></span> <span data-ttu-id="2612c-109">Pokud chcete tootag virtuální počítač s Linuxem, najdete v části [jak tootag virtuální počítač s Linuxem v Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2612c-109">If you want tootag a Linux virtual machine, see [How tootag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a><span data-ttu-id="2612c-110">Označování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2612c-110">Tagging with PowerShell</span></span>
<span data-ttu-id="2612c-111">toocreate, přidání a odstranění značky pomocí prostředí PowerShell, musíte nejprve tooset si vaše [prostředí PowerShell s Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span><span class="sxs-lookup"><span data-stu-id="2612c-111">toocreate, add, and delete tags through PowerShell, you first need tooset up your [PowerShell environment with Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span></span> <span data-ttu-id="2612c-112">Po dokončení instalace hello můžete umístit značky na výpočty, síť a úložiště prostředků při vytváření nebo po vytvoření hello prostředků pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2612c-112">Once you have completed hello setup, you can place tags on Compute, Network, and Storage resources at creation or after hello resource is created via PowerShell.</span></span> <span data-ttu-id="2612c-113">Tento článek se zaměří na zobrazení nebo úpravu značky umístěny na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2612c-113">This article will concentrate on viewing/editing tags placed on Virtual Machines.</span></span>

<span data-ttu-id="2612c-114">Nejprve přejděte tooa virtuálního počítače prostřednictvím hello `Get-AzureRmVM` rutiny.</span><span class="sxs-lookup"><span data-stu-id="2612c-114">First, navigate tooa Virtual Machine through hello `Get-AzureRmVM` cmdlet.</span></span>

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

<span data-ttu-id="2612c-115">Pokud virtuální počítač už obsahuje značky, zobrazí se všechny značky hello na prostředek:</span><span class="sxs-lookup"><span data-stu-id="2612c-115">If your Virtual Machine already contains tags, you will then see all hello tags on your resource:</span></span>

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

<span data-ttu-id="2612c-116">Pokud chcete značky tooadd pomocí prostředí PowerShell, můžete použít hello `Set-AzureRmResource` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2612c-116">If you would like tooadd tags through PowerShell, you can use hello `Set-AzureRmResource` command.</span></span> <span data-ttu-id="2612c-117">Poznámka: při aktualizaci značky pomocí prostředí PowerShell, značky jsou aktualizovány jako celek.</span><span class="sxs-lookup"><span data-stu-id="2612c-117">Note when updating tags through PowerShell, tags are updated as a whole.</span></span> <span data-ttu-id="2612c-118">Takže pokud přidáváte jeden prostředek tooa značky, který už má značky, budete potřebovat tooinclude všechny hello značky, které chcete umístit do prostředků hello toobe.</span><span class="sxs-lookup"><span data-stu-id="2612c-118">So if you are adding one tag tooa resource that already has tags, you will need tooinclude all hello tags that you want toobe placed on hello resource.</span></span> <span data-ttu-id="2612c-119">Níže je příklad, jak tooadd další značky tooa prostředků pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2612c-119">Below is an example of how tooadd additional tags tooa resource through PowerShell Cmdlets.</span></span>

<span data-ttu-id="2612c-120">Tato první rutina nastaví všechny značky hello umístit na *MyTestVM* toohello *$tags* proměnné pomocí hello `Get-AzureRmResource` a `Tags` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2612c-120">This first cmdlet sets all of hello tags placed on *MyTestVM* toohello *$tags* variable, using hello `Get-AzureRmResource` and `Tags` property.</span></span>

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

<span data-ttu-id="2612c-121">druhý příkaz Hello zobrazí hello značky pro hello zadané proměnné.</span><span class="sxs-lookup"><span data-stu-id="2612c-121">hello second command displays hello tags for hello given variable.</span></span>

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

<span data-ttu-id="2612c-122">třetí příkaz Hello přidá další značky toohello *$tags* proměnné.</span><span class="sxs-lookup"><span data-stu-id="2612c-122">hello third command adds an additional tag toohello *$tags* variable.</span></span> <span data-ttu-id="2612c-123">Všimněte si použití hello hello  **+=**  tooappend hello nové toohello dvojice klíč/hodnota *$tags* seznamu.</span><span class="sxs-lookup"><span data-stu-id="2612c-123">Note hello use of hello **+=** tooappend hello new key/value pair toohello *$tags* list.</span></span>

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

<span data-ttu-id="2612c-124">příkaz čtvrtý Hello nastaví všechny značky hello definované v hello *$tags* proměnné toohello zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="2612c-124">hello fourth command sets all of hello tags defined in hello *$tags* variable toohello given resource.</span></span> <span data-ttu-id="2612c-125">V takovém případě je MyTestVM.</span><span class="sxs-lookup"><span data-stu-id="2612c-125">In this case, it is MyTestVM.</span></span>

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

<span data-ttu-id="2612c-126">Hello páté příkaz zobrazí všechny značky hello na hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="2612c-126">hello fifth command displays all of hello tags on hello resource.</span></span> <span data-ttu-id="2612c-127">Jak vidíte, *umístění* je nyní definována jako značka *MyLocation* jako hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="2612c-127">As you can see, *Location* is now defined as a tag with *MyLocation* as hello value.</span></span>

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

<span data-ttu-id="2612c-128">Další informace o toolearn označování pomocí prostředí PowerShell, podívejte se na hello [rutiny Azure Resource][Azure Resource Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="2612c-128">toolearn more about tagging through PowerShell, check out hello [Azure Resource Cmdlets][Azure Resource Cmdlets].</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="2612c-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2612c-129">Next steps</span></span>
* <span data-ttu-id="2612c-130">toolearn Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [tooorganize pomocí značky vašich prostředků Azure] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="2612c-130">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="2612c-131">toosee způsobu značky vám může pomoci spravovat používání prostředky Azure, najdete v [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="2612c-131">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
