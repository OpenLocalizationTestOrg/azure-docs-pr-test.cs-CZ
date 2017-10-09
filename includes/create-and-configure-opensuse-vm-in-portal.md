1. Přihlaste se toohello [portál Azure classic](http://manage.windowsazure.com).  
2. Na panelu příkazů hello v hello dolní části okna hello, klikněte na tlačítko **nový**.
3. V části **výpočetní**, klikněte na tlačítko **virtuálního počítače**a potom klikněte na **z Galerie**.
   
    ![Vytvoření nového virtuálního počítače][Image1]
4. V části hello **SUSE** skupiny, vyberte bitovou kopii virtuálního počítače OpenSUSE a pak klikněte na šipku toocontinue hello.
5. Na hello první **konfigurace virtuálního počítače** stránky:
   
   * Zadejte **název virtuálního počítače**, jako je například "testlinuxvm". Hello název musí obsahovat 3 až 15 znaků, může obsahovat pouze písmena, číslice a pomlčky a musí začínat písmenem a končit písmenem nebo číslicí.
   * Ověřte hello **vrstvy** a vyberte **velikost**. úroveň Hello určuje hello velikostí, které můžete vybrat z. Hello velikost ovlivňuje hello náklady na používání ho i jako možnosti konfigurace, například kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Zadejte **nové uživatelské jméno**, nebo přijměte výchozí hello **azureuser**. Tento název se přidá soubor seznamu Sudoers toohello.
   * Rozhodněte, který typ **ověřování** toouse. Obecné heslo, naleznete na adrese [silná hesla](http://msdn.microsoft.com/library/ms161962.aspx).
6. Na hello Další **konfigurace virtuálního počítače** stránky:
   
   * Použít výchozí hello **vytvořte novou cloudovou službu**.
   * V hello **název DNS** zadejte jedinečné toouse název DNS v rámci hello adresy, jako je například "testlinuxvm".
   * V hello **oblasti nebo vztahů skupiny nebo virtuální síť** vyberte oblast, kde se bude hostovat tuto bitovou kopii virtuálního.
   * V části **koncové body**, zachovat koncový bod SSH hello. Můžete přidat další nyní, nebo přidat, změnit nebo odstranit, je po vytvoření hello virtuálního počítače.
     
     > [!NOTE]
     > Pokud chcete virtuální počítač toouse virtuální síť, můžete **musí** zadejte hello virtuální síť, když vytvoříte hello virtuálního počítače. Tooa virtuální síť virtuálních počítačů nejde přidat po vytvoření virtuálního počítače hello. Další informace najdete v tématu [Přehled virtuálních sítí](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. Na hello poslední **konfigurace virtuálního počítače** stránky, zachovat hello výchozí nastavení a potom klikněte na toofinish zaškrtnutí hello.

Hello portál uvádí hello nového virtuálního počítače v části **virtuální počítače**. Při hello stav je uveden jako **(zřizování)**, se nastavuje hello virtuálního počítače. Pokud stav hello je uveden jako **systémem**, můžete přesunout na další krok toohello.

## <a name="connect-toohello-virtual-machine"></a>Připojit toohello virtuálního počítače
SSH nebo PuTTY tooconnect toohello virtuálního počítače, v závislosti na hello operační systém v počítači hello, budete se připojují z budete používat:

* Z počítače se systémem Linux pomocí SSH. Hello příkazového řádku zadejte:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Zadejte heslo uživatele hello.
* Z počítače se systémem Windows použití klienta PuTTY. Pokud nemáte nainstalováno, ji stáhnout z hello [stránku položek ke stažení PuTTY][PuTTYDownload].
  
    Uložit **putty.exe** tooa adresáře v počítači. Otevřete příkazový řádek, přejděte toothat složky a spusťte **putty.exe**.
  
    Zadejte název hostitele hello, jako je například "testlinuxvm.cloudapp.net" a zadejte "22" pro hello **Port**.
  
    ![PuTTY obrazovky][Image6]  

## <a name="update-hello-virtual-machine-optional"></a>Aktualizace hello virtuálního počítače (volitelné)
1. Poté, co jste si připojené toohello virtuálního počítače, můžete volitelně nainstalovat opravy a aktualizace systému. toorun hello aktualizace, zadejte:
   
    `$ sudo zypper update`
2. Vyberte **softwaru**, pak **Online aktualizace** toolist dostupné aktualizace. Vyberte **přijmout** toostart hello instalace a použít všechny nové dostupných oprav (s výjimkou hello ty, které jsou volitelné).
3. Po dokončení instalace, vyberte **Dokončit**.  Systém je nyní až toodate.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
