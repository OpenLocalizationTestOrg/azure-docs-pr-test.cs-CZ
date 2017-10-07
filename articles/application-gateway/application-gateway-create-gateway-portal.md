---
title: "aaaCreate služby Application Gateway - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate služby Application Gateway pomocí portálu"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 24c1d5701eae372cd233162ceb58dea36a3b6a39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-hello-portal"></a>Vytvoření služby application gateway pomocí portálu hello

[Aplikační brána](application-gateway-introduction.md) je vyhrazené virtuální zařízení poskytuje aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaši aplikaci. Tento článek vás provede kroky toocreate hello služby Application Gateway pomocí hello portál Azure a přidání existujícího serveru jako člen back-end.

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlášení toohello portál Azure na [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

toocreate služby application gateway dokončení hello kroky, které provést. Aplikační brána vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, ponechte dostatek místa toohave adresu více podsítí. Po hello application gateway je nasazené tooa podsíť, jenom jiné application Gateway se dá přidat tooit.

1. V podokně Oblíbené hello hello portálu, klikněte na **nový**
1. V hello **nový** okně klikněte na tlačítko **sítě**. V hello **sítě** okně klikněte na tlačítko **Application Gateway**, jak ukazuje následující obrázek hello:

    ![Vytvoření aplikační brány][1]

1. V hello **Základy** okno, které se zobrazí, zadejte následující hodnoty hello a pak klikněte na **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti**|
   |---|---|---|
   |**Název**|AdatumAppGateway|Název Hello hello aplikační brány|
   |**Vrstvy**|Standard|Dostupné jsou hodnoty Standard a firewall webových aplikací. Navštivte [brány firewall webových aplikací](application-gateway-web-application-firewall-overview.md) toolearn více informací o firewall webových aplikací.|
   |**Velikost SKU**|Střednědobé používání|Možnosti při výběru úrovně Standard jsou malé, střední a velká. Při výběru úrovně firewall webových aplikací, možnosti jsou střední a velké jenom.|
   |**Počet instancí**|2|Počet instancí hello aplikační brány pro vysokou dostupnost. Počty instancí 1 by měl použít pouze pro účely testování.|
   |**Předplatné**|[Vaše předplatné]|Vyberte bránu předplatné toocreate hello aplikace v.|
   |**Skupina prostředků**|**Vytvořit nový:** AdatumAppGatewayRG|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) článek s přehledem.|
   |**Umístění**|Západní USA||

1. V hello **nastavení** okno, které se zobrazí pod **virtuální síť**, klikněte na tlačítko **vyberte virtuální síť**. Hello **zvolte virtuální síť** otevře se okno.  Klikněte na tlačítko **vytvořit nový** tooopen hello **vytvořit virtuální síť** okno.

   ![Vyberte virtuální síť][2]

1. Na hello **okno vytvořit virtuální síť** zadejte hello následující hodnoty a potom klikněte na tlačítko **OK**. Hello **vytvořit virtuální síť** a **zvolte virtuální síť** okna zavřete. Tento krok naplní hello **podsíť** na hello **nastavení** okno s podsítí hello vybrali.

   | **Nastavení** | **Hodnota** | **Podrobnosti**|
   |---|---|---|
   |**Název**|AdatumAppGatewayVNET|Název hello aplikační brány|
   |**Adresní prostor**|10.0.0.0/16|Toto je hello adresního prostoru virtuální sítě hello|
   |**Název podsítě**|AppGatewaySubnet|Název podsítě hello hello application Gateway.|
   |**Rozsah adres podsítě**|10.0.0.0/28|Tato podsíť umožňuje více další podsítě ve virtuální síti hello členy fondu back-end|

1. Na hello **nastavení** okno pod **konfigurace IP front-endu**, zvolte **veřejné** jako hello **typ IP adresy**

1. Na hello **nastavení** okno pod **veřejnou IP adresu** klikněte na tlačítko **zvolte veřejnou IP adresu**, hello **zvolte veřejnou IP adresu** otevře se okno , klikněte na tlačítko **vytvořit nový**.

   ![Zvolte veřejnou IP adresu][3]

1. Na hello **vytvoření veřejné IP adresy** přijměte hello výchozí hodnotu a klikněte na tlačítko **OK**. Hello okno se zavře a naplní hello **veřejnou IP adresu** s hello veřejnou IP adresou vybrali.

1. Na hello **nastavení** okno pod **konfiguraci naslouchacího procesu**, klikněte na tlačítko **HTTP** pod **protokol**. Zadejte hello port toouse v hello **Port** pole.

2. Klikněte na tlačítko **OK** na hello **nastavení** toocontinue okno.

1. Zkontrolujte nastavení hello na hello **Souhrn** a klikněte na **OK** toostart vytvoření hello aplikační brány. Vytvoření služby application gateway je dlouhotrvající úlohy a trvá toocomplete čas.

## <a name="add-servers-toobackend-pools"></a>Přidání fondů toobackend servery

Jakmile vytvoříte hello aplikační bránu, potřebujete hello systémů, které jsou hostiteli vyrovnáváním zatížení pro toobe hello aplikace ještě toobe přidané toohello aplikační brány. Hello IP adres, plně kvalifikovaný název domény nebo síťové adaptéry z těchto serverů se přidají toohello fondy adres back-end.

### <a name="ip-address-or-fqdn"></a>IP adresa nebo plně kvalifikovaný název domény

1. S bránou hello aplikace vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **AdatumAppGateway** hello Aplikační brána v okně všechny prostředky. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **AdatumAppGateway** v hello **filtrovat podle názvu...** pole tooeasily přístup hello aplikační brány.

1. Zobrazí se Hello aplikační bránu, kterou jste vytvořili. Klikněte na tlačítko **back-endové fondy**a vyberte fond back-end aktuální hello **appGatewayBackendPool**, hello **appGatewayBackendPool** otevře se okno.

   ![Fondy aplikací s back-end brány][4]

1. Klikněte na tlačítko **přidat cíl** tooadd IP adresy plně kvalifikovaný název domény. Zvolte **IP adresu nebo plně kvalifikovaný název domény** jako hello **typu** a do pole hello zadejte IP adresu nebo plně kvalifikovaný název domény. Tento krok opakujte pro členy fondu další back-end. Po dokončení klikněte na tlačítko **Uložit**.

### <a name="virtual-machine-and-nic"></a>Virtuální počítač a síťový adaptér

Síťové adaptéry virtuálního počítače můžete také přidat jako členy fondu back-end. Pouze virtuální počítače v rámci stejné virtuální síti jako hello Application Gateway jsou k dispozici prostřednictvím hello hello rozevíracího seznamu.

1. S bránou hello aplikace vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **AdatumAppGateway** hello Aplikační brána v okně všechny prostředky. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **AdatumAppGateway** v hello **filtrovat podle názvu...** pole tooeasily přístup hello aplikační brány.

1. Zobrazí se Hello aplikační bránu, kterou jste vytvořili. Klikněte na tlačítko **back-endové fondy**a vyberte fond back-end aktuální hello **appGatewayBackendPool**, hello **appGatewayBackendPool** otevře se okno.

   ![Fondy aplikací s back-end brány][5]

1. Klikněte na tlačítko **přidat cíl** tooadd IP adresy plně kvalifikovaný název domény. Zvolte **virtuálního počítače** jako hello **typu** a vyberte hello virtuálního počítače a toouse síťový adaptér. Po dokončení klikněte na tlačítko **uložit**

   > [!NOTE]
   > Jenom virtuální počítače v hello stejné virtuální síti, protože nejsou k dispozici v hello rozevíracího seznamu hello aplikační brány.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, odstraňte skupinu prostředků hello, aplikační brány a všechny související prostředky. toodo Ano, vyberte skupinu prostředků hello z okna hello aplikací brány a klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto scénáři nasazení služby application gateway a přidat toohello back-end serveru. Další kroky Hello jsou tooconfigure hello aplikační bránu pomocí změny nastavení a úprava pravidla brány hello. Tyto kroky můžete navštívením hello následující články:

Zjistěte, jak testy toocreate vlastní stavu navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak tooconfigure snižování zátěže protokolu SSL a proveďte hello nákladná SSL dešifrování vypnout navštivte stránky webových serverů [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

Zjistěte, jak tooprotect vaše aplikace s [brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md) funkce aplikační brány.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
