---
title: "aaaManage DNS zóny v Azure DNS - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 2.0 zóny DNS. Tento článek ukazuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 3945a558b2db3490e50678d8395a47e55a85c8fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-20"></a>Jak hello toomanage zóny DNS v Azure DNS pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)


Tato příručka ukazuje, jak toomanage DNS zóny pomocí hello platformě Azure CLI, která je k dispozici pro Windows, Mac a Linux. Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo hello portálu Azure.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.

## <a name="introduction"></a>Úvod

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Nastavení Azure CLI 2.0 pro Azure DNS

### <a name="before-you-begin"></a>Než začnete

Ověřte, zda máte následující položky před zahájením konfigurace hello.

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

* Nainstalujte nejnovější verzi hello hello 2.0 rozhraní příkazového řádku Azure pro Windows, Linux nebo MAC. Další informace najdete v [hello instalace Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-tooyour-azure-account"></a>Přihlaste se tooyour účet Azure

Otevřete okno konzoly a proveďte ověření pomocí svých přihlašovacích údajů. Další informace naleznete v protokolu v tooAzure z hello rozhraní příkazového řádku Azure

```
az login
```

### <a name="select-hello-subscription"></a>Vyberte předplatné hello

Zkontrolujte předplatná hello pro účet hello.

```
az account list
```

Zvolte, které vaše toouse předplatných Azure.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků. Ale vzhledem k tomu, že všechny prostředky DNS jsou globální, a ne místní, hello Volba umístění skupiny prostředků nemá žádný vliv na Azure DNS.

Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Získání nápovědy

Všechny příkazy rozhraní příkazového řádku 2.0 týkající se tooAzure DNS začínat `az network dns`. Je k dispozici u každého příkazu pomocí hello Nápověda `--help` možnost (krátký tvar `-h`).  Například:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóny DNS je vytvořený pomocí hello `az network dns zone create` příkaz. Nápovědu získáte příkazem `az network dns zone create -h`.

Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a>toocreate zóny DNS pomocí značek

Hello následující příklad ukazuje, jak toocreate DNS zóny se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*, pomocí hello `--tags` parametr (krátký tvar `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Získat zóny DNS

tooretrieve zóny DNS, použijte `az network dns zone show`. Nápovědu získáte příkazem `az network dns zone show --help`.

Hello následující příklad vrací zóny DNS hello *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

Následující ukázka Hello je hello odpovědi.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Všimněte si, že nejsou vrácené záznamy DNS `az network dns zone show`. toolist záznamy DNS, použijte `az network dns record-set list`.


## <a name="list-dns-zones"></a>Seznam zón DNS

tooenumerate zóny DNS, použijte `az network dns zone list`. Nápovědu získáte příkazem `az network dns zone list --help`.

Zadání skupiny prostředků hello uvádí pouze pásma v rámci skupiny prostředků hello:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Vynechání skupiny prostředků hello seznam všech zón v předplatném hello:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Aktualizaci zóny DNS

Změny tooa prostředků zónu DNS můžete provést pomocí `az network dns zone update`. Nápovědu získáte příkazem `az network dns zone update --help`.

Tento příkaz nelze aktualizovat všechny hello sad záznamů DNS v rámci zóny hello (viz [jak záznamy DNS tooManage](dns-operations-recordsets-cli.md)). Je pouze použité tooupdate vlastnosti prostředku zóny hello, sám sebe. Tyto vlastnosti jsou aktuálně omezená toohello [Azure Resource Manager, značky'](dns-zones-records.md#tags) pro prostředek zóny hello.

Hello následující příklad ukazuje, jak tooupdate hello značky na zónu DNS. existující značky Hello se nahrazují hello hodnota.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Zóny DNS lze odstranit pomocí `az network dns zone delete`. Nápovědu získáte příkazem `az network dns zone delete --help`.

> [!NOTE]
> Odstranění zóny DNS také odstraní všechny záznamy DNS v rámci zóny hello. Tuto operaci nelze vrátit zpět. Pokud zónu DNS hello se používá, službám pomocí hello zóny se nezdaří, při odstranění zóny hello.
>
>najdete v části tooprotect proti náhodnému zóny odstranění [jak tooprotect DNS zóny a zaznamenává](dns-protect-zones-recordsets.md).

Tento příkaz zobrazí výzvu k potvrzení. Hello volitelné `--yes` přepínač potlačí tuto výzvu.

Hello následující příklad ukazuje, jak toodelete hello zóny *contoso.com* ze skupiny prostředků *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli.md) ve vaší zóně DNS.

Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).

