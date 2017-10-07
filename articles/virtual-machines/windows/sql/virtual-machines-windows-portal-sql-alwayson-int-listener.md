---
title: "aaaCreate SQL Server naslouchací proces skupiny dostupnosti ve virtuálních počítačích Azure | Microsoft Docs"
description: "Podrobné pokyny pro vytvoření naslouchacího procesu pro skupiny dostupnosti Always On pro SQL Server na virtuálních počítačích Azure"
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/01/2017
ms.author: mikeray
ms.openlocfilehash: c6a44dc5c7c18b572c2bf5772b4651b7210aacbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>Konfigurace vyrovnávání zatížení pro skupiny dostupnosti Always On v Azure
Tento článek vysvětluje, jak toocreate Vyrovnávání zatížení pro skupinu dostupnosti SQL serveru Always On v Azure virtuální počítače, které běží s Azure Resource Manager. Skupiny dostupnosti vyžaduje nástroj pro vyrovnávání zatížení, pokud jsou hello instance systému SQL Server na virtuálních počítačích Azure. Nástroj pro vyrovnávání zatížení Hello ukládá hello IP adresu pro naslouchací proces skupiny dostupnosti hello. Pokud skupinu dostupnosti zahrnuje několik oblastí, musí každá oblast Vyrovnávání zatížení.

toocomplete tuto úlohu je nutné toohave skupinu dostupnosti systému SQL Server nasazený na Azure virtuální počítače, které běží s Resource Managerem. Virtuální počítače systému SQL Server musí patřit toohello stejné skupině dostupnosti. Můžete použít hello [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically vytvoření skupiny dostupnosti hello ve službě Správce prostředků. Tato šablona interní nástroj pro vás automaticky vytvoří. 

Pokud dáváte přednost, můžete [ručně nakonfigurovat skupinu dostupnosti](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Tento článek vyžaduje vaše skupiny dostupnosti jsou již nakonfigurována.  

Související témata:

* [Konfigurace skupin dostupnosti Always On virtuální počítač Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Nakonfigurujte připojení VNet-to-VNet s použitím Azure Resource Manageru a prostředí PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Podle s návodem v tomto článku, vytvořit a nakonfigurovat Vyrovnávání zatížení v hello portálu Azure. Po dokončení procesu hello nakonfigurujete hello clusteru toouse hello IP adresu z hello Vyrovnávání zatížení pro naslouchací proces skupiny dostupnosti hello.

## <a name="create-and-configure-hello-load-balancer-in-hello-azure-portal"></a>Vytvoření a konfigurace pro vyrovnávání zatížení hello v hello portálu Azure
V této části hello úkolu hello následující:

1. V hello portálu Azure vytvořit nástroj pro vyrovnávání zatížení hello a nakonfigurovat hello IP adresy.
2. Nakonfigurujte fond back-end hello.
3. Vytvořte test paměti hello. 
4. Nastavit pravidla Vyrovnávání zatížení hello.

> [!NOTE]
> Pokud instance systému SQL Server hello v více skupin prostředků a oblastí, provedení každého kroku dvakrát, jednou v každé skupině prostředků.
> 
> 

### <a name="step-1-create-hello-load-balancer-and-configure-hello-ip-address"></a>Krok 1: Vytvoření hello nástroj pro vyrovnávání zatížení a nakonfigurovat hello IP adresy
Nejprve vytvořte hello nástroj pro vyrovnávání zatížení. 

1. V hello portálu Azure otevřete hello skupinu prostředků, která obsahuje virtuální počítače, hello systému SQL Server. 

2. Ve skupině prostředků hello, klikněte na tlačítko **přidat**.

3. Vyhledejte **nástroj pro vyrovnávání zatížení** a potom ve výsledcích hledání hello, vyberte **Vyrovnávání zatížení**, která je publikována serverem **Microsoft**.

4. Na hello **nástroj pro vyrovnávání zatížení** okně klikněte na tlačítko **vytvořit**.

5. V hello **vytvořit nástroj pro vyrovnávání zatížení** dialogové okno pole, nástroj pro vyrovnávání zatížení hello nakonfigurovat následujícím způsobem:

   | Nastavení | Hodnota |
   | --- | --- |
   | **Název** |Název text představující hello nástroj pro vyrovnávání zatížení. Například **sqlLB**. |
   | **Typ** |**Interní**: většinu implementací použijte Vyrovnávání zatížení interní, která umožňuje aplikacím v rámci hello stejné skupiny dostupnosti toohello tooconnect virtuální sítě.  </br> **Externí**: umožňuje aplikacím tooconnect toohello skupiny dostupnosti prostřednictvím veřejného Internetu připojení. |
   | **Virtuální síť** |Vyberte hello virtuální síť, která jsou intances hello systému SQL Server v. |
   | **Podsíť** |Vyberte hello podsítě, která jsou instance systému SQL Server hello v. |
   | **Přiřazení IP adresy** |**Statické** |
   | **Privátní IP adresa** |Zadejte dostupnou IP adresu z podsítě hello. Při vytváření naslouchací proces na hello clusteru, použijte tuto adresu IP. Ve skriptu prostředí PowerShell, dále v tomto článku, použijte tuto adresu pro hello `$ILBIP` proměnné. |
   | **Předplatné** |Pokud máte více předplatných, může se objevit v tomto poli. Vyberte hello předplatné, které chcete tooassociate se tento prostředek. Ho je obvykle hello stejného předplatného jako všechny hello prostředky pro skupinu dostupnosti hello. |
   | **Skupina prostředků** |Vyberte skupinu prostředků hello, jestli jsou instance systému SQL Server hello v. |
   | **Umístění** |Vyberte umístění Azure, které jsou instance systému SQL Server hello v hello. |

6. Klikněte na možnost **Vytvořit**. 

Azure vytvoří nástroj pro vyrovnávání zatížení hello. Nástroj pro vyrovnávání zatížení Hello patří tooa konkrétní sítě, podsítě, skupinu prostředků a umístění. Jakmile Azure dokončí úlohu hello, ověřte nastavení nástroje pro vyrovnávání zatížení hello v Azure. 

### <a name="step-2-configure-hello-back-end-pool"></a>Krok 2: Konfigurace fondu back-end hello
Azure volání hello fond adres back-end *fond back-end*. V takovém případě fond back-end hello je hello adresy hello dvě instance systému SQL Server ve skupině dostupnosti. 

1. Ve skupině prostředků klikněte na tlačítko hello Vyrovnávání zatížení, který jste vytvořili. 

2. Na **nastavení**, klikněte na tlačítko **back-endové fondy**.

3. Na **back-endové fondy**, klikněte na tlačítko **přidat** toocreate fond back-end adresy. 

4. Na **přidejte fond back-end**v části **název**, zadejte název pro fond back-end hello.

5. V části **virtuální počítače**, klikněte na tlačítko **přidat virtuální počítač**. 

6. V části **vyberte virtuální počítače**, klikněte na tlačítko **zvolit skupinu dostupnosti**a pak zadejte hello dostupnosti nastavit, že virtuální počítače systému SQL Server hello patří do.

7. Po zvolení hello skupinu dostupnosti, klikněte na tlačítko **zvolte hello virtuální počítače**, vyberte hello dva virtuální počítače, které hostit hello instance systému SQL Server ve skupině dostupnosti hello a pak klikněte na tlačítko **vyberte**. 

8. Klikněte na tlačítko **OK** tooclose hello okna pro **vyberte virtuální počítače**, a **přidejte fond back-end**. 

Azure aktualizuje hello nastavení fondu back-end adres hello. Vaše skupina dostupnosti má nyní fond dvě instance systému SQL Server.

### <a name="step-3-create-a-probe"></a>Krok 3: Vytvoření sondu
Test Hello definuje, jak Azure ověřuje, která z instance systému SQL Server hello aktuálně vlastníkem hello naslouchací proces skupiny dostupnosti. Azure sondy hello služby na základě IP adresy hello portu, které definujete při vytváření hello testu.

1. Nástroj pro vyrovnávání zatížení na hello **nastavení** okně klikněte na tlačítko **testy stavu**. 

2. Na hello **testy stavu** okně klikněte na tlačítko **přidat**.

3. Konfigurace testu hello na hello **přidat test** okno. Hello použijte následující hodnoty tooconfigure hello testu:

   | Nastavení | Hodnota |
   | --- | --- |
   | **Název** |Název text představující hello testu. Například **SQLAlwaysOnEndPointProbe**. |
   | **Protokol** |**TCP** |
   | **Port** |Můžete použít jakýkoli dostupný port. Například *59999*. |
   | **Interval** |*5* |
   | **Prahová hodnota špatného stavu** |*2* |

4.  Klikněte na **OK**. 

> [!NOTE]
> Ujistěte se, že je otevřen v bráně firewall hello obou instancí systému SQL Server hello port, který zadáte. Obě instance vyžadují příchozí pravidlo hello port TCP, který používáte. Další informace najdete v tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure vytvoří hello kontrolu a používá je tootest, kterou instanci systému SQL Server má hello naslouchací proces skupiny dostupnosti hello.

### <a name="step-4-set-hello-load-balancing-rules"></a>Krok 4: Nastavení pravidla Vyrovnávání zatížení hello
pravidla Vyrovnávání zatížení Hello nakonfigurujte, jak nástroj pro vyrovnávání zatížení hello směruje provoz instance systému SQL Server toohello. Pro tuto službu Vyrovnávání zatížení můžete povolit přímá odpověď ze serveru, protože pouze jedna ze dvou instancí systému SQL Server hello vlastní prostředek naslouchací proces skupiny dostupnosti hello najednou.

1. Nástroj pro vyrovnávání zatížení na hello **nastavení** okně klikněte na tlačítko **pravidla Vyrovnávání zatížení**. 

2. Na hello **pravidla Vyrovnávání zatížení** okně klikněte na tlačítko **přidat**.

3. Na hello **pravidla Vyrovnávání zatížení přidat** okně nakonfigurovat pravidlo Vyrovnávání zatížení hello. Použijte hello následující nastavení: 

   | Nastavení | Hodnota |
   | --- | --- |
   | **Název** |Název text představující hello pravidla Vyrovnávání zatížení. Například **SQLAlwaysOnEndPointListener**. |
   | **Protokol** |**TCP** |
   | **Port** |*1433* |
   | **Back-endový Port** |*1433*. Tato hodnota se ignoruje, protože toto pravidlo používá **plovoucí IP adresa (přímá odpověď ze serveru)**. |
   | **Test** |Použijte název hello hello kontroly, kterou jste vytvořili pro tuto službu Vyrovnávání zatížení. |
   | **Trvalost relace** |**None** |
   | **Časový limit nečinnosti (minuty)** |*4* |
   | **Plovoucí IP adresa (přímá odpověď ze serveru)** |**Povoleno** |

   > [!NOTE]
   > Máte tooscroll dolů hello okno tooview všechna nastavení hello.
   > 

4. Klikněte na **OK**. 
5. Azure nakonfiguruje hello pravidlo Vyrovnávání zatížení. Nástroj pro vyrovnávání zatížení hello je teď nakonfigurované tooroute instance systému SQL Server toohello v provozu, který je hostitelem hello naslouchací proces skupiny dostupnosti hello. 

V tomto okamžiku má skupina prostředků hello Vyrovnávání zatížení, která se připojuje tooboth počítače systému SQL Server. Nástroj pro vyrovnávání zatížení Hello také obsahuje IP adresu pro hello SQL serveru Always On naslouchací proces skupiny dostupnosti, aby mohl počítač buď odpovídat toorequests pro skupiny dostupnosti hello.

> [!NOTE]
> Pokud vaše instance systému SQL Server ve dvou oblastech, opakujte kroky hello v hello jiné oblasti. Každou oblast, vyžaduje nástroj pro vyrovnávání zatížení. 
> 
> 

## <a name="configure-hello-cluster-toouse-hello-load-balancer-ip-address"></a>Konfigurace toouse clusteru hello, hello adresy IP nástroje pro vyrovnávání zatížení
dalším krokem Hello je naslouchací proces hello tooconfigure na hello clusteru a převeďte hello online naslouchací proces. Hello následující: 

1. Vytvořte naslouchací proces skupiny dostupnosti hello na hello převzetí služeb při selhání clusteru. 

2. Přepněte hello online naslouchací proces.

### <a name="step-5-create-hello-availability-group-listener-on-hello-failover-cluster"></a>Krok 5: Vytvoření hello naslouchací proces skupiny dostupnosti v clusteru převzetí služeb při selhání hello
V tomto kroku vytvoříte ručně hello naslouchací proces skupiny dostupnosti ve Správci clusteru převzetí služeb při selhání a SQL Server Management Studio.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-hello-configuration-of-hello-listener"></a>Ověření konfigurace hello hello naslouchacího procesu

Pokud prostředky clusteru hello a závislosti jsou správně nakonfigurovány, musí být schopný tooview hello naslouchání v aplikaci SQL Server Management Studio. tooset hello port naslouchacího procesu, hello následující:

1. Spusťte SQL Server Management Studio a připojte toohello primární repliky.

2. Přejděte příliš**vysoké dostupnosti AlwaysOn** > **skupiny dostupnosti** > **naslouchací procesy skupiny dostupnosti**.  
    Teď byste měli vidět název hello naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání. 

3. Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **vlastnosti**.

4. V hello **Port** zadejte číslo portu naslouchacího procesu skupiny dostupnosti hello hello pomocí hello $EndpointPort jste použili předtím (1433 byla výchozí hello) a potom klikněte na **OK**.

Nyní máte skupinu dostupnosti v Azure virtuální počítače běžící v režimu Resource Manager. 

## <a name="test-hello-connection-toohello-listener"></a>Naslouchací proces toohello testovací hello připojení
Testovací připojení hello díky hello následující:

1. Instance systému SQL Server tooa RDP, který je v hello stejné virtuální síti však není vlastní hello repliky. Tento server můžete hello jiné instance systému SQL Server v clusteru hello.

2. Použití **sqlcmd** nástroj tootest hello připojení. Například vytvoří hello následující skript **sqlcmd** připojení toohello primární repliky prostřednictvím hello naslouchací proces s ověřováním systému Windows:
   
        sqlcmd -S <listenerName> -E

Hello SQLCMD připojení automaticky připojí toohello instance systému SQL Server, který je hostitelem primární repliky hello. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Vytvoření IP adresu pro skupinu dostupnosti další

Každou skupinu dostupnosti používá samostatné naslouchací proces. Každý naslouchací proces má svou vlastní IP adresu. Použití hello stejná IP adresa hello toohold služby Vyrovnávání zatížení pro další moduly pro naslouchání. Po vytvoření skupiny dostupnosti, přidejte hello IP adresu toohello nástroj pro vyrovnávání zatížení a pak nakonfigurujte hello naslouchací proces.

tooadd k IP adresu tooa pro vyrovnávání zatížení s hello portál Azure, hello následující:

1. V hello portálu Azure otevřete hello skupinu prostředků, který obsahuje nástroj pro vyrovnávání zatížení hello a pak klikněte na nástroj pro vyrovnávání zatížení hello. 

2. V části **nastavení**, klikněte na tlačítko **fondu IP front-endu**a potom klikněte na **přidat**. 

3. V části **přidat IP adresu front-endu**, přiřadit název hello front-endu. 

4. Ověřte, že hello **virtuální síť** a hello **podsíť** jsou hello stejná hodnota jako hello instance systému SQL Server.

5. Nastavit hello IP adresu pro naslouchací proces hello. 
   
   >[!TIP]
   >Můžete nastavit hello IP adresu toostatic a zadejte adresu, která není právě používána v podsíti hello. Alternativně můžete nastavit hello IP adresu toodynamic a uložit hello nové front-endu fond IP adres. Pokud tak učiníte, hello portál Azure automaticky přiřadí k dispozici toohello fondu IP adres. Můžete znovu otevřít fondu hello front-end IP adres a změnit přiřazení toostatic hello. 

6. Uložte hello IP adresu pro naslouchací proces hello. 

7. Přidáte test stavu pomocí hello následující nastavení:

   |Nastavení |Hodnota
   |:-----|:----
   |**Název** |Název tooidentify hello testu.
   |**Protokol** |TCP
   |**Port** |Nepoužitého portu TCP, který musí být k dispozici na všech virtuálních počítačů. Nelze zadat pro žádným jiným účelem. Žádné dva naslouchací procesy můžete použít hello stejný port testu. 
   |**Interval** |Hello mezi testu pokusů. Použijte výchozí hello (5).
   |**Prahová hodnota špatného stavu** |Hello počet po sobě jdoucích prahové hodnoty, které musí selhat, aby virtuální počítač není v pořádku.

8. Klikněte na tlačítko **OK** toosave hello testu. 

9. Vytvořte pravidlo Vyrovnávání zatížení. Klikněte na tlačítko **pravidla Vyrovnávání zatížení**a potom klikněte na **přidat**.

10. Nakonfigurujte hello nové služby Vyrovnávání zatížení pravidla pomocí hello následující nastavení:

   |Nastavení |Hodnota
   |:-----|:----
   |**Název** |Název tooidentify hello načíst pravidlo vyrovnávání. 
   |**Adresa IP front-endu** |Vyberte hello IP adresu, kterou jste vytvořili. 
   |**Protokol** |TCP
   |**Port** |Použijte hello port, který používáte hello instance systému SQL Server. Výchozí instance používá port 1433, pokud jste změnili ho. 
   |**Back-endový port** |Hello použijte stejnou hodnotu jako **Port**.
   |**Fond back-end** |Hello fond, který obsahuje hello virtuálních počítačů s instancí systému SQL Server hello. 
   |**Test stavu** |Zvolte hello test, který jste vytvořili.
   |**Trvalost relace** |Žádný
   |**Časový limit nečinnosti (minuty)** |Výchozí (4)
   |**Plovoucí IP adresa (přímá odpověď ze serveru)** | Povoleno

### <a name="configure-hello-availability-group-toouse-hello-new-ip-address"></a>Konfigurace hello dostupnosti skupiny toouse hello nové IP adresy

toofinish konfigurace clusteru hello opakování hello kroky, které jste udělali, když jste provedli hello první skupiny dostupnosti. To znamená, nakonfigurujte hello [toouse hello novou IP adresu clusteru](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Po přidání IP adresu pro naslouchací proces hello, nakonfigurujte skupinu dostupnosti další hello pomocí hello následující: 

1. Ověřte, zda je na virtuální počítače systému SQL Server otevřen port hello testu pro hello novou IP adresu. 

2. [Ve Správci clusteru přidat hello klientský přístupový bod](#addcap).

3. [Konfigurace prostředků hello IP pro skupinu dostupnosti hello](#congroup).

   >[!IMPORTANT]
   >Když vytvoříte hello IP adresu, použijte hello IP adresu, zda jste přidali Vyrovnávání zatížení toohello.  

4. [Vytvořte prostředek skupiny dostupnosti systému SQL Server hello závislost na hello klientský přístupový bod](#dependencyGroup).

5. [Usnadnění přístupu klienta hello bodu prostředků, které jsou závislé na IP adrese hello](#listname).
 
6. [Nastavení parametrů clusteru hello v prostředí PowerShell](#setparam).

Po dokončení konfigurace hello dostupnosti skupiny toouse hello novou IP adresu, nakonfigurujte hello připojení toohello naslouchací proces. 

## <a name="next-steps"></a>Další kroky

- [Konfigurovat skupinu dostupnosti SQL serveru Always On na virtuálních počítačích, které jsou v různých oblastech Azure](virtual-machines-windows-portal-sql-availability-group-dr.md)
