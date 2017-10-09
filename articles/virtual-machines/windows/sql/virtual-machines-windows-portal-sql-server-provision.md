---
title: "aaaProvision virtuálního počítače s SQL serverem | Microsoft Docs"
description: "Vytvořit a připojit tooa virtuálního počítače SQL serverem v Azure pomocí portálu hello. Tento kurz používá hello režimu Resource Manager."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: acb52b180103d83715b51b46e2519211c8f0e362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-in-hello-azure-portal"></a>Zřízení virtuálního počítače s SQL Server v hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

Tento kurz začátku do konce ukazuje, jak toouse hello Azure portálu tooprovision virtuálního počítače se systémem SQL Server.

Galerie virtuálních počítačů (VM) Azure Hello obsahuje několik imagí, které obsahují Microsoft SQL Server. Pomocí několika kliknutí můžete vybrat jednu z hello z Galerie hello bitové kopie virtuálního počítače s SQL a zřídit v prostředí Azure.

V tomto kurzu provedete následující:

* [Vyberte bitovou kopii virtuálního počítače s SQL z Galerie hello](#select-a-sql-vm-image-from-the-gallery)
* [Nakonfigurujte a vytvořte hello virtuálních počítačů](#configure-the-vm)
* [Otevřete hello virtuálního počítače pomocí vzdálené plochy](#open-the-vm-with-remote-desktop)
* [Vzdálené připojení tooSQL serveru](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-hello-gallery"></a>Vyberte bitovou kopii virtuálního počítače s SQL z Galerie hello

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí svého účtu.

   > [!NOTE]
   > Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

2. Na portálu Azure hello, klikněte na tlačítko **nový**. Hello portál otevře hello **nový** okno.

3. V hello **nový** okně klikněte na tlačítko **výpočetní** a pak klikněte na **zobrazit všechny**.

   ![Okno Nová služba Compute](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

4. Do pole hledání text hello, zadejte **systému SQL Server**, a stiskněte klávesu ENTER.

5. Pak klikněte na tlačítko hello **filtru** ikonu a vyberte **Microsoft** pro vydavatele hello. Klikněte na tlačítko **provádí** na výsledky hello hello filtru okno toofilter tooMicrosoft publikovaná bitové kopie systému SQL Server.

   ![Okno Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

5. Zkontrolujte hello dostupných imagí SQL serveru. U každé image je označena příslušná verze SQL Serveru a operační systém.

6. Vyberte hello image s názvem **volné licence: SQL Server 2016 SP1 vývojáře v systému Windows Server 2016**.

   > [!TIP]
   > Hello Developer edition je v tomto kurzu použít, protože je na plné verzi systému SQL Server, který je zdarma pro vývoj pro účely testování. Platíte jenom za hello náklady na provozování hello virtuálních počítačů. Však jsou volné toochoose žádné hello toouse bitové kopie v tomto kurzu.

   > [!TIP]
   > Virtuální počítač SQL Image zahrnují náklady na licencování hello pro SQL Server do hello ceny za minutu z hello virtuální počítač vytvoříte (s výjimkou hello edice Developer a Express). SQL Server Developer je zdarma pro vývoj/testování (ale ne pro produkci) a SQL Express je zdarma pro nenáročné úlohy (méně než 1 GB paměti, méně než 10GB úložiště). Není jiná možnost toobring vaše vlastníte licence (BYOL) a platím pouze pro hello virtuálních počítačů. Tyto názvy bitových kopií mají předponu {BYOL}. 
   >
   > Další informace o těchto možnostech najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

7. V části **Vybrat model nasazení** ověřte, že je vybraný **Resource Manager**. Správce prostředků je hello doporučená model nasazení pro nové virtuální počítače. 

8. Klikněte na možnost **Vytvořit**.

    ![Vytvoření virtuálního počítače s SQL Serverem pomocí Resource Manageru](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-hello-vm"></a>Konfigurace hello virtuálních počítačů
Pro konfiguraci virtuálního počítače s SQL Serverem se používá pět oken.

| Krok | Popis |
| --- | --- |
| **Základy** |[Konfigurace základního nastavení](#1-configure-basic-settings) |
| **Velikost** |[Volba velikosti virtuálního počítače](#2-choose-virtual-machine-size) |
| **Nastavení** |[Konfigurace volitelných funkcí](#3-configure-optional-features) |
| **Nastavení SQL Serveru** |[Konfigurace nastavení SQL Serveru](#4-configure-sql-server-settings) |
| **Souhrn** |[Zkontrolujte hello souhrn](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Konfigurace základního nastavení

Na hello **Základy** okno, zadejte hello následující informace:

* Zadejte jedinečný **název** virtuálního počítače.

* Vyberte **SSD** jako typ disku virtuálního počítače pro zajištění optimálního výkonu.

* Zadejte **uživatelské jméno** pro účet místního správce hello na hello virtuálních počítačů. Tento účet je taky přidaný toohello systému SQL Server **sysadmin** pevné role serveru.

* Zadejte silné **heslo**.

* Pokud máte více předplatných, ověřte, zda je správný pro předplatné hello hello nového virtuálního počítače.

* V hello **skupiny prostředků** zadejte název pro novou skupinu prostředků. Případně, toouse existující skupinu prostředků. Kliknutím na tlačítko **použít existující**. Skupina prostředků je kolekce souvisejících prostředků v Azure (virtuální počítače, účty úložiště, virtuální sítě atd.).

  > [!NOTE]
  > Použití nové skupinu prostředků je užitečné, pokud testujete nasazení SQL Serveru v Azure nebo se snažíte o něm dozvědět více. Po dokončení s testováním, odstraňte hello prostředků skupiny tooautomatically odstranění hello virtuálních počítačů a všemi prostředky spojenými s danou skupinu prostředků. Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../../../azure-resource-manager/resource-group-overview.md).

* Vyberte **umístění** pro hello oblast Azure, který bude hostitelem toto nasazení.

* Klikněte na tlačítko **OK** toosave hello nastavení.

    ![Okno Základy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Volba velikosti virtuálního počítače

Na hello **velikost** kroku, zvolte velikost virtuálního počítače v hello **zvolte velikost** okno. okno Hello jeho otevření zobrazí doporučené velikosti počítačů na základě hello bitové kopie, kterou jste vybrali.

> [!IMPORTANT]
> Hello odhadované měsíční náklady na zobrazené na hello **zvolte velikost** okno nezahrnuje náklady na licencování SQL serveru. Toto je hello náklady hello virtuální počítač samostatně. Pro hello edice systému SQL Server Express a vývojáře Toto je celkový počet odhadované náklady hello. Pro jiné edice, najdete v části hello [virtuální počítače s Windows stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) a vyberte váš cílový edici systému SQL Server. Viz také hello [ceny pokyny pro virtuální počítače Azure SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md).

![Možnosti velikosti virtuálního počítače s SQL Serverem](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Úlohy v produkčním prostředí najdete v části hello doporučené velikosti počítačů a konfiguraci v [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md). Pokud potřebujete velikost počítače, která není uvedena, klikněte na tlačítko hello **zobrazit všechny** tlačítko.

> [!NOTE]
> Další informace o velikostech virtuálních počítačů najdete v tématu [Velikosti virtuálních počítačů](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zvolte velikost počítače a potom klikněte na **Vybrat**.

## <a name="3-configure-optional-features"></a>3. Konfigurace volitelných funkcí

Na hello **nastavení** okno, nakonfigurovat úložiště Azure, sítě a monitorování pro virtuální počítač hello.

* V části **Úložiště** vyberte **Ano** u možnosti **Použít Managed Disks**.

   > [!NOTE]
   > Microsoft pro SQL Server doporučuje službu Managed Disks. Spravované obslužné rutiny úložiště disky pozadí hello. Kromě toho, když virtuální počítače s spravované disky jsou ve hello stejné skupině dostupnosti, Azure distribuuje hello úložiště prostředky tooprovide odpovídající redundance. Další informace najdete v tématu [Přehled služby Azure Managed Disks](../../../storage/storage-managed-disks-overview.md). Podrobnosti o spravovaných discích ve skupině dostupnosti najdete v tématu [Použití spravovaných disků pro virtuální počítače ve skupině dostupnosti](../manage-availability.md).

* V části **sítě**, může přijmout hodnoty hello vyplní automaticky. Můžete také kliknutím na jednotlivé funkce toomanually konfigurace hello **virtuální síť**, **podsíť**, **veřejnou IP adresu**, a **skupinu zabezpečení sítě**. Pro účely tohoto kurzu hello ponechte výchozí hodnoty hello.

* Azure umožňuje **monitorování** ve výchozím nastavení s hello stejný účet úložiště určené pro hello virtuálních počítačů. Tato nastavení tady můžete změnit.

* V části **sadu dostupnosti**, můžete ponechat výchozí hello **žádné** pro účely tohoto kurzu. Pokud máte v plánu tooset skupin dostupnosti AlwaysOn SQL, nakonfigurujte hello dostupnosti tooavoid opakované vytvoření hello virtuálního počítače.  Další informace najdete v tématu [hello Správa dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Po dokončení konfigurace těchto nastavení klikněte na **OK**.

## <a name="4-configure-sql-server-settings"></a>4. Konfigurace nastavení SQL Serveru
Na hello **nastavení systému SQL Server** okně nakonfigurovat konkrétní nastavení a optimalizace pro SQL Server. Hello nastavení, které můžete nakonfigurovat pro SQL Server zahrnují následující hello.

| Nastavení |
| --- |
| [Připojení](#connectivity) |
| [Ověřování](#authentication) |
| [Konfigurace úložiště](#storage-configuration) |
| [Automatizované opravy](#automated-patching) |
| [Automatizované zálohování](#automated-backup) |
| [Integrace se službou Azure Key Vault](#azure-key-vault-integration) |
| [Služby R](#r-services) |

### <a name="connectivity"></a>Připojení

V části **připojení SQL**, zadejte hello typ přístupu, kterou chcete toohello instance systému SQL Server na tomto virtuálním počítači. Pro účely tohoto kurzu hello, vyberte **veřejné (internet)** hello tooallow připojení tooSQL serveru z počítačů nebo služeb na Internetu. Tuto možnost Azure automaticky nakonfiguruje bránu firewall hello a provoz tooallow skupiny zabezpečení sítě hello na portu 1433.

![Možnosti připojení SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Ve výchozím nastavení SQL Server naslouchá na dobře známém portu **1433**. Pro zvýšení zabezpečení změňte port hello v hello předchozí dialogové okno toolisten na jiné než výchozí port, jako třeba 1401. Pokud to provedete, ze všech nástrojů klienta, jako je SSMS, bude nutné se přihlašovat pomocí tohoto portu.

tooconnect tooSQL serveru prostřednictvím hello internet, musíte také povolit ověřování SQL serveru, který je popsán v další části hello.

Pokud si přejete toonot povolit připojení toohello databázový stroj prostřednictvím Internetu Dobrý den, vyberte jednu z hello následující možnosti:

* **Místní (jen uvnitř virtuálního počítače)** tooallow připojení tooSQL pouze ze serveru v rámci hello virtuálních počítačů.
* **Privátní (uvnitř virtuální sítě)** tooallow připojení tooSQL serveru z počítačů nebo služeb v hello stejné virtuální síti.

Obecně platí zvýšit zabezpečení výběrem nejvíce omezujícího připojení hello, které váš scénář umožňuje. Ale jsou všechny možnosti hello zabezpečit prostřednictvím pravidel skupin zabezpečení sítě a ověřování SQL serveru a Windows. Po hello je vytvořena virtuálního počítače můžete upravit skupinu zabezpečení sítě. Další informace najdete v tématu [Informace o zabezpečení pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-security.md).

> [!NOTE]
> Hello bitovou kopii virtuálního počítače pro SQL Server Express edition automaticky není povolen protokol hello TCP/IP. To platí i pro hello veřejných a privátních připojení možnosti. Pro edice Express, musíte použít SQL Server Configuration Manager příliš[ručně povolte protokol hello TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) po vytvoření hello virtuálních počítačů.

### <a name="authentication"></a>Authentication

Pokud budete chtít vyžadovat ověřování SQL Serveru, klikněte v části **Ověřování SQL** na **Povolit**.

![Ověřování SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Pokud máte v plánu tooaccess systému SQL Server přes hello Internetu (tj. hello možnosti veřejného připojení), je nutné povolit ověřování SQL sem. Veřejný přístup toohello systému SQL Server vyžaduje hello použití ověřování SQL.
> 
> 

Pokud povolíte ověřování SQL Serveru, zadejte **přihlašovací jméno** a **heslo**. Toto uživatelské jméno je nakonfigurovaný jako přihlašovací jméno ověřování SQL serveru a členem hello **sysadmin** pevné role serveru. Další informace o režimech ověřování najdete v tématu [Volba režimu ověřování](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

Pokud nepovolíte ověřování systému SQL Server, můžete použít hello účtu místního správce v instanci systému SQL Server hello virtuálních počítačů tooconnect toohello.

### <a name="storage-configuration"></a>Konfigurace úložiště

Klikněte na tlačítko **konfigurace úložiště** požadavky na úložiště toospecify hello.

![Konfigurace úložiště SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Pokud ručně nakonfigurované toouse standardní úložiště virtuálních počítačů, tato možnost není k dispozici. Automatická optimalizace úložiště je k dispozici pouze pro Storage úrovně Premium.

> [!TIP]
> Hello počet zastaví a horní limity hello jednotlivých posuvníků je závislá na hello velikost virtuálního počítače jste vybrali. Je větší a výkonnější virtuální počítač schopný tooscale až více.

Požadavky můžete zadat jako vstupně-výstupní operace za sekundu (IOPs), propustnost v MB/s a celkovou velikost úložiště. Tyto hodnoty nakonfigurujte pomocí posuvníků hello. Tato nastavení úložiště můžete podle náročnosti zpracovávaných úloh změnit. Hello portálu automaticky vypočítá počet disků tooattach hello a nakonfigurovat na základě těchto požadavků.

V části **optimalizace úložiště**, vyberte jednu z hello následující možnosti:

* **Obecné** se hello výchozí nastavení a podporuje většinu úloh.
* **Transakční** zpracování optimalizuje hello úložiště pro standardní úlohy databází OLTP.
* **Datové sklady** optimalizuje hello úložiště pro úlohy analýz a generování sestav.

### <a name="automated-patching"></a>Automatizované opravy

**Automatizované opravy** jsou ve výchozím nastavení povolené. Automatizované opravy umožňují Azure tooautomatically oprava SQL Server a hello operačního systému. Zadejte den v týdnu hello, čas a dobu trvání intervalu údržby. V té době pak Azure nainstaluje potřebné opravy. plánování intervalu údržby Hello používá národní prostředí virtuálních počítačů hello dobu. Pokud nechcete, aby Azure tooautomatically oprava SQL Server a hello operačního systému, klikněte na tlačítko **zakázat**.  

![Automatizované opravy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Další informace najdete v tématu [Automatizované opravy pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatizované zálohování

Automatické zálohování databází můžete pro všechny databáze povolit v části **Automatizované zálohování**. Automatizované zálohování je ve výchozím nastavení zakázané.

Když povolíte automatizované zálohování SQL, můžete nakonfigurovat následující hello:

* Doba uchování dat (dny) pro zálohování
* Toouse účet úložiště pro zálohování
* Možnost šifrování a heslo pro zálohování
* Zálohování systémových databází
* Konfigurování plánu zálohování

tooencrypt hello zálohování, klikněte na tlačítko **povolit**. Pak zadejte hello **heslo**. Azure vytvoří certifikát tooencrypt hello zálohy a používá hello zadaný tooprotect heslo certifikátu.

![Automatizované zálohování SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Další informace najdete v tématu [Automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integrace se službou Azure Key Vault

Klikněte na tlačítko toostore tajné klíče zabezpečení v Azure pro šifrování, **integrace Azure trezoru klíčů** a klikněte na tlačítko **povolit**.

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Hello následující tabulka uvádí požadované tooconfigure parametry hello Azure Key Vault integrace.

| PARAMETR | POPIS | PŘÍKLAD |
| --- | --- | --- |
| **Adresa URL služby Key Vault** |umístění Hello hello trezoru klíčů. |https://contosokeyvault.vault.azure.net/ |
| **Název objektu zabezpečení** |Hlavní název služby Azure Active Directory. Tento název se také označují tooas hello ID klienta. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Tajný kód objektu zabezpečení** |Tajný klíč objektu zabezpečení služby Azure Active Directory. Tento tajný klíč se také označují tooas hello tajný klíč klienta. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Název přihlašovacího údaje** |**Název přihlašovacího údaje**: integrace se službou AZURE vytvoří přihlašovacích údajů v rámci SQL Server, a umožní hello virtuálních počítačů toohave přístup toohello klíče trezoru. Zvolte název pro tyto přihlašovací údaje. |moje_přihlaš1 |

Další informace najdete v tématu [Konfigurace Integrace se službou Azure Key Vault pro virtuální počítače Azure](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="r-services"></a>Služby R

Máte možnost tooenable hello [služby SQL Server R](https://msdn.microsoft.com/library/mt604845.aspx). To vám umožní toouse pokročilé analýzy s SQL Server 2016. Klikněte na tlačítko **povolit** na hello **nastavení systému SQL Server** okno.

> [!NOTE]
> Pro SQL Server 2016 Developer Edition tato možnost je zakázaná nesprávně portálem hello. Pro edici Developer je potřeba služby R povolit ručně po vytvoření virtuálního počítače.

![Povolení služeb R na SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

Po dokončení konfigurace nastavení SQL Serveru klikněte na **OK**.

## <a name="5-review-hello-summary"></a>5. Zkontrolujte hello souhrn

Na hello **souhrnné** okně zkontrolujte hello shrnutí a klikněte na tlačítko **nákupu** toocreate systému SQL Server, skupinu prostředků a prostředky zadané pro tento virtuální počítač.

Můžete monitorovat nasazení hello z hello portálu Azure. Hello **oznámení** tlačítka v horní části hello hello obrazovky zobrazuje základní stav nasazení hello.

> [!NOTE]
> tooprovide jste udělali představu o nasazení krát, nasadil jsem virtuální počítač SQL oblast východní USA toohello s výchozím nastavením. Toto testovací nasazení trvalo celkem 26 minut toocomplete. Na základě vaší oblasti a vybraného nastavení ale můžete zaznamenat kratší nebo delší čas nasazení.

## <a name="open-hello-vm-with-remote-desktop"></a>Otevřete hello virtuálního počítače pomocí vzdálené plochy

Použijte následující postup tooconnect toohello systému SQL Server virtuálního počítače pomocí vzdálené plochy hello:

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Po připojení virtuálního počítače toohello SQL serverem můžete spustit SQL Server Management Studio a připojit pomocí ověřování systému Windows pomocí přihlašovacích údajů místního správce. Pokud jste povolili ověřování systému SQL Server, můžete také připojit pomocí ověřování SQL pomocí hello SQL přihlašovací jméno a heslo, které jste nakonfigurovali během zřizování.

Počítač toohello přístupu vám umožní toodirectly změnu počítače a nastavení systému SQL Server podle svých požadavků. Můžete například nakonfigurovat nastavení brány firewall hello nebo změnit nastavení konfigurace systému SQL Server.

## <a name="enable-tcpip-for-developer-and-express-editions"></a>Povolení protokolu TCP/IP pro edice Developer a Express

Při zřizování nové virtuální počítač SQL Server, Azure automaticky neumožňuje hello protokol TCP/IP pro vývojáře serveru SQL a edice Express. Hello kroky vysvětlují, jak toomanually povolte protokol TCP/IP, takže se můžete vzdáleně připojit pomocí IP adresy.

Následující postup použijte Hello **SQL Server Configuration Manager** tooenable hello TCP/IP protokol pro vývojáře serveru SQL a edice Express.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-toosql-server-remotely"></a>Vzdálené připojení tooSQL serveru

V tomto kurzu jsme vybrali **veřejné** přístup pro hello virtuálního počítače a **ověřování systému SQL Server**. Tato nastavení automaticky konfigurované hello virtuálního počítače tooallow systému SQL Server připojení z libovolného klienta přes hello internet (za předpokladu, že mají hello správné přihlašovací údaje SQL).

> [!NOTE]
> Pokud jste během zřizování nevybrali veřejný, můžete změnit nastavení připojení k SQL prostřednictvím portálu hello po zřízení. Další informace najdete v tématu popisujícím [změnu nastavení připojení SQL](virtual-machines-windows-sql-connect.md#change).

Následující části Hello ukazují, jak hello tooconnect tooyour instance systému SQL Server na vašem virtuálním počítači z jiného počítače přes internet.

> [!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Další kroky

Další informace o používání systému SQL Server v Azure najdete v tématu [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md) a hello [– nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md).
