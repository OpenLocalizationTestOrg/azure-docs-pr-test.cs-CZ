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
# <a name="how-tooprotect-dns-zones-and-records"></a>Jak tooprotect DNS zóny a zaznamenává

Zóny DNS a záznamy jsou důležité prostředky. Odstraňování zónu DNS, nebo jenom jeden záznam DNS, může mít za následek výpadku celkový služeb.  Proto je důležité, aby kritické zóny DNS a záznamy chráněná před neoprávněným nebo náhodné změny.

Tento článek vysvětluje, jak Azure DNS umožňuje vám tooprotect zón DNS a záznamy proti tyto změny.  Jsme použít dvě funkce efektivní zabezpečení poskytované pomocí Správce prostředků Azure: [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) a [uzamčení prostředků](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Řízení přístupu na základě role

Azure na základě rolí řízení přístupu (RBAC) umožňuje přesnou správu přístupu pro Azure uživatele, skupiny a prostředky. Pomocí RBAC, můžete udělit přesněji hello množství přístup tito uživatelé si musí tooperform svou práci. Další informace o tom, jak AZURE pomůže spravovat přístup, najdete v části [co je řízení přístupu na základě Role](../active-directory/role-based-access-control-what-is.md).

### <a name="hello-dns-zone-contributor-role"></a>role Hello 'Přispěvatel zóny DNS.

role, Přispěvatel zóny DNS"Hello je předdefinovaná role, poskytovaný platformou Azure pro správu prostředků služby DNS.  Přiřazení Přispěvatel zóny DNS oprávnění tooa uživatele nebo skupiny umožňuje této skupiny toomanage DNS prostředky, ale ne prostředky žádným jiným typem.

Předpokládejme například, že myzones' hello prostředku skupiny"obsahuje pět zóny pro společnost Contoso. Udělení hello DNS správce, Přispěvatel zóny DNS, oprávnění toothat skupinu prostředků, umožňuje plnou kontrolu nad tyto zóny DNS. Také se vyhnete, udělení nadbytečná oprávnění, například Správce DNS hello nelze vytvořit nebo zastavit virtuální počítače.

Hello nejjednodušší způsob, jak tooassign RBAC oprávnění je [prostřednictvím portálu Azure hello](../active-directory/role-based-access-control-configure.md).  Otevřete okno hello 'řízení přístupu (IAM)' pro skupinu prostředků hello, pak klikněte na tlačítko "Přidat" a potom vyberte role hello 'Přispěvatel zóny DNS a vyberte hello požadované uživatele nebo skupiny toogrant oprávnění.

![Úrovni skupiny prostředků RBAC prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/rbac1.png)

Oprávnění může být také [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

ekvivalentní příkaz Hello je také [dostupné prostřednictvím rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Úroveň zóny RBAC

Azure RBAC pravidla mohou být předplatné použité tooa prostředků skupiny nebo tooan jednotlivých prostředku. V případě hello Azure DNS může být tento prostředek jednotlivé zóny DNS, nebo i jednotlivé sady záznamů.

Předpokládejme například, že obsahuje myzones' hello prostředku skupiny"hello zónu"contoso.com"a subzone 'customers.contoso.com, ve kterém jsou vytvořeny záznamy CNAME pro každou účtu zákazníka.  účet používaný toomanage Hello tyto záznamy CNAME by měla být přiřazená oprávnění toocreate záznamy hello 'customers.contoso.com' pouze zóny, by neměl mít přístup toohello jiné zóny.

Oprávnění na úrovni zóny RBAC může být poskytnuto prostřednictvím hello portálu Azure.  Otevřete okno hello "Řízení přístupu (IAM)" pro hello zónu, pak klikněte na tlačítko "Přidat" a potom vyberte role hello 'Přispěvatel zóny DNS a vyberte hello požadované uživatele nebo skupiny toogrant oprávnění.

![Zóna DNS úrovně RBAC prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/rbac2.png)

Oprávnění může být také [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions tooa specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

ekvivalentní příkaz Hello je také [dostupné prostřednictvím rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions tooa specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Úroveň RBAC sady záznamů

Jeden krok jsme může pokračovat dál. Vezměte v úvahu hello e-mailu správce společnosti Contoso Corporation, který potřebuje přístup toohello MX a TXT záznamy na vrcholu hello zónu "contoso.com" hello.  Uživatel nebude potřebovat přístup k tooany dalších záznamů MX nebo TXT nebo tooany záznamy o jiný typ.  Azure DNS umožňuje vám tooassign oprávnění v hello sady záznamů úrovně, tooprecisely hello záznamy, které hello e-mailu správce potřebuje přístup k.  Hello e-mailu správce má oprávnění přesněji hello řízení Jana potřebuje a je nelze toomake další změny.

Oprávnění na úrovni RBAC sadu záznamů je možné nakonfigurovat přes hello portál Azure, pomocí tlačítka hello uživatelé v okně hello sada záznamů:

![Sady záznamů úroveň RBAC prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/rbac3.png)

Může být také sadu záznamů oprávnění na úrovni RBAC [udělena pomocí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant permissions tooa specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

ekvivalentní příkaz Hello je také [dostupné prostřednictvím rozhraní příkazového řádku Azure hello](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant permissions tooa specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Vlastní role

Předdefinovaná role, Přispěvatel zóny DNS"Hello umožňuje plnou kontrolu nad prostředků DNS. Ho je také možné toobuild vlastní zákazník Azure rolí, tooprovide i citlivější řízení.

Zvažte znovu hello příklad, ve kterém se vytvoří záznam CNAME v hello zóny 'customers.contoso.com' u každého účtu zákazníka Contoso Corporation.  účet používaný toomanage Hello těchto záznamů CNAME udělení oprávnění toomanage záznamy CNAME jenom.  Je pak nelze toomodify záznamy jiné typy (jako je například změna záznamů MX) nebo provádět operace na úrovni zóny například odstranit zónu.

Hello následující příklad ukazuje definice vlastních rolí pro správu pouze záznamy CNAME:

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

Hello akce vlastnost definuje hello následující DNS konkrétní oprávnění:

* `Microsoft.Network/dnsZones/CNAME/*`uděluje plnou kontrolu nad záznamy CNAME
* `Microsoft.Network/dnsZones/read`uděluje oprávnění tooread DNS zóny, ale není toomodify je povolení jste toosee hello zónu, ve které hello se vytváří CNAME.

Hello zbývající akce jsou zkopírovány ze hello [předdefinovaná role Přispěvatel zóny DNS](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Použití vlastní tooprevent role RBAC odstraňování záznamu sad stále povolením toobe aktualizovat nejsou efektivní řízení. Sady záznamů zabrání odstraňuje, ale nezabrání je upravovat.  Povolené změny zahrnují přidávání a odebírání záznamů z sady záznamů hello, včetně odebrání všech záznamů tooleave 'prázdná' sada záznamů. Tato akce nemá hello stejného efektu jako odstraňování záznamu hello nastavit z hlediska rozlišení DNS.

Definice vlastní role nemůže být definovaný aktuálně prostřednictvím hello portálu Azure. Pomocí Azure Powershellu lze vytvořit vlastní role založená na této definici role:

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Také možné vytvářet prostřednictvím hello rozhraní příkazového řádku Azure:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

Hello role pak lze přistupovat v hello stejný způsobem jako pro předdefinované role, jak je popsáno výše v tomto článku.

Další informace o tom, jak toocreate, spravovat a přiřadit vlastní role, naleznete v části [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).

## <a name="resource-locks"></a>Uzamčení prostředků

V přidání tooRBAC Azure Resource Manager podporuje jiný typ řízení zabezpečení, a to možnost too'lock hello se prostředky. Kde RBAC pravidla povolit akce hello toocontrol konkrétních uživatelů a skupin, uzamčení prostředků jsou použité toohello prostředků a jsou platné ve všech uživatelů a rolí. Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).

Existují dva typy prostředků zámku: **DoNotDelete** a **jen pro čtení**. Ty lze použít buď tooa zónu DNS, nebo sadu záznamů jednotlivých tooan.  Hello následující části popisují několik běžných scénářů a jak toosupport je pomocí uzamčení prostředků.

### <a name="protecting-against-all-changes"></a>Ochrana proti všechny změny

tooprevent změny prováděné, použít zónu toohello zámku jen pro čtení.  To brání tomu, aby nové sady záznamů se vytvořil a existující sady záznamů upravit nebo odstranit.

Zónu prostředků s úrovní zámky lze vytvořit prostřednictvím hello portálu Azure.  V okně zóny DNS hello, klikněte na tlačítko 'Zámky' pak přidejte:

![Zónu prostředků s úrovní zámky prostřednictvím hello portálu Azure](./media/dns-protect-zones-recordsets/locks1.png)

Úroveň zóny prostředku, který zámky. můžete také vytvořit prostředí Azure PowerShell:

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Konfigurace prostředků Azure zámků není aktuálně podporován prostřednictvím hello rozhraní příkazového řádku Azure.

### <a name="protecting-individual-records"></a>Ochrana jednotlivých záznamů

tooprevent existujícího záznamu DNS nastavit pro úpravy, použít sadu záznamů toohello zámku jen pro čtení.

> [!NOTE]
> Použití tooa zámku DoNotDelete sady záznamů není efektivní řízení. Zabraňuje sady před odstraněním hello záznamů, ale nezabrání ho upravovat.  Povolené změny zahrnují přidávání a odebírání záznamů z sady záznamů hello, včetně odebrání všech záznamů tooleave 'prázdná' sada záznamů. Tato akce nemá hello stejného efektu jako odstraňování záznamu hello nastavit z hlediska rozlišení DNS.

Sady záznamů prostředků s úrovní zámky momentálně můžete být pouze nakonfigurovat pomocí prostředí Azure PowerShell.  Nejsou podporovány v hello portál Azure nebo Azure CLI.

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Ochrana proti odstranění zóny

Při odstranění zóny v Azure DNS se také odstraní všechny sady záznamů v zóně hello.  Tuto operaci nelze vrátit zpět.  Potenciální toohave hello nechtěnému odstranění zónu kritické má významné obchodní dopad.  Je proto velmi důležité tooprotect proti náhodnému zóny odstranění.

Před odstraněním použití zónu DoNotDelete zámku tooa zabrání hello zóny.  Ale vzhledem k tomu, že zámky jsou zdědí podřízené prostředky, zabrání také libovolné sady záznamů v zóně hello před odstraněním, což může být žádoucí.  Kromě toho jak je popsáno v poznámce hello výše, je také neúčinná vzhledem k tomu, že záznamy lze přesto odebrat z existující sady záznamů hello.

Jako alternativu zvažte použití záznam DoNotDelete zámku tooa nastavit v zóně hello, jako je například sady záznamů SOA hello.  Vzhledem k tomu, že zóna hello nelze odstranit bez také odstranění sad záznamů hello, je to ochrana proti odstranění zóny, zároveň umožňuje sady záznamů v rámci toobe zóny hello libovolně změnit. Pokud se pokus o toodelete hello zóny, Azure Resource Manager zjistí, to by také odstranit hello sady záznamů SOA a bloky hello volání, protože je uzamčen hello SOA.  Žádné sady záznamů se odstraní.

Hello následující příkaz prostředí PowerShell vytvoří DoNotDelete zámku proti záznamu SOA hello hello zadané zóny:

```powershell
# Protect against zone delete with DoNotDelete lock on hello record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Jiným způsobem, jak tooprevent zóny náhodného odstranění je pomocí vlastní role tooensure hello operátor a toomanage účty používané služby zón mít není zónu odstranit oprávnění. Pokud budete potřebovat toodelete zónu, můžete vynutit odstranění dvoustupňové, první udělující oprávnění zóny delete (v hello zóny oboru, tooprevent odstranění zóny nesprávný hello) a druhý toodelete hello zóny.

Druhý přístup má výhodu hello, zda funguje pro všechny zóny přístup tyto účty, bez nutnosti tooremember toocreate žádné zámky. Obsahuje všechny účty s oprávněními odstranit zónu, jako je například hello vlastník předplatného, můžete stále omylem odstranit zónu kritické nevýhodou hello.

Je možné toouse obou přístupů - uzamčení prostředků a vlastní role - na hello stejný čas, jako ochrany obrany do hloubky přístup tooDNS zóny.

## <a name="next-steps"></a>Další kroky

* Další informace o práci s RBAC najdete v tématu [Začínáme se správou přístupu v hello portál Azure](../active-directory/role-based-access-control-what-is.md).
* Další informace o práci s uzamčení prostředků najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).

