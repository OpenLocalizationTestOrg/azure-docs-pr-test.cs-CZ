---
title: "Ochrana zóny DNS a záznamy | Microsoft Docs"
description: "Jak chránit zóny DNS a sady záznamů v Microsoft Azure DNS."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 190e69eb-e820-4fc8-8e9a-baaf0b3fb74a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2016
ms.author: jonatul
ms.openlocfilehash: 0b7040d6273b3a6b85cd55850d596807226b87fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-protect-dns-zones-and-records"></a><span data-ttu-id="4fbaf-103">Jak chránit zóny DNS a záznamy</span><span class="sxs-lookup"><span data-stu-id="4fbaf-103">How to protect DNS zones and records</span></span>

<span data-ttu-id="4fbaf-104">Zóny DNS a záznamy jsou důležité prostředky.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-104">DNS zones and records are critical resources.</span></span> <span data-ttu-id="4fbaf-105">Odstraňování zónu DNS, nebo jenom jeden záznam DNS, může mít za následek výpadku celkový služeb.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-105">Deleting a DNS zone or even just a single DNS record can result in a total service outage.</span></span>  <span data-ttu-id="4fbaf-106">Proto je důležité, aby kritické zóny DNS a záznamy chráněná před neoprávněným nebo náhodné změny.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-106">It is therefore important that critical DNS zones and records are protected against unauthorized or accidental changes.</span></span>

<span data-ttu-id="4fbaf-107">Tento článek vysvětluje, jak Azure DNS umožňuje chránit zóny DNS a záznamy proti tyto změny.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-107">This article explains how Azure DNS enables you to protect your DNS zones and records against such changes.</span></span>  <span data-ttu-id="4fbaf-108">Jsme použít dvě funkce efektivní zabezpečení poskytované pomocí Správce prostředků Azure: [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) a [uzamčení prostředků](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-108">We apply two powerful security features provided by Azure Resource Manager: [role-based access control](../active-directory/role-based-access-control-what-is.md) and [resource locks](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="4fbaf-109">Řízení přístupu na základě role</span><span class="sxs-lookup"><span data-stu-id="4fbaf-109">Role-based access control</span></span>

<span data-ttu-id="4fbaf-110">Azure na základě rolí řízení přístupu (RBAC) umožňuje přesnou správu přístupu pro Azure uživatele, skupiny a prostředky.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-110">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure users, groups, and resources.</span></span> <span data-ttu-id="4fbaf-111">Pomocí RBAC, můžete udělit přesněji úroveň přístupu, aby uživatelé potřebují k provádění svých úloh.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-111">Using RBAC, you can grant precisely the amount of access that users need to perform their jobs.</span></span> <span data-ttu-id="4fbaf-112">Další informace o tom, jak AZURE pomůže spravovat přístup, najdete v části [co je řízení přístupu na základě Role](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-112">For more information about how RBAC helps you manage access, see [What is Role-Based Access Control](../active-directory/role-based-access-control-what-is.md).</span></span>

### <a name="the-dns-zone-contributor-role"></a><span data-ttu-id="4fbaf-113">Role, Přispěvatel zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-113">The 'DNS Zone Contributor' role</span></span>

<span data-ttu-id="4fbaf-114">Role, Přispěvatel zóny DNS, je předdefinovaná role, poskytovaný platformou Azure pro správu prostředků služby DNS.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-114">The 'DNS Zone Contributor' role is a built-in role provided by Azure for managing DNS resources.</span></span>  <span data-ttu-id="4fbaf-115">Přiřazení oprávnění přispěvatele zóny DNS na uživatele nebo skupinu umožňuje této skupiny pro správu prostředků DNS, ale ne prostředky žádným jiným typem.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-115">Assigning DNS Zone Contributor permissions to a user or group enables that group to manage DNS resources, but not resources of any other type.</span></span>

<span data-ttu-id="4fbaf-116">Předpokládejme například, že skupina prostředků 'myzones' obsahuje pět zóny pro společnost Contoso.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-116">For example, suppose the resource group 'myzones' contains five zones for Contoso Corporation.</span></span> <span data-ttu-id="4fbaf-117">Udělení Správce DNS, Přispěvatel zóny DNS, oprávnění k této skupině zdrojů, umožňuje plnou kontrolu nad tyto zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-117">Granting the DNS administrator 'DNS Zone Contributor' permissions to that resource group, enables full control over those DNS zones.</span></span> <span data-ttu-id="4fbaf-118">Také se vyhnete, udělení nadbytečná oprávnění, například Správce DNS nelze vytvořit nebo zastavit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-118">It also avoids granting unnecessary permissions, for example the DNS administrator cannot create or stop Virtual Machines.</span></span>

<span data-ttu-id="4fbaf-119">Nejjednodušší způsob, jak přiřadit oprávnění RBAC je [prostřednictvím portálu Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-119">The simplest way to assign RBAC permissions is [via the Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>  <span data-ttu-id="4fbaf-120">Otevře se okno, řízení přístupu (IAM)' pro skupinu prostředků, pak klikněte na tlačítko "Přidat", pak vyberte roli, Přispěvatel zóny DNS a vyberte požadované uživatele nebo skupiny, které chcete udělit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-120">Open the 'Access control (IAM)' blade for the resource group, then click 'Add', then select the 'DNS Zone Contributor' role and select the required users or groups to grant permissions.</span></span>

![Úrovni skupiny prostředků RBAC prostřednictvím portálu Azure](./media/dns-protect-zones-recordsets/rbac1.png)

<span data-ttu-id="4fbaf-122">Oprávnění může být také [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="4fbaf-122">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="4fbaf-123">Také je ekvivalentní příkaz [prostřednictvím rozhraní příkazového řádku Azure k dispozici](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="4fbaf-123">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a><span data-ttu-id="4fbaf-124">Úroveň zóny RBAC</span><span class="sxs-lookup"><span data-stu-id="4fbaf-124">Zone level RBAC</span></span>

<span data-ttu-id="4fbaf-125">Azure RBAC pravidla lze použít pro předplatné, skupinu prostředků nebo pro jednotlivé zdroje.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-125">Azure RBAC rules can be applied to a subscription, a resource group or to an individual resource.</span></span> <span data-ttu-id="4fbaf-126">V případě Azure DNS může být tento prostředek jednotlivé zóny DNS, nebo i jednotlivé sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-126">In the case of Azure DNS, that resource can be an individual DNS zone, or even an individual record set.</span></span>

<span data-ttu-id="4fbaf-127">Předpokládejme například, že skupiny prostředků, myzones' obsahuje zóny contoso.com a subzone 'customers.contoso.com, ve kterém jsou vytvořeny záznamy CNAME pro každou účtu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-127">For example, suppose the resource group 'myzones' contains the zone 'contoso.com' and a subzone 'customers.contoso.com' in which CNAME records are created for each customer account.</span></span>  <span data-ttu-id="4fbaf-128">Účet použitý ke správě tyto záznamy CNAME by se měla přiřadit oprávnění k vytváření záznamů v zóně 'customers.contoso.com', by neměl mít přístup k jiné zóně.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-128">The account used to manage these CNAME records should be assigned permissions to create records in the 'customers.contoso.com' zone only, it should not have access to the other zones.</span></span>

<span data-ttu-id="4fbaf-129">Prostřednictvím portálu Azure můžete udělit oprávnění RBAC na úrovni zóny.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-129">Zone-level RBAC permissions can be granted via the Azure portal.</span></span>  <span data-ttu-id="4fbaf-130">Otevře se okno, řízení přístupu (IAM)"pro zónu, pak klikněte na tlačítko"Přidat", pak vyberte roli, Přispěvatel zóny DNS a vyberte požadované uživatele nebo skupiny, které chcete udělit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-130">Open the 'Access control (IAM)' blade for the zone, then click 'Add', then select the 'DNS Zone Contributor' role and select the required users or groups to grant permissions.</span></span>

![Zóna DNS úrovně RBAC prostřednictvím portálu Azure](./media/dns-protect-zones-recordsets/rbac2.png)

<span data-ttu-id="4fbaf-132">Oprávnění může být také [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="4fbaf-132">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

<span data-ttu-id="4fbaf-133">Také je ekvivalentní příkaz [prostřednictvím rozhraní příkazového řádku Azure k dispozici](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="4fbaf-133">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a><span data-ttu-id="4fbaf-134">Úroveň RBAC sady záznamů</span><span class="sxs-lookup"><span data-stu-id="4fbaf-134">Record set level RBAC</span></span>

<span data-ttu-id="4fbaf-135">Jeden krok jsme může pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-135">We can go one step further.</span></span> <span data-ttu-id="4fbaf-136">Vezměte v úvahu e-mailu správce pro společnosti Contoso Corporation, který potřebuje přístup k MX a TXT záznamů na vrcholu zónu "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="4fbaf-136">Consider the mail administrator for Contoso Corporation, who needs access to the MX and TXT records at the apex of the 'contoso.com' zone.</span></span>  <span data-ttu-id="4fbaf-137">Jana nepotřebuje přístup k jiné MX nebo TXT záznamů, nebo všechny záznamy o jiný typ.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-137">She doesn't need access to any other MX or TXT records, or to any records of any other type.</span></span>  <span data-ttu-id="4fbaf-138">Azure DNS umožňuje přiřadit oprávnění na úrovni sady záznamů, přesněji na záznamy, které potřebuje přístup k e-mailu správce.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-138">Azure DNS allows you to assign permissions at the record set level, to precisely the records that the mail administrator needs access to.</span></span>  <span data-ttu-id="4fbaf-139">E-mailu správce má oprávnění přesně řídit, která potřebuje a nemůže provést další změny.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-139">The mail administrator is granted precisely the control she needs, and is unable to make any other changes.</span></span>

<span data-ttu-id="4fbaf-140">Oprávnění na úrovni RBAC sadu záznamů, můžete nakonfigurovat prostřednictvím portálu Azure, pomocí tlačítka 'Uživatele' v okně Sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-140">Record-set level RBAC permissions can be configured via the Azure portal, using the 'Users' button in the record set blade:</span></span>

![Sady záznamů úroveň RBAC prostřednictvím portálu Azure](./media/dns-protect-zones-recordsets/rbac3.png)

<span data-ttu-id="4fbaf-142">Může být také sadu záznamů oprávnění na úrovni RBAC [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="4fbaf-142">Record-set level RBAC permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

<span data-ttu-id="4fbaf-143">Také je ekvivalentní příkaz [prostřednictvím rozhraní příkazového řádku Azure k dispozici](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="4fbaf-143">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a><span data-ttu-id="4fbaf-144">Vlastní role</span><span class="sxs-lookup"><span data-stu-id="4fbaf-144">Custom roles</span></span>

<span data-ttu-id="4fbaf-145">Předdefinovaná role, Přispěvatel zóny DNS, umožňuje plnou kontrolu nad prostředků DNS.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-145">The built-in 'DNS Zone Contributor' role enables full control over a DNS resource.</span></span> <span data-ttu-id="4fbaf-146">Je také možné vytvořit vlastní zákazník Azure role, které zajišťují řízení i citlivější.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-146">It is also possible to build your own customer Azure roles, to provide even finer-grained control.</span></span>

<span data-ttu-id="4fbaf-147">Zvažte znovu příklad, ve kterém se vytvoří záznam CNAME v zóně 'customers.contoso.com' u každého účtu zákazníka Contoso Corporation.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-147">Consider again the example in which a CNAME record in the zone 'customers.contoso.com' is created for each Contoso Corporation customer account.</span></span>  <span data-ttu-id="4fbaf-148">Účet použitý ke správě těchto záznamů CNAME musí udělit oprávnění ke správě pouze záznamy CNAME.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-148">The account used to manage these CNAMEs should be granted permission to manage CNAME records only.</span></span>  <span data-ttu-id="4fbaf-149">Pak se nepodařilo upravit záznamy o dalších typů (jako je například změna záznamů MX) nebo provádět operace na úrovni zóny například odstranit zónu.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-149">It is then unable to modify records of other types (such as changing MX records) or perform zone-level operations such as zone delete.</span></span>

<span data-ttu-id="4fbaf-150">Následující příklad ukazuje definice vlastních rolí pro správu pouze záznamy CNAME:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-150">The following example shows a custom role definition for managing CNAME records only:</span></span>

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

<span data-ttu-id="4fbaf-151">Vlastnost akce definuje následující DNS konkrétní oprávnění:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-151">The Actions property defines the following DNS-specific permissions:</span></span>

* <span data-ttu-id="4fbaf-152">`Microsoft.Network/dnsZones/CNAME/*`uděluje plnou kontrolu nad záznamy CNAME</span><span class="sxs-lookup"><span data-stu-id="4fbaf-152">`Microsoft.Network/dnsZones/CNAME/*` grants full control over CNAME records</span></span>
* <span data-ttu-id="4fbaf-153">`Microsoft.Network/dnsZones/read`uděluje oprávnění ke čtení zóny DNS, ale není o jejich úpravu, umožňuje najdete v části zónu, ve kterém se vytváří CNAME.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-153">`Microsoft.Network/dnsZones/read` grants permission to read DNS zones, but not to modify them, enabling you to see the zone in which the CNAME is being created.</span></span>

<span data-ttu-id="4fbaf-154">Zbývající akce, které jsou zkopírovány ze [předdefinovaná role Přispěvatel zóny DNS](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-154">The remaining Actions are copied from the [DNS Zone Contributor built-in role](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span></span>

> [!NOTE]
> <span data-ttu-id="4fbaf-155">Aby se zabránilo odstranění sady záznamů, zatímco stále což jim umožní aktualizovat není efektivní řízení pomocí vlastní role RBAC.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-155">Using a custom RBAC role to prevent deleting record sets while still allowing them to be updated is not an effective control.</span></span> <span data-ttu-id="4fbaf-156">Sady záznamů zabrání odstraňuje, ale nezabrání je upravovat.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-156">It prevents record sets from being deleted, but it does not prevent them from being modified.</span></span>  <span data-ttu-id="4fbaf-157">Povolené změny zahrnují přidávání a odebírání záznamů ze sady záznamů, včetně odebrat všechny záznamy chcete nechat 'prázdná' sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-157">Permitted modifications include adding and removing records from the record set, including removing all records to leave an 'empty' record set.</span></span> <span data-ttu-id="4fbaf-158">Tato akce nemá stejný účinek jako odstranění sady z hlediska rozlišení DNS záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-158">This has the same effect as deleting the record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="4fbaf-159">Definice vlastní role nemůže být definovaný aktuálně prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-159">Custom role definitions cannot currently be defined via the Azure portal.</span></span> <span data-ttu-id="4fbaf-160">Pomocí Azure Powershellu lze vytvořit vlastní role založená na této definici role:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-160">A custom role based on this role definition can be created using Azure PowerShell:</span></span>

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

<span data-ttu-id="4fbaf-161">Také možné vytvářet prostřednictvím rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-161">It can also be created via the Azure CLI:</span></span>

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

<span data-ttu-id="4fbaf-162">Role pak lze přiřadit stejným způsobem jako pro předdefinované role, jak je popsáno výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-162">The role can then be assigned in the same way as built-in roles, as described earlier in this article.</span></span>

<span data-ttu-id="4fbaf-163">Další informace o tom, jak vytvářet, spravovat a přiřadit vlastní role, naleznete v části [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-163">For more information on how to create, manage, and assign custom roles, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="resource-locks"></a><span data-ttu-id="4fbaf-164">Uzamčení prostředků</span><span class="sxs-lookup"><span data-stu-id="4fbaf-164">Resource locks</span></span>

<span data-ttu-id="4fbaf-165">Kromě RBAC Azure Resource Manager podporuje jiný typ řízení zabezpečení, a to možnost prostředky 'lock'.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-165">In addition to RBAC, Azure Resource Manager supports another type of security control, namely the ability to 'lock' resources.</span></span> <span data-ttu-id="4fbaf-166">Kde RBAC pravidla umožňují řídit akce konkrétní uživatele a skupiny, uzamčení prostředků se použijí k prostředku a jsou platné ve všech uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-166">Where RBAC rules allow you to control the actions of specific users and groups, resource locks are applied to the resource, and are effective across all users and roles.</span></span> <span data-ttu-id="4fbaf-167">Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-167">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

<span data-ttu-id="4fbaf-168">Existují dva typy prostředků zámku: **DoNotDelete** a **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-168">There are two types of resource lock: **DoNotDelete** and **ReadOnly**.</span></span> <span data-ttu-id="4fbaf-169">Ty lze použít buď na zónu DNS, nebo na sadu záznamů jednotlivých.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-169">These can be applied either to a DNS zone, or to an individual record set.</span></span>  <span data-ttu-id="4fbaf-170">Následující části popisují několik běžné scénáře a postupy, které je podporují pomocí uzamčení prostředků.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-170">The following sections describe several common scenarios, and how to support them using resource locks.</span></span>

### <a name="protecting-against-all-changes"></a><span data-ttu-id="4fbaf-171">Ochrana proti všechny změny</span><span class="sxs-lookup"><span data-stu-id="4fbaf-171">Protecting against all changes</span></span>

<span data-ttu-id="4fbaf-172">Pokud chcete zabránit změny prováděné, použije zámek jen pro čtení do zóny.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-172">To prevent any changes being made, apply a ReadOnly lock to the zone.</span></span>  <span data-ttu-id="4fbaf-173">To brání tomu, aby nové sady záznamů se vytvořil a existující sady záznamů upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-173">This prevents new record sets from being created, and existing record sets from being modified or deleted.</span></span>

<span data-ttu-id="4fbaf-174">Zónu prostředků s úrovní zámky lze vytvořit prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-174">Zone level resource locks can be created via the Azure portal.</span></span>  <span data-ttu-id="4fbaf-175">V okně zóny DNS, klikněte na tlačítko 'Zámky' pak přidejte:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-175">From the DNS zone blade, click 'Locks', then 'Add':</span></span>

![Zónu prostředků s úrovní zámky prostřednictvím portálu Azure](./media/dns-protect-zones-recordsets/locks1.png)

<span data-ttu-id="4fbaf-177">Úroveň zóny prostředku, který zámky. můžete také vytvořit prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-177">Zone-level resource locks can also be created via Azure PowerShell:</span></span>

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

<span data-ttu-id="4fbaf-178">Konfigurace prostředků Azure zámků není aktuálně podporován prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-178">Configuring Azure resource locks is not currently supported via the Azure CLI.</span></span>

### <a name="protecting-individual-records"></a><span data-ttu-id="4fbaf-179">Ochrana jednotlivých záznamů</span><span class="sxs-lookup"><span data-stu-id="4fbaf-179">Protecting individual records</span></span>

<span data-ttu-id="4fbaf-180">Aby se zabránilo existujícího záznamu DNS nastavit pro úpravy, použije zámek jen pro čtení do sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-180">To prevent an existing DNS record set against modification, apply a ReadOnly lock to the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="4fbaf-181">Použití DoNotDelete zámku na sadu záznamů není efektivní řízení.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-181">Applying a DoNotDelete lock to a record set is not an effective control.</span></span> <span data-ttu-id="4fbaf-182">Zabraňuje před odstraněním sady záznamů, ale nezabrání ho upravovat.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-182">It prevents the record set from being deleted, but it does not prevent it from being modified.</span></span>  <span data-ttu-id="4fbaf-183">Povolené změny zahrnují přidávání a odebírání záznamů ze sady záznamů, včetně odebrat všechny záznamy chcete nechat 'prázdná' sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-183">Permitted modifications include adding and removing records from the record set, including removing all records to leave an 'empty' record set.</span></span> <span data-ttu-id="4fbaf-184">Tato akce nemá stejný účinek jako odstranění sady z hlediska rozlišení DNS záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-184">This has the same effect as deleting the record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="4fbaf-185">Sady záznamů prostředků s úrovní zámky momentálně můžete být pouze nakonfigurovat pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-185">Record set level resource locks can currently only be configured using Azure PowerShell.</span></span>  <span data-ttu-id="4fbaf-186">Nejsou podporovány v portálu Azure nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-186">They are not supported in the Azure portal or Azure CLI.</span></span>

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a><span data-ttu-id="4fbaf-187">Ochrana proti odstranění zóny</span><span class="sxs-lookup"><span data-stu-id="4fbaf-187">Protecting against zone deletion</span></span>

<span data-ttu-id="4fbaf-188">Při odstranění zóny v Azure DNS se také odstraní všechny sady záznamů v zóně.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-188">When a zone is deleted in Azure DNS, all record sets in the zone are also deleted.</span></span>  <span data-ttu-id="4fbaf-189">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-189">This operation cannot be undone.</span></span>  <span data-ttu-id="4fbaf-190">Nechtěnému odstranění kritické zóny se může mít významné obchodní dopad.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-190">Accidentally deleting a critical zone has the potential to have a significant business impact.</span></span>  <span data-ttu-id="4fbaf-191">Je proto velmi důležité pro ochranu proti náhodnému zóny odstranění.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-191">It is therefore very important to protect against accidental zone deletion.</span></span>

<span data-ttu-id="4fbaf-192">Před odstraněním použití DoNotDelete zámku na zónu zabrání zóny.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-192">Applying a DoNotDelete lock to a zone prevents the zone from being deleted.</span></span>  <span data-ttu-id="4fbaf-193">Ale vzhledem k tomu, že zámky jsou zdědí podřízené prostředky, zabrání také libovolné sady záznamů v zóně před odstraněním, což může být žádoucí.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-193">However, since locks are inherited by child resources, it also prevents any record sets in the zone from being deleted, which may be undesirable.</span></span>  <span data-ttu-id="4fbaf-194">Kromě toho jak je popsáno v poznámce výše, je také neúčinná vzhledem k tomu, že záznamy lze přesto odebrat z existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-194">Furthermore, as described in the note above, it is also ineffective since records can still be removed from the existing record sets.</span></span>

<span data-ttu-id="4fbaf-195">Jako alternativu zvažte použití DoNotDelete uzamčení záznamů v zóně, jako je například sady záznamů SOA.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-195">As an alternative, consider applying a DoNotDelete lock to a record set in the zone, such as the SOA record set.</span></span>  <span data-ttu-id="4fbaf-196">Vzhledem k tomu, že zóna nelze odstranit bez také odstranění sady záznamů, je to ochrana proti odstranění zóny, zároveň umožňuje sad záznamů v rámci zóny volně upravovat.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-196">Since the zone cannot be deleted without also deleting the record sets, this protects against zone deletion, while still allowing record sets within the zone to be modified freely.</span></span> <span data-ttu-id="4fbaf-197">Pokud je proveden pokus o odstranit zónu, Azure Resource Manager zjistí, to by také odstranit sadu záznamů SOA a blokuje volání, protože je pevně nastavené SOA.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-197">If an attempt is made to delete the zone, Azure Resource Manager detects this would also delete the SOA record set, and blocks the call because the SOA is locked.</span></span>  <span data-ttu-id="4fbaf-198">Žádné sady záznamů se odstraní.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-198">No record sets are deleted.</span></span>

<span data-ttu-id="4fbaf-199">Následující příkaz prostředí PowerShell vytvoří DoNotDelete zámku na záznam SOA dané zóny:</span><span class="sxs-lookup"><span data-stu-id="4fbaf-199">The following PowerShell command creates a DoNotDelete lock against the SOA record of the given zone:</span></span>

```powershell
# Protect against zone delete with DoNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="4fbaf-200">Jiný způsob, jak zabránit náhodnému zóny odstranění je pomocí vlastní role zajistit operátor a účty služby pro správu zón nemáte oprávnění odstranit zónu.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-200">Another way to prevent accidental zone deletion is by using a custom role to ensure the operator and service accounts used to manage your zones do not have zone delete permissions.</span></span> <span data-ttu-id="4fbaf-201">Pokud musíte odstranit zónu, můžete vynutit odstranění dvoustupňové, první udělení oprávnění zóny delete (v oboru zóny, aby se zabránilo odstraněním této zóny nesprávný) a druhý odstranit zónu.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-201">When you do need to delete a zone, you can enforce a two-step delete, first granting zone delete permissions (at the zone scope, to prevent deleting the wrong zone) and second to delete the zone.</span></span>

<span data-ttu-id="4fbaf-202">Druhý přístup má výhodu, který funguje pro všechny zóny přístup tyto účty, aniž by museli nezapomeňte vytvořit žádné zámky.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-202">This second approach has the advantage that it works for all zones accessed by those accounts, without having to remember to create any locks.</span></span> <span data-ttu-id="4fbaf-203">Obsahuje všechny účty s oprávněními odstranit zónu, jako je vlastník předplatného, můžete stále omylem odstranit zónu kritické nevýhodou.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-203">It has the disadvantage that any accounts with zone delete permissions, such as the subscription owner, can still accidentally delete a critical zone.</span></span>

<span data-ttu-id="4fbaf-204">Je možné použít obou přístupů - uzamčení prostředků a vlastní role - ve stejnou dobu jako obrany zabezpečení přístupu k ochraně zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="4fbaf-204">It is possible to use both approaches - resource locks and custom roles - at the same time, as a defense-in-depth approach to DNS zone protection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fbaf-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4fbaf-205">Next steps</span></span>

* <span data-ttu-id="4fbaf-206">Další informace o práci s RBAC najdete v tématu [Začínáme se správou přístupu na portálu Azure](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-206">For more information about working with RBAC, see [Get started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>
* <span data-ttu-id="4fbaf-207">Další informace o práci s uzamčení prostředků najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaf-207">For more information about working with resource locks, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

