---
title: "Ukázka odebrat Azure Service Fabric rozhraní příkazového řádku skriptu"
description: "Odebrání aplikace z clusteru služby Azure Service Fabric pomocí rozhraní příkazového řádku Azure Service Fabric"
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
ms.openlocfilehash: d86f195d2c37a71e476c5ba4eec040dd46931d23
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="b2f46-103">Odebrání aplikace z clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b2f46-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="b2f46-104">Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2f46-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from the cluster.</span></span>  <span data-ttu-id="b2f46-105">Odstranění instance aplikace na také odstraní všechny spuštěné služby instance přidružené s touto aplikací.</span><span class="sxs-lookup"><span data-stu-id="b2f46-105">Deleting the application instance also deletes all the running service instances associated with that application.</span></span> <span data-ttu-id="b2f46-106">Soubory aplikace v dalším kroku se odstraní z úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="b2f46-106">Next, the application files are deleted from the image store.</span></span> 

<span data-ttu-id="b2f46-107">V případě potřeby nainstalujte [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b2f46-107">If needed, install the [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="b2f46-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b2f46-108">Sample script</span></span>

<span data-ttu-id="b2f46-109">[!code-sh[hlavní](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "odebrání aplikace z clusteru")]</span><span class="sxs-lookup"><span data-stu-id="b2f46-109">[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2f46-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2f46-110">Next steps</span></span>

<span data-ttu-id="b2f46-111">Další informace najdete v tématu [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b2f46-111">For more information, see the [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="b2f46-112">Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric najdete v [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b2f46-112">Additional Service Fabric CLI samples for Azure Service Fabric can be found in the [Service Fabric CLI samples](../samples-cli.md).</span></span>
