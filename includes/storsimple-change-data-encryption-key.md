<!--author=SharS last changed: 12/01/15-->

### <a name="step-1-authorize-a-device-toochange-hello-service-data-encryption-key-in-hello-azure-classic-portal"></a>Krok 1: Autorizace zařízení toochange hello šifrovacího klíče dat služby v hello portál Azure classic
Správce zařízení hello obvykle bude požadovat tohoto správce služeb hello autorizovat šifrovací klíče zařízení toochange služby data. Správce služeb Hello bude potom budete autorizovat hello zařízení toochange hello klíč.

Tento krok se provádí v hello portál Azure classic. Správce služeb Hello můžete vybrat zařízení ze seznamu zobrazených hello zařízení, které jsou způsobilé toobe oprávnění. Hello zařízení je pak šifrovacího klíče dat služby autorizovaný toostart hello změňte procesu.

#### <a name="which-devices-can-be-authorized-toochange-service-data-encryption-keys"></a>Zařízení, která může být oprávnění šifrovací klíče dat služby toochange?
Zařízení musí splňovat následující kritéria, aby mohla být autorizovaným tooinitiate služby dat šifrování klíče změny hello:

* Hello zařízení musí být online toobe vhodné pro službu data šifrování změny klíče autorizace.
* Můžete povolit hello stejného zařízení znovu po 30 minutách Pokud klíč hello změnit nebyla inicializována.
* Jiné zařízení, můžete povolit za předpokladu, že změny klíče hello nebyla inicializována hello dříve oprávněným zařízením. Po hello nové zařízení byla potvrzena, nelze staré zařízení hello zahajte změnu hello.
* Zařízení nejde autorizovat, když probíhá výměna šifrovacího klíče dat služby hello hello.
* Zařízení můžete povolit, když některé z hello zařízení registrovaná službou hello mít převracet hello šifrování, jiné mají není. V takových případech jsou způsobilé zařízení hello hello ty, které byly dokončeny změny hello do služby data šifrovací klíče.

> [!NOTE]
> V hello portál Azure classic nejsou v hello seznam zařízení, které mohou být zobrazeny virtuální zařízení StorSimple oprávnění změny klíče toostart hello.
> 
> 

Proveďte následující kroky tooselect hello a autorizovat zařízení tooinitiate hello služby dat šifrování změny klíče.

#### <a name="tooauthorize-a-device-toochange-hello-key"></a>tooauthorize klíč zařízení toochange hello
1. Na stránce řídicího panelu služby hello, klikněte na **šifrovacího klíče dat služby změnu**.
   
    ![Šifrovací klíč služby změn](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)
2. V hello **šifrovacího klíče dat služby změnu** dialogovém okně vyberte a autorizovat zařízení tooinitiate hello služby dat šifrování změny klíče. Hello rozevíracího seznamu má všech způsobilých zařízeních hello, které lze udělit oprávnění.
3. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### <a name="step-2-use-windows-powershell-for-storsimple-tooinitiate-hello-service-data-encryption-key-change"></a>Krok 2: Použití Windows Powershellu pro StorSimple tooinitiate hello služby šifrování klíče změny dat
Tento krok se provádí v hello Windows Powershellu pro StorSimple rozhraní na hello oprávnění zařízení StorSimple.

> [!NOTE]
> Žádné operace lze provést v hello portál Azure classic služby StorSimple Manager, dokud se nedokončí hello výměny klíčů.
> 
> 

Pokud používáte hello zařízení konzoly sériového portu tooconnect toohello rozhraní Windows PowerShell, proveďte následující kroky hello.

#### <a name="tooinitiate-hello-service-data-encryption-key-change"></a>změna šifrovacího klíče dat služby tooinitiate hello
1. Vyberte možnost 1 toolog na s úplným přístupem.
2. Hello příkazového řádku zadejte:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Po úspěšném dokončení hello rutiny, zobrazí se nové šifrovacího klíče dat služby. Zkopírujte a uložte tento klíč pro použití v kroku 3 tohoto procesu. Tento klíč bude použit tooupdate všechny hello zbývající zařízení registrovaná službou StorSimple Manager hello.
   
   > [!NOTE]
   > Tento proces musí zahájit v rámci čtyři hodiny autorizace zařízení StorSimple.
   > 
   > 
   
   Tento nový klíč se pak odešlou toohello služby toobe nabídnutých tooall hello zařízení, které jsou registrovány hello služby. Na řídicím panelu služby hello se potom zobrazí výstrahu. Služba Hello zakáže všechny operace hello na hello zaregistrovat zařízení a Správce zařízení hello bude nutné tooupdate šifrovacího klíče dat služby hello na hello jiná zařízení. Ale hello vstupně-výstupních operací (odesílání dat v cloudu toohello hostitele) nesmí být přerušeny.
   
   Pokud máte jedno zařízení zaregistrován tooyour služby, proces výměny hello je nyní dokončen a hello další krok můžete vynechat. Pokud máte více zařízení registrovaná tooyour službu, pokračujte toostep 3.

### <a name="step-3-update-hello-service-data-encryption-key-on-other-storsimple-devices"></a>Krok 3: Aktualizace hello šifrovacího klíče dat služby na jiná zařízení StorSimple
Tyto kroky je potřeba provést v prostředí Windows PowerShell rozhraní hello zařízení StorSimple, pokud máte více služby StorSimple Manager tooyour registrovaných zařízení. Hello klíč, který jste získali v kroku 2: použití Windows Powershellu pro StorSimple tooinitiate hello dat šifrování klíče změny služby musí být použité tooupdate všechny hello zbývající zaregistrována hello služby StorSimple Manager zařízení StorSimple.

Proveďte následující kroky tooupdate hello šifrování dat pro služby ve vašem zařízení hello.

#### <a name="tooupdate-hello-service-data-encryption-key"></a>šifrovací klíč dat služby tooupdate hello
1. Pomocí Windows Powershellu pro StorSimple tooconnect toohello konzolu. Vyberte možnost 1 toolog na s úplným přístupem.
2. Hello příkazového řádku zadejte:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Zadejte hello služby datový šifrovací klíč, který jste získali v [krok 2: použití Windows Powershellu pro StorSimple tooinitiate hello dat šifrování klíče změny služby](#to-initiate-the-service-data-encryption-key-change).

