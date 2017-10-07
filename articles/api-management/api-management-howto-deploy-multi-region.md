---
title: aaaDeploy Azure API Management services toomultiple Azure oblasti | Microsoft Docs
description: "Zjistěte, jak toodeploy službě Azure API Management služby toomultiple instanci Azure oblasti."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 04a3e762261237d73a769320a21363f99f1d20cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-an-azure-api-management-service-instance-toomultiple-azure-regions"></a>Jak toodeploy službě Azure API Management služby toomultiple instanci Azure oblastí
API Management podporuje nasazení s více oblast, což umožňuje rozhraní API vydavatelů toodistribute jeden služby API management napříč jakékoli číslo požadované oblasti Azure. To pomáhá zkrátit žádosti o latence, jak jej distribuovat geograficky spotřebitelé rozhraní API a také zvyšuje dostupnost služby, pokud jedna oblast přejde do režimu offline. 

Když služby API Management je původně vytvořen, obsahuje pouze jeden [jednotky] [ unit] a se nachází v jedné oblasti Azure, který je určený jako primární oblasti hello. Pomocí hello portálu Azure můžete snadno přidat další oblasti. API Management server brány je nasazené tooeach oblasti a provoz volání bude směrované toohello nejbližší brány. V oblasti přejde do režimu offline, hello je-li automaticky znovu směrovanou toohello další nejbližší brány. 

> [!IMPORTANT]
> Nasazení s více oblasti je dostupná v hello jenom  **[Premium] [ Premium]**  vrstvy.
> 
> 

## <a name="add-region"></a>Nasazení rozhraní API správy služby instance tooa novou oblast
> [!NOTE]
> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

V hello portálu Azure přejděte toohello **škálování a ceny** stránky pro instanci služby API Management. 

![Karta škálování][api-management-scale-service]

toodeploy tooa novou oblast, klikněte na **+ přidat oblast** hello panelu nástrojů.

![Přidat oblast][api-management-add-region]

Vyberte umístění hello hello rozevíracího seznamu a nastavte hello počet jednotek pro s hello posuvníku.

![Zadejte jednotky][api-management-select-location-units]

Klikněte na tlačítko **přidat** tooplace výběr v tabulce umístění hello. 

Tento postup opakujte, dokud máte nakonfigurované všechny umístění a klikněte na tlačítko **Uložit** z procesu nasazení hello nástrojů toostart hello.

## <a name="remove-region"></a>Odstranit z umístění instanci služby API Management
V hello portálu Azure přejděte toohello **škálování a ceny** stránky pro instanci služby API Management. 

![Karta škálování][api-management-scale-service]

Pro umístění hello chcete tooremove otevřete nabídku kontextu hello pomocí hello **...**  tlačítko na pravém konci hello hello tabulky. Vyberte hello **odstranit** možnost.

![Odebrat oblast][api-management-remove-region]

Potvrdit odstranění hello a klikněte na tlačítko **Uložit** tooapply hello změny.

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance tooa new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

