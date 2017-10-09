Virtuální počítače v Azure můžete vytvořit pomocí Průzkumníka serveru v sadě Visual Studio.

## <a name="create-an-azure-virtual-machine-in-server-explorer"></a>Vytvoření virtuálního počítače Azure v Průzkumníku serveru
Když vytvoříte virtuální počítač v hello [portálu pro správu Azure](http://go.microsoft.com/fwlink/?LinkID=253103), můžete také vytvořit virtuální počítač v Azure pomocí příkazů v Průzkumníku serveru. Virtuální počítače lze použít, například tooprovide popředí koncová za běžných Vyrovnávání zatížení sítě veřejný koncový bod.

### <a name="toocreate-a-new-virtual-machine"></a>toocreate nového virtuálního počítače
1. V Průzkumníku serveru otevřete hello **Azure** uzel a klikněte na tlačítko **virtuální počítače**.
2. V místní nabídce hello, klikněte na tlačítko **vytvořit virtuální počítač**.
   
    Hello **vytvořit nový virtuální počítač** zobrazí se průvodce.
   
    ![Hello příkaz Vytvořit virtuální počítač](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718342.png)
3. Na hello **zvolte předplatné** vyberte předplatné toouse při vytváření hello virtuálního počítače a pak klikněte na tlačítko **Další**.
   
    Pokud nejste přihlášení tooAzure, klikněte na tlačítko **přihlásit** toosign v. Potom vyberte předplatné Azure v rozevíracím seznamu hello, pokud ještě nebyla vybrána.
4. Na hello **vyberte bitovou kopii virtuálního počítače** vyberte typ bitové kopie v hello **typem bitové kopie** rozevírací seznam pole a pak vyberte bitové kopie virtuálních počítačů v hello **název bitové kopie** pole se seznamem. Když jste hotovi, klikněte na tlačítko **Další**.
   
    ![Vybrat stránku bitové kopie virtuálního počítače](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744137.png)
   
    Můžete použít následující typy obrázků hello.
   
   * **Veřejné obrázky** uvádí bitové kopie virtuálních počítačů, operačních systémů a server software, například Windows Server a SQL Server.
   * **Imagím MSDN** uvádí bitové kopie virtuálních počítačů k dispozici tooMSDN odběratelů softwaru, jako je například Visual Studio a Microsoft Dynamics.
   * **Obrázky privátní** seznamy specializuje a zobecněn bitové kopie virtuálních počítačů, které jste vytvořili.
     
     toolearn o specializuje a zobecněný virtuální počítače, najdete v tématu [Image virtuálního počítače](https://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/). V tématu [jak tooCapture tooUse virtuálního počítače Windows jako šablona](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) informace o tom, jak vytvořit nový virtuální počítač do šablony, které můžete použít tooquickly tooturn předem nakonfigurované virtuální počítače.
     
     Klikněte na tlačítko a virtuální počítač image název toosee informace o bitové kopie hello na pravé straně hello hello stránky.
     
     > [!NOTE]
     > Nelze přidat toohello bitové kopie virtuálního počítače **veřejné obrázky** nebo **Imagím MSDN** uvádí, protože jsou jen pro čtení. Všechny virtuální počítače, které vytvoříte, se přidají toohello **privátní image** seznamu.
     > 
     > 
     
     Pokud jste webu MSDN odběratele se Visual Studio na úrovni předplatného, můžete vytvořit předem připravené Azure virtuální počítač, který obsahuje sady Visual Studio, jakož i několik dalších bitové kopie. Další informace najdete v tématu [vytvoření virtuálního počítače v sadě Visual Studio pomocí bitové kopie Visual Studio 2013 Galerie obrázek pro předplatitele MSDN](http://visualstudio2013msdngalleryimage.azurewebsites.net) a [předplatné MSDN](https://www.visualstudio.com/products/msdn-subscriptions-vs). |
5. Na hello **základní nastavení virtuálního počítače** stránky, zadejte název počítače a poté přidejte hello specifikace hello virtuálního počítače, včetně velikosti hello a uživatelské jméno a heslo. Když jste hotovi, klikněte na tlačítko **Další**.
   
    Budete používat hello nový název a heslo toolog do hello počítače pomocí vzdálené plochy, takže je vhodné toowrite, je dolů v případě, že jste zapomněli. Po vytvoření virtuálního počítače Azure v sadě Visual Studio, můžete změnit jeho velikost a další nastavení v hello [portálu pro správu Azure](http://go.microsoft.com/fwlink/?LinkID=253103).
   
   > [!NOTE]
   > Pokud si zvolíte větší velikosti pro hello virtuálního počítače, další poplatky. V tématu [podrobnosti o cenách virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/) Další informace.
   > 
   > 
6. Virtuální počítače vytvořené v sadě Visual Studio vyžadují cloudové služby. Na hello **nastavení služby Cloud** vyberte cloudovou službu pro hello virtuálního počítače nebo klikněte na tlačítko **< vytvořit novou... >** v hello rozevírací seznam, pokud ještě nemáte cloudové služby nebo chcete toouse nový jeden. Účet úložiště je také nutný, takže zvolte účet úložiště (nebo vytvořit nový účet úložiště) v hello **účet úložiště** rozevíracím seznamu. V tématu [tooMicrosoft Úvod Azure Storage](../articles/storage/common/storage-introduction.md) Další informace.
7. Pokud chcete toospecify virtuální sítě (což je volitelný), vyberte ho v hello virtuální síť a podsíť rozevírací seznamy.
   
    Virtuální počítače, které jsou členy skupiny dostupnosti jsou nasazené toodifferent domén selhání. V tématu [Azure Virtual Network](https://azure.microsoft.com/services/virtual-network/) Další informace.
8. Pokud chcete, aby váš virtuální počítač toobelong tooan skupinu dostupnosti (volitelné), vyberte hello **zadejte nastavení dostupnosti** zaškrtněte políčko a potom vyberte v rozevíracím seznamu hello sadu dostupnosti. Až budete hotoví, zvolte hello **Další** tlačítko.
   
    Přidání vaší tooan virtuálního počítače sady dostupnosti. pomáhá aplikace zůstanou k dispozici při selhání sítě, selhání hardwaru místního disku a žádné plánované výpadky. Je třeba toouse hello [portálu pro správu Azure](http://go.microsoft.com/fwlink/?LinkID=253103) nastaví toocreate virtuální sítě, podsítě a dostupnost. V tématu [hello Správa dostupnosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/manage-availability-virtual-machines/) Další informace.
9. Na hello **koncové body** zadejte hello veřejné koncové body, které chcete k dispozici toousers virtuálního počítače. Například můžete zvolit tooenable HTTP (Port 80) kromě toohello vzdálené plochy a prostředí PowerShell koncové body, které jsou ve výchozím nastavení povolené. tooadd na koncový bod, vyberte jednu v hello **Name portu** rozevírací seznam pole a zvolte hello **přidat** tlačítko. tooremove na koncový bod, vyberte hello red **X** další toohello název v seznamu koncových bodů hello.
   
    ![stránka Hello koncové body v Průvodci hello virtuálních počítačů.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718351.png)
   
    Hello koncové body, které jsou k dispozici závisí na hello cloudové služby, které jste vybrali pro virtuální počítač. V tématu [koncové body služby Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) Další informace.
   
   > [!NOTE]
   > Povolení veřejné koncové body služby vytváří na váš virtuální počítač k dispozici toohello Internetu. Být jisti tooinstall a správně nakonfigurujte hello koncových bodů a služby ve vašem virtuálním počítači, například seznamy nastavení řízení přístupu (ACL) pro koncové body hello. V tématu [jak tooSet až koncové body tooa virtuální počítač](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) Další informace.
   > 
   > 
10. Po dokončení konfigurace nastavení virtuálního počítače hello, zvolte hello **vytvořit** tlačítko toocreate hello virtuálního počítače.
    
     Jak Azure vytvoří hello virtuální počítač, hello **protokol činnosti Azure** hello zobrazuje průběh operace vytvoření virtuálního počítače hello.
    
     ![Virtuální počítač protokol aktivit - probíhá.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744138.png)
    
     informace o virtuální počítač tooview, zvolte hello **virtuální počítače** ve hello **protokol činnosti Azure**.
    
     ![Protokol aktivit virtuálního počítače – byla dokončena.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744139.png)
    
     Po úspěšném dokončení operace hello hello nový virtuální počítač se zobrazí pod hello **virtuální počítače** uzlu v Průzkumníku serveru. Může přihlásit k jeho kliknutím hello **připojit pomocí vzdálené plochy** zástupce.
    
     ![Virtuální počítač, které jsou uvedeny v Průzkumníku serveru.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744140.png)

## <a name="manage-your-virtual-machines"></a>Správa virtuálních počítačů
Na stránce konfigurace virtuálního počítače hello kromě tooshutting dolů, připojení, aktualizace a přidání kontrolních bodů toohello vybraný virtuální počítač, můžete také zobrazit nebo změnit nastavení pro virtuální počítač hello. Můžete:

* Změnit velikost virtuálního počítače hello.
* Vyberte hello dostupnosti nastavit toouse s hello virtuálního počítače.
* Přidat, odebrat nebo změnit nastavení pro veřejné koncové body.
* Přidat, odebrat nebo nakonfigurovat rozšíření virtuálního počítače.
* Zobrazení informací o hello disků asociovaných s hello virtuálního počítače.

### <a name="view-or-change-virtual-machine-settings"></a>Zobrazit nebo změnit nastavení virtuálního počítače
1. V Průzkumníku serveru, vyberte virtuální počítač v hello **virtuální počítače Azure** uzlu.
2. V místní nabídce hello, zvolte **konfigurace** stránku konfigurace virtuálního počítače tooview hello.
   
    ![Stránka Konfigurace virtuálního počítače Azure Hello](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744141.png)
3. Zobrazit informace o virtuálním počítači hello nebo ho změnit.

### <a name="save-or-restore-hello-status-of-your-virtual-machine"></a>Uložení nebo obnovení hello stav virtuálního počítače
Jak nakonfigurovat virtuální počítač a na něj nainstalovat software, je vhodné tooregularly uložit průběh vytvořením kontrolní body virtuálních počítačů. Kontrolní bod je snímek, nebo obrázek hello aktuální stav virtuálního počítače. Pokud se něco pokazí hello virtuálnímu počítači, nebo chcete tooreconfigure hello virtuálního počítače, můžete ušetřit čas jeho obnovením tooa předchozího kontrolního bodu stavu místo přes od začátku.

### <a name="toocreate-a-virtual-machine-checkpoint"></a>toocreate kontrolní bod virtuálního počítače
1. V Průzkumníku serveru, vyberte virtuální počítač v hello **virtuální počítače Azure** uzlu.
2. V místní nabídce hello, zvolte **konfigurace** stránku konfigurace virtuálního počítače tooview hello.
3. Na stránce konfigurace hello zvolte hello **zachytit bitovou kopii** tlačítko.
   
    ![Tlačítko zachycení stránku konfigurace služby Azure](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744142.png)
   
    Hello **zachytit virtuální počítač** otevře se dialogové okno.
   
    ![Dialogové okno Azure zachycení virtuálního počítače](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744143.png)
4. Zadejte popisek bitové kopie a popis. Popis a výchozí štítek jsou k dispozici, ale můžete jej přepsat vlastními Pokud chcete.
5. Pokud jste již spustili nástroj Sysprep na tomto virtuálním počítači, vyberte hello **na hello virtuálního počítače mají spustit program Sysprep** pole.
   
    Nástroj Sysprep je nástroj, který mimo jiné odebere data specifická pro systémy z virtuálního počítače hello verze systému Windows, což šablonu, kterou můžete použít jiné. V tématu [jak tooCapture tooUse virtuálního počítače Windows jako šablona](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) Další informace. Zálohujte hello virtuálních počítačů před spuštěním nástroje Sysprep.
6. Po dokončení konfigurace nastavení zachycení hello, zvolte hello **zaznamenat** tlačítko toocreate hello kontrolního bodu.
   
    Jak Azure vytvoří kontrolní bod hello, hello **protokol činnosti Azure** hello zobrazuje průběh operace hello.
   
    ![Zaznamenání kontrolní bod virtuálního počítače](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744144.png)
   
    Po dokončení operace hello kontrolního bodu se zobrazí v hello **protokol činnosti Azure**.
   
    ![Kontrolní bod operace byla dokončena](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744145.png)

## <a name="toomanage-virtual-machine-checkpoints"></a>toomanage kontrolní body virtuálních počítačů
### <a name="toorestore-a-virtual-machine-tooa-previously-saved-state"></a>dříve toorestore tooa virtuální počítač v uloženém stavu
* Postupujte podle kroků hello uvedených v [krok za krokem: provedení cloudu obnoví Microsoft virtuálních počítačů Azure pomocí prostředí PowerShell - část 2](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx).

### <a name="toodelete-a-checkpoint"></a>toodelete kontrolní bod
1. Přejděte toohello [portálu pro správu Azure](http://go.microsoft.com/fwlink/?LinkID=253103).
2. Na stránce konfigurace hello virtuálního počítače, zvolte hello **bitové kopie** karty v horní části hello hello stránky.
3. Vyberte kontrolní bod hello má toodelete a potom vyberte hello **odstranit** tlačítko v hello dolní části stránky hello.

## <a name="shut-down-your-virtual-machine"></a>Vypnout virtuální počítač
1. V Průzkumníku serveru, vyberte virtuální počítač hello chcete tooshut dolů v hello **virtuální počítače Azure** uzlu.
2. V místní nabídce hello, vybrat, zda text hello **vypnutí** příkazu, nebo zvolte **konfigurace** tooview hello stránku konfigurace virtuálního počítače a potom zvolte hello **vypnutí**tlačítko.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o vytváření virtuálních počítačů, najdete v části [vytvořit virtuálním počítači systémem Linux](../articles/virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [vytvoření virtuálního počítače se systémem Windows na portálu Azure preview hello](../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

