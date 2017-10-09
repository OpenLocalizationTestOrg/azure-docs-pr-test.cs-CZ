<!--author=alkohli last changed: 07/19/2017-->

#### <a name="toocreate-a-volume"></a>toocreate svazku
1. Z hello tabulkové seznam zařízení hello v hello **zařízení** okně vyberte příslušné zařízení. Klikněte na **+ Přidat svazek**.

    ![Přidání nového svazku](./media/storsimple-8000-create-volume-u2/step5createvol1.png)

2. V hello **přidat svazek** okno:
   
   1. Hello **vyberte zařízení** s aktuální zařízení se automaticky vyplní pole.

   2. Hello rozevíracím seznamu vyberte kontejner svazků hello, kde je nutné tooadd svazku. 

   3.  Zadejte **Název** svazku. Po vytvoření hello svazku se nedá přejmenovat svazku.

   4. V rozevíracím seznamu hello, vyberte hello **typ** svazku. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte typ svazku **Místně vázaný**. Pro ostatní typy dat vyberte typ svazku **Vrstvený**. Pokud tento svazek používáte k archivaci dat, zaškrtněte políčko **Použít tento svazek pro archivní data s méně častým přístupem**.
      
       Vrstvený svazek je zřizovaný dynamicky a lze jej rychle vytvořit. Výběr **použít tento svazek pro archivní data s méně často používaná** pro vrstvený svazek určený pro velikost bloku archivní data změny hello odstranění duplicitních dat pro svazek too512 KB. Pokud políčko není zaškrtnuté, příslušný vrstvený svazek hello používá velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje hello zařízení tooexpedite hello přenos velkých objemů archivních dat toohello cloudu.
       
       Místně vázaný svazek je tlustě zřízený a zajišťuje, že hello primární dat na svazku hello zůstává místní toohello zařízení a nebudou distribuována toohello cloudu.  Pokud vytvoříte místně vázaný svazek, zařízení hello kontrolovat dostupný prostor v místních vrstvách hello tooprovision hello objem hello požadovanou velikost. Hello operaci vytváření místně vázaný svazek může být existující data z cloudu toohello hello zařízení a doba hello toocreate hello svazku může trvat dlouho. Celkový čas Hello závisí na velikosti hello hello zřídit svazek, dostupnou šířku pásma sítě a hello dat ve vašem zařízení.

   5. Zadejte hello **zřízená kapacita** svazku. Poznamenejte si hello kapacity, která je k dispozici podle vybraného typu svazku hello. Hello zadat velikost svazku nesmí překročit hello volné místo.
      
       Můžete zřídit místně vázaných svazků až too8.5 TB a vrstvené svazky až too200 TB na zařízení 8100 hello. Větší zařízení 8600 hello můžete zřídit místně vázaných svazků až too22.5 TB a vrstvené svazky až too500 TB. Volné místo v zařízení hello je požadovaná toohost hello pracovní sady vrstvených svazků, vytváření místně vázaných svazků ovlivní hello místa ke zřizování vrstvených svazků. Proto pokud vytvoříte místně připojený svazek, zmenší se dostupné volné místo pro vytváření vrstvených svazků. Podobně pokud vrstvený svazek je vytvořen, se snižuje hello volného prostoru vytváření místně vázaných svazků.
      
       Pokud v zařízení 8100 zřídíte místně vázaný svazek šířka 8,5 TB (maximální možná velikost), vyčerpáte všechny hello volné místo dostupné v zařízení hello. Nelze vytvořit žádné vrstvené svazky od tohoto okamžiku už nezbude žádné volné místo na hello zařízení toohost hello pracovní sadu hello vrstvené svazku. Hello místa ovlivňují také vrstvené svazky existující. Pokud například používáte zařízení 8100, ve kterém jsou už zřízeny vrstvené svazky o velikosti zhruba 106 TB, k vytváření místně vázaných svazků zbude už jenom 4 TB dostupného volného místa.

    6. V hello **připojení hostitele** pole, klikněte na šipku hello. 

        ![Připojení hostitelé](./media/storsimple-8000-create-volume-u2/step5createvol2.png)

    7. V hello **připojení hostitele** okně zvolte existující ACR nebo přidat nové ACR provedením hello následující kroky:

       1. Zadejte **Název** záznamu ACR.
       2. V části **název iniciátoru iSCSI**, poskytovat hello iSCSI kvalifikovaný název (IQN) hostitele s Windows. Pokud hello IQN nemáte, přejděte příliš[Get hello názvu IQN hostitele Windows serveru](#get-the-iqn-of-a-windows-server-host).

    9. Klikněte na možnost **Vytvořit**. Svazek je vytvořen s hello zadané nastavení.

        ![Kliknutí na Vytvořit](./media/storsimple-8000-create-volume-u2/step5createvol3.png)

        > [!NOTE]
        > Upozorňujeme, že není chráněn hello svazek, který jste vytvořili v tomto poli. Pomocí tohoto svazku tootake naplánované zálohování budete potřebovat toocreate a přidružit zásady zálohování. 

