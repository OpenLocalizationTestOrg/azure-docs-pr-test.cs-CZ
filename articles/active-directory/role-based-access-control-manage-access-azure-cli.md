---
title: "aaaManage na základě rolí řízení přístupu (RBAC) pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak rozhraní toomanage na základě rolí řízení přístupu (RBAC) pomocí příkazového řádku Azure hello seznam rolí a rolí akce a přiřazení rolí obory toohello předplatné a aplikace."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 438418e5f6ee9b98908c9c264d516eb722a4e26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-azure-command-line-interface"></a>Správa řízení přístupu na základě rolí pomocí rozhraní příkazového řádku Azure hello
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


Řízení přístupu na základě Role (RBAC) můžete použít v hello portál Azure a rozhraní API služby Azure Resource Manager toomanage přístup tooyour předplatného a prostředky na velice přesné úrovni. Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role toothem v určitém rozsahu.

Než budete moci použít hello rozhraní příkazového řádku Azure (CLI) toomanage RBAC, musíte mít hello následující požadavky:

* Azure CLI verze 0.8.8 nebo novější. tooinstall hello nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).
* Azure Resource Manager v rozhraní příkazového řádku Azure. Přejděte příliš[hello pomocí rozhraní příkazového řádku Azure s hello Resource Manager](../xplat-cli-azure-resource-manager.md) další podrobnosti.

## <a name="list-roles"></a>Seznam rolí
### <a name="list-all-available-roles"></a>Zobrazí seznam všech dostupných rolí
toolist všechny dostupné role, použijte:

        azure role list

Hello následující příklad ukazuje hello seznam *všechny dostupné role*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Seznam akcí role
Akce hello toolist role, použijte:

    azure role show "<role name>"

Hello následující příklad ukazuje hello akce hello *Přispěvatel* a *Přispěvatel virtuálních počítačů* role.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure příkazového řádku - azure role zobrazit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Přístup k seznamu
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Přiřazení rolí seznamu efektivní ve skupině prostředků.
přiřazení rolí hello toolist které existují ve skupině prostředků, použijte:

    azure role assignment list --resource-group <resource group name>

Hello následující příklad ukazuje hello přiřazení rolí v hello *pharma. prodej projecforcast* skupiny.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu podle skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Seznam přiřazení role pro uživatele
přiřazení rolí hello toolist pro konkrétního uživatele a přiřazení hello, které jsou přiřazeny skupiny tooa uživatelů, použijte:

    azure role assignment list --signInName <user email>

Můžete také zjistit přiřazení rolí, které se dědí z skupin změnou hello příkaz:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Hello následující příklad ukazuje hello přiřazení role, kterým je uděleno oprávnění toohello  *sameert@aaddemo.com*  uživatele. To zahrnuje role, které jsou přiřazeny přímo toohello uživatele a role, které se dědí z skupin.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure příkazového řádku - azure role přiřazení seznamu uživatelem – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Udělení přístupu
toogrant přístup po zjištění hello role, které chcete tooassign, použijte:

    azure role assignment create

### <a name="assign-a-role-toogroup-at-hello-subscription-scope"></a>Přiřazení role toogroup v oboru předplatné hello
tooassign skupinu tooa role v hello obor předplatného, použijte:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Hello následující příklad přiřadí hello *čtečky* role příliš*Jana Koch Team* v hello *předplatné* oboru.

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a>Přiřadit k aplikaci tooan role v oboru předplatné hello
tooassign aplikace tooan role za hello obor předplatného, použijte:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Hello následující příklad uděluje hello *Přispěvatel* role tooan *Azure AD* aplikace na hello vybrané předplatné.

 ![Příkaz Azure RBAC - přiřazení role v azure vytvořit aplikací](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a>Přiřazení role uživatele tooa v oboru skupiny prostředků hello
tooassign tooa role uživatele v oboru skupiny hello prostředků, použijte:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Hello následující příklad uděluje hello *Přispěvatel virtuálních počítačů* role příliš *samert@aaddemo.com*  uživatel na hello *Pharma. prodej ProjectForcast* oboru skupiny prostředků.

![Příkaz Azure RBAC - přiřazení role v azure vytvořit uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a>Přiřadit role tooa skupinu v oboru prostředků hello
tooassign skupinu tooa role v oboru hello prostředků, použijte:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Hello následující příklad uděluje hello *Přispěvatel virtuálních počítačů* role tooan *Azure AD* na *podsítě*.

![Příkaz Azure RBAC - přiřazení role v azure vytvořit skupiny – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Odebrání přístupu
tooremove přiřazení role, použijte:

    azure role assignment delete --objectId <object id toofrom which tooremove role> --roleName "<role name>"

Hello následující příklad odebere hello *Přispěvatel virtuálních počítačů* přiřazení role z hello  *sammert@aaddemo.com*  uživatele na hello *Pharma. prodej ProjectForcast* Skupina prostředků.
přiřazení role hello Hello příklad pak odebere ze skupiny v předplatném hello.

![RBAC Azure příkazového řádku - azure role přiřazení odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
toocreate vlastní roli, použijte:

    azure role create --inputfile <file path>

Hello následující příklad vytvoří vlastní role s názvem *virtuální počítač operátor*. Tato vlastní role uděluje přístup tooall číst operace *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* prostředků poskytovatelů a uděluje přístup toostart, restartování a monitorování virtuálních počítačů. Tuto vlastní roli můžete použít ve dvou předplatných. Tento příklad používá soubor JSON jako vstup.

![JSON – definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure příkazového řádku - azure role vytvořit – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Upravit vlastní roli
toomodify vlastní role, nejprve použijte hello `azure role show` příkaz tooretrieve definice role. Druhý zkontrolujte soubor definice role toohello požadované změny hello. Nakonec použijte `azure role set` toosave hello změnil definici role.

    azure role set --inputfile <file path>

Hello následující příklad přidá hello *Microsoft.Insights/diagnosticSettings/* operace toohello **akce**a předplatnému Azure toohello **AssignableScopes**vlastní role virtuálního počítače operátor hello.

![JSON - upravit definice vlastních rolí – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Snímek obrazovky příkazového řádku - azure roli set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Odstranit vlastní roli
toodelete vlastní role, nejprve použijte hello `azure role show` příkaz toodetermine hello **ID** hello role. Poté použijte hello `azure role delete` příkaz toodelete hello role zadáním hello **ID**.

Hello následující příklad odebere hello *virtuální počítač operátor* vlastní role.

![RBAC Azure příkazového řádku - azure role odstranění – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Vlastní role seznamu
toolist hello role, které jsou k dispozici pro přiřazení v oboru, které používají hello `azure role list` příkaz.

Hello následující příkaz vypíše všechny role, které jsou k dispozici pro přiřazení v hello vybrané předplatné.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure příkazového řádku - azure role seznamu – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

V následujícím příkladu hello, hello *virtuální počítač operátor* vlastní role není k dispozici v hello *Production4* předplatné vzhledem k tomu, že toto předplatné není v hello  **AssignableScopes** hello role.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure příkazového řádku - azure role seznam pro vlastní role – snímek obrazovky](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Další kroky
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

