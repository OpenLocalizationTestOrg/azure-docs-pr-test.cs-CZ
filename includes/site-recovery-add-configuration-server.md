1. Spusťte hello sjednocený instalační program instalační soubor.
2. V **než začnete**, vyberte **nainstalovat hello konfigurační server a procesový server**.

    ![Než začnete](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. V **licence k softwaru třetích stran**, klikněte na tlačítko **souhlasím** toodownload a nainstalujte MySQL.

    ![Software jiných výrobců](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. V **registrace**vyberte registrační klíč hello jste si stáhli z trezoru hello.

    ![Registrace](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. V **nastavení Internetu**, zadejte, jak hello hello zprostředkovatel, který běží na konfiguračním serveru hello připojí tooAzure Site Recovery přes Internet.

   a. Pokud chcete tooconnect s hello proxy, který je aktuálně nastavený na hello počítače, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.

   b. Pokud chcete hello zprostředkovatele tooconnect přímo, vyberte **připojit přímo bez proxy server tooAzure Site Recovery**.

   c. Pokud hello stávající proxy server vyžaduje ověřování, nebo pokud chcete použít pro připojení poskytovatele hello toouse vlastní proxy server, vyberte **Connect s vlastním nastavením proxy**.

     * Pokud používáte vlastní proxy server, musíte toospecify hello adresu, port a přihlašovací údaje.
     * Pokud používáte proxy server, které by už mít povolené hello adresy URL popisované v [požadavky](#prerequisites).

     ![Brána firewall](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. V **Kontrola předpokladů**, instalace se spustí kontrola toomake jistotu, že můžete spustit instalaci. Pokud se zobrazí upozornění o hello **čas globální synchronizace kontrola**, ověřte, že hello času na hello systémových hodin (**datum a čas** nastavení) hello stejné jako hello časové pásmo.

    ![Požadavky](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. V **MySQL konfigurace**, vytvořit přihlašovací údaje pro protokolování na instanci serveru MySQL toohello, který je nainstalován.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. V **prostředí podrobnosti**vyberte, jestli budete virtuální počítače VMware tooreplicate. Pokud jste, potom instalační program kontroluje, že je nainstalována PowerCLI 6.0.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. V **umístění instalovat**, vyberte, kde chcete tooinstall hello binární soubory a úložiště mezipaměti hello. Hello jednotku, kterou vyberete, musí mít minimálně 5 GB volného místa na disku, ale doporučujeme mezipaměti jednotku s alespoň 600 GB volného místa.

    ![Umístění instalace](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. V **výběr sítě**, zadejte naslouchací proces hello (síťový adaptér a SSL port) na které hello konfigurační server odesílá a přijímá data replikace. 9443 je hello výchozí port pro odesílání a příjem přenosů replikace, ale můžete upravit tento port číslo toosuit požadavků vašeho prostředí. V přidání toohello port 9443 jsme také otevřít port 443, který je používán operací replikace tooorchestrate webového serveru. Nepoužívejte port 443 pro odesílání nebo přijímání provoz replikace.

    ![Výběr sítě](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. V **Souhrn**, zkontrolujte hello informace a klikněte na tlačítko **nainstalovat**. Po dokončení instalace se vygeneruje heslo. Budete ho potřebovat k povolení replikace, proto si ho zkopírujte a uložte na bezpečném místě.

    ![Souhrn](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Po dokončení registrace hello server se zobrazí na hello **nastavení** > **servery** okno v trezoru hello.
