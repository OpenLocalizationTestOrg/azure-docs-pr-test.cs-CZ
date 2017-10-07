---
title: "aaaHow toocreate oboru názvů Service Bus v hello portálu Azure | Microsoft Docs"
description: "Vytvoření oboru názvů Service Bus pomocí hello portálu Azure."
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
ms.openlocfilehash: d8907e7e4a804056f6d66d5a177d9ace967ed2ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-using-hello-azure-portal"></a>Vytvoření oboru názvů Service Bus pomocí hello portálu Azure

Obor názvů je kontejner oboru pro všechny součásti zasílání zpráv. Součástí jednoho oboru názvů může být několik front a témat, přičemž obory názvů často slouží jako kontejnery aplikací. Existují dva různé způsoby toocreate oboru názvů Service Bus:

1. Portál Azure Portal (tento článek)
2. [Šablony Resource Manageru][create-namespace-using-arm]

## <a name="create-a-namespace-in-hello-azure-portal"></a>Vytvoření oboru názvů v hello portálu Azure

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Blahopřejeme! Právě jste vytvořili obor názvů zasílání zpráv služby Service Bus.

## <a name="next-steps"></a>Další kroky

Podívejte se na naše [Githubu ukázky][github-samples], které představují některé hello pokročilejší funkce Azure zasílání zpráv Service Bus.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
