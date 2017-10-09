

Když vytvoříte projekt webové aplikace pro Azure, můžete zřídit virtuální počítač v Azure. Potom můžete nakonfigurovat další software hello virtuálního počítače nebo použít hello virtuální počítač pro účely ladění a diagnostiky.

toocreate virtuální počítač při vytvoření webové aplikace, postupujte takto:

1. V sadě Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu** > **webové**a potom vyberte **Webové aplikace ASP.NET** (v části hello **Visual C#** nebo **jazyka Visual Basic** uzly).
2. V hello **nový projekt ASP.NET** dialogové okno, vyberte hello typ webové aplikace a v hello Azure části dialogového okna hello (v pravém dolním rohu, hello), ujistěte se, že hello **hostitel v cloudu hello**je zaškrtnuté políčko (Toto zaškrtávací políčko je označeno **vytvořit vzdálené prostředky** některé instalace).
   
    ![][0]
3. V tomto příkladu v rozevíracím seznamu hello v rámci Microsoft Azure, zvolte **virtuálního počítače (v1)**a potom klikněte na hello **OK** tlačítko.
4. Pokud se zobrazí výzva, přihlaste tooAzure. Hello **vytvořit virtuální počítač** zobrazí se dialogové okno.
   
    ![][2]
5. V hello **název DNS** pole, zadejte název pro virtuální počítač hello. název DNS Hello musí být jedinečný v Azure. Pokud není k dispozici hello jméno, které jste zadali, se zobrazí červený vykřičník.
6. V hello **Image** vyberte hello bitovou kopii toobase hello virtuálního počítače na. Můžete použít standardní virtuální počítač Azure hello bitové kopie nebo bitové kopie, že jste odeslali tooAzure.
7. Nechte hello **povolit IIS a Web Deploy** zaškrtávací políčko Vybrat, pokud máte v plánu tooinstall na jiný webový server. Pokud zakážete, nasazení webu, nebudete se moct toopublish ze sady Visual Studio. Můžete přidat službu IIS a Web Deploy tooany bitových kopií systému Windows Server hello zabalené, včetně vlastních bitových kopií.
8. V hello **velikost** vyberte hello velikost hello virtuálního počítače.
9. Zadejte hello přihlašovací údaje pro tento virtuální počítač. Poznamenejte si z nich, protože je budete potřebovat tooaccess hello počítači pomocí vzdálené plochy.
10. V hello **umístění** vyberte hello oblast toohost hello virtuálního počítače.
11. Klikněte na tlačítko hello **OK** tlačítko toostart vytváření hello virtuálního počítače. Můžete postupovat podle hello průběh operace hello v hello **výstup** okno.
    
    ![][3]
12. Při zřízení hello virtuálního počítače, ve vytvoří publikované skripty **PublishScripts** uzlu ve vašem řešení. Hello publikovala skript se spustí a zřizuje virtuálního počítače v Azure. Hello **výstup** okno zobrazuje stav hello. Hello skript provede následující akce tooset hello virtuálního počítače hello:
    
    * Pokud ještě neexistuje, vytvoří hello virtuální počítač.
    * Vytvoří účet úložiště s názvem, který začíná `devtest`, ale jenom v případě, že v zadané oblasti hello není již účtu úložiště.
    * Vytvoří cloudovou službu jako kontejner pro hello virtuální počítač a vytvoří webové role pro hello webovou aplikaci.
    * Nakonfiguruje Web Deploy na hello virtuálního počítače.
    * Nakonfiguruje službu IIS a ASP.NET hello virtuálního počítače.
    
    ![][4]
13. (Volitelné) Toohello nový virtuální počítač se můžete připojit. V **Průzkumníka serveru**, rozbalte položku hello **virtuální počítače** uzlu, zvolte hello uzel pro virtuální počítač hello jste vytvořili a v jeho místní nabídce a potom vyberte **připojit pomocí vzdálené plochy**. Můžete taky v **Průzkumník cloudu** můžete **otevřít na portálu** hello místní nabídky a připojit toohello virtuálního počítače existuje.
    
    ![][5]

## <a name="next-steps"></a>Další kroky
Pokud chcete toocustomize hello publikované skripty, které jste vytvořili, přečtěte si podrobné informace v [tooDev tooPublish pomocí skriptů Windows PowerShell a testovací prostředí](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
