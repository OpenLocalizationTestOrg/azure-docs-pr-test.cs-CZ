---
title: "aaaPoint společnosti internetové domény tooa název domény Traffic Manageru | Microsoft Docs"
description: "Tento článek vám pomůže bodu domény název tooa Traffic Manager název domény vaší společnosti."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 84c428f60a1dc70452bf957d98a68c95e0b51715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="point-a-company-internet-domain-tooan-azure-traffic-manager-domain"></a>Nasměrování společnosti internetové domény tooan Azure Traffic Manager domény

Když vytvoříte profil Traffic Manageru, Azure mu automaticky přiřadí název DNS. toouse název ze zóny DNS vytvořit záznam CNAME DNS, který mapuje název domény toohello vašeho profilu Traffic Manageru. Název domény Traffic Manageru hello můžete najít v hello **Obecné** části na stránce konfigurace hello hello profil služby Traffic Manager.

Například toopoint název www.contoso.com toohello contoso.trafficmanager.net název DNS Traffic Manageru, vytvořili byste hello následující záznam prostředku DNS:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Veškeré požadavky na provoz příliš*www.contoso.com* získat směrované příliš*contoso.trafficmanager.net*.

> [!IMPORTANT]
> Nemůže odkazovat domény druhé úrovně, jako například *contoso.com*, toohello doménu Traffic Manageru. Standardy protokolu DNS nepovolují záznamy CNAME pro názvy domén druhé úrovně.

## <a name="next-steps"></a>Další kroky

* [Metody směrování Traffic Manageru](traffic-manager-routing-methods.md)
* [Traffic Manager – Zakázání, povolení nebo odstranění profilu](disable-enable-or-delete-a-profile.md)
* [Traffic Manager – Zakázání nebo povolení koncového bodu](disable-or-enable-an-endpoint.md)
