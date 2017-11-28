---
title: "aaaAzure odebrat ukázka serveru Service Fabric rozhraní příkazového řádku skriptu"
description: "Odebrání aplikace z clusteru služby Azure Service Fabric pomocí hello příkazového řádku Azure Service Fabric"
services: service-fabric
documentationcenter: 
author: thraka
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
ms.openlocfilehash: 3ccefd4a04c5b7af71a2f959e11da6e402f25881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="f36ec-103">Odebrání aplikace z clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f36ec-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="f36ec-104">Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f36ec-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from hello cluster.</span></span>  <span data-ttu-id="f36ec-105">Odstraňování instance aplikace hello také odstraní všechny hello spuštění instance služby, které jsou přidružené s touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="f36ec-105">Deleting hello application instance also deletes all hello running service instances associated with that application.</span></span> <span data-ttu-id="f36ec-106">V dalším kroku hello aplikace soubory se odstraní z úložiště bitových kopií hello.</span><span class="sxs-lookup"><span data-stu-id="f36ec-106">Next, hello application files are deleted from hello image store.</span></span> 

<span data-ttu-id="f36ec-107">V případě potřeby nainstalujte hello [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f36ec-107">If needed, install hello [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="f36ec-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f36ec-108">Sample script</span></span>

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a><span data-ttu-id="f36ec-109">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f36ec-109">Next steps</span></span>

<span data-ttu-id="f36ec-110">Další informace najdete v tématu hello [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f36ec-110">For more information, see hello [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="f36ec-111">Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric lze nalézt v hello [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f36ec-111">Additional Service Fabric CLI samples for Azure Service Fabric can be found in hello [Service Fabric CLI samples](../samples-cli.md).</span></span>
