---
title: "vlastní role aaaCreate pro Azure RBAC | Microsoft Docs"
description: "Zjistěte, jak toodefine vlastní role pomocí řízení přístupu pro přesnější správu identit ve vašem předplatném Azure."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 60df12632ef6c086d5feeb1809196d7c4ee5e021
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a>Vytvořit vlastní role pro řízení přístupu
Vytvořte vlastní roli v řízení řízení přístupu (RBAC), pokud žádná z předdefinovaných rolí hello podle svých potřeb konkrétní přístup. Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](role-based-access-control-manage-access-powershell.md), [rozhraní příkazového řádku Azure](role-based-access-control-manage-access-azure-cli.md) (CLI) a hello [REST API](role-based-access-control-manage-access-rest.md). Stejně jako předdefinované role můžete přiřadit toousers vlastní role, skupiny a aplikace na předplatné, skupinu prostředků a prostředků obory. Vlastní role jsou uloženy v klient služby Azure AD a mohlo sdílet víc předplatných.

Každý klient můžete vytvořit až too2000 vlastní role. 

Hello následující příklad ukazuje vlastní role pro monitorování a restartování virtuálního počítače:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Akce
Hello **akce** vlastnost vlastní role určuje hello Azure operations toowhich hello role uděluje přístup. Jedná se o kolekci operaci řetězců, které identifikují zabezpečitelné operations poskytovatelů prostředků Azure. Operace řetězce podle hello formát `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Operace řetězce, které obsahují zástupné znaky (\*) udělit přístup tooall operace, které odpovídají řetězci operace hello. Například:

* `*/read`uděluje přístup k tooread operací pro všechny typy prostředků všech poskytovatelů prostředků Azure.
* `Microsoft.Compute/*`uděluje přístup k tooall operací pro všechny typy prostředků v hello zprostředkovateli prostředků Microsoft.Compute.
* `Microsoft.Network/*/read`uděluje přístup k tooread operací pro všechny typy prostředků v poskytovatel prostředků Microsoft.Network hello Azure.
* `Microsoft.Compute/virtualMachines/*`uděluje přístup k tooall operace virtuálních počítačů a jeho podřízené typy prostředků.
* `Microsoft.Web/sites/restart/Action`uděluje přístup k webům toorestart.

Použití `Get-AzureRmProviderOperation` (v prostředí PowerShell) nebo `azure provider operations show` (v Azure CLI) toolist operations poskytovatelů prostředků Azure. Můžete také používat tyto příkazy tooverify, zda je řetězec v operaci platný a tooexpand zástupný znak operaci řetězce.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Snímek obrazovky prostředí PowerShell - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI – snímek obrazovky - azure zprostředkovatele operations zobrazit "Microsoft.Compute/virtualMachines/\*/action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Použití hello **NotActions** vlastnost Pokud hello sadu operací, které chcete tooallow je snadno definovanou s výjimkou operací s omezeným přístupem. Hello udělení přístupu podle vlastní role se počítá odečtením hello **NotActions** operací z hello **akce** operace.

> [!NOTE]
> Pokud je uživatel přiřazenou roli, která nezahrnuje operace v **NotActions**a je mu přiřazená druhá role, která uděluje přístup toohello stejné operace, hello uživatele je povoleno tooperform tuto operaci. **NotActions** není odepření pravidlo – je jednoduše to pohodlný způsob toocreate sadu povolených operací při konkrétních operací potřebovat toobe vyloučeny.
>
>

## <a name="assignablescopes"></a>AssignableScopes
Hello **AssignableScopes** vlastnost hello vlastní role určuje hello obory (odběry, skupiny prostředků nebo prostředky) v rámci které hello vlastní role je k dispozici pro přiřazení. Můžete vytvořit vlastní role hello k dispozici pro přiřazení v pouze hello předplatných nebo skupiny prostředků, které vyžadují a ne zbytečné soubory uživatele prostředí pro zbytek hello předplatná hello nebo skupiny prostředků.

Příklady platný přiřaditelnými obory:

* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - zpřístupní hello role pro přiřazení do dvou předplatných.
* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - zpřístupní hello role pro přiřazení v rámci jednoho předplatného.
* "/ odběry nebo c276fc76-9cd4-44c9-99a7-4fd71546436e/Skupinyprostředků/síťových" - Díky hello role, které jsou k dispozici pro přiřazení pouze ve skupině prostředků sítě hello.

> [!NOTE]
> Musíte použít aspoň jeden předplatné, skupinu prostředků nebo ID prostředku.
>
>

## <a name="custom-roles-access-control"></a>Vlastní role řízení přístupu
Hello **AssignableScopes** vlastnost hello vlastní role také určuje, kdo může zobrazit, upravit a odstranit roli hello.

* Kdo mohou vytvářet vlastní role?
    Vlastníci (a správcům přístup uživatele) předplatných, skupiny prostředků a prostředků můžete vytvořit vlastní role pro použití v tyto obory.
    Hello uživatel vytváření hello role musí mít tooperform toobe `Microsoft.Authorization/roleDefinition/write` operace na všechny hello **AssignableScopes** hello role.
* Kdo může upravit vlastní roli?
    Vlastníci (a správcům přístup uživatele) předplatných, skupiny prostředků a prostředků můžete upravit vlastní role v tyto obory. Uživatelé potřebují hello možné tooperform toobe `Microsoft.Authorization/roleDefinition/write` operace na všechny hello **AssignableScopes** vlastní role.
* Kdo může zobrazit vlastní role?
    Všechny předdefinované role v Azure RBAC povolí zobrazení rolí, které jsou k dispozici pro přiřazení. Uživatelé, kteří mohou provádět hello `Microsoft.Authorization/roleDefinition/read` operace v oboru můžete zobrazit role RBAC hello, které jsou k dispozici pro přiřazení v tomto oboru.

## <a name="see-also"></a>Viz také
* [Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC v hello portálu Azure.
* Zjistěte, jak přistupovat k toomanage se:
  * [PowerShell](role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
  * [REST API](role-based-access-control-manage-access-rest.md)
* [Předdefinované role](role-based-access-built-in-roles.md): získání podrobností o hello rolí, které jsou v RBAC standardní.
