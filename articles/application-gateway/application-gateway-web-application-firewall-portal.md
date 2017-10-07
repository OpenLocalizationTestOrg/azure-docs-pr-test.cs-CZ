---
title: "aaaCreate nebo aktualizovat služby Azure Application Gateway pomocí brány firewall webových aplikací | Microsoft Docs"
description: "Zjistěte, jak hello toocreate služby Application Gateway pomocí brány firewall webových aplikací pomocí portálu"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: 68d140fef14499da654ea251d1208e6a800f55a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-web-application-firewall-by-using-hello-portal"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací pomocí portálu hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Zjistěte, jak toocreate brány firewall webových aplikací povoleno aplikační brány.

Hello brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace. Webová aplikace chrání před řadu hello OWASP top 10 běžné webové ohrožení zabezpečení.

## <a name="scenarios"></a>Scénáře

V tomto článku jsou ve dvou situacích:

V hello prvního scénáře, zjistíte příliš[vytvoření služby application gateway pomocí brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall)

V hello druhý scénář, zjistíte příliš[webové aplikace brány firewall tooan existující aplikace brány přidat](#add-web-application-firewall-to-an-existing-application-gateway).

![Příklad scénáře][scenario]

> [!NOTE]
> Další konfigurace hello aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po hello aplikace brána je nakonfigurovaná a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

Služba Azure Application Gateway vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, ponechte dostatek místa toohave adresu více podsítí. Jakmile nasadíte tooa podsítě application gateway, jsou pouze další aplikaci brány možné toobe přidat toohello podsítě.

##<a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Přidání webové aplikace brány firewall tooan existující aplikační brány

Tento příklad aktualizuje existující aplikaci brány toosupport brány firewall webových aplikací v režimu ochrany.

1. V portálu Azure hello **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello existující aplikační brána v hello **všechny prostředky** okno. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat název hello v hello **filtrovat podle názvu...** pole tooeasily přístup hello DNS zóny.

   ![Vytvoření aplikační brány][1]

1. Klikněte na tlačítko **brány firewall webových aplikací** a aktualizovat nastavení služby application gateway hello. Po dokončení klikněte na tlačítko **uložit**

    nastavení tooupdate Hello existující aplikaci brány toosupport brány firewall webových aplikací jsou:

   | **Nastavení** | **Hodnota** | **Podrobnosti**
   |---|---|---|
   |**Upgrade tooWAF vrstvy**| Zaškrtnuté | Nastaví tato úroveň hello hello aplikační brány toohello firewall webových aplikací vrstvě.|
   |**Stav brány firewall**| Povoleno | Toto nastavení umožňuje hello brány firewall na hello firewall webových aplikací.|
   |**Režimu brány firewall** | Prevention (Prevence) | Toto nastavení je, jak má zacházet s škodlivý přenos brány firewall webových aplikací. **Detekce** režimu jenom protokoluje události hello, kde **prevence** režimu protokoluje události hello a zastaví hello škodlivý přenos.|
   |**Sada pravidel**|3.0|Toto nastavení určuje hello [základní sada pravidel](application-gateway-web-application-firewall-overview.md#core-rule-sets) tedy použité tooprotect hello back-end členy fondu.|
   |**Konfigurace zakázaná pravidla**|Je to různé.|tooprevent možné falešně pozitivních zjištění, toto nastavení umožňuje toodisable určité [pravidla a pravidla skupiny](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Při upgradu existující brány aplikace toohello SKU firewall webových aplikací, hello SKU velikost změny příliš**střední**. Můžete třeba překonfigurovat po dokončení konfigurace.

    ![okno zobrazuje základní nastavení][2-1]

    > [!NOTE]
    > musí být povolena brána firewall protokoly tooview webové aplikace, diagnostiky a ApplicationGatewayFirewallLog vybrané. Počet instancí 1 lze zvolit pro účely testování. Je důležité tooknow, aby všechny instance počet v části dvě instance není předmětem hello SLA a proto nedoporučují. Malé brány nejsou k dispozici, při použití brány firewall webových aplikací.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací

Tento scénář se:

* Vytvoření brány aplikace střední webové aplikace brány firewall se dvěma instancemi.
* Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.
* Konfigurujte certifikát pro přesměrování zpracování SSL.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/free)
1. V podokně Oblíbené hello hello portálu, klikněte na **nový**
1. V hello **nový** okně klikněte na tlačítko **sítě**. V hello **sítě** okně klikněte na tlačítko **Application Gateway**, jak ukazuje následující obrázek hello:
1. Přejděte toohello portálu Azure, klikněte na **nový** > **sítě** > **Application Gateway**

    ![Vytvoření aplikační brány][1]

1. V hello **Základy** okno, které se zobrazí, zadejte následující hodnoty hello a pak klikněte na **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti**
   |---|---|---|
   |**Název**|AdatumAppGateway|Název Hello hello aplikační brány|
   |**Vrstvy**|FIREWALL WEBOVÝCH APLIKACÍ|Dostupné jsou hodnoty Standard a firewall webových aplikací. Navštivte [brány firewall webových aplikací](application-gateway-web-application-firewall-overview.md) toolearn více informací o firewall webových aplikací.|
   |**Velikost SKU**|Střednědobé používání|Možnosti při výběru úrovně Standard jsou malé, střední a velká. Při výběru úrovně firewall webových aplikací, možnosti jsou střední a velké jenom.|
   |**Počet instancí**|2|Počet instancí hello aplikační brány pro vysokou dostupnost. Počty instancí 1 by měl použít pouze pro účely testování.|
   |**Předplatné**|[Vaše předplatné]|Vyberte bránu předplatné toocreate hello aplikace v.|
   |**Skupina prostředků**|**Vytvořit nový:** AdatumAppGatewayRG|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) článek s přehledem.|
   |**Umístění**|Západní USA||

   ![okno zobrazuje základní nastavení][2-2]

1. V hello **nastavení** okno, které se zobrazí pod **virtuální síť**, klikněte na tlačítko **vyberte virtuální síť**. Tento krok otevře zadejte hello **zvolte virtuální síť** okno.  Klikněte na tlačítko **vytvořit nový** tooopen hello **vytvořit virtuální síť** okno.

   ![Vyberte virtuální síť][2]

1. Na hello **okno vytvořit virtuální síť** zadejte hello následující hodnoty a potom klikněte na tlačítko **OK**. Tento krok zavře hello **vytvořit virtuální síť** a **zvolte virtuální síť** okna. Tím vyplníte hello **podsíť** na hello **nastavení** okno s podsítí hello vybrali.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|AdatumAppGatewayVNET|Název hello aplikační brány|
   |**Adresní prostor**|10.0.0.0/16| Tato hodnota je hello adresního prostoru virtuální sítě hello|
   |**Název podsítě**|AppGatewaySubnet|Název podsítě hello hello application Gateway.|
   |**Rozsah adres podsítě**|10.0.0.0/28 | Tato podsíť umožňuje více další podsítě ve virtuální síti hello členy fondu back-end|

1. Na hello **nastavení** okno pod **konfigurace IP front-endu**, zvolte **veřejné** jako hello **typ IP adresy**

1. Na hello **nastavení** okno pod **veřejnou IP adresu**, klikněte na tlačítko **zvolte veřejnou IP adresu**, tento krok otevře hello **zvolte veřejnou IP adresu**okně klikněte na tlačítko **vytvořit nový**.

   ![Zvolte veřejnou IP adresu][3]

1. Na hello **vytvoření veřejné IP adresy** přijměte hello výchozí hodnotu a klikněte na tlačítko **OK**. Tento krok zavře hello **zvolte veřejnou IP adresu** okno, hello **vytvoření veřejné IP adresy** okně a naplnit **veřejnou IP adresu** s hello veřejnou IP adresou vybrali.

1. Na hello **nastavení** okno pod **konfiguraci naslouchacího procesu**, klikněte na tlačítko **HTTP** pod **protokol**. toouse **https**, je vyžadován certifikát. privátní klíč certifikátu hello Hello je potřeba, tak, aby export .pfx hello certifikátu musí zadat toobe a hello heslo pro soubor hello.

1. Konfigurace hello **firewall webových aplikací** specifická nastavení.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Stav brány firewall**| Povoleno| Toto nastavení zapne firewall webových aplikací nebo vypne.|
   |**Režimu brány firewall** | Prevention (Prevence)| Toto nastavení určuje hello akce, které trvá firewall webových aplikací na škodlivý přenos. Pokud **detekce** jste vybrali, je provoz jenom protokolována.  Pokud **prevence** jste vybrali, provoz je zaznamenána a zastavena 403 neoprávněným odpovědi.|


1. Zkontrolujte souhrn stránku hello a klikněte na **OK**.  Aplikační brána hello je nyní zařazen do fronty a vytvořit.

1. Po vytvoření aplikační brány hello přejděte tooit v konfiguraci portálu toocontinue hello hello aplikační brány.

    ![Zobrazení prostředků brány aplikace][10]

Tyto kroky vytvořit základní aplikační brána s výchozím nastavením pro naslouchací proces hello, fond back-end, nastavení http back-end a pravidla. Můžete upravit tyto toosuit nastavení nasazení po úspěšné hello zřizování

> [!NOTE]
> Application Gateway, které jsou vytvořené pomocí konfigurace brány firewall hello základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="next-steps"></a>Další kroky

Dále můžete další informace jak tooconfigure alias vlastní doménu pro hello [veřejnou IP adresu](../dns/dns-custom-domain.md#public-ip-address) pomocí Azure DNS nebo jiného poskytovatele DNS.

Zjistěte, jak protokolování diagnostiky tooconfigure, toolog hello události, které jsou zjištěna nebo předcházet pomocí brány firewall webových aplikací, navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)

Zjistěte, jak testy toocreate vlastní stavu navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak tooconfigure snižování zátěže protokolu SSL a proveďte hello nákladná SSL dešifrování vypnout navštivte stránky webových serverů [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
