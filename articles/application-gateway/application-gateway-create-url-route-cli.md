---
title: "Vytvoření služby application gateway pomocí pravidel směrování adres URL - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření, konfiguraci služby Azure application gateway pomocí pravidel směrování adres URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Vytvoření služby application gateway pomocí na základě cestu směrování s Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování adres URL umožňuje přidružit tras na základě cesty adresy URL požadavku Http. Se ověří, zda je trasu k back-end fondu pro adresu URL v službu Application Gateway nakonfigurovaná a odešle síťový provoz do definované fond back-end. Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.

Na základě adresy URL směrování zavádí nový typ pravidla aplikační brány. Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting. Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy při PathBasedRouting kromě distribučních kruhové dotazování, také vzorek cesty adresy URL žádosti bere v úvahu při výběru fondu back-end.

## <a name="scenario"></a>Scénář

V následujícím příkladu se Aplikační brána obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: výchozí fond serverů a fond bitové kopie serveru.

Požadavky pro http://contoso.com/image * jsou směrovány do fondu serverů bitové kopie (imagesBackendPool), pokud vzorek cesty neodpovídá, je vybrán výchozí fond serverů (appGatewayBackendPool).

![Adresa URL trasy](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Otevřete **Microsoft Azure příkazového řádku**a přihlaste se. 

```azurecli
az login -u "username"
```

> [!NOTE]
> Můžete také použít `az login` bez přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.

Jakmile zadáte v předchozím příkladu, je k dispozici kód. Přejděte do https://aka.ms/devicelogin ve prohlížeči a pokračujte v procesu přihlášení.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči zadejte kód, který jste dostali. Budete přesměrováni na stránku přihlášení.

![prohlížeče k zadání kódu][2]

Jakmile byl zadán kód jste přihlášeni, zavřete prohlížeč pokračovat na tento scénář.

![Úspěšné přihlášení][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Přidat pravidlo založené na cestu k existující aplikační brány

Vytvoření služby application gateway s definované pravidlo cesty

### <a name="create-a-new-back-end-pool"></a>Vytvoření nového fondu back-end

Konfigurace nastavení brány aplikace **imagesBackendPool** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro nový fond back-end. Každý fond back-end může mít vlastní nastavení fondu back-end.  Nastavení back-endu HTTP jsou využívána pravidly pro směrování provozu do správných členů fondu back-end. Určuje protokol a port, který se používá při odesílání provozu do členy fondu back-end. Podle nastavení HTTP back-endu se určují i relace založené na souborech cookie.  Pokud je tato funkce povolena, spřažení relace založené na souborech cookie odesílá provoz do stejného back-endu jako předchozí požadavky pro jednotlivé pakety.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Vytvořit nový port front-end

Nakonfigurujte front-end port pro službu Application Gateway. Objekt konfigurace portu front-end je používán naslouchacím procesem k definování portu, na kterém služba Application Gateway v naslouchacím procesu naslouchá provozu.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Vytvořte nový

Nakonfigurujte naslouchací proces. V tomto kroku je nakonfigurován naslouchací proces pro veřejnou IP adresu a port používaný pro příjem příchozího síťového provozu. Následující příklad trvá dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje naslouchací proces. V tomto příkladu naslouchá naslouchací proces HTTP přenosy na portu 82 na veřejnou IP adresu, který jste vytvořili.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Vytvořit mapování cesty adresy Url

Konfigurovat pravidla cesty adresy URL pro back endové fondy. Tento krok nakonfiguruje relativní cestu používá aplikační bránu můžete definovat mapování mezi cestu adresy URL a je mu přiřazená kterému fondu back-end pro zpracování příchozí přenosy.

> [!IMPORTANT]
> Každá cesta musí začínat znakem / a bude jediným místem "\*" je povolena, je na konci. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *. Řetězec dodáni do objekt přiřazení vzorce cesta nezahrnuje jakýkoli text po první "?" nebo "#" a tyto znaky nejsou povoleny. 

Následující příklad vytvoří jedno pravidlo pro "/ Image / *" cestu směrování provozu na back-end "imagesBackendPool." Toto pravidlo zajišťuje, že přenosy dat pro každou sadu adresy URL se směruje na back-end. Například http://adatum.com/images/figure1.jpg přejde na "imagesBackendPool." Pokud cesta neodpovídá žádné z pravidel předem definovaná cesta, nakonfiguruje konfiguraci pravidla cesty mapy taky výchozího fondu adres back-end. Například http://adatum.com/shoppingcart/test.html přejde na pool1, jako je definován jako výchozí fond pro neodpovídající provoz.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Další kroky

Pokud chcete další informace o přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
