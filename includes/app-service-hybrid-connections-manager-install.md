
1. <span data-ttu-id="18bf8-101">V **hybridní připojení** okno, klikněte na tlačítko hybridní připojení, který jste právě vytvořili, klikněte **naslouchací proces instalace**.</span><span class="sxs-lookup"><span data-stu-id="18bf8-101">In the **Hybrid connections** blade, click the hybrid connection you just created, then click **Listener Setup**.</span></span>
   
    ![Klikněte na tlačítko naslouchací proces instalace](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. <span data-ttu-id="18bf8-103">**Vlastnosti hybridní připojení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="18bf8-103">The **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="18bf8-104">V části **místní správce hybridního připojení**, zvolte **stáhnout a nakonfigurovat ručně**, uložte stažený balíček HybridConnectionManager.msi a zkopírujte připojovací řetězec brány.</span><span class="sxs-lookup"><span data-stu-id="18bf8-104">Under **On-premises Hybrid Connection Manager**, choose **download and configure manually**, save the downloaded HybridConnectionManager.msi package, and copy the gateway connection string.</span></span>
   
    ![Chcete-li nainstalovat, klikněte sem](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. <span data-ttu-id="18bf8-106">Z příkazového řádku správce zadejte následující příkaz, který spouští instalační program:</span><span class="sxs-lookup"><span data-stu-id="18bf8-106">From an administrator command prompt, type the following command to start the installer:</span></span>
   
        start HybridConnectionManager.msi
4. <span data-ttu-id="18bf8-107">Po spuštění instalačního programu, klikněte na tlačítko **teď ne**, pak přejděte do složky %ProgramFiles%\Microsoft\HybridConnectionManager, spustí HCMConfigWizard.exe a klikněte na tlačítko **Ano** v **uživatelský účet Ovládací prvek** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18bf8-107">After the installer runs, click **Not now**, then browse to the %ProgramFiles%\Microsoft\HybridConnectionManager folder, run HCMConfigWizard.exe and click **Yes** in the **User Account Control** dialog.</span></span>
5. <span data-ttu-id="18bf8-108">Vložte hybridní připojovací řetězec, který jste zkopírovali dříve a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="18bf8-108">Paste the hybrid connection string that you copied earlier and click **OK**.</span></span> 
   
    ![Instalace](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. <span data-ttu-id="18bf8-110">Po dokončení instalace klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="18bf8-110">When the install completes, click **Close**.</span></span>
   
    ![Klikněte na tlačítko Zavřít](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    <span data-ttu-id="18bf8-112">Na **hybridní připojení** okně **stav** teď zobrazuje sloupec **připojeno**.</span><span class="sxs-lookup"><span data-stu-id="18bf8-112">On the **Hybrid connections** blade, the **Status** column now shows **Connected**.</span></span> 
   
    ![Připojené stav](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

