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
# <a name="how-tootag-a-windows-virtual-machine-in-azure"></a>Jak tootag virtuálního počítače s Windows v Azure
Tento článek popisuje různé způsoby tootag virtuálního počítače s Windows v Azure pomocí modelu nasazení Resource Manager hello. Značky jsou páry klíč – hodnota definovaný uživatelem, které mohou být umístěny přímo na prostředek nebo skupina zdrojů. Azure aktuálně podporuje až too15 značky na prostředků a skupina prostředků. Značky mohou být uvedeny na prostředku v době vytvoření hello nebo přidat tooan existující prostředek. Upozorňujeme, že značky jsou podporovány pro vytvořené prostřednictvím hello modelu nasazení Resource Manager pouze prostředky. Pokud chcete tootag virtuální počítač s Linuxem, najdete v části [jak tootag virtuální počítač s Linuxem v Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Označování pomocí prostředí PowerShell
toocreate, přidání a odstranění značky pomocí prostředí PowerShell, musíte nejprve tooset si vaše [prostředí PowerShell s Azure Resource Manager][PowerShell environment with Azure Resource Manager]. Po dokončení instalace hello můžete umístit značky na výpočty, síť a úložiště prostředků při vytváření nebo po vytvoření hello prostředků pomocí prostředí PowerShell. Tento článek se zaměří na zobrazení nebo úpravu značky umístěny na virtuální počítače.

Nejprve přejděte tooa virtuálního počítače prostřednictvím hello `Get-AzureRmVM` rutiny.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Pokud virtuální počítač už obsahuje značky, zobrazí se všechny značky hello na prostředek:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Pokud chcete značky tooadd pomocí prostředí PowerShell, můžete použít hello `Set-AzureRmResource` příkaz. Poznámka: při aktualizaci značky pomocí prostředí PowerShell, značky jsou aktualizovány jako celek. Takže pokud přidáváte jeden prostředek tooa značky, který už má značky, budete potřebovat tooinclude všechny hello značky, které chcete umístit do prostředků hello toobe. Níže je příklad, jak tooadd další značky tooa prostředků pomocí rutin prostředí PowerShell.

Tato první rutina nastaví všechny značky hello umístit na *MyTestVM* toohello *$tags* proměnné pomocí hello `Get-AzureRmResource` a `Tags` vlastnost.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

druhý příkaz Hello zobrazí hello značky pro hello zadané proměnné.

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

třetí příkaz Hello přidá další značky toohello *$tags* proměnné. Všimněte si použití hello hello  **+=**  tooappend hello nové toohello dvojice klíč/hodnota *$tags* seznamu.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

příkaz čtvrtý Hello nastaví všechny značky hello definované v hello *$tags* proměnné toohello zadaný prostředek. V takovém případě je MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Hello páté příkaz zobrazí všechny značky hello na hello prostředku. Jak vidíte, *umístění* je nyní definována jako značka *MyLocation* jako hodnota hello.

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

Další informace o toolearn označování pomocí prostředí PowerShell, podívejte se na hello [rutiny Azure Resource][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o označování prostředků Azure, najdete v části [přehled Azure Resource Manageru] [ Azure Resource Manager Overview] a [tooorganize pomocí značky vašich prostředků Azure] [ Using Tags tooorganize your Azure Resources].
* toosee způsobu značky vám může pomoci spravovat používání prostředky Azure, najdete v [pochopení vaše faktura Azure] [ Understanding your Azure Bill] a [proniknout do vaší spotřeby prostředků Microsoft Azure] [Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
