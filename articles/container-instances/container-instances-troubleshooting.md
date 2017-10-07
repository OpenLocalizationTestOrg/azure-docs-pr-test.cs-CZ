---
title: "aaaTroubleshooting instancí kontejnerů Azure"
description: "Zjistěte, jak tootroubleshoot problémy s instancemi Azure kontejneru"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/03/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: dfec636a0a174c74a6f2e9d9c4da6e871f8d2fda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Řešení potíží s nasazením s instancemi Azure kontejneru

Tento článek ukazuje, jak tootroubleshoot problémy při nasazení tooAzure kontejnery instancí kontejnerů. Popisuje také některé hello běžné problémy, které může spustíte do.

## <a name="getting-diagnostic-events"></a>Získání diagnostických událostí

tooview protokolů z vašeho kódu aplikace v rámci kontejneru, můžete použít hello [az kontejneru protokoly](/cli/azure/container#logs) příkaz. Ale pokud vaše kontejneru není úspěšně nasazena, je třeba tooreview hello diagnostické informace poskytované poskytovatelem prostředků Azure kontejner instancí hello. tooview hello události pro váš kontejner, spusťte následující příkaz hello:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

výstup Hello zahrnuje hello základní vlastnosti kontejneru, společně s událostí nasazení:

```bash
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...

      "events": [
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Běžné problémy při nasazení

Tento účet pro většinu chyb v nasazení existuje několik běžných problémů.

### <a name="unable-toopull-image"></a>Obrázek nelze toopull

Pokud Azure kontejner instancí je nelze toopull bitové kopie na začátku se pokusí po nějakou dobu před selháním nakonec. Pokud nelze načíst obrázek hello, jsou uvedeny událostmi, jako je hello následující:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed toopull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

tooresolve, odstraňte hello kontejneru a opakujte vaše nasazení, platící zvýšené pozornosti, že je správně zadán název bitové kopie hello.

### <a name="container-continually-exits-and-restarts"></a>Ukončení a restartování průběžně kontejneru

Kontejner instancí Azure v současné době podporuje pouze dlouhodobé služby. Pokud vaše kontejneru používá toocompletion a ukončí, automaticky restartuje a znovu spustí. V takovém případě se zobrazí událostmi, jako je následující. Poznámka: Tento kontejner hello úspěšně spustí a potom rychle restartuje. zahrnuje technologie Hello kontejner instancí rozhraní API `retryCount` restartování vlastnost, která ukazuje, jak často konkrétním kontejneru.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> Většina kontejneru bitových kopií pro Linuxových distribucích nastavit prostředí, jako je například bash, jako výchozí příkaz hello. Vzhledem k tomu, že prostředí svoje vlastní není služba dlouho běžící, tyto kontejnery okamžitě ukončit a spadají do smyčku restartování.

### <a name="container-takes-a-long-time-toostart"></a>Kontejner trvá dlouho toostart

Pokud vaše kontejneru trvá dlouho toostart, ale nakonec úspěšná, spusťte prohlížením hello velikost bitové kopie kontejneru. Vzhledem k tomu instancí kontejnerů Azure vrátí kontejner image na vyžádání, je čas spuštění hello zaznamenáte přímo související tooits velikost.

Můžete zobrazit hello velikost bitové kopie kontejneru pomocí příkazového řádku Dockeru hello:

```bash
docker images
```

Výstup:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

malá velikost obrázků klíče tookeeping Hello zajišťuje, že finální image neobsahuje nic, který není nutný za běhu. Toto je s jedním ze způsobů toodo [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Více fáze sestavení bylo snadné tooensure hello finální image obsahuje pouze hello artefakty potřebné pro aplikaci, a nikoli jakákoliv hello navíc obsahu, kterou nebyla nutná v čase vytvoření buildu.

Hello další způsob tooreduce hello dopad hello vyžádání bitové kopie na vaše kontejneru spuštění je toohost hello kontejneru image pomocí hello registru kontejner Azure v hello stejné oblasti, kde chcete toouse Azure kontejner instancí. Hello síťové cestě, která hello kontejneru image musí tootravel, výrazně zkrátit dobu stahování hello se zkrátí.
