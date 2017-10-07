---
title: "aaaSet virtuální počítač systému SQL Server jako server poznámkového bloku IPython | Microsoft Docs"
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
ms.openlocfilehash: ee83d1d5de671d9817c1bc1abd6b4f9c256dde8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Nastavení virtuálního počítače Azure SQL Serveru jako serveru IPython Notebook pro pokročilé analýzy
Toto téma ukazuje, jak tooprovision a nakonfigurujte toobe virtuálního počítače serveru SQL Server používá jako součást prostředí cloudové data vědecké účely. virtuální počítač Windows Hello nastavena podpora nástrojů, jako je IPython Poznámkový blok, Azure Storage Explorer a AzCopy, jakož i jiné nástroje, které jsou užitečné pro projekty vědecké účely data. Azure Storage Explorer a AzCopy, například poskytují pohodlný způsoby úložiště objektů blob tooAzure tooupload data z místního počítače nebo toodownload ho tooyour místního počítače z úložiště objektů blob.

virtuální počítač Azure Galerie Hello obsahuje několik imagí, které obsahují Microsoft SQL Server. Vyberte virtuální počítač s SQL serverem bitovou kopii, který je vhodný pro vaše potřeby data. Doporučené Image jsou:

* SQL Server 2012 SP2 Enterprise pro malé toomedium velikosti dat
* SQL Server 2012 SP2 Enterprise optimalizované pro úlohy DataWarehousing velikostí velkých toovery velkých objemů dat
  
  > [!NOTE]
  > Bitové kopie systému SQL Server 2012 SP2 Enterprise **neobsahuje datový disk**. Bude nutné tooadd nebo připojit jednu nebo více virtuálních pevných disků toostore vaše data. Když vytvoříte virtuální počítač Azure, má disku pro jednotku toohello C operačního systému namapované hello a dočasným diskovým mapovat jednotku toohello D. Nepoužívejte hello D jednotky toostore data. Jak hello název napovídá, obsahuje pouze dočasné úložiště. Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.
  > 
  > 

## <a name="Provision"></a>Připojení toohello portálu Azure Classic a zřízení virtuálního počítače SQL serverem
1. Přihlaste se toohello [portálu Azure Classic](http://manage.windowsazure.com/) pomocí svého účtu.
   Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
2. Na portálu Azure Classic hello v levé dolní hello hello webové stránky, klikněte na tlačítko **+ nový**, klikněte na tlačítko **výpočetní**, klikněte na tlačítko **VIRTUÁLNÍHO počítače**a pak klikněte na tlačítko **FROM GALERIE**.
3. Na hello **vytvoření virtuálního počítače** vyberte bitovou kopii virtuálního počítače obsahující na základě potřeb dat systému SQL Server a potom klikněte na šipku další hello v pravém dolním rohu stránky hello. Nejaktuálnější informace o hello na hello podporovány bitové kopie systému SQL Server na platformě Azure, najdete v části [Začínáme s SQL serverem v Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) téma v hello [systému SQL Server v Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentace.
   
   ![Vyberte server SQL Server virtuálního počítače][1]
4. Na hello první **konfigurace virtuálního počítače** stránky, zadejte následující informace:
   
   * Zadejte **název VIRTUÁLNÍHO počítače**.
   * V hello **nové uživatelské jméno** pole, zadejte jedinečné uživatelské jméno pro hello účet místního Správce virtuálních počítačů.
   * V hello **nové heslo** zadejte silné heslo. Další informace najdete v tématu [Silná hesla](http://msdn.microsoft.com/library/ms161962.aspx).
   * V hello **POTVRDIT heslo** pole, zadejte znovu heslo hello.
   * Vyberte odpovídající hello **velikost** z hello rozevíracím seznamu.
     
     > [!NOTE]
     > velikost Hello hello virtuálního počítače se zadává během zřizování: A2 je nejmenší velikost hello doporučuje pro úlohy v produkčním prostředí. Minimální doporučená velikost virtuálního počítače je A3 při použití SQL Server Enterprise Edition. Vyberte A3 nebo vyšší, pokud používáte SQL Server Enterprise Edition. Vyberte A4 při používání SQL Server 2012 nebo 2014 Enterprise optimalizovány pro transakční úlohy bitové kopie.
     > Vyberte A7 při používání SQL Server 2012 nebo 2014 Enterprise optimalizovány pro bitové kopie pracovního vytížení datových skladů. vybraná velikost Hello omezí počet datových disků, které můžete konfigurovat. Nejaktuálnější informace o dostupných velikostí virtuálních počítačů a hello počet datových disků, je možné připojit virtuální počítač tooa najdete v tématu [velikostí virtuálních počítačů pro Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Informace o cenách najdete v části [virtuální ceny Macines](https://azure.microsoft.com/pricing/details/virtual-machines/).
     > 
     > 
   
   Klikněte na šipku další hello na toocontinue pravé dolní hello.
   
   ![Konfigurace virtuálního počítače][2]
5. Na hello druhý **konfigurace virtuálního počítače** nakonfigurujte prostředky pro sítě, úložiště a dostupnosti:
   
   * V hello **Cloudová služba** vyberte **vytvořte novou cloudovou službu**.
   * V hello **název cloudové služby DNS** zadejte hello první část názvu DNS podle svého výběru tak, aby jeho dokončení název ve formátu **TESTNAME.cloudapp.net**
   * V hello **oblasti nebo vztahů skupiny nebo virtuální síť** vyberte oblast, kde se bude hostovat tuto bitovou kopii virtuálního.
   * V hello **účet úložiště**vyberte stávající účet úložiště nebo automaticky generované jeden.
   * V hello **sadu dostupnosti** vyberte **(none)**.
   * Přečtěte si a přijměte hello informace o cenách.
6. V hello **koncové body** klikněte na položku v hello prázdný rozevírací seznam v části **název**a vyberte **MSSQL** zadejte číslo portu hello instance hello databázový stroj (**1433** pro hello výchozí instance).
7. Virtuální počítač s SQL serverem můžou také fungovat jako Server IPython Poznámkový blok, který bude nakonfigurován v pozdější fázi.
   Přidáte nový koncový bod toospecify hello port toouse pro váš server IPython Poznámkový blok. Zadejte název v hello **název** sloupce, vyberte číslo portu podle svého výběru pro hello veřejný port a 9999 pro privátní port hello.
   
   Klikněte na šipku další hello na toocontinue pravé dolní hello.
   
   ![Vybrat MSSQL a IPython porty][3]
8. Přijměte výchozí hello **agenta virtuálního počítače nainstalujte** možnost zkontrolovat a klikněte na tlačítko zaškrtnutí hello hello v hello pravém dolním rohu hello Průvodce toocomplete hello procesu zřizování virtuálních počítačů.
   
   `![Poslední možnosti virtuálního počítače][4]
9. Počkejte, až Azure připraví virtuálního počítače. Očekávat tooproceed stav virtuálního počítače hello prostřednictvím:
   
   * Spouští se (zřizování)
   * Zastaveno
   * Spouští se (zřizování)
   * Spuštění (zřizování)
   * Běžící (Spuštěno)

## <a name="RemoteDesktop"></a>Otevřete hello virtuálního počítače pomocí vzdálené plochy a dokončete instalaci
1. Po dokončení zřizování klikněte na název hello stránky řídicí panel toohello toogo virtuálního počítače. V dolní části hello hello stránky, klikněte na tlačítko **Connect**.
2. Vyberte soubor rpd hello tooopen pomocí programu Vzdálená plocha Windows hello (`%windir%\system32\mstsc.exe`).
3. V hello **zabezpečení systému Windows** dialogovém okně zadejte hello heslo pro účet místního správce, který jste zadali v předchozím kroku.
   (Možná tooverify hello pověření hello virtuálního počítače.)
4. Hello při prvním přihlášení toothis virtuálního počítače, několik procesů může být nutné toocomplete, včetně nastavení plochy, aktualizace systému Windows a dokončení úlohy počáteční konfigurace Windows hello (sysprep). Po dokončení nástroje sysprep systému Windows, instalační program systému SQL Server dokončení úlohy konfigurace. Tyto úlohy mohou způsobit zpoždění několik minut, při dokončení. `SELECT @@SERVERNAME`nemusí vracet hello správný název, až do dokončení instalace systému SQL Server a SQL Server Management Studio nemusí být visable na úvodní stránku hello.

Až se připojené toohello virtuálního počítače pomocí vzdálené plochy Windows hello virtuální počítač funguje podobně jako ostatní počítače. Připojit toohello výchozí instance systému SQL Server s SQL Server Management Studio (spuštěny na virtuálním počítači hello) v hello obvyklým způsobem.

## <a name="InstallIPython"></a>Instalace IPython Poznámkový blok a další podpůrné nástroje
tooconfigure takové AzCopy, Azure Storage Explorer, užitečné balíčků Python datové vědy a dalších nástrojů pro vaše nové tooserve virtuální počítač SQL Server jako server IPython Poznámkový blok a další podpůrné instalace, speciální přizpůsobení skript je poskytován tooyou. tooinstall:

1. Klikněte pravým tlačítkem na hello **spustit Windows** ikonu a klikněte na tlačítko **příkazového řádku (správce)**
2. Zkopírujte hello následující příkazy a vložte hello příkazového řádku.
  
        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
3. Po zobrazení výzvy zadejte heslo podle svého výběru pro hello serveru IPython Poznámkový blok.
4. přizpůsobení skriptu Hello automatizuje několik postupů následné instalace, které zahrnují:
    * Instalace a nastavení serveru IPython Poznámkový blok
    * Otevřít porty TCP v bráně firewall systému Windows hello pro koncové body hello vytvořili dříve:
    * Pro vzdálené připojení k SQL serveru
    * Pro vzdálené připojení k serveru IPython Poznámkový blok
    * Načítání ukázka IPython poznámkových bloků a skripty SQL
    * Stažení a instalaci užitečné balíčků Python Data vědecké účely
    * Stahování a instalace nástroje Azure, jako je například AzCopy a Azure Storage Explorer  
    <br>
5. Můžete přistupovat a spustit z libovolného prohlížeče místního nebo vzdáleného pomocí adresy URL ve formátu hello IPython Poznámkový blok `https://<virtual_machine_DNS_name>:<port>`, kde je veřejný port, hello IPython jste vybrali při zřizování hello virtuálního počítače.
6. IPython Poznámkový blok server je spuštěn jako služba na pozadí a bude restartován automaticky při dalším spuštění hello virtuálního počítače.

## <a name="Optional"></a>Připojit datový disk podle potřeby
Pokud vaše image virtuálního počítače neobsahuje datových disků, tedy disky než jednotka C (disk operačního systému) a diskovou jednotku d (dočasný disk), je třeba tooadd jeden nebo více dat disky toostore vaše data. Hello image virtuálního počítače pro SQL Server 2012 SP2 Enterprise optimalizované pro úlohy DataWarehousing vybavená předem nakonfigurovaným rozhraním dalších disků pro soubory protokolu a data systému SQL Server.

> [!NOTE]
> Nepoužívejte hello D jednotky toostore data. Jak hello název napovídá, obsahuje pouze dočasné úložiště. Vzhledem k tomu, že není uložena v úložišti Azure nabízí žádné redundance nebo zálohování.
> 
> 

tooattach dalších datových disků, postupujte podle kroků hello popsaných v [jak tooAttach tooa datový Disk virtuálního počítače Windows](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), které vám pomohou prostřednictvím:

1. Připojení prázdné disky toohello virtuálního počítače zřízené v dřívějších krocích
2. Inicializace hello nové disky ve virtuálním počítači hello

## <a name="SSMS"></a>Připojení tooSQL Server Management Studio a povolit smíšený režim ověřování
Hello databázového stroje SQL Server nelze použít ověřování systému Windows bez prostředí domény. tooconnect toohello databázový stroj z jiného počítače, nakonfigurujte pro smíšený režim ověřování systému SQL Server. Smíšený režim ověřování umožňuje ověřování systému SQL Server i ověřování systému Windows. Režim ověřování systému SQL je nutné v pořadí tooingest data přímo z vaší databáze virtuální počítač s SQL serverem v [Azure Machine Learning Studio](https://studio.azureml.net) pomocí modulu hello importovat Data.

1. Při připojené toohello virtuálního počítače pomocí vzdálené plochy, používat hello Windows **vyhledávání** podokno a typ **SQL Server Management Studio** (SMSS). Klikněte na tlačítko toostart hello SQL Server Management Studio (SSMS). Můžete tooadd tooSSMS zástupce na ploše pro budoucí použití.
   
   ![Spuštění aplikace SSMS][5]
   
   Při prvním spuštění Management Studio musí vytvořit prostředí Management Studio uživatelé hello Hello. Tato operace může chvíli trvat.
2. Při otevírání, Management Studio uvede hello **připojit tooServer** dialogové okno. V hello **název serveru** pole, název typu hello hello virtuálního počítače tooconnect toohello databázový stroj s hello Průzkumník objektů.
   (Místo hello název virtuálního počítače můžete také použít **(místní)** nebo jedno období jako hello **název serveru**. Vyberte **ověřování systému Windows**a nechat ** *vaše\_virtuálních počítačů\_název*\\vaše\_místní\_správce ** v hello **uživatelské jméno** pole. Klikněte na **Připojit**.
   
   ![Připojit tooServer][6]
   
   <br>
   
   > [!TIP]
   > Můžete změnit režim ověřování systému SQL Server hello pomocí změny klíče registru Windows nebo pomocí hello SQL Server Management Studio. režim ověřování toochange pomocí hello změny klíče registru, spuštění **nový dotaz** a spusťte následující skript hello:
   > 
   > 
   
       USE master
       go
   
       EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
       go

    režim ověřování hello toochange pomocí systému SQL Server management Studio:

1. V **Průzkumník objektů systému SQL Server Management Studio**, klikněte pravým tlačítkem na název hello instance systému SQL Server (hello název virtuálního počítače) a pak klikněte na tlačítko **vlastnosti**.
   
   ![Vlastnosti serveru][7]
2. Na hello **zabezpečení** v části **ověřování serveru**, vyberte **systému SQL Server a ověřování systému Windows režimu**a potom klikněte na **OK** .
   
   ![Výběr režimu ověřování][8]
3. V hello **SQL Server Management Studio** dialogové okno, klikněte na tlačítko **OK** potvrdit hello požadavek toorestart systému SQL Server.
4. V **Průzkumník objektů**, klikněte pravým tlačítkem na váš server a pak klikněte na tlačítko **restartujte**. (Pokud je spuštěný agent systému SQL Server, musí se také restartovat.)
   
   ![Restartování][9]
5. V hello **SQL Server Management Studio** dialogové okno, klikněte na tlačítko **Ano** vyjádřete, že chcete toorestart systému SQL Server.

## <a name="Logins"></a>Vytvoření přihlášení ověřování serveru SQL
tooconnect toohello databázový stroj z jiného počítače, musíte vytvořit alespoň jeden účet ověřování systému SQL Server.  

Můžete vytvořit nové přihlášení serveru SQL Server prostřednictvím kódu programu, nebo pomocí hello SQL Server Management Studio. toocreate nového sysadmin uživatele pomocí ověřování SQL prostřednictvím kódu programu, spuštění **nový dotaz** a spusťte následující skript hello. Nahraďte < nové uživatelské jméno\> a < nové heslo\> s vaši volbu *uživatelské jméno* a *heslo*. 

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Podle potřeby upravte nastavení zásad pro hesla hello (hello ukázkový kód vypne zásady vypršení platnosti kontrolu a heslo). Další informace o přihlášeních SQL Serveru najdete v tématu věnovaném [vytvoření přihlášení](http://msdn.microsoft.com/library/aa337562.aspx).  

nové přihlášení serveru SQL toocreate pomocí hello SQL Server Management Studio:

1. V **Průzkumník objektů systému SQL Server Management Studio**, rozbalte složku hello instance hello serveru, který chcete toocreate hello nové přihlašovací údaje.
2. Klikněte pravým tlačítkem na hello **zabezpečení** složky, bod příliš**nový**a vyberte **přihlášení... **.
   
   ![Nové přihlášení][10]
3. V hello **přihlášení – nové** dialogové okno, v hello **Obecné** stránky, zadejte název hello hello nového uživatele do hello **přihlašovací jméno** pole.
4. Vyberte **Ověřování SQL Serveru**.
5. V hello **heslo** pole, zadejte heslo pro nového uživatele hello. Znovu zadejte toto heslo do hello **Potvrdit heslo** pole.
6. Vyberte možnosti zásad tooenforce heslo pro složitost a vynucení, **vynutit zásady hesel** (doporučeno). Toto je výchozí možnost, pokud je vybrána ověřování serveru SQL Server.
7. Vyberte možnosti zásad tooenforce heslo pro vypršení platnosti, **vynutit vypršení platnosti hesla** (doporučeno). Vynutit zásady hesel musí být vybraný tooenable toto políčko. Toto je výchozí možnost, pokud je vybrána ověřování serveru SQL Server.
8. Vyberte tooforce hello uživatele toocreate nové heslo po hello nejprve čas přihlášení se používá, **musí uživatel změnit heslo při příštím přihlášení** (doporučuje se, pokud je toto přihlášení pro někdo jiný toouse. Pokud hello přihlášení pro vlastní použití, nevybírejte tuto možnost.) Vynutit vypršení platnosti hesla musí být vybraný tooenable toto políčko. Toto je výchozí možnost, pokud je vybrána ověřování serveru SQL Server.
9. Z hello **výchozí databázi** seznamu, vyberte výchozí databázi pro přihlášení hello. **hlavní** je hello výchozího nastavení pro tuto možnost. Pokud jste ještě nevytvořili uživatelské databáze, nechte toto nastavení příliš**hlavní**.
10. V hello **výchozí jazyk** ponechejte **výchozí** jako hodnota hello.
    
    ![Vlastnosti přihlášení][11]
11. Pokud je to první přihlášení hello, kterou vytváříte, můžete určit toto přihlášení jako správce systému SQL Server. Pokud tomu tak je, na stránce **Role serveru** zaškrtněte **správce systému**.
    
    > [!IMPORTANT]
    > Členové pevné role serveru sysadmin hello mít úplnou kontrolu nad hello databázového stroje. Z bezpečnostních důvodů důkladným omezením členství v této roli.
    > 
    > 
    
    ![Správce systému][12]
12. Klikněte na tlačítko OK.

## <a name="DNS"></a>Určení názvu DNS hello hello virtuálního počítače
tooconnect toohello databázového stroje SQL Server z jiného počítače, musíte znát hello systému DNS (Domain Name) název hello virtuálního počítače.

(Toto je hello název hello Internetu používá tooidentify hello virtuálního počítače. Můžete použít hello IP adresu, ale hello IP adresy mohou změnit, pokud Azure přesune prostředky pro redundanci nebo údržby. název DNS Hello budou stabilní, protože může být přesměrován tooa novou IP adresu.)

1. V portálu Azure Classic hello (nebo z předchozího kroku hello), vyberte **virtuální počítače**.
2. Na hello **INSTANCÍ VIRTUÁLNÍHO počítače** stránku hello **název DNS** sloupce, najít a kopírování hello název DNS pro virtuální počítač hello, která se zobrazí před sebou **http://**. (hello uživatelské rozhraní se nemusí zobrazit hello celý název, ale můžete klikněte na něj pravým tlačítkem myši a vyberte Kopírovat.)

## <a name="cde"></a>Připojit toohello databázový stroj z jiného počítače
1. Na počítači připojené toohello Internetu, otevřete SQL Server Management Studio.
2. V hello **připojit tooServer** nebo **připojit tooDatabase modul** dialogové okno, v hello **název serveru** zadejte název DNS hello virtuálního počítače (určený v hello předchozí úlohy) a číslo portu veřejný koncový bod ve formátu hello *DNSName, ČísloPortu* například **tutorialtestVM.cloudapp.net,57500**.
3. V hello **ověřování** vyberte **ověřování systému SQL Server**.
4. V hello **přihlášení** pole, název typu hello přihlašovací jméno, které jste vytvořili v rámci předchozí úlohy.
5. V hello **heslo** pole, zadejte heslo hello hello přihlášení, který vytvoříte v rámci předchozí úlohy.
6. Klikněte na **Připojit**.

## <a name="amlconnect"></a>Připojení z Azure Machine Learning toohello databázový stroj
V novějších fázích hello proces vědecké účely Team dat, budete používat hello [Azure Machine Learning Studio](https://studio.azureml.net) toobuild a nasazovat modely machine learning. tooingest data z databáze virtuální počítač SQL Server přímo do Azure Machine Learning pro školení nebo vyhodnocování, použijte hello **importovat Data** modulu v nové [Azure Machine Learning Studio](https://studio.azureml.net) experiment. Toto téma je zahrnuté v další podrobnosti prostřednictvím hello proces vědecké účely dat Team příručka odkazy. Úvod najdete v části [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

1. V hello **vlastnosti** podokně hello [importovat Data modulu](https://msdn.microsoft.com/library/azure/dn905997.aspx), vyberte **Azure SQL Database** z hello **zdroj dat** rozevíracího seznamu.
2. V hello **název databázového serveru** textové pole, zadejte`tcp:<DNS name of your virtual machine>,1433`
3. Zadejte uživatelské jméno SQL hello v hello **název uživatelského účtu serveru** textové pole.
4. Zadejte heslo uživatele sql hello v hello **heslo uživatelského účtu serveru** textové pole.
   
   ![Azure Machine Learning Import dat][13]

## <a name="shutdown"></a>Vypnutí a zrušit přidělení virtuálního počítače, když není používán
Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**. tooensure, která nejsou se účtují, pokud nepoužíváte virtuální počítač, má toobe v hello **zastaveno (Deallocated)** stavu.

> [!NOTE]
> Vypínání hello virtuálního počítače z uvnitř (s použitím možnosti napájení systému Windows), hello virtuálního počítače je zastavena, ale přidělené zůstanou. tooensure nejsou fakturuje probíhá, vždy zastavit virtuální počítače z hello [portálu Azure Classic](http://manage.windowsazure.com/). Můžete také zastavit hello virtuálních počítačů pomocí prostředí Powershell voláním ShutdownRoleOperation se rovná "PostShutdownAction" příliš "StoppedDeallocated".
> 
> 

tooshutdown a zrušit přidělení hello virtuálního počítače:

1. Přihlaste se toohello [portálu Azure Classic](http://manage.windowsazure.com/) pomocí svého účtu.  
2. Vyberte **virtuální počítače** z hello levém navigačním panelu.
3. V seznamu hello virtuálních počítačů, klikněte na název virtuálního počítače a přejděte toohello hello **řídicí panel** stránky.
4. V dolní části hello hello stránky, klikněte na tlačítko **vypnutí**.

![Vypnutí virtuálního počítače][15]

Hello virtuálního počítače budou navrácena, ale nebyl odstraněn. Kdykoli hello portálu Azure Classic můžete restartovat virtuální počítač.

## <a name="your-azure-sql-server-vm-is-ready-toouse-whats-next"></a>Virtuální počítač Azure SQL Server je připraven toouse: co je další?
Virtuální počítač je nyní připraven toouse v cvičení vědecké účely vaše data. virtuální počítač Hello je také připravené pro použití jako server služby IPython Poznámkový blok pro zkoumání hello a zpracování dat a další úkoly ve spojení s Azure Machine Learning a hello tým datové vědy procesu (TDSP).

Hello další kroky v procesu vědecké účely hello data jsou v namapované hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, proces a ukázkové ho existuje v rámci přípravy učení se z dat hello s počítači Azure Učení.

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

