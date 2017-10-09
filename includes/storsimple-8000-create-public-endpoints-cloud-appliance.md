#### <a name="toocreate-public-endpoints-on-hello-cloud-appliance"></a>veřejné koncové body toocreate na zařízení cloudu hello

1. Přihlaste se toohello portálu Azure.
2. Přejděte příliš**virtuální počítače**a poté vyberte a klikněte na tlačítko hello virtuální počítač, který je používán jako vaše zařízení cloudu.
    
3. Je nutné toocreate k zabezpečení sítě (NSG) skupiny pravidlo toocontrol hello toku provozu a deaktivovat virtuálního počítače. Proveďte následující kroky toocreate pravidlo NSG hello.
    1. Vyberte **Skupina zabezpečení sítě**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Klikněte na tlačítko hello skupinu zabezpečení sítě v výchozí, který se zobrazí.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. Vyberte **Příchozí pravidla zabezpečení**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Klikněte na tlačítko **+ přidat** toocreate pravidlo příchozí zabezpečení.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        V okně pravidla zabezpečení příchozích hello přidat:

        1. Pro hello **název**, typ hello následující název koncového bodu hello: WinRMHttps.
        
        2. Pro hello **s prioritou**, vyberte číslo menší než 1 000 (což je hello prioritu hello výchozí pravidlo). Vyšší hodnota hello, hello s nižší prioritou.

        3. Sada hello **zdroj** příliš**žádné**.

        4. Pro hello **služby**, vyberte **WinRM**. Hello **protokol** bude automaticky nastavena příliš**TCP** a hello **rozsah portů** je nastaven příliš**5986**.

        5. Klikněte na tlačítko **OK** toocreate hello pravidlo.

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Posledním krokem je tooassociate skupiny zabezpečení sítě se podsíť nebo konkrétní síťové rozhraní. Proveďte následující kroky tooassociate hello vaše skupina zabezpečení sítě s podsítí.
    1. Přejděte příliš**podsítě**.
    2. Klikněte na **+ Přidružit**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Vyberte virtuální síť a pak vyberte příslušnou podsítí hello.
    4. Klikněte na tlačítko **OK** toocreate hello pravidlo.

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Po vytvoření pravidla hello můžete zobrazit jeho podrobnosti toodetermine hello veřejná virtuální IP (VIP) adres. Tuto adresu si poznamenejte.


