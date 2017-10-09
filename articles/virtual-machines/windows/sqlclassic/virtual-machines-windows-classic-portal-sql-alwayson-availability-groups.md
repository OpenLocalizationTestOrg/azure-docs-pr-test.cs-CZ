---
title: "aaaConfigure skupiny dostupnosti Always On v Azure Virtual Machines (klasické) | Microsoft Docs"
description: "Vytvoření skupiny dostupnosti Always On s virtuálními počítači Azure. V tomto kurzu primárně nepoužíváte hello uživatelské rozhraní a nástroje pro skriptování."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: f428ad994a55378c281c959f4696fdcaf50632b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines (klasické)
> [!div class="op_single_selector"]
> * [Klasické: uživatelského rozhraní](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasické: prostředí PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Než začnete, vezměte v úvahu, že je nyní možné dokončit tuto úlohu v modelu Azure Resource Manager. Doporučujeme, abyste modelu Azure Resource Manager pro nová nasazení. V tématu [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Azure má dva různé nasazení modely toocreate a pracovat s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek vysvětluje, jak toouse hello modelu nasazení classic. 

toocomplete tuto úlohu s hello modelu Azure Resource Manager, najdete v části [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

Tento kurz začátku do konce ukazuje, jak ke skupinám dostupnosti tooimplement pomocí SQL serveru Always On spuštěná ve virtuálních počítačích Azure.

Na konci hello hello kurzu vaše řešení SQL serveru Always On v Azure budou tvořeny hello následující prvky:

* Virtuální síť, která obsahuje několik podsítí a zahrnuje front-end a back-end podsítě
* Řadič domény s doménou služby Active Directory (Azure AD)
* Dva virtuální počítače, na kterých běží SQL Server a jsou nasazené toohello back-end podsíť a připojený k toohello Azure AD domain
* Clusteru s podporou převzetí služeb při selhání tři uzly s model kvora Většina uzlů hello
* Skupiny dostupnosti, která má dva replik se synchronním potvrzováním databáze dostupnosti

Následující obrázek Hello je grafické reprezentace hello řešení.

![Architektura testovací laboratoř pro skupiny dostupnosti v Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Všimněte si, že toto je možné jednu konfiguraci. Například můžete minimalizovat hello počet virtuálních počítačů pro skupinu dostupnosti dvě repliky. Tato konfigurace se uloží na výpočetní hodiny v Azure pomocí řadiče domény hello jako určující sdílená složka souboru hello kvora v clusteru se dvěma uzly. Tato metoda snižuje počet hello virtuálních počítačů pomocí jedné z hello ilustrovaný konfigurace.

Tento kurz předpokládá hello následující:

* Už máte účet Azure.
* Už víte, jak toouse hello grafickým uživatelským rozhraním v hello virtuálního počítače Galerie tooprovision klasické virtuální počítač, který spouští server SQL Server.
* Už máte plnou Principy skupin dostupnosti Always On. Další informace najdete v tématu [skupin dostupnosti Always On (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Pokud vás zajímá pomocí skupin dostupnosti Always On s SharePoint, také zobrazit [nakonfigurovat SQL Server 2012 skupin dostupnosti Always On pro SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-hello-virtual-network-and-domain-controller-server"></a>Vytvoření virtuální sítě hello a server řadiče domény
Začínat nové zkušební účet Azure. Po nastavení účtu musí být na domovské obrazovce hello hello portál Azure classic.

1. Klikněte na tlačítko hello **nový** tlačítko v hello levém horním rohu hello dolní části stránky hello, jak ukazuje následující snímek obrazovky hello.
   
    ![Kliknutím na tlačítko Nová hello portálu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Klikněte na tlačítko **síťové služby** > **virtuální sítě** > **vytvořit vlastní**, jak ukazuje následující snímek obrazovky hello.
   
    ![Vytvoření virtuální sítě](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. V hello **vytvořit virtuální síť A** dialogové okno pole, vytvořte nové virtuální sítě tak, že procházení hello stránky a pomocí nastavení hello ve hello následující tabulka. 
   
   | Stránka | Nastavení |
   | --- | --- |
   | Podrobnosti virtuální sítě |**Název = ContosoNET**<br/>**OBLAST = západní USA** |
   | Servery DNS a připojení VPN |Žádný |
   | Adresní prostory virtuální sítě |Hello následující snímek obrazovky ukazuje nastavení: ![Vytvoření virtuální sítě](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Vytvořte hello virtuální počítač, který budete používat jako hello řadič domény (DC). Klikněte na tlačítko **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**, jak je znázorněno v Následující snímek obrazovky Hello.
   
    ![Vytvoření virtuálního počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. V hello **vytvořit virtuální počítač A** dialogové okno Nový virtuální počítač nakonfigurujte tak, že procházení hello stránky a pomocí nastavení hello ve hello následující tabulka. 
   
   | Stránka | Nastavení |
   | --- | --- |
   | Vyberte operační systém virtuálního počítače hello |Windows Server 2012 R2 Datacenter |
   | Konfigurace virtuálního počítače |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoDC<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A2 (2 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |
   | Konfigurace virtuálního počítače |**CLOUDOVÉ služby** = vytvořit novou cloudovou službu<br/>**Název CLOUDOVÉ služby DNS** = název jedinečný cloudové služby<br/>**NÁZEV DNS** = jedinečný název (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**Skupina dostupnosti** = (None) |
   | Možnosti virtuálního počítače |Použít výchozí nastavení |

Po dokončení konfigurace hello nový virtuální počítač, počkejte provsioned toobe hello virtuálního počítače. Tento proces trvat některé toofinish čas. Pokud kliknete na tlačítko hello **virtuálního počítače** ve hello Azure classic portálu, můžete zobrazit stavy recyklace ContosoDC z **počáteční (zřizování)** příliš**Zastaveno**, **Od**, **spuštění (zřizování)**a v neposlední řadě **systémem**.

Nyní je úspěšně zřízen Hello serveru řadiče domény. Dále nakonfigurujete hello domény služby Active Directory na tomto serveru řadiče domény.

## <a name="configure-hello-domain-controller"></a>Konfigurace řadiče domény hello
V hello následující kroky můžete nakonfigurovat hello počítači ContosoDC jako řadič domény pro spolecnost.contoso.com.

1. Hello portálu, vyberte hello **ContosoDC** počítače. Na hello **řídicí panel** , klikněte na **připojit** tooopen souboru vzdálené plochy (RDP) pro přístup ke vzdálené ploše.
   
    ![Připojit tooVritual počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Přihlaste se pomocí účtu správce nakonfigurované (**\AzureAdmin**) a heslo (**Contoso! 000**).
3. Ve výchozím nastavení, hello **správce serveru** měla by se zobrazit řídicí panel.
4. Klikněte na tlačítko hello **přidat role a funkce** odkaz na řídicí panel hello.
   
    ![V Průzkumníku serveru přidat role](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Klikněte na tlačítko **Další** dokud nezískáte toohello **role serveru** části.
6. Vyberte hello **Active Directory Domain Services** a **DNS Server** role. Po zobrazení výzvy, přidejte další funkce, které vyžadují tyto role.
   
   > [!NOTE]
   > Zobrazí se upozornění, že neexistuje žádná statická IP adresa ověřování. Pokud testujete hello konfigurace, klikněte na tlačítko **pokračovat**. Pro produkčních scénářích [použijte PowerShell tooset hello statickou IP adresu počítače řadiče domény hello](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Role dialogové okno Přidání](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Klikněte na tlačítko **Další** dokud se nedostanete hello **potvrzení** části. Vyberte hello **restartování hello cílový server automaticky podle potřeby** zaškrtávací políčko.
8. Klikněte na **Nainstalovat**.
9. Po instalaci funkce hello vraťte toohello **správce serveru** řídicího panelu.
10. Vyberte hello nové **služby AD DS** možnost v levém podokně hello.
11. Klikněte na tlačítko hello **Další** odkaz na panelu hello žlutý upozornění.
    
     ![AD DS dialogové ve virtuálním počítači serveru DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. V hello **akce** sloupec hello **všechny podrobnosti úlohy serveru** dialogové okno, klikněte na tlačítko **zvýšit úroveň tohoto řadiče domény tooa server**.
13. V hello **Průvodce konfigurací služby domény služby Active Directory**, použijte hello následující hodnoty:
    
    | Stránka | Nastavení |
    | --- | --- |
    | Konfigurace nasazení |**Přidat novou doménovou strukturu** = vybrané<br/>**Název kořenové domény** = corp.contoso.com |
    | Možnosti řadiče domény |**Heslo** = Contoso! 000<br/>**Potvrzení hesla** = Contoso! 000 |
14. Klikněte na tlačítko **Další** toogo prostřednictvím hello dalších stránek v Průvodci hello. Na hello **Kontrola předpokladů** ověřte, najdete v části hello následující zprávou: **všech požadovaných součástí byly úspěšně zkontrolovány**. Všimněte si, že byste měli zkontrolovat všechny příslušné zprávy upozornění, ale je možné toocontinue s instalací hello.
15. Klikněte na **Nainstalovat**. Hello **ContosoDC** virtuální počítač se automaticky restartuje.

## <a name="configure-domain-accounts"></a>Konfigurace účtů domény
Další kroky Hello nakonfigurovat hello účtů služby Active Directory pro pozdější použití.

1. Přihlaste se znovu toohello **ContosoDC** počítače.
2. V **správce serveru**, klikněte na tlačítko **nástroje** > **Centrum správy služby Active Directory**.
   
    ![Centrum správy služby Active Directory](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. V hello **Centrum správy služby Active Directory**, vyberte **corp (místní)** v levém podokně hello.
4. Na pravém hello **úlohy** podokně klikněte na tlačítko **nový** > **uživatele**. Použijte hello následující nastavení:
   
   | Nastavení | Hodnota |
   | --- | --- |
   | **Křestní jméno** |Instalace |
   | **SamAccountName uživatele** |Instalace |
   | **Heslo** |Contoso! 000 |
   | **Potvrdit heslo** |Contoso! 000 |
   | **Další možnosti hesla** |Vybráno |
   | **Heslo je platné stále** |Zaškrtnuté |
5. Klikněte na tlačítko **OK** toocreate hello **nainstalovat** uživatele. Tento účet bude použité tooconfigure hello převzetí služeb při selhání clusteru a hello skupiny dostupnosti.
6. Vytvořte dva další uživatele, **CORP\SQLSvc1** a **CORP\SQLSvc2**, s hello stejný postup. Tyto účty se použije pro instance systému SQL Server hello. Dále musíte toogive **CORP\Install** hello clustering převzetí služeb při selhání Windows tooconfigure potřebná oprávnění.
7. V hello **Centrum správy služby Active Directory**, klikněte na tlačítko **corp (místní)** v levém podokně hello. V hello **úlohy** podokně klikněte na tlačítko **vlastnosti**.
   
    ![Vlastnosti uživatele CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Vyberte **rozšíření**a potom klikněte na hello **Upřesnit** na hello tlačítko **zabezpečení** kartě.
9. V hello **Upřesnit nastavení zabezpečení pro corp** dialogové okno, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **vybrat objekt zabezpečení**, vyhledejte **CORP\Install**a potom klikněte na **OK**.
11. Vyberte hello **číst vlastnosti všech** a **vytvářet objekty počítačů** oprávnění.
    
     ![Oprávnění uživatele Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Klikněte na tlačítko **OK**a potom klikněte na **OK** znovu. Okno vlastností corp zavřít hello.

Teď, když jste nakonfigurovali služby Active Directory a hello uživatelských objektů, vytvoříte tři virtuální počítače systému SQL Server a připojovat je toothis domény.

## <a name="create-hello-sql-server-virtual-machines"></a>Vytvořit virtuální počítače systému SQL Server hello
Vytvořte tři virtuální počítače. Jeden je pro uzel clusteru a dva jsou pro systém SQL Server. toocreate hello virtuálních počítačů, přejděte zpět toohello portál Azure classic, klikněte na tlačítko **nový** > **výpočetní** > **virtuálního počítače**  >  **Z Galerie**. Pak pomocí šablony hello v následující tabulce toohelp vytvoříte virtuální počítače hello hello.

| Stránka | VM1 | VIRTUÁLNÍHO POČÍTAČE 2 | VM3 |
| --- | --- | --- | --- |
| Vyberte operační systém virtuálního počítače hello |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Konfigurace virtuálního počítače |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoWSFCNode<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A2 (2 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoSQL1<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A3 (4 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoSQL2<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A3 (4 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |
| Konfigurace virtuálního počítače |**CLOUDOVÉ služby** = název DNS dříve vytvořenou jedinečný cloudové služby (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**SKUPINY dostupnosti** = vytvořit dostupnosti nastavit<br/>**NÁZEV SADY DOSTUPNOSTI** = SQLHADR |**CLOUDOVÉ služby** = název DNS dříve vytvořenou jedinečný cloudové služby (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**SKUPINY dostupnosti** = SQLHADR (můžete také nakonfigurovat hello dostupnosti nastavit po vytvoření hello počítače. Všechny tři počítače by se měla přiřadit skupinu dostupnosti SQLHADR toohello.) |**CLOUDOVÉ služby** = název DNS dříve vytvořenou jedinečný cloudové služby (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**SKUPINY dostupnosti** = SQLHADR (můžete také nakonfigurovat hello dostupnosti nastavit po vytvoření hello počítače. Všechny tři počítače by se měla přiřadit skupinu dostupnosti SQLHADR toohello.) |
| Možnosti virtuálního počítače |Použít výchozí nastavení |Použít výchozí nastavení |Použít výchozí nastavení |

<br/>

> [!NOTE]
> předchozí konfiguraci Hello doporučuje úroveň STANDARD virtuálních počítačů, protože základní vrstvě počítače nepodporují koncové body s vyrovnáváním zatížení. Je třeba koncové body s vyrovnáváním zatížení novější toocreate naslouchací proces skupiny dostupnosti. Navíc hello velikosti počítačů, které jsou zde navrhovaná jsou určené pro testování v Azure Virtual Machines skupiny dostupnosti. Hello nejlepší výkon v produkčním prostředí, najdete v části hello doporučení pro velikosti počítačů systému SQL Server a konfigurace v [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Po hello tři virtuální počítače jsou plně zřízený, je nutné toojoin je toohello **corp.contoso.com** domény a přidělit počítače toohello CORP\Install práva správce. toodo hello tento, použijte následující kroky pro každý hello tři virtuální počítače.

1. Nejprve změňte adresu serveru DNS hello upřednostňovaný. Stáhnout adresář místního souboru tooyour protokolu RDP pro každý virtuální počítač výběrem hello virtuálního počítače v seznamu hello a kliknutím na hello **Connect** tlačítko. tooselect virtuálního počítače, klikněte na libovolné místo ale hello první buňky v řádku hello, jak ukazuje následující snímek obrazovky hello.
   
    ![Stáhnout soubor RDP hello](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Otevřete hello protokolu RDP souboru, který jste stáhli a přihlaste se pomocí účtu správce nakonfigurované toohello virtuálního počítače (**BUILTIN\AzureAdmin**) a heslo (**Contoso! 000**).
3. Po přihlášení, měli byste vidět hello **správce serveru** řídicího panelu. Klikněte na tlačítko **místní Server** v levém podokně hello.
4. Klikněte na tlačítko hello **IPv4 adresy přiřazené serverem DHCP, pro protokol IPv6** odkaz.
5. V hello **připojení k síti** dialogovém okně klikněte na ikonu sítě hello.
   
    ![Server DNS virtuálního počítače preferované hello změn](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Na panelu příkazů hello, klikněte na tlačítko **změnit hello nastavení pro toto připojení**. (V závislosti na hello velikost okna aplikace, můžete mít tooclick hello dvojitá šipka vpravo toosee tento příkaz).
7. Vyberte **Internet Protocol verze 4 (TCP/IPv4)**a potom klikněte na **vlastnosti**.
8. Vyberte **hello použijte následující adresy serverů DNS** a pak zadejte **10.10.2.4** v **upřednostňovaný server DNS**.
9. Hello **10.10.2.4** adresa je hello adresu, která je přiřazená tooa virtuálního počítače v podsíti 10.10.2.0/24 hello v virtuální sítě Azure. Tento virtuální počítač je **ContosoDC**. tooverify **ContosoDC**na IP adresu, použijte **nslookup contosodc** v okně příkazového řádku hello, jak ukazuje následující snímek obrazovky hello.
   
    ![Použití nástroje NSLOOKUP toofind IP adresy pro řadič domény](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Klikněte na tlačítko **OK** > **Zavřít** toocommit hello změny. Nyní můžete připojit virtuální počítač hello příliš**corp.contoso.com**.
11. Zpět v hello **místní Server** okně klikněte na tlačítko hello **pracovní skupiny** odkaz.
12. V hello **název počítače** klikněte na tlačítko **změnu**.
13. Vyberte hello **domény** zaškrtávací políčko, typ **corp.contoso.com** v hello textového pole a pak klikněte na **OK**.
14. V hello **zabezpečení systému Windows** dialogovém okně zadejte hello pověření pro účet správce domény výchozí hello (**CORP\AzureAdmin**) a heslo hello (**Contoso! 000**).
15. Když se zobrazí zpráva "Vítá toohello doméně corp.contoso.com" hello, klikněte na tlačítko **OK**.
16. Klikněte na tlačítko **Zavřít** > **restartovat nyní** v dialogovém okně hello.

### <a name="add-hello-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Přidat hello Corp\Install uživatele jako správce na každém virtuálním počítači
1. Počkejte na restartování hello virtuálního počítače a pak otevřete hello RDP soubor znovu toosign toohello virtuální počítač pomocí hello **BUILTIN\AzureAdmin** účtu.
2. V **správce serveru** klikněte na tlačítko **nástroje** > **Správa počítače**.
   
    ![Správa počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. V hello **Správa počítače** dialogové okno, rozbalte seznam **místní uživatelé a skupiny**a potom klikněte na **skupiny**.
4. Klikněte dvakrát na hello **správci** skupiny.
5. V hello **správci vlastnosti** dialogovém okně klikněte na hello **přidat** tlačítko.
6. Zadejte uživatele hello **CORP\Install**a potom klikněte na **OK**. Po zobrazení výzvy k zadání pověření, použijte hello **AzureAdmin** účet s hello **Contoso! 000** heslo.
7. Klikněte na tlačítko **OK** tooclose hello **vlastností správce** dialogové okno.

### <a name="add-hello-failover-clustering-feature-tooeach-virtual-machine"></a>Přidat hello Clustering převzetí služeb při selhání funkce tooeach virtuální počítač
1. V hello **správce serveru** řídicí panel, klikněte na tlačítko **přidat role a funkce**.
2. V hello **Průvodce přidáním rolí a funkcí**, klikněte na tlačítko **Další** dokud nezískáte toohello **funkce** stránky.
3. Vyberte **Clustering převzetí služeb při selhání**. Po zobrazení výzvy, přidejte další závislé součásti.
   
    ![Přidejte funkci Clustering převzetí služeb při selhání toovirtual počítač](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Klikněte na tlačítko **Další**a potom klikněte na **nainstalovat** na hello **potvrzení** stránky.
5. Když hello **Clustering převzetí služeb při selhání** po dokončení instalace funkce, klikněte na tlačítko **Zavřít**.
6. Odhlásit z hello virtuálního počítače.
7. Hello kroků v této části pro všechny tři servery: **ContosoWSFCNode**, **ContosoSQL1**, a **ContosoSQL2**.

Hello teď zřizování virtuálních počítačů systému SQL Server a spuštěna, ale každá z nich má hello výchozí možnosti pro SQL Server.

## <a name="create-hello-failover-cluster"></a>Vytvoření clusteru převzetí služeb při selhání hello
V této části vytvoříte clusteru hello převzetí služeb při selhání, který bude hostovat hello skupiny dostupnosti, který chcete vytvořit později. Nyní by měli mít provádí hello následující tooeach hello tři virtuální počítače, které budete používat v clusteru převzetí služeb při selhání hello:

* Zcela zřizované hello virtuálního počítače v Azure
* Připojené k doméně toohello hello virtuálního počítače
* Přidat **CORP\Install** toohello místní skupiny Administrators
* Funkci clustering převzetí služeb při selhání přidané hello

Všechny tyto jsou požadavky na každém virtuálním počítači, než bude možné se připojit toohello převzetí služeb při selhání clusteru.

Také Upozorňujeme, že hello virtuální síť Azure není chovat v hello stejný způsob jako místní sítě. Je třeba toocreate hello clusteru v hello následující pořadí:

1. Vytvoření clusteru s jedním uzlem na jednom uzlu (**ContosoSQL1**).
2. Upravit hello clusteru IP adresu tooan nepoužívané IP adresu (**10.10.2.101**).
3. Uveďte název clusteru hello online.
4. Přidání dalších uzlů hello (**ContosoSQL2** a **ContosoWSFCNode**).

Pomocí následujících kroků toocomplete hello úlohy, které plně konfigurace clusteru hello hello.

1. Otevřete hello soubor RDP pro **ContosoSQL1**a přihlaste se pomocí účtu domény hello **CORP\Install**.
2. V hello **správce serveru** řídicí panel, klikněte na tlačítko **nástroje** > **Správce clusteru převzetí služeb při selhání**.
3. V levém podokně hello, klikněte pravým tlačítkem na **Správce clusteru převzetí služeb při selhání**a potom klikněte na **vytvoření clusteru s podporou**, jak ukazuje následující snímek obrazovky hello.
   
    ![Vytvoření clusteru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. V hello Průvodce vytvořením clusteru, vytvoření clusteru s jedním uzlem procházení hello stránky a použitím hello nastavení v následující tabulce hello:
   
   | Stránka | Nastavení |
   | --- | --- |
   | Než začnete |Použít výchozí nastavení |
   | Vyberte servery |Typ **ContosoSQL1** v **zadejte název serveru** a klikněte na tlačítko **přidat** |
   | Upozornění ověření |Vyberte **ne. I nevyžadují podporu společnosti Microsoft pro tento cluster a proto nechcete, aby toorun hello ověřovací testy. Po klepnutí na tlačítko Další, pokračovat ve vytváření clusteru hello**. |
   | Přístupový bod pro hello Správa clusteru |Typ **Cluster1** v **název clusteru** |
   | Potvrzení |Použít výchozí hodnoty, pokud nepoužíváte prostory úložiště. Zobrazí upozornění hello za touto tabulkou. |
   
   > [!WARNING]
   > Pokud používáte [prostory úložiště](https://technet.microsoft.com/library/hh831739), které skupiny více disků do fondů úložiště, musíte vymazat hello **přidejte všechna vhodná úložiště toohello cluster** políčko hello **potvrzení** stránky. Pokud není zaškrtnutí tohoto políčka, bude během procesu clusteringu hello odpojit virtuální disky hello. V důsledku toho se také neobjeví v Správce disků nebo v Průzkumníku dokud hello prostory úložiště se odeberou z hello clusteru a znovu připojit pomocí prostředí PowerShell.
   > 
   > 
5. V levém podokně hello rozbalte **Správce clusteru převzetí služeb při selhání**a potom klikněte na **Cluster1.corp.contoso.com**.
6. V prostředním podokně hello, posuňte se dolů toohello **základní prostředky clusteru** části a rozbalte hello **název: Clutser1** podrobnosti. Měli byste vidět obou hello **název** a hello **IP adresu** prostředky v hello **se nezdařilo** stavu. Hello prostředek IP adresy nelze do online režimu protože hello clusteru je přiřazen hello stejnou IP adresu jako počítač hello, sebe, což je duplicitní adresu.
7. Klikněte pravým tlačítkem na hello se nezdařilo **IP adresu** prostředek a pak klikněte na tlačítko **vlastnosti**.
   
    ![Vlastnosti clusteru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Vyberte **statickou IP adresu**, zadejte **10.10.2.101** v hello **adresu** textového pole a pak klikněte na tlačítko **OK**.
9. V hello **základní prostředky clusteru** části, klikněte pravým tlačítkem na **název: Cluster1**a potom klikněte na **přepnout do režimu Online**. Počkejte, dokud jsou obě prostředky online. Při přechodu hello prostředek názvu clusteru do režimu online, hello serveru řadiče domény se aktualizuje pomocí nového účtu počítače služby Active Directory. Tento účet služby Active Directory se použije Clusterová služba skupiny dostupnosti hello toorun později.
10. Přidáte zbývající uzly clusteru toohello hello. Ve stromu hello prohlížeče, klikněte pravým tlačítkem na **Cluster1.corp.contoso.com**a potom klikněte na **přidat uzel**, jak ukazuje následující snímek obrazovky hello.
    
     ![Přidání uzlu clusteru toohello](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. V hello **Průvodce přidáním uzlu**, klikněte na tlačítko **Další** na hello **zvolit servery** přidejte **ContosoSQL2** a **ContosoWSFCNode**  toohello seznamu tak, že zadáte název serveru hello v **zadejte název serveru** a pak levým na **přidat**. Až budete hotovi, klikněte na tlačítko **Další**.
12. Na hello **upozornění ověření** klikněte na tlačítko **ne**, i když v produkční scénář, měli byste provést testy pro ověření hello. Pak klikněte na **Další**.
13. Na hello **potvrzení** klikněte na tlačítko **Další** tooadd hello uzly.
    
    > [!WARNING]
    > Pokud používáte [prostory úložiště](https://technet.microsoft.com/library/hh831739), které skupiny více disků do fondů úložiště, musíte vymazat hello **přidejte všechna vhodná úložiště toohello cluster** zaškrtávací políčko. Pokud není zaškrtnutí tohoto políčka, bude během procesu clusteringu hello odpojit virtuální disky hello. V důsledku toho také nezobrazí v Správce disků nebo v Průzkumníku dokud hello prostory úložiště jsou odebrány z clusteru a znovu připojit pomocí prostředí PowerShell.
    > 
    > 
14. Po přidání clusteru toohello hello uzly, klikněte na tlačítko **Dokončit**. Správce clusteru převzetí služeb při selhání by měl nyní zobrazit, že má tři uzly clusteru a jejich seznam v hello **uzly** kontejneru.
15. Odhlásit z relace vzdálené plochy hello.

## <a name="prepare-hello-sql-server-instances-for-availability-groups"></a>Příprava hello instance systému SQL Server pro skupiny dostupnosti
V této části provedete text hello, postupujte na obou **ContosoSQL1** a **contosoSQL2**:

* Přidat přihlašovací údaje pro **NT AUTHORITY\System** potřebná oprávnění, nastavte toohello výchozí instanci SQL serveru.
* Přidat **CORP\Install** jako instanci systému SQL Server výchozí toohello role sysadmin.
* Otevřete hello brány firewall pro vzdálený přístup systému SQL Server.
* Povolte funkci hello vždy na dostupnosti skupiny.
* Změna účtu služby SQL Server hello příliš**CORP\SQLSvc1** a **CORP\SQLSvc2**, v uvedeném pořadí.

Tyto akce lze provést v libovolném pořadí. Nicméně hello následující kroky provede je v pořadí. Postupujte podle kroků hello pro obě **ContosoSQL1** a **ContosoSQL2**:

1. Pokud nebyly odhlásili z relace vzdálené plochy hello hello virtuálního počítače, udělejte to teď.
2. Otevřete hello RDP soubory pro **ContosoSQL1** a **ContosoSQL2**a přihlaste se jako **BUILTIN\AzureAdmin**.
3. Přidat **NT AUTHORITY\System** toohello přihlášení serveru SQL Server potřebná oprávnění. Otevřete **SQL Server Management Studio**.
4. Klikněte na tlačítko **připojit** tooconnect toohello výchozí instanci SQL serveru.
5. V **Průzkumník objektů**, rozbalte položku **zabezpečení**a potom rozbalte **přihlášení**.
6. Klikněte pravým tlačítkem na hello **NT AUTHORITY\System** přihlašovací údaje a pak klikněte na tlačítko **vlastnosti**.
7. Na hello **zabezpečitelné prostředky** stránku hello místního serveru, vyberte **Grant** pro hello následující oprávnění a potom klikněte na **OK**.
   
   * Příkaz ALTER žádnou skupinu dostupnosti
   * Připojení SQL
   * Zobrazení stavu serveru
8. Přidat **CORP\Install** jako **sysadmin** role toohello výchozí instanci SQL serveru. V **Průzkumník objektů**, klikněte pravým tlačítkem na **přihlášení**a potom klikněte na **nového přihlašovacího jména**.
9. Typ **CORP\Install** v **přihlašovací jméno**.
10. Na hello **role serveru** vyberte **sysadmin**a potom klikněte na **OK**. Po vytvoření hello přihlášení, zobrazí se rozšířením **přihlášení** v **Průzkumník objektů**.
11. toocreate pravidlo brány firewall pro SQL Server hello **spustit** obrazovce otevřete **brány Windows Firewall s pokročilým zabezpečením**.
12. V levém podokně hello vyberte **příchozí pravidla**. V pravém podokně hello, klikněte na **nové pravidlo**.
13. Na hello **typ pravidla** klikněte na tlačítko **programu** > **Další**.
14. Na hello **programu** vyberte **tato cesta k programu**, typ **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** v hello textového pole a pak klikněte na **Další**. Pokud jsou následující těchto pokynů, ale pomocí SQL Server 2012, adresář systému SQL Server hello je **MSSQL11. MSSQLSERVER**.
15. Na hello **akce** ponechte **povolit připojení hello** vybrané a potom klikněte na **Další**.
16. Na hello **profil** přijměte hello výchozí nastavení a pak klikněte na tlačítko **Další**.
17. Na hello **název** stránky, zadejte název pravidla, například **systému SQL Server (pravidlo programu)**, v hello **název** textové pole a pak klikněte na **Dokončit**.
18. tooenable hello **skupin dostupnosti Always On** funkce na hello **spustit** obrazovce otevřete **SQL Server Configuration Manager**.
19. Ve stromu hello prohlížeče, klikněte na tlačítko **služby SQL Server**, klikněte pravým tlačítkem na hello **serveru SQL (MSSQLSERVER)** služby a potom klikněte na **vlastnosti**.
20. Klikněte na tlačítko hello **vždy na vysokou dostupnost** vyberte **povolte skupiny dostupnosti Always On**, jak je znázorněno v hello následující snímek obrazovky a pak klikněte na **použít**. Klikněte na tlačítko **OK** v hello dialogové okno a nezavírejte hello **vlastnosti** zatím dialogové. Služba SQL Server hello se restartuje po provedení změny účtu služby hello.
    
     ![Vždy zapnout na skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. toochange hello účet služby SQL Server, klikněte na tlačítko hello **přihlášení** , zadejte **CORP\SQLSvc1** (pro **ContosoSQL1**) nebo **CORP\SQLSvc2** () pro **ContosoSQL2**) v **název účtu**, vyplňte a potvrďte heslo hello a pak klikněte na tlačítko **OK**.
22. V poli hello dialogové okno, které se otevře, klikněte na tlačítko **Ano** toorestart hello služby SQL Server. Po restartování služby SQL Server hello změny provedené v hello **vlastnosti** dialogové okno, které jsou platné.
23. Odhlásit z hello virtuálních počítačů.

## <a name="create-hello-availability-group"></a>Vytvoření skupiny dostupnosti hello
Nyní je připraven tooconfigure skupiny dostupnosti. Dole je přehled co provedete:

* Vytvořit novou databázi (**MyDB1**) na **ContosoSQL1**.
* Proveďte úplné zálohování a zálohování protokolu transakcí databáze hello.
* Hello úplné obnovení a zálohování protokolů příliš**ContosoSQL2** s hello **NORECOVERY** možnost.
* Vytvoření skupiny dostupnosti hello (**AG1**) se synchronním potvrzováním, automatické převzetí služeb při selhání a čitelných místních replikách.

### <a name="create-hello-mydb1-database-on-contososql1"></a>Vytvořit databázi MyDB1 hello na ContosoSQL1
1. Pokud jste již nezaregistrovali mimo relací vzdálené plochy hello pro **ContosoSQL1** a **ContosoSQL2**, udělejte to teď.
2. Otevřete hello soubor RDP pro **ContosoSQL1**a přihlaste se jako **CORP\Install**.
3. V **Průzkumníka souborů**v části **C:\\**, vytvořte adresář s názvem **zálohování**. Budete používat tento adresář tooback a obnovení databáze.
4. Klikněte pravým tlačítkem na nový adresář hello, přejděte příliš**sdílet s**a potom klikněte na **Určití lidé**, jak ukazuje následující snímek obrazovky hello.
   
    ![Vytvořte složku s zálohování](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Přidat **CORP\SQLSvc1**a dejte mu hello **pro čtení a zápis** oprávnění. Přidat **CORP\SQLSvc2**a dejte mu hello **čtení** oprávnění, jak je znázorněno v hello následující snímek obrazovky a pak klikněte na **sdílené složky**. Po dokončení procesu hello sdílení souborů, klikněte na tlačítko **provádí**.
   
    ![Udělení oprávnění pro složku zálohování](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. toocreate hello databáze, z hello **spustit** nabídce otevřete **SQL Server Management Studio**a potom klikněte na **připojit** tooconnect toohello výchozí instanci SQL serveru.
7. V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze**a potom klikněte na **novou databázi**.
8. V **název databáze**, typ **MyDB1**a potom klikněte na **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Proveďte úplné zálohování MyDB1 a obnovte ji na ContosoSQL2
1. toomake hello úplnou zálohu databáze v **Průzkumník objektů**, rozbalte položku **databáze**, klikněte pravým tlačítkem na **MyDB1**, bod příliš**úlohy**, a pak klikněte na tlačítko **zálohování**.
2. V hello **zdroj** část, zachovat **typ zálohování** nastavit příliš**úplné**. V hello **cílové** klikněte na tlačítko **odebrat** tooremove hello výchozí cestu k souboru pro záložní soubor hello.
3. V hello **cílové** klikněte na tlačítko **přidat**.
4. V hello **název souboru** textového pole, typ  **\\ContosoSQL1\backup\MyDB1.bak**, klikněte na tlačítko **OK**a potom klikněte na **OK** znovu tooback zálohu databáze hello. Po dokončení operace zálohování hello, klikněte na tlačítko **OK** znovu tooclose hello dialogové okno.
5. toomake transakce zálohu databáze hello přihlásit **Průzkumník objektů**, rozbalte položku **databáze**, klikněte pravým tlačítkem na **MyDB1**, bod příliš**úlohy**a potom klikněte na **zálohování**.
6. V **typ zálohování**, vyberte **transakčního protokolu**. Zachovat hello **cílové** souboru toohello cesta sady, jeden jste dřív zadali a potom klikněte na **OK**. Po dokončení operace zálohování hello, klikněte na tlačítko **OK** znovu.
7. toorestore hello úplné a transakce protokolu zálohování **ContosoSQL2**, otevřete soubor hello protokolu RDP pro **ContosoSQL2**a přihlaste se jako **CORP\Install**. Nechte relace vzdálené plochy hello pro **ContosoSQL1** otevřete.
8. Z hello **spustit** nabídce otevřete **SQL Server Management Studio**a potom klikněte na **připojit** tooconnect toohello výchozí instanci SQL serveru.
9. V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze**a potom klikněte na **obnovit databázi**.
10. V hello **zdroj** vyberte **zařízení**a klikněte na tlačítko se třemi tečkami hello **...** .
11. V **vyberte zálohovací zařízení**, klikněte na tlačítko **přidat**.
12. V **umístění souboru zálohy**, typ  **\\ContosoSQL1\backup**, klikněte na tlačítko **aktualizovat**, vyberte **MyDB1.bak**, klikněte na tlačítko **OK**a potom klikněte na **OK** znovu. Teď byste měli vidět hello úplné zálohy a zálohy protokolu hello v hello **zálohovací sklady toorestore** podokně.
13. Přejděte toohello **možnosti** vyberte **RESTORE WITH NORECOVERY** v **stav obnovení**a potom klikněte na **OK** toorestore hello databáze. Po hello obnovit dokončení operace, klikněte na tlačítko **OK**.

### <a name="create-hello-availability-group"></a>Vytvoření skupiny dostupnosti hello
1. Přejděte zpět relace vzdálené plochy toohello pro **ContosoSQL1**. V **Průzkumník objektů** v systému SQL Server Management Studio klikněte pravým tlačítkem na **vždy na vysokou dostupnost**a potom klikněte na **Průvodce novou skupinou dostupnosti**, jak je znázorněno v hello Následující snímek obrazovky.
   
    ![Spuštění Průvodce vytvořením skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. Na hello **ÚVOD** klikněte na tlačítko **Další**. Na hello **zadejte název skupiny dostupnosti** zadejte **AG1** v **název skupiny dostupnosti**, pak klikněte na tlačítko **Další** znovu.
   
    ![Průvodce pro nové skupiny dostupnosti, zadejte název skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. Na hello **vyberte databáze** vyberte **MyDB1**a potom klikněte na **Další**. databáze Hello splňuje požadavky hello pro skupinu dostupnosti, protože jste provedli aspoň jednu úplnou zálohu na hello určený primární repliky.
   
    ![Průvodce pro nové skupiny Availabilty, vyberte možnost databáze](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. na hello **zadejte repliky** klikněte na tlačítko **přidat repliky**.
   
    ![Průvodce pro nové skupiny Availabilty, zadejte repliky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. V hello **připojit tooServer** dialogové okno, typ **ContosoSQL2** v **název serveru**a potom klikněte na **Connect**.
   
    ![Průvodce pro nové skupiny Availabilty, tooserver připojení](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Zpět na hello **zadejte repliky** stránky, měli byste vidět **ContosoSQL2** uvedené v **dostupné repliky**. Konfigurace replik hello, jak ukazuje následující snímek obrazovky hello. Až budete hotovi, klikněte na tlačítko **Další**.
   
    ![Průvodce pro nové skupiny Availabilty, zadejte repliky (Dokončit)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. Na hello **vybrat počáteční synchronizaci dat** vyberte **připojení pouze**a potom klikněte na **Další**. Jste již provedli synchronizace dat ručně při probíhají zálohování úplné a transakce hello **ContosoSQL1** a obnoví je na **ContosoSQL2**. Můžete zvolit není tooperform hello operace zálohování a obnovení databáze a místo toho vybrat **úplné** toolet hello Průvodce novou skupinou dostupnosti proveďte synchronizaci dat pro vás. Nedoporučujeme ale tuto možnost pro velké databáze, které se nacházejí v některé podniky.
   
    ![Průvodce novou skupinou Availabilty, vyberte data počáteční synchronizace](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. Na hello **ověření** klikněte na tlačítko **Další**. Tato stránka by měla vypadat podobně jako toohello následující snímek obrazovky. Protože jste nenakonfigurovali naslouchací proces skupiny dostupnosti je upozornění pro konfiguraci naslouchacího procesu hello. Toto upozornění můžete ignorovat, protože v tomto kurzu nekonfiguruje naslouchací proces. naslouchací proces hello tooconfigure po dokončení tohoto kurzu, najdete v tématu [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).
   
    ![Průvodce vytvořením skupiny Availabilty, ověření](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. Na hello **Souhrn** klikněte na tlačítko **Dokončit**a potom chvíli počkejte, než Průvodce hello nakonfiguruje hello nové skupiny dostupnosti. Na hello **průběh** stránky, můžete kliknout na **podrobnosti** tooview hello podrobný průběh. Po dokončení Průvodce hello zkontrolovat hello **výsledky** tooverify stránku této skupiny dostupnosti hello je úspěšně vytvořen, jak je znázorněno v hello následující snímek obrazovky a pak klikněte na **Zavřít** tooexit hello Průvodce.
   
    ![Průvodce vytvořením skupiny Availabilty, výsledky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. V **Průzkumník objektů**, rozbalte položku **vždy na vysokou dostupnost**a potom rozbalte **skupiny dostupnosti**. Teď byste měli vidět hello nové skupiny dostupnosti v tomto kontejneru. Klikněte pravým tlačítkem na **AG1 (primární)**a potom klikněte na **zobrazit řídicí panel**.
    
     ![Řídicí panel zobrazit skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. Vaše **vždy na řídicí panel** by měl vypadá podobně jako toohello jednu v hello následující snímek obrazovky. Můžete zobrazit hello repliky, režim převzetí služeb při selhání hello každé repliky a stav synchronizace hello.
    
     ![Řídicí panel skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Vrátí příliš**správce serveru**, klikněte na tlačítko **nástroje**a pak otevřete **Správce clusteru převzetí služeb při selhání**.
13. Rozbalte položku **Cluster1.corp.contoso.com**a potom rozbalte **služeb a aplikací**. Vyberte **role** a Všimněte si, že hello **AG1** byla vytvořená role skupiny dostupnosti. Všimněte si, že AG1 nemá IP adresu podle databázi, kterou klienti mohou připojit toohello skupiny dostupnosti, protože jste nenakonfigurovali naslouchací proces. Můžete připojit přímo toohello primární uzel pro operace čtení a zápis a hello sekundární uzel pro dotazy jen pro čtení.
    
     ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Nepokoušejte toofail přes hello skupiny dostupnosti z hello Správce clusteru převzetí služeb při selhání. Všechny operace převzetí služeb při selhání by měla provést uvnitř **vždy na řídicí panel** v systému SQL Server Management Studio. Další informace najdete v tématu [hello omezení pomocí Správce clusteru převzetí služeb při selhání se skupinami dostupnosti](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Další kroky
Jste teď úspěšně implementovali SQL serveru Always On tak, že vytvoříte skupinu dostupnosti v Azure. tooconfigure naslouchací proces pro tuto skupinu dostupnosti, najdete v části [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).

Další informace o používání systému SQL Server v Azure najdete v tématu [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

