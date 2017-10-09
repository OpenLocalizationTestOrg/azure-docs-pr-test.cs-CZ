<!--author=alkohli last changed: 06/22/17-->

#### <a name="toocreate-a-volume-container"></a>toocreate kontejneru svazků
1. Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**. Hello tabulkové výpis hello zařízení vyberte a klikněte na zařízení. 

    ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. Na řídicím panelu hello zařízení, klikněte na tlačítko **+ přidat kontejner svazků**

    ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. V hello **přidat kontejner svazků** okno:
   
   1. Hello zařízení je automaticky vybrán.
   2. Zadejte **Název** kontejneru svazků. Hello název musí mít 3 too32 znaků. Kontejner svazků se po vytvoření už nedá přejmenovat.
   3. Vyberte **povolit šifrování úložiště cloudu** tooenable šifrování hello dat odesílaných ze hello zařízení toohello cloud.
   4. Zadejte a potvrďte **šifrovací klíč cloudového úložiště** tedy 8 too32 znaků. Tento klíč se používá hello zařízení tooaccess zašifrovaná data.
   5. Vyberte **účet úložiště** tooassociate s tímto kontejnerem svazků. Můžete vybrat existující účet úložiště nebo hello výchozí účet, který se vygeneruje při hello čas vytvoření služby. Můžete taky hello **přidat nový** toospecify možnost účet úložiště, které nejsou propojené toothis předplatné služby.
   6. Vyberte **neomezený** v hello **zadejte šířku pásma** rozevíracího seznamu, pokud chcete tooconsume veškerou dostupnou šířku pásma hello. Můžete také nastavit tuto možnost také**vlastní** tooemploy řízení šířky pásma a zadejte hodnotu v rozmezí 1 až 1 000 MB/s.
      Pokud máte k dispozici informace o vaší šířky pásma využití, může být schopný tooallocate šířky pásma na základě plánu zadáním **vybrat šablonu šířky pásma**. Podrobný postup najdete příliš[přidání šablony šířky pásma](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).

      ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. Klikněte na možnost **Vytvořit**.

        ![Okno Kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       Upozornění se zobrazí při úspěšném vytvoření kontejneru svazků hello.

       ![Oznámení o vytvoření kontejneru svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   Hello nově vytvořený kontejner svazků je uvedena v seznamu hello kontejnery svazků pro vaše zařízení.

   ![Okno Přidat kontejner svazků](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


