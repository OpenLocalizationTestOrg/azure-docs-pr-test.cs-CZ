
## <a name="azure-backup"></a>Azure Backup

Pro zálohování Azure virtuální počítače spuštěné úlohy v produkčním prostředí, pomocí Azure Backup. Azure Backup podporuje zálohování konzistentní s aplikací pro Windows a virtuální počítače s Linuxem. Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jenom určité soubory. 

Jednoduché praktických Úvod do služby Azure Backup pro virtuální počítače Azure, projděte si kurz "zálohovat virtuální počítače Azure" pro [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) nebo [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).

Další informace o tom, jak Azure Backup funguje, najdete v části [plánování vaší infrastruktury zálohování virtuálních počítačů v Azure](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery chrání virtuální počítače z hlavní havárii, když dojde výpadku způsobeného hlavní přírodní katastrofě nebo přerušení služeb rozšířeným celou oblast. Azure Site Recovery pro virtuální počítače můžete nakonfigurovat tak, aby vaše aplikace s jedním kliknutím v několika minut můžete obnovit. Můžete replikovat do oblasti Azure podle vaší volby, není omezen na spárované oblasti. 

Můžete spustit Nácvik zotavení po havárii s na vyžádání testovací převzetí služeb při selhání, aniž by to ovlivňovalo produkční zatížení nebo probíhající replikace. Vytvořte plány obnovení pro orchestraci převzetí služeb při selhání a navrácení služeb po obnovení celé aplikace běžící na několika virtuálních počítačích. Funkce plánu obnovení je integrovaná s runbooky služby Azure automation.

Abyste mohli začít podle [replikaci virtuálních počítačů](https://aka.ms/a2a-getting-started). 

## <a name="managed-snapshots"></a>Spravované snímky 

V vývojová a testovací prostředí poskytují snímky rychlá a jednoduchá možnost pro zálohování virtuálních počítačů, které používají spravovaný disky. Spravované snímek je jen pro čtení úplnou kopii se spravovaným diskem. Snímky existují nezávisle na zdrojový disk a slouží k vytvoření nové spravované disky pro virtuální počítač znovu sestavit. Se fakturují, v závislosti na používané části disku. Například pokud vytvoříte snímek se spravovaným diskem s zřízená kapacita 64 GB a skutečná data použité velikosti 10 GB, bude účtován snímku pouze pro velikost dat používaných 10 GB.  

Další informace o vytváření snímků najdete v tématu:

* [Vytvoření kopie VHD uložené jako spravovaný disk pomocí snímků ve Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Vytvoření kopie VHD uložené jako spravovaný disk pomocí snímků v Linuxu](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Další postup
Můžete vyzkoušet Azure Backup pomocí následujících "Záložní kurzu virtuální počítače Windows" pro [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) nebo [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).
