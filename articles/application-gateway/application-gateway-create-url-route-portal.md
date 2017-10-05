---
title: "Vytvoření pravidla na základě cesta - Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet pravidla na základě cesty pro aplikační bránu pomocí portálu"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: c184e94a04cfbdedcae70ed154aeb7dd134d1baf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Vytvoření pravidla na základě cesty pro aplikační bránu pomocí portálu

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování adres URL umožňuje přidružit tras na základě cesty adresy URL protokolu Http žádosti. Se ověří, zda je trasu k back-end fondu pro adresu URL uvedené v bráně aplikace nakonfigurovaná a odešle síťový provoz do definované fond back-end. Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.

Na základě adresy URL směrování zavádí nový typ pravidla aplikační brány. Application gateway poskytuje dva typy pravidel: pravidla základní a na základě cesty. Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy při pravidla na základě cesty kromě distribučních kruhové dotazování, také vzorek cesty adresy URL žádosti bere v úvahu při výběru příslušné back-endový fond.

## <a name="scenario"></a>Scénář

V následujícím scénáři projde vytvoření pravidla na základě cesty v existující aplikační brány.
Tento scénář předpokládá, že jste již provedli postup [vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).

![Adresa URL trasy][scenario]

## <a name="createrule"></a>Vytvoření pravidla na základě cesty

Pravidla na základě cesty vyžaduje vlastní naslouchací proces, předtím, než při vytváření pravidla ověřte máte k dispozici naslouchací proces používat.

### <a name="step-1"></a>Krok 1

Přejděte na [portál Azure](http://portal.azure.com) a vyberte existující aplikační brány. Klikněte na tlačítko **pravidla**

![Přehled služby Application Gateway][1]

### <a name="step-2"></a>Krok 2

Klikněte na tlačítko **na základě cesty** tlačítko Přidat nové pravidlo na základě cesty.

### <a name="step-3"></a>Krok 3

**Přidat na základě cesty pravidlo** okno obsahuje dvě části. V první části je, kde jste definovali naslouchací proces, název pravidla a nastavení výchozí cesty. Výchozí cesta nastavení jsou pro trasy, která nespadají pod vlastní trasy na základě cesty. Druhá část **přidat na základě cesty pravidlo** je okno, kde můžete definovat pravidla na základě cesty sami.

**Základní nastavení**

* **Název** – tato hodnota je popisný název pravidla, která je přístupná na portálu.
* **Naslouchací proces** – tato hodnota je naslouchací proces, který se používá pro pravidlo.
* **Výchozí fond back-end** – toto nastavení je nastavení, která definuje back-end, který se má použít pro výchozí pravidlo
* **Výchozí nastavení HTTP** – toto nastavení je nastavení, které definuje nastavení protokolu HTTP, která má být použit pro výchozí pravidlo.

**Pravidla na základě cesty**

* **Název** – tato hodnota je popisný název, který pravidlem cesty.
* **Cesty** – toto nastavení definuje cestu toto pravidlo vyhledá při předávání provozu
* **Fond back-end** – toto nastavení je nastavení, která definuje back-end, který se má použít pro pravidlo
* **Nastavení HTTP** – toto nastavení je nastavení, které definuje nastavení protokolu HTTP, která má být použit pro pravidlo.

> [!IMPORTANT]
> Cesty: Seznam vzorů cesta tak, aby odpovídaly. Každý musí začínat znakem / a bude jediným místem "\*" je povolena je na konci. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.  

![Přidat pravidlo na základě cesty okno s informacemi o doplnit][2]

Přidávání pravidla na základě cestu k existující aplikační brány je jednoduchý proces prostřednictvím portálu. Po vytvoření pravidla na základě cesty, se dá upravit pro přidání dalších pravidlech. 

![Přidání další pravidla na základě cesty][3]

Tento krok nakonfiguruje trasu na základě cesty. Je důležité pochopit, které požadavky nejsou přepsaná, protože mají různé požadavky aplikační brána zkontroluje žádosti a basic na vzor adresy url odešle požadavek na příslušné back-end.

## <a name="next-steps"></a>Další kroky

Další postup konfigurace snižování zátěže protokolu SSL s Azure Application Gateway najdete v tématu [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
