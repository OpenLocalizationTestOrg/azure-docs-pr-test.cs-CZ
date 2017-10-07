---
title: "aaaReference pro navigaci hello portálu Azure"
description: "Přečtěte si informace hello jiné uživatelské prostředí pro webové aplikace služby mezi hello portálu pro správu a hello portálu Azure"
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
ms.openlocfilehash: dcf7c1fc17f9a0c31005ad0f2fd53723d2966058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference-for-navigating-hello-azure-portal"></a>Referenční dokumentace pro navigaci hello portálu Azure
Weby Azure se nyní nazývají [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Aktualizujeme všechny naše dokumentace tooreflect tento název změn a tooprovide pokyny pro hello portálu Azure. Až do dokončení tohoto procesu můžete použít tento dokument jako vodítko pro práci s webovými aplikacemi v hello portálu Azure.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="hello-future-of-hello-azure-classic-portal"></a>Hello budoucnosti hello portálu Azure Classic
Když si všimnete hello branding změny hello portálu Azure Classic, je tohoto portálu v hello proces nahrazují hello portálu Azure. Protože portál classic hello je vyřazován, je hello fokus pro nový vývoj posunem toohello portálu Azure. Vrátí všechny nadcházející nové funkce pro webové aplikace se v hello portálu Azure. Začít používat hello portálu Azure tootake výhod hello nejnovější a největší webové aplikace musí mít toooffer.

## <a name="layout-differences-between-hello-azure-classic-portal-and-azure-portal"></a>Rozložení rozdíly mezi hello portálu Azure Classic a portálu Azure
Na portálu classic hello všechny hello Azure services jsou uvedeny na levé straně hello. Navigace na portálu classic hello následuje stromová struktura, kde můžete spustit ze služby hello a přejděte na každý element. Tato struktura dobře funguje při správě nezávislé komponenty. Aplikace založené na Azure se však kolekce vzájemně propojené služby, a tato struktura stromu není ideální pro práci s kolekcí služby. 

Hello portál Azure umožňuje snadno toobuild aplikace na kompletní s komponentami z více služeb. portál Hello jsou uspořádána jako *cesty*. A *cesty* je řada *okna*, které jsou kontejnery pro hello různé součásti. Například nastavení automatického škálování pro webovou aplikaci *cesty* kterého přejdete několika oknech jak ukazuje následující příklad hello: hello **webu** okno (aby nadpis okna ještě nebylo aktualizované toouse Hello nové terminologie), hello **nastavení** okno a hello **škálovat** okno. V příkladu hello automatické škálování se nastavuje toodepend podle využití procesoru, takže je zde také **procento využití procesoru** okno. Hello součásti v rámci hello *okna* se nazývají *částí*, které vypadají dlaždice. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigace příklad: vytvoření webové aplikace
Vytvoření nové webové aplikace je stále stejně snadná jako 1-2-3. Následující obrázek ukazuje hello classic portál a hello portálu vedle sebe toodemonstrate není mnohem změněných v hello počet kroků Hello potřeba tooget webové aplikace nahoru a spuštěna. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Hello portálu můžete z nejběžnějších typů webových aplikací, včetně aplikací oblíbených galerie, např. WordPress hello. Úplný seznam dostupných aplikací, najdete v článku hello [Azure Marketplace].

Když vytvoříte webovou aplikaci, je třeba zadat adresu URL, plán služby App Service a umístění portálu hello stejně jako se provádí v portálu classic hello. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Kromě toho hello portál umožňuje definovat další obecná nastavení. Například [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) nastavit jej jako jednoduchý toosee a správu souvisejících prostředků Azure. 

## <a name="navigation-example-settings-and-features"></a>Příklad navigace: nastavení a funkcí
Všechny hello nastavení a funkce jsou nyní logicky seskupeny v jednom okně, ze kterého můžete přejít.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Například můžete vytvořit vlastní domény kliknutím **vlastní domény a SSL** v hello **nastavení** okno.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

tooset si upozornění, klikněte na tlačítko **požadavky a chyby** a potom **přidat výstraha**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Klikněte na tlačítko tooenable diagnostiky, **protokolů diagnostiky** v hello **nastavení** okno.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

Klikněte na tlačítko Nastavení aplikace tooconfigure, **nastavení aplikace** v hello **nastavení** okno. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Jiný než název značky hello, bylo přejmenováno pár věcí hello portálu nebo jinak seskupené toomake je snazší toofind je. Například dál je snímek hello odpovídající stránce pro nastavení aplikace (**konfigurace**) na portálu classic hello.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Další materiály
[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

