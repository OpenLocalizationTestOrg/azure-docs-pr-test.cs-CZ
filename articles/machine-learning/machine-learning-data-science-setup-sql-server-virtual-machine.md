---
title: "Nastavení virtuálního počítače systému SQL Server jako server poznámkového bloku IPython | Microsoft Docs"
description: "Nastavte si Data vědecké účely virtuální počítač s SQL Server a IPython Server."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1fd6014a-d180-4558-b4eb-d9b5a331a99f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: 8a151a6a15d4d000a774e3ec4e38bfa0e58ca33b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Nastavení virtuálního počítače Azure SQL Serveru jako serveru IPython Notebook pro pokročilé analýzy
Toto téma ukazuje, jak zřídit a nakonfigurovat se virtuální počítač systému SQL Server, který se má použít jako součást prostředí cloudové data vědecké účely. Virtuální počítač Windows nakonfigurovaný s Podpora nástrojů, jako je IPython Poznámkový blok, Azure Storage Explorer a AzCopy, jakož i jiné nástroje, které jsou užitečné pro projekty vědecké účely data. Azure Storage Explorer a AzCopy, například poskytují pohodlný způsoby odesílání dat do úložiště objektů blob v Azure z místního počítače nebo se stáhne do místního počítače z úložiště objektů blob.

Galerie virtuálních počítačů Azure obsahuje několik imagí, které obsahují Microsoft SQL Server. Vyberte virtuální počítač s SQL serverem bitovou kopii, který je vhodný pro vaše potřeby data. Doporučené Image jsou:

* SQL Server 2012 SP2 Enterprise pro malé a střední data velikosti
* SQL Server 2012 SP2 Enterprise optimalizované pro úlohy DataWarehousing velikostí a velmi velkých velkých objemů dat
  
  > [!NOTE]
  > Bitové kopie systému SQL Server 2012 SP2 Enterprise **neobsahuje datový disk**. Musíte přidat nebo připojit nejméně jeden virtuální pevné disky ukládat data. Když vytvoříte virtuální počítač Azure, je disk pro operační systém, který je namapovaný na jednotce C a dočasným diskovým namapované na diskovou jednotku d. Nepoužívejte k ukládání dat diskovou jednotku d. Jak již název napovídá, obsahuje pouze dočasné úložiště. Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.
  > 
  > 

## <a name="Provision"></a>Připojte se k portálu Azure Classic a zřízení virtuálního počítače SQL serverem
1. Přihlaste se k [portálu Azure Classic](http://manage.windowsazure.com/) pomocí svého účtu.
   Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
2. Na portálu Azure Classic, v levém dolním webové stránky, klikněte na tlačítko **+ nový**, klikněte na tlačítko **výpočetní**, klikněte na tlačítko **VIRTUÁLNÍHO počítače**a pak klikněte na tlačítko **FROM GALERIE** .
3. Na **vytvoření virtuálního počítače** vyberte bitovou kopii virtuálního počítače obsahující na základě potřeb dat systému SQL Server a pak klikněte na šipku další v pravém dolním rohu stránky. Nejaktuálnější informace o podporovaných imagí SQL serveru v Azure najdete v tématu [Začínáme s SQL serverem v Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) téma v [systému SQL Server v Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentace.
   
   ![Vyberte server SQL Server virtuálního počítače][1]
4. V prvním **konfigurace virtuálního počítače** stránky, zadejte následující informace:
   
   * Zadejte **název VIRTUÁLNÍHO počítače**.
   * V **nové uživatelské jméno** pole, zadejte jedinečné uživatelské jméno pro účet místního Správce virtuálních počítačů.
   * V **nové heslo** zadejte silné heslo. Další informace najdete v tématu [Silná hesla](http://msdn.microsoft.com/library/ms161962.aspx).
   * V **POTVRDIT heslo** pole, zadejte heslo znovu.
   * Vyberte odpovídající **velikost** z rozevíracího seznamu.
     
     > [!NOTE]
     > Velikost virtuálního počítače se zadává během zřizování: A2 je nejmenší velikost doporučuje pro úlohy v produkčním prostředí. Minimální doporučená velikost virtuálního počítače je A3 při použití SQL Server Enterprise Edition. Vyberte A3 nebo vyšší, pokud používáte SQL Server Enterprise Edition. Vyberte A4 při používání SQL Server 2012 nebo 2014 Enterprise optimalizovány pro transakční úlohy bitové kopie.
     > Vyberte A7 při používání SQL Server 2012 nebo 2014 Enterprise optimalizovány pro bitové kopie pracovního vytížení datových skladů. Vybraná velikost omezí počet datových disků, které můžete konfigurovat. Nejaktuálnější informace o dostupných velikostí virtuálních počítačů a počet datových disků, které lze připojit k virtuálnímu počítači najdete v tématu [velikostí virtuálních počítačů pro Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Informace o cenách najdete v části [virtuální ceny Macines](https://azure.microsoft.com/pricing/details/virtual-machines/).
     > 
     > 
   
   Klikněte na šipku další vpravo dole pokračovat.
   
   ![Konfigurace virtuálního počítače][2]
5. Na druhé stránce **konfigurace virtuálního počítače** nakonfigurujte prostředky pro sítě, úložiště a dostupnosti:
   
   * V **Cloudová služba** vyberte **vytvořte novou cloudovou službu**.
   * V **název cloudové služby DNS** zadejte jeho první část názvu DNS podle svého výběru tak, aby jeho dokončení název ve formátu **TESTNAME.cloudapp.net**
   * V **oblasti nebo vztahů skupiny nebo virtuální síť** vyberte oblast, kde se bude hostovat tuto bitovou kopii virtuálního.
   * V **účet úložiště**vyberte stávající účet úložiště nebo automaticky generované jeden.
   * V **sadu dostupnosti** vyberte **(none)**.
   * Přečtěte si a přijměte informace o cenách.
6. V **koncové body** klikněte v rozevírací nabídce prázdný pod **název**a vyberte **MSSQL** zadejte číslo portu instance databázového stroje (**1433** pro výchozí instance).
7. Virtuální počítač s SQL serverem můžou také fungovat jako Server IPython Poznámkový blok, který bude nakonfigurován v pozdější fázi.
   Přidáte nový koncový bod zadejte port pro váš server IPython Poznámkový blok. Zadejte název do pole **název** sloupce, vyberte číslo portu podle svého výběru pro veřejný port a 9999 pro privátní port.
   
   Klikněte na šipku další vpravo dole pokračovat.
   
   ![Vybrat MSSQL a IPython porty][3]
8. Přijměte výchozí nastavení **agenta virtuálního počítače nainstalujte** zaškrtnuto políčko a klikněte na tlačítko zaškrtnutí v pravém dolním rohu průvodce a dokončete proces zajišťování virtuálních počítačů.
   
   `![Poslední možnosti virtuálního počítače][4]
9. Počkejte, až Azure připraví virtuálního počítače. Očekávat stavu virtuálního počítače, chcete-li pokračovat pomocí:
   
   * Spouští se (zřizování)
   * Zastaveno
   * Spouští se (zřizování)
   * Spuštění (zřizování)
   * Běžící (Spuštěno)

## <a name="RemoteDesktop"></a>Otevřete virtuálnímu počítači pomocí vzdálené plochy a dokončete instalaci
1. Po dokončení zřizování, klikněte na název virtuálního počítače přejděte na stránku řídicího PANELU. V dolní části stránky klikněte na tlačítko **Connect**.
2. Zvolte k otevření souboru rpd pomocí programu Vzdálená plocha systému Windows (`%windir%\system32\mstsc.exe`).
3. Na **zabezpečení systému Windows** dialogové okno pole, zadejte heslo pro účet místního správce, který jste zadali v předchozím kroku.
   (Možná se ověřují přihlašovací údaje virtuálního počítače.)
4. Při prvním přihlášení k tomuto virtuálnímu počítači několik procesy pravděpodobně nutné provést, včetně nastavení plochy, aktualizace systému Windows a dokončení úlohy počáteční konfigurace systému Windows (sysprep). Po dokončení nástroje sysprep systému Windows, instalační program systému SQL Server dokončení úlohy konfigurace. Tyto úlohy mohou způsobit zpoždění několik minut, při dokončení. `SELECT @@SERVERNAME`nemusí vracet správný název, až do dokončení instalace systému SQL Server a SQL Server Management Studio nemusí být visable na stránce start.

Jakmile se připojíte k virtuálnímu počítači pomocí vzdálené plochy Windows, virtuální počítač funguje podobně jako ostatní počítače. Připojte k výchozí instanci systému SQL Server s SQL Server Management Studio (spuštěny na virtuálním počítači) běžným způsobem.

## <a name="InstallIPython"></a>Instalace IPython Poznámkový blok a další podpůrné nástroje
Pokud chcete konfigurovat nový virtuální počítač serveru SQL jako server IPython Poznámkový blok, a nainstalovat další podpůrné nástroje takové AzCopy, Azure Storage Explorer, užitečné balíčků Python datové vědy a dalších, je které jste získali speciální přizpůsobení skriptu. K instalaci:

1. Klikněte pravým tlačítkem myši **spustit Windows** ikonu a klikněte na tlačítko **příkazového řádku (správce)**
2. Zkopírujte následující příkazy a vložte na příkazovém řádku.
  
        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
3. Po zobrazení výzvy zadejte heslo podle svého výběru pro server IPython Poznámkový blok.
4. Přizpůsobení skriptu automatizuje několik postupů následné instalace, které zahrnují:
    * Instalace a nastavení serveru IPython Poznámkový blok
    * Otevřít porty protokolu TCP v bráně Windows firewall pro koncové body vytvořili dříve:
    * Pro vzdálené připojení k SQL serveru
    * Pro vzdálené připojení k serveru IPython Poznámkový blok
    * Načítání ukázka IPython poznámkových bloků a skripty SQL
    * Stažení a instalaci užitečné balíčků Python Data vědecké účely
    * Stahování a instalace nástroje Azure, jako je například AzCopy a Azure Storage Explorer  
    <br>
5. Můžete přistupovat a spustit z libovolného prohlížeče místního nebo vzdáleného pomocí adresy URL ve formátu IPython Poznámkový blok `https://<virtual_machine_DNS_name>:<port>`, kde je veřejný port IPython jste vybrali při zřizování virtuálního počítače.
6. Poznámkový blok IPython server je spuštěn jako služba pozadí a bude restartován automaticky, pokud restartování virtuálního počítače.

## <a name="Optional"></a>Připojit datový disk podle potřeby
Pokud vaše image virtuálního počítače neobsahuje datových disků, tedy disky než jednotka C (disk operačního systému) a diskovou jednotku d (dočasný disk), budete muset přidat jeden nebo více datových disků ukládat data. Image virtuálního počítače pro SQL Server 2012 SP2 Enterprise optimalizované pro úlohy DataWarehousing vybavená předem nakonfigurovaným rozhraním dalších disků pro soubory protokolu a data systému SQL Server.

> [!NOTE]
> Nepoužívejte k ukládání dat diskovou jednotku d. Jak již název napovídá, obsahuje pouze dočasné úložiště. Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.
> 
> 

K připojení dalších datových disků, postupujte podle kroků popsaných v [jak připojit datový Disk do virtuálního počítače s Windows](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), které vám pomohou prostřednictvím:

1. Prázdné disky se připojuje k virtuálnímu počítači zřízené v dřívějších krocích
2. Inicializace nové disky ve virtuálním počítači.

## <a name="SSMS"></a>Připojení k SQL Server Management Studio a povolit smíšený režim ověřování
Databázový stroj SQL Serveru nemůže používat ověřování systému Windows bez doménového prostředí. Pokud se chcete připojit k databázovému stroji z jiného počítače, nakonfigurujte SQL Server ověřování ve smíšeném režimu. Smíšený režim ověřování umožňuje ověřování systému SQL Server i ověřování systému Windows. Režim ověřování systému SQL je vyžadováno k načítání dat přímo z vaší databáze virtuální počítač s SQL serverem v [Azure Machine Learning Studio](https://studio.azureml.net) pomocí modulu importovat Data.

1. Při připojení k virtuálnímu počítači pomocí vzdálené plochy, použití Windows **vyhledávání** podokno a typ **SQL Server Management Studio** (SMSS). Kliknutím spustíte SQL Server Management Studio (SSMS). Můžete přidat zástupce do aplikace SSMS na ploše pro budoucí použití.
   
   ![Spuštění aplikace SSMS][5]
   
   Při prvním otevření sady Management Studio se musí vytvořit uživatelské prostředí sady Management Studio. Tato operace může chvíli trvat.
2. Při otevírání, uvede Management Studio **připojit k serveru** dialogové okno. V **název serveru** zadejte název virtuálního počítače pro připojení k databázovému stroji se Průzkumník objektů.
   (Místo názvu virtuálního počítače můžete také použít **(místní)** nebo jedno období jako **název serveru**. Vyberte **ověřování systému Windows**a nechat  ***vaše\_virtuálních počítačů\_název*\\vaše\_místní\_správce**  v **uživatelské jméno** pole. Klikněte na **Připojit**.
   
   ![Připojení k serveru][6]
   
   <br>
   
   > [!TIP]
   > Můžete změnit režim ověřování systému SQL Server pomocí změny klíče registru Windows nebo pomocí SQL Server Management Studio. Chcete-li změnit režim ověřování pomocí změny klíče registru, spusťte **nový dotaz** a spusťte následující skript:
   > 
   > 
   
       USE master
       go
   
       EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
       go

    Chcete-li změnit režim ověřování pomocí serveru SQL Server management Studio:

1. V **Průzkumník objektů systému SQL Server Management Studio**, klikněte pravým tlačítkem na název instance systému SQL Server (název virtuálního počítače) a pak klikněte na tlačítko **vlastnosti**.
   
   ![Vlastnosti serveru][7]
2. Na stránce **Zabezpečení** v části **Ověření serveru** vyberte **SQL Server and Windows Authentication mode** (SQL Server a režim ověřování systému Windows) a potom klikněte na **OK**.
   
   ![Výběr režimu ověřování][8]
3. V **SQL Server Management Studio** dialogové okno, klikněte na tlačítko **OK** potvrdit požadavek na restartování systému SQL Server.
4. V **Průzkumník objektů**, klikněte pravým tlačítkem na váš server a pak klikněte na tlačítko **restartujte**. (Pokud je spuštěný agent systému SQL Server, musí se také restartovat.)
   
   ![Restartování][9]
5. V **SQL Server Management Studio** dialogové okno, klikněte na tlačítko **Ano** vyjádřete, že chcete restartujte systém SQL Server.

## <a name="Logins"></a>Vytvoření přihlášení ověřování serveru SQL
Pokud se chcete připojit k databázovému stroji z jiného počítače, musíte vytvořit nejméně jeden účet ověřování SQL Serveru.  

Můžete vytvořit nové přihlášení serveru SQL Server prostřednictvím kódu programu nebo pomocí SQL Server Management Studio. Chcete-li vytvořit nového uživatele správce systému s ověřování SQL prostřednictvím kódu programu, spusťte **nový dotaz** a spusťte následující skript. Nahraďte < nové uživatelské jméno\> a < nové heslo\> s vaši volbu *uživatelské jméno* a *heslo*. 

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Podle potřeby (ukázkový kód vypne zásady vypršení platnosti kontrolu a heslo), upravte zásady hesel. Další informace o přihlášeních SQL Serveru najdete v tématu věnovaném [vytvoření přihlášení](http://msdn.microsoft.com/library/aa337562.aspx).  

Chcete-li vytvořit nové přihlášení serveru SQL pomocí SQL Server Management Studio:

1. V **Průzkumník objektů systému SQL Server Management Studio**, rozbalte složku instance serveru, ve kterém chcete vytvořit nové přihlašovací údaje.
2. Klikněte pravým tlačítkem myši **zabezpečení** složku, přejděte na příkaz **nový**a vyberte **přihlášení...** .
   
   ![Nové přihlášení][10]
3. V dialogovém okně **Přihlášení – nové** na stránce **Obecné** zadejte jméno nového uživatele do pole **Přihlašovací jméno**.
4. Vyberte **Ověřování SQL Serveru**.
5. Do pole **Heslo** zadejte heslo pro nového uživatele. Do pole **Potvrdit heslo** zadejte toto heslo ještě jednou.
6. K vynucení zásad hesel pro složitost a vynucení, vyberte **vynutit zásady hesel** (doporučeno). Toto je výchozí možnost, pokud je vybrána ověřování serveru SQL Server.
7. K vynucení zásad hesel pro vypršení platnosti, vyberte **vynutit vypršení platnosti hesla** (doporučeno). Vynutit zásady hesel musí být vybrali toto políčko. Toto je výchozí možnost, pokud je vybrána ověřování serveru SQL Server.
8. Chcete-li vynutit uživatelům vytvořit nové heslo po prvním přihlášení se používá, vyberte **musí uživatel změnit heslo při příštím přihlášení** (doporučuje se, pokud toto přihlášení je na použití jiného uživatele. Pokud přihlašovací údaje pro vlastní použití, nevybírejte tuto možnost.) Vynutit vypršení platnosti hesla musí být vybrali toto políčko. Toto je výchozí možnost, pokud je vybrána ověřování serveru SQL Server.
9. V seznamu **Výchozí databáze** vyberte výchozí databázi pro toto přihlášení. Pro tuto možnost je výchozí nastavení **hlavní**. Pokud jste ještě nevytvořili uživatelská databázi, nastavení **hlavní** ponechejte.
10. V **výchozí jazyk** ponechejte **výchozí** jako hodnotu.
    
    ![Vlastnosti přihlášení][11]
11. Pokud toto je první přihlášení, které vytváříte, budete ho pravděpodobně chtít nastavit jako správce SQL Serveru. Pokud tomu tak je, na stránce **Role serveru** zaškrtněte **správce systému**.
    
    > [!IMPORTANT]
    > Členové pevné role správce serveru mají naprostou kontrolu nad databázovým strojem. Z bezpečnostních důvodů důkladným omezením členství v této roli.
    > 
    > 
    
    ![Správce systému][12]
12. Klikněte na tlačítko OK.

## <a name="DNS"></a>Zjistit název DNS virtuálního počítače
Pokud se chcete připojit k databázovému stroji SQL Serveru z jiného počítače, musíte znát název DNS (Domain Name System) virtuálního počítače.

(Je to název, který internet používá k identifikaci virtuálního počítače. Můžete použít IP adres, ale IP adresa se může změnit, když Azure přesune prostředky kvůli redundanci nebo údržbě. Název DNS bude stabilní, protože je možné ho přesměrovat na novou IP adresu.)

1. Na portálu Azure Classic (nebo z předchozího kroku), vyberte **virtuální počítače**.
2. Na **INSTANCÍ VIRTUÁLNÍHO počítače** stránky v **název DNS** sloupce, najít a kopírování název DNS pro virtuální počítač, který se zobrazí před sebou **http://**. (Uživatelské rozhraní se nemusí zobrazit celý název, ale můžete klikněte na něj pravým tlačítkem myši a vyberte Kopírovat.)

## <a name="cde"></a>Připojit k databázovému stroji z jiného počítače
1. Na počítači připojeném k internetu otevřete SQL Server Management Studio.
2. V **připojit k serveru** nebo **připojit k databázovém stroji** dialogu **název serveru** zadejte název DNS virtuálního počítače (určený v předchozí úloze) a číslo portu veřejný koncový bod ve formátu *DNSName, ČísloPortu* například **tutorialtestVM.cloudapp.net,57500**.
3. V poli **Ověřování** vyberte **Ověřování serveru SQL Server**.
4. Do pole **Přihlášení** zadejte název přihlášení, které jste vytvořili v předchozím úkolu.
5. Do pole **Heslo** zadejte heslo přihlášení, které jste vytvořili v předchozím úkolu.
6. Klikněte na **Připojit**.

## <a name="amlconnect"></a>Připojit k databázovému stroji z Azure Machine Learning
V novější fázích procesu Team dat vědecké účely, budete používat [Azure Machine Learning Studio](https://studio.azureml.net) k vytváření a nasazování modelů machine learning. K načítání dat z databáze virtuální počítač SQL Server přímo do Azure Machine Learning pro školení nebo vyhodnocování, použijte **importovat Data** modulu v nové [Azure Machine Learning Studio](https://studio.azureml.net) experiment. Toto téma je zahrnuté v další podrobnosti prostřednictvím odkazů Průvodce proces vědecké účely dat týmu. Úvod najdete v části [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

1. V **vlastnosti** podokně [importovat Data modulu](https://msdn.microsoft.com/library/azure/dn905997.aspx), vyberte **Azure SQL Database** z **zdroj dat** rozevíracího seznamu.
2. V **název databázového serveru** textové pole, zadejte`tcp:<DNS name of your virtual machine>,1433`
3. Zadejte uživatelské jméno SQL v **název uživatelského účtu serveru** textové pole.
4. Zadejte heslo uživatele sql **heslo uživatelského účtu serveru** textové pole.
   
   ![Azure Machine Learning Import dat][13]

## <a name="shutdown"></a>Vypnutí a zrušit přidělení virtuálního počítače, když není používán
Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**. Aby se zajistilo, že nejsou se fakturuje Pokud nepoužíváte virtuální počítač, musí být v **zastaveno (Deallocated)** stavu.

> [!NOTE]
> Vypínání virtuálního počítače z uvnitř (s použitím možnosti napájení systému Windows), se zastaví virtuální počítač ale přidělené zůstanou. Aby se zajistilo, nejsou se fakturuje, vždy zastaví virtuální počítače z [portálu Azure Classic](http://manage.windowsazure.com/). Virtuální počítač taky můžete zastavit přes Powershell voláním příkazu ShutdownRoleOperation s parametrem „PostShutdownAction“ nastaveným na „StoppedDeallocated“.
> 
> 

Vypnutí a zrušit přidělení virtuálního počítače:

1. Přihlaste se k [portálu Azure Classic](http://manage.windowsazure.com/) pomocí svého účtu.  
2. Vyberte **virtuální počítače** levém navigačním panelu.
3. V seznamu virtuálních počítačů, klikněte na název virtuálního počítače potom přejděte na stránku **řídicí panel** stránky.
4. V dolní části stránky klikněte na tlačítko **vypnutí**.

![Vypnutí virtuálního počítače][15]

Virtuální počítač bude zrušeno, ale nebyl odstraněn. Kdykoli z portálu Azure Classic můžete restartovat virtuální počítač.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Virtuální počítač Azure SQL Server je připravený k použití: co je další?
Virtuální počítač je nyní připraven k použití v cvičení vědecké účely vaše data. Virtuální počítač je taky připravené pro použití jako server IPython Poznámkový blok pro zkoumání a zpracování dat a dalších úloh ve spojení s Azure Machine Learning a proces pro vědecké účely Data Team (TDSP).

Další kroky v procesu vědecké účely data jsou namapované v [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, proces a ukázkové ho existuje v rámci přípravy learning z dat pomocí Azure Machine Learning .

[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png

