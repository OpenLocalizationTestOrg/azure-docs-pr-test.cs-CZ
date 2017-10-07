---
title: "aaaGet začít s Azure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak toocreate DNS zóna a záznam v Azure DNS. Toto je podrobný průvodce toocreate a spravovat první zónu DNS a záznam pomocí hello Azure CLI 1.0."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: e200c848ad261160e593306dbb8a1d92bf26880b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Začínáme s DNS Azure pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Tento článek vás provede kroky toocreate hello první zónu DNS a pomocí záznamu hello 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux. Můžete také provést tyto kroky, pomocí hello portál Azure nebo Azure PowerShell.

Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény. toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Nakonec toopublish DNS zóny toohello Internet, musíte tooconfigure hello názvové servery pro doménu hello. Jednotlivé kroky jsou popsány níže.

Tyto pokyny předpokládají, je již nainstalován a přihlášení tooAzure 1.0 rozhraní příkazového řádku. Nápovědu najdete v tématu [jak toomanage DNS zóny pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Před vytvořením zóny DNS hello, skupinu prostředků se vytvoří zónu DNS toocontain hello. Následující Hello ukazuje příkaz hello.

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóny DNS je vytvořený pomocí hello `azure network dns zone create` příkaz. Zadejte toosee nápovědu pro tento příkaz `azure network dns zone create -h`.

Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*. Použijte příklad hello toocreate zóny DNS, nahraďte hello hodnoty vlastními.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

toocreate záznam DNS použijte hello `azure network dns record-set add-record` příkaz. Nápovědu získáte příkazem `azure network dns record-set add-record -h`.

Hello následující příklad vytvoří záznam s relativním názvem "www" v zóně DNS "contoso.com" ve skupině prostředků "MyResourceGroup" hello hello. Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com". Hello typ záznamu je "A" s IP adresou "1.2.3.4" a použije se výchozí TTL 3 600 sekund (1 hodina).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

U jiných typů záznamů sady záznamů s více než jeden záznam pro alternativní hodnoty TTL a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí Azure CLI 1.0 hello](dns-operations-recordsets-cli-nodejs.md).


## <a name="view-records"></a>Zobrazení záznamů

toolist hello záznamy DNS v zóně, použijte:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>Aktualizace názvových serverů

Jakmile jste uspokojit, že zóna DNS a záznamy byly nastaveny správně, musíte tooconfigure váš název domény toouse názvových serverů Azure DNS hello. To umožňuje jiných uživatelů v Internetu toofind hello svoje záznamy DNS.

Hello názvové servery zóny jsou poskytují hello `azure network dns zone show` příkaz:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Tyto názvové servery musí být nakonfigurovaný u registrátora názvu domény hello (kde jste zakoupili hello název domény). Vám Registrátor nabídne možnost tooset hello až hello názvové servery pro doménu hello. Další informace najdete v tématu [delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Odstranění všech prostředků
 
toodelete vytvořit všechny prostředky v tomto článku, hello proveďte následující krok:

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Další kroky

toolearn Další informace o Azure DNS, najdete v části [přehled Azure DNS](dns-overview.md).

toolearn Další informace o správě zóny DNS v Azure DNS, najdete v části [zóny DNS spravovat v Azure DNS pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

toolearn Další informace o správě záznamů DNS v Azure DNS, najdete v části [sad záznamů a záznamů DNS spravovat v Azure DNS pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

