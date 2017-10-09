Virtuální počítače Azure podporují připojení řady datových disků. Pro optimální výkon budete chtít, aby byl připojen toolimit hello počet vysoce využívané disky toohello virtuální počítač tooavoid možných omezení. Pokud všechny disky nejsou využívání vysoce v hello současně, účet úložiště hello může podporovat větší počet disků.

* **Pro spravované disky systému Azure:** limit počtu spravované disků je místní a také závisí na typu úložiště hello. Hello výchozí a také hello maximální limit je 10 000 jedno předplatné, jednotlivých oblasti a typ úložiště. Například můžete vytvořit až too10, spravované 000 standardní disky a také 10 000 premium spravované disky v předplatném a v oblasti. 

    Spravované snímky a bitové kopie, se počítají proti hello omezit spravované disky.

* **Pro účty úložiště úrovně Standard:** Účet úložiště úrovně Standard má maximální celkovou frekvenci požadavků 20 000 IOPS. Hello celkový počet IOPS pro všechny disky virtuálního počítače v standardní účet úložiště nesmí být delší než tento limit.
  
    Můžete vypočítat zhruba hello počet vysoce využívané disky podporuje jednu standardní účet úložiště založené na omezení četnosti hello požadavku. Například pro vrstvu základní virtuální počítač, hello maximální počet vysoce využité disků je o 66 (20 000/300 IOPS na disk) a pro standardní vrstvy virtuálního počítače, je o 40 (20 000/500 IOPS na disk), jak je znázorněno v tabulce hello. 
* **Pro účty úložiště úrovně Premium:** Účet úložiště úrovně Premium má maximální propustnost 50 Gb/s. Celková propustnost Hello všechny disky virtuálního počítače by neměl překročí toto omezení.

