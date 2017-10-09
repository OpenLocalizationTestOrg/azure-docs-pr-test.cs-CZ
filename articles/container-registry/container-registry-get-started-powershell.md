---
title: "aaaAzure kontejneru registru úložiště | Microsoft Docs"
description: "Jak toouse registru Azure kontejner úložiště pro Docker bitové kopie"
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
ms.openlocfilehash: 448fb812f537c9502041ce5fb372b0681a9dac4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-powershell"></a>Vytvořit privátní registru kontejner Docker pomocí hello prostředí Azure PowerShell
Pomocí příkazů v [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate registru kontejneru a spravovat nastavení z počítače se systémem Windows. Můžete také vytvořit a spravovat pomocí hello registrech kontejneru [portál Azure](container-registry-get-started-portal.md), hello [rozhraní příkazového řádku Azure](container-registry-get-started-azure-cli.md), nebo prostřednictvím kódu programu hello kontejneru registru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Pozadí a koncepty najdete v tématu [hello – přehled](container-registry-intro.md)
* Úplný seznam podporovaných rutin najdete v tématu [rutiny správy registru kontejner Azure](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).


## <a name="prerequisites"></a>Požadavky
* **Prostředí Azure PowerShell**: tooinstall a začít pracovat s Azure PowerShell najdete v tématu hello [pokyny k instalaci](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). Přihlaste se spuštěním tooyour předplatného Azure `Login-AzureRMAccount`. Další informace najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).
* **Skupina prostředků:** Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků. Zkontrolujte, zda je skupina prostředků hello v umístění, kde je hello registru kontejneru služby [k dispozici](https://azure.microsoft.com/regions/services/). toocreate skupiny prostředků pomocí Azure PowerShell, najdete v části [hello referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).
* **Účet úložiště** (volitelné): vytvořte standardní Azure [účet úložiště](../storage/common/storage-introduction.md) tooback hello kontejneru registru hello stejné umístění. Pokud nezadáte účet úložiště při vytváření registru s `New-AzureRMContainerRegistry`, hello příkaz vytvoří za vás. toocreate úložiště účtu pomocí prostředí PowerShell najdete v tématu [hello referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount). Storage úrovně Premium se v tuto chvíli nepodporuje.
* **Instanční objekt** (volitelné): Když vytvoříte soubor registru pomocí prostředí PowerShell, ve výchozím nastavení není nastaven pro přístup. Podle potřeby můžete přiřadit existujícího registru hlavní tooa služby Azure Active Directory nebo vytvořte a přiřaďte nové. Alternativně můžete povolit hello registru Správce uživatelský účet. Najdete v části hello později v tomto článku. Další informace o přístup k registru najdete v tématu [ověřit s hello kontejneru registru](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů
Spustit hello `New-AzureRMContainerRegistry` příkaz toocreate registru kontejneru.

> [!TIP]
> Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice. Název registru Hello v příkladech hello je `MyRegistry`, ale nahraďte vlastní jedinečný název.
>
>

Dobrý den, následující příkaz používá hello minimální parametry toocreate kontejneru registru `MyRegistry` ve skupině prostředků hello `MyResourceGroup` v hello jihu USA umístění:

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* Parametr `-StorageAccountName` je volitelný. Pokud není zadán, s názvem, který se skládá z název registru hello je vytvoření účtu úložiště a časovým razítkem v hello zadat skupinu prostředků.

## <a name="assign-a-service-principal"></a>Přiřazení instančního objektu
Použít tooassign příkazy prostředí PowerShell Azure Active Directory [instanční objekt](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registru. Hello instančního objektu v těchto příkladech je přiřazena hello vlastníka role, ale můžete přiřadit [jiné role](../active-directory/role-based-access-control-configure.md) podle potřeby.

### <a name="create-a-service-principal"></a>Vytvoření instančního objektu
V hello následující příkaz je vytvořen nový instanční objekt. Zadejte silné heslo s hello `-Password` parametr.

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a>Přiřadit nový nebo existující instanční objekt
Můžete přiřadit nového nebo existujícího objektu tooa registru služby. tooassign jeho vlastníka role přístup toohello registru, spusťte příkaz podobný toohello následující ukázka:

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-toohello-registry-with-hello-service-principal"></a>Přihlaste se toohello registr s využitím hello instančního objektu
Po přiřazení hello služby hlavní toohello registru, můžete přihlásit pomocí hello následující příkaz:

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a>Správa přihlašovacích údajů správce
Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý. Hello následující příklady ukazují příkazy prostředí PowerShell přihlašovací údaje správce hello toomanage pro vaše registru kontejneru.

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
* [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md)
