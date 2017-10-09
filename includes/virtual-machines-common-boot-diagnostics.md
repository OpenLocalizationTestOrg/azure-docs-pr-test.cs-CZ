V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager. 

Při zpětném přepnutí vlastní image tooAzure nebo i spouštění jeden z Image platformy hello, může být mnoha důvodů, proč se virtuální počítač získá do jiných spouštěcího stavu. Tyto funkce povolit tooeasily jste diagnostiku a obnovení virtuálních počítačů v případě selhání spuštění.

Pro virtuální počítače s Linuxem, můžete snadno zobrazit výstup hello protokolu konzoly z hello portálu:

![portál Azure](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Pro systém Windows a virtuální počítače s Linuxem, je však Azure také umožňuje toosee snímek hello virtuálních počítačů ze hypervisoru hello:

![Chyba](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech. Všimněte si, snímky a výstup může trvat až minut tooappear too10 ve vašem účtu úložiště.

## <a name="common-boot-errors"></a>Běžné chyby spuštění

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Operační systém nebyl nalezen](https://support.microsoft.com/help/4010142)
- [Chyba při spuštění nebo NEDOSTUPNÉ_SPOUŠTĚCÍ_ZAŘÍZENÍ](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Povolení diagnostiky na novém virtuálním počítači
1. Při vytváření nového virtuálního počítače z hello portál Preview, vyberte hello **Azure Resource Manager** z rozevíracího seznamu model nasazení hello:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Nakonfigurujte hello monitorování možnost tooselect hello účet úložiště kam chcete tooplace tyto diagnostické soubory.
 
    ![Vytvoření virtuálního počítače](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Pokud nasazujete z šablony Azure Resource Manager, přejděte tooyour prostředek virtuálního počítače a připojte oddíl profilu hello diagnostiky. Mějte na paměti, hlavičkou verze rozhraní API "2015-06-15" hello toouse.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Hello diagnostického profilu můžete účet úložiště hello tooselect místo tooput tyto protokoly.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

toodeploy ukázku virtuální počítač s Diagnostika spouštění povoleno, podívejte se na naše úložišti sem.

## <a name="update-an-existing-virtual-machine"></a>Aktualizace stávajícího virtuálního počítače ##

Diagnostika spouštění tooenable prostřednictvím hello portál, můžete také aktualizovat existující virtuální počítač prostřednictvím hello portálu. Diagnostika spouštění vyberte hello možnost a uložte. Restartujte vliv tootake hello virtuálních počítačů.

![Aktualizace stávajícího virtuálního počítače](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

