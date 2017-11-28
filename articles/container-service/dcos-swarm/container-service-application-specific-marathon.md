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
# <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="ce6bd-104">Vytvoření služby Marathon specifické pro aplikaci nebo uživatele</span><span class="sxs-lookup"><span data-stu-id="ce6bd-104">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="ce6bd-105">Azure Container Service poskytuje sadu hlavních serverů, na kterých předem konfigurujeme Apache Mesos a Marathon.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-105">Azure Container Service provides a set of master servers on which we preconfigure Apache Mesos and Marathon.</span></span> <span data-ttu-id="ce6bd-106">Mohou to být použité tooorchestrate aplikace na hello clusteru, ale osvědčené není toouse hello hlavních serverů pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-106">These can be used tooorchestrate your applications on hello cluster, but it's best not toouse hello master servers for this purpose.</span></span> <span data-ttu-id="ce6bd-107">Například postupně je upravujte hello konfigurace Marathonu vyžaduje protokolování do hello hlavních serverů, sami a provádění změn – to může vést ke vzniku jedinečných hlavních serverů, které jsou mírně liší od standardní hello a nutnost toobe postaráno a spravované nezávisle.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-107">For example, tweaking hello configuration of Marathon requires logging into hello master servers themselves and making changes--this encourages unique master servers that are a little different from hello standard and need toobe cared for and managed independently.</span></span> <span data-ttu-id="ce6bd-108">Kromě toho konfigurace hello vyžaduje jeden tým, nemusí být hello optimální konfigurace pro jiný tým.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-108">Additionally, hello configuration required by one team might not be hello optimal configuration for another team.</span></span>

<span data-ttu-id="ce6bd-109">V tomto článku vám objasníme, jak tooadd služby Marathon specifické pro uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-109">In this article, we'll explain how tooadd an application or user-specific Marathon service.</span></span>

<span data-ttu-id="ce6bd-110">Protože tato služba bude patřit jedinému uživateli tooa nebo týmu, jsou volné tooconfigure ho žádným způsobem, který chtějí mít.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-110">Because this service will belong tooa single user or team, they are free tooconfigure it in any way that they desire.</span></span> <span data-ttu-id="ce6bd-111">Navíc Azure Container Service zajistí, že hello služba bude stále toorun.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-111">Also, Azure Container Service will ensure that hello service continues toorun.</span></span> <span data-ttu-id="ce6bd-112">Pokud služba hello selže, Azure Container Service ho restartuje.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-112">If hello service fails, Azure Container Service will restart it for you.</span></span> <span data-ttu-id="ce6bd-113">Většinu času hello si ani nevšimnete, že došlo k výpadku.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-113">Most of hello time you won't even notice it had downtime.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce6bd-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce6bd-114">Prerequisites</span></span>
<span data-ttu-id="ce6bd-115">[Nasaďte instanci Azure Container Service](container-service-deployment.md) s nástrojem orchestrator zadejte DC/OS a [ověřte, zda váš klient může připojit tooyour cluster](../container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="ce6bd-115">[Deploy an instance of Azure Container Service](container-service-deployment.md) with orchestrator type DC/OS and  [ensure that your client can connect tooyour cluster](../container-service-connect.md).</span></span> <span data-ttu-id="ce6bd-116">Navíc hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-116">Also, do hello following steps.</span></span>

[!INCLUDE [install hello DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="ce6bd-117">Vytvoření služby Marathon specifické pro aplikaci nebo uživatele</span><span class="sxs-lookup"><span data-stu-id="ce6bd-117">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="ce6bd-118">Začněte tím, že vytvoření konfiguračního souboru JSON, který definuje název hello hello aplikace služby, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-118">Begin by creating a JSON configuration file that defines hello name of hello application service that you want toocreate.</span></span> <span data-ttu-id="ce6bd-119">Tady používáme `marathon-alice` jako název rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-119">Here we use `marathon-alice` as hello framework name.</span></span> <span data-ttu-id="ce6bd-120">Uložte soubor hello jako něco podobného jako `marathon-alice.json`:</span><span class="sxs-lookup"><span data-stu-id="ce6bd-120">Save hello file as something like `marathon-alice.json`:</span></span>

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

<span data-ttu-id="ce6bd-121">Potom použijte instanci Marathonu hello rozhraní příkazového řádku DC/OS tooinstall hello s hello možnosti, které jsou nastaveny v konfiguračním souboru:</span><span class="sxs-lookup"><span data-stu-id="ce6bd-121">Next, use hello DC/OS CLI tooinstall hello Marathon instance with hello options that are set in your configuration file:</span></span>

```bash
dcos package install --options=marathon-alice.json marathon
```

<span data-ttu-id="ce6bd-122">Teď byste měli vidět vaše `marathon-alice` služby spuštěné v kartě služeb hello uživatelské rozhraní DC/OS.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-122">You should now see your `marathon-alice` service running in hello Services tab of your DC/OS UI.</span></span> <span data-ttu-id="ce6bd-123">Hello uživatelského rozhraní bude `http://<hostname>/service/marathon-alice/` Pokud chcete, aby tooaccess přímo.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-123">hello UI will be `http://<hostname>/service/marathon-alice/` if you want tooaccess it directly.</span></span>

## <a name="set-hello-dcos-cli-tooaccess-hello-service"></a><span data-ttu-id="ce6bd-124">Nastavení rozhraní příkazového řádku DC/OS tooaccess hello hello služby</span><span class="sxs-lookup"><span data-stu-id="ce6bd-124">Set hello DC/OS CLI tooaccess hello service</span></span>
<span data-ttu-id="ce6bd-125">Volitelně můžete nakonfigurovat vaše rozhraní příkazového řádku DC/OS tooaccess této nové službě podle nastavení hello `marathon.url` vlastnost toopoint toohello `marathon-alice` instance následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ce6bd-125">You can optionally configure your DC/OS CLI tooaccess this new service by setting hello `marathon.url` property toopoint toohello `marathon-alice` instance as follows:</span></span>

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

<span data-ttu-id="ce6bd-126">Můžete ověřit, kterou instancí Marathonu, který vaše rozhraní příkazového řádku pracuje s hello `dcos config show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-126">You can verify which instance of Marathon that your CLI is working against with hello `dcos config show` command.</span></span> <span data-ttu-id="ce6bd-127">Můžete se vrátit toousing hlavní služby Marathon pomocí příkazu hello `dcos config unset marathon.url`.</span><span class="sxs-lookup"><span data-stu-id="ce6bd-127">You can revert toousing your master Marathon service with hello command `dcos config unset marathon.url`.</span></span>

