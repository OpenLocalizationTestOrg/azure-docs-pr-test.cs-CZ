---
title: "Víc konfigurací IP adres v Azure pro vyrovnávání zatížení | Microsoft Docs"
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
ms.openlocfilehash: cf1e68c7b37b2506de007bdf24eea63a27187a33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-the-azure-portal"></a>Víc konfigurací IP adres pomocí portálu Azure pro vyrovnávání zatížení

> [!div class="op_single_selector"]
> * [Azure Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [Rozhraní příkazového řádku](load-balancer-multiple-ip-cli.md)

Tento článek popisuje, jak používat nástroj pro vyrovnávání zatížení Azure s více IP adres v sekundárním síťovém rozhraní (NIC). V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťový adaptér. Každé sekundární síťové karty má dvě konfigurace protokolu IP. Každý virtuální počítač hostuje weby contoso.com a fabrikam.com. Každý web je vázána na jednu z konfigurace protokolu IP v sekundárním síťovém adaptéru. Nástroj pro vyrovnávání zatížení Azure používáme ke zveřejnění dvě front-end IP adresy, jednu pro každý web, distribuovat provoz do příslušných konfiguraci protokolu IP pro web. Tento scénář používá stejné číslo portu mezi frontends, jak oba back-end fondu IP adres.

![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a>Požadavky
Tento příklad předpokládá, že máte skupinu prostředků s názvem *contosofabrikam* s následující konfigurací:
 -  obsahuje virtuální síť s názvem *myVNet*, dva virtuální počítače, které volá *VM1* a *virtuálního počítače 2* v uvedeném pořadí v rámci stejné skupině s názvem dostupnosti *myAvailset*. 
 - Každý virtuální počítač má síťový adaptér primární a sekundární síťový adaptér. Primární síťové adaptéry jsou pojmenované *VM1NIC1* a *VM2NIC1* a sekundární síťové adaptéry jsou pojmenované *VM1NIC2* a *VM2NIC2*. Další informace o vytváření virtuálních počítačů s více síťovými kartami najdete v tématu [vytvoření virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Postup na víc konfigurací IP adres Vyrovnávání zatížení

Postupujte podle těchto pokynů dosáhnout scénáři uvedeném v tomto článku:

### <a name="step-1-configure-the-secondary-nics-for-each-vm"></a>Krok 1: Konfigurace sekundární síťové adaptéry pro každý virtuální počítač

Pro každý virtuální počítač ve virtuální síti přidejte sadu konfiguraci protokolu IP pro sekundární síťový adaptér následujícím způsobem:  

1. V prohlížeči přejděte na portálu Azure: http://portal.azure.com a přihlaste se pomocí účtu Azure.
2. Na levé straně horní části obrazovky, klikněte na ikonu skupiny prostředků a potom klikněte na skupinu prostředků virtuálních počítačů, které se nacházejí v (například *contosofabrikam*). **Skupiny prostředků** se nyní zobrazí okno, které jsou uvedeny všechny prostředky, které společně s rozhraní sítě pro virtuální počítače.
3. Na sekundární síťový adaptér každého virtuálního počítače přidejte konfiguraci IP adres následujícím způsobem:
    1. Vyberte síťové rozhraní, které chcete přidat konfigurace IP adresy na.
    2. V okně, který se zobrazí pro síťový adaptér, který jste vybrali, klikněte na tlačítko **konfigurace protokolu IP**. Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.
    3. V **přidat IP konfigurace** okně přidejte druhý konfigurace protokolu IP na síťový adaptér následujícím způsobem: 
        1. Zadejte název vaší sekundární konfiguraci IP adresy (například pro VM1 a virtuálního počítače 2 název konfigurace protokolu IP jako *VM1NIC2 ipconfig2* a *VM2NIC2 ipconfig2* v uvedeném pořadí).
        2. Pro **privátní IP adresa**, pro **přidělení**, vyberte **statické**.
        3. Klikněte na **OK**.
        4. Po dokončení druhé konfiguraci protokolu IP pro sekundární síťový adaptér se zobrazí v **konfigurace protokolu IP** okno nastavení pro daný síťový adaptér.

### <a name="step-2-create-a-load-balancer"></a>Krok 2: Vytvoření služby Vyrovnávání zatížení

Nástroj pro vyrovnávání zatížení vytvořte takto:

1. V prohlížeči přejděte na portálu Azure: http://portal.azure.com a přihlaste se pomocí účtu Azure.
2. Na levé straně horní části obrazovky, klikněte na tlačítko **nový** > **sítě** > **nástroj pro vyrovnávání zatížení**. Klikněte na tlačítko **vytvořit**.
3. V okně **Vytvořit nástroj pro vyrovnávání zatížení** zadejte název svého nástroje pro vyrovnávání zatížení. Zde je volána *mylb*.
4. V části veřejnou IP adresu vytvořit novou veřejnou IP adresu volat **PublicIP1**.
5. Ve skupině prostředků, vyberte existující skupinu prostředků virtuálních počítačů (například *contosofabrikam*). Pak vyberte odpovídající umístění a pak klikněte na tlačítko **OK**. Nástroj pro vyrovnávání zatížení se začne nasazovat. Úspěšné dokončení nasazení potrvá několik minut.
6. Po nasazení nástroje pro vyrovnávání zatížení se zobrazí jako prostředek ve vaší skupině prostředků.

### <a name="step-3-configure-the-frontend-ip-pool"></a>Krok 3: Konfigurace fondu IP front-endu

Konfigurujte fond IP front-endu pro každý web (Contoso a Fabrikam) následujícím způsobem:

1. Na portálu, klikněte na tlačítko **další služby** > typ **veřejnou IP adresu** pole filtru, a pak klikněte na **veřejné IP adresy**. Klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.
2. Nakonfigurovat dvě veřejné IP adresy (*PublicIP1* a *PublicIP2*) pro oba weby (contoso a fabrikam) následujícím způsobem:
    1. Zadejte název pro vaše IP adresa front-endu.
    2. Pro **skupiny prostředků**, vyberte existující skupinu prostředků virtuálních počítačů (například *contosofabrikam*).
    3. Pro **umístění**, vyberte stejné umístění jako virtuální počítače.
    4. Klikněte na **OK**.
    5. Po vytvoření jsou dvě veřejné IP adresy, jsou obě zobrazená v **veřejnou IP adresu** okno adresy.
3. Na portálu, klikněte na tlačítko **další služby** > typ **nástroj pro vyrovnávání zatížení** pole filtru, a pak klikněte na **Vyrovnávání zatížení**.  
4. Vyberte pro vyrovnávání zatížení (*mylb*), které chcete přidat fondu IP front-endu.
5. V části **nastavení**, vyberte **front-endové fondy**. Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.
6. Zadejte název pro vaše IP adresa front-endu (*farbikamfe* nebo **contosofe*).
7. Klikněte na tlačítko **IP adresu** a na **zvolte veřejnou IP adresu** okně vyberte IP adresy pro vaší front-endu (*PublicIP1* nebo *PublicIP2*).
8. Zopakujte kroky 3 až 7 v této části, chcete-li vytvořit druhou IP adresu front-endu.
9. Po dokončení konfigurace fondu IP front-endu obě IP adresy front-endu jsou zobrazeny v **fond IP front-endu** okno Nástroj pro vyrovnávání zatížení. 
    
### <a name="step-4-configure-the-backend-pool"></a>Krok 4: Konfigurace fondu back-end   
Konfigurace back-endové fondy adres na nástroj pro vyrovnávání zatížení pro každý web (Contoso a Fabrikam) následujícím způsobem:
        
1. Na portálu, klikněte na tlačítko **další služby** > zadejte nástroj pro vyrovnávání zatížení do pole Filtr a potom klikněte na **Vyrovnávání zatížení**.  
2. Vyberte pro vyrovnávání zatížení (*mylb*), které chcete přidat back-endové fondy k.
3. V části **nastavení**, vyberte **back-endové fondy**. Zadejte název pro fond back-end (například *contosopool* nebo *fabrikampool*). Klikněte **přidat** tlačítko k horní části okna, který se zobrazí. 
4. Pro **přidružené k**, vyberte **sadu dostupnosti**.
5. Pro **sadu dostupnosti**, vyberte **myAvailset**.
6. Přidejte cílový konfigurace IP sítě, pro oba virtuální počítače (viz obrázek 2):  
    1. Pro **cílový virtuální počítač**, vyberte virtuální počítač, který chcete přidat do fondu back-end (například VM1 nebo virtuálního počítače 2).
    2. Pro **konfiguraci IP adresy sítě**, vyberte sekundární konfiguraci IP síťových adaptérů pro tento virtuální počítač (například VM1NIC2 ipconfig2 nebo VM2NIC2 ipconfig2).
    ![Scénář LB bitové kopie](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
            
        **Obrázek 2**: konfigurace pro vyrovnávání zatížení s back-endové fondy  
7. Klikněte na **OK**.
8. Po dokončení konfigurace fondu back-end i back-endové fondy adres se zobrazují v **okno fond back-end** z nástroj pro vyrovnávání zatížení.

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a>Krok 5: Konfigurace test stavu pro nástroj pro vyrovnávání zatížení
Test stavu pro nástroj pro vyrovnávání zatížení nakonfigurujte následujícím způsobem:
    1. Na portálu, klikněte na tlačítko Další služby > zadejte nástroj pro vyrovnávání zatížení do pole Filtr a potom klikněte na **Vyrovnávání zatížení**.  
    2. Vyberte, kterou chcete přidat fondy back-end pro vyrovnávání zatížení.
    3. V části **nastavení**, vyberte **test stavu**. Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.
    4. Zadejte název pro test stavu (například protokol HTTP) a klikněte na tlačítko **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>Krok 6: Konfigurace pravidel Vyrovnávání zatížení
Konfigurace pravidel Vyrovnávání zatížení (*HTTPc* a *HTTPf*) pro každý web následujícím způsobem:
    
1. V části **nastavení**, vyberte **test stavu**. Pak klikněte na tlačítko **přidat** směrem nahoru v okně, který se zobrazí.
2. Pro **název**, zadejte název pro pravidlo Vyrovnávání zatížení (například *HTTPc* pro společnost Contoso, nebo *HTTPf* pro společnost Fabrikam)
3. Pro adresu IP front-endu, vyberte front-endovou IP adresu (například *Contosofe* nebo *Fabrikamfe*)
4. Pro **Port** a **back-endový port**, ponechte výchozí hodnotu **80**.
5. Pro **plovoucí IP adresa (přímá odpověď ze serveru)**, klikněte na tlačítko **povoleno**.
6. Klikněte na **OK**.
7. Opakujte kroky 1 až 6 v rámci této části vytvoříte druhé pravidlo Vyrovnávání zatížení.
8. Pokud Vyrovnávání zatížení pravidla konfigurace je hotová, jak pravidla ((*HTTPc* a *HTTPf*) se zobrazí v **pravidla Vyrovnávání zatížení** okno Nástroj pro vyrovnávání zatížení.

### <a name="step-7-configure-dns-records"></a>Krok 7: Nakonfigurovat záznamy DNS
Nakonec je nutné nakonfigurovat záznamy prostředků DNS tak, aby odkazoval na příslušné front-endovou IP adresu služby Vyrovnávání zatížení. Může hostovat vaše domény v Azure DNS. Další informace o používání s nástrojem pro vyrovnávání zatížení Azure DNS najdete v tématu [pomocí Azure DNS s jinými službami Azure](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Další kroky
- Další informace o tom, jak kombinací v Azure v rámci služby Vyrovnávání zatížení [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Zjistěte, jak můžete použít různé typy protokolů v Azure ke správě a odstraňování potíží pro vyrovnávání zatížení v [protokolu pro vyrovnávání zatížení Azure analytics](../load-balancer/load-balancer-monitor-log.md).
