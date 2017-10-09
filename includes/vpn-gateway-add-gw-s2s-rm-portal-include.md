1. Na levé straně stránky portálu hello hello, klikněte na tlačítko  **+**  a zadejte brány virtuální sítě v hledání. V části **Výsledky** vyhledejte položku **Brána virtuální sítě** a klikněte na ni.
2. Hello dolní části okna "Brána virtuální sítě" hello, klikněte na **vytvořit**. Tím se otevře hello **vytvořit bránu virtuální sítě** okno.

    ![Vytvoření polí okna brány virtuální sítě](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Nová brána")

3. Na hello **vytvořit bránu virtuální sítě** okno, zadejte hello hodnoty pro bránu virtuální sítě.

  - **Název**: Zadejte pro bránu název. Toto není hello stejné jako podsítě brány. To je hello název objektu hello brány, kterou vytváříte.
  - **Typ brány**: Vyberte **VPN**. Brány sítě VPN použít typ brány virtuální sítě hello **VPN**. 
  - **Typ sítě VPN**: Vyberte hello typ sítě VPN, který je určený pro vaši konfiguraci. Většina konfigurací vyžaduje trasový typ VPN.
  - **Skladová položka**: skladová položka brány hello vyberte z rozevíracího seznamu hello. SKU Hello uvedené v rozevírací nabídce hello závisí na hello typ sítě VPN, které vyberete. Další informace o SKU brány najdete v tématu [SKU brány](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Umístění**: může být nutné tooscroll toosee umístění. Upravit hello **umístění** pole toopoint toohello umístění, kde se nachází virtuální sítě. Pokud umístění hello neukazuje toohello oblasti, kde je umístěn virtuální sítě, hello virtuální sítě se nezobrazí v dalším kroku hello 'vybrat virtuální síť, rozevírací seznam.
  - **Virtuální síť**: Zvolte hello virtuální sítě toowhich tooadd chcete tuto bránu. Klikněte na tlačítko **virtuální síť** tooopen hello 'Vybrat virtuální síť' okno. Vyberte hello virtuální sítě. Pokud nevidíte virtuální síť, ujistěte se, že pole umístění hello ukazovat toohello oblast, ve kterém se nachází virtuální sítě.
  - **Veřejná IP adresa**: hello "Vytvoření veřejné IP adresy, okno vytvoří objekt veřejnou IP adresu. Hello veřejnou IP adresu přiřazována dynamicky při vytvoření brány VPN hello. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. Však neznamená to, že hello IP adresa změní po byl přiřazen tooyour brány VPN. Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.

    - Nejprve, klikněte na tlačítko **veřejnou IP adresu** tooopen hello okno 'vybrat veřejnou IP adresu, a pak klikněte na **+ vytvořit nový** tooopen hello "Vytvoření veřejné IP adresy, okno.
    - V dalším kroku vstup **název** pro svoji veřejnou IP adresu, pak klikněte na **OK** v hello dolní části tohoto okna toosave změny.

      ![Vytvoření veřejné IP adresy](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "Vytvořit PIP")

4. Ověřte nastavení hello. Můžete vybrat **Pin toodashboard** v hello dolní části okna hello, pokud chcete, aby vaše tooappear brány na řídicím panelu hello. 
5. Klikněte na tlačítko **vytvořit** toobegin vytváření brány VPN hello. Hello nastavení budou ověřena a uvidíte hello "Brána nasazení virtuální sítě" dlaždici na řídicím panelu hello. Vytvoření brány může trvat až too45 minut. Může být nutné toorefresh stav hello dokončit toosee vaší stránky portálu.

Po vytvoření brány hello zobrazte hello IP adresu, která byla přiřazena tooit prohlížením hello virtuální sítě hello portálu. Hello brány se zobrazí jako připojené zařízení. Můžete kliknout na tooview hello připojené zařízení (bránu virtuální sítě) Další informace.
