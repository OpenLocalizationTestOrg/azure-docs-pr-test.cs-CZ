---
title: "Referenční dokumentace pro navigace na portálu Azure"
description: "Přečtěte si informace různých uživatelského prostředí pro webové aplikace služby mezi portálu pro správu a portálu Azure"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: jaime-espinosa
ms.openlocfilehash: d1ef6e87d82df0840e49412154df40cc937b320c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a>Referenční dokumentace pro navigace na portálu Azure
Weby Azure se nyní nazývají [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Aktualizujeme všechny naší dokumentaci, aby odrážela tuto změnu názvu a poskytují pokyny k portálu Azure. Dokud nebude tento proces probíhá, můžete tento dokument použít jako vodítko pro práci s webovými aplikacemi na portálu Azure.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-portal"></a>Budoucí portálu Azure Classic
Když si všimnete změny brandingu na portálu Azure Classic, tohoto portálu právě nahrazují portálu Azure. Jak na portálu classic je vyřazován, je fokus pro nový vývoj přejdou k portálu Azure. Vrátí všechny nadcházející nové funkce pro webové aplikace se na portálu Azure. Začít používat portál Azure využívat nejnovější a největší splnit webové aplikace na nabídku.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Rozložení rozdíly mezi portálu Azure Classic a portálu Azure
Na portálu classic jsou uvedeny všech služeb Azure na levé straně. Navigace na portálu classic řídí stromová struktura, kde můžete spustit ze služby a přejděte na každý element. Tato struktura dobře funguje při správě nezávislé komponenty. Aplikace založené na Azure se však kolekce vzájemně propojené služby, a tato struktura stromu není ideální pro práci s kolekcí služby. 

Portál Azure lze snadno vytvářet aplikace pro kompletní s komponentami z více služeb. Na portálu jsou uspořádána jako *cesty*. A *cesty* je řada *okna*, které jsou kontejnery pro různé součásti. Například nastavení automatického škálování pro webové aplikace je *cesty* kterého přejdete několika oknech jak je znázorněno v následujícím příkladu: **webu** (aby nadpis okna nebyla aktualizována použití nového okna terminologie), **nastavení** okně a **škálovat** okno. V příkladu automatické škálování se nastavuje na závisí na využití procesoru, takže je zde také **procento využití procesoru** okno. Součásti v rámci *okna* se nazývají *částí*, který vypadat podobně jako dlaždice. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigace příklad: vytvoření webové aplikace
Vytvoření nové webové aplikace je stále stejně snadná jako 1-2-3. Následující obrázek ukazuje na klasickém portálu a portálu – souběžného prokázat, že není mnohem došlo ke změně počtu kroky potřebné ke zprovoznění webové aplikace a systémem. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Na portálu můžete z nejběžnějších typů webových aplikací, včetně aplikací oblíbených galerie, např. WordPress. Úplný seznam dostupných aplikací, najdete v článku [Azure Marketplace].

Když vytvoříte webovou aplikaci, je třeba zadat adresu URL, plán služby App Service a umístění na portálu, stejně jako se provádí v portálu classic. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Kromě toho portálu umožňuje definovat další obecná nastavení. Například [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) usnadňují prohlížení a správu souvisejících prostředků Azure. 

## <a name="navigation-example-settings-and-features"></a>Příklad navigace: nastavení a funkcí
Nastavení a funkcí jsou nyní logicky seskupeny v jednom okně, ze kterého můžete přejít.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Například můžete vytvořit vlastní domény kliknutím **vlastní domény a SSL** v **nastavení** okno.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Chcete-li nastavit upozornění na monitorování, klikněte na tlačítko **požadavky a chyby** a potom **přidat výstraha**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Chcete-li povolit diagnostiky, klikněte na tlačítko **protokolů diagnostiky** v **nastavení** okno.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

Chcete-li nakonfigurovat nastavení aplikace, klikněte na tlačítko **nastavení aplikace** v **nastavení** okno. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Jiný než název značky mít bylo přejmenováno nebo jinak seskupené, aby bylo snazší najít je pár věcí na portálu. Například dál je snímek odpovídající stránce pro nastavení aplikace (**konfigurace**) na portálu classic.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Další materiály
[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

