toocreate virtuální sítě v modelu nasazení Resource Manager hello pomocí hello portál Azure, postupujte podle následujících kroků hello. Použití hello [ukázkové hodnoty](#values) Pokud používáte tyto kroky jako kurzu. Pokud tyto kroky nejsou to jako kurz, být jisti tooreplace hello hodnoty vlastními. Další informace o práci s virtuálními sítěmi najdete v tématu hello [Přehled virtuálních sítí](../articles/virtual-network/virtual-networks-overview.md).

1. V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se pomocí účtu Azure.
2. Klikněte na možnost **Nové**. V hello **vyhledávání hello marketplace** pole, zadejte "Virtuální sítě". Vyhledejte **virtuální sítě** z hello vrátil seznamu a klikněte na tlačítko tooopen hello **virtuální sítě** okno.
3. Téměř hello dolní části okna virtuální sítě hello, z hello **vybrat model nasazení** vyberte **Resource Manager**a potom klikněte na **vytvořit**. Otevře se okno vytvořit virtuální síť hello.

    ![Okno pro vytvoření virtuální sítě](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Okno pro vytvoření virtuální sítě")
4. Na hello **vytvořit virtuální síť** okno, nakonfigurujte nastavení sítě VNet hello. Po vyplnění polí hello hello červený vykřičník stane zelená značka zaškrtnutí při hello znaků zadané v poli hello jsou platné.

  - **Název**: Zadejte název hello vaší virtuální sítě. V tomto příkladu používáme TestVNet1.
  - **Adresní prostor**: Zadejte hello adresní prostor. Pokud máte více tooadd prostory adres, přidejte první adresní prostor. Můžete přidat další adresní prostory později, po vytvoření hello virtuální sítě. Ujistěte se, že hello adresní prostor, který zadáte, nepřekrývá s adresním prostorem hello pro vaše místní umístění.
  - **Název podsítě**: přidejte hello první názvem a podsíť rozsah adres podsítě. Můžete přidat další podsítě a podsíť brány hello později, po vytvoření této virtuální sítě. 
  - **Předplatné**: Ověřte, že uvedeného odběru hello je hello správná. Odběry můžete změnit pomocí rozevíracího seznamu hello.
  - **Skupina prostředků**: Vyberte existující skupinu prostředků nebo vytvořte novou zadáním názvu nové skupiny prostředků. Pokud vytváříte novou skupinu, skupinu prostředků hello název podle tooyour plánované hodnoty konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
  - **Umístění**: Vyberte hello umístění pro virtuální síť. umístění Hello Určuje, kde se bude nacházet hello prostředky, abyste nasadili toothis virtuální sítě.

5. Vyberte **Pin toodashboard** Jestliže chcete mít toofind toobe virtuální síť snadno na řídicím panelu hello a pak klikněte na tlačítko **vytvořit**. Po kliknutí na **vytvořit**, zobrazí se na dlaždici na řídicím panelu se projeví hello průběh vaší virtuální sítě. změny dlaždice Hello jako hello se vytváří virtuální sítě.
