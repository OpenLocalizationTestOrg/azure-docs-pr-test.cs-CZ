---
title: "pravidla pro aplikační bránu pomocí směrování adres URL - aaaCreate 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfigurace služby Azure application gateway pomocí pravidel směrování adres URL"
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
ms.openlocfilehash: 335b52be258945e1172eb0252b732e0e6ecb2ef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Vytvoření služby application gateway pomocí na základě cestu směrování s Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování adres URL umožňuje vám trasy tooassociate na základě hello cesty adresy URL požadavku Http. Se kontroluje, jestli je fond back-end tooa trasy pro URL hello uvedené v hello Aplikační brána nakonfigurovaná a odešle hello síťový provoz toohello definovanými fond back-end. Běžně používá pro směrování podle adresy URL je tooload vyrovnávat požadavky pro různé typy obsahu toodifferent back-end serverů fondy.

Na základě adresy URL směrování zavádí novou bránu tooapplication typ pravidla. Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting. Typ základní pravidlo poskytuje kruhového dotazování služby pro hello back-end fondy při PathBasedRouting kromě tooround každý s každým distribuční, také vzorek cesty adresy URL žádosti hello bere v úvahu při výběru fondu back-end hello.

## <a name="scenario"></a>Scénář

V následujícím příkladu hello, Application Gateway obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: výchozí fond serverů a fond bitové kopie serveru.

Požadavky pro http://contoso.com/image * jsou směrovány fondu serverů tooimage (imagesBackendPool), pokud hello vzorek cesty neodpovídá, je vybraný výchozí fond serverů (appGatewayBackendPool).

![Adresa URL trasy](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello **Microsoft Azure příkazového řádku**a přihlaste se. 

```azurecli
az login -u "username"
```

> [!NOTE]
> Můžete také použít `az login` bez hello přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.

Jakmile zadáte hello předchozím příkladu, je k dispozici kód. Přejděte toohttps://aka.ms/devicelogin v procesu přihlášení hello toocontinue prohlížeče.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči hello zadejte kód hello, kterou jste obdrželi. Jste přihlašovací stránku přesměrovaného tooa.

![Prohlížeč tooenter kódu][2]

Jakmile byl zadán kód hello jste přihlášeni, Zavřít hello prohlížeče toocontinue se scénářem hello.

![Úspěšné přihlášení][3]

## <a name="add-a-path-based-rule-tooan-existing-application-gateway"></a>Přidat bránu existující aplikace na základě cesty pravidlo tooan

Vytvoření služby application gateway s definované pravidlo cesty

### <a name="create-a-new-back-end-pool"></a>Vytvoření nového fondu back-end

Konfigurace nastavení brány aplikace **imagesBackendPool** hello Vyrovnávání zatížení síťových přenosů ve fondu back-end hello. V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro nový fond back-end hello. Každý fond back-end může mít vlastní nastavení fondu back-end.  Nastavení HTTP back-end jsou používány pravidla tooroute provoz toohello správné back-end členy fondu. Určuje hello protokol a port, který se používá při odesílání provozu toohello členy fondu back-end. Na základě souborů cookie relací jsou určeny také nastavení HTTP back-end hello.  Pokud je povoleno, spřažení relace na základě souborů cookie odešle provoz toohello stejnou back-end jako předchozí požadavky jednotlivých paketů.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Vytvořit nový port front-end

Nakonfigurujte port front-end hello aplikační brány. objekt konfigurace Hello front-end port je používán toodefine naslouchací proces port hello Application Gateway naslouchá provoz na hello naslouchací proces.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Vytvořte nový

Nakonfigurujte hello naslouchací proces. Tento krok nakonfiguruje hello naslouchací proces pro hello veřejnou IP adresu a port používá tooreceive příchozí síťový provoz. Následující ukázka Hello trvá hello dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje hello naslouchací proces. V tomto příkladu naslouchá naslouchací proces hello tooHTTP přenosy na portu 82 na hello veřejnou IP adresu, která jste vytvořili.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-hello-url-path-map"></a>Vytvořit mapování cesty adresy Url hello

Konfigurovat pravidla cesty adresy URL pro hello back endové fondy. Tento krok nakonfiguruje hello relativní cesta používaná systémem brány toodefine hello mapování mezi cestu adresy URL a kterému fondu back-end je přiřazen toohandle hello příchozí přenosy.

> [!IMPORTANT]
> Každá cesta musí začínat znakem / a jediným místem hello "\*" je povolena, je na konci hello. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *. Hello řetězec dodáni toohello cesta objekt přiřazení vzorce nezahrnuje jakýkoli text po hello nejprve "?" nebo "#" a tyto znaky nejsou povoleny. 

Hello následující příklad vytvoří jedno pravidlo pro "/ Image / *" cestu směrování provozu tooback-end "imagesBackendPool." Toto pravidlo zajišťuje, že přenosy dat pro každou sadu adres URL směrované toohello back-end. Například http://adatum.com/images/figure1.jpg přejde příliš "imagesBackendPool." Pokud cesta hello neodpovídá žádné z pravidel hello předem definovaná cesta, hello pravidla cesty mapy konfigurace nakonfiguruje taky výchozího fondu adres back-end. Například http://adatum.com/shoppingcart/test.html přejde toopool1, jako je definována jako hello výchozí fond pro neodpovídající provoz.

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

Pokud chcete toolearn o přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
