## <a name="defining-a-backup-policy"></a>Definování zásad zálohování
Zásady zálohování definují při hello snímky dat pořizovat a jak dlouho jsou uchovávány těchto snímků. Při definování zásad pro zálohování virtuálních počítačů můžete úlohu zálohování aktivovat *jednou denně*. Když vytvoříte novou zásadu, je použité toohello trezoru. Hello rozhraní zásad zálohování vypadá takto:

![Zásady zálohování](./media/backup-create-policy-for-vms/backup-policy.png)

toocreate zásady:

1. Zadejte název hello **název zásady**.
2. Snímky dat můžete pořizovat v denních nebo týdenních intervalech. Použití hello **četnost záloh** rozevírací nabídky toochoose tom, jestli jsou snímky dat pořizovat denně nebo týdně.
   
   * Pokud si zvolíte denní interval, pomocí hello zvýrazněná řízení tooselect hello hello denní dobu pro vytvoření snímku hello. toochange hello hodinu, zrušíte výběr hello hodiny a vyberete novou hodinu hello.
     
     ![Zásady pro denní zálohování](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Pokud zvolíte týdenní interval, použijte hello zvýrazněný ovládací prvek tooselect hello dny v týdnu hello a čas hello den tootake hello snímku. V nabídce hello den vyberte jeden nebo několik dnů. V nabídce hello hodinu vyberte jednu hodinu. toochange hello hodinu, zrušíte výběr aktuální hodiny hello a vyberete novou hodinu hello.
     
     ![Zásady pro týdenní zálohování](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Ve výchozím nastavení jsou vybrané všechny možnosti **Rozsah uchování**. Zrušte zaškrtnutí políčka některý limit rozsahu uchování nechcete toouse. Potom můžete určete hello interval(s) toouse.
   
    Měsíční a roční rozsahy uchovávání umožňují toospecify hello snímky podle denního nebo týdenního přírůstku.
   
   > [!NOTE]
   > Když chráníte virtuální počítač, úloha zálohování se spustí jednou denně. čas Hello při spuštění zálohování hello je hello stejné pro všechny rozsahy uchování.
   > 
   > 
4. Po nastavení všech možností zásad hello, klikněte v horní části hello hello okna na **Uložit**.
   
    nové zásady Hello je okamžitě použité toohello trezoru.

