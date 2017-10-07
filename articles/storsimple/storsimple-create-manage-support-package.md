---
title: "aaaCreate balíčku pro podporu StorSimple | Microsoft Docs"
description: "Zjistěte, jak toocreate, dešifrování a upravit balíčku pro podporu pro zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 209aeee50e823fd2ca96ababd1d0cf3ea9cdad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a>Vytvoření a Správa balíčku pro podporu zařízení StorSimple
## <a name="overview"></a>Přehled
Balíček pro podporu zařízení StorSimple je mechanismus snadné použití, který shromažďuje všechny relevantní protokoly tooassist Microsoft Support vyřešit všechny problémy zařízení StorSimple. Hello shromažďovat protokoly jsou zašifrované a komprimované.

Tento kurz zahrnuje toocreate podrobné pokyny a spravovat balíček pro podporu hello.

## <a name="create-and-upload-a-support-package-in-hello-azure-classic-portal"></a>Vytvoření a odeslání balíčku pro podporu v hello portál Azure classic
Můžete vytvořit a nahrát lokalitě podporu balíček toohello Microsoft Support prostřednictvím hello **údržby** stránku hello služby v hello portál Azure classic.

> [!NOTE]
> nahrávání Hello vyžaduje podporu klíč. Pracovníka podpory by měl poskytovat tento tooyou e-mailem.
> 
> 

Podpoře šifrované a komprimované balíčku (.cab) je vytvořený a nahrané toohello podporu lokality. pracovník podpory Hello pak můžete načíst tento balíček z lokality hello podporu pro řešení potíží s hello.

Proveďte následující kroky v hello klasického portálu toocreate balíčku pro podporu hello.

#### <a name="toocreate-a-support-package-in-hello-azure-classic-portal"></a>toocreate balíčku pro podporu v hello portál Azure classic
1. Vyberte **zařízení** > **údržby**.
2. V hello **balíček pro podporu** vyberte **vytvoření a nahrání balíčku pro podporu**.
3. V hello **vytvoření a nahrání balíčku pro podporu** dialogové okno pole, hello následující:
   
    ![Vytvoření balíčku pro podporu](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * V hello **přístupový klíč podporu** text zadejte hello přístupový klíč. Pracovníka podpory společnosti Microsoft, by měli poslat tooyou tento klíč v e-mailu.
   * Vyberte hello políčko tooprovide souhlasu tooautomatically nahrávání hello podporu balíček toohello Microsoft Support lokalitu.
   * Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-create-manage-support-package/IC740895.png).

## <a name="manually-create-a-support-package"></a>Ruční vytvoření balíčku pro podporu
V některých případech budete potřebovat toomanually vytvořit balíček podporu hello prostřednictvím Windows Powershellu pro StorSimple. Například:

* Pokud potřebujete tooremove citlivé informace z protokolu souborů předchozí toosharing s Microsoft Support.
* Pokud máte potíže s odeslání balíčku hello kvůli tooconnectivity problémy.

Váš balíček ručně generovaného podpory můžete sdílet s Microsoft Support prostřednictvím e-mailu. Proveďte následující kroky toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple hello.

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a>toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple
1. toostart relaci prostředí Windows PowerShell s oprávněními správce na hello vzdálený počítač, který byl použit zařízení StorSimple tooyour tooconnect, zadejte následující příkaz hello:
   
    `Start PowerShell`
2. V relaci prostředí Windows PowerShell text hello připojte toohello SSAdmin konzoly vašeho zařízení:
   
   * Hello příkazového řádku zadejte:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * V poli hello dialogové okno, které se otevře zadejte heslo správce zařízení. výchozí heslo Hello je:
     
      `Password1`
     
      ![Dialogové okno přihlašovacích údajů prostředí PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)
   * Vyberte **OK**.
   * Hello příkazového řádku zadejte:
     
      `Enter-PSSession $MS`
3. V relaci hello, které se otevře zadejte příslušný příkaz hello.
   
   * Pro sdílené síťové složky, které jsou chráněné heslem zadejte:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       (Protože balíček pro podporu hello je zašifrován) budete vyzváni k zadání hesla, cesta toohello sdílené síťové složky a šifrovací přístupové heslo. Balíček pro podporu je poté jste vytvořili v zadané složce hello.
   * Pro sdílené složky, které nejsou chráněné heslem, není nutné hello `-Credential` parametr. Zadejte hello následující:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       balíček pro podporu Hello se vytvoří pro oba řadiče v hello zadané síťové sdílené složky. Je soubor zašifrované, komprimované, který může odeslat tooMicrosoft podporu pro řešení potíží. Další informace najdete v tématu [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a>Parametry rutiny Export-HcsSupportPackage Hello
Můžete použít následující parametry pomocí rutiny Export-HcsSupportPackage hello hello.

| Parametr | Požadované a volitelné | Popis |
| --- | --- | --- |
| `-Path` |Požaduje se |Použijte umístění hello tooprovide hello síťové sdílené složky, ve které hello je umístěn balíček pro podporu. |
| `-EncryptionPassphrase` |Požaduje se |Použití tooprovide toohelp přístupové heslo šifrování balíček pro podporu hello. |
| `-Credential` |Nepovinné |Použijte přihlašovací údaje toosupply přístupu pro sdílenou síťovou složku hello. |
| `-Force` |Nepovinné |Pomocí tooskip hello šifrovací přístupové heslo potvrzení kroku. |
| `-PackageTag` |Nepovinné |Použít toospecify adresář v *cesta* ve které podporu hello je umístěn balíčku. Hello výchozí hodnota je [název]-[aktuální datum a time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Nepovinné |Zadejte jako **clusteru** toocreate (výchozí) balíčku pro podporu pro oba řadiče. Pokud chcete pouze pro aktuální řadič hello toocreate balíčku, zadejte **řadič**. |

## <a name="edit-a-support-package"></a>Upravte balíček podpory
Po vygenerování balíčku pro podporu, bude pravděpodobně nutné tooedit hello balíček tooremove citlivé informace. To může zahrnovat názvů svazků, zařízení IP adresy a názvy zálohování ze souborů protokolů hello.

> [!IMPORTANT]
> Můžete upravit pouze podporu balíček, který byl vytvořen pomocí Windows Powershellu pro StorSimple. Nelze upravit balíček vytvořen v hello portál Azure classic pomocí služby StorSimple Manager.
> 
> 

tooedit balíčku pro podporu před nahráním na webu Microsoft Support hello, nejprve dešifrovat balíček pro podporu hello, upravit hello soubory a pak ho znovu zašifrovat. Proveďte následující kroky hello.

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a>tooedit balíčku pro podporu ve Windows Powershellu pro StorSimple
1. Generovat balíček podporu jak je popsáno dříve, v [toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Stáhnout skript hello](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na vašeho klienta.
3. Importujte modul prostředí Windows PowerShell hello. Zadejte hello cesta toohello místní složku, ve které jste stáhli hello skriptu. modul hello tooimport, zadejte:
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. Všechny soubory hello *.aes* soubory, které jsou komprimovaná a šifrovaná. toodecompress a dešifrování souborů, zadejte:
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    Všimněte si, že hello skutečný soubor rozšíření se nyní zobrazují pro všechny soubory hello.
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750706.png)
5. Když se zobrazí výzva k hello šifrovací přístupové heslo, zadejte přístupové heslo hello, který jste použili při vytvoření balíčku pro podporu hello.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. Vyhledejte složku toohello, který obsahuje soubory protokolu hello. Protože soubory protokolu hello jsou nyní dekomprimovat a dešifrovat, to bude mít původní přípony souborů. Upravte tyto soubory tooremove žádné informace o zákazníkovi například názvy svazku a IP adresy zařízení a ukládat soubory hello.
7. Zavřít hello soubory toocompress je s gzip šifrovat pomocí standardu AES 256. Toto je pro rychlost a zabezpečení v balíčku pro podporu hello přenosu přes síť. toocompress a šifrování souborů, zadejte následující hello:
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750707.png)
8. Po zobrazení výzvy zadejte šifrovací přístupové heslo hello upravené podporu balíčku.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. Poznamenejte si nové heslo hello, takže ho můžete sdílet s Microsoft Support vyžádání.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Příklad: Úprava souborů v balíčku pro podporu ve sdílené složce chráněné heslem
Hello následující příklad ukazuje, jak toodecrypt, upravit a znovu zašifrovat balíčku pro podporu.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[použití podpůrných balíčků a zařízení v protokolech tootroubleshoot nasazením zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

