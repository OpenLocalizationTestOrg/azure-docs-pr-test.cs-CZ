#### <a name="toocreate-a-virtual-device"></a>toocreate virtuálního zařízení
1. V hello portálu Azure, přejděte toohello **StorSimple Manager** služby.
2. Přejděte toohello **zařízení** stránky. Klikněte na tlačítko **vytvořit virtuální zařízení** dole hello hello **zařízení** stránky.
3. V hello **dialogové okno vytvořit virtuální zařízení**, zadejte následující podrobnosti hello.
   
    ![Vytvoření virtuálního zařízení StorSimple](./media/storsimple-create-virtual-device-u2/CreatePremiumsva1.png)
   
   1. **Název** – jedinečný název virtuálního zařízení.
   2. **Model** – zvolte model virtuálního zařízení hello hello. Toto pole se zobrazí, jenom pokud používáte verzi Update 2 nebo novější. Model zařízení 8010 nabízí úložiště Standard Storage 30 TB a model 8020 úložiště Premium Storage 64 TB. Pokud chcete nasadit zařízení pro scénáře obnovování položek ze záloh,
   3. toodeploy scénáře úrovně načítání položek ze zálohy. Vyberte 8020 toodeploy vysoký výkon, úloh s nízkou latencí nebo používat jako sekundární zařízení pro zotavení po havárii.
   4. **Verze** – zvolte verzi hello hello virtuální zařízení. Pokud je vybrán model zařízení 8020, pak pole verze hello nebude předkládané toohello uživatele. Tato možnost je chybí Pokud všechny hello fyzického zařízení registrovaná ve službě běží Update 1 (nebo novější). Toto pole se zobrazí jenom v případě, že máte směs před aktualizací 1 a Update 1 fyzického zařízení zaregistrovaná s hello stejné služby. Zadaný hello verzi virtuálního zařízení hello určí, které fyzické zařízení můžete převzetí služeb při selhání či klonovat, je důležité vytvořit příslušnou verzi virtuálního zařízení hello. Vyberte:
      
      * Verzi Update 0.3, pokud budete provádět převzetí služeb při selhání nebo zotavení při havárii z fyzického zařízení s verzí Update 0.3 nebo starší. 
      * Verzi Update 1, pokud budete provádět převzetí služeb při selhání nebo klonování z fyzického zařízení s verzí Update 1 nebo novější. 
   5. **Virtuální síť** – zadejte virtuální síť, které chcete toouse s tímto virtuálním zařízením. Pokud používáte Premium Storage (Update 2 nebo novější), je nutné vybrat virtuální síť, která je podporována s hello účet služby Premium Storage. v rozevíracím seznamu hello zobrazí šedě Hello nepodporované virtuální sítě. Pokud vyberete nepodporovanou virtuální síť, zobrazí se upozornění. 
   6. **Účet úložiště pro vytvoření virtuálního zařízení** – vyberte image účtu storage toohold hello hello virtuálního zařízení během zřizování. Tento účet úložiště by měl být v hello stejné oblasti jako virtuální zařízení hello a virtuální sítě. Není vhodné jej použít pro ukládání dat hello fyzické nebo virtuální zařízení hello. Ve výchozím nastavení se za tímto účelem vytvoří nový účet úložiště. Ale pokud víte, že už máte účet úložiště, který je vhodný k tomuto účelu, můžete vybrat ji ze seznamu hello. Při vytváření prémiového virtuálního zařízení, hello rozevíracím seznamu se zobrazí jenom účty úložiště Premium. 
      
      > [!NOTE]
      > Hello virtuálního zařízení můžete pracovat pouze s účty úložiště Azure hello. Ostatní poskytovatele cloudových služeb například Amazon, HP a OpenStack (které jsou podporovány pro fyzické zařízení hello) nejsou podporovány pro virtuální zařízení StorSimple hello.
      > 
      > 
   7. Klikněte na tlačítko zaškrtnutí tooindicate hello, který víte, že se bude hostovat hello data uložená na virtuální zařízení hello v datacentru společnosti Microsoft. Pokud používáte jenom fyzické zařízení, váš šifrovací klíč se uchovává se zařízením, a Microsoft proto nemůže data dešifrovat. 
      
       Při použití virtuálního zařízení hello šifrovací klíč a hello dešifrovací klíč jsou uložené ve službě Microsoft Azure. Další informace najdete v článku o [bezpečnostních aspektech použití virtuálního zařízení](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Klikněte na tlačítko hello zkontrolujte ikonu toocreate hello virtuální zařízení. Hello zařízení může trvat přibližně 30 minut toobe zřízený.
      
      ![Fáze vytváření virtuálního zařízení StorSimple](./media/storsimple-create-virtual-device-u2/StorSimple_VirtualDeviceCreating1M.png)

