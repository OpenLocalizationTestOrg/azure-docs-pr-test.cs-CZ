---
title: "Kontejner Azure registru úložiště | Microsoft Docs"
description: "Jak používat Azure registru kontejner úložiště pro imagí Dockeru"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.openlocfilehash: 1e5d5ea5b1ec121fe008abc48178b1d58f540ce1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-private-docker-container-registry-using-the-azure-powershell"></a>Vytvořit privátní registru kontejner Docker pomocí Azure PowerShell
Pomocí příkazů v [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) vytvoření kontejneru registru a jeho nastavení z počítače se systémem Windows. Můžete také vytvořit a spravovat pomocí registrech kontejneru [portál Azure](container-registry-get-started-portal.md), [rozhraní příkazového řádku Azure](container-registry-get-started-azure-cli.md), nebo prostřednictvím kódu programu s registrem kontejneru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Související informace a koncepty najdete v tématu [s přehledem](container-registry-intro.md).
* Úplný seznam podporovaných rutin najdete v tématu [rutiny správy registru kontejner Azure](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).


## <a name="prerequisites"></a>Požadavky
* **Prostředí Azure PowerShell**: instalace a Začínáme s Azure PowerShell najdete v tématu [pokyny k instalaci](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). Přihlaste se ke svému předplatnému Azure spuštěním příkazu `Login-AzureRMAccount`. Další informace najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).
* **Skupina prostředků:** Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků. Ujistěte se, že je skupina prostředků v umístění, kde je služba Container Registry [dostupná](https://azure.microsoft.com/regions/services/). Vytvoření skupiny prostředků pomocí Azure Powershellu naleznete v tématu [referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).
* **Účet úložiště** (volitelné): Pro účely zálohování registru kontejnerů vytvořte standardní [účet úložiště](../storage/common/storage-introduction.md) Azure ve stejném umístění. Pokud při vytváření registru pomocí příkazu `New-AzureRMContainerRegistry` nezadáte účet úložiště, příkaz ho vytvoří za vás. Pokud chcete vytvořit účet úložiště pomocí prostředí PowerShell, najdete v části [referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount). Storage úrovně Premium se v tuto chvíli nepodporuje.
* **Instanční objekt** (volitelné): Když vytvoříte soubor registru pomocí prostředí PowerShell, ve výchozím nastavení není nastaven pro přístup. Podle potřeby můžete přiřadit existující objekt služby Azure Active Directory do registru nebo vytvořit a přiřadit nový. Alternativně můžete povolit v registru Správce uživatelský účet. Pokyny najdete v dalších částech tohoto článku. Další informace o přístupu k registru najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
Spuštěním příkazu `New-AzureRMContainerRegistry` vytvořte registr kontejnerů.

> [!TIP]
> Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice. V tomto příkladu je název registru `MyRegistry`, ale nahraďte jej vlastním jedinečným názvem.
>
>

Následující příkaz pomocí minimálního počtu parametrů vytvoří registr kontejnerů `MyRegistry` ve skupině prostředků `MyResourceGroup` v umístění Střed USA – jih:

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* Parametr `-StorageAccountName` je volitelný. Pokud není zadán, vytvoří se v zadané skupině prostředků účet úložiště s názvem, který se skládá z názvu registru a časového razítka.

## <a name="assign-a-service-principal"></a>Přiřazení instančního objektu
Použít příkazy prostředí PowerShell přiřadit Azure Active Directory [instanční objekt](../azure-resource-manager/resource-group-authenticate-service-principal.md) do registru. Instančnímu objektu v těchto příkladech je přiřazena role vlastníka, ale pokud chcete, můžete mu přiřadit [jiné role](../active-directory/role-based-access-control-configure.md).

### <a name="create-a-service-principal"></a>Vytvoření instančního objektu
V následující příkaz je vytvořen nový instanční objekt. V parametru `-Password` zadejte silné heslo.

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a>Přiřadit nový nebo existující instanční objekt
Nový nebo existující službu objektu zabezpečení můžete přiřadit k registru. Pokud chcete přiřadit vlastníka role přístup k registru, spusťte příkaz podobně jako v následujícím příkladu:

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-to-the-registry-with-the-service-principal"></a>Přihlaste se k registru s hlavní službou
Po přiřazení objektu služby v registru, můžete přihlásit pomocí následujícího příkazu:

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a>Správa přihlašovacích údajů správce
Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý. Následující příklady ukazují příkazy prostředí PowerShell ke správě přihlašovacích údajů správce pro vašeho kontejneru registru.

### <a name="obtain-admin-user-credentials"></a>Získání přihlašovacích údajů uživatele s právy pro správu
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Povolení uživatele s právy pro správu pro existující registr
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Zakázání uživatele s právy pro správu pro existující registr
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a>Další kroky
* [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)
