---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Tato stránka poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí portálu"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: f61be0cc4c9274c9914f7c468ce48a2a3d0a4f4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí portálu

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.

## <a name="scenario"></a>Scénář

V následujícím scénáři projde konfigurace přesměrování zpracování SSL na existující aplikační brány. Tento scénář předpokládá, že jste již provedli postup [vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Než začnete

Pokud chcete konfigurovat přesměrování zpracování SSL pomocí služby application gateway, je vyžadován certifikát. Tento certifikát je načíst ve službě application gateway a používat k šifrování a dešifrování přenosy odesílané prostřednictvím protokolu SSL. Certifikát musí být ve formátu Personal Information Exchange (pfx). Tento formát souboru umožňuje pro export privátního klíče, který je vyžadován součástí služby application gateway k šifrování a dešifrování přenosů.

## <a name="add-an-https-listener"></a>Přidejte naslouchací proces HTTPS

Naslouchací proces HTTPS hledá provozu na základě jeho konfigurace a pomáhá směrovat přenosy back-endové fondy.

### <a name="step-1"></a>Krok 1

Přejděte na portál Azure a vyberte existující aplikační brány

### <a name="step-2"></a>Krok 2

Klikněte na tlačítko naslouchací procesy a klikněte na tlačítko Přidat přidejte naslouchací proces.

![okno Přehled brány aplikace][1]

### <a name="step-3"></a>Krok 3

Zadejte požadované informace pro naslouchací proces a nahrání certifikátu .pfx, po dokončení klikněte na tlačítko OK.

**Název** – tato hodnota je popisný název naslouchacího procesu.

**Konfigurace IP front-endu** – tato hodnota je front-endovou konfiguraci protokolu IP, který se používá pro naslouchací proces.

**Front-endový port (název/Port)** – popisný název pro tento port používají na front-endu aplikační brány a skutečný port používat.

**Protokol** -přepínače k určení, pokud se používá protokol https nebo http pro front-endu.

**Certifikát (jméno a heslo)** – přesměrování zpracování SSL Pokud se používá, je vyžadován pro toto nastavení certifikát .pfx a popisný název a heslo jsou povinné.

![Přidejte naslouchací proces okno][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Vytvoření pravidla a přidružit ho k naslouchacímu procesu

Byla vytvořena naslouchací proces. Je čas k vytvoření pravidla pro zpracování provozu z naslouchací proces. Pravidla určují, jak se provoz směruje na back-endové fondy na základě více nastavení konfigurace, včetně toho, jestli se používá spřažení na základě souboru cookie relace, protokol, port a sondy stavu.

### <a name="step-1"></a>Krok 1

Klikněte **pravidla** aplikační brány a pak klikněte na tlačítko Přidat.

![okně pravidla aplikace brány][3]

### <a name="step-2"></a>Krok 2

Na **přidat základní pravidlo** okno, zadejte popisný název pravidla a zvolte naslouchací proces vytvořili v předchozím kroku. Zvolte odpovídající back-endový fond a http nastavení a klikněte na tlačítko **OK**

![okno nastavení protokolu HTTPS][4]

Nastavení jsou nyní uloženy do služby application gateway. Proces ukládání pro toto nastavení může chvíli trvat, než jsou k dispozici k zobrazení prostřednictvím portálu nebo pomocí prostředí PowerShell. Po uložení služby application gateway zpracovává šifrování a dešifrování přenosů. Prostřednictvím protokolu http se budou zpracovávat všechny přenosy mezi aplikační bránu a webovými servery back-end. Veškeré komunikace zpět do klienta, pokud iniciované přes protokol https, bude vrácen klientovi zašifrovaná.

## <a name="next-steps"></a>Další kroky

Pokud chcete dozvědět, jak nakonfigurovat vlastní stav testu s Azure Application Gateway, přečtěte si téma [vytvořit sondu vlastní stavu](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
