---
title: "aaaConfigure SSL snižování zátěže - Azure Application Gateway - Azure Portal | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate služby application gateway pomocí protokolu SSL snižování zátěže přes portál hello"
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
ms.openlocfilehash: e87ac0bbe10ac45e307c18802741c7bc31764a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-portal"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí portálu hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway může být relace Secure Sockets Layer (SSL) nakonfigurované tooterminate hello v hello brány tooavoid nákladná SSL dešifrování úlohy toohappen v hello webové farmy. Přesměrování zpracování SSL zjednodušuje i nastavení serveru front-end hello a Správa webové aplikace hello.

## <a name="scenario"></a>Scénář

Následující scénář Hello projde konfigurace přesměrování zpracování SSL na existující aplikační brány. Hello scénář předpokládá, že jste již provedli kroky hello příliš[vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Než začnete

tooconfigure přesměrování zpracování SSL pomocí služby application gateway, je vyžadován certifikát. Tento certifikát je načteno ve hello application gateway a používá tooencrypt a dešifrování přenosů hello odeslána prostřednictvím protokolu SSL. certifikát Hello musí toobe formát Personal Information Exchange (pfx). Tento formát souboru umožňuje pro hello privátní klíče toobe exportovali, které je požadované hello aplikace brány tooperform hello šifrování a dešifrování přenosů.

## <a name="add-an-https-listener"></a>Přidejte naslouchací proces HTTPS

naslouchací proces HTTPS Hello hledá provozu na základě jeho konfigurace a pomáhá trasy hello provoz toohello back-endové fondy.

### <a name="step-1"></a>Krok 1

Přejděte toohello portál Azure a vyberte existující aplikační brány

### <a name="step-2"></a>Krok 2

Klikněte na tlačítko naslouchací procesy a klikněte na tlačítko hello přidat tlačítko tooadd naslouchací proces.

![okno Přehled brány aplikace][1]

### <a name="step-3"></a>Krok 3

Vyplňte hello požadované informace pro naslouchací proces hello a nahrávání hello certifikátů .pfx, po dokončení klikněte na tlačítko OK.

**Název** – tato hodnota je popisný název hello naslouchacího procesu.

**Konfigurace IP front-endu** – tato hodnota je konfigurace IP front-endu hello, který se používá pro naslouchací proces hello.

**Front-endový port (název/Port)** -popisný název hello port používaný na front-endu hello aplikační brány a hello skutečný port používaný hello.

**Protokol** -toodetermine přepínače, pokud se používá protokol https nebo http pro hello front-endu.

**Certifikát (jméno a heslo)** – přesměrování zpracování SSL Pokud se používá, je vyžadován pro toto nastavení certifikát .pfx a popisný název a heslo jsou povinné.

![Přidejte naslouchací proces okno][2]

## <a name="create-a-rule-and-associate-it-toohello-listener"></a>Vytvoření pravidla a přidružit ho toohello naslouchací proces

byla vytvořena Hello naslouchací proces. Je čas toocreate přenosem hello toohandle pravidlo z hello naslouchací proces. Pravidla určují, jak provoz se směruje toohello back-endové fondy na základě více nastavení konfigurace, včetně toho, jestli se používá spřažení na základě souboru cookie relace, protokol, port a sondy stavu.

### <a name="step-1"></a>Krok 1

Klikněte na tlačítko hello **pravidla** hello aplikace brány a pak klikněte na tlačítko Přidat.

![okně pravidla aplikace brány][3]

### <a name="step-2"></a>Krok 2

Na hello **přidat základní pravidlo** okno, zadejte popisný název pravidla hello hello a zvolte naslouchací proces hello vytvořili v předchozím kroku hello. Zvolte hello odpovídající back-endový fond a http nastavení a klikněte na tlačítko **OK**

![okno nastavení protokolu HTTPS][4]

nastavení Hello se nyní ukládají toohello aplikační brány. Hello uložit proces pro toto nastavení může chvíli trvat, než jsou k dispozici tooview prostřednictvím hello portálu nebo pomocí prostředí PowerShell. Aplikační brána jednou uložené hello zpracovává hello šifrování a dešifrování přenosů. Prostřednictvím protokolu http se budou zpracovávat všechny přenosy mezi hello aplikační bránu a webovými servery služby hello back-end. Jakýkoli klient back toohello komunikace Pokud iniciované přes protokol https, bude vrácen toohello klienta zašifrovaná.

## <a name="next-steps"></a>Další kroky

toolearn způsobu tooconfigure vlastní stavu testu s Azure Application Gateway, najdete v [vytvořit sondu vlastní stavu](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
