---
title: "aaaManage Azure Redis Cache pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello rozhraní příkazového řádku Azure na jakékoli platformě, jak toouse ho tooconnect tooyour účet Azure a jak toocreate a správě mezipaměti Redis z hello rozhraní příkazového řádku Azure."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: e8ee30090133e6b4edea93dcd13fd171e75989bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-azure-redis-cache-using-hello-azure-command-line-interface-azure-cli"></a>Jak toocreate a spravovat Azure Redis Cache pomocí hello rozhraní příkazového řádku Azure (Azure CLI)
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
>
>

Hello rozhraní příkazového řádku Azure je skvělým způsobem toomanage infrastruktury Azure z libovolné platformě. Tento článek ukazuje, jak toocreate a správě vaší instancí Azure Redis Cache pomocí hello rozhraní příkazového řádku Azure.

> [!NOTE]
> Tento článek se týká tooa předchozí verzi rozhraní příkazového řádku Azure. Hello nejnovější Azure CLI 2.0 ukázkové skripty, najdete v části [ukázky rozhraní příkazového řádku služby Azure Redis cache](cli-samples.md).
> 
> 

## <a name="prerequisites"></a>Požadavky
toocreate a spravovat instance služby Azure Redis Cache pomocí rozhraní příkazového řádku Azure, je třeba provést následující kroky hello.

* Musí mít účet Azure. Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) za pár chvil.
* [Instalace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).
* Připojit vaše instalace rozhraní příkazového řádku Azure osobní účet Azure, nebo s pracovní nebo školní účet Azure a přihlásit z hello rozhraní příkazového řádku Azure pomocí hello `azure login` příkaz. toounderstand hello rozdíly a vybrat, najdete v části [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure (Azure CLI) hello](../xplat-cli-connect.md).
* Před spuštěním, žádný z následujících příkazů hello, přepnout do režimu Resource Manager hello rozhraní příkazového řádku Azure spuštěním hello `azure config mode arm` příkaz. Další informace najdete v tématu [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky](../xplat-cli-azure-resource-manager.md).

## <a name="redis-cache-properties"></a>Vlastnosti mezipaměti redis
Hello následující vlastnosti se používají při vytváření nebo aktualizaci instance služby Redis Cache.

| Vlastnost | Přepínač | Popis |
| --- | --- | --- |
| jméno |-n, – název |Název hello Redis Cache. |
| skupina prostředků |-g, – skupinu prostředků |Název skupiny prostředků hello. |
| location |-l, – umístění |Mezipaměť toocreate umístění. |
| Velikost |-z, – velikost |Velikost hello Redis Cache. Platné hodnoty: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| SKU |-x, – sku |Redis SKU. By měl být jeden z: [Basic, Standard, Premium] |
| EnableNonSslPort |-e, – enable bez--port ssl |Vlastnost EnableNonSslPort hello Redis Cache. Pokud chcete Port tooenable hello bez SSL pro mezipaměť přidat tento příznak |
| Konfigurace redis |-c, – konfigurací redis |Redis konfigurace. Zadejte řetězec formátu JSON konfigurace klíčů a hodnot v tomto poli. Formát: "{" ":""," ":" "}" |
| Konfigurace redis |-f, – redis. konfigurační soubor |Redis konfigurace. Zadejte hello cestu k souboru obsahující konfigurační klíče a hodnoty v tomto poli. Formát pro vstupní soubor hello: {"": "","": ""} |
| Počet horizontálních |-r, – počet horizontálních |Počet toocreate horizontálních oddílů v clusteru Cache ve verzi Premium s clusteringem. |
| Virtual Network |-v, – virtuální sítě |Při hostování vaší mezipaměti ve virtuální síti, určuje hello přesný ARM ID prostředku hello virtuální sítě toodeploy hello redis v mezipaměti. Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Typ klíče |-t, – typ klíče |Typ klíče toorenew. Platné hodnoty: [primární, sekundární] |
| StaticIP |-p, – statické ip < statické ip > |Při hostování vaší mezipaměti ve virtuální síti, určuje jedinečnou IP adresu v podsíti hello hello mezipaměti. Pokud není zadaná, jeden z podsítě hello vybrali za vás. |
| Podsíť |t, – podsítě<subnet> |Při hostování vaší mezipaměti ve virtuální síti, určuje název hello hello podsítě v mezipaměti které toodeploy hello. |
| VirtualNetwork |-v, – virtuální sítě < virtuálních sítí > |Při hostování vaší mezipaměti ve virtuální síti, určuje hello přesný ARM ID prostředku hello virtuální sítě toodeploy hello redis v mezipaměti. Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Předplatné |-s, – předplatné |identifikátor předplatného Hello. |

## <a name="see-all-redis-cache-commands"></a>Zobrazit všechny příkazy pro Redis Cache
toosee všechny příkazy Redis Cache a jejich parametrů, použijte hello `azure rediscache -h` příkaz.

    C:\>azure rediscache -h
    help:    Commands toomanage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew hello authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Vytvoření Redis Cache
toocreate Redis Cache, hello použijte následující příkaz:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Další informace o tomto příkazu Spustit hello `azure rediscache create -h` příkaz.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -l, --location <location>                                Location toocreate cache.
    help:      -z, --size <size>                                        Size of hello Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of hello Redis Cache. Add this flag if you want tooenable hello Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here. Format for hello file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards toocreate on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Odstraňte existující Redis Cache
toodelete Redis Cache, hello použijte následující příkaz:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Další informace o tomto příkazu Spustit hello `azure rediscache delete -h` příkaz.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which hello cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Zobrazí seznam všech mezipaměti Redis v rámci předplatného nebo skupinu prostředků
použít všechny mezipaměti Redis v rámci předplatného nebo skupinu prostředků, toolist hello následující příkaz:

    azure rediscache list [options]

Další informace o tomto příkazu Spustit hello `azure rediscache list -h` příkaz.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Zobrazit vlastnosti existující Redis Cache
tooshow vlastnosti existující Redis Cache, hello použijte následující příkaz:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Další informace o tomto příkazu Spustit hello `azure rediscache show -h` příkaz.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a>Změňte nastavení existující Redis Cache
nastavení toochange existující Redis Cache, hello použijte následující příkaz:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Další informace o tomto příkazu Spustit hello `azure rediscache set -h` příkaz.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-hello-authentication-key-for-an-existing-redis-cache"></a>Obnovit hello ověřovací klíč pro existující Redis Cache
toorenew hello ověřovací klíč pro existující Redis Cache, hello použijte následující příkaz:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Zadejte `Primary` nebo `Secondary` pro `key-type`.

Další informace o tomto příkazu Spustit hello `azure rediscache renew-key -h` příkaz.

    C:\>azure rediscache renew-key -h
    help:    Renew hello authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key toorenew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Seznam primární a sekundární klíče existující mezipaměti Redis
toolist primární a sekundární klíče existující Redis Cache, použijte následující příkaz hello:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Další informace o tomto příkazu Spustit hello `azure rediscache list-keys -h` příkaz.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
