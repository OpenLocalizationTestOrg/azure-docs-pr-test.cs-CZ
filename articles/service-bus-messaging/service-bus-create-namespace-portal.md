---
title: "Vytvoření oboru názvů služby Service Bus na webu Azure Portal | Dokumentace Microsoftu"
description: "Vytvořte obor názvů služby Service Bus pomocí webu Azure Portal."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: c8654ed547a9001e2e968d2a45d990a73ef27d3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a><span data-ttu-id="74560-103">Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="74560-103">Create a Service Bus namespace using the Azure portal</span></span>

<span data-ttu-id="74560-104">Obor názvů je kontejner oboru pro všechny součásti zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="74560-104">A namespace is a scoping container for all messaging components.</span></span> <span data-ttu-id="74560-105">Součástí jednoho oboru názvů může být několik front a témat, přičemž obory názvů často slouží jako kontejnery aplikací.</span><span class="sxs-lookup"><span data-stu-id="74560-105">Multiple queues and topics can reside within a single namespace, and namespaces often serve as application containers.</span></span> <span data-ttu-id="74560-106">Obory názvů služby Service Bus je možné vytvořit dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="74560-106">There are two different ways to create a Service Bus namespace:</span></span>

1. <span data-ttu-id="74560-107">Portál Azure Portal (tento článek)</span><span class="sxs-lookup"><span data-stu-id="74560-107">Azure portal (this article)</span></span>
2. <span data-ttu-id="74560-108">[Šablony Resource Manageru][create-namespace-using-arm]</span><span class="sxs-lookup"><span data-stu-id="74560-108">[Resource Manager templates][create-namespace-using-arm]</span></span>

## <a name="create-a-namespace-in-the-azure-portal"></a><span data-ttu-id="74560-109">Vytvoření oboru názvů na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="74560-109">Create a namespace in the Azure portal</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

<span data-ttu-id="74560-110">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="74560-110">Congratulations!</span></span> <span data-ttu-id="74560-111">Právě jste vytvořili obor názvů zasílání zpráv služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="74560-111">You have now created a Service Bus Messaging namespace.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74560-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74560-112">Next steps</span></span>

<span data-ttu-id="74560-113">Podívejte se na naše [ukázky GitHub][github-samples], které představují některé pokročilejší funkce zasílání zpráv služby Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="74560-113">Check out our [GitHub samples][github-samples], which show some of the more advanced features of Azure Service Bus Messaging.</span></span>

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
