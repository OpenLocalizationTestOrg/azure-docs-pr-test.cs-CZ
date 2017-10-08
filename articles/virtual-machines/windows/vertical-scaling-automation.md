---
title: "Azure Automation toovertically aaaUse škálování virtuálního počítače s Windows | Microsoft Docs"
description: "Svisle škálování virtuálního počítače s Windows v odpovědi toomonitoring výstrahy s Azure Automation."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24d07f3e2e217668f18676e6d6873be4f9770349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Svisle škálování virtuálních počítačů Windows s Azure Automation.

Svislé škálování je proces hello zvýšením nebo snížením hello prostředky počítače v odpovědi toohello zatížení. V Azure to můžete udělat tak, že změníte velikost hello hello virtuálního počítače. To může pomoct v hello následující scénáře

* Pokud hello virtuálního počítače není často používají, můžete změnit velikost ho dolů tooa menší velikost tooreduce náklady na měsíční
* Pokud hello virtuálního počítače je očekávaný zátěž ve špičce, můžete se změněnou velikostí tooa větší velikost tooincrease své kapacity

Hello outline pro tooaccomplish kroky hello jedná jako níže

1. Instalační program Azure Automation tooaccess virtuální počítače
2. Importování sad runbook hello Azure Automation vertikální Škálováním do předplatného
3. Přidat runbook tooyour webhooku
4. Přidat výstrahy tooyour virtuálního počítače

> [!NOTE]
> Z důvodu hello velikost hello první virtuální počítač, velikosti hello je možné rozšířit, bude možná omezen z důvodu toohello dostupnost hello ostatní formáty v clusteru hello aktuální virtuální počítač je nasazena v. V hello publikovaných runbooků služeb automatizace používané v tomto článku jsme postará o tento případ a škálovat jenom v rámci hello níže páry velikost virtuálního počítače. To znamená, že virtuální počítač Standard_D1v2 není najednou škálovat tooStandard_G5 nebo škálovat dolů tooBasic_A0.
> 
> | Škálování pár velikosti virtuálních počítačů |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Na úrovni Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-tooaccess-your-virtual-machines"></a>Instalační program Azure Automation tooaccess virtuální počítače
První věc Hello potřebujete toodo je, vytvořit účet Azure Automation, který bude hostitelem tooscale hello sady runbook používá virtuální počítač. Služba Automation hello nedávno zavedl "Účet Spustit jako" funkci hello, která usnadňuje nastavení hello instanční objekt automaticky spouští sady runbook hello jménem hello uživatele velmi snadné. Další informace najdete v článku hello níže:

* [Ověření runbooků pomocí účtu Spustit v Azure jako](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-hello-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importování sad runbook hello Azure Automation vertikální Škálováním do předplatného
Hello sady runbook, které jsou potřebné pro svisle škálování virtuálního počítače jsou již publikován v hello Galerie Runbooků automatizace Azure. Budete potřebovat tooimport je do vašeho předplatného. Dozvíte, jak hello tooimport sady runbook ve čtení tohoto článku.

* [Galerie Runbooků a modulů pro Azure Automation.](../../automation/automation-runbook-gallery.md)

v hello obrázek níže jsou uvedeny Hello sady runbook, které je třeba importovat toobe

![Importování sad runbook](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-tooyour-runbook"></a>Přidat runbook tooyour webhooku
Jakmile importujete sady runbook hello budete potřebovat tooadd webhooku toohello runbook, mohou být aktivovány výstrahu z virtuálního počítače. Hello podrobnosti o vytváření webhooku pro vaše sada Runbook můžete přečíst zde

* [Webhooky Azure Automation.](../../automation/automation-webhooks.md)

Ujistěte se, že zkopírujete hello webhooku před jeho zavřením hello webhooku dialogu, jak je budete potřebovat v další části hello.

## <a name="add-an-alert-tooyour-virtual-machine"></a>Přidat výstrahy tooyour virtuálního počítače
1. Vyberte nastavení virtuálního počítače
2. Vyberte "Pravidla výstrah"
3. Vyberte možnost "Přidat upozornění"
4. Vyberte výstrahu hello metriky toofire na
5. Vyberte podmínku, která v případě splnění bude způsobit výstrahy toofire hello
6. Prahová hodnota pro podmínku hello vyberte v kroku 5. toobe splněny
7. Vyberte období, přes které hello službu monitorování zkontrolujte hello podmínku a prahovou hodnotu v kroky 5 a 6
8. Vložte hello webhooku, které jste zkopírovali z předchozí části hello.

![Přidat výstrahy tooVirtual 1 počítač](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Přidat výstrahy tooVirtual Machine 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

