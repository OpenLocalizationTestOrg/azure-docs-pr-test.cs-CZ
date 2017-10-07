---
title: aaaOverview Azure DNS | Microsoft Docs
description: "Přehled systému DNS, který je hostitelem služby v Microsoft Azure. Hostitel doménu v Microsoft Azure."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: a10f87c488356469e9c04aabde31129049563891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-overview"></a>Přehled Azure DNS

Hello systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) k webu nebo službě název tooits IP adresu. Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure. Hostování domény do Azure, můžete spravovat DNS záznamů pomocí hello stejné přihlašovací údaje, rozhraní API, nástroje a fakturace jako jinými službami Azure.

![Přehled systému DNS](./media/dns-overview/scenario.png)

## <a name="features"></a>Funkce

* **Spolehlivost a výkon** -domén DNS v Azure DNS jsou hostované v Azure globální síti názvových serverů DNS. Používáme všesměrového vysílání sítě tak, aby každý dotaz DNS je zodpovězen pomocí serveru DNS nejbližší dostupné hello. To poskytuje vysoký výkon a vysokou dostupnost vaší domény.

* **Bezproblémová integrace** – služba Azure DNS hello lze použít toomanage záznamy DNS pro služeb Azure a může být použité tooprovide DNS pro vaše externí prostředky. Azure DNS je integrován v hello portál Azure a používá hello stejné přihlašovací údaje, fakturaci a podporu kontrakt jako jinými službami Azure.

* **Zabezpečení** -hello služba Azure DNS je založena na Azure Resource Manager. Jako takový výhody z funkce služby Správce prostředků například řízení přístupu na základě rolí, protokoly auditu a uzamčení prostředků. Doménách a záznamech můžete spravovat prostřednictvím hello portál Azure, rutin prostředí Azure PowerShell a hello rozhraní příkazového řádku Azure napříč platformami. Aplikace, které potřebují Automatická správa DNS může integrovat hello service pomocí hello REST API a sady SDK.

Azure DNS aktuálně nepodporuje nákup názvů domén. Pokud chcete, aby toopurchase domény, je třeba toouse doménového registrátora názvu domény třetí strany. Hello Registrátor obvykle účtuje malý roční poplatek. pro správu záznamů DNS, může být Hello domény pak hostovaný v Azure DNS. V tématu [delegovat tooAzure domény DNS](dns-domain-delegation.md) podrobnosti.

## <a name="pricing"></a>Ceny

Fakturace DNS je založena na hello počet zóny DNS hostované v Azure a číslem hello dotazů DNS. Další informace o cenách najdete na toolearn [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy k Azure DNS, najdete v části hello [Azure DNS – nejčastější dotazy](dns-faq.md).

## <a name="next-steps"></a>Další kroky

Další informace o zóny DNS a záznamy, navštivte stránky: [DNS zóny a zaznamenává přehled](dns-zones-records.md).

Zjistěte, jak příliš[vytvořit zónu DNS](./dns-getstarted-create-dnszone-portal.md) v Azure DNS.

Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure.

