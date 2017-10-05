---
title: "Ukázka nasazení skriptu rozhraní příkazového řádku Azure Service Fabric"
description: "Nasazení aplikace do clusteru služby Azure Service Fabric pomocí rozhraní příkazového řádku Azure Service Fabric"
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
ms.openlocfilehash: c77ecfccdf7d3e34b0b3133e9c63810a04fb1132
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a><span data-ttu-id="36076-103">Nasazení aplikace do clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="36076-103">Deploy an application to a Service Fabric cluster</span></span>

<span data-ttu-id="36076-104">Tento ukázkový skript zkopíruje balíček aplikace do úložiště bitové kopie clusteru, zaregistruje typ aplikace v clusteru a vytvoří instanci aplikace z typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="36076-104">This sample script copies an application package to a cluster image store, registers the application type in the cluster, and creates an application instance from the application type.</span></span> <span data-ttu-id="36076-105">V tuto chvíli se také vytvořit žádné výchozí služby.</span><span class="sxs-lookup"><span data-stu-id="36076-105">Any default services are also created at this time.</span></span>

<span data-ttu-id="36076-106">V případě potřeby nainstalujte [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="36076-106">If needed, install the [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="36076-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="36076-107">Sample script</span></span>

<span data-ttu-id="36076-108">[!code-sh[hlavní](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "nasazení aplikace do clusteru")]</span><span class="sxs-lookup"><span data-stu-id="36076-108">[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="36076-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="36076-109">Clean up deployment</span></span>

<span data-ttu-id="36076-110">Až budete hotoví, [odebrat](cli-remove-application.md) skriptu lze aplikaci odebrat.</span><span class="sxs-lookup"><span data-stu-id="36076-110">When done, the [remove](cli-remove-application.md) script can be used to remove the application.</span></span> <span data-ttu-id="36076-111">Odebrat skript odstraní instanci aplikace, zrušení registrace typu aplikace a balíček aplikace z úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="36076-111">The remove script deletes the application instance, unregisters the application type, and deletes the application package from the image store.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36076-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36076-112">Next steps</span></span>

<span data-ttu-id="36076-113">Další informace najdete v tématu [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="36076-113">For more information, see the [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="36076-114">Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric najdete v [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="36076-114">Additional Service Fabric CLI samples for Azure Service Fabric can be found in the [Service Fabric CLI samples](../samples-cli.md).</span></span>
