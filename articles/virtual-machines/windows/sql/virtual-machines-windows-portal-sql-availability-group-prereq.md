---
title: "skupiny aaaSQL dostupnost serveru – virtuální počítače Azure - Prereqs | Microsoft Docs"
description: "Tento kurz ukazuje, jak seskupit tooconfigure hello požadavky pro vytváření dostupnosti SQL serveru Always On na virtuálních počítačích Azure."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: eed0729ead25c7793bb17a04cd7fd996c7dc8c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="complete-hello-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Dokončení hello požadavky pro vytváření skupin dostupnosti Always On na virtuálních počítačích Azure

Tento kurz ukazuje, jak toocomplete hello požadavky pro vytváření [SQL serveru skupiny dostupnosti Always On na virtuálních počítačích Azure (VM)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). Pokud jste dokončili hello požadavky, máte řadiče domény, dva virtuální počítače serveru SQL a monitorovací server v jedna skupina prostředků.

**Odhad času**: může trvat několik hodin toocomplete hello požadavky. Velká část této doby je stráví vytváření virtuálních počítačů.

Hello následující diagram znázorňuje sestavení v kurzu hello.

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Přečtěte si dokumentaci k skupiny dostupnosti

Tento kurz předpokládá, že máte základní znalosti o SQL serveru Always On skupiny dostupnosti. Pokud si nejste obeznámeni s touto technologií, přečtěte si téma [přehled o skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Vytvoření účtu Azure
Potřebujete mít účet Azure. Můžete [otevřít bezplatný účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody předplatitele Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
1. Přihlaste se toohello [portál Azure](http://portal.azure.com).
2. Klikněte na tlačítko  **+**  toocreate nový objekt v portálu hello.

   ![Nový objekt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Typ **skupiny prostředků** v hello **Marketplace** okně vyhledávání.

   ![Skupina prostředků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Klikněte na tlačítko **skupiny prostředků**.
5. Klikněte na možnost **Vytvořit**.
6. Na hello **skupiny prostředků** okno, v části **název skupiny prostředků**, zadejte název pro skupinu prostředků hello. Můžete například zadat **sql-ha-rg**.
7. Pokud máte víc předplatných Azure, ověřte, zda text hello předplatné hello předplatného Azure, které chcete skupinu dostupnosti hello toocreate v.
8. Vyberte umístění. umístění Hello je hello oblast Azure, kam chcete skupinu dostupnosti toocreate hello. V tomto kurzu vytvoříme toobuild všechny prostředky v jednom místě, Azure.
9. Ověřte, že **Pin toodashboard** je zaškrtnuté. Toto volitelné nastavení umístí zástupce pro skupinu prostředků hello na hello řídicí panel portálu Azure.

   ![Skupina prostředků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Klikněte na tlačítko **vytvořit** skupiny prostředků toocreate hello.

Azure vytvoří skupinu prostředků hello a PIN kódů skupiny prostředků toohello zástupce hello portálu.

## <a name="create-hello-network-and-subnets"></a>Vytvoření hello sítě a podsítě
dalším krokem Hello je toocreate hello sítě a podsítě v hello skupina prostředků Azure.

řešení Hello používá jednu virtuální síť se dvěma podsítěmi. Hello [Přehled virtuálních sítí](../../../virtual-network/virtual-networks-overview.md) poskytuje další informace o sítích v Azure.

toocreate hello virtuální sítě:

1. V hello portál Azure, ve skupině prostředků, klikněte na **+ přidat**. Otevře se Azure hello **všechno, co** okno.

   ![Nová položka](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Vyhledejte **virtuální sítě**.

     ![Hledání virtuální sítě](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Klikněte na tlačítko **virtuální síť**.
4. Na hello **virtuální síť** okně klikněte na tlačítko hello **Resource Manager** modelu nasazení a pak klikněte na tlačítko **vytvořit**.

    Hello následující tabulka uvádí nastavení hello hello virtuální sítě:

   | **Pole** | Hodnota |
   | --- | --- |
   | **Název** |autoHAVNET |
   | **Adresní prostor** |10.33.0.0/24 |
   | **Název podsítě** |Správce |
   | **Rozsah adres podsítě** |10.33.0.0/29 |
   | **Předplatné** |Zadejte hello předplatné, že máte v úmyslu toouse. **Předplatné** je prázdná, pokud máte pouze jedno předplatné. |
   | **Skupina prostředků** |Zvolte **použít existující** a vyberte hello název skupiny prostředků hello. |
   | **Umístění** |Zadejte hello umístění Azure. |

   Vaše adresa prostoru a podsítě rozsah adres může být odlišný od hello tabulky. V závislosti na vaše předplatné navrhuje hello portálu k dispozici adresní prostor a odpovídající rozsah adres podsítě. Pokud je k dispozici žádné dostatečný Adresní prostor, použijte jiný odběr.

   Příklad Hello používá hello název podsítě **správce**. Tato podsíť je pro řadiče domény hello.

5. Klikněte na možnost **Vytvořit**.

   ![Konfigurace virtuální sítě hello](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure vám vrátí toohello řídicí panel portálu a vás upozorní, jakmile se vytvoří nová síť hello.

### <a name="create-a-second-subnet"></a>Vytvořte druhou podsíť
Hello novou virtuální síť obsahuje jednu podsíť, s názvem **správce**. použít tuto podsíť, hello řadiče domény. Hello virtuálním počítačům systému SQL Server používat druhou podsíť s názvem **SQL**. tooconfigure této podsítě:

1. Na řídicím panelu, klikněte na tlačítko hello skupinu prostředků, kterou jste vytvořili, **SQL-HA-RG**. Vyhledejte hello sítě ve skupině prostředků hello pod **prostředky**.

    Pokud **SQL-HA-RG** není viditelná, klepněte na tlačítko **skupiny prostředků** a filtrování podle názvu skupiny prostředků hello.
2. Klikněte na tlačítko **autoHAVNET** na hello seznamu prostředků. Azure otevře se okno Konfigurace sítě hello.
3. Na hello **autoHAVNET** okna virtuální sítě v části **nastavení** , klikněte na tlačítko **podsítě**.

    Všimněte si hello podsíť, kterou jste už vytvořili.

   ![Konfigurace virtuální sítě hello](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. Vytvořte druhou podsíť. Klikněte na tlačítko **+ podsítě**.
6. Na hello **přidat podsíť** okně nakonfigurovat podsítě hello zadáním **sqlsubnet** pod **název**. Azure automaticky určuje platná **rozsahu adres**. Ověřte, zda tento rozsah adres má minimálně 10 adresy v ní. V produkčním prostředí může vyžadovat další adresy.
7. Klikněte na **OK**.

    ![Konfigurace virtuální sítě hello](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

Hello následující tabulka shrnuje nastavení konfigurace sítě hello:

| **Pole** | Hodnota |
| --- | --- |
| **Název** |**autoHAVNET** |
| **Adresní prostor** |Tato hodnota závisí na hello k dispozici adresní prostory v rámci vašeho předplatného. Typické hodnota je 10.0.0.0/16. |
| **Název podsítě** |**Správce** |
| **Rozsah adres podsítě** |Tato hodnota závisí na hello dostupnou adresu rozsahů v rámci vašeho předplatného. Typické hodnota je 10.0.0.0/24. |
| **Název podsítě** |**sqlsubnet** |
| **Rozsah adres podsítě** |Tato hodnota závisí na hello dostupnou adresu rozsahů v rámci vašeho předplatného. Typické hodnota je 10.0.1.0/24. |
| **Předplatné** |Zadejte hello předplatné, že máte v úmyslu toouse. |
| **Skupina prostředků** |**SQL-HA-RG** |
| **Umístění** |Zadejte stejné umístění, které jste zvolili pro skupinu prostředků hello hello. |

## <a name="create-availability-sets"></a>Vytvoření skupin dostupnosti

Než vytvoříte virtuální počítače, je třeba toocreate skupiny dostupnosti. Skupiny dostupnosti zkrátit dobu prostojů hello události plánované i neplánované údržby. Nastavení Azure dostupnosti je logické skupiny prostředků, které Azure umístí na fyzických domén selhání a aktualizace domény. Domény selhání zajistí, že členové hello sady dostupnosti hello samostatné napájení a síťové prostředky. Doméně aktualizace zajistí, že členové sady dostupnosti hello nejsou snížila pro údržbu na hello stejný čas. Další informace najdete v tématu [spravovat hello dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Je třeba dvě skupiny dostupnosti. Jeden je pro řadiče domény hello. Hello druhou je pro virtuální počítače hello SQL serveru.

toocreate dostupnosti nastavit, přejděte toohello skupinu prostředků a klikněte na tlačítko **přidat**. Filtrování výsledků hello zadáním **skupinu dostupnosti**. Klikněte na tlačítko **sadu dostupnosti** v hello výsledky a potom klikněte na **vytvořit**.

Konfigurace dvou sad dostupnosti podle toohello parametry v následující tabulce hello:

| **Pole** | Skupina dostupnosti řadiče domény | Skupinu dostupnosti systému SQL Server |
| --- | --- | --- |
| **Název** |adavailabilityset |sqlavailabilityset |
| **Skupina prostředků** |SQL-HA-RG |SQL-HA-RG |
| **Domén selhání** |3 |3 |
| **Aktualizace domén** |5 |3 |

Po vytvoření skupiny dostupnosti hello vrátí toohello skupiny prostředků v hello portálu Azure.

## <a name="create-domain-controllers"></a>Vytvoření řadiče domény
Po vytvoření hello sítě, podsítě, skupiny dostupnosti a k straně Internetu pro vyrovnávání zatížení, jste připravené toocreate hello virtuálních počítačů pro řadiče domény hello.

### <a name="create-virtual-machines-for-hello-domain-controllers"></a>Vytváření virtuálních počítačů pro hello řadiče domény
toocreate a nakonfigurovat hello řadiče domény, vrátí toohello **SQL-HA-RG** skupinu prostředků.

1. Klikněte na tlačítko **Přidat**. Hello **všechno, co** otevře se okno.
2. Typ **systému Windows Server 2016 Datacenter**.
3. Klikněte na tlačítko **systému Windows Server 2016 Datacenter**. V hello **Windows Server 2016 Datacenter** okno, ověřte, že model nasazení hello **Resource Manager**a potom klikněte na **vytvořit**. Otevře se Azure hello **vytvořit virtuální počítač** okno.

Opakováním předchozích kroků toocreate dva virtuální počítače hello. Název hello dva virtuální počítače:

* řadič domény AD primární
* řadič domény AD sekundární

  > [!NOTE]
  > Hello **řadiče domény ad sekundární** virtuální počítač je volitelné, tooprovide vysoká dostupnost pro Active Directory Domain Services.
  >
  >

Hello následující tabulka uvádí hello nastavení pro tyto dva počítače:

| **Pole** | Hodnota |
| --- | --- |
| **Název** |První řadič domény: *řadiče domény ad primární*.</br>Druhý řadič domény *řadiče domény ad sekundární*. |
| **Typ disku virtuálního počítače** |SSD |
| **Uživatelské jméno** |DomainAdmin |
| **Heslo** |Contoso! 0000 |
| **Předplatné** |*Vaše předplatné* |
| **Skupina prostředků** |SQL-HA-RG |
| **Umístění** |*Vaše umístění* |
| **Velikost** |DS1_V2 |
| **Úložiště** | **Používat spravované disky** - **Ano** |
| **Virtuální síť** |autoHAVNET |
| **Podsíť** |Správce |
| **Veřejná IP adresa** |*Stejný název jako hello virtuálních počítačů* |
| **Skupina zabezpečení sítě** |*Stejný název jako hello virtuálních počítačů* |
| **Sady dostupnosti.** |adavailabilityset </br>**Poruch domén**: 2</br>**Aktualizovat domén**: 2|
| **Diagnostika** |Povoleno |
| **Účet úložiště diagnostiky** |*Automaticky vytvoří* |

   >[!IMPORTANT]
   >Virtuální počítač můžete umístit pouze ve skupině dostupnosti nastavena při jeho vytvoření. Nelze změnit hello dostupnosti nastavit po vytvoření virtuálního počítače. V tématu [spravovat hello dostupnosti virtuálních počítačů](../manage-availability.md).

Azure vytvoří hello virtuální počítače.

Po vytvoření hello virtuálních počítačů, nakonfigurujte hello řadič domény.

### <a name="configure-hello-domain-controller"></a>Konfigurace řadiče domény hello
V hello následující kroky konfigurace hello **řadiče domény ad primární** počítače jako řadič domény pro spolecnost.contoso.com.

1. Hello portálu, otevřete hello **SQL-HA-RG** skupinu prostředků a vyberte hello **řadiče domény ad primární** počítače. Na hello **řadiče domény ad primární** okně klikněte na tlačítko **Connect** souboru tooopen protokolu RDP pro přístup ke vzdálené ploše.

    ![Připojit tooa virtuálního počítače](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Přihlaste se pomocí účtu správce nakonfigurované (**\DomainAdmin**) a heslo (**Contoso! 0000**).
3. Ve výchozím nastavení, hello **správce serveru** měla by se zobrazit řídicí panel.
4. Klikněte na tlačítko hello **přidat role a funkce** odkaz na řídicí panel hello.

    ![Správce serveru – přidání rolí](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Vyberte **Další** dokud nezískáte toohello **role serveru** části.
6. Vyberte hello **Active Directory Domain Services** a **DNS Server** role. Když se zobrazí výzva, přidejte všechny další funkce, které jsou vyžadované tyto role.

   > [!NOTE]
   > Windows vás varuje, že neexistuje žádná statická IP adresa. Pokud testujete hello konfigurace, klikněte na tlačítko **pokračovat**. U produkčních scénářích nastavit hello IP adresu toostatic v hello portál Azure, nebo [použijte PowerShell tooset hello statickou IP adresu počítače řadiče domény hello](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Role dialogové okno Přidání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Klikněte na tlačítko **Další** dokud se nedostanete hello **potvrzení** části. Vyberte hello **restartování hello cílový server automaticky podle potřeby** zaškrtávací políčko.
8. Klikněte na **Nainstalovat**.
9. Po hello funkce dokončit instalaci, vrátí toohello **správce serveru** řídicího panelu.
10. Vyberte hello nové **služby AD DS** možnost v levém podokně hello.
11. Klikněte na tlačítko hello **Další** odkaz na panelu hello žlutý upozornění.

    ![Dialogové okno AD DS na hello virtuální počítač Server DNS](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. V hello **akce** sloupec hello **všechny podrobnosti úlohy serveru** dialogové okno, klikněte na tlačítko **zvýšit úroveň tohoto řadiče domény tooa server**.
13. V hello **Průvodce konfigurací služby domény služby Active Directory**, použijte hello následující hodnoty:

    | **Stránka** | Nastavení |
    | --- | --- |
    | **Konfigurace nasazení** |**Přidat novou doménovou strukturu**<br/> **Název kořenové domény** = corp.contoso.com |
    | **Možnosti řadiče domény** |**Heslo režimu obnovení adresářových služeb** = Contoso! 0000<br/>**Potvrzení hesla** = Contoso! 0000 |
14. Klikněte na tlačítko **Další** toogo prostřednictvím hello dalších stránek v Průvodci hello. Na hello **Kontrola předpokladů** ověřte, najdete v části hello následující zprávou: **všech požadovaných součástí byly úspěšně zkontrolovány**. Můžete zkontrolovat všechny příslušné zprávy upozornění, ale je možné toocontinue s instalací hello.
15. Klikněte na **Nainstalovat**. Hello **řadiče domény ad primární** virtuální počítač automaticky restartuje.

### <a name="note-hello-ip-address-of-hello-primary-domain-controller"></a>Poznámka: adresa IP hello hello primárního řadiče domény

Použití hello primární řadič domény pro službu DNS. Poznamenejte si adresu IP řadiče domény hello.

Jedním ze způsobů tooget hello primární doménu řadiče IP adresa je prostřednictvím hello portálu Azure.

1. Na portálu Azure hello otevřete skupinu prostředků hello.

2. Klikněte na tlačítko hello primární řadič domény.

3. V okně řadiče hello primární doménu, klikněte na tlačítko **síťových rozhraní**.

![Síťová rozhraní](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Všimněte si hello privátní IP adresu pro tento server.

### <a name="configure-hello-virtual-network-dns"></a>Konfigurace virtuální sítě hello DNS
Po vytvoření hello prvního řadiče domény a povolte DNS na prvním serveru hello, nakonfigurujte virtuální síť toouse hello tento server pro službu DNS.

1. V hello portálu Azure klikněte na virtuální síť hello.

2. V části **nastavení**, klikněte na tlačítko **DNS Server**.

3. Klikněte na tlačítko **vlastní**a zadejte hello privátní IP adresu hello primární řadič domény.

4. Klikněte na **Uložit**.

### <a name="configure-hello-second-domain-controller"></a>Konfigurace hello druhého řadiče domény
Po restartování počítače hello primární řadič domény, můžete nakonfigurovat hello druhého řadiče domény. Tento volitelný krok je pro vysokou dostupnost. Postupujte podle těchto kroků tooconfigure hello druhého řadiče domény:

1. Hello portálu, otevřete hello **SQL-HA-RG** skupinu prostředků a vyberte hello **řadiče domény ad sekundární** počítače. Na hello **řadiče domény ad sekundární** okně klikněte na tlačítko **Connect** souboru tooopen protokolu RDP pro přístup ke vzdálené ploše.
2. Přihlaste se pomocí účtu správce nakonfigurované toohello virtuálních počítačů (**BUILTIN\DomainAdmin**) a heslo (**Contoso! 0000**).
3. Změna hello Upřednostňovaná adresa toohello adresu serveru DNS řadiče domény hello.
4. V **Centrum sítí a sdílení**, klikněte na tlačítko hello síťové rozhraní.
   ![Síťové rozhraní](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. Klikněte na **Vlastnosti**.
6. Vyberte **Internet Protocol verze 4 (TCP/IPv4)** a klikněte na tlačítko **vlastnosti**.
7. Vyberte **hello použijte následující adresy serverů DNS** a zadejte adresu hello hello primárního řadiče domény v **upřednostňovaný server DNS**.
8. Klikněte na tlačítko **OK**a potom **Zavřít** toocommit hello změny. Nyní jste možnost toojoin hello virtuálního počítače příliš**corp.contoso.com**.

   >[!IMPORTANT]
   >Pokud po změně nastavení DNS hello ztratíte hello připojení tooyour vzdálené plochy, přejděte toohello portál a restartujte hello virtuální počítač Azure.

9. Hello vzdálené plochy toohello sekundární řadič domény, otevřete **řídicí panel Správce serveru**.
10. Klikněte na tlačítko hello **přidat role a funkce** odkaz na řídicí panel hello.

    ![Správce serveru – přidání rolí](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Vyberte **Další** dokud nezískáte toohello **role serveru** části.
12. Vyberte hello **Active Directory Domain Services** a **DNS Server** role. Když se zobrazí výzva, přidejte všechny další funkce, které jsou vyžadované tyto role.
13. Po hello funkce dokončit instalaci, vrátí toohello **správce serveru** řídicího panelu.
14. Vyberte hello nové **služby AD DS** možnost v levém podokně hello.
15. Klikněte na tlačítko hello **Další** odkaz na panelu hello žlutý upozornění.
16. V hello **akce** sloupec hello **všechny podrobnosti úlohy serveru** dialogové okno, klikněte na tlačítko **zvýšit úroveň tohoto řadiče domény tooa server**.
17. V části **konfigurace nasazení**, vyberte **přidat existující doménu tooan řadič domény**.
   ![Konfigurace nasazení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. Klikněte na **Vybrat**.
19. Připojit pomocí účtu správce hello (**CORP. CONTOSO.COM\domainadmin**) a heslo (**Contoso! 0000**).
20. V **zvolte doménu z doménové struktury hello**, klikněte na doménu a pak klikněte na tlačítko **OK**.
21. V **možnosti řadiče domény**, použijte hello výchozí hodnoty a nastavit heslo režimu obnovení adresářových služeb.

   >[!NOTE]
   >Hello **možnosti služby DNS** stránky může varovat, že nelze vytvořit delegování pro tento server DNS. Toto upozornění v testovacím prostředí můžete ignorovat.
22. Klikněte na tlačítko **Další** dokud nedosáhne hello dialogu hello **požadavky** zkontrolujte. Pak klikněte na **Nainstalovat**.

Po dokončení změny konfigurace hello hello serveru restartujte hello server.

### <a name="add-hello-private-ip-address-toohello-second-domain-controller-toohello-vpn-dns-server"></a>Přidat hello privátní IP adresa toohello druhé doméně řadič toohello DNS serveru VPN

V hello portál Azure, v rámci virtuální sítě změňte hello DNS Server tooinclude hello IP adresu hello sekundární řadič domény. To umožňuje redundance služby DNS hello.

### <a name=DomainAccounts></a>Konfigurace účtů domény hello

V dalších krocích hello můžete nakonfigurovat hello účtů služby Active Directory. Hello následující tabulka uvádí hello účty:

| |Účet instalace<br/> |SQL Server-0 <br/>Účet systému SQL Server a služba SQL Agent |SQL Server-1<br/>Účet systému SQL Server a služba SQL Agent
| --- | --- | --- | ---
|**Křestní jméno** |Instalace |SQLSvc1 | SQLSvc2
|**SamAccountName uživatele** |Instalace |SQLSvc1 | SQLSvc2

Pomocí následujících kroků toocreate hello každý účet.

1. Přihlaste se toohello **řadiče domény ad primární** počítače.
2. V **správce serveru**, vyberte **nástroje**a potom klikněte na **Centrum správy služby Active Directory**.   
3. Vyberte **corp (místní)** v levém podokně hello.
4. Na pravém hello **úlohy** podokně, vyberte **nový**a potom klikněte na **uživatele**.
   ![Centrum správy služby Active Directory](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Nastavte pro každý účet složité heslo.<br/> Pro nevýrobní prostředí sadu hello uživatele účtu toonever vyprší.

5. Klikněte na tlačítko **OK** toocreate hello uživatele.
6. Opakováním předchozích kroků pro jednotlivé účty tři hello hello.

### <a name="grant-hello-required-permissions-toohello-installation-account"></a>Udělení oprávnění hello vyžaduje účet instalace toohello
1. V hello **Centrum správy služby Active Directory**, vyberte **corp (místní)** v levém podokně hello. Poté v pravém hello **úlohy** podokně klikněte na tlačítko **vlastnosti**.

    ![Vlastnosti uživatele CORP](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Vyberte **rozšíření**a potom klikněte na hello **Upřesnit** na hello tlačítko **zabezpečení** kartě.
3. V hello **Upřesnit nastavení zabezpečení pro corp** dialogové okno, klikněte na tlačítko **přidat**.
4. Klikněte na tlačítko **vybrat objekt zabezpečení**, vyhledejte **CORP\Install**a potom klikněte na **OK**.
5. Vyberte hello **číst vlastnosti všech** zaškrtávací políčko.

6. Vyberte hello **vytvářet objekty počítačů** zaškrtávací políčko.

     ![Oprávnění uživatele Corp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Klikněte na tlačítko **OK**a potom klikněte na **OK** znovu. Zavřít hello **corp** vlastnosti – okno.

Teď, když jste dokončili konfigurace služby Active Directory a hello uživatelské objekty, vytvořte dva virtuální počítače SQL serveru a serveru s kopií clusteru virtuálních počítačů. Potom připojí všechny tři toohello domény.

## <a name="create-sql-server-vms"></a>Vytvoření virtuálních počítačů serveru SQL

Vytvořte tři další virtuální počítače. Hello řešení vyžaduje dva virtuální počítače s instancí systému SQL Server. Třetí virtuální počítač bude fungovat jako určující. Můžete použít Windows Server 2016 [cloudu určující](http://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), ale tento dokument konzistenci předchozích operačních systémech používá virtuální počítač pro určující.  

Než budete pokračovat zvažte následující rozhodnutí deisign hello.

* **Úložiště – spravované Azure disky**

   Pro úložiště virtuálního počítače hello použijte Azure spravované disky. Společnost Microsoft doporučuje spravované disky pro virtuální počítače systému SQL Server. Spravované obslužné rutiny úložiště disky pozadí hello. Kromě toho, když virtuální počítače s spravované disky jsou ve hello stejné skupině dostupnosti, Azure distribuuje hello úložiště prostředky tooprovide odpovídající redundance. Další informace najdete v tématu [Přehled služby Azure Managed Disks](../managed-disks-overview.md). Podrobnosti o spravovaných disků v nastavení dostupnosti, najdete v části [použijte spravované disky pro virtuální počítače v nastavení dostupnosti](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Síť – privátní IP adresy v produkčním prostředí**

   Pro virtuální počítače hello tento kurz používá veřejné IP adresy. To umožňuje připojení ke vzdálené přímo hello toohello virtuálního počítače přes internet – umožňuje jednodušší kroky konfigurace. V produkčním prostředí společnost Microsoft doporučuje pouze privátní IP adresy v pořadí tooreduce hello ohrožení zabezpečení nároků instance systému SQL Server hello prostředků virtuálního počítače.

### <a name="create-and-configure-hello-sql-server-vms"></a>Vytvoření a konfigurace virtuálních počítačů hello SQL serveru
Dále vytvořte tři virtuální počítače – dva virtuální počítače serveru SQL a virtuálního počítače pro do dodatečného uzlu clusteru. toocreate hello virtuálních počítačů, přejděte zpět toohello **SQL-HA-RG** skupinu prostředků, klikněte na tlačítko **přidat**, vyhledejte položku odpovídající Galerie hello, klikněte na **virtuálního počítače**a potom Klikněte na tlačítko **z Galerie**. Použijte hello informace v následující tabulce toohelp vytvoříte virtuální počítače hello hello:


| Stránka | VM1 | VIRTUÁLNÍHO POČÍTAČE 2 | VM3 |
| --- | --- | --- | --- |
| Vyberte položku odpovídající Galerie hello |**Windows Server 2016 Datacenter** |**SQL Server 2016 SP1 Enterprise na systém Windows Server 2016** |**SQL Server 2016 SP1 Enterprise na systém Windows Server 2016** |
| Konfigurace virtuálního počítače **základy** |**Název** = fsw clusteru<br/>**Uživatelské jméno** = DomainAdmin<br/>**Heslo** = Contoso! 0000<br/>**Předplatné** = vaše předplatné<br/>**Skupina prostředků** = SQL-HA-RG<br/>**Umístění** = vaše umístění azure |**Název** sqlserver-0 =<br/>**Uživatelské jméno** = DomainAdmin<br/>**Heslo** = Contoso! 0000<br/>**Předplatné** = vaše předplatné<br/>**Skupina prostředků** = SQL-HA-RG<br/>**Umístění** = vaše umístění azure |**Název** = sqlserver-1<br/>**Uživatelské jméno** = DomainAdmin<br/>**Heslo** = Contoso! 0000<br/>**Předplatné** = vaše předplatné<br/>**Skupina prostředků** = SQL-HA-RG<br/>**Umístění** = vaše umístění azure |
| Konfigurace virtuálního počítače **velikost** |**VELIKOST** = DS1\_V2 (1 jádro, 3.5 GB) |**VELIKOST** = DS2\_V2 (2 jádra, 7 GB)</br>velikost Hello musí podporovat úložiště SSD (podpora disku Premium. )) |**VELIKOST** = DS2\_V2 (2 jádra, 7 GB) |
| Konfigurace virtuálního počítače **nastavení** |**Úložiště**: použijte spravované disky.<br/>**Virtuální síť** = autoHAVNET<br/>**Podsíť** = sqlsubnet(10.1.1.0/24)<br/>**Veřejná IP adresa** automaticky generovaný.<br/>**Skupina zabezpečení sítě** = None<br/>**Monitorování diagnostiky** = povoleno<br/>**Účet úložiště diagnostiky** = použít účet úložiště automaticky generované<br/>**Skupina dostupnosti** = sqlAvailabilitySet<br/> |**Úložiště**: použijte spravované disky.<br/>**Virtuální síť** = autoHAVNET<br/>**Podsíť** = sqlsubnet(10.1.1.0/24)<br/>**Veřejná IP adresa** automaticky generovaný.<br/>**Skupina zabezpečení sítě** = None<br/>**Monitorování diagnostiky** = povoleno<br/>**Účet úložiště diagnostiky** = použít účet úložiště automaticky generované<br/>**Skupina dostupnosti** = sqlAvailabilitySet<br/> |**Úložiště**: použijte spravované disky.<br/>**Virtuální síť** = autoHAVNET<br/>**Podsíť** = sqlsubnet(10.1.1.0/24)<br/>**Veřejná IP adresa** automaticky generovaný.<br/>**Skupina zabezpečení sítě** = None<br/>**Monitorování diagnostiky** = povoleno<br/>**Účet úložiště diagnostiky** = použít účet úložiště automaticky generované<br/>**Skupina dostupnosti** = sqlAvailabilitySet<br/> |
| Konfigurace virtuálního počítače **nastavení systému SQL Server** |Neuvedeno |**Připojení k SQL** = privátní (uvnitř virtuální sítě)<br/>**Port** = 1433<br/>**Ověřování SQL** = zakázat<br/>**Konfigurace úložiště** = obecné<br/>**Automatizované opravy** = neděle na 2:00<br/>**Automatizované zálohování** = zakázáno</br>**Integrace se službou Azure Key Vault** = zakázáno |**Připojení k SQL** = privátní (uvnitř virtuální sítě)<br/>**Port** = 1433<br/>**Ověřování SQL** = zakázat<br/>**Konfigurace úložiště** = obecné<br/>**Automatizované opravy** = neděle na 2:00<br/>**Automatizované zálohování** = zakázáno</br>**Integrace se službou Azure Key Vault** = zakázáno |

<br/>

> [!NOTE]
> velikosti počítačů Hello navrhované tady jsou určené pro testování skupin dostupnosti ve virtuálních počítačích Azure. Hello nejlepší výkon v produkčním prostředí, najdete v části hello doporučení pro velikosti počítačů systému SQL Server a konfigurace v [nejlepší postupy z hlediska výkonu pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Po hello tři virtuální počítače jsou plně zřízený, je nutné toojoin je toohello **corp.contoso.com** domény a přidělit počítače toohello CORP\Install práva správce.

### <a name="joinDomain"></a>Připojení k doméně toohello servery hello

Jste nyní možné toojoin hello virtuální počítače příliš**corp.contoso.com**. Pro virtuální počítače hello SQL serveru a souboru hello následující hello sdílí monitorovací server:

1. Vzdálené připojení toohello virtuální počítač s **BUILTIN\DomainAdmin**.
2. V **správce serveru**, klikněte na tlačítko **místní Server**.
3. Klikněte na tlačítko hello **pracovní skupiny** odkaz.
4. V hello **název počítače** klikněte na tlačítko **změnu**.
5. Vyberte hello **domény** zaškrtávací políčko a zadejte **corp.contoso.com** hello textového pole. Klikněte na **OK**.
6. V hello **zabezpečení systému Windows** místní dialogové okno, zadejte hello pověření pro účet správce domény výchozí hello (**CORP\DomainAdmin**) a heslo hello (**Contoso! 0000**).
7. Když se zobrazí zpráva "Vítá toohello doméně corp.contoso.com" hello, klikněte na tlačítko **OK**.
8. Klikněte na tlačítko **Zavřít**a potom klikněte na **restartovat nyní** v automaticky otevřeném okně. dialog hello.

### <a name="add-hello-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Přidat hello Corp\Install uživatele s oprávněními správce na každý cluster virtuálních počítačů

Po restartování každý virtuální počítač jako člena domény hello přidat **CORP\Install** jako člen skupiny local administrators hello.

1. Počkejte, dokud hello virtuální počítač se restartuje a potom spusťte soubor RDP hello znovu z toosign řadič domény hello v příliš**sqlserver-0** pomocí hello **CORP\DomainAdmin** účtu.
   >[!TIP]
   >Ujistěte se, že přihlásíte pomocí účtu správce domény hello. V předchozích krocích hello jste používali účet správce v PŘEDDEFINOVANÉ hello. Teď, když hello je server v doméně hello, použijte účet domény hello. V relaci RDP, zadejte *domény*\\*uživatelské jméno*.

2. V **správce serveru**, vyberte **nástroje**a potom klikněte na **Správa počítače**.
3. V hello **Správa počítače** okně rozbalte **místní uživatelé a skupiny**a potom vyberte **skupiny**.
4. Klikněte dvakrát na hello **správci** skupiny.
5. V hello **správci vlastnosti** dialogové okno, klikněte na tlačítko hello **přidat** tlačítko.
6. Zadejte uživatele hello **CORP\Install**a potom klikněte na **OK**.
7. Klikněte na tlačítko **OK** tooclose hello **vlastností správce** dialogové okno.
8. Opakujte předchozí kroky hello na **sqlserver-1** a **clusteru fsw**.

### <a name="setServiceAccount"></a>Nastavit účty služby SQL Server hello

Na každý virtuální počítač SQL Server nastavte účet služby SQL Server hello. Použít hello účty, které jste vytvořili při jste [nakonfigurované účty domény hello](#DomainAccounts).

1. Otevřete **Správce konfigurace systému SQL Server**.
2. Klikněte pravým tlačítkem na hello služby SQL Server a pak klikněte na **vlastnosti**.
3. Nastavte hello účet a heslo.
4. Opakujte tyto kroky na hello další virtuální počítač SQL Server.  

Pro skupiny dostupnosti systému SQL Server musí každý virtuální počítač s SQL serverem toorun jako účet domény.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-hello-installation-account"></a>Vytvoření přihlášení na každý virtuální počítač SQL Server k účtu instalace hello

Použití hello instalace účtu (CORP\install) tooconfigure hello skupiny dostupnosti. Tento účet musí toobe členem hello **sysadmin** pevné role serveru na každý virtuální počítač SQL Server. Hello následujících kroků vytvořte přihlášení k účtu instalace hello:

1. Připojení serveru toohello prostřednictvím hello protokolu RDP (Remote Desktop) pomocí hello  *\<MachineName\>\DomainAdmin* účtu.

1. Otevřete aplikaci SQL Server Management Studio a připojte toohello místní instance systému SQL Server.

1. V **Průzkumník objektů**, klikněte na tlačítko **zabezpečení**.

1. Klikněte pravým tlačítkem na **přihlášení**. Klikněte na tlačítko **nového přihlašovacího jména**.

1. V **přihlášení – nové**, klikněte na tlačítko **vyhledávání**.

1. Klikněte na tlačítko **umístění**.

1. Zadejte přihlašovací údaje hello domény správce sítě.

1. Používejte účet instalace hello.

1. Nastavit hello přihlášení toobe členem hello **sysadmin** pevné role serveru.

1. Klikněte na **OK**.

Opakováním předchozích kroků na hello další virtuální počítač SQL Server hello.

## <a name="add-failover-clustering-features-tooboth-sql-server-vms"></a>Přidat Clustering převzetí služeb při selhání funkce tooboth virtuálním počítačům systému SQL Server

funkce Clustering převzetí služeb při selhání tooadd hello následující na oba virtuální počítače SQL serveru:

1. Připojení virtuálního počítače systému SQL Server toohello přes hello protokolu RDP (Remote Desktop) pomocí hello *CORP\install* účtu. Otevřete **řídicí panel Správce serveru**.
2. Klikněte na tlačítko hello **přidat role a funkce** odkaz na řídicí panel hello.

    ![Správce serveru – přidání rolí](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Vyberte **Další** dokud nezískáte toohello **funkce serveru** části.
4. V **funkce**, vyberte **Clustering převzetí služeb při selhání**.
5. Přidejte všechny další požadované funkce.
6. Klikněte na tlačítko **nainstalovat** tooadd hello funkce.

Opakujte kroky hello na hello další virtuální počítač SQL Server.

## <a name="a-nameendpoint-firewall-configure-hello-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall">Konfigurace brány firewall hello na každý virtuální počítač s SQL serverem

Hello řešení vyžaduje následující porty TCP toobe otevřít v bráně firewall hello hello:

- **Virtuální počítač systému SQL Server**:<br/>
   Port 1433 pro výchozí instanci systému SQL Server.
- **Sondu nástroje pro vyrovnávání zatížení Azure:**<br/>
   Jakýkoli dostupný port. Příklady často používají 59999.
- **Koncový bod zrcadlení databáze:** <br/>
   Jakýkoli dostupný port. Příklady často používají 5022.

Hello brány firewall porty nutné toobe otevřete na oba virtuální počítače serveru SQL.

Metoda Hello otevření portů hello závisí na řešení hello brány firewall, který používáte. Hello další části se dozvíte, jak tooopen hello porty v bráně Windows Firewall. Otevřete porty hello vyžaduje na všech virtuálních počítačů serveru SQL.

### <a name="open-a-tcp-port-in-hello-firewall"></a>Otevřete TCP port v bráně firewall hello

1. Na hello první systému SQL Server **spustit** obrazovky, spusťte **brány Windows Firewall s pokročilým zabezpečením**.
2. V levém podokně hello vyberte **příchozí pravidla**. V pravém podokně hello, klikněte na tlačítko **nové pravidlo**.
3. Pro **typ pravidla**, zvolte **Port**.
4. Hello port, zadejte **TCP** a typ hello vhodné čísla portů. Viz následující ukázka hello:

   ![Brány firewall pro SQL](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. Klikněte na **Další**.
6. Na hello **akce** ponechte **povolit připojení hello** vybrané a potom klikněte na **Další**.
7. Na hello **profil** přijměte hello výchozí nastavení a pak klikněte na tlačítko **Další**.
8. Na hello **název** stránky, zadejte název pravidla (například **Azure LB testu**) v hello **název** textového pole a pak klikněte na tlačítko **Dokončit**.

Opakujte tyto kroky na hello druhý virtuální počítač SQL Server.

## <a name="next-steps"></a>Další kroky

* [Vytvoření skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
