#### <a name="toocreate-a-cloud-appliance"></a>toocreate zařízení cloudu

1. V hello portálu Azure, přejděte toohello **Manager zařízení StorSimple** služby.
2. Přejděte toohello **zařízení** okno. Na panelu příkazů hello v okně Souhrn služby hello, klikněte na **vytvořit cloudu zařízení**.
    ![Vytvoření cloudového zařízení StorSimple](./media/storsimple-8000-create-cloud-appliance-u2/sca-create1.png)
3. V hello **vytvořit cloudu zařízení** okno, zadejte následující podrobnosti hello.
   
    ![Vytvoření cloudového zařízení StorSimple](./media/storsimple-8000-create-cloud-appliance-u2/sca-create2m.png)
   
   1. **Název** – Jedinečný název cloudového zařízení.
   2. **Model** -vyberte model hello hello cloudu zařízení. Zařízení 8010 nabízí službu Storage úrovně Standard s 30 TB úložiště, zatímco zařízení 8020 má službu Storage úrovně Premium s 64 TB úložiště. Zadejte 8010 toodeploy položku úrovně načtení scénáře ze zálohy. Vyberte 8020, toodeploy vysoký výkon, nízká latence úloh, nebo použijte jako sekundární zařízení pro zotavení po havárii.
   3. **Verze** – zvolte verzi hello hello cloudu zařízení. Hello verze odpovídá verzi toohello hello bitové kopie virtuálního disku, který je použité toocreate hello cloudu zařízení. Zadaný hello verzi hello cloudu zařízení určuje, které fyzické zařízení převzetí služeb při selhání nebo klonovat z, je důležité vytvořit příslušnou verzi hello cloudu zařízení.
   4. **Virtuální síť** – zadejte virtuální síť, které chcete toouse se tato zařízení cloudu. Pokud používáte Storage úrovně Premium, je nutné vybrat virtuální síť, která je podporována s hello účet služby Premium Storage. Hello nepodporované virtuální sítě jsou zobrazena šedě v rozevíracím seznamu hello. Pokud vyberete nepodporovanou virtuální síť, zobrazí se upozornění.
   5. **Podsíť** -založená na vybranou virtuální síť hello, hello rozevíracího seznamu zobrazí hello přidružené podsítě. Přiřadíte zařízení cloudu tooyour s podsítí.
   6. **Účet úložiště** – vyberte image účtu storage toohold hello hello cloudu zařízení během zřizování. Tento účet úložiště by měl být v hello stejné oblasti jako zařízení hello cloudu a virtuální sítě. Není vhodné jej použít pro ukládání dat hello fyzické nebo hello cloudu zařízení. Ve výchozím nastavení se za tímto účelem vytvoří nový účet úložiště. Ale pokud víte, že už máte účet úložiště, který je vhodný k tomuto účelu, můžete vybrat ji ze seznamu hello. Pokud vytváříte o zařízení cloudu premium, hello rozevíracího seznamu zobrazí jenom účty služby Premium Storage.
      
      > [!NOTE]
      > Hello cloudu zařízení můžete pracovat pouze s účty úložiště Azure hello.
    
   7. Vyberte zaškrtávací políčko tooindicate hello, který pochopit, hello dat uložených na zařízení cloudu hello je hostována v datacentru společnosti Microsoft.
       * Pokud používáte jenom fyzické zařízení, váš šifrovací klíč se uchovává se zařízením, a Microsoft proto nemůže data dešifrovat.

       * Pokud používáte zařízení s cloud, hello šifrovací klíč a hello dešifrovací klíč jsou uložené ve službě Microsoft Azure. Další informace najdete v tématu popisujícím [aspekty zabezpečení pro používání cloudového zařízení](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Klikněte na tlačítko **vytvořit** tooprovision hello cloudu zařízení. Hello zařízení může trvat přibližně 30 minut toobe zřízený. Budete upozorněni, když zařízení cloudu hello je úspěšně vytvořen. Přejděte tooDevices okno a hello seznam zařízení, aktualizuje toodisplay hello cloudu zařízení. Stav Hello hello zařízení je **připraveni tooset až**.
      
      ![Zařízení StorSimple cloudu připravené tooset nahoru](./media/storsimple-8000-create-cloud-appliance-u2/sca-create3.png)

