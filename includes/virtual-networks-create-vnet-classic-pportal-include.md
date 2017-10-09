## <a name="how-toocreate-a-classic-vnet-in-hello-azure-portal"></a>Jak toocreate klasické virtuální sítě v hello portálu Azure
toocreate klasické virtuální sítě založené na scénář hello výše, postupujte podle následujících kroků hello.

1. V prohlížeči přejděte toohttp://portal.azure.com a, v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **nový** > **sítě** > **virtuální síť**, Všimněte si, že hello **vybrat model nasazení** Zobrazí seznam již **Classic**a potom klikněte na **vytvořit**, jak je vidět na následujícím obrázku hello.
   
    ![Vytvoření virtuální sítě VNet na portálu Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. Na hello **virtuální síť** okno, typ hello **název** hello virtuální síť a potom klikněte na tlačítko **adresní prostor**. Nakonfigurujte nastavení prostoru adres pro hello virtuální sítě a jeho první podsíť a pak klikněte na tlačítko **OK**. Hello obrázek níže ukazuje nastavení blok CIDR hello pro náš scénář.
   
    ![Okno prostoru adres](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Klikněte na tlačítko **skupiny prostředků** a vyberte hello mezi virtuálními tooadd skupiny prostředků, nebo klikněte na **vytvořit novou skupinu prostředků** tooadd hello virtuální síť tooa novou skupinu prostředků. Hello následující obrázek znázorňuje hello prostředků nastavení skupiny pro novou skupinu prostředků s názvem **TestRG**. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
   
    ![Vytvořit okno skupiny prostředků](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. V případě potřeby změňte hello **předplatné** a **umístění** nastavení pro virtuální síť. 
6. Pokud nechcete, aby toosee hello virtuální síť jako dlaždici v hello **úvodní panel**, zakažte **Pin tooStartboard**. 
7. Klikněte na tlačítko **vytvořit** a Všimněte si hello dlaždice s názvem **vytváření virtuální sítě** jak je znázorněno na následujícím obrázku hello.
   
    ![Vytvoření sítě VNet na portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Počkejte hello toobe virtuální síť vytvořili, a až se zobrazí hello dlaždici níže, klikněte na něj tooadd další podsítě.
   
    ![Vytvoření sítě VNet na portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Měli byste vidět hello **konfigurace** pro vaši virtuální síť, jak je uvedeno níže. 
   
    ![Vytvoření sítě VNet na portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Klikněte na tlačítko **podsítě** > **přidat**, zadejte **název** a zadejte **rozsahu (blok CIDR) adres** pro podsíť a potom Klikněte na tlačítko **OK**. Hello obrázek níže ukazuje hello nastavení pro náš scénář aktuální.
    
    ![Vytvoření virtuální sítě VNet na portálu Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

