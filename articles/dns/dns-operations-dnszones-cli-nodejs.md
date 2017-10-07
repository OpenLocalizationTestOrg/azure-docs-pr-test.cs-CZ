---
title: "aaaManage DNS zóny v Azure DNS - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 1.0 zóny DNS. Tento článek ukazuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: cb9790cc46626ef7f38a43edb57511104fe6057e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-10"></a>Jak hello toomanage zóny DNS v Azure DNS pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Tato příručka ukazuje, jak toomanage DNS zóny pomocí hello platformě Azure CLI 1.0, která je k dispozici pro Windows, Mac a Linux. Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo hello portálu Azure.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.

## <a name="introduction"></a>Úvod

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Získání nápovědy

Všechny příkazy rozhraní příkazového řádku 1.0 týkající se tooAzure DNS začínat `azure network dns`. Je k dispozici u každého příkazu pomocí hello Nápověda `--help` možnost (krátký tvar `-h`).  Například:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóny DNS je vytvořený pomocí hello `azure network dns zone create` příkaz. Nápovědu získáte příkazem `azure network dns zone create -h`.

Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a>toocreate zóny DNS pomocí značek

Hello následující příklad ukazuje, jak toocreate DNS zóny se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*, pomocí hello `--tags` parametr (krátký tvar `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Získat zóny DNS

tooretrieve zóny DNS, použijte `azure network dns zone show`. Nápovědu získáte příkazem `azure network dns zone show -h`.

Hello následující příklad vrací zóny DNS hello *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

Následující ukázka Hello je hello odpovědi.

```
info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Všimněte si, že nejsou vrácené záznamy DNS `azure network dns zone show`. toolist záznamy DNS, použijte `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Seznam zón DNS

tooenumerate zóny DNS, použijte `azure network dns zone list`. Nápovědu získáte příkazem `azure network dns zone list -h`.

Zadání skupiny prostředků hello uvádí pouze pásma v rámci skupiny prostředků hello:

```azurecli
azure network dns zone list MyResourceGroup
```

Vynechání skupiny prostředků hello seznam všech zón v předplatném hello:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Aktualizaci zóny DNS

Změny tooa prostředků zónu DNS můžete provést pomocí `azure network dns zone set`. Nápovědu získáte příkazem `azure network dns zone set -h`.

Tento příkaz nelze aktualizovat všechny hello sad záznamů DNS v rámci zóny hello (viz [jak záznamy DNS tooManage](dns-operations-recordsets-cli-nodejs.md)). Je pouze použité tooupdate vlastnosti prostředku zóny hello, sám sebe. Tyto vlastnosti jsou aktuálně omezená toohello [Azure Resource Manager, značky'](dns-zones-records.md#tags) pro prostředek zóny hello.

Hello následující příklad ukazuje, jak tooupdate hello značky na zónu DNS. existující značky Hello se nahrazují hello hodnota.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Zóny DNS lze odstranit pomocí `azure network dns zone delete`. Nápovědu získáte příkazem `azure network dns zone delete -h`.

> [!NOTE]
> Odstranění zóny DNS také odstraní všechny záznamy DNS v rámci zóny hello. Tuto operaci nelze vrátit zpět. Pokud zónu DNS hello se používá, službám pomocí hello zóny se nezdaří, při odstranění zóny hello.
>
>najdete v části tooprotect proti náhodnému zóny odstranění [jak tooprotect DNS zóny a zaznamenává](dns-protect-zones-recordsets.md).

Tento příkaz zobrazí výzvu k potvrzení. Hello volitelné `--quiet` přepínače (krátký tvar `-q`) potlačí tuto výzvu.

Hello následující příklad ukazuje, jak toodelete hello zóny *contoso.com* ze skupiny prostředků *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli-nodejs.md) ve vaší zóně DNS.

Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).

