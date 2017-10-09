<!--author=alkohli last changed: 02/06/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a>tooinstall aktualizace z hello portálu Azure

1. Na stránce služby StorSimple hello vyberte zařízení. Přejděte příliš**zařízení** > **údržby**.
2. V dolní části hello hello stránky, klikněte na tlačítko **kontrolovat aktualizace**. Úloha je vytvořena tooscan dostupné aktualizace. Budete informováni hello úloha úspěšně dokončena.
3. V hello **aktualizace softwaru** části na hello stejné stránce hello nové aktualizace softwaru jsou k dispozici. Doporučujeme, abyste zkontrolovali hello poznámky k verzi před instalací aktualizace na vašem zařízení.
4. V dolní části hello hello stránky, klikněte na tlačítko **instalovat aktualizace**a potom **OK**.
5. V hello **nainstalovat aktualizace** dialogové okno, ujistěte se, že jste udělali hello doporučení a pak vyberte **I pochopit hello výše požadavek a mě připravené tooupgrade zařízení** a klikněte na tlačítko zaškrtnutí hello tlačítko.
   
    ![Potvrzovací zpráva](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. Spustí se sada kontrol požadavků. Mezi tyto kontroly patří:
   
   * **Kontroly stavu řadiče** tooverify obě text hello řadiče zařízení jsou v pořádku a online.
   * **Kontroly stavu součásti hardwaru** tooverify, že všechny hardwarové součásti v zařízení StorSimple hello jsou v pořádku.
   * **Ověří DATA 0** tooverify, že DATA 0 je povolena na vašem zařízení. Pokud toto rozhraní není povolené, musíte jej povolit a poté to zkusit znovu.
   * **DATA 2 a DATA 3 kontroly** tooverify, že nejsou povoleny síťová rozhraní DATA 2 a DATA 3. Pokud tato rozhraní jsou povolené, musíte zakázat tyto a opakujte tooupdate zařízení. Tato kontrola se provádí, pouze pokud provádíte aktualizaci ze zařízení, na kterém běží software verze GA. U zařízení s verzemi 0.1, 0.2 nebo 0.3 tato kontrola není nutná.
   * **Kontrola brány** na libovolném zařízení se systémem předchozí tooUpdate verze 1. Tato kontrola se provádí na všechna zařízení hello 1 softwarem před aktualizací, nezdaří se však hello zařízení, která byla nakonfigurována pro síťové rozhraní než DATA 0.
     
     aktualizace Hello je použita, pokud všechny kontroly byly úspěšně dokončeny. Budete upozorněni, když byly v průběhu kontroly hello.
     
     ![Upozornění na předběžnou kontrolu](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     Hello tady je příklad, ve kterém hello kontroly se nezdařilo. Je nutné ověřit, zda jsou oba řadiče zařízení hello v pořádku a online. Budete také potřebovat toocheck hello stav hello hardwarové součásti. V tomto příkladu vyžadují pozornost komponenty Controller 0 a Controller 1. Pokud nelze vyřešit tyto problémy tak, že sami může být nutné toocontact Microsoft Support.
     
       ![Selhání kontrol](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. Po úspěšném dokončení kontroly hello se vytvoří úloha aktualizace. Budete informováni hello aktualizace úlohy je úspěšně vytvořen.
   
    ![Vytvoření úlohy aktualizace](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    aktualizace Hello se potom použije ve vašem zařízení.
    
8. Klikněte na tlačítko toomonitor hello průběh úlohy aktualizace hello, **zobrazit úlohy**. Na hello **úlohy** stránky, můžete zobrazit hello aktualizovat průběh.
9. aktualizace Hello trvat několik hodin toocomplete. Vyberte hello aktualizace úlohu a klikněte na tlačítko **podrobnosti** tooview hello podrobnosti úlohy hello kdykoli.
10. Po dokončení úlohy hello přejděte toohello **údržby** stránky a posuňte se dolů příliš**aktualizace softwaru**.

