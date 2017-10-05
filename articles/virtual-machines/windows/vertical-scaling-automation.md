---
title: "Svisle škálování virtuální počítače s Windows pomocí Azure Automation | Microsoft Docs"
description: "Svisle škálování virtuálního počítače s Windows v reakci na monitorování výstrahy s Azure Automation."
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
ms.openlocfilehash: ea5169c1a95f00e78ae3f5f177812466eb7a0deb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Svisle škálování virtuálních počítačů Windows s Azure Automation.

Svislé škálování je proces zvýšením nebo snížením prostředky počítače v reakci na zatížení. V Azure to můžete provést změnou velikosti virtuálního počítače. To může pomoct v těchto scénářích

* Pokud virtuální počítač není často používají, můžete jeho velikost a menší velikosti, abyste snížili náklady na měsíční
* Pokud virtuální počítač se zobrazuje zátěž ve špičce, můžete změnit na větší velikost ke zvýšení jeho kapacity

Osnova pokyny k tomu je jako níže

1. Instalační program přístup k virtuálním počítačům Azure Automation.
2. Import sady runbook Azure Automation vertikální Škálováním do předplatného
3. Přidat webhook, jehož do runbooku
4. Přidání oznámení k virtuálnímu počítači

> [!NOTE]
> Vzhledem k velikosti první virtuální počítač, velikosti, které je možné rozšířit, může být omezen z důvodu dostupnosti v clusteru velikosti aktuální virtuální počítač je nasazena v. V sadách runbook publikované automatizace používané v tomto článku jsme postará o tento případ a škálovat jenom v rámci níže páry velikost virtuálního počítače. To znamená, že Standard_D1v2 virtuální počítač není najednou rozšířit tak, aby na úrovni Standard_G5 nebo škálovat na Basic_A0.
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

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Instalační program přístup k virtuálním počítačům Azure Automation.
První věc, které musíte udělat, je vytvořit účet Azure Automation, který bude hostitelem sady runbook používá škálování virtuálního počítače. Služba Automation se nedávno zavedl funkci "Spustit jako účet", která zajišťuje nastavení se objekt služby pro automatické spouštění sady runbook jménem uživatele velmi snadné. Další informace najdete v článku níže:

* [Ověření runbooků pomocí účtu Spustit v Azure jako](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Import sady runbook Azure Automation vertikální Škálováním do předplatného
Sady runbook, které jsou potřebné pro svisle škálování virtuálního počítače jsou již publikován v galerii Azure Automation Runbook. Musíte je importovat do vašeho předplatného. Můžete postup importování sad runbook načtením v následujícím článku.

* [Galerie Runbooků a modulů pro Azure Automation.](../../automation/automation-runbook-gallery.md)

Na obrázku níže jsou uvedeny sady runbook, které potřebují k importu

![Importování sad runbook](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Přidat webhook, jehož do runbooku
Jakmile importujete sady runbook, které budete muset přidat webhook, jehož do sady runbook, mohou být aktivovány výstrahu z virtuálního počítače. Podrobnosti o vytváření webhooku pro vaše sada Runbook můžete přečíst zde

* [Webhooky Azure Automation.](../../automation/automation-webhooks.md)

Ujistěte se, že zkopírujete webhooku před jeho zavřením dialogu webhooku, jak je budete potřebovat v další části.

## <a name="add-an-alert-to-your-virtual-machine"></a>Přidání oznámení k virtuálnímu počítači
1. Vyberte nastavení virtuálního počítače
2. Vyberte "Pravidla výstrah"
3. Vyberte možnost "Přidat upozornění"
4. Vyberte metriku chcete aktivovat upozornění na
5. Vyberte podmínku, která v případě splnění bude způsobit výstrahu, kterou chcete aktivovat
6. Prahová hodnota pro podmínku vyberte v kroku 5. musí být splněny
7. Vyberte dobu, za které bude služba monitorování zkontrolujte pro podmínky a prahovou hodnotu v kroky 5 a 6
8. Vložte webhook, které jste zkopírovali z předchozí části.

![Přidání upozornění k virtuálnímu počítači 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Přidání upozornění k virtuálnímu počítači 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

