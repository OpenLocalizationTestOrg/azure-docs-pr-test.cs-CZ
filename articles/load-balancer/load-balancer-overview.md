---
title: "Přehled nástroje pro vyrovnávání zatížení aaaAzure | Microsoft Docs"
description: "Přehled funkce Vyrovnávání zatížení Azure, architekturu a implementace. Zjistěte, jak funguje nástroj pro vyrovnávání zatížení hello a využít v cloudu hello."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 2376a02f7cbbbed6a90f216419c0c3d30f594272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-load-balancer-overview"></a>Azure Load Balancer – přehled

Azure nástroj pro vyrovnávání zatížení zajišťuje vysoké dostupnosti a výkonu v síti tooyour aplikace. Je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která distribuuje příchozí komunikaci mezi pořádku instancí služby definované v sadě s vyrovnáváním zatížení.

Azure nástroj pro vyrovnávání zatížení můžete nakonfigurovat:

* Vyrovnávat zatížení příchozí internetové přenosy toovirtual počítačů. Tato konfigurace se označuje jako [Vyrovnávání zatížení internetového](load-balancer-internet-overview.md).
* Přenosy Vyrovnávání zatížení mezi virtuálními počítači ve virtuální síti, mezi virtuálními počítači v cloudové služby nebo mezi místními počítači a virtuální počítače ve virtuální síti mezi různými místy. Tato konfigurace se označuje jako [Vyrovnávání zatížení pro vnitřní](load-balancer-internal-overview.md).
* Předat dál externích přenosů tooa konkrétní virtuální počítač.

Všechny prostředky v cloudu hello potřebovat dosažitelný z Internetu hello veřejné toobe adres IP. Hello cloudové infrastruktury v Azure používá směrovat IP adresy pro její prostředky. Azure používá překlad síťových adres (NAT) s veřejné IP adresy toocommunicate toohello Internetu.

## <a name="azure-deployment-models"></a>Modelech nasazení Azure

Je důležité toounderstand hello rozdíly mezi hello Azure classic a Resource Manager [modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md). Azure nástroj pro vyrovnávání zatížení je nakonfigurována jinak než v každém modelu.

### <a name="azure-classic-deployment-model"></a>Model nasazení Azure classic

Virtuální počítače nasazené v rámci hranice cloudové služby může být seskupené toouse nástroj pro vyrovnávání zatížení. V tomto modelu veřejnou IP adresu a plně kvalifikovaný název domény, (FQDN) jsou přiřazeny tooa cloudové služby. Nástroj pro vyrovnávání zatížení Hello portu překlad a vyrovnává zatížení hello síťového provozu pomocí hello veřejnou IP adresu pro hello cloudové služby.

Provoz s vyrovnáváním zatížení je definována pomocí koncových bodů. Koncové body překlad portu jsou v relaci mezi hello přiřazené veřejný port hello veřejné IP adresy a hello místního portu přiřazené toohello službu na konkrétní virtuální počítač. Koncové body Vyrovnávání zatížení mít vztah jeden mnoho mezi hello veřejnou IP adresu a hello místní porty přiřazené toohello služby hello virtuálních počítačů v rámci hello cloudové služby.

![Azure pro vyrovnávání zatížení v modelu nasazení classic hello](./media/load-balancer-overview/asm-lb.png)

Obrázek 1 – Azure pro vyrovnávání zatížení v modelu nasazení classic hello

Popisek domény Hello hello veřejnou IP adresu, která hello používá nástroj pro vyrovnávání zatížení pro tento model nasazení je \<název cloudové služby\>. cloudapp.net. Hello následující obrázek znázorňuje hello nástroj pro vyrovnávání zatížení Azure v tomto modelu.

### <a name="azure-resource-manager-deployment-model"></a>Model nasazení Azure Resource Manager

V modelu nasazení Resource Manager hello je bez nutnosti toocreate cloudové služby. Nástroj pro vyrovnávání zatížení Hello se vytvoří tooexplicitly směrovat provoz mezi více virtuálními počítači.

Veřejná IP adresa je jednotlivých prostředků, který má popisek domény (název DNS). Hello veřejná IP adresa je přidružen hello prostředek pro vyrovnávání zatížení. Pravidla nástroje pro vyrovnávání zatížení a příchozího pravidla NAT použít hello veřejnou IP adresu jako hello Internet koncový bod pro hello prostředky, které jsou přijímá provoz s vyrovnáváním zatížení sítě.

Privátní nebo veřejné IP adresa je přiřazen toohello síťové rozhraní prostředků připojené tooa virtuálního počítače. Po přidání fondu vyrovnávání zatížení tooa back-end IP adres rozhraní sítě hello nástroj pro vyrovnávání zatížení je možné toosend Vyrovnávání zatížení síťových přenosů na základě Vyrovnávání zatížení sítě pravidla hello, které jsou vytvořeny.

Hello následující obrázek ukazuje hello nástroj pro vyrovnávání zatížení Azure v tomto modelu:

![Pro vyrovnávání zatížení Azure ve službě Správce prostředků](./media/load-balancer-overview/arm-lb.png)

Obrázek 2 – ve službě Správce prostředků pro vyrovnávání zatížení Azure

Nástroj pro vyrovnávání zatížení Hello je možné spravovat prostřednictvím šablony založené na správci prostředků, rozhraní API a nástroje. toolearn víc o službě Správce prostředků, najdete v části hello [Přehled služby Správce prostředků](../azure-resource-manager/resource-group-overview.md).

## <a name="load-balancer-features"></a>Funkce nástroje pro vyrovnávání zatížení

* Na základě hodnoty hash distribuce

    Azure nástroj pro vyrovnávání zatížení používá algoritmus hash na základě distribuce. Ve výchozím nastavení používá složené zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port a protokol typ toomap provoz tooavailable serverů hash 5 řazené kolekce členů. Poskytuje věrnosti pouze *v rámci* relace přenosu. Pakety hello stejné relace TCP nebo UDP bude přesměruje toohello stejné instance za koncový bod Vyrovnávání zatížení sítě hello. Pokud se klient hello zavře a znovu otevře hello připojení nebo spustí novou relaci z hello stejné zdrojové IP adresy, hello zdrojového portu změny. To může způsobit hello provoz toogo tooa jinému koncovému bodu v jiném datovém centru.

    Další podrobnosti najdete v tématu [režim distribuce nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md). Hello následující obrázek ukazuje distribuci založenou na hash hello:

    ![Na základě hodnoty hash distribuce](./media/load-balancer-overview/load-balancer-distribution.png)

    Obrázek 3 - Hash na základě distribuce

* Přesměrování portu

    Azure poskytuje nástroj pro vyrovnávání zatížení můžete řídit, je spravovaný přes jak příchozí komunikaci. Tato komunikace zahrnuje provozu iniciované z hostitele Internet, virtuální počítače v jiných cloudových služeb nebo virtuálních sítí. Tento ovládací prvek je reprezentována koncový bod (také nazývané vstupní koncový bod).

    Vstupní koncový bod naslouchá na veřejný port a předává přenos tooan interní port. Můžete namapovat hello stejné porty pro koncový bod interních nebo externích nebo použít jiný port pro ně. Například můžete mít webový server nakonfigurovaný toolisten tooport 81, při mapování hello veřejný koncový bod je port 80. Vytvoření Hello veřejný koncový bod se aktivuje hello vytvoření instance služby Vyrovnávání zatížení.

    Když vytvořené pomocí hello portálu Azure, portál hello automaticky vytvoří koncové body toohello virtuální počítač pro hello protokolu RDP (Remote Desktop) a vzdálenou komunikaci relace prostředí Windows PowerShell. Můžete použít tyto koncové body tooremotely spravovat hello virtuálního počítače přes hello Internet.

* Automatické překonfigurování

    Azure nástroj pro vyrovnávání zatížení okamžitě automatických při změně velikosti instancí nahoru nebo dolů. Například tuto změnu konfigurace se stane, když zvýšíte hello počet instancí pro web/role pracovního procesu v cloudové službě nebo při přidání dalších virtuálních počítačů do hello nastavit stejné Vyrovnávání zatížení sítě.

* Monitorování služeb

    Azure nástroj pro vyrovnávání zatížení můžete testů hello stav hello různé instance serveru. Pokud sondu nezdaří toorespond, nástroj pro vyrovnávání zatížení hello zastaví odesílání nových připojení není v pořádku instance toohello. Nejsou ovlivněná existující připojení.

    Jsou podporovány tři typy sondy:

    + **Test agenta hosta (na platformě jako služby pouze ve virtuálních počítačích):** hello nástroj pro vyrovnávání zatížení využívá agenta hosta hello uvnitř hello virtuálního počítače. Hello agenta hosta naslouchá a odpoví odpověď HTTP 200 OK jenom v případě hello instance je ve stavu Připraveno hello (tj. hello instance není ve stavu jako o volném čase, recyklace nebo ukončení). V případě selhání toorespond se HTTP 200 OK hello agenta značky nástroje pro vyrovnávání zatížení hello hello instance jako reagovat a zastaví odesílání přenosů toothat instance. Nástroj pro vyrovnávání zatížení Hello pokračuje tooping hello instance. Pokud agenta hosta hello odpoví HTTP 200, odešle nástroj pro vyrovnávání zatížení hello provoz toothat instance znovu. Pokud používáte webovou roli, kód webu obvykle běží v w3wp.exe, který není monitorován pomocí hello prostředků infrastruktury Azure nebo agenta hosta. To znamená, že selhání v w3wp.exe (např. odpovědi HTTP 500) nebude agent hosta hlášené toohello a nástroj pro vyrovnávání zatížení hello nebude vědět tootake tuto instanci mimo otočení.
    + **Vlastní test paměti HTTP:** tento test přepsání hello výchozí (agenta hosta) kontroly. Můžete ji toocreate použít vlastní stav vlastní logiky toodetermine hello hello role instance. Nástroj pro vyrovnávání zatížení Hello bude pravidelně testů váš koncový bod (každých 15 sekund, ve výchozím nastavení). Hello instance je považován za toobe v otočení, pokud ji odpoví TCP ACK nebo HTTP 200 v časovém limitu hello (výchozí hodnoty 31 sekund). To je užitečné pro implementaci vlastní logiky tooremove instancí z otočení hello služby Vyrovnávání zatížení. Můžete například nakonfigurovat hello instance tooreturn stav bez 200, pokud hello instance je vyšší než 90 % využití procesoru. Pro webové role, které používají w3wp.exe, můžete také získat automatické monitorování vašeho webu, protože selhání ve vašem kódu webu vrátit sondu toohello stav bez 200.
    + **Vlastní test paměti TCP:** tento test spoléhá na úspěšné TCP relace navázání tooa definované testu portu.

    Další informace najdete v tématu hello [LoadBalancerProbe schématu](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Zdroj NAT

    Všechny odchozí přenosy toohello zde nevyskytlo Internet, která pochází z vaší služby zdroje NAT (SNAT) pomocí hello stejnou adresu VIP jako hello příchozí přenosy. Překládat pomocí SNAT poskytuje důležité výhody:

    + Umožňuje snadno upgradu a zotavení po havárii služeb, protože hello VIP může být dynamicky namapované tooanother instanci služby hello.
    + Ho usnadňuje správu seznamu ACL řízení přístupu. Seznamy ACL, vyjádřené jako virtuální IP adresy neměňte jako služby škálování nahoru, dolů nebo získat znovu nasazena.

    Konfigurace služby Vyrovnávání zatížení Hello podporuje úplné kuželový NAT pro UDP. Úplné kuželový NAT je typ zařízení NAT, kde hello portu umožňuje příchozí připojení z jakékoli externího hostitele (v odpovědi tooan odchozí požadavku).

    Pro každý nový odchozí připojení, které zahájí virtuálního počítače je nástroj pro vyrovnávání zatížení hello také přidělena odchozí port. Hello externího hostitele vidí provoz s virtuální port přidělené virtuální IP adresy IP. Pro scénáře, které vyžadují hodně odchozí připojení, je doporučeno toouse [veřejná IP adresa na úrovni instance](../virtual-network/virtual-networks-instance-level-public-ip.md) adresy tak, aby virtuální počítače hello odchozí vyhrazenou IP adresu pro překládat pomocí SNAT. Tím se snižuje riziko hello vyčerpání portu.

    Najdete v tématu [odchozí připojení](load-balancer-outbound-connections.md) článku Další podrobnosti k tomuto tématu.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Podpora pro více Vyrovnávání zatížení sítě IP adres pro virtuální počítače
Můžete přiřadit více než jeden Vyrovnávání zatížení sítě veřejnou IP adresu tooa sada virtuálních počítačů. Se tato možnost může být hostitelem více webů SSL nebo více naslouchací procesy skupiny dostupnosti AlwaysOn serveru SQL na hello stejnou sadu virtuálních počítačů. Další informace najdete v tématu [několika virtuálními IP adresami za cloudové služby](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Omezení

Fondy back-end pro vyrovnávání zatížení může obsahovat všechny SKU virtuálních počítačů s výjimkou základní vrstvě.

## <a name="next-steps"></a>Další kroky

- Další informace o [internetového nástroj pro vyrovnávání zatížení](load-balancer-internet-overview.md)

- Další informace o [přehled nástroje pro vyrovnávání zatížení interní](load-balancer-internal-overview.md)

- Vytvoření [internetového nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-portal.md)

- Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure

