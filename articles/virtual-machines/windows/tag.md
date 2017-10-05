---
title: "Jak k označování prostředků virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o označování Windows virtuální počítač vytvořený v Azure pomocí modelu nasazení Resource Manager"
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
ms.openlocfilehash: 5f00c4265cea3db02dbb09a7f81be636a3fdd3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="75613-103">Postup označení virtuálního počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="75613-103">How to tag a Windows virtual machine in Azure</span></span>
<span data-ttu-id="75613-104">Tento článek popisuje různé způsoby, jak označit virtuálního počítače s Windows v Azure pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="75613-104">This article describes different ways to tag a Windows virtual machine in Azure through the Resource Manager deployment model.</span></span> <span data-ttu-id="75613-105">Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů.</span><span class="sxs-lookup"><span data-stu-id="75613-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="75613-106">Azure aktuálně podporuje až 15 značky pro každý prostředků a skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="75613-106">Azure currently supports up to 15 tags per resource and resource group.</span></span> <span data-ttu-id="75613-107">Značky mohou umístit na prostředku v době vytvoření nebo přidat do existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="75613-107">Tags may be placed on a resource at the time of creation or added to an existing resource.</span></span> <span data-ttu-id="75613-108">Upozorňujeme, že značky jsou podporovány pro prostředky vytvořené pomocí modelu nasazení Resource Manager jenom.</span><span class="sxs-lookup"><span data-stu-id="75613-108">Please note that tags are supported for resources created via the Resource Manager deployment model only.</span></span> <span data-ttu-id="75613-109">Pokud chcete označit virtuální počítač s Linuxem, přečtěte si téma [jak k označování virtuální počítač s Linuxem v Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="75613-109">If you want to tag a Linux virtual machine, see [How to tag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a><span data-ttu-id="75613-110">Označování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="75613-110">Tagging with PowerShell</span></span>
<span data-ttu-id="75613-111">Pokud chcete vytvořit, přidat a odstranit značky pomocí prostředí PowerShell, nejprve musíte nastavit vaše [prostředí PowerShell s Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span><span class="sxs-lookup"><span data-stu-id="75613-111">To create, add, and delete tags through PowerShell, you first need to set up your [PowerShell environment with Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span></span> <span data-ttu-id="75613-112">Po dokončení instalace můžete umístit značky na výpočty, síť a úložiště prostředků při vytváření nebo po vytvoření prostředku pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75613-112">Once you have completed the setup, you can place tags on Compute, Network, and Storage resources at creation or after the resource is created via PowerShell.</span></span> <span data-ttu-id="75613-113">Tento článek se zaměří na zobrazení nebo úpravu značky umístěny na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="75613-113">This article will concentrate on viewing/editing tags placed on Virtual Machines.</span></span>

<span data-ttu-id="75613-114">První, přejděte k virtuálnímu počítači prostřednictvím `Get-AzureRmVM` rutiny.</span><span class="sxs-lookup"><span data-stu-id="75613-114">First, navigate to a Virtual Machine through the `Get-AzureRmVM` cmdlet.</span></span>

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

<span data-ttu-id="75613-115">Pokud virtuální počítač už obsahuje značky, zobrazí se všechny značky na prostředek:</span><span class="sxs-lookup"><span data-stu-id="75613-115">If your Virtual Machine already contains tags, you will then see all the tags on your resource:</span></span>

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

<span data-ttu-id="75613-116">Pokud chcete přidat značky pomocí prostředí PowerShell, můžete použít `Set-AzureRmResource` příkaz.</span><span class="sxs-lookup"><span data-stu-id="75613-116">If you would like to add tags through PowerShell, you can use the `Set-AzureRmResource` command.</span></span> <span data-ttu-id="75613-117">Poznámka: při aktualizaci značky pomocí prostředí PowerShell, značky jsou aktualizovány jako celek.</span><span class="sxs-lookup"><span data-stu-id="75613-117">Note when updating tags through PowerShell, tags are updated as a whole.</span></span> <span data-ttu-id="75613-118">Proto pokud přidáváte jednu značku k prostředku, který již má značky, musíte zahrnout všechny značky, které chcete umístit do prostředku.</span><span class="sxs-lookup"><span data-stu-id="75613-118">So if you are adding one tag to a resource that already has tags, you will need to include all the tags that you want to be placed on the resource.</span></span> <span data-ttu-id="75613-119">Dole je příklad toho, jak přidat další značky prostředku pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75613-119">Below is an example of how to add additional tags to a resource through PowerShell Cmdlets.</span></span>

<span data-ttu-id="75613-120">Tato první rutina nastaví všechny uvedené značky, umístit na *MyTestVM* k *$tags* proměnné, pomocí `Get-AzureRmResource` a `Tags` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="75613-120">This first cmdlet sets all of the tags placed on *MyTestVM* to the *$tags* variable, using the `Get-AzureRmResource` and `Tags` property.</span></span>

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

<span data-ttu-id="75613-121">Druhý příkaz zobrazí značek pro daný proměnnou.</span><span class="sxs-lookup"><span data-stu-id="75613-121">The second command displays the tags for the given variable.</span></span>

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

<span data-ttu-id="75613-122">Třetí příkaz přidá značku další na *$tags* proměnné.</span><span class="sxs-lookup"><span data-stu-id="75613-122">The third command adds an additional tag to the *$tags* variable.</span></span> <span data-ttu-id="75613-123">Všimněte si použití  **+=**  připojit nový pár klíč/hodnota do *$tags* seznamu.</span><span class="sxs-lookup"><span data-stu-id="75613-123">Note the use of the **+=** to append the new key/value pair to the *$tags* list.</span></span>

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

<span data-ttu-id="75613-124">Čtvrtý příkaz nastaví všechny značky definované v *$tags* proměnnou pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="75613-124">The fourth command sets all of the tags defined in the *$tags* variable to the given resource.</span></span> <span data-ttu-id="75613-125">V takovém případě je MyTestVM.</span><span class="sxs-lookup"><span data-stu-id="75613-125">In this case, it is MyTestVM.</span></span>

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

<span data-ttu-id="75613-126">Páté příkaz zobrazí všechny značek u prostředku.</span><span class="sxs-lookup"><span data-stu-id="75613-126">The fifth command displays all of the tags on the resource.</span></span> <span data-ttu-id="75613-127">Jak vidíte, *umístění* je nyní definována jako značka *MyLocation* jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75613-127">As you can see, *Location* is now defined as a tag with *MyLocation* as the value.</span></span>

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

<span data-ttu-id="75613-128">Další informace o označování pomocí prostředí PowerShell, podívejte se [rutiny Azure Resource][Azure Resource Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="75613-128">To learn more about tagging through PowerShell, check out the [Azure Resource Cmdlets][Azure Resource Cmdlets].</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="75613-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75613-129">Next steps</span></span>
* <span data-ttu-id="75613-130">Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [pomocí značek k uspořádání prostředků Azure][Using Tags to organize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="75613-130">To learn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].</span></span>
* <span data-ttu-id="75613-131">Jak značky vám může pomoci spravovat používání prostředků Azure, najdete v tématu [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure][Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="75613-131">To see how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
