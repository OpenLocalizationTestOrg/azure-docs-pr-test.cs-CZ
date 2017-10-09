1. V portálu hello z **všechny prostředky**, klikněte na tlačítko **+ přidat**. 
2. V hello **všechno, co** okno vyhledávacího pole, typ **brány místní sítě**, pak klikněte na tlačítko toosearch. Zobrazí se seznam. Klikněte na tlačítko **brány místní sítě** tooopen hello okna a potom klikněte na **vytvořit** tooopen hello **vytvořit bránu místní sítě** okno.

  ![Vytvoření brány místní sítě](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)

3. Na hello **okno brány místní sítě vytvořit**, zadejte hello hodnoty pro bránu místní sítě.

  - **Název**: Zadejte název objektu brány místní sítě.
  - **IP adresa:** jde hello hello zařízení VPN, který chcete Azure tooconnect na veřejnou IP adresu. Zadejte platnou veřejnou IP adresu. Hello IP adresa nesmí být za serverem NAT a má toobe dostupný v Azure. Pokud teď nemáte hello IP adresu, můžete použít hello hodnoty zobrazené v hello snímek obrazovky, ale budete potřebovat toogo zpět a nahraďte zástupný symbol IP adresu hello veřejnou IP adresu vašeho zařízení VPN. Azure, jinak nebude možné tooconnect.
  - **Adresní prostor** odkazuje rozsahy adres toohello hello sítě, který představuje tuto místní sítě. Můžete přidat více různých rozsahů adres. Ujistěte se, že hello rozsahy, který zde určíte nepřekrývají s jinými sítěmi, které chcete tooconnect na rozsahy. Azure bude směrovat rozsah adres hello, zda jste zadali toohello místní VPN zařízení IP adresu. *Tady používat svoje vlastní hodnoty, není hello hodnoty zobrazené na snímku obrazovky hello*.
  - **Předplatné:** ověřte, že hello správné předplatné se zobrazuje.
  - **Skupina prostředků:** hello vyberte skupinu prostředků, které chcete toouse. Můžete vytvořit novou skupinu prostředků, nebo vybrat skupinu prostředků, kterou jste už vytvořili.
  - **Umístění:** vyberte hello umístění, která tento objekt se vytvoří v. Můžete chtít tooselect hello stejné umístění, které virtuální sítě se nachází v, ale nejste požadované toodo tak.

4. Klepněte na tlačítko zadání hodnoty hello **vytvořit** dolnímu hello brány místní sítě hello okno toocreate hello.
