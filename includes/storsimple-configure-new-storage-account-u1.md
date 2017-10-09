<!--author=alkohli last changed: 9/17/15-->

#### <a name="tooadd-a-storage-account-in-storsimple-8000-series-update-10"></a>tooadd účet úložiště v zařízení StorSimple řady 8000 Update 1.0
1. Na úvodní stránce služby StorSimple Manager hello vyberte svoji službu a dvakrát na ni klikněte. Tím přejdete toohello **rychlý Start** stránky. Vyberte hello **konfigurace** stránky.
2. Klikněte na **Přidat nebo upravit účet úložiště**.
3. V hello **přidat nebo upravit účet úložiště** dialogové okno, klikněte na tlačítko **přidat nový**.
4. V hello **zprostředkovatele** hello pole, vyberte příslušného poskytovatele cloudové služby. Hello podporované poskytovatelé jsou Azure, Amazon S3, Amazon S3 s RRS, HP a OpenStack. Zadejte přihlašovací údaje hello a hello umístění přidružené k účtu úložiště vašich poskytovatelů cloudových služeb hello. Hello pole zobrazená pro přihlašovací údaje budou různé v závislosti na hello poskytovatele cloudové služby, který jste zadali. 
   
   * Pokud jste jako poskytovatele cloudových služeb vybrali Azure, zadejte hello **název** a hello primární **přístupový klíč** pro váš účet úložiště Microsoft Azure. Pro účet Azure hello umístění se automaticky vyplní.
     
        ![Přidání účtu úložiště Azure](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)
   * Pokud jste jako poskytovatele cloudových služeb vybrali Amazon S3 nebo Amazon S3 s RRS, zadejte popisný **Název účtu úložiště**, **Přístupový klíč** a **Tajný klíč**. U poskytovatelů Amazon S3 a Amazon S3 s RRS jsou podporovány hello následující umístění:
     
     * USA – standardní
     * USA – západ (Oregon)
     * USA – západ (severní Kalifornie)
     * EU (Irsko)
     * Asie a Tichomoří (Singapur)
     * Asie a Tichomoří (Sydney)
     * Asie a Tichomoří (Tokio)
     * Jižní Amerika (Sao Paulo)
       
       ![Přidání účtu úložiště Amazon](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
   * Pokud jste jako poskytovatele cloudových služeb vybrali HP, zadejte popisný **Název účtu úložiště**, **ID klienta**, **Uživatelské jméno** a **Heslo**. V případě poskytovatele HP jsou podporována hello následující umístění:
     
     * USA – východ
     * USA – západ
       
       ![Přidání účtu úložiště HP](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
   * Pokud jste jako poskytovatele cloudových služeb vybrali **Openstack**, zadejte **Název hostitele**, **Přístupový klíč** a **Tajný klíč**.
     
     > [!NOTE]
     > Pro všechny hello poskytovatelů cloudových služeb kromě Azure je povoleno popisný název. Můžete používat různé popisné názvy a vytvořit více než jeden účet úložiště s hello stejné nastavení přihlašovacích údajů.
     > 
     > 
     
        ![Přidání účtu úložiště Openstack](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)
5. Vyberte **povolit režim SSL** toocreate zabezpečený kanál pro síťovou komunikaci mezi vaše zařízení a hello cloudu. Vymazat hello **povolit režim SSL** políčko pouze v případě, že pracujete v privátním cloudu.
   
   > [!NOTE]
   > Pokud jako poskytovatele cloudových služeb používáte HP, protokol SSL bude vždy povolen.
   > 
   > 
6. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). Po úspěšném vytvoření účtu úložiště hello, budete upozorněni.
7. Zobrazí se Hello nově vytvořený účet úložiště na hello **konfigurace** v části **účty úložiště**. Klikněte na tlačítko **Uložit** toosave hello nový účet úložiště. Po zobrazení výzvy k potvrzení klikněte na **OK**.

