Při přidávání dat tooa disky virtuálního počítače s Linuxem, mohou nastat chyby, pokud disk neexistuje na logické jednotce 0. Pokud přidáváte ručně pomocí hello disk `azure vm disk attach-new` příkazu a zadáte na logické jednotce (`--lun`) namísto povolení hello platformy Azure toodetermine hello odpovídající logické jednotky, ujistěte se, že disk již existuje / bude existovat na logické jednotce 0. 

Vezměte v úvahu následující příklad zobrazuje fragment výstup hello hello `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

Hello dvě datové disky existovat logická jednotka 0 a 1 logická jednotka (hello první sloupec v hello `lsscsi` výstup podrobnosti `[host:channel:target:lun]`). Oba disky by měl být accessbile z v rámci hello virtuálních počítačů. Pokud byl ručně zadané hello první disk toobe přidat na logickou jednotku LUN 1 a hello druhý disk na logické jednotce 2, nemusíte vidět hello disky správně ze v rámci virtuálního počítače.

> [!NOTE]
> Hello Azure `host` hodnota je 5 v těchto příkladech, ale to se může lišit v závislosti na typu hello úložiště, které vyberete.
> 
> 

Toto chování disku není problém s Azure, ale způsob hello, ve které hello Linux jádra následuje specifikace SCSI hello. Sběrnice SCSI hello hledá připojená zařízení prohledávání hello Linux jádra zařízení musí být nalezen na logickou jednotku LUN 0 v pořadí pro toocontinue systému hello vyhledávání dalších zařízení. Takto:

* Zkontrolujte výstup hello `lsscsi` po přidání tooverify disku data máte disk na logické jednotce 0.
* Pokud disk nezobrazuje správně v rámci virtuálního počítače, ověřte, zda že disk na logické jednotce 0 existuje.

