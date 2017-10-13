## <a name="defining-a-backup-policy"></a>Definování zásad zálohování
Zásady zálohování definují postupy, podle kterých se pořizují snímky dat včetně doby uchovávání těchto snímků. Při definování zásad pro zálohování virtuálních počítačů můžete úlohu zálohování aktivovat *jednou denně*. Když vytvoříte nové zásady, budou použity na trezor. Rozhraní zásad zálohování vypadá následovně:

![Zásady zálohování](./media/backup-create-policy-for-vms/backup-policy.png)

Vytvoření zásad:

1. Zadejte **Název zásad**.
2. Snímky dat můžete pořizovat v denních nebo týdenních intervalech. V rozevírací nabídce **Četnost zálohování** vyberte, jestli chcete snímky dat pořizovat denně nebo týdně.
   
   * Pokud zvolíte denní interval, použijte zvýrazněný ovládací prvek a vyberte čas, kdy se má snímek pořídit. Hodinu můžete změnit tak, že zrušíte výběr aktuální hodiny a vyberete novou hodinu.
     
     ![Zásady pro denní zálohování](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Pokud zvolíte týdenní interval, použijte zvýrazněný ovládací prvek a vyberte dny v týdnu a čas, kdy chcete snímek pořizovat. V nabídce dnů vyberte jeden nebo několik dnů. V nabídce hodin vyberte požadovanou hodinu. Hodinu můžete změnit tak, že zrušíte výběr aktuální hodiny a vyberete novou hodinu.
     
     ![Zásady pro týdenní zálohování](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Ve výchozím nastavení jsou vybrané všechny možnosti **Rozsah uchování**. Pokud některý limit rozsahu uchování nechcete používat, zrušte jeho zaškrtnutí. Potom zadejte interval nebo intervaly, které chcete používat.
   
    Měsíční a roční rozsahy uchovávání umožňují stanovit pořizování snímků na základě denního nebo týdenního přírůstku.
   
   > [!NOTE]
   > Když chráníte virtuální počítač, úloha zálohování se spustí jednou denně. Čas spuštění zálohování je stejný pro všechny rozsahy uchování.
   > 
   > 
4. Po nastavení všech možností zásad klikněte v horní části okna na **Uložit**.
   
    Nové zásady se okamžitě použijí na trezor.

