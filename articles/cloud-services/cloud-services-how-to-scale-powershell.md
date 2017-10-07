---
title: "aaaScale Azure cloud service v prostředí Windows PowerShell | Microsoft Docs"
description: "(klasické) Zjistěte, jak toouse prostředí PowerShell tooscale webovou roli nebo role pracovního procesu nebo zmenšit v Azure."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: cfac6660e84f8ae24e4e9bdd5bf2016fb9cd7045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-a-cloud-service-in-powershell"></a>Jak služba tooscale cloudu v prostředí PowerShell

Tooscale prostředí Windows PowerShell můžete použít webovou roli nebo role pracovního procesu příchozí nebo odchozí přidáním nebo odebráním instancí.  

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Před provedením jakékoli operace na vaše předplatné pomocí prostředí PowerShell, musíte být přihlášení:

```powershell
Add-AzureAccount
```

Pokud máte více předplatných, které jsou spojené s vaším účtem, musíte toochange hello aktuálního předplatného v závislosti na tom, kde se nachází cloudové služby. toocheck hello aktuální předplatné, spusťte:

```powershell
Get-AzureSubscription -Current
```

Pokud potřebujete toochange hello aktuální předplatné, spusťte:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-hello-current-instance-count-for-your-role"></a>Zkontrolujte hello aktuální počet instancí pro vaši roli

toocheck hello aktuální stav role, spusťte:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Informace o hello role, včetně jeho aktuální počet verze a instance operačního systému by měla vrátit zpět. V takovém případě má hello role jediné instance.

![Informace o roli hello](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-hello-role-by-adding-more-instances"></a>Škálování hello role přidáním více instancí

tooscale se vaše role průchodu hello požadovaného počtu instancí jako hello **počet** parametr toohello **Set-AzureRole** rutiny:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

bloky rutiny Hello na okamžik při hello nové instance jsou zřízený a spuštěna. Během této doby, otevřete nové okno prostředí PowerShell a volání **Get-AzureRole** jako je uvedené výše, zobrazí se počet instancí cílové nové hello. A je-li si prohlédnout stav role hello hello portálu, měli byste vidět hello novou instanci spuštění:

![Instance virtuálního počítače od portálu](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Jakmile nové instance hello spustili, vrátí rutina hello úspěšně:

![Úspěch zvýšení instance role](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-hello-role-by-removing-instances"></a>Škálování role hello odebráním instancí

Je možné škálovat v roli odebráním instancí v hello stejným způsobem. Sada hello **počet** parametr na **Set-AzureRole** toohello počet instancí chcete toohave po dokončení škálování hello v operaci.

## <a name="next-steps"></a>Další kroky

Není možné tooconfigure automatickému škálování pro cloudové služby z prostředí PowerShell. toodo, který najdete v části [jak tooauto škálování cloudové služby](cloud-services-how-to-scale-portal.md).
