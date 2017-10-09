
1. V hello **hybridní připojení** okně klikněte hello hybridní připojení jste právě vytvořili a pak klikněte na **naslouchací proces instalace**.
   
    ![Klikněte na tlačítko naslouchací proces instalace](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. Hello **vlastnosti hybridní připojení** otevře se okno. V části **místní správce hybridního připojení**, zvolte **stáhnout a nakonfigurovat ručně**, uložte balíček HybridConnectionManager.msi hello stáhli a zkopírujte připojovací řetězec hello brány.
   
    ![Kliknutím sem tooinstall](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Příkazový řádek správce zadejte následující příkaz toostart hello instalačního programu hello:
   
        start HybridConnectionManager.msi
4. Po hello spustí instalační program, klikněte na tlačítko **teď ne**, vyhledejte složku %ProgramFiles%\Microsoft\HybridConnectionManager toohello, spustí HCMConfigWizard.exe a klikněte na tlačítko **Ano** v hello **uživatele Účet řízení** dialogové okno.
5. Vložte hello hybridní připojovací řetězec, který jste zkopírovali dříve a klikněte na tlačítko **OK**. 
   
    ![Instalace](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Po dokončení instalace hello, klikněte na tlačítko **Zavřít**.
   
    ![Klikněte na tlačítko Zavřít](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Na hello **hybridní připojení** okno, hello **stav** teď zobrazuje sloupec **připojeno**. 
   
    ![Připojené stav](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

