---
title: "Vytvořit nebo aktualizovat služby Azure Application Gateway pomocí brány firewall webových aplikací | Microsoft Docs"
description: "Informace o vytvoření služby Application Gateway pomocí brány firewall webových aplikací pomocí portálu"
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
ms.openlocfilehash: 650f26d19615d27a94f3947aad7b7904b6c1fabc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací pomocí portálu

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Naučte se vytvořit bránu webové aplikace povolena brána firewall aplikace.

Brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace. Webová aplikace chrání před řadu OWASP top 10 běžné webové chyb zabezpečení.

## <a name="scenarios"></a>Scénáře

V tomto článku jsou ve dvou situacích:

První scénář, můžete postup [vytvoření služby application gateway pomocí brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall)

Druhý scénář, můžete postup [přidání brány firewall webových aplikací do existující aplikace bránu](#add-web-application-firewall-to-an-existing-application-gateway).

![Příklad scénáře][scenario]

> [!NOTE]
> Další konfigurace aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po dokončení konfigurace aplikační brány a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

Služba Azure Application Gateway vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, že necháte dostatek adresní prostor tak, aby měl více podsítí. Po nasazení služby application gateway k podsíti, jedinými dodatečnými application Gateway je moct přidat do podsítě.

##<a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Přidání brány firewall webových aplikací do existující aplikační brány

Tento příklad aktualizuje existující aplikační brány pro podporu brány firewall webových aplikací v režimu ochrany.

1. Na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. Klikněte na existující aplikační brána v **všechny prostředky** okno. Pokud jste vybrali, již předplatné neobsahuje několik prostředků, můžete zadat název v **filtrovat podle názvu...** pro snadný přístup k zóně DNS.

   ![Vytvoření aplikační brány][1]

1. Klikněte na tlačítko **brány firewall webových aplikací** a aktualizovat nastavení aplikační brány. Po dokončení klikněte na tlačítko **uložit**

    Nastavení aktualizovat existující aplikační brány pro podporu brány firewall webových aplikací jsou:

   | **Nastavení** | **Hodnota** | **Podrobnosti**
   |---|---|---|
   |**Upgradujte na úroveň firewall webových aplikací**| Zaškrtnuté | Nastaví tato úroveň služby application gateway k vrstvě firewall webových aplikací.|
   |**Stav brány firewall**| Povoleno | Toto nastavení umožňuje v bráně firewall na firewall webových aplikací.|
   |**Režimu brány firewall** | Prevention (Prevence) | Toto nastavení je, jak má zacházet s škodlivý přenos brány firewall webových aplikací. **Detekce** režimu pouze protokoly událostí, kde **prevence** režim protokoly událostí a zastaví škodlivý přenos.|
   |**Sada pravidel**|3.0|Toto nastavení určuje, [základní sada pravidel](application-gateway-web-application-firewall-overview.md#core-rule-sets) používané k ochraně členy fondu back-end.|
   |**Konfigurace zakázaná pravidla**|Je to různé.|Abyste zabránili falešně pozitivních zjištění možných, toto nastavení lze zakázat určité [pravidla a pravidla skupiny](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Při upgradu existující aplikační brány pro SKU firewall webových aplikací, změny velikosti SKU **střední**. Můžete třeba překonfigurovat po dokončení konfigurace.

    ![okno zobrazuje základní nastavení][2-1]

    > [!NOTE]
    > Chcete-li zobrazit protokoly brány firewall webových aplikací, musí být povolena diagnostiky a ApplicationGatewayFirewallLog vybraná. Počet instancí 1 lze zvolit pro účely testování. Je důležité vědět, že libovolný počet instancí v rámci dvě instance není předmětem smlouvě SLA a proto nedoporučují. Malé brány nejsou k dispozici, při použití brány firewall webových aplikací.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací

Tento scénář se:

* Vytvoření brány aplikace střední webové aplikace brány firewall se dvěma instancemi.
* Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.
* Konfigurujte certifikát pro přesměrování zpracování SSL.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/free)
1. V podokně oblíbených položek na portálu, klikněte na tlačítko **nový**
1. V okně **Nový** klikněte na **Sítě**. V **sítě** okně klikněte na tlačítko **Application Gateway**, jak je znázorněno na následujícím obrázku:
1. Přejděte na portál Azure, klikněte na **nový** > **sítě** > **Application Gateway**

    ![Vytvoření aplikační brány][1]

1. V **Základy** okno, které se zobrazí, zadejte následující hodnoty a pak klikněte na **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti**
   |---|---|---|
   |**Název**|AdatumAppGateway|Název služby application gateway|
   |**Vrstvy**|FIREWALL WEBOVÝCH APLIKACÍ|Dostupné jsou hodnoty Standard a firewall webových aplikací. Navštivte [brány firewall webových aplikací](application-gateway-web-application-firewall-overview.md) Další informace o firewall webových aplikací.|
   |**Velikost SKU**|Střednědobé používání|Možnosti při výběru úrovně Standard jsou malé, střední a velká. Při výběru úrovně firewall webových aplikací, možnosti jsou střední a velké jenom.|
   |**Počet instancí**|2|Počet instancí služby application gateway pro vysokou dostupnost. Počty instancí 1 by měl použít pouze pro účely testování.|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém se má služba Application Gateway vytvořit.|
   |**Skupina prostředků**|**Vytvořit nový:** AdatumAppGatewayRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

   ![okno zobrazuje základní nastavení][2-2]

1. V **nastavení** okno, které se zobrazí pod **virtuální síť**, klikněte na tlačítko **vyberte virtuální síť**. Tento krok se otevře, zadejte **zvolte virtuální síť** okno.  Klikněte na tlačítko **vytvořit nový** otevřete **vytvořit virtuální síť** okno.

   ![Vyberte virtuální síť][2]

1. Na **okno vytvořit virtuální síť** zadejte následující hodnoty a potom klikněte na tlačítko **OK**. Zavře tento krok **vytvořit virtuální síť** a **zvolte virtuální síť** okna. Tím vyplníte **podsíť** na **nastavení** okno s podsítí vybrali.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|AdatumAppGatewayVNET|Název služby application gateway|
   |**Adresní prostor**|10.0.0.0/16| Tato hodnota je adresní prostor virtuální sítě|
   |**Název podsítě**|AppGatewaySubnet|Název podsítě pro službu application gateway|
   |**Rozsah adres podsítě**|10.0.0.0/28 | Tato podsíť umožňuje více další podsítě ve virtuální síti pro členy fondu back-end|

1. Na **nastavení** okno pod **konfigurace IP front-endu**, zvolte **veřejné** jako **typ IP adresy**

1. Na **nastavení** okno pod **veřejnou IP adresu**, klikněte na tlačítko **zvolte veřejnou IP adresu**, otevře se tento krok **zvolte veřejnou IP adresu** okno, klikněte na tlačítko **vytvořit nový**.

   ![Zvolte veřejnou IP adresu][3]

1. Na **vytvoření veřejné IP adresy** okno, přijměte výchozí hodnotu a klikněte na tlačítko **OK**. Zavře tento krok **zvolte veřejnou IP adresu** okně **vytvoření veřejné IP adresy** okně a naplnit **veřejnou IP adresu** s veřejnou IP adresou vybrali.

1. Na **nastavení** okno pod **konfiguraci naslouchacího procesu**, klikněte na tlačítko **HTTP** pod **protokol**. Chcete-li použít **https**, je vyžadován certifikát. Privátní klíč certifikátu je potřeba, aby .pfx export je nutné zadat certifikát a heslo pro soubor.

1. Konfigurace **firewall webových aplikací** specifická nastavení.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Stav brány firewall**| Povoleno| Toto nastavení zapne firewall webových aplikací nebo vypne.|
   |**Režimu brány firewall** | Prevention (Prevence)| Toto nastavení určuje, že firewall webových aplikací akce trvá na škodlivý přenos. Pokud **detekce** jste vybrali, je provoz jenom protokolována.  Pokud **prevence** jste vybrali, provoz je zaznamenána a zastavena 403 neoprávněným odpovědi.|


1. Zkontrolujte stránce Souhrn a klikněte na **OK**.  Službu application gateway je nyní zařazen do fronty a vytvořit.

1. Po vytvoření služby application gateway, přejděte k němu na portálu pokračujte konfigurace aplikační brány.

    ![Zobrazení prostředků brány aplikace][10]

Tyto kroky vytvořit základní aplikační brána s výchozím nastavením pro naslouchací proces, fond back-end, nastavení http back-end a pravidla. Můžete upravit toto nastavení tak, aby vyhovovala vašemu nasazení po úspěšné zajišťování

> [!NOTE]
> Application Gateway vytvoří s konfigurací brány firewall základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="next-steps"></a>Další kroky

Dále můžete Naučte se konfigurovat vlastní doménu alias pro funkci [veřejnou IP adresu](../dns/dns-custom-domain.md#public-ip-address) pomocí Azure DNS nebo jiného poskytovatele DNS.

Naučte se konfigurovat protokolování diagnostiky, do protokolu událostí, které jsou zjištěna nebo nebylo možné provést pomocí brány firewall webových aplikací navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)

Naučte se vytvářet vlastní stavu sondy navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak nakonfigurovat snižování zátěže protokolu SSL a trvat nákladná dešifrování SSL vypnout webových serverů, navštivte stránky [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
