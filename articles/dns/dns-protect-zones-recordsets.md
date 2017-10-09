---
title: "aaaProtecting zóny DNS a záznamy | Microsoft Docs"
description: "Jak zóny DNS tooprotect sad záznamů a záznamů v Microsoft Azure DNS."
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
ms.openlocfilehash: 7945f6240feeed3d79a11d340f9f845e083026ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-dns-zones-and-records"></a><span data-ttu-id="2c7c8-103">Jak tooprotect DNS zóny a zaznamenává</span><span class="sxs-lookup"><span data-stu-id="2c7c8-103">How tooprotect DNS zones and records</span></span>

<span data-ttu-id="2c7c8-104">Zóny DNS a záznamy jsou důležité prostředky.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-104">DNS zones and records are critical resources.</span></span> <span data-ttu-id="2c7c8-105">Odstraňování zónu DNS, nebo jenom jeden záznam DNS, může mít za následek výpadku celkový služeb.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-105">Deleting a DNS zone or even just a single DNS record can result in a total service outage.</span></span>  <span data-ttu-id="2c7c8-106">Proto je důležité, aby kritické zóny DNS a záznamy chráněná před neoprávněným nebo náhodné změny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-106">It is therefore important that critical DNS zones and records are protected against unauthorized or accidental changes.</span></span>

<span data-ttu-id="2c7c8-107">Tento článek vysvětluje, jak Azure DNS umožňuje vám tooprotect zón DNS a záznamy proti tyto změny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-107">This article explains how Azure DNS enables you tooprotect your DNS zones and records against such changes.</span></span>  <span data-ttu-id="2c7c8-108">Jsme použít dvě funkce efektivní zabezpečení poskytované pomocí Správce prostředků Azure: [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) a [uzamčení prostředků](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-108">We apply two powerful security features provided by Azure Resource Manager: [role-based access control](../active-directory/role-based-access-control-what-is.md) and [resource locks](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="2c7c8-109">Řízení přístupu na základě role</span><span class="sxs-lookup"><span data-stu-id="2c7c8-109">Role-based access control</span></span>

<span data-ttu-id="2c7c8-110">Azure na základě rolí řízení přístupu (RBAC) umožňuje přesnou správu přístupu pro Azure uživatele, skupiny a prostředky.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-110">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure users, groups, and resources.</span></span> <span data-ttu-id="2c7c8-111">Pomocí RBAC, můžete udělit přesněji hello množství přístup tito uživatelé si musí tooperform svou práci.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-111">Using RBAC, you can grant precisely hello amount of access that users need tooperform their jobs.</span></span> <span data-ttu-id="2c7c8-112">Další informace o tom, jak AZURE pomůže spravovat přístup, najdete v části [co je řízení přístupu na základě Role](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-112">For more information about how RBAC helps you manage access, see [What is Role-Based Access Control](../active-directory/role-based-access-control-what-is.md).</span></span>

### <a name="hello-dns-zone-contributor-role"></a><span data-ttu-id="2c7c8-113">role Hello 'Přispěvatel zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-113">hello 'DNS Zone Contributor' role</span></span>

<span data-ttu-id="2c7c8-114">role, Přispěvatel zóny DNS"Hello je předdefinovaná role, poskytovaný platformou Azure pro správu prostředků služby DNS.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-114">hello 'DNS Zone Contributor' role is a built-in role provided by Azure for managing DNS resources.</span></span>  <span data-ttu-id="2c7c8-115">Přiřazení Přispěvatel zóny DNS oprávnění tooa uživatele nebo skupiny umožňuje této skupiny toomanage DNS prostředky, ale ne prostředky žádným jiným typem.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-115">Assigning DNS Zone Contributor permissions tooa user or group enables that group toomanage DNS resources, but not resources of any other type.</span></span>

<span data-ttu-id="2c7c8-116">Předpokládejme například, že myzones' hello prostředku skupiny"obsahuje pět zóny pro společnost Contoso.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-116">For example, suppose hello resource group 'myzones' contains five zones for Contoso Corporation.</span></span> <span data-ttu-id="2c7c8-117">Udělení hello DNS správce, Přispěvatel zóny DNS, oprávnění toothat skupinu prostředků, umožňuje plnou kontrolu nad tyto zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-117">Granting hello DNS administrator 'DNS Zone Contributor' permissions toothat resource group, enables full control over those DNS zones.</span></span> <span data-ttu-id="2c7c8-118">Také se vyhnete, udělení nadbytečná oprávnění, například Správce DNS hello nelze vytvořit nebo zastavit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-118">It also avoids granting unnecessary permissions, for example hello DNS administrator cannot create or stop Virtual Machines.</span></span>

<span data-ttu-id="2c7c8-119">Hello nejjednodušší způsob, jak tooassign RBAC oprávnění je [prostřednictvím portálu Azure hello](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-119">hello simplest way tooassign RBAC permissions is [via hello Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>  <span data-ttu-id="2c7c8-120">Otevřete okno hello 'řízení přístupu (IAM)' pro skupinu prostředků hello, pak klikněte na tlačítko "Přidat" a potom vyberte role hello 'Přispěvatel zóny DNS a vyberte hello požadované uživatele nebo skupiny toogrant oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-120">Open hello 'Access control (IAM)' blade for hello resource group, then click 'Add', then select hello 'DNS Zone Contributor' role and select hello required users or groups toogrant permissions.</span></span>

![Úrovni skupiny prostředků RBAC prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/rbac1.png)

<span data-ttu-id="2c7c8-122">Oprávnění může být také [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="2c7c8-122">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="2c7c8-123">ekvivalentní příkaz Hello je také [dostupné prostřednictvím rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="2c7c8-123">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a><span data-ttu-id="2c7c8-124">Úroveň zóny RBAC</span><span class="sxs-lookup"><span data-stu-id="2c7c8-124">Zone level RBAC</span></span>

<span data-ttu-id="2c7c8-125">Azure RBAC pravidla mohou být předplatné použité tooa prostředků skupiny nebo tooan jednotlivých prostředku.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-125">Azure RBAC rules can be applied tooa subscription, a resource group or tooan individual resource.</span></span> <span data-ttu-id="2c7c8-126">V případě hello Azure DNS může být tento prostředek jednotlivé zóny DNS, nebo i jednotlivé sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-126">In hello case of Azure DNS, that resource can be an individual DNS zone, or even an individual record set.</span></span>

<span data-ttu-id="2c7c8-127">Předpokládejme například, že obsahuje myzones' hello prostředku skupiny"hello zónu"contoso.com"a subzone 'customers.contoso.com, ve kterém jsou vytvořeny záznamy CNAME pro každou účtu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-127">For example, suppose hello resource group 'myzones' contains hello zone 'contoso.com' and a subzone 'customers.contoso.com' in which CNAME records are created for each customer account.</span></span>  <span data-ttu-id="2c7c8-128">účet používaný toomanage Hello tyto záznamy CNAME by měla být přiřazená oprávnění toocreate záznamy hello 'customers.contoso.com' pouze zóny, by neměl mít přístup toohello jiné zóny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-128">hello account used toomanage these CNAME records should be assigned permissions toocreate records in hello 'customers.contoso.com' zone only, it should not have access toohello other zones.</span></span>

<span data-ttu-id="2c7c8-129">Oprávnění na úrovni zóny RBAC může být poskytnuto prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-129">Zone-level RBAC permissions can be granted via hello Azure portal.</span></span>  <span data-ttu-id="2c7c8-130">Otevřete okno hello "Řízení přístupu (IAM)" pro hello zónu, pak klikněte na tlačítko "Přidat" a potom vyberte role hello 'Přispěvatel zóny DNS a vyberte hello požadované uživatele nebo skupiny toogrant oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-130">Open hello 'Access control (IAM)' blade for hello zone, then click 'Add', then select hello 'DNS Zone Contributor' role and select hello required users or groups toogrant permissions.</span></span>

![Zóna DNS úrovně RBAC prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/rbac2.png)

<span data-ttu-id="2c7c8-132">Oprávnění může být také [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="2c7c8-132">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions tooa specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

<span data-ttu-id="2c7c8-133">ekvivalentní příkaz Hello je také [dostupné prostřednictvím rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="2c7c8-133">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions tooa specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a><span data-ttu-id="2c7c8-134">Úroveň RBAC sady záznamů</span><span class="sxs-lookup"><span data-stu-id="2c7c8-134">Record set level RBAC</span></span>

<span data-ttu-id="2c7c8-135">Jeden krok jsme může pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-135">We can go one step further.</span></span> <span data-ttu-id="2c7c8-136">Vezměte v úvahu hello e-mailu správce společnosti Contoso Corporation, který potřebuje přístup toohello MX a TXT záznamy na vrcholu hello zónu "contoso.com" hello.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-136">Consider hello mail administrator for Contoso Corporation, who needs access toohello MX and TXT records at hello apex of hello 'contoso.com' zone.</span></span>  <span data-ttu-id="2c7c8-137">Uživatel nebude potřebovat přístup k tooany dalších záznamů MX nebo TXT nebo tooany záznamy o jiný typ.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-137">She doesn't need access tooany other MX or TXT records, or tooany records of any other type.</span></span>  <span data-ttu-id="2c7c8-138">Azure DNS umožňuje vám tooassign oprávnění v hello sady záznamů úrovně, tooprecisely hello záznamy, které hello e-mailu správce potřebuje přístup k.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-138">Azure DNS allows you tooassign permissions at hello record set level, tooprecisely hello records that hello mail administrator needs access to.</span></span>  <span data-ttu-id="2c7c8-139">Hello e-mailu správce má oprávnění přesněji hello řízení Jana potřebuje a je nelze toomake další změny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-139">hello mail administrator is granted precisely hello control she needs, and is unable toomake any other changes.</span></span>

<span data-ttu-id="2c7c8-140">Oprávnění na úrovni RBAC sadu záznamů je možné nakonfigurovat přes hello portál Azure, pomocí tlačítka hello uživatelé v okně hello sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-140">Record-set level RBAC permissions can be configured via hello Azure portal, using hello 'Users' button in hello record set blade:</span></span>

![Sady záznamů úroveň RBAC prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/rbac3.png)

<span data-ttu-id="2c7c8-142">Může být také sadu záznamů oprávnění na úrovni RBAC [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="2c7c8-142">Record-set level RBAC permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant permissions tooa specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

<span data-ttu-id="2c7c8-143">ekvivalentní příkaz Hello je také [dostupné prostřednictvím rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="2c7c8-143">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant permissions tooa specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a><span data-ttu-id="2c7c8-144">Vlastní role</span><span class="sxs-lookup"><span data-stu-id="2c7c8-144">Custom roles</span></span>

<span data-ttu-id="2c7c8-145">Předdefinovaná role, Přispěvatel zóny DNS"Hello umožňuje plnou kontrolu nad prostředků DNS.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-145">hello built-in 'DNS Zone Contributor' role enables full control over a DNS resource.</span></span> <span data-ttu-id="2c7c8-146">Ho je také možné toobuild vlastní zákazník Azure rolí, tooprovide i citlivější řízení.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-146">It is also possible toobuild your own customer Azure roles, tooprovide even finer-grained control.</span></span>

<span data-ttu-id="2c7c8-147">Zvažte znovu hello příklad, ve kterém se vytvoří záznam CNAME v hello zóny 'customers.contoso.com' u každého účtu zákazníka Contoso Corporation.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-147">Consider again hello example in which a CNAME record in hello zone 'customers.contoso.com' is created for each Contoso Corporation customer account.</span></span>  <span data-ttu-id="2c7c8-148">účet používaný toomanage Hello těchto záznamů CNAME udělení oprávnění toomanage záznamy CNAME jenom.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-148">hello account used toomanage these CNAMEs should be granted permission toomanage CNAME records only.</span></span>  <span data-ttu-id="2c7c8-149">Je pak nelze toomodify záznamy jiné typy (jako je například změna záznamů MX) nebo provádět operace na úrovni zóny například odstranit zónu.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-149">It is then unable toomodify records of other types (such as changing MX records) or perform zone-level operations such as zone delete.</span></span>

<span data-ttu-id="2c7c8-150">Hello následující příklad ukazuje definice vlastních rolí pro správu pouze záznamy CNAME:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-150">hello following example shows a custom role definition for managing CNAME records only:</span></span>

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

<span data-ttu-id="2c7c8-151">Hello akce vlastnost definuje hello následující DNS konkrétní oprávnění:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-151">hello Actions property defines hello following DNS-specific permissions:</span></span>

* <span data-ttu-id="2c7c8-152">`Microsoft.Network/dnsZones/CNAME/*`uděluje plnou kontrolu nad záznamy CNAME</span><span class="sxs-lookup"><span data-stu-id="2c7c8-152">`Microsoft.Network/dnsZones/CNAME/*` grants full control over CNAME records</span></span>
* <span data-ttu-id="2c7c8-153">`Microsoft.Network/dnsZones/read`uděluje oprávnění tooread DNS zóny, ale není toomodify je povolení jste toosee hello zónu, ve které hello se vytváří CNAME.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-153">`Microsoft.Network/dnsZones/read` grants permission tooread DNS zones, but not toomodify them, enabling you toosee hello zone in which hello CNAME is being created.</span></span>

<span data-ttu-id="2c7c8-154">Hello zbývající akce jsou zkopírovány ze hello [předdefinovaná role Přispěvatel zóny DNS](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-154">hello remaining Actions are copied from hello [DNS Zone Contributor built-in role](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span></span>

> [!NOTE]
> <span data-ttu-id="2c7c8-155">Použití vlastní tooprevent role RBAC odstraňování záznamu sad stále povolením toobe aktualizovat nejsou efektivní řízení.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-155">Using a custom RBAC role tooprevent deleting record sets while still allowing them toobe updated is not an effective control.</span></span> <span data-ttu-id="2c7c8-156">Sady záznamů zabrání odstraňuje, ale nezabrání je upravovat.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-156">It prevents record sets from being deleted, but it does not prevent them from being modified.</span></span>  <span data-ttu-id="2c7c8-157">Povolené změny zahrnují přidávání a odebírání záznamů z sady záznamů hello, včetně odebrání všech záznamů tooleave 'prázdná' sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-157">Permitted modifications include adding and removing records from hello record set, including removing all records tooleave an 'empty' record set.</span></span> <span data-ttu-id="2c7c8-158">Tato akce nemá hello stejného efektu jako odstraňování záznamu hello nastavit z hlediska rozlišení DNS.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-158">This has hello same effect as deleting hello record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="2c7c8-159">Definice vlastní role nemůže být definovaný aktuálně prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-159">Custom role definitions cannot currently be defined via hello Azure portal.</span></span> <span data-ttu-id="2c7c8-160">Pomocí Azure Powershellu lze vytvořit vlastní role založená na této definici role:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-160">A custom role based on this role definition can be created using Azure PowerShell:</span></span>

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

<span data-ttu-id="2c7c8-161">Také možné vytvářet prostřednictvím hello rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-161">It can also be created via hello Azure CLI:</span></span>

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

<span data-ttu-id="2c7c8-162">Hello role pak lze přistupovat v hello stejný způsobem jako pro předdefinované role, jak je popsáno výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-162">hello role can then be assigned in hello same way as built-in roles, as described earlier in this article.</span></span>

<span data-ttu-id="2c7c8-163">Další informace o tom, jak toocreate, spravovat a přiřadit vlastní role, naleznete v části [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-163">For more information on how toocreate, manage, and assign custom roles, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="resource-locks"></a><span data-ttu-id="2c7c8-164">Uzamčení prostředků</span><span class="sxs-lookup"><span data-stu-id="2c7c8-164">Resource locks</span></span>

<span data-ttu-id="2c7c8-165">V přidání tooRBAC Azure Resource Manager podporuje jiný typ řízení zabezpečení, a to možnost too'lock hello se prostředky.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-165">In addition tooRBAC, Azure Resource Manager supports another type of security control, namely hello ability too'lock' resources.</span></span> <span data-ttu-id="2c7c8-166">Kde RBAC pravidla povolit akce hello toocontrol konkrétních uživatelů a skupin, uzamčení prostředků jsou použité toohello prostředků a jsou platné ve všech uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-166">Where RBAC rules allow you toocontrol hello actions of specific users and groups, resource locks are applied toohello resource, and are effective across all users and roles.</span></span> <span data-ttu-id="2c7c8-167">Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-167">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

<span data-ttu-id="2c7c8-168">Existují dva typy prostředků zámku: **DoNotDelete** a **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-168">There are two types of resource lock: **DoNotDelete** and **ReadOnly**.</span></span> <span data-ttu-id="2c7c8-169">Ty lze použít buď tooa zónu DNS, nebo sadu záznamů jednotlivých tooan.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-169">These can be applied either tooa DNS zone, or tooan individual record set.</span></span>  <span data-ttu-id="2c7c8-170">Hello následující části popisují několik běžných scénářů a jak toosupport je pomocí uzamčení prostředků.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-170">hello following sections describe several common scenarios, and how toosupport them using resource locks.</span></span>

### <a name="protecting-against-all-changes"></a><span data-ttu-id="2c7c8-171">Ochrana proti všechny změny</span><span class="sxs-lookup"><span data-stu-id="2c7c8-171">Protecting against all changes</span></span>

<span data-ttu-id="2c7c8-172">tooprevent změny prováděné, použít zónu toohello zámku jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-172">tooprevent any changes being made, apply a ReadOnly lock toohello zone.</span></span>  <span data-ttu-id="2c7c8-173">To brání tomu, aby nové sady záznamů se vytvořil a existující sady záznamů upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-173">This prevents new record sets from being created, and existing record sets from being modified or deleted.</span></span>

<span data-ttu-id="2c7c8-174">Zónu prostředků s úrovní zámky lze vytvořit prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-174">Zone level resource locks can be created via hello Azure portal.</span></span>  <span data-ttu-id="2c7c8-175">V okně zóny DNS hello, klikněte na tlačítko 'Zámky' pak přidejte:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-175">From hello DNS zone blade, click 'Locks', then 'Add':</span></span>

![Zónu prostředků s úrovní zámky prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/locks1.png)

<span data-ttu-id="2c7c8-177">Úroveň zóny prostředku, který zámky. můžete také vytvořit prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-177">Zone-level resource locks can also be created via Azure PowerShell:</span></span>

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

<span data-ttu-id="2c7c8-178">Konfigurace prostředků Azure zámků není aktuálně podporován prostřednictvím hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-178">Configuring Azure resource locks is not currently supported via hello Azure CLI.</span></span>

### <a name="protecting-individual-records"></a><span data-ttu-id="2c7c8-179">Ochrana jednotlivých záznamů</span><span class="sxs-lookup"><span data-stu-id="2c7c8-179">Protecting individual records</span></span>

<span data-ttu-id="2c7c8-180">tooprevent existujícího záznamu DNS nastavit pro úpravy, použít sadu záznamů toohello zámku jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-180">tooprevent an existing DNS record set against modification, apply a ReadOnly lock toohello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="2c7c8-181">Použití tooa zámku DoNotDelete sady záznamů není efektivní řízení.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-181">Applying a DoNotDelete lock tooa record set is not an effective control.</span></span> <span data-ttu-id="2c7c8-182">Zabraňuje sady před odstraněním hello záznamů, ale nezabrání ho upravovat.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-182">It prevents hello record set from being deleted, but it does not prevent it from being modified.</span></span>  <span data-ttu-id="2c7c8-183">Povolené změny zahrnují přidávání a odebírání záznamů z sady záznamů hello, včetně odebrání všech záznamů tooleave 'prázdná' sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-183">Permitted modifications include adding and removing records from hello record set, including removing all records tooleave an 'empty' record set.</span></span> <span data-ttu-id="2c7c8-184">Tato akce nemá hello stejného efektu jako odstraňování záznamu hello nastavit z hlediska rozlišení DNS.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-184">This has hello same effect as deleting hello record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="2c7c8-185">Sady záznamů prostředků s úrovní zámky momentálně můžete být pouze nakonfigurovat pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-185">Record set level resource locks can currently only be configured using Azure PowerShell.</span></span>  <span data-ttu-id="2c7c8-186">Nejsou podporovány v hello portál Azure nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-186">They are not supported in hello Azure portal or Azure CLI.</span></span>

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a><span data-ttu-id="2c7c8-187">Ochrana proti odstranění zóny</span><span class="sxs-lookup"><span data-stu-id="2c7c8-187">Protecting against zone deletion</span></span>

<span data-ttu-id="2c7c8-188">Při odstranění zóny v Azure DNS se také odstraní všechny sady záznamů v zóně hello.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-188">When a zone is deleted in Azure DNS, all record sets in hello zone are also deleted.</span></span>  <span data-ttu-id="2c7c8-189">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-189">This operation cannot be undone.</span></span>  <span data-ttu-id="2c7c8-190">Potenciální toohave hello nechtěnému odstranění zónu kritické má významné obchodní dopad.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-190">Accidentally deleting a critical zone has hello potential toohave a significant business impact.</span></span>  <span data-ttu-id="2c7c8-191">Je proto velmi důležité tooprotect proti náhodnému zóny odstranění.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-191">It is therefore very important tooprotect against accidental zone deletion.</span></span>

<span data-ttu-id="2c7c8-192">Před odstraněním použití zónu DoNotDelete zámku tooa zabrání hello zóny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-192">Applying a DoNotDelete lock tooa zone prevents hello zone from being deleted.</span></span>  <span data-ttu-id="2c7c8-193">Ale vzhledem k tomu, že zámky jsou zdědí podřízené prostředky, zabrání také libovolné sady záznamů v zóně hello před odstraněním, což může být žádoucí.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-193">However, since locks are inherited by child resources, it also prevents any record sets in hello zone from being deleted, which may be undesirable.</span></span>  <span data-ttu-id="2c7c8-194">Kromě toho jak je popsáno v poznámce hello výše, je také neúčinná vzhledem k tomu, že záznamy lze přesto odebrat z existující sady záznamů hello.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-194">Furthermore, as described in hello note above, it is also ineffective since records can still be removed from hello existing record sets.</span></span>

<span data-ttu-id="2c7c8-195">Jako alternativu zvažte použití záznam DoNotDelete zámku tooa nastavit v zóně hello, jako je například sady záznamů SOA hello.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-195">As an alternative, consider applying a DoNotDelete lock tooa record set in hello zone, such as hello SOA record set.</span></span>  <span data-ttu-id="2c7c8-196">Vzhledem k tomu, že zóna hello nelze odstranit bez také odstranění sad záznamů hello, je to ochrana proti odstranění zóny, zároveň umožňuje sady záznamů v rámci toobe zóny hello libovolně změnit.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-196">Since hello zone cannot be deleted without also deleting hello record sets, this protects against zone deletion, while still allowing record sets within hello zone toobe modified freely.</span></span> <span data-ttu-id="2c7c8-197">Pokud se pokus o toodelete hello zóny, Azure Resource Manager zjistí, to by také odstranit hello sady záznamů SOA a bloky hello volání, protože je uzamčen hello SOA.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-197">If an attempt is made toodelete hello zone, Azure Resource Manager detects this would also delete hello SOA record set, and blocks hello call because hello SOA is locked.</span></span>  <span data-ttu-id="2c7c8-198">Žádné sady záznamů se odstraní.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-198">No record sets are deleted.</span></span>

<span data-ttu-id="2c7c8-199">Hello následující příkaz prostředí PowerShell vytvoří DoNotDelete zámku proti záznamu SOA hello hello zadané zóny:</span><span class="sxs-lookup"><span data-stu-id="2c7c8-199">hello following PowerShell command creates a DoNotDelete lock against hello SOA record of hello given zone:</span></span>

```powershell
# Protect against zone delete with DoNotDelete lock on hello record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="2c7c8-200">Jiným způsobem, jak tooprevent zóny náhodného odstranění je pomocí vlastní role tooensure hello operátor a toomanage účty používané služby zón mít není zónu odstranit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-200">Another way tooprevent accidental zone deletion is by using a custom role tooensure hello operator and service accounts used toomanage your zones do not have zone delete permissions.</span></span> <span data-ttu-id="2c7c8-201">Pokud budete potřebovat toodelete zónu, můžete vynutit odstranění dvoustupňové, první udělující oprávnění zóny delete (v hello zóny oboru, tooprevent odstranění zóny nesprávný hello) a druhý toodelete hello zóny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-201">When you do need toodelete a zone, you can enforce a two-step delete, first granting zone delete permissions (at hello zone scope, tooprevent deleting hello wrong zone) and second toodelete hello zone.</span></span>

<span data-ttu-id="2c7c8-202">Druhý přístup má výhodu hello, zda funguje pro všechny zóny přístup tyto účty, bez nutnosti tooremember toocreate žádné zámky.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-202">This second approach has hello advantage that it works for all zones accessed by those accounts, without having tooremember toocreate any locks.</span></span> <span data-ttu-id="2c7c8-203">Obsahuje všechny účty s oprávněními odstranit zónu, jako je například hello vlastník předplatného, můžete stále omylem odstranit zónu kritické nevýhodou hello.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-203">It has hello disadvantage that any accounts with zone delete permissions, such as hello subscription owner, can still accidentally delete a critical zone.</span></span>

<span data-ttu-id="2c7c8-204">Je možné toouse obou přístupů - uzamčení prostředků a vlastní role - na hello stejný čas, jako ochrany obrany do hloubky přístup tooDNS zóny.</span><span class="sxs-lookup"><span data-stu-id="2c7c8-204">It is possible toouse both approaches - resource locks and custom roles - at hello same time, as a defense-in-depth approach tooDNS zone protection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c7c8-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c7c8-205">Next steps</span></span>

* <span data-ttu-id="2c7c8-206">Další informace o práci s RBAC najdete v tématu [Začínáme se správou přístupu v hello portál Azure](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-206">For more information about working with RBAC, see [Get started with access management in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>
* <span data-ttu-id="2c7c8-207">Další informace o práci s uzamčení prostředků najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c8-207">For more information about working with resource locks, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

