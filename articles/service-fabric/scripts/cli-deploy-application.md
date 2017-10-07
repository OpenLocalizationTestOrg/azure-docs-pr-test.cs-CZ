---
title: "aaaAzure Service Fabric rozhraní příkazového řádku skriptu nasazení ukázkové"
description: "Nasazení clusteru Azure Service Fabric tooan aplikace pomocí příkazového řádku Azure Service Fabric hello"
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: aaec7042a4fd7ed32ad706cde70361f23d18fb48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a><span data-ttu-id="0ec14-103">Nasazení clusteru Service Fabric tooa aplikace</span><span class="sxs-lookup"><span data-stu-id="0ec14-103">Deploy an application tooa Service Fabric cluster</span></span>

<span data-ttu-id="0ec14-104">Tento ukázkový skript zkopíruje úložišti aplikace clusteru tooa balíček bitové kopie, zaregistruje typ aplikace hello v clusteru hello a vytvoří instanci aplikace z typu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0ec14-104">This sample script copies an application package tooa cluster image store, registers hello application type in hello cluster, and creates an application instance from hello application type.</span></span> <span data-ttu-id="0ec14-105">V tuto chvíli se také vytvořit žádné výchozí služby.</span><span class="sxs-lookup"><span data-stu-id="0ec14-105">Any default services are also created at this time.</span></span>

<span data-ttu-id="0ec14-106">V případě potřeby nainstalujte hello [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0ec14-106">If needed, install hello [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="0ec14-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0ec14-107">Sample script</span></span>

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0ec14-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="0ec14-108">Clean up deployment</span></span>

<span data-ttu-id="0ec14-109">Až budete hotoví, hello [odebrat](cli-remove-application.md) skriptu lze použít tooremove hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ec14-109">When done, hello [remove](cli-remove-application.md) script can be used tooremove hello application.</span></span> <span data-ttu-id="0ec14-110">skript odebrat Hello odstraní instanci aplikace hello, zrušení registrace typu aplikace hello a balíček aplikace hello z úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="0ec14-110">hello remove script deletes hello application instance, unregisters hello application type, and deletes hello application package from the image store.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ec14-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ec14-111">Next steps</span></span>

<span data-ttu-id="0ec14-112">Další informace najdete v tématu hello [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0ec14-112">For more information, see hello [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="0ec14-113">Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric lze nalézt v hello [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0ec14-113">Additional Service Fabric CLI samples for Azure Service Fabric can be found in hello [Service Fabric CLI samples](../samples-cli.md).</span></span>
