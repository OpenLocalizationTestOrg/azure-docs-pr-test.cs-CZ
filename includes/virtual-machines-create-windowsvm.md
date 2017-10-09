1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Spuštění v levé horní části hello, klikněte na tlačítko **nový > výpočetní > Windows Server 2016 Datacenter**.

    ![Přejděte Image virtuálního počítače Azure toohello hello portálu](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. Na Windows Server 2016 Datacenter hello vyberte model nasazení Classic hello. Klikněte na Vytvořit.

    ![Snímek obrazovky zobrazující Image virtuálního počítače Azure hello v hello portálu k dispozici](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Okno Základy

okno základy Hello požadavků pro správu informace o virtuálním počítači hello.

1. Zadejte **název** hello virtuálního počítače. V příkladu hello _HeroVM_ je název hello hello virtuálního počítače. Hello název musí mít 1 až 15 znaků a nesmí obsahovat speciální znaky.

2. Zadejte **uživatelské jméno** a silné **heslo** , které jsou používané toocreate místní účet hello virtuálních počítačů. Hello místní účet se používá toosign v tooand spravovat hello virtuálních počítačů. V příkladu hello _azureuser_ je hello uživatelské jméno.

 Hello heslo musí mít 8-123 znaků a musí splňovat tři mimo hello čtyři následující požadavky na složitost: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak. Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../articles/virtual-machines/windows/faq.md).

3. Hello **předplatné** je volitelný. Jedním z běžných nastavení jsou průběžné platby.

4. Vyberte existující **skupiny prostředků** nebo název typu hello nové. V příkladu hello _HeroVMRG_ je hello název skupiny prostředků hello.

5. Vyberte datové centrum Azure **umístění** místo toorun hello virtuálních počítačů. V příkladu hello **východní USA** hello umístění.

6. Až budete hotovi, klikněte na tlačítko **Další** toocontinue toohello další okno.

    ![Snímek obrazovky, který zobrazuje nastavení hello na hello okno základy pro konfiguraci virtuálního počítače Azure](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Okno Velikost

Velikost okna Hello identifikuje podrobnosti konfigurace hello hello virtuálních počítačů a uvádí různé volby, které zahrnují operačního systému, počet procesorů, typ disku úložiště a odhadované měsíční náklady na využití.  

Zvolte velikost virtuálního počítače a pak klikněte na tlačítko **vyberte** toocontinue. V tomto příkladu _DS1_\__V2 standardní_ je hello velikost virtuálního počítače.

  ![Snímek obrazovky okna velikost hello, který ukazuje hello velikosti virtuálního počítače Azure, které můžete vybrat](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Okno Nastavení

okno nastavení Hello požadavků možností úložiště a síť. Můžete přijmout výchozí nastavení hello. Azure v případě potřeby vytvoří odpovídající položky.

Pokud jste vybrali velikost virtuálního počítače, která to podporuje, a jako typ disku vyberete Premium (SSD), můžete vyzkoušet službu Azure Storage úrovně Premium.

Jakmile budete se změnami hotovi, klikněte na **OK**.

## <a name="4-summary-blade"></a>4. Okno Souhrn

Souhrn okno Hello uvádí hello nastavení zadané v předchozích oken hello. Klikněte na tlačítko **OK** až budete připravené toomake hello image.

 ![Sestava souhrnu okna poskytnutí zadaná nastavení hello virtuálního počítače](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

Po vytvoření virtuálního počítače hello hello portál uvádí hello nového virtuálního počítače v části **všechny prostředky**a zobrazí dlaždice hello virtuálního počítače na řídicím panelu hello. Hello odpovídající cloudové služby a účet úložiště se taky vytvořit a uvedené. Automaticky spustit hello virtuálního počítače a cloudové služby a jejich stav je uveden jako **systémem**.

 ![Konfigurace agenta virtuálního počítače a hello koncových bodů hello virtuálního počítače](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
