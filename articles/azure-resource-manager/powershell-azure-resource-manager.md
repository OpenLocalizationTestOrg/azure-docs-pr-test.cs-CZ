---
title: "aaaManage Azure řešení pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí Azure PowerShell a správce prostředků toomanage vašich prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 47a91af8d7eb59585bcfd43571ce76b90a0d7971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Správa prostředků pomocí Azure PowerShell a správce prostředků
> [!div class="op_single_selector"]
> * [Azure Portal](resource-group-portal.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST API](resource-manager-rest-api.md)
>
>

V tomto článku se dozvíte, jak toomanage řešení Azure PowerShell a Azure Resource Manager. Pokud nejste obeznámeni s Resource Managerem, přečtěte si téma [Přehled služby Správce prostředků](resource-group-overview.md). Toto téma se zaměřuje na úlohy správy. Vaším úkolem je:

1. Vytvoření skupiny prostředků
2. Přidat skupinu prostředků toohello prostředků
3. Přidání značka toohello prostředku
4. Zadat dotaz na prostředky na základě názvy nebo hodnoty značky
5. Použít a odebere se zámek na prostředku hello
6. Odstranit skupinu prostředků.

Tento článek nezobrazuje jak toodeploy předplatné tooyour šablony Resource Manageru. Podrobnosti naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Začínáme s Azure PowerShell

Pokud jste prostředí Azure PowerShell nenainstalovali, přečtěte si téma [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Pokud jste nainstalovali Azure PowerShell v posledních hello ale nebyly aktualizovány nedávno, zvažte instalaci nejnovější verze hello. Můžete aktualizovat verzi hello prostřednictvím hello stejnou metodu používá tooinstall ho. Například pokud jste použili hello instalačního programu webové platformy, spusťte znovu a vyhledejte aktualizaci.

toocheck vaší verzi hello modul prostředky Azure, použijte následující rutinu hello:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Toto téma bylo aktualizováno pro verzi 3.3.0. Pokud máte starší verzi, nemusí odpovídat prostředí hello kroky uvedené v tomto tématu. Dokumentaci o rutinách hello v této verzi najdete v tématu [AzureRM.Resources modulu](/powershell/module/azurerm.resources).

## <a name="log-in-tooyour-azure-account"></a>Přihlaste se tooyour účet Azure
Před zahájením práce na řešení, musíte se přihlásit v tooyour účtu.

toolog v tooyour účet Azure, použijte hello **Login-AzureRmAccount** rutiny.

```powershell
Login-AzureRmAccount
```

Hello rutina vás vyzve k zadání hello přihlašovací údaje k účtu Azure. Po přihlášení stahování nastavení svého účtu tak, aby byly k dispozici tooAzure prostředí PowerShell.

Hello rutina vrátí informace o odběru toouse pro úlohy hello váš účet a hello.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Pokud máte více než jedno předplatné, můžete přepnout tooa jiné předplatné. První Podíváme se, Všechna předplatná hello pro váš účet.

```powershell
Get-AzureRmSubscription
```

Vrátí povolených a zakázaných odběrů.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

tooswitch tooa jiného předplatného, zadejte název odběru hello s hello **Set-AzureRmContext** rutiny.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Před nasazením libovolné předplatné, tooyour prostředky, musíte vytvořit skupinu prostředků, která bude obsahovat prostředky hello.

toocreate skupinu prostředků použijte hello **New-AzureRmResourceGroup** rutiny. příkaz Hello používá hello **název** toospecify parametr název skupiny prostředků hello a hello **umístění** parametr toospecify jeho umístění.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

výstup Hello je ve formátu hello:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Pokud potřebujete skupinu prostředků tooretrieve hello později, použijte hello následující rutiny:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

tooget všechny hello skupiny prostředků v rámci vašeho předplatného, nezadávejte název:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-tooa-resource-group"></a>Přidat skupinu prostředků tooa prostředky
tooadd skupinu prostředků toohello prostředků, můžete použít hello **New-AzureRmResource** rutina nebo rutiny, která je toohello konkrétní typ prostředku, kterou vytváříte (jako je **AzureRmStorageAccount nový**). Může pro vás snadnější toouse rutinu, která je tooa konkrétní typ prostředku, protože obsahuje parametry pro hello vlastnosti, které jsou potřebné pro nový prostředek hello. toouse **New-AzureRmResource**, musíte znát všechny vlastnosti tooset hello bez výzvy pro ně.

Přidání prostředku pomocí rutin však může být matoucí budoucí protože hello nový prostředek neexistuje v šabloně Resource Manager. Společnost Microsoft doporučuje definování hello infrastrukturu pro vaše řešení Azure v šabloně Resource Manager. Šablony umožňují tooreliably a opakovaného nasazování svého řešení. Pro toto téma vytvoříte účet úložiště s rutiny prostředí PowerShell, ale později generování šablonu z vaší skupiny prostředků.

Hello následující rutina vytvoří účet úložiště. Nepoužívejte název hello v příkladu hello zadejte jedinečný název pro účet úložiště hello. Název Hello musí být v rozmezí 3 až 24 znaků a použít pouze čísla a malá písmena. Pokud použijete název hello hello příkladu, zobrazí chybu, protože tento název je již používán.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Pokud potřebujete tooretrieve tohoto prostředku novější, použijte hello následující rutiny:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Přidání značky

Značky umožňují tooorganize vaše prostředky podle toodifferent vlastnosti. Například může mít několik prostředků v různých prostředků skupiny, které patří toohello stejného oddělení. Můžete použít toomark oddělení značky a hodnota toothose prostředky je jako patřící toohello stejné kategorii. Nebo můžete označit, zda je prostředek použít v produkčním i testovacím prostředí. V tomto tématu použít jeden prostředek tooonly značky, ale ve vašem prostředí nejpravděpodobnější má smysl, že tooapply značky tooall vašich prostředků.

následující rutina Hello platí dva účet úložiště tooyour značky:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Značky jsou aktualizovány jako jednoho objektu. tooadd prostředek tooa značky, která již obsahuje značky, nejdřív načtěte stávající značky hello. Přidejte hello nové značky toohello objekt, který obsahuje stávající značky hello a znovu použít všechny hello značky toohello prostředků.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Hledat prostředky

Použití hello **najít AzureRmResource** rutiny tooretrieve prostředky pro různé vyhledávací podmínky.

* tooget prostředek podle názvu, zadejte hello **ResourceNameContains** parametr:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* tooget všechny hello prostředky ve skupině prostředků, poskytovat hello **ResourceGroupNameContains** parametr:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* tooget všechny prostředky hello se název značky a hodnotou, zadejte hello **TagName** a **TagValue** parametry:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* tooall hello zdroje s konkrétní typ prostředku, obsahují hello **ResourceType** parametr:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a>Zamknout prostředku

Pokud budete potřebovat toomake opravdu důležité prostředku není omylem odstranit nebo upravit, použít toohello prostředek lock. Můžete buď zadat **CanNotDelete** nebo **jen pro čtení**.

zámky správy toocreate nebo odstranit, musíte mít přístup příliš`Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce. Hello předdefinovaných rolí pouze vlastník a správce přístupu uživatelů mají tyto akce.

tooapply zámek, použijte následující rutinu hello:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Hello uzamčeném prostředků v předchozím příkladu hello nelze odstranit, dokud se neodstraní hello zámku. tooremove zámek, použijte:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Další informace o nastavení zámky najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Odebrat prostředky nebo skupinu prostředků
Můžete odebrat prostředek nebo skupina prostředků. Když odeberete skupinu prostředků, je také odebrat všechny hello prostředky v příslušné skupině prostředků.

* toodelete prostředku z hello skupinu prostředků, použijte hello **odebrat AzureRmResource** rutiny. Tato rutina odstraní hello prostředků, ale nedojde k odstranění skupiny prostředků hello.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* toodelete skupinu prostředků a všechny její prostředky používají hello **Remove-AzureRmResourceGroup** rutiny.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Pro obě rutiny se zobrazí výzva tooconfirm chcete tooremove hello prostředek nebo skupina prostředků. Pokud se odstraní hello operace úspěšně hello prostředek nebo skupina prostředků, vrátí **True**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Spusťte skripty správce prostředků se Azure Automation.

Toto téma ukazuje, jak tooperform základní operace na vašich prostředků Azure PowerShell. Pro pokročilejší scénáře správy můžete obvykle mají toocreate skriptu a znovu použít tento skript podle potřeby nebo podle plánu. [Služby Azure Automation](../automation/automation-intro.md) poskytuje způsob pro vás často používané tooautomate skriptů, které spravují řešení Azure.

Hello následující témata ukazují, jak toouse automatizace Azure Resource Manager a prostředí PowerShell tooeffectively provádět úlohy správy:

- Informace o vytvoření sady runbook najdete v tématu [Můj první Powershellový runbook](../automation/automation-first-runbook-textual-powershell.md).
- Informace o práci s galerií skriptů, najdete v tématu [Galerie Runbooků a modulů pro Azure Automation](../automation/automation-runbook-gallery.md).
- Sady runbook, které spuštění a zastavení virtuálních počítačů, najdete v části [scénáře Azure Automation: formátu JSON pomocí značek toocreate plán pro virtuální počítač Azure spuštění a vypnutí](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Sady runbook, které spuštění a zastavení počítačem nepracujete virtuální počítače, naleznete v části [spuštění a zastavení virtuálních počítačů během řešení počítačem nepracujete v automatizaci](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Další kroky
* toolearn o vytváření šablon Resource Manageru, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).
* toolearn o nasazení šablony, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).
* Můžete přesunout existující prostředky tooa novou skupinu prostředků. Příklady najdete v tématu [tooNew přesunutí prostředků skupiny prostředků nebo předplatného](resource-group-move-resources.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

