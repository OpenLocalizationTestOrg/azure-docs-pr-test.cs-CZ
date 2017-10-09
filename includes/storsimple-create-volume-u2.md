<!--author=alkohli last changed: 08/16/2016-->

#### <a name="toocreate-a-volume"></a>toocreate svazku
1. Na zařízení hello **rychlý Start** klikněte na tlačítko **přidat svazek** toostart hello Průvodci přidáním svazku.
2. V hello Průvodce přidáním svazku, v části **základní nastavení**:
   
   1. Zadejte **Název** svazku.
   2. V rozevíracím seznamu hello, vyberte hello **typ použití** svazku. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte typ svazku **Místně vázaný**. Pro ostatní typy dat vyberte typ svazku **Vrstvený**. Pokud tento svazek používáte k archivaci dat, zaškrtněte políčko **Použít tento svazek pro archivní data s méně častým přístupem**. 
      
       Místně vázaný svazek je tlustě zřízený a zajišťuje, že hello primární dat na svazku hello zůstává místní toohello zařízení a nebudou distribuována toohello cloudu.  Pokud vytvoříte místně vázaný svazek, zařízení hello kontrolovat dostupný prostor v místních vrstvách hello tooprovision hello objem hello požadovanou velikost. Hello operaci vytváření místně vázaný svazek může být existující data z cloudu toohello hello zařízení a doba hello toocreate hello svazku může trvat dlouho. Celkový čas Hello závisí na velikosti hello hello zřídit svazek, dostupnou šířku pásma sítě a hello dat ve vašem zařízení. 
      
       Vrstvený svazek je zřizovaný dynamicky a lze jej rychle vytvořit. Výběr **použít tento svazek pro archivní data s méně často používaná** pro vrstvený svazek určený pro velikost bloku archivní data změny hello odstranění duplicitních dat pro svazek too512 KB. Pokud políčko není zaškrtnuté, příslušný vrstvený svazek hello používá velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje hello zařízení tooexpedite hello přenos velkých objemů archivních dat toohello cloudu.
   3. Zadejte hello **zřízená kapacita** svazku. Poznamenejte si hello kapacity, která je k dispozici podle vybraného typu svazku hello. Hello zadat velikost svazku nesmí překročit hello volné místo.
      
       Můžete zřídit místně vázaných svazků až too8.5 TB a vrstvené svazky až too200 TB na zařízení 8100 hello. Větší zařízení 8600 hello můžete zřídit místně vázaných svazků až too22.5 TB a vrstvené svazky až too500 TB. Volné místo v zařízení hello je požadovaná toohost hello pracovní sady vrstvených svazků, vytváření místně vázaných svazků ovlivní hello místa ke zřizování vrstvených svazků. Pokud vytvoříte místně vázaný svazek, zmenší se dostupné volné místo potřebné k vytváření vrstvených svazků. Podobně pokud vrstvený svazek je vytvořen, se snižuje hello volného prostoru vytváření místně vázaných svazků.
      
       Pokud v zařízení 8100 zřídíte místně vázaný svazek šířka 8,5 TB (maximální možná velikost), vyčerpáte všechny hello volné místo dostupné v zařízení hello. Nebudete moct toocreate žádné vrstvené svazky z, a vyšší there bod je žádné volné místo na hello zařízení toohost hello pracovní sadu hello vrstvené svazku. Hello místa ovlivňují také vrstvené svazky existující. Pokud například používáte zařízení 8100, ve kterém jsou už zřízeny vrstvené svazky o velikosti zhruba 106 TB, k vytváření místně vázaných svazků zbude už jenom 4 TB dostupného volného místa.
      
       Hello následující obrázek ukazuje hello **základní nastavení** dialogové okno pro místně vázaný svazek.
      
        ![Přidání místního svazku](./media/storsimple-create-volume-u2/add-local-volume-include.png)
      
       Hello následující obrázek ukazuje hello **základní nastavení** dialogové okno pro vrstvený svazek.
      
        ![Přidání místního svazku](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)
   
   1. Klikněte na ikonu šipky hello ![ikona šipky](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) toogo toohello další stránku.
3. V hello **další nastavení** dialogu přidat nový záznam řízení přístupu (ACR):
   
   1. Zadejte **Název** záznamu ACR.
   2. V části **název iniciátoru iSCSI**, poskytovat hello iSCSI kvalifikovaný název (IQN) hostitele s Windows. Pokud hello IQN nemáte, přejděte příliš[Get hello názvu IQN hostitele Windows serveru](#get-the-iqn-of-a-windows-server-host).
   3. V části **výchozí zálohování tohoto svazku?**, vyberte hello **povolit** zaškrtávací políčko. Hello výchozí zálohování vytvoří zásadu, která se spustí ve 22:30 každý den (čas zařízení) a vytvoří cloudový snímek tohoto svazku.
      
      > [!NOTE]
      > Po povolení zálohování hello zde nelze vrátit. Je nutné tooedit hello svazku toomodify toto nastavení.
      > 
      > 
      
      ![Přidání svazku](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)
4. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png). Svazek je vytvořen s hello zadané nastavení.

