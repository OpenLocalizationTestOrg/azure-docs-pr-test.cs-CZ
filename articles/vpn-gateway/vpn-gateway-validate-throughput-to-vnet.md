---
title: "Ověření sítě VPN propustnost tooa Microsoft Azure Virtual Network | Microsoft Docs"
description: "Hello účelem tohoto dokumentu je toohelp uživatele ověřit propustnost sítě hello ze své místní prostředky tooan virtuální počítač Azure."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 9cf0b67145645a3c2a9556b0cc910066cc62a9ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toovalidate-vpn-throughput-tooa-virtual-network"></a>Jak toovalidate VPN propustnost tooa virtuální sítě

Připojení k bráně VPN vám umožní zabezpečit připojení mezi různými místy tooestablish mezi vaší virtuální sítě v rámci Azure a místní infrastrukturu IT.

Tento článek ukazuje, jak toovalidate propustnost sítě z hello místní prostředky tooan virtuální počítač Azure. Obsahuje také pokyny k odstraňování problémů.

>[!NOTE]
>Tento článek je určený toohelp Diagnostika a řešení běžných problémů. Pokud jste nelze toosolve hello problém pomocí hello následující informace, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Přehled

Hello připojení brány sítě VPN zahrnuje hello následující součásti:

- Místní zařízení VPN (zobrazení seznamu [ověřit zařízení VPN)](vpn-gateway-about-vpn-devices.md#devicetable).
- Veřejný Internet
- Služba Azure VPN gateway
- Virtuální počítače Azure

Hello následující diagram znázorňuje hello logické připojení místní síti tooan virtuální síť Azure prostřednictvím sítě VPN.

![TooMSFT logické síti zákazníka připojení k síti pomocí sítě VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-hello-maximum-expected-ingressegress"></a>Vypočítat hello maximální očekávaný vstup/výstup

1.  Určete požadavky na propustnost směrného plánu vaší aplikace.
2.  Určení vaší omezení propustnosti brány Azure VPN. Nápovědu najdete v části hello "agregovaná propustnost podle typů SKU a síť VPN" [plánování a návrhu pro bránu VPN](vpn-gateway-plan-design.md).
3.  Určení hello [virtuálního počítače Azure propustnost pokyny](../virtual-machines/virtual-machines-windows-sizes.md) pro vaše velikost virtuálního počítače.
4.  Určuje šířku pásma vašeho poskytovatele služeb sítě Internet (ISP).
5.  Vypočítat očekávané propustnost - minimálně šířka pásma (virtuální počítač, brány, poskytovatele internetových služeb) * 0,8.

Pokud vaše počítané propustnost nesplňuje požadavky na propustnost směrného plánu vaší aplikace, musíte šířky pásma hello tooincrease hello prostředku, který jste určili jako hello potíže. tooresize služby Azure VPN Gateway najdete v části [změna skladová položka brány](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku). tooresize virtuálního počítače, najdete v části [změnit velikost virtuálního počítače](../virtual-machines/virtual-machines-windows-resize-vm.md). Pokud k těmto problémům očekávané šířky pásma Internetu, můžete také toocontact vašeho poskytovatele internetových služeb.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Ověření propustnost sítě pomocí nástroje pro sledování výkonu

Toto ověření je třeba provést v době mimo špičku jako sytost propustnost tunelové propojení VPN během testování není poskytnuta přesné výsledky.

Hello nástroj, který jsme použít pro tento test je iPerf, který má klient i server režimy a funguje v systému Windows a Linux. Je omezená too3 GB/s pro virtuální počítače Windows.

Tento nástroj neprovádí žádné operace toodisk pro čtení a zápis. Vytvoří výhradně generovaný sám sebou TCP provoz z jednoho koncového toohello jiné. Vygeneroval statistiky podle experimenty, které měří hello šířky pásma mezi klientem a serverem uzly k dispozici. Při testování mezi dvěma uzly, jeden funguje jako hello server a hello jiných jako klient. Po dokončení se tento test, doporučujeme zpětné hello role tootest obě nahrávání a stahování propustnost v obou uzlech.

### <a name="download-iperf"></a>Stažení nástroje iPerf
Stáhněte si [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Podrobnosti najdete v tématu [iPerf dokumentaci](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >Hello produkty třetích stran, které tento článek popisuje, pocházejí od společností, které jsou nezávislé na Microsoftu. Společnost Microsoft neposkytuje žádné záruky, předpokládanou ani jinou o hello výkonem a spolehlivostí těchto produktů.
 >
 >

### <a name="run-iperf-iperf3exe"></a>Spuštění nástroje iPerf (iperf3.exe)
1. Povolte pravidlo NSG nebo seznamu ACL umožňuje hello provoz (pro veřejné IP adresy testování na virtuálním počítači Azure).

2. V obou uzlech povolte výjimku brány firewall pro port 5001.

    **Windows:** následující hello spustit příkaz jako správce:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    pravidlo hello tooremove při testování je dokončena, spusťte tento příkaz:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    **Azure Linux:** obrázků Azure Linux je projektovou brány firewall. Pokud je aplikace, která naslouchá na portu, hello provoz je povolený prostřednictvím. Vlastní Image, které jsou zabezpečené může být nutné explicitně otevřené porty. Běžné brány firewall vrstvy operačního systému Linux zahrnují `iptables`, `ufw`, nebo `firewalld`.

3. Na uzlu serveru hello změňte adresář toohello, které je extrahován iperf3.exe. Potom spusťte iPerf v režimu serveru a nastavte ji toolisten na portu 5001 jako hello následující příkazy:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. V uzlu hello klienta změna toohello adresáře, kde je nástroj iperf extrahovat a pak spusťte následující příkaz hello:

    ```CMD
    iperf3.exe -c <IP of hello iperf Server> -t 30 -p 5001 -P 32
    ```

    Klient Hello způsobuje přenosy na portu 5001 toohello serveru po dobu 30 sekund. Hello příznak '-P "určující používáme 32 uzel serveru toohello souběžných připojení.

    Hello následující obrazovka ukazuje výstup hello z tohoto příkladu:

    ![Výstup](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. (Volitelné) toopreserve hello testování výsledků, spusťte tento příkaz:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. Po dokončení předchozích kroků hello spusťte hello stejný postup s rolemi hello vrátit zpět, tak, aby hello uzel serveru teď bude hello klienta a naopak.

## <a name="address-slow-file-copy-issues"></a>Řeší problémy kopie pomalé souboru
Může dojít k pomalé souboru kopírování při pomocí Průzkumníka Windows nebo přetahování a vkládání přes relaci protokolu RDP. Tento problém je obvykle kvůli tooone nebo obou hello následující faktory:

- Při kopírování souborů aplikace kopírování souboru, jako je například Průzkumník Windows a protokolu RDP, nepoužívejte více vláken. Pro lepší výkon použijte aplikaci kopie Vícevláknová souborového [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy soubory pomocí 16 nebo 32 vláken. Klikněte na tlačítko toochange hello vlákno číslo pro kopírování souborů v Richcopy, **akce** > **možnosti kopírování** > **kopírování souboru**.<br><br>
![Pomalé souboru kopie problémy](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Nedostatečná rychlost čtení/zápis disku virtuálního počítače. Další informace najdete v tématu [řešení potíží s Azure Storage](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>Místní zařízení externí přístupných rozhraní
Pokud hello místní zařízení VPN internetového IP adresa je součástí hello [místní sítě](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definice v Azure, se můžete setkat nemohou toobring hello VPN ojediněle odpojí nebo problémy s výkonem.

## <a name="checking-latency"></a>Kontrola latence
Tracert tootrace tooMicrosoft Azure hraniční zařízení toodetermine použijte, pokud nejsou žádné zpoždění vyšší než 100 ms mezi segmenty směrování.

Z hello do místní sítě, spusťte *tracert* toohello VIP hello brány Azure nebo virtuálních počítačů. Jakmile se zobrazí pouze * vrátí, víte, bylo dosaženo hello Azure okraj. Když se názvy DNS, které zahrnují "MSN" vrátil, víte, že jste dosáhli páteřní Microsoft hello.<br><br>
![Kontrola latence](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Další kroky
Další informace a nápovědu najdete na naší hello následující odkazy:

- [Optimalizovat propustnost sítě pro virtuální počítače Azure](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [Podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
