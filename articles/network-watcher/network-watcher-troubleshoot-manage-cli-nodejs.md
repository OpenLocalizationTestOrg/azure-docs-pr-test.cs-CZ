---
title: "aaaTroubleshoot brány virtuální sítě Azure a připojení - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toouse hello sledovací proces sítě Azure vyřešit potíže s Azure CLI 1.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: a0511689d773f9c7216b0fe5470e27f754eb600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a>Řešení potíží s brány virtuální sítě a připojení pomocí Azure sítě sledovacích procesů Azure CLI 1.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure. Jeden z těchto funkcí je prostředek řešení potíží. Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.

Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux. 

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

Seznam podporovaných brány typy návštěvě [typy podporované brány](/network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Přehled

Řešení potíží s prostředků poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení. Když je žádosti tooresource řešení potíží s protokoly Probíhá dotaz a prověřovány. Po dokončení kontroly hello výsledky se vrátí. Požadavky na zdroje, řešení problémů s požadavky jsou dlouho spuštěný, což může trvat několik minut tooreturn výsledku. Hello protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Načtení připojení brány virtuální sítě

V tomto příkladu je se řešení potíží s prostředků spustili na připojení. Můžete také předat ji bránu virtuální sítě. Hello následující rutiny uvádí připojení vpn hello ve skupině prostředků.

```azurecli
azure network vpn-connection list -g resourceGroupName
```

Můžete také spustit příkaz toosee hello hello připojení v předplatném.

```azurecli
azure network vpn-connection list -s subscription
```

Jakmile máte hello název hello připojení, můžete spustit tento příkaz tooget jeho Id prostředku:

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Řešení potíží s prostředků vrátí data o stavu hello hello prostředku, uloží zároveň protokoly tooa úložiště účet toobe zkontrolovat. V tomto kroku vytvoříme účet úložiště, pokud existuje stávající účet úložiště můžete ji použít.

1. Vytvořit účet úložiště hello

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. Získání klíče účtu úložiště hello

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. Vytvoření kontejneru hello

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Spustit Poradce při potížích sledovací proces sítě prostředků

Řešení potíží s prostředky s hello `network watcher troubleshoot` rutiny. Jsme předat skupinu prostředků hello hello rutiny, hello název hello sledovací proces sítě, hello Id připojení hello hello Id účtu úložiště hello a hello cesta toohello blob toostore hello řešení potíží s výsledky v.

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

Po spuštění rutiny hello sledovací proces sítě zkontroluje stav hello tooverify hello prostředků. Se vrátí výsledky hello toohello prostředí a ukládá protokoly hello výsledků v zadaný účet úložiště hello.

## <a name="understanding-hello-results"></a>Seznámení s výsledky hello

text Hello akce obsahuje obecné pokyny k jak tooresolve hello problém. Pokud může být akce hello problému, je k dispozici odkaz s další pokyny. V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.  Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)

Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Jiný nástroj, který je možné je Storage Explorer. Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)

## <a name="next-steps"></a>Další kroky

Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.
