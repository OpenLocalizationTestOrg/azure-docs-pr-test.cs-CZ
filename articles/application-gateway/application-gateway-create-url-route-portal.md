---
title: "aaaCreate cesta založená pravidlo - Azure Application Gateway - Azure Portal | Microsoft Docs"
description: "Zjistěte, jak hello toocreate pravidlo na základě cesty pro aplikační bránu pomocí portálu"
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
ms.openlocfilehash: 21cb52c426ca5f7dfedf07a96e87fbc85d243647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-hello-portal"></a>Vytvoření pravidla na základě cesty pro aplikační bránu pomocí portálu hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Na základě cestu směrování adres URL umožňuje vám trasy tooassociate na základě hello cesty adresy URL požadavku Http. Se kontroluje, jestli je fond back-end tooa trasy pro URL hello uvedené v hello Aplikační brána nakonfigurovaná a odešle hello síťový provoz toohello definovanými fond back-end. Běžně používá pro směrování podle adresy URL je tooload vyrovnávat požadavky pro různé typy obsahu toodifferent back-end serverů fondy.

Na základě adresy URL směrování zavádí novou bránu tooapplication typ pravidla. Application gateway poskytuje dva typy pravidel: pravidla základní a na základě cesty. Dobrý den pravidel základní typ, poskytuje služby kruhového dotazování pro hello back-end fondu při pravidla založená na cestu kromě tooround každý s každým distribuční, také vzorek cesty adresy URL žádosti hello bere v úvahu při výběru hello odpovídající back-endový fond.

## <a name="scenario"></a>Scénář

Hello následující scénář projde vytvoření pravidla na základě cesty v existující aplikační brány.
Hello scénář předpokládá, že jste již provedli kroky hello příliš[vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).

![Adresa URL trasy][scenario]

## <a name="createrule"></a>Vytvoření pravidla na základě cesty hello

Pravidla na základě cesty vyžaduje vlastní naslouchací proces, před vytvořením pravidel hello být jisti tooverify máte toouse dostupné naslouchací proces.

### <a name="step-1"></a>Krok 1

Přejděte toohello [portál Azure](http://portal.azure.com) a vyberte existující aplikační brány. Klikněte na tlačítko **pravidla**

![Přehled služby Application Gateway][1]

### <a name="step-2"></a>Krok 2

Klikněte na tlačítko **na základě cesty** tooadd tlačítko Nové pravidlo na základě cesty.

### <a name="step-3"></a>Krok 3

Hello **přidat na základě cesty pravidlo** okno obsahuje dvě části. první část Hello je, kde jste definovali hello naslouchací proces a název hello hello pravidla a nastavení výchozí cesty hello. Nastavení cesty výchozí Hello jsou pro trasy, která nespadají pod hello vlastní trasy na základě cesty. Druhá část hello Hello **přidat pravidlo na základě cesty** okno je, kde můžete definovat hello cesta pravidla založená na sami.

**Základní nastavení**

* **Název** – tato hodnota je popisný název toohello pravidlo, které je přístupné hello portálu.
* **Naslouchací proces** – tato hodnota je hello naslouchací proces, který se používá pro pravidlo hello.
* **Výchozí fond back-end** – toto nastavení je hello nastavení, která definuje toobe back-end hello používá pro hello výchozí pravidlo
* **Výchozí nastavení HTTP** – toto nastavení je hello nastavení, která definuje toobe nastavení HTTP hello používá pro hello výchozí pravidlo.

**Pravidla na základě cesty**

* **Název** – tato hodnota je pravidlo na základě toopath popisný název.
* **Cesty** – toto nastavení definuje hello cesta hello pravidlo hledá při předávání provozu
* **Fond back-end** – toto nastavení je hello nastavení, která definuje toobe back-end hello používá pro pravidlo hello
* **Nastavení HTTP** – toto nastavení je hello nastavení, která definuje toobe nastavení HTTP hello používá pro pravidlo hello.

> [!IMPORTANT]
> Cesty: hello seznam vzorů toomatch cesta. Každý musí začínat znakem / a jediným místem hello "\*" je povolena je na konci hello. Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.  

![Přidat pravidlo na základě cesty okno s informacemi o doplnit][2]

Přidání pravidla na základě cesty tooan existující application gateway je jednoduchý proces prostřednictvím portálu hello. Po vytvoření pravidla na základě cesty, může být upravený tooadd dalších pravidlech. 

![Přidání další pravidla na základě cesty][3]

Tento krok nakonfiguruje trasu na základě cesty. Je důležité toounderstand, že požadavky nejsou přepsaná, protože mají různé požadavky aplikační brána zkontroluje hello požadavku a basic na hello url vzor zasílá hello požadavek toohello odpovídající back endu.

## <a name="next-steps"></a>Další kroky

jak tooconfigure s Azure Application Gateway, snižování zátěže protokolu SSL najdete v části toolearn [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
