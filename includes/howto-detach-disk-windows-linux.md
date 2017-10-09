Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač, můžete ho snadno odpojit. Disk se odpojuje odebere hello disk z virtuálního počítače hello, ale neodstraní hello disku z hello účtu úložiště Azure.

Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.  

> [!NOTE]
> toodetach disk operačního systému, musíte nejdřív toodelete hello virtuálního počítače.
>

## <a name="find-hello-disk"></a>Najít hello disk
Pokud si nejste jisti hello název hello disku nebo chcete tooverify ji předtím, než jej odpojte, postupujte podle těchto kroků.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Klikněte na tlačítko **virtuální počítače**, a pak vyberte hello odpovídající virtuálních počítačů.

3. Klikněte na tlačítko **disky** podél hello levá strana řídicí panel hello virtuálního počítače, v části **nastavení**.

 virtuální počítač Hello řídicí panel uvádí hello název a typ všech připojených discích. Třeba tato obrazovka ukazuje virtuální počítač s jedním diskem operačního systému (OS) a jedním datovým diskem:

    ![Vyhledání datového disku](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-hello-disk"></a>Odpojte hello disk
1. Z hello portálu Azure, klikněte na tlačítko **virtuální počítače**a potom klikněte na název hello hello virtuálního počítače, který má datový disk hello chcete toodetach.

2. Klikněte na tlačítko **disky** podél hello levá strana řídicí panel hello virtuálního počítače, v části **nastavení**.

3. Klikněte na disk hello chcete toodetach.

  ![Identifikovat toodetach disku hello](./media/howto-detach-disk-windows-linux/disklist.png)

4. Na panelu příkazů hello, klikněte na **odpojení**.

  ![Vyhledejte hello detach – příkaz](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. V okně potvrzení hello klikněte **Ano** toodetach hello disku.

  ![Potvrďte odpojuje hello disku](./media/howto-detach-disk-windows-linux/confirmdetach.png)

Hello disk zůstává v úložišti, ale je už připojené tooa virtuální počítač.
