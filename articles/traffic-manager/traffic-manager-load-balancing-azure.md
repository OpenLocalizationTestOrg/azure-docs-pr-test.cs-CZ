---
title: "Vyrovnávání zatížení aaaUsing služby v Azure | Microsoft Docs"
description: "Tento kurz ukazuje, jak toocreate scénář pomocí hello Azure portfolií Vyrovnávání zatížení: Traffic Manager, aplikační brány a vyrovnávání zatížení."
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: d217047102d8c4828250dc0733e8ec9ee1e84b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-load-balancing-services-in-azure"></a>Použití služby Vyrovnávání zatížení v Azure

## <a name="introduction"></a>Úvod

Microsoft Azure poskytuje několik služeb pro správu, jak je distribuován síťový provoz a skupinu s vyrovnáváním zatížení. Můžete použít tyto služby samostatně nebo v kombinaci své metody podle svých potřeb, toobuild hello optimální řešení.

V tomto kurzu jsme nejprve definovat případu použití zákazníka a v tématu jak ji můžete provést robustnější a původce pomocí hello následující Azure portfolií Vyrovnávání zatížení: Traffic Manager, aplikační brány a vyrovnávání zatížení. Jsme potom poskytují podrobné pokyny pro vytvoření nasazení, které je geograficky redundantní distribuuje provoz tooVMs a vám pomůže spravovat různé typy požadavků.

Koncepční úrovni každý z těchto služeb hraje důležitou roli v hierarchii hello Vyrovnávání zatížení.

* **Správce provozu** poskytuje globální DNS Vyrovnávání zatížení. To vypadá na příchozí požadavky na DNS a odpoví funkční koncový bod, v souladu s hello směrování zákazníka hello zásad vybráno. Možnosti pro směrování metody jsou následující:
  * Výkon směrování toosend hello toohello nejbližší koncový bod žadatele z hlediska latence.
  * Priorita směrování toodirect všechny přenosy tooan koncový bod, se ostatní koncové body zálohu.
  * Vážené kruhové dotazování směrování, která distribuuje provoz podle hello vyvážení, který je přiřazen tooeach koncový bod.

  Hello klient připojí přímo toothat koncový bod. Azure Traffic Manager zjistí, že koncový bod není v pořádku a pak přesměruje hello klientům tooanother pořádku instance. Odkazovat příliš[dokumentace Azure Traffic Manager](traffic-manager-overview.md) toolearn více informací o hello služby.
* **Aplikační brána** poskytuje aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 Vyrovnávání zatížení možnosti pro vaši aplikaci. Umožňuje zákazníkům toooptimize webové farmy produktivitu přesměrováním náročná na prostředky procesoru SSL ukončení toohello aplikační brány. Další možnosti směrování vrstvy 7 zahrnují kruhového dotazování distribuce příchozí provoz, spřažení na základě souboru cookie relace, na základě cestu směrování adres URL a hello možnost toohost více webů za bránu jednu aplikaci. Application Gateway můžete nakonfigurovat jako bránu internetové brány, bránu pouze interní nebo obojí. Application Gateway je plně Azure spravované, škálovatelné a vysoce dostupné. Nabízí celou řadu možností diagnostiky a protokolování, které zlepšují správu.
* **Nástroj pro vyrovnávání zatížení** je nedílnou součástí zásobníku hello Azure SDN, který poskytuje vysoce výkonné, nízkou latencí vrstvy 4 Vyrovnávání zatížení služby pro všechny protokolu UDP a TCP protokoly. Spravuje příchozí a odchozí připojení. Můžete nakonfigurovat veřejný a interní Vyrovnávání zatížení sítě koncové body a definovat pravidla toomap příchozí připojení tooback-end fondu cíle pomocí TCP a HTTP zjišťování stavu možnosti toomanage dostupnost služeb.

## <a name="scenario"></a>Scénář

V tomto ukázkovém scénáři používáme jednoduché web, který má dva typy obsahu: bitové kopie a dynamicky vykreslené webové stránky. Hello webu musí být geograficky redundantní a svým uživatelům z hello nejbližší (nejnižší latenci) by měla sloužit toothem umístění. Hello vývojář aplikace se rozhodla, že všechny adresy URL, které odpovídají hello vzor/Image / * ze fond virtuálních počítačů, které se liší od hello zbytek hello webové farmy.

Fond virtuálních počítačů výchozí hello obsluhující hello dynamický obsah se navíc musí databázi tootalk tooa back-end, který je hostován na clusteru s vysokou dostupností. Hello celé nasazení je nastavit prostřednictvím Správce Azure Resource Manager.

Pomocí Správce provozu, aplikační brány a nástroj pro vyrovnávání zatížení umožňuje tento web tooachieve, tyto cíle návrhu:

* **Více geografická redundance**: Pokud jedné oblasti ocitne mimo provoz, provoz směrování Traffic Manager bezproblémově toohello nejbližší oblast bez jakéhokoli zásahu od vlastníka aplikace hello.
* **Nižší latenci**: protože Traffic Manager automaticky přesměruje hello zákazníka toohello nejbližší oblast, hello zákazníka vyskytne nižší latenci žádosti o obsah webovou stránku hello.
* **Nezávislé škálovatelnost**: protože hello webové aplikace zatížení je oddělená typ obsahu, majitel aplikace hello možné škálovat nezávisle na sobě navzájem úlohy požadavek hello. Aplikační brána zajišťuje, že hello přenosů směrovaných toohello správné fondů založená na hello zadaná pravidla a hello stavu aplikace hello.
* **Vyrovnávání zatížení pro vnitřní**: protože Vyrovnávání zatížení je před hello vysokou dostupnost clusteru, jen hello aktivní a pořádku endpoint pro databázi je zveřejněné toohello aplikace. Kromě toho správce databáze můžete optimalizovat hello zatížení distribuci aktivní a pasivní repliky v clusteru hello nezávislé na front-endu aplikace hello. Nástroj pro vyrovnávání zatížení poskytuje připojení toohello vysokou dostupnost clusteru a zajišťuje, že pouze v pořádku databáze přijímat žádosti o připojení.

Hello následující diagram znázorňuje architekturu hello tento scénář:

![Diagram Vyrovnávání zatížení architektura](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> V tomto příkladu je pouze jeden z mnoha možné konfigurace služby Vyrovnávání zatížení služeb hello, které Azure nabízí. Traffic Manager, aplikační brány a vyrovnávání zatížení můžete kombinovat a odpovídající toobest vyhovovaly potřebám vaší služby Vyrovnávání zatížení. Například pokud přesměrování zpracování SSL nebo zpracování vrstvy 7 není nezbytné, nástroj pro vyrovnávání zatížení můžete použít místo Application Gateway.

## <a name="setting-up-hello-load-balancing-stack"></a>Nastavení služby Vyrovnávání zatížení zásobníku hello

### <a name="step-1-create-a-traffic-manager-profile"></a>Krok 1: Vytvoření profilu Traffic Manageru

1. V hello portálu Azure, klikněte na **nový**a pak hledání hello marketplace pro "Profil služby Traffic Manager."
2. Na hello **profil služby Traffic Manager vytvořit** okno, zadejte hello následující základní informace:

  * **Název**: udělte vašeho profilu Traffic Manageru DNS název předpony.
  * **Metody směrování**: Vyberte hello zásad metodu směrování provozu. Další informace o metodách hello najdete v tématu [metodách směrování provozu Traffic Manager](traffic-manager-routing-methods.md).
  * **Předplatné**: Vyberte hello předplatné, který obsahuje profil hello.
  * **Skupina prostředků**: Vyberte hello skupinu prostředků, která obsahuje profil hello. Může být nový nebo stávající skupiny prostředků.
  * **Umístění skupiny prostředků**: služby Traffic Manager je globální a není vázaná tooa umístění. Však musíte zadat oblast pro hello skupiny, které se nachází hello metadata spojená se profil služby Traffic Manager hello. Toto umístění nemá žádný vliv na běhovou dostupnost hello hello profilu.

3. Klikněte na tlačítko **vytvořit** toogenerate hello profil služby Traffic Manager.

  ![Okno "Vytvořit Traffic Manager"](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-hello-application-gateways"></a>Krok 2: Vytvoření hello application Gateway

1. V hello portál Azure, v levém podokně hello, klikněte na **nový** > **sítě** > **Application Gateway**.
2. Zadejte hello následující základní informace o hello aplikační brány:

  * **Název**: název hello hello aplikační brány.
  * **Velikost SKU**: hello velikost hello aplikační bránu, k dispozici jako malé, střední nebo velké.
  * **Instance počet**: hello počet instancí, hodnotu v rozmezí 2 až 10.
  * **Skupina prostředků**: hello skupinu prostředků, který obsahuje hello aplikační brány. Může být existující skupinu prostředků nebo novou.
  * **Umístění**: oblast hello hello aplikační brány, který je hello stejné umístění jako skupina prostředků hello. Hello umístění je důležité, protože hello virtuální síť a veřejnou IP adresu musí být v hello stejné umístění jako brána hello.
3. Klikněte na **OK**.
4. Zadejte hello virtuální sítě, podsítě, front-end IP adresu a konfigurace naslouchacího procesu pro službu hello application gateway. V tomto scénáři je IP adresa front-endu hello **veřejné**, což umožňuje toobe později přidat jako toohello koncový bod profilu služby Traffic Manager.
5. Nakonfigurujte hello naslouchací proces s jedním z hello následující možnosti:
    * Pokud používáte protokol HTTP, není co tooconfigure. Klikněte na **OK**.
    * Pokud použijete protokol HTTPS, další konfigurace se nevyžaduje. Odkazovat příliš[vytvoření služby application gateway](../application-gateway/application-gateway-create-gateway-portal.md)začínající kroku 9. Po dokončení konfigurace hello, klikněte na tlačítko **OK**.

#### <a name="configure-url-routing-for-application-gateways"></a>Konfigurace adresy URL směrování pro application Gateway

Když zvolíte fond back-end, aplikační bránu, která je konfigurováno pravidlo na základě cesty trvá vzorek cesty adresy URL žádosti hello přidání tooround dotazováním rozdělení. V tomto scénáři přidáváme na základě cesty pravidlo toodirect libovolná adresa URL s "/images/\*" fondu serverů toohello bitové kopie. Další informace o konfiguraci adresy URL na základě cestu směrování pro aplikační bránu, najdete v části příliš[vytvoření pravidla na základě cesty pro aplikační bránu](../application-gateway/application-gateway-create-url-route-portal.md).

![Diagram webová vrstva brány aplikace](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Z vaší skupiny prostředků přejděte toohello instanci hello aplikační bránu, kterou jste vytvořili v předcházející části hello.
2. V části **nastavení**, vyberte **back-endové fondy**a potom vyberte **přidat** tooadd hello virtuálních počítačů, které chcete tooassociate s hello webová vrstva back endové fondy.
3. Na hello **přidejte fond back-end** okno, zadejte název fondu back-end hello hello a všechny IP adresy hello hello počítačů, které jsou umístěny ve fondu hello. V tomto scénáři se připojujete, dvě back-end serverů fondy virtuálních počítačů.

  ![Okna "Přidat fond back-end" brány aplikací](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. V části **nastavení** hello aplikační bránu, vyberte **pravidla**a potom klikněte na hello **na základě cest** tlačítko tooadd pravidlo.

  ![Tlačítko "Cesta založená" pravidla brány aplikace](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Na hello **přidat na základě cesty pravidlo** okně nakonfigurovat pravidlo hello tím, že poskytuje hello následující informace.

   Základní nastavení:

   + **Název**: hello popisný název hello pravidla, která je přístupná hello portálu.
   + **Naslouchací proces**: hello naslouchací proces, který se používá pro pravidlo hello.
   + **Výchozí fond back-end**: hello toobe fond back-end použít s hello výchozí pravidlo.
   + **Výchozí nastavení HTTP**: toobe nastavení hello HTTP použít s hello výchozí pravidlo.

   Pravidla na základě cesty:

   + **Název**: hello popisný název pravidla na základě cesty hello.
   + **Cesty**: hello pravidlo cesty, který se používá pro předávání dál provoz.
   + **Fond back-end**: hello toobe fond back-end použít s tímto pravidlem.
   + **Nastavení HTTP**: toobe nastavení hello HTTP použít s tímto pravidlem.

   > [!IMPORTANT]
   > Cesty: Platná cesta musí začínat znakem "/". zástupný text Hello "\*" je povolena pouze na konci hello. Platnými hodnotami jsou /xyz, /xyz\*, nebo /xyz/\*.

   ![Okna "Přidat na základě cesty pravidlo" brány aplikací](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-toohello-traffic-manager-endpoints"></a>Krok 3: Přidání aplikace brány toohello Traffic Manager koncové body

V tomto scénáři je Traffic Manager brány připojené tooapplication (podle konfigurace v předchozích krocích hello), které se nacházejí v různých oblastech. Teď, když jsou nakonfigurované hello application Gateway, hello dalším krokem je tooconnect je tooyour profil služby Traffic Manager.

1. Otevřete svůj profil Traffic Manageru. toodo Ano, podívejte se ve vaší skupině prostředků nebo vyhledejte název hello profil služby Traffic Manager hello z **všechny prostředky**.
2. V levém podokně hello vyberte **koncové body**a potom klikněte na **přidat** tooadd koncový bod.

  ![Správce provozu koncové body "Přidat" tlačítko](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Na hello **přidání koncového bodu** okně vytvořit koncový bod zadáním hello následující informace:

  * **Typ**: Vyberte typ hello tooload vyrovnávání koncový bod. V tomto scénáři vyberte **koncového bodu Azure** vzhledem k tomu, že jsme ho připojujete toohello aplikace brány instancí, které byly dříve nakonfigurovány.
  * **Název**: Zadejte název hello hello koncového bodu.
  * **Typ prostředku cíle**: vyberte **veřejnou IP adresu** a pak v části **cíle prostředků**, vyberte hello veřejnou IP adresu brány hello aplikace, který byl dříve nakonfigurován.

   ![Okno "Přidat koncový bod" Traffic Manageru](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Nyní můžete otestovat vašeho nastavení přístup s hello DNS vašeho profilu Traffic Manageru (v tomto příkladu: TrafficManagerScenario.trafficmanager.net). Můžete znovu odeslal požadavky, zprovoznit nebo převést virtuální počítače a webové servery, které byly vytvořeny v různých oblastech a změnit tootest nastavení profilu Traffic Manageru hello vašeho nastavení.

### <a name="step-4-create-a-load-balancer"></a>Krok 4: Vytvoření služby Vyrovnávání zatížení

Nástroj pro vyrovnávání zatížení v tomto scénáři distribuuje připojení z hello webové vrstvy toohello databází v rámci clusteru s vysokou dostupností.

Pokud váš cluster vysokou dostupnost databáze používá SQL Server AlwaysOn, podívejte se příliš[nakonfigurovat jeden nebo více vždy na dostupnosti naslouchací procesy skupiny](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) podrobné pokyny.

Další informace o konfiguraci Vyrovnávání zatížení interní najdete v tématu [v hello portálu Azure vytvořit interní nástroj](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. V hello portál Azure, v levém podokně hello, klikněte na **nový** > **sítě** > **nástroj pro vyrovnávání zatížení**.
2. Na hello **vytvořit nástroj pro vyrovnávání zatížení** okně, vyberte název pro nástroj pro vyrovnávání zatížení.
3. Sada hello **typ** příliš**interní**a zvolte hello odpovídající virtuální síť a podsíť pro tooreside nástroje pro vyrovnávání zatížení hello v.
4. V části **přiřazení IP adresy**, vyberte buď **dynamické** nebo **statické**.
5. V části **skupiny prostředků**, zvolte skupinu prostředků hello nástroje pro vyrovnávání zatížení hello.
6. V části **umístění**, vyberte příslušnou oblast hello nástroje pro vyrovnávání zatížení hello.
7. Klikněte na tlačítko **vytvořit** nástroj pro vyrovnávání zatížení toogenerate hello.

#### <a name="connect-a-back-end-database-tier-toohello-load-balancer"></a>Připojit Vyrovnávání zatížení vrstvy toohello databázi back-end

1. Od vaší skupiny prostředků najít hello Vyrovnávání zatížení, který byl vytvořen v předchozích krocích hello.
2. V části **nastavení**, klikněte na tlačítko **back-endové fondy**a potom klikněte na **přidat** tooadd fond back-end.

  ![Okno "Přidat fond back-end" nástroje pro vyrovnávání zatížení](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Na hello **přidejte fond back-end** okno, zadejte název fondu back-end hello hello.
4. Přidáte jednotlivé počítače nebo fond back-end toohello sady dostupnosti.

#### <a name="configure-a-probe"></a>Nakonfigurovat test paměti

1. V nástroj pro vyrovnávání zatížení v části **nastavení**, vyberte **sondy**a potom klikněte na **přidat** tooadd sondu.

 ![Okno "Přidat test" nástroje pro vyrovnávání zatížení](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Na hello **přidat test** okno, zadejte název hello hello kontroly.
3. Vyberte hello **protokol** u testu paměti hello. Pro databázi můžete chtít sondou TCP, nikoli sondu HTTP. toolearn Další informace o vyrovnávání zatížení sondy odkazovat příliš[sondy nástroje pro vyrovnávání zatížení Rady pro pochopení](../load-balancer/load-balancer-custom-probe-overview.md).
4. Zadejte hello **Port** z toobe vaše databáze používá pro přístup k testu hello.
5. V části **Interval**, zadejte, jak často tooprobe hello aplikace.
6. V části **prahová hodnota špatného stavu**, zadejte číslo hello průběžné kontroly chyb, které se musí vyskytovat u hello back-end virtuálních počítačů toobe považoval za poškozený.
7. Klikněte na tlačítko **OK** toocreate hello testu.

#### <a name="configure-hello-load-balancing-rules"></a>Konfigurace pravidel Vyrovnávání zatížení hello

1. V části **nastavení** nástroj pro vyrovnávání zatížení, vyberte **pravidla Vyrovnávání zatížení**a potom klikněte na **přidat** toocreate pravidlo.
2. Na hello **pravidlo Vyrovnávání zatížení přidat** okno, zadejte hello **název** pro pravidlo Vyrovnávání zatížení hello.
3. Zvolte hello **front-endovou IP adresu** z hello vyrovnávání zátěže, **protokol**, a **Port**.
4. V části **back-endový port**, zadejte toobe port hello používá ve fondu back-end hello.
5. Vyberte hello **fond back-end** a hello **testu** vytvořených v hello předchozí kroky tooapply hello pravidlo.
6. V části **trvalost relace**, vyberte, jak chcete toopersist relací hello.
7. V části **časové limity nečinnosti**, zadejte hello počet minut, než časový limit nečinnosti.
8. V části **plovoucí IP adresa**, vyberte buď **zakázané** nebo **povoleno**.
9. Klikněte na tlačítko **OK** toocreate hello pravidlo.

### <a name="step-5-connect-web-tier-vms-toohello-load-balancer"></a>Krok 5: Připojení Vyrovnávání zatížení toohello webovou vrstvu virtuálních počítačů

Nyní nakonfigurujeme hello IP adres a vyrovnávání zatížení front-end port v hello aplikacích, které jsou spuštěny na webovou vrstvu virtuálních počítačů pro všechna připojení databáze. Tato konfigurace je konkrétní toohello aplikace, které běží na těchto virtuálních počítačích. tooconfigure hello cílovou IP adresu a port, naleznete v dokumentaci toohello aplikace. toofind hello IP adresu hello front-endu, v hello portál Azure, přejděte na hello fond IP front-endu toohello **nastavení nástroje pro vyrovnávání zatížení** okno.

![Navigační podokno "IP front-endu fond" nástroje pro vyrovnávání zatížení](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Další kroky

* [Přehled služby Traffic Manager](traffic-manager-overview.md)
* [Přehled brány aplikace](../application-gateway/application-gateway-introduction.md)
* [Azure Load Balancer – přehled](../load-balancer/load-balancer-overview.md)
