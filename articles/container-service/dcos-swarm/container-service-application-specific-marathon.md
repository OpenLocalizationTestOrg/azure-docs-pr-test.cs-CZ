---
title: "aaaApplication nebo služby Marathon specifické pro uživatele | Microsoft Docs"
description: "Vytvoření služby Marathon specifické pro aplikaci nebo uživatele"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, Marathon, mikroslužby, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 1e6f69ed64e113a3a059788a71ddb57b6d3ad8da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a>Vytvoření služby Marathon specifické pro aplikaci nebo uživatele
Azure Container Service poskytuje sadu hlavních serverů, na kterých předem konfigurujeme Apache Mesos a Marathon. Mohou to být použité tooorchestrate aplikace na hello clusteru, ale osvědčené není toouse hello hlavních serverů pro tento účel. Například postupně je upravujte hello konfigurace Marathonu vyžaduje protokolování do hello hlavních serverů, sami a provádění změn – to může vést ke vzniku jedinečných hlavních serverů, které jsou mírně liší od standardní hello a nutnost toobe postaráno a spravované nezávisle. Kromě toho konfigurace hello vyžaduje jeden tým, nemusí být hello optimální konfigurace pro jiný tým.

V tomto článku vám objasníme, jak tooadd služby Marathon specifické pro uživatele nebo aplikace.

Protože tato služba bude patřit jedinému uživateli tooa nebo týmu, jsou volné tooconfigure ho žádným způsobem, který chtějí mít. Navíc Azure Container Service zajistí, že hello služba bude stále toorun. Pokud služba hello selže, Azure Container Service ho restartuje. Většinu času hello si ani nevšimnete, že došlo k výpadku.

## <a name="prerequisites"></a>Požadavky
[Nasaďte instanci Azure Container Service](container-service-deployment.md) s nástrojem orchestrator zadejte DC/OS a [ověřte, zda váš klient může připojit tooyour cluster](../container-service-connect.md). Navíc hello následující kroky.

[!INCLUDE [install hello DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Vytvoření služby Marathon specifické pro aplikaci nebo uživatele
Začněte tím, že vytvoření konfiguračního souboru JSON, který definuje název hello hello aplikace služby, které chcete toocreate. Tady používáme `marathon-alice` jako název rozhraní hello. Uložte soubor hello jako něco podobného jako `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Potom použijte instanci Marathonu hello rozhraní příkazového řádku DC/OS tooinstall hello s hello možnosti, které jsou nastaveny v konfiguračním souboru:

```bash
dcos package install --options=marathon-alice.json marathon
```

Teď byste měli vidět vaše `marathon-alice` služby spuštěné v kartě služeb hello uživatelské rozhraní DC/OS. Hello uživatelského rozhraní bude `http://<hostname>/service/marathon-alice/` Pokud chcete, aby tooaccess přímo.

## <a name="set-hello-dcos-cli-tooaccess-hello-service"></a>Nastavení rozhraní příkazového řádku DC/OS tooaccess hello hello služby
Volitelně můžete nakonfigurovat vaše rozhraní příkazového řádku DC/OS tooaccess této nové službě podle nastavení hello `marathon.url` vlastnost toopoint toohello `marathon-alice` instance následujícím způsobem:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Můžete ověřit, kterou instancí Marathonu, který vaše rozhraní příkazového řádku pracuje s hello `dcos config show` příkaz. Můžete se vrátit toousing hlavní služby Marathon pomocí příkazu hello `dcos config unset marathon.url`.

