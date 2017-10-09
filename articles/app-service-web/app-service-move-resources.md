---
title: "aaaMove prostředky webové aplikace tooanother skupiny prostředků"
description: "Popisuje hello scénáře, kde můžete přesunout webové aplikace a služby, aplikace z jednoho tooanother skupinu prostředků."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 931012fee656b7f8a4b2c225c38739a9171d3b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="supported-move-configurations"></a>Přesunutí podporované konfigurace
Přesunutím prostředků webové aplikace Azure pomocí hello [Resource Manager přesunout prostředky API](../azure-resource-manager/resource-group-move-resources.md).

Azure Web Apps v současné době podporuje následující scénáře přesunutí hello:

* Přesun celého obsahu hello skupiny prostředků (webové aplikace, plány služby app a certifikáty) tooanother skupinu prostředků. 
   > [!Note]
   > Skupina prostředků cílového Hello nesmí obsahovat žádné Microsoft.Web prostředky v tomto scénáři.

* Přesuňte jednotlivé webové aplikace tooa jiné skupině prostředků, při hostování je stále v jejich aktuální plán služby app service (hello aplikace služby plán zůstane ve skupině prostředků staré hello).


