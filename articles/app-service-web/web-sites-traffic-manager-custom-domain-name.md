---
title: "aaaConfigure vlastní název domény pro webovou aplikaci v Azure App Service, která používá Traffic Manager pro vyrovnávání zatížení."
description: "Vlastní název domény pro použití webové aplikace ve službě Azure App Service, která zahrnuje Traffic Manager pro vyrovnávání zatížení."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: dfde5fc6b445b30b10e03dcb03e8d072130d9377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service pomocí Traffic Manager
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Tento článek obsahuje obecné pokyny pro používání vlastního názvu domény službou Azure App Service, které pomocí služby Traffic Manager pro vyrovnávání zatížení.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Principy záznamy DNS
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurace webových aplikacích pro standardní režim
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Přidejte záznam DNS pro vaši vlastní doménu.
> [!NOTE]
> Pokud jste zakoupili domény prostřednictvím Azure App Service Web Apps přeskočit následující kroky a najdete v posledním kroku toohello [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md) článku.
> 
> 

tooassociate vaše vlastní doména se webové aplikace ve službě Azure App Service musí přidáte nový záznam v tabulce hello DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných hello doménového registrátora, které jste zakoupili název domény z. Použijte následující kroky toolocate hello a pomocí nástrojů DNS hello.

1. Přihlašovací účet tooyour u doménového registrátora a vyhledejte stránku pro správu záznamy DNS. Vyhledejte odkazy nebo oblasti lokality hello označený jako **název domény**, **DNS**, nebo **název serveru správy**. Často se odkaz toothis stránku můžete najít zobrazení informací o vašem účtu a pak hledá odkaz, jako **mé domény**.
2. Jakmile naleznete stránku hello správy pro název domény, vyhledejte odkaz, který vám umožní záznamy DNS tooedit hello. To může být uveden jako **souboru zóny**, **záznamy DNS**, nebo jako **Upřesnit** odkazu na konfiguraci.
   
   * stránku Hello bude mít pravděpodobně několik záznamů, které jsou již vytvořeny, jako je například přidružení vstupního '**@**'nebo'\*' 'domény parkovací' stránky. Záznamy pro běžné dílčím doménám domény může obsahovat také jako **www**.
   * bude zmínili stránku Hello **záznamy CNAME**, nebo zadejte rozevíracího seznamu tooselect typu záznamu. Například je může další záznamy také zmínili **záznamy A** a **záznamů MX**. V některých případech záznamy CNAME bude volat jiné názvy, jako **záznam aliasu**.
   * stránku Hello bude mít i pole, které vám umožňují příliš**mapy** z **název hostitele** nebo **název domény** tooanother název domény.
3. Při hello specifikace každý Registrátor liší, obecně namapujete *z* vlastního názvu domény (například **contoso.com**,) *k* název domény Traffic Manageru hello (**contoso.trafficmanager.net**) používané pro vaši webovou aplikaci.
   
   > [!NOTE]
   > Případně pokud záznam se už používá a je nutné toopreemptively vazby tooit vaší aplikace, můžete vytvořit další záznam CNAME. Například toopreemptively vazby **www.contoso.com** tooyour webové aplikace, vytvořte záznam CNAME z **awverify.www** příliš**contoso.trafficmanager.net**. Poté můžete přidat "www.contoso.com" tooyour webové aplikace beze změny záznam CNAME hello "www". Další informace najdete v tématu [záznamy DNS vytvořit pro webové aplikace ve vlastní doménu][CREATEDNS].
   > 
   > 
4. Po dokončení přidávání nebo úpravě záznamů DNS u registrátora uložte změny hello.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Povolit správce provozu
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře Node.js](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
