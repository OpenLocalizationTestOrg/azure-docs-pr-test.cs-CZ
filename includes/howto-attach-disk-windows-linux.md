


## <a name="attach-an-empty-disk"></a>Připojení prázdného disku
Prázdný disk se připojuje je jednoduchý způsob tooadd, data na disku, protože pro vás vytvoří soubor VHD hello Azure a ukládá je v účtu úložiště hello.

1. Klikněte na tlačítko **virtuálních počítačů (klasické)**, a pak vyberte hello odpovídající virtuálních počítačů.

2. V nabídce nastavení hello, klikněte na tlačítko **disky**.

   ![Připojit nový prázdný disk](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Na panelu příkazů hello, klikněte na tlačítko **připojit nový**.  
    Hello **připojit nový disk** zobrazí se dialogové okno.

    ![Připojit nový disk](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Vyplňte hello následující informace:
    - V **název souboru**, přijměte výchozí název hello nebo zadejte jinou pro soubor VHD hello. datový disk Hello používá automaticky vygenerovaným názvem, pokud zadáte jiný název souboru VHD hello.
    - Vyberte hello **typ** z hello datový disk. Všechny virtuální počítače podporují standardní disky. Mnoho virtuálních počítačů také nepodporují prémiové disky.
    - Vyberte hello **velikost (GB)** z hello datový disk.
    - Pro **použití mezipaměti u hostitele**, zvolte Žádný, nebo jen pro čtení.
    - Klikněte na tlačítko OK toofinish.

4. Jakmile hello datový disk se vytvoří a připojené, je uvedena v části disky hello hello virtuálních počítačů.

   ![Nové a prázdný datový disk se úspěšně připojil](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Po přidání datový disk, potřebujete toolog na toohello virtuálních počítačů a inicializovat hello disk, aby ho bylo možné použít.

## <a name="how-to-attach-an-existing-disk"></a>Postupy: připojit stávající disk
Připojení stávajícího disku vyžaduje, aby v účtu úložiště byl dostupný soubor .vhd. Použití hello [přidat AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) rutiny tooupload hello VHD souboru toohello účet úložiště. Po vytvoření a nahrát soubor VHD hello, abyste ho připojili tooa virtuálních počítačů.

1. Klikněte na tlačítko **virtuálních počítačů (klasické)**, a pak vyberte hello odpovídající virtuální počítač.

2. V nabídce nastavení hello, klikněte na tlačítko **disky**.

3. Na panelu příkazů hello, klikněte na tlačítko **připojit existující**.

    ![Připojení datového disku](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Klikněte na tlačítko **umístění**. účty úložiště k dispozici Hello zobrazení. Potom vyberte účet odpovídající úložiště ze seznamu.

    ![Zadejte účet úložiště disku](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. A **účet úložiště** obsahuje jeden nebo více kontejnerů, které obsahují disky (VHD). Vyberte z uvedených hello odpovídajícího kontejneru.

    ![Zadejte kontejner virtuálního počítače windows](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. Hello **virtuální pevné disky** panel uvádí uchovávat v kontejneru hello hello diskových jednotek. Klikněte na jednu hello disků a potom klikněte na vybrat.

    ![Zadejte bitovou kopii disku pro virtuální počítače windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. Hello **připojit stávající disk** panelu se zobrazí znovu, umístěním hello obsahující hello účet úložiště, kontejneru a vybraného pevného disku (vhd) tooadd toohello virtuálního počítače.

  Nastavit **použití mezipaměti u hostitele** toonone nebo pro čtení, klikněte na tlačítko OK.

    ![Datový disk se úspěšně připojil](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
