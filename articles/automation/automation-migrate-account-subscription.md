---
title: "aaaMigrate účet Automation a prostředky | Microsoft Docs"
description: "Tento článek popisuje, jak toomove Automation účtu v Azure Automation a přidružených prostředků z tooanother jedno předplatné."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte
ms.openlocfilehash: 201c9091cd2d78d7ea407c1e5fb27f366bb4fa8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-automation-account-and-resources"></a>Migraci účet Automation a prostředků
Pro účty Automation a jeho přidružené prostředky (tj. prostředky, sady runbook, moduly atd.), které jste vytvořili v hello portál Azure a chcete toomigrate z jednoho prostředku skupiny tooanother nebo z tooanother jedno předplatné, můžete to provést snadno s Hello [přesunout prostředky](../azure-resource-manager/resource-group-move-resources.md) funkce, které jsou k dispozici v hello portálu Azure. Před pokračováním této akce, je však měli nejdřív si následující hello [kontrolní seznam před přesunutím prostředků](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) a kromě toho hello seznamu konkrétní tooAutomation.   

1. Hello cílového předplatného/skupiny prostředků musí být ve stejné oblasti jako zdroj hello.  Znamená, nelze jej přesunout účty Automation v oblastech.
2. Při přesouvání prostředků (např. sady runbook, úlohy atd.), hello zdrojová skupina a hello cílové skupiny jsou zamčené pro dobu trvání hello hello operace. Zápis a operace odstranění se v hello skupiny blokuje až po dokončení přesunu hello.  
3. Všechny runbooky a proměnné, které odkazují na ID prostředků nebo předplatného z existujícího předplatného hello potřebovat toobe aktualizován po dokončení migrace.   

> [!NOTE]
> Tato funkce nepodporuje přesunutí klasické prostředky služby automation.
>
>

## <a name="toomove-hello-automation-account-using-hello-portal"></a>toomove hello účtu Automation pomocí portálu hello
1. Z vašeho účtu Automation, klikněte na tlačítko **přesunout** hello horní části okna hello.<br> ![Přesuňte možnost](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. Na hello **přesunout prostředky** okno, Všimněte si, že uvede tooboth související prostředky účtu Automation a vaší skupiny prostředků.  Vyberte hello **předplatné** a **skupiny prostředků** z rozevíracího seznamu hello nebo vyberte hello možnost **vytvořit novou skupinu prostředků** a zadejte nový název skupiny prostředků v Zadané pole Hello.  
3. Přečtěte si a vyberte hello políčko tooacknowledge jste *pochopit nástroje a skripty, bude nutné aktualizovat toobe toouse nový prostředek ID po přesunutí prostředků* a pak klikněte na **OK**.<br> ![Přesun prostředků okna](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Tato akce bude trvat několik minut toocomplete.  V **oznámení**, zobrazí se stav každé akce, který probíhá - ověřování, migrace a potom nakonec po dokončení.     

## <a name="toomove-hello-automation-account-using-powershell"></a>toomove hello účtu Automation pomocí prostředí PowerShell
toomove existující skupiny prostředků tooanother prostředky Automation nebo předplatného, použijte hello **Get-AzureRmResource** rutiny tooget hello konkrétní účet Automation a potom **přesunutí AzureRmResource** rutiny tooperform hello přesunout.

Hello první příklad ukazuje, jak toomove Automation účet tooa novou skupinu prostředků.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

Po provedení hello výše ukázkový kód bude výzvami tooverify tooperform chcete tuto akci.  Po kliknutí na tlačítko **Ano** a povolit hello tooproceed skriptu, neobdržíte žádné oznámení, když provádí migraci hello.  

toomove tooa nové předplatné, vložte hodnotu pro hello *DestinationSubscriptionId* parametr.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Stejně jako v předchozím příkladu hello bude výzvami tooconfirm hello move.  

## <a name="next-steps"></a>Další kroky
* Další informace o přesunutí skupiny prostředků toonew prostředků nebo předplatného najdete v tématu [přesunout skupiny prostředků toonew prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md)
* Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v části příliš[řízení přístupu na základě Role ve službě Azure Automation](automation-role-based-access-control.md).
* toolearn o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](../azure-resource-manager/powershell-azure-resource-manager.md)
* toolearn o funkcích portálu pro správu předplatného, najdete v části [pomocí hello portálu Azure toomanage prostředky](../azure-resource-manager/resource-group-portal.md).
