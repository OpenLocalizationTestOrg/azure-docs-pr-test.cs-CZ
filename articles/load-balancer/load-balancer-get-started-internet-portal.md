---
title: "Vyrovnávání zatížení aaaCreate internetové - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate směřujících Internetu pro vyrovnávání zatížení v Resource Manager pomocí portálu Azure"
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: annahar
ms.openlocfilehash: 390ba8ec1474c54cf2c0022c4a3c219d21b1a659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-hello-azure-portal"></a>Vytváření k straně Internetu pro vyrovnávání zatížení pomocí hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [zjistěte, jak toocreate směřujících Internetu pro vyrovnávání zátěže pomocí klasického nasazení](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Toto se vztahuje hello pořadí jednotlivých úloh, které mají toocreate toobe provádí nástroj pro vyrovnávání zatížení a podrobně popisují, co probíhá tooaccomplish hello cíle.

## <a name="what-is-required-toocreate-an-internet-facing-load-balancer"></a>Co je požadovaná toocreate Vyrovnávání zatížení straně Internetu?

Je třeba toocreate a nakonfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení.

* Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.
* Fond adres back-end – obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.
* Pravidla pro vyrovnávání zatížení – obsahuje pravidla mapování veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello.
* Příchozí pravidla NAT – obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.
* Sondy – obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálních počítačů ve fondu adres back-end hello.

Další informace o komponentách nástroje pro vyrovnávání zatížení s Azure Resource Managerem najdete v tématu [Podpora nástroje Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-a-load-balancer-in-azure-portal"></a>Nastavení nástroje pro vyrovnávání zatížení na webu Azure Portal

> [!IMPORTANT]
> Tento příklad předpokládá, že máte virtuální síť s názvem **myVNet**. Odkazovat příliš[vytvořit virtuální síť](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) toodo to. Předpokládá také není podsítí v rámci **myVNet** názvem **LB podsíť být** a dva virtuální počítače, které volá **web1** a **web2** v uvedeném pořadí v Hello stejné skupině volané dostupnosti **myAvailSet** v **myVNet**. Odkazovat příliš[tento odkaz](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toocreate virtuálních počítačů.

1. V prohlížeči přejděte toohello portálu Azure: [http://portal.azure.com](http://portal.azure.com) a přihlaste se pomocí účtu Azure.
2. Na nejvyšší levé straně obrazovky hello hello vyberte **nový** > **sítě** > **nástroj pro vyrovnávání zatížení.**
3. V hello **vytvořit nástroj pro vyrovnávání zatížení** okno, zadejte název pro nástroj pro vyrovnávání zatížení. Tady jsme ho nazvali **myLoadBalancer**.
4. V části **Typ** vyberte **Veřejný**.
5. V části **Veřejná IP adresa** vytvořte novou veřejnou IP adresu s názvem **myPublicIP**.
6. V části Skupina prostředků vyberte **myRG**. Následně vyberte odpovídající **Umístění** a klikněte na **OK**. Nástroj pro vyrovnávání zatížení Hello se pak spustí toodeploy a bude trvat několik minut toosuccessfully dokončení nasazení.

    ![Aktualizace skupiny prostředků nástroje pro vyrovnávání zatížení](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)

## <a name="create-a-back-end-address-pool"></a>Vytvoření fondu back-endových adres

1. Jakmile je váš nástroj pro vyrovnávání zatížení úspěšně nasazen, vyberte jej z vašich prostředků. V nastavení vyberte Back-endové fondy. Zadejte název svého back-endového fondu. Potom klikněte na hello **přidat** tlačítko směrem k hello horní části okna hello, který se zobrazí.
2. Klikněte na **přidat virtuální počítač** v hello **přidejte fond back-end** okno.  V části **Skupina dostupnosti** vyberte možnost **Vybrat skupinu dostupnosti** a vyberte **myAvailSet**. Potom vyberte **zvolte hello virtuální počítače** v části hello část virtuálních počítačů v okně hello a klikněte na **web1** a **web2**, hello dva virtuální počítače vytvořené pro vyrovnávání zatížení. Zajistěte, aby obě modré značky zaškrtnutí toohello left, jak ukazuje následující obrázek hello. Potom klikněte na **vyberte** v tomto okně následuje OK v hello **vyberte virtuální počítače** okna a potom **OK** v hello **přidejte fond back-end** okno.

    ![Přidání fondu adres back-end toohello- ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Zkontrolujte, zda oznámení rozevíracím seznamu toomake má aktualizace ohledně ukládání fond back-end Vyrovnávání zatížení hello v přidání tooupdating hello síťové rozhraní pro obě hello virtuální počítače **web1** a **web2**.

## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Vytvoření testu, pravidla nástroje pro vyrovnávání zatížení a pravidel překladu adres (NAT)

1. Vytvořte test stavu.

    V části Nastavení vašeho nástroje pro vyrovnávání zatížení vyberte Testy. Pak klikněte na tlačítko **přidat** nachází v horní části hello hello okna.

    Existují dva způsoby tooconfigure sondu: HTTP nebo TCP. Tento příklad ukazuje HTTP, ale TCP lze nakonfigurovat podobným způsobem.
    Aktualizujte hello potřebné informace. Jak už bylo zmíněno, **myLoadBalancer** bude vyrovnávat zatížení provozu na portu 80. Vybraná cesta Hello je HealthProbe.aspx, Interval je 15 sekund a prahová hodnota špatného stavu je 2. Po dokončení klikněte na tlačítko **OK** toocreate hello testu.

    Podržte ukazatel myši nad toolearn ikonu hello 'i' více o těchto jednotlivých konfiguracích a jak může být změněné toocater tooyour požadavky.

    ![Přidání testu](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Vytvořte pravidlo nástroje pro vyrovnávání zatížení.

    Klikněte na pravidla v hello Vyrovnávání zatížení v oddílu nastavení nástroj pro vyrovnávání zatížení. V novém okně hello, klikněte na **přidat**. Zadejte název pravidla. Tady je to HTTP. Zvolte hello portů front-endu a back-end. Tady je pro oba vybrán port 80. Zvolte **LB back-end** jako back-endový fond a hello vytvořili **HealthProbe** jako hello testu. Další konfigurace se dá nastavit podle tooyour požadavky. Klikněte na tlačítko pravidlo Vyrovnávání zatížení OK toosave hello.

    ![Přidání pravidla vyrovnávání zatížení](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Vytvořte pravidla příchozího překladu adres (NAT).

    Klikněte na příchozí NAT pravidla uvedená v části Nastavení hello nástroj pro vyrovnávání zatížení. V okně nové hello, který, klikněte na tlačítko **přidat**. Potom zadejte název pravidla příchozího překladu adres (NAT). Tady jsme ho nazvali **inboundNATrule1**. Hello cíl musí být hello, které vytvořili veřejnou IP adresu. Vyberte vlastní pod službou a vyberte protokol hello chcete toouse. Tady je vybrán protokol TCP. Zadejte hello port 3441 a hello cílový port, v takovém případě 3389. Klikněte na tlačítko OK toosave toto pravidlo.

    Po vytvoření první pravidlo hello tento krok opakujte pro hello druhý příchozí pravidlo NAT volat inboundNATrule2 z portu 3442 tooTarget portu 3389.

    ![Přidání pravidla příchozího překladu adres (NAT)](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Odebrání nástroje pro vyrovnávání zatížení

toodelete nástroj pro vyrovnávání zatížení nástroje pro vyrovnávání zatížení vyberte hello chcete tooremove. V hello *nástroj pro vyrovnávání zatížení* okně klikněte na **odstranit** nachází v horní části hello hello okna. Po zobrazení výzvy pak vyberte **Ano**.

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
