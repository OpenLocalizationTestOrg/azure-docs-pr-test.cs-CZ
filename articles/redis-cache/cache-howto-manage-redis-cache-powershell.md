---
title: "aaaManage Azure Redis Cache pomocí Azure Powershellu | Microsoft Docs"
description: "Zjistěte, jak tooperform úlohy správy pro Azure Redis Cache pomocí Azure PowerShell."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 1d526ce65c4bc05345cd6c3ff370211ed562cab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Spravovat mezipaměť Redis systému Azure pomocí Azure Powershellu
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

Toto téma ukazuje, jak tooperform běžné úkoly, jako je vytváření, aktualizace a škálovat vaše instance služby Azure Redis Cache, jak tooregenerate přístupových klíčů a jak tooview informace o své mezipaměti. Úplný seznam rutin Powershellu pro Azure Redis Cache najdete v tématu [rutiny Azure Redis Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Další informace o modelu nasazení classic hello najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Požadavky
Pokud jste již Azure PowerShell nainstalovali, musíte mít prostředí Azure PowerShell verze 1.0.0 nebo později. Můžete zkontrolovat hello verzi prostředí Azure PowerShell, který jste nainstalovali pomocí tohoto příkazu na příkazovém řádku prostředí Azure PowerShell hello.

    Get-Module azure | format-table version


Nejdřív musíte být přihlášení tooAzure pomocí tohoto příkazu.

    Login-AzureRmAccount

Zadejte hello e-mailovou adresu účtu Azure a jeho heslo v dialogovém okně hello přihlášení Microsoft Azure.

Dále pokud máte víc předplatných Azure, musíte tooset vašeho předplatného Azure. Seznam odběrů aktuální toosee tento příkaz spustit.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

toospecify hello předplatného, spusťte následující příkaz hello. V následujícím příkladu hello, je název odběru hello `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Než budete moct použít prostředí Windows PowerShell s Azure Resource Manager, je třeba hello následující:

* Prostředí Windows PowerShell, verze 3.0 nebo 4.0. toofind hello verzi Windows PowerShell, zadejte:`$PSVersionTable` a ověřte hodnotu hello `PSVersion` je 3.0 nebo 4.0. tooinstall kompatibilní verze, najdete v části [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

tooget podrobnou nápovědu pro všechny rutiny, které se zobrazí v tomto kurzu, použijte rutinu Get-Help hello.

    Get-Help <cmdlet-name> -Detailed

Například tooget nápovědu pro hello `New-AzureRmRedisCache` rutiny, zadejte:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-tooconnect-tooother-clouds"></a>Jak tooconnect tooother cloudy
Ve výchozím nastavení hello Azure je prostředí `AzureCloud`, což představuje hello instance globální cloudu Azure. tooconnect tooa jinou instanci, použijte hello `Add-AzureRmAccount` s hello `-Environment` nebo -`EnvironmentName` přepínač příkazového řádku s názvem prostředí nebo hello požadované prostředí.

toosee hello seznam dostupných prostředí, spusťte hello `Get-AzureRmEnvironment` rutiny.

### <a name="tooconnect-toohello-azure-government-cloud"></a>tooconnect toohello Cloud vlády Azure
tooconnect toohello Azure Cloud vlády, použijte jednu z následujících příkazů hello.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

nebo

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

toocreate mezipaměti hello Azure Cloud vlády, použijte jednu z následujících umístění hello.

* Vláda USA Virginia
* Iowa vláda USA

Další informace o hello Cloud vlády Azure najdete v tématu [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) a [Průvodce pro vývojáře k Microsoft Azure Government](../azure-government-developer-guide.md).

### <a name="tooconnect-toohello-azure-china-cloud"></a>tooconnect toohello Číně cloudu Azure
tooconnect toohello Číně cloudu Azure, použijte jednu z následujících příkazů hello.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

nebo

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

toocreate mezipaměti hello Číně cloudu Azure, použijte jednu z následujících umístění hello.

* Čína – východ
* Čína – sever

Další informace o hello Číně cloudu Azure najdete v tématu [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/).

### <a name="tooconnect-toomicrosoft-azure-germany"></a>tooconnect tooMicrosoft Azure v Německu
tooconnect tooMicrosoft Azure v Německu, použijte jednu z následujících příkazů hello.

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


nebo

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

toocreate mezipaměti v Microsoft Azure v Německu, použijte jednu z následujících umístění hello.

* Německo – střed
* Německo – severovýchod

Další informace o Microsoft Azure v Německu najdete v tématu [Microsoft Azure v Německu](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Vlastnosti používané pro Azure Redis Cache prostředí PowerShell
Hello následující tabulka obsahuje vlastnosti a popisy pro běžně používané parametry při vytváření a správě vaší instance služby Azure Redis Cache pomocí Azure PowerShell.

| Parametr | Popis | Výchozí |
| --- | --- | --- |
| Name (Název) |Název mezipaměti hello | |
| Umístění |Umístění mezipaměti hello | |
| Název skupiny prostředků |Název skupiny prostředků v mezipaměti které toocreate hello | |
| Velikost |Hello velikost mezipaměti hello. Platné hodnoty jsou: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1GB |
| ShardCount |Hello počet horizontálních oddílů toocreate při vytváření cache ve verzi premium s povoleným clusteringem. Platné hodnoty jsou: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| Skladová jednotka (SKU) |Určuje hello SKU hello mezipaměti. Platné hodnoty jsou: Basic, Standard a Premium |Standard |
| RedisConfiguration |Určuje nastavení konfigurace Redis. Podrobnosti o jednotlivých nastaveních najdete v tématu hello následující [RedisConfiguration vlastnosti](#redisconfiguration-properties) tabulky. | |
| EnableNonSslPort |Určuje, zda je povoleno port bez SSL hello. |False |
| MaxMemoryPolicy |Tento parametr se již nepoužívá – místo toho použijte RedisConfiguration. | |
| StaticIP |Při hostování vaší mezipaměti ve virtuální síti, určuje jedinečnou IP adresu v podsíti hello hello mezipaměti. Pokud není zadaná, jeden z podsítě hello vybrali za vás. | |
| Podsíť |Při hostování vaší mezipaměti ve virtuální síti, určuje název hello hello podsítě v mezipaměti které toodeploy hello. | |
| VirtualNetwork |Při hostování vaší mezipaměti ve virtuální síti, určuje ID prostředku hello hello virtuální síť, ve které toodeploy hello mezipaměti. | |
| Typ_klíče. |Určuje, které přístupový klíč tooregenerate při obnovování přístupové klíče. Platné hodnoty jsou: primární, sekundární | |

### <a name="redisconfiguration-properties"></a>Vlastnosti RedisConfiguration
| Vlastnost | Popis | Cenové úrovně |
| --- | --- | --- |
| Povolit zálohování RDB |Jestli [trvalosti dat Redis](cache-how-to-premium-persistence.md) je povoleno |Pouze Premium |
| RDB úložiště připojovacího řetězce |Hello připojovací řetězec toohello účet úložiště pro [trvalosti dat Redis](cache-how-to-premium-persistence.md) |Pouze Premium |
| četnost záloh RDB |Hello četnost záloh pro [trvalosti dat Redis](cache-how-to-premium-persistence.md) |Pouze Premium |
| vyhrazené maxmemory |Nakonfiguruje hello [paměti vyhrazené](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro procesy bez ukládání do mezipaměti |Standard a Premium |
| maxmemory zásady |Nakonfiguruje hello [zásady vyřazení](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro mezipaměť hello |Všechny cenové úrovně |
| oznámení události keyspace |Nakonfiguruje [oznámení keyspace](cache-configure.md#keyspace-notifications-advanced-settings) |Standard a Premium |
| Hodnota hash-max-ziplist – položky |Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy |Standard a Premium |
| max-ziplist hodnota hash |Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy |Standard a Premium |
| set-max-intset – položky |Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy |Standard a Premium |
| zset-max-ziplist – položky |Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy |Standard a Premium |
| zset-max-ziplist – hodnota |Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy |Standard a Premium |
| databáze |Nakonfiguruje hello počet databází. Tuto vlastnost lze nastavit pouze při vytváření mezipaměti. |Standard a Premium |

## <a name="toocreate-a-redis-cache"></a>toocreate Redis Cache
Nové instance služby Azure Redis Cache lze vytvořit pomocí hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny.

> [!IMPORTANT]
> Hello prvním vytvoření mezipaměti Redis v předplatném pomocí hello portál Azure, portál hello zaregistruje hello `Microsoft.Cache` obor názvů pro toto předplatné. Pokud se pokusíte toocreate hello nejprve Redis cache v předplatném pomocí prostředí PowerShell, nejprve je nutné zaregistrovat tento obor názvů pomocí hello následující příkaz; jinak rutin, jako `New-AzureRmRedisCache` a `Get-AzureRmRedisCache` nezdaří.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

toosee seznam dostupných parametrů a jejich popisy pro `New-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        hello New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of hello redis cache toocreate.

        -ResourceGroupName <String>
            Name of resource group in which toocreate hello redis cache.

        -Location <String>
            Location in which toocreate hello redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, hello default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        -VirtualNetwork <String>
            hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

toocreate mezipaměti s výchozími parametry, spusťte následující příkaz hello.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, a `Location` jsou požadované parametry, ale hello rest jsou volitelné a mají výchozí hodnoty. Předchozí příkaz hello vytvoří instanci standardní SKU Azure Redis Cache s hello zadaný název, umístění a skupinu prostředků, který je 1 GB velikost port bez SSL hello zakázána.

velikost P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), zadejte toocreate cache ve verzi premium P3 (26 GB - 260 GB), nebo P4 (53 GB - 530 GB). tooenable clustering, zadat počet horizontálních pomocí hello `ShardCount` parametr. Hello následující příklad vytvoří mezipaměť premium P1 s 3 horizontálními oddíly. Mezipaměť premium P1 je 6 GB velikost a vzhledem k tomu, že jsme zadali, že tři horizontálních oddílů hello celková velikost je 18 GB (3 × 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

toospecify hodnoty pro hello `RedisConfiguration` parametr, uzavřete hello hodnoty uvnitř `{}` jako klíč/hodnota, jako páry `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Hello následující příklad vytvoří standardní 1 GB mezipaměti s `allkeys-random` maxmemory zásady a keyspace oznámení nakonfigurované s `KEA`. Další informace najdete v tématu [oznámení Keyspace (rozšířené nastavení)](cache-configure.md#keyspace-notifications-advanced-settings) a [paměti zásady](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="tooconfigure-hello-databases-setting-during-cache-creation"></a>databáze hello tooconfigure nastavení v průběhu vytvoření mezipaměti
Hello `databases` nastavení se dá nakonfigurovat jenom během vytváření mezipaměti. Hello následující příklad vytvoří premium P3 (26 GB) mezipaměti s 48 databází pomocí hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Další informace o hello `databases` vlastnost, najdete v části [konfigurace serveru výchozí Azure Redis Cache](cache-configure.md#default-redis-server-configuration). Další informace o vytvoření mezipaměti pomocí hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny, najdete v části hello předchozí [toocreate Redis Cache](#to-create-a-redis-cache) části.

## <a name="tooupdate-a-redis-cache"></a>tooupdate mezipaměti Redis
Instance služby Azure Redis Cache jsou aktualizovány pomocí hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) rutiny.

toosee seznam dostupných parametrů a jejich popisy pro `Set-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        hello Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooupdate.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. hello default value is null and no change will be made toothe
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Hello `Set-AzureRmRedisCache` rutiny lze použít tooupdate vlastnosti, jako `Size`, `Sku`, `EnableNonSslPort`a hello `RedisConfiguration` hodnoty. 

Hello následující příkaz, že aktualizace hello maxmemory zásady pro hello Redis Cache s názvem myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="tooscale-a-redis-cache"></a>tooscale mezipaměti Redis
`Set-AzureRmRedisCache`může být použité tooscale instance mezipaměti Azure Redis při hello `Size`, `Sku`, nebo `ShardCount` jsou upraveny vlastnosti. 

> [!NOTE]
> Škálování mezipaměti pomocí prostředí PowerShell je subjektu toohello stejné omezení a pokyny jako škálování mezipaměti z hello portálu Azure. Je možné škálovat tooa jinou cenovou úroveň s hello následující omezení.
> 
> * Nelze škálovat z vyšší cenová úroveň tooa nižší cenová úroveň.
> * Nelze škálovat od **Premium** mezipaměti dolů tooa **standardní** nebo **základní** mezipaměti.
> * Nelze škálovat od **standardní** mezipaměti dolů tooa **základní** mezipaměti.
> * Je možné škálovat od **základní** mezipaměti tooa **standardní** mezipaměti, ale nemůže změnit velikost hello v hello stejnou dobu. Pokud potřebujete jinou velikost, můžete provést následné velikost toohello potřeby operace škálování.
> * Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti. Musí škálování z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následných škálování operace.
> * Nelze škálovat z větší velikost dolů toohello **C0 (250 MB)** velikost.
> 
> Další informace najdete v tématu [jak tooScale Azure mezipaměti Redis](cache-how-to-scale.md).
> 
> 

Hello následující příklad ukazuje, jak tooscale mezipaměti s názvem `myCache` tooa 2,5 GB mezipaměti. Všimněte si, že tento příkaz lze použít pro základní nebo standardní mezipaměti.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Po vydání tento příkaz, je vrácen stav hello hello mezipaměti (podobně jako toocalling `Get-AzureRmRedisCache`). Všimněte si, že hello `ProvisioningState` je `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Po dokončení operace škálování hello hello `ProvisioningState` změní příliš`Succeeded`. Pokud potřebujete toomake následné škálování operace, jako je například změna ze základní tooStandard a pak změníte velikost hello musíte počkat, dokud dokončení předchozí operace hello nebo se zobrazí následující toohello podobné k chybě.

    Set-AzureRmRedisCache : Conflict: hello resource '...' is not in a stable state, and is currently unable tooaccept hello update request.

## <a name="tooget-information-about-a-redis-cache"></a>tooget informace o mezipaměti Redis
Můžete načíst informace o mezipaměti pomocí hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) rutiny.

toosee seznam dostupných parametrů a jejich popisy pro `Get-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in hello specified resource group or all caches in hello current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCache cmdlet gets hello details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in hello specified resource group.

        If no parameters are given than it will return details about all caches hello current subscription.

    PARAMETERS
        -Name <String>
            hello name of hello cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns hello details for hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns hello details of hello cache specified by Name. If only hello ResourceGroup
            parameter is provided, then details for all caches in hello resource group are returned.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Spustit tooreturn informace o všech mezipaměti v aktuálním předplatném hello, `Get-AzureRmRedisCache` bez parametrů.

    Get-AzureRmRedisCache

Spustit tooreturn informace o všech mezipamětí ve skupině prostředků konkrétní `Get-AzureRmRedisCache` s hello `ResourceGroupName` parametr.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Spustit tooreturn informace o konkrétní mezipaměti, `Get-AzureRmRedisCache` s hello `Name` parametr, který obsahuje název hello hello mezipaměti a hello `ResourceGroupName` parametr s hello skupinu prostředků obsahující mezipaměť.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="tooretrieve-hello-access-keys-for-a-redis-cache"></a>tooretrieve hello přístupové klíče pro Redis cache
tooretrieve hello přístupové klíče pro mezipaměť, můžete použít hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) rutiny.

toosee seznam dostupných parametrů a jejich popisy pro `Get-AzureRmRedisCacheKey`spusťte hello následující příkaz.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets hello accesskeys for hello specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCacheKey cmdlet gets hello access keys for hello specified cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

tooretrieve hello klíče pro mezipaměť, volání hello `Get-AzureRmRedisCacheKey` rutiny a předejte jí hello název mezipaměti hello název skupiny prostředků hello, který obsahuje hello mezipaměti.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="tooregenerate-access-keys-for-your-redis-cache"></a>tooregenerate přístupové klíče pro vaše mezipaměť Redis
tooregenerate hello přístupové klíče pro mezipaměť, můžete použít hello [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) rutiny.

toosee seznam dostupných parametrů a jejich popisy pro `New-AzureRmRedisCacheKey`spusťte hello následující příkaz.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates hello access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        hello New-AzureRmRedisCacheKey cmdlet regenerate hello access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -KeyType <String>
            Specifies whether tooregenerate hello primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When hello Force parameter is provided, hello specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

tooregenerate hello primární nebo sekundární klíč pro mezipaměť, volání hello `New-AzureRmRedisCacheKey` rutiny a předejte jí hello name, skupinu prostředků a zadejte buď `Primary` nebo `Secondary` pro hello `KeyType` parametr. V následujícím příkladu hello hello sekundární přístupový klíč pro mezipaměť je znovu vygenerovat.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want tooregenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="toodelete-a-redis-cache"></a>toodelete mezipaměti Redis
toodelete mezipaměti Redis, použijte hello [odebrat AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) rutiny.

toosee seznam dostupných parametrů a jejich popisy pro `Remove-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        hello Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooremove.

        -ResourceGroupName <String>
            Name of hello resource group of hello cache tooremove.

        -Force
            When hello Force parameter is provided, hello cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes hello cache and does not return any value. If hello PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating hello success of hello operatio

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

V následujícím příkladu hello, hello mezipaměti s názvem `myCache` se odebere.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want tooremove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="tooimport-a-redis-cache"></a>tooimport mezipaměti Redis
Data můžete importovat do instanci služby Azure Redis Cache pomocí hello `Import-AzureRmRedisCache` rutiny.

> [!IMPORTANT]
> Import a Export je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti. Další informace o importu a exportu najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

toosee seznam dostupných parametrů a jejich popisy pro `Import-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs tooAzure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Import-AzureRmRedisCache cmdlet imports data from hello specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into hello cache.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -Force
            When hello Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If hello PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating hello success of the
            operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Hello následující příkaz importuje data z objektu blob hello určeného identifikátorem hello SAS uri do Azure Redis Cache.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="tooexport-a-redis-cache"></a>tooexport mezipaměti Redis
Data můžete exportovat z instance Azure Redis Cache pomocí hello `Export-AzureRmRedisCache` rutiny.

> [!IMPORTANT]
> Import a Export je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti. Další informace o importu a exportu najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

toosee seznam dostupných parametrů a jejich popisy pro `Export-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache tooa specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache tooa specified container.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Prefix <String>
            Prefix toouse for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Hello následující příkaz exportuje data z instance Azure Redis Cache do kontejneru hello určeného identifikátor uri SAS hello.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="tooreboot-a-redis-cache"></a>tooreboot mezipaměti Redis
Restartujete instance služby Azure Redis Cache pomocí hello `Reset-AzureRmRedisCache` rutiny.

> [!IMPORTANT]
> Restart je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti. Další informace o restartování mezipaměti najdete v tématu [mezipaměti správy – restartovat](cache-administration.md#reboot).
> 
> 

toosee seznam dostupných parametrů a jejich popisy pro `Reset-AzureRmRedisCache`spusťte hello následující příkaz.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Reset-AzureRmRedisCache cmdlet reboots hello specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -RebootType <String>
            Which node tooreboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard tooreboot when rebooting a premium cache with clustering enabled.

        -Force
            When hello Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Hello následující příkaz restartuje obou uzlech hello zadaný mezipaměti.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Další kroky
toolearn Další informace o použití prostředí Windows PowerShell s Azure, najdete v části hello následující prostředky:

* [Azure Redis Cache rutiny dokumentaci na webu MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* [Rutiny Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkID=394765): Další toouse hello rutiny v modulu Azure Resource Manager hello.
* [Použití prostředků skupiny prostředků Azure toomanage](../azure-resource-manager/resource-group-template-deploy-portal.md): Zjistěte, jak toocreate a spravovat skupiny prostředků v hello portálu Azure.
* [Azure blog](http://blogs.msdn.com/windowsazure): Další informace o nových funkcích v Azure.
* [Blogu o prostředí Windows PowerShell](http://blogs.msdn.com/powershell): Další informace o nové funkce v prostředí Windows PowerShell.
* ["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): získání reálného tipy a triky z hello komunity prostředí Windows PowerShell.

