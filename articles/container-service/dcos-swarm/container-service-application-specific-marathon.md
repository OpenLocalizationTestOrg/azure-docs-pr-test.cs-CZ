---
title: "Služba Marathon specifická pro aplikaci nebo pro uživatele | Dokumentace Microsoftu"
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
ms.openlocfilehash: b265763fb5dad240edd710cd8d0fb1079e3a7b51
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="dd1f8-104">Vytvoření služby Marathon specifické pro aplikaci nebo uživatele</span><span class="sxs-lookup"><span data-stu-id="dd1f8-104">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="dd1f8-105">Azure Container Service poskytuje sadu hlavních serverů, na kterých předem konfigurujeme Apache Mesos a Marathon.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-105">Azure Container Service provides a set of master servers on which we preconfigure Apache Mesos and Marathon.</span></span> <span data-ttu-id="dd1f8-106">Ty je možné použít k orchestrování aplikací v clusteru, ale vhodnější je hlavní servery k tomuto účelu nepoužívat.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-106">These can be used to orchestrate your applications on the cluster, but it's best not to use the master servers for this purpose.</span></span> <span data-ttu-id="dd1f8-107">Například při úpravách konfigurace Marathonu je nutné se k přihlásit k samotným hlavním serverům a provést změny přímo na nich – to může vést ke vzniku jedinečných hlavních serverů, které se jen málo liší od těch standardních a je třeba o ně nezávisle pečovat a spravovat je.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-107">For example, tweaking the configuration of Marathon requires logging into the master servers themselves and making changes--this encourages unique master servers that are a little different from the standard and need to be cared for and managed independently.</span></span> <span data-ttu-id="dd1f8-108">Navíc konfigurace, kterou vyžaduje jeden tým, nemusí být optimální pro jiný tým.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-108">Additionally, the configuration required by one team might not be the optimal configuration for another team.</span></span>

<span data-ttu-id="dd1f8-109">V tomto článku si vysvětlíme, jak přidat službu Marathon specifickou pro uživatele nebo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-109">In this article, we'll explain how to add an application or user-specific Marathon service.</span></span>

<span data-ttu-id="dd1f8-110">Protože tato služba bude patřit jedinému uživateli nebo týmu, je možné ji nakonfigurovat, jakkoli si budoucí uživatelé přejí.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-110">Because this service will belong to a single user or team, they are free to configure it in any way that they desire.</span></span> <span data-ttu-id="dd1f8-111">Služba Azure Container Service zajistí, že bude služba nadále v provozu.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-111">Also, Azure Container Service will ensure that the service continues to run.</span></span> <span data-ttu-id="dd1f8-112">V případě selhání služby ji služba Azure Container Service restartuje za vás.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-112">If the service fails, Azure Container Service will restart it for you.</span></span> <span data-ttu-id="dd1f8-113">Ve většině případů si ani nevšimnete, že došlo k výpadku.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-113">Most of the time you won't even notice it had downtime.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd1f8-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd1f8-114">Prerequisites</span></span>
<span data-ttu-id="dd1f8-115">[Nasaďte instanci Azure Container Service](container-service-deployment.md) s typem orchestrátoru DC/OS a [ověřte, že se váš klient může připojit ke clusteru](../container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="dd1f8-115">[Deploy an instance of Azure Container Service](container-service-deployment.md) with orchestrator type DC/OS and  [ensure that your client can connect to your cluster](../container-service-connect.md).</span></span> <span data-ttu-id="dd1f8-116">Také proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-116">Also, do the following steps.</span></span>

[!INCLUDE [install the DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="dd1f8-117">Vytvoření služby Marathon specifické pro aplikaci nebo uživatele</span><span class="sxs-lookup"><span data-stu-id="dd1f8-117">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="dd1f8-118">Začněte tím, že vytvoříte konfigurační soubor JSON, který definuje název vytvářené aplikační služby.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-118">Begin by creating a JSON configuration file that defines the name of the application service that you want to create.</span></span> <span data-ttu-id="dd1f8-119">Zde jako název rozhraní používáme `marathon-alice`.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-119">Here we use `marathon-alice` as the framework name.</span></span> <span data-ttu-id="dd1f8-120">Uložte soubor s názvem, jako je `marathon-alice.json`:</span><span class="sxs-lookup"><span data-stu-id="dd1f8-120">Save the file as something like `marathon-alice.json`:</span></span>

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

<span data-ttu-id="dd1f8-121">Pak pomocí rozhraní příkazového řádku DC/OS nainstalujte instanci Marathonu s možnostmi nastavenými v konfiguračním souboru:</span><span class="sxs-lookup"><span data-stu-id="dd1f8-121">Next, use the DC/OS CLI to install the Marathon instance with the options that are set in your configuration file:</span></span>

```bash
dcos package install --options=marathon-alice.json marathon
```

<span data-ttu-id="dd1f8-122">Nyní byste svou službu `marathon-alice` měli vidět spuštěnou na kartě služeb v uživatelském rozhraní DC/OS.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-122">You should now see your `marathon-alice` service running in the Services tab of your DC/OS UI.</span></span> <span data-ttu-id="dd1f8-123">Pokud chcete k uživatelskému rozhraní přistupovat přímo, bude na adrese `http://<hostname>/service/marathon-alice/`.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-123">The UI will be `http://<hostname>/service/marathon-alice/` if you want to access it directly.</span></span>

## <a name="set-the-dcos-cli-to-access-the-service"></a><span data-ttu-id="dd1f8-124">Nastavte rozhraní příkazového řádku DC/OS pro přístup ke službě</span><span class="sxs-lookup"><span data-stu-id="dd1f8-124">Set the DC/OS CLI to access the service</span></span>
<span data-ttu-id="dd1f8-125">Volitelně je možné následujícím způsobem nakonfigurovat rozhraní příkazového řádku DC/OS pro přístup k této nové službě, a to tím, že nastavíte vlastnost `marathon.url`, aby ukazovala na instanci `marathon-alice`:</span><span class="sxs-lookup"><span data-stu-id="dd1f8-125">You can optionally configure your DC/OS CLI to access this new service by setting the `marathon.url` property to point to the `marathon-alice` instance as follows:</span></span>

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

<span data-ttu-id="dd1f8-126">Pomocí příkazu `dcos config show` můžete ověřit, proti které instanci Marathon pracuje vaše CLI.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-126">You can verify which instance of Marathon that your CLI is working against with the `dcos config show` command.</span></span> <span data-ttu-id="dd1f8-127">Můžete se vrátit k hlavní službě Marathon pomocí příkazu `dcos config unset marathon.url`.</span><span class="sxs-lookup"><span data-stu-id="dd1f8-127">You can revert to using your master Marathon service with the command `dcos config unset marathon.url`.</span></span>

