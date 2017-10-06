---
title: "aaaCreate vlastní probe - Azure Application Gateway - Azure Portal | Microsoft Docs"
description: "Zjistěte, jak toocreate vlastní sběru dat pro službu Application Gateway pomocí portálu hello"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 9e9309045ef33ba1010868783949b4fde31619ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-hello-portal"></a>Vytvořit vlastní test paměti pro službu Application Gateway pomocí portálu hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Classic PowerShell](application-gateway-create-probe-classic-ps.md)

V tomto článku můžete přidat vlastní test paměti tooan existující aplikace bránu prostřednictvím hello portálu Azure. Vlastní testy paměti jsou užitečné pro aplikace, které mají na stránce Kontrola konkrétní stav nebo pro aplikace, které neposkytuje úspěšné odpovědi na hello výchozí webové aplikace.

## <a name="before-you-begin"></a>Než začnete

Pokud již nemáte služby application gateway, navštivte [vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md) toocreate toowork brány aplikaci s.

## <a name="createprobe"></a>Vytvořit test paměti hello

Sondy jsou nakonfigurované ve dvou krocích prostřednictvím portálu hello. prvním krokem Hello je toocreate hello testu. V druhém kroku hello přidejte nastavení http hello testu toohello back-end brány aplikace hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/free)

1. V hello portál Azure podokno Oblíbené klepněte na všechny prostředky. Všechny prostředky okně klikněte na tlačítko hello Aplikační brána v hello. Pokud hello odběr, který jste již vybrali neobsahuje několik prostředků, můžete zadat partners.contoso.net v hello filtr podle názvu... pole tooeasily přístup hello aplikační brány.

1. Klikněte na tlačítko **sondy** a klikněte na tlačítko hello **přidat** tlačítko tooadd sondu.

  ![Přidat test okno s informacemi o doplnit][1]

1. Na hello **test stavu přidat** okno, zadejte informace požadované hello u testu paměti hello a po dokončení klikněte na tlačítko **OK**.

  |**Nastavení** | **Hodnota** | **Podrobnosti**|
  |---|---|---|
  |**Název**|customProbe|Tato hodnota je sondu toohello popisný název, který je přístupný hello portálu.|
  |**Protokol**|Protokol HTTP nebo HTTPS | Hello protokol, který hello test stavu používá.|
  |**Hostitele**|jednofaktorovému contoso.com|Tato hodnota je hello název hostitele, který se používá pro test hello. Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1". Tato hodnota se liší od hello název hostitele virtuálního počítače.|
  |**Cesta**|/ nebo jiné cesty|Hello zbytek hello úplnou adresu url pro vlastní test paměti hello. Platná cesta začíná '/'. Na výchozí cestu hello http://contoso.com stačí použít '/' |
  |**Interval (sekundy)**|30|Jak často hello test běží toocheck pro stavu. Není doporučeno tooset hello nižší než 30 sekund.|
  |**Časový limit (sekundy)**|30|Hello množství času hello testu čeká, než vyprší časový limit. Hello časový limit intervalu potřebám toobe dostatečně vysoký, že volání protokolu http můžete provedeny tooensure hello back-end stavu stránka je k dispozici.|
  |**Prahová hodnota špatného stavu**|3|Počet nezdařených pokusů o toobe považoval za poškozený. Prahová hodnota 0 znamená, že pokud se nezdaří Kontrola stavu hello back-end je určen okamžitě není v pořádku.|

  > [!IMPORTANT]
  > název hostitele Hello není hello stejný jako název serveru. Tato hodnota je hello název virtuálního hostitele hello běží na serveru aplikace hello. Test Hello je odeslán toohttp: / /(host name):(port from httpsetting)/urlPath

## <a name="add-probe-toohello-gateway"></a>Přidání brány toohello testu

Teď, když hello test byl vytvořen, je čas tooadd ho toohello brány. Test nastavení je nastaveno na nastavení http back-end hello hello aplikační brány.

1. Klikněte na tlačítko **nastavení HTTP** ve hello application gateway, toobring až hello konfigurace okna klikněte na tlačítko hello aktuální back-end http nastavení uvedené v okně hello.

  ![okno nastavení protokolu HTTPS][2]

1. Na hello **appGatewayBackEndHttpSettings** okno nastavení, zkontrolujte hello **použití vlastní test paměti** zaškrtávací políčko a zvolte hello testu vytvořené v hello [vytvořit hello testu](#createprobe) části na hello **vlastní test** rozevíracího seznamu...
Po dokončení klikněte na tlačítko **Uložit** a hello nastavení se použijí.

Hello výchozí kontroly kontroluje hello výchozí přístup toohello webové aplikace. Teď, když byla vytvořena vlastní test paměti, brána aplikace hello používá hello vlastní cestu definovanou toomonitor stavu pro hello back-end serverů. Podle hello kritéria, která byla definována, hello aplikační brány ověří hello cesta zadaná v testu hello. Pokud hello volání toohost:Port / cesta nevrací HTTP 200 399 stav odpovědi, hello server se dostala mimo otočení po dosažení hello prahová hodnota špatného stavu. Zjišťování bude pokračovat na toodetermine hello instance není v pořádku, když se stane znovu v pořádku. Po přidání fondu serverů back toohealthy hello instance provoz začne znovu průchodu tooit a testování toohello instance pokračuje v zadaném intervalu uživatele jako normální.

## <a name="next-steps"></a>Další kroky

jak tooconfigure s Azure Application Gateway, snižování zátěže protokolu SSL najdete v části toolearn [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

