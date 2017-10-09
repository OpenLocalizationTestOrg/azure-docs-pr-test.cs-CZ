1. Spusťte hello Azure Site Recovery UnifiedSetup.exe
2. V **před zahájením**, vyberte **přidat další proces servery tooscale se nasazení**.

  ![Přidání procesového serveru](./media/site-recovery-add-process-server/ps-page-1.png)

3. V **podrobnosti o konfiguraci serveru**, zadejte IP adresu hello hello konfigurační Server a hello přístupové heslo.

  ![Přidání procesového serveru 2](./media/site-recovery-add-process-server/ps-page-2.png)
4. V **nastavení Internetu**, zadejte, jak hello hello zprostředkovatel, který běží na hello konfigurační Server připojí tooAzure Site Recovery přes Internet.

  ![Přidání procesového serveru 3](./media/site-recovery-add-process-server/ps-page-3.png)

   * Pokud chcete tooconnect s hello proxy, který je aktuálně nastavený na hello počítače, vyberte **připojit se s existujícím nastavením proxy serveru**.
   * Pokud chcete hello zprostředkovatele tooconnect přímo, vyberte **připojit přímo bez proxy**.
   * Pokud hello stávající proxy server vyžaduje ověřování, nebo pokud chcete použít pro připojení poskytovatele hello toouse vlastní proxy server, vyberte **Connect s vlastním nastavením proxy**.

     * Pokud používáte vlastní proxy server, musíte toospecify hello adresu, port a přihlašovací údaje.
     * Pokud používáte proxy server, můžete musí už mít povolené adresy URL služby toohello přístup.

5. V **Kontrola předpokladů**, instalace se spustí kontrola toomake jistotu, že můžete spustit instalaci. Pokud se zobrazí upozornění o hello **čas globální synchronizace kontrola**, ověřte, že hello času na hello systémových hodin (**datum a čas** nastavení) hello stejné jako hello časové pásmo.

     ![Přidání procesového serveru 4](./media/site-recovery-add-process-server/ps-page-4.png)

6. V **prostředí podrobnosti**vyberte, jestli budete virtuální počítače VMware tooreplicate. Pokud ano, instalační program zkontroluje, že je nainstalované rozhraní PowerCLI 6.0.

     ![Přidání procesového serveru 5](./media/site-recovery-add-process-server/ps-page-5.png)

7. V **umístění instalovat**, vyberte, kde chcete tooinstall hello binární soubory a úložiště mezipaměti hello. Hello jednotku, kterou vyberete, musí mít minimálně 5 GB volného místa na disku, ale doporučujeme mezipaměti jednotku s alespoň 600 GB volného místa.
     ![Přidání procesového serveru 5](./media/site-recovery-add-process-server/ps-page-6.png)

8. V **výběr sítě**, zadejte naslouchací proces hello (síťový adaptér a SSL port) na které hello konfigurační Server odesílá a přijímá data replikace. 9443 je hello výchozí port pro odesílání a příjem přenosů replikace, ale můžete upravit tento port číslo toosuit požadavků vašeho prostředí. V přidání toohello port 9443 jsme také otevřít port 443, který je používán operací replikace tooorchestrate webového serveru. Nepoužívejte port 443 pro odesílání nebo příjem provozu replikace.

     ![Přidání procesového serveru 6](./media/site-recovery-add-process-server/ps-page-7.png)
9. V **Souhrn**, zkontrolujte hello informace a klikněte na tlačítko **nainstalovat**. Po dokončení instalace se vygeneruje heslo. Budete ho potřebovat k povolení replikace, proto si ho zkopírujte a uložte na bezpečném místě.

     ![Přidání procesového serveru 7](./media/site-recovery-add-process-server/ps-page-8.png)
