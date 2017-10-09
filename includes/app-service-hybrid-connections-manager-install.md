
1. <span data-ttu-id="e36be-101">V hello **hybridní připojení** okně klikněte hello hybridní připojení jste právě vytvořili a pak klikněte na **naslouchací proces instalace**.</span><span class="sxs-lookup"><span data-stu-id="e36be-101">In hello **Hybrid connections** blade, click hello hybrid connection you just created, then click **Listener Setup**.</span></span>
   
    ![Klikněte na tlačítko naslouchací proces instalace](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. <span data-ttu-id="e36be-103">Hello **vlastnosti hybridní připojení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="e36be-103">hello **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="e36be-104">V části **místní správce hybridního připojení**, zvolte **stáhnout a nakonfigurovat ručně**, uložte balíček HybridConnectionManager.msi hello stáhli a zkopírujte připojovací řetězec hello brány.</span><span class="sxs-lookup"><span data-stu-id="e36be-104">Under **On-premises Hybrid Connection Manager**, choose **download and configure manually**, save hello downloaded HybridConnectionManager.msi package, and copy hello gateway connection string.</span></span>
   
    ![Kliknutím sem tooinstall](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. <span data-ttu-id="e36be-106">Příkazový řádek správce zadejte následující příkaz toostart hello instalačního programu hello:</span><span class="sxs-lookup"><span data-stu-id="e36be-106">From an administrator command prompt, type hello following command toostart hello installer:</span></span>
   
        start HybridConnectionManager.msi
4. <span data-ttu-id="e36be-107">Po hello spustí instalační program, klikněte na tlačítko **teď ne**, vyhledejte složku %ProgramFiles%\Microsoft\HybridConnectionManager toohello, spustí HCMConfigWizard.exe a klikněte na tlačítko **Ano** v hello **uživatele Účet řízení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e36be-107">After hello installer runs, click **Not now**, then browse toohello %ProgramFiles%\Microsoft\HybridConnectionManager folder, run HCMConfigWizard.exe and click **Yes** in hello **User Account Control** dialog.</span></span>
5. <span data-ttu-id="e36be-108">Vložte hello hybridní připojovací řetězec, který jste zkopírovali dříve a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e36be-108">Paste hello hybrid connection string that you copied earlier and click **OK**.</span></span> 
   
    ![Instalace](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. <span data-ttu-id="e36be-110">Po dokončení instalace hello, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="e36be-110">When hello install completes, click **Close**.</span></span>
   
    ![Klikněte na tlačítko Zavřít](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    <span data-ttu-id="e36be-112">Na hello **hybridní připojení** okno, hello **stav** teď zobrazuje sloupec **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="e36be-112">On hello **Hybrid connections** blade, hello **Status** column now shows **Connected**.</span></span> 
   
    ![Připojené stav](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

