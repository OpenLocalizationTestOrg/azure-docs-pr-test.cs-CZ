---
title: "Nasměrování internetové domény společnosti na název domény Traffic Manageru | Dokumentace Microsoftu"
description: "Tento článek vám pomůže nasměrovat název domény společnosti na název domény Traffic Manageru."
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
ms.openlocfilehash: 0322b3510cfd4f94031d8c1db8f1cc032b997fa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a><span data-ttu-id="8734b-103">Nasměrování internetové domény společnosti na doménu Azure Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="8734b-103">Point a company Internet domain to an Azure Traffic Manager domain</span></span>

<span data-ttu-id="8734b-104">Když vytvoříte profil Traffic Manageru, Azure mu automaticky přiřadí název DNS.</span><span class="sxs-lookup"><span data-stu-id="8734b-104">When you create a Traffic Manager profile, Azure automatically assigns a DNS name for that profile.</span></span> <span data-ttu-id="8734b-105">Pokud chcete použít název z vaší zóny DNS, vytvořte záznam DNS CNAME, který se namapuje na název domény vašeho profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8734b-105">To use a name from your DNS zone, create a CNAME DNS record that maps to the domain name of your Traffic Manager profile.</span></span> <span data-ttu-id="8734b-106">Název domény Traffic Manageru najdete v části **Obecné** na stránce Konfigurace pro příslušný profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8734b-106">You can find the Traffic Manager domain name in the **General** section on the Configuration page of the Traffic Manager profile.</span></span>

<span data-ttu-id="8734b-107">Pokud byste například chtěli nasměrovat název www.contoso.com na název DNS Traffic Manageru contoso.trafficmanager.net, aktualizovali byste následující záznam prostředku DNS:</span><span class="sxs-lookup"><span data-stu-id="8734b-107">For example, to point name www.contoso.com to the Traffic Manager DNS name contoso.trafficmanager.net, you would create the following DNS resource record:</span></span>

    www.contoso.com IN CNAME contoso.trafficmanager.net

<span data-ttu-id="8734b-108">Veškeré žádosti o přenos na *www.contoso.com* se budou směrovat na *contoso.trafficmanager.net*.</span><span class="sxs-lookup"><span data-stu-id="8734b-108">All traffic requests to *www.contoso.com* get directed to *contoso.trafficmanager.net*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8734b-109">Doménu druhé úrovně, například *contoso.com*, nelze nasměrovat na doménu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8734b-109">You cannot point a second-level domain, such as *contoso.com*, to the Traffic Manager domain.</span></span> <span data-ttu-id="8734b-110">Standardy protokolu DNS nepovolují záznamy CNAME pro názvy domén druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="8734b-110">DNS protocol standards do not allow CNAME records for second-level domain names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8734b-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8734b-111">Next steps</span></span>

* [<span data-ttu-id="8734b-112">Metody směrování Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="8734b-112">Traffic Manager routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="8734b-113">Traffic Manager – Zakázání, povolení nebo odstranění profilu</span><span class="sxs-lookup"><span data-stu-id="8734b-113">Traffic Manager - Disable, enable or delete a profile</span></span>](disable-enable-or-delete-a-profile.md)
* [<span data-ttu-id="8734b-114">Traffic Manager – Zakázání nebo povolení koncového bodu</span><span class="sxs-lookup"><span data-stu-id="8734b-114">Traffic Manager - Disable or enable an endpoint</span></span>](disable-or-enable-an-endpoint.md)
