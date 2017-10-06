---
title: "aaaLoad vyrovnávání na víc konfigurací IP adres v Azure | Microsoft Docs"
description: "Vyrovnávání zatížení napříč primární a sekundární konfigurace protokolu IP."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 8619493b8102e9d158d428fe6c59ecf3f32edc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-hello-azure-portal"></a>Na víc konfigurací IP adres pomocí portálu Azure hello Vyrovnávání zatížení

> [!div class="op_single_selector"]
> * [Azure Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [Rozhraní příkazového řádku](load-balancer-multiple-ip-cli.md)

Tento článek popisuje, jak toouse Vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC). V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér. Každý z hello sekundární síťové adaptéry mají dvě konfigurace protokolu IP. Každý virtuální počítač hostuje weby contoso.com a fabrikam.com. Každý web je vázané tooone hello konfigurace protokolu IP v síťovém adaptéru hello sekundární. Můžeme použít nástroj pro vyrovnávání zatížení Azure tooexpose dvě front-end IP adresy, jednu pro každý web, toodistribute provoz toohello odpovídající konfiguraci protokolu IP pro web hello. Tento scénář používá hello stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a>Požadavky
Tento příklad předpokládá, že máte skupinu prostředků s názvem *contosofabrikam* s hello následující konfigurace:
 -  obsahuje virtuální síť s názvem *myVNet*, dva virtuální počítače, které volá *VM1* a *virtuálního počítače 2* v uvedeném pořadí uvnitř stejné hello dostupnosti s názvem *myAvailset*. 
 - Každý virtuální počítač má síťový adaptér primární a sekundární síťový adaptér. Hello primární síťové adaptéry jsou pojmenované *VM1NIC1* a *VM2NIC1* a hello sekundární síťové adaptéry jsou pojmenované *VM1NIC2* a *VM2NIC2*. Další informace o vytváření virtuálních počítačů s více síťovými kartami najdete v tématu [vytvoření virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a>Vyrovnávat tooload kroky na víc konfigurací IP adres

Postupujte podle těchto kroků tooachieve hello scénář popsaný v tomto článku:

### <a name="step-1-configure-hello-secondary-nics-for-each-vm"></a>Krok 1: Konfigurace hello sekundární síťové adaptéry pro každý virtuální počítač

Pro každý virtuální počítač ve virtuální síti, přidejte sadu konfiguraci protokolu IP pro hello sekundární síťový adaptér následujícím způsobem:  

1. V prohlížeči přejděte toohello portálu Azure: http://portal.azure.com a přihlaste se pomocí účtu Azure.
2. V hello nejvyšší levé straně obrazovky hello, klikněte na ikonu hello skupinu prostředků a potom klikněte hello skupiny prostředků hello virtuální počítače jsou umístěné v (například *contosofabrikam*). Hello **skupiny prostředků** se nyní zobrazí okno, které jsou uvedeny všechny prostředky hello společně s hello síťová rozhraní pro hello virtuálních počítačů.
3. toohello sekundární síťový adaptér každého virtuálního počítače, přidejte konfiguraci IP adres následujícím způsobem:
    1. Vyberte hello síťové rozhraní, které chcete tooadd hello konfiguraci IP.
    2. V hello zobrazeném okně pro hello síťové adaptéry, které jste vybrali, klikněte na **konfigurace protokolu IP**. Pak klikněte na tlačítko **přidat** směrem hello horní části okna hello, který se zobrazí.
    3. V hello **přidat IP konfigurace** okně přidejte druhý toohello konfigurace IP seskupování následujícím způsobem: 
        1. Zadejte název vaší sekundární konfiguraci IP adresy (například pro VM1 a virtuálního počítače 2 název hello konfigurací IP adres jako *VM1NIC2 ipconfig2* a *VM2NIC2 ipconfig2* v uvedeném pořadí).
        2. Pro **privátní IP adresa**, pro **přidělení**, vyberte **statické**.
        3. Klikněte na **OK**.
        4. Když hello druhý konfiguraci protokolu IP pro hello sekundární síťový adaptér je kompletní, zobrazí se v hello **konfigurace protokolu IP** okno nastavení pro hello Zadaný síťový adaptér.

### <a name="step-2-create-a-load-balancer"></a>Krok 2: Vytvoření služby Vyrovnávání zatížení

Nástroj pro vyrovnávání zatížení vytvořte takto:

1. V prohlížeči přejděte toohello portálu Azure: http://portal.azure.com a přihlaste se pomocí účtu Azure.
2. Na hello nejvyšší levé straně úvodní obrazovka, klikněte na tlačítko **nový** > **sítě** > **nástroj pro vyrovnávání zatížení**. Klikněte na tlačítko **vytvořit**.
3. V hello **vytvořit nástroj pro vyrovnávání zatížení** okno, zadejte název pro nástroj pro vyrovnávání zatížení. Zde je volána *mylb*.
4. V části veřejnou IP adresu vytvořit novou veřejnou IP adresu volat **PublicIP1**.
5. Ve skupině prostředků, vyberte hello existující skupiny prostředků virtuálních počítačů (například *contosofabrikam*). Pak vyberte odpovídající umístění a pak klikněte na tlačítko **OK**. Nástroj pro vyrovnávání zatížení Hello se pak spustí toodeploy a bude trvat několik minut toosuccessfully dokončení nasazení.
6. Po nasazení nástroje pro vyrovnávání zatížení hello se zobrazí jako prostředek ve vaší skupině prostředků.

### <a name="step-3-configure-hello-frontend-ip-pool"></a>Krok 3: Konfigurace fondu IP front-endu hello

Konfigurujte fond IP front-endu pro každý web (Contoso a Fabrikam) následujícím způsobem:

1. Hello portálu, klikněte na tlačítko **další služby** > typ **veřejnou IP adresu** v hello pole filtru a potom klikněte na **veřejné IP adresy**. Klikněte na tlačítko **přidat** směrem hello horní části okna hello, který se zobrazí.
2. Nakonfigurovat dvě veřejné IP adresy (*PublicIP1* a *PublicIP2*) pro oba weby (contoso a fabrikam) následujícím způsobem:
    1. Zadejte název pro vaše IP adresa front-endu.
    2. Pro **skupiny prostředků**, vyberte hello existující skupiny prostředků hello virtuální počítače (například *contosofabrikam*).
    3. Pro **umístění**, vyberte hello stejné umístění jako hello virtuálních počítačů.
    4. Klikněte na **OK**.
    5. Po vytvoření hello dvě veřejné IP adresy jsou obě zobrazená v hello **veřejnou IP adresu** okno adresy.
3. Hello portálu, klikněte na tlačítko **další služby** > typ **nástroj pro vyrovnávání zatížení** v hello pole filtru a potom klikněte na **Vyrovnávání zatížení**.  
4. Vyberte nástroj pro vyrovnávání zatížení hello (*mylb*), které chcete tooadd hello front-endovou IP fondu.
5. V části **nastavení**, vyberte **front-endové fondy**. Pak klikněte na tlačítko **přidat** směrem hello horní části okna hello, který se zobrazí.
6. Zadejte název pro vaše IP adresa front-endu (*farbikamfe* nebo **contosofe*).
7. Klikněte na tlačítko **IP adresu** a na hello **zvolte veřejnou IP adresu** okně, vyberte hello IP adres pro vaší front-endu (*PublicIP1* nebo *PublicIP2*).
8. Zopakujte kroky 3 too7 v této části toocreate hello druhou front-endovou IP adresu.
9. Po dokončení konfigurace fondu IP front-endu hello obě IP adresy front-endu jsou zobrazeny v hello **fond IP front-endu** okno Nástroj pro vyrovnávání zatížení. 
    
### <a name="step-4-configure-hello-backend-pool"></a>Krok 4: Nakonfigurujte fond back-end hello   
Konfigurujte fondy adres back-end hello na nástroj pro vyrovnávání zatížení pro každý web (Contoso a Fabrikam) následujícím způsobem:
        
1. Hello portálu, klikněte na tlačítko **další služby** > zadejte do pole filtru hello nástroj pro vyrovnávání zatížení a pak klikněte na **Vyrovnávání zatížení**.  
2. Vyberte nástroj pro vyrovnávání zatížení hello (*mylb*), které chcete tooadd hello fondy back-end.
3. V části **nastavení**, vyberte **back-endové fondy**. Zadejte název pro fond back-end (například *contosopool* nebo *fabrikampool*). Pak klikněte na tlačítko hello **přidat** tlačítko směrem k hello horní části okna hello, který se zobrazí. 
4. Pro **přidružené k**, vyberte **sadu dostupnosti**.
5. Pro **sadu dostupnosti**, vyberte **myAvailset**.
6. Přidejte cílový konfigurace IP sítě, pro oba virtuální počítače (viz obrázek 2):  
    1. Pro **cílový virtuální počítač**, vyberte hello virtuálních počítačů, které chcete tooadd toohello back-end fondu (například VM1 nebo virtuálního počítače 2).
    2. Pro **konfiguraci IP adresy sítě**vyberte hello sekundární konfigurace IP síťových adaptérů pro tento virtuální počítač (například VM1NIC2 ipconfig2 nebo VM2NIC2 ipconfig2).
    ![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
            
        **Obrázek 2**: Konfigurace hello nástroj pro vyrovnávání zatížení s back-endové fondy  
7. Klikněte na **OK**.
8. Po dokončení konfigurace fondu back-end hello i back-endové fondy adres se zobrazují v hello **okno fond back-end** z nástroj pro vyrovnávání zatížení.

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a>Krok 5: Konfigurace test stavu pro nástroj pro vyrovnávání zatížení
Test stavu pro nástroj pro vyrovnávání zatížení nakonfigurujte následujícím způsobem:
    1. Hello portálu, klikněte na tlačítko Další služby > zadejte do pole filtru hello nástroj pro vyrovnávání zatížení a pak klikněte na **Vyrovnávání zatížení**.  
    2. Vyberte hello Vyrovnávání zatížení, která chcete tooadd hello fondy back-end.
    3. V části **nastavení**, vyberte **test stavu**. Pak klikněte na tlačítko **přidat** směrem hello horní části okna hello, který se zobrazí.
    4. Zadejte název pro test stavu hello (například protokol HTTP) a klikněte na tlačítko **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>Krok 6: Konfigurace pravidel Vyrovnávání zatížení
Konfigurace pravidel Vyrovnávání zatížení (*HTTPc* a *HTTPf*) pro každý web následujícím způsobem:
    
1. V části **nastavení**, vyberte **test stavu**. Pak klikněte na tlačítko **přidat** směrem hello horní části okna hello, který se zobrazí.
2. Pro **název**, zadejte název pro pravidlo Vyrovnávání zatížení hello (například *HTTPc* pro společnost Contoso, nebo *HTTPf* pro společnost Fabrikam)
3. Pro adresu IP front-endu, vyberte hello hello front-endovou IP adresu (například *Contosofe* nebo *Fabrikamfe*)
4. Pro **Port** a **back-endový port**, ponechte výchozí hodnotu hello **80**.
5. Pro **plovoucí IP adresa (přímá odpověď ze serveru)**, klikněte na tlačítko **povoleno**.
6. Klikněte na **OK**.
7. Opakujte kroky 1 too6 v této části toocreate hello druhý nástroj pro vyrovnávání zatížení pravidlo.
8. Pokud Vyrovnávání zatížení hello pravidla konfigurace je hotová, jak pravidla ((*HTTPc* a *HTTPf*) se zobrazí v hello **pravidla Vyrovnávání zatížení** okno Nástroj pro vyrovnávání zatížení.

### <a name="step-7-configure-dns-records"></a>Krok 7: Nakonfigurovat záznamy DNS
Nakonec musíte nakonfigurovat DNS prostředků záznamů toopoint toohello příslušných front-endovou IP adresu služby Vyrovnávání zatížení hello. Může hostovat vaše domény v Azure DNS. Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Další kroky
- Další informace o tom, jak Vyrovnávání zatížení toocombine služby v Azure v [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Zjistěte, jak můžete použít různé typy protokolů v Azure toomanage a řešení vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).
