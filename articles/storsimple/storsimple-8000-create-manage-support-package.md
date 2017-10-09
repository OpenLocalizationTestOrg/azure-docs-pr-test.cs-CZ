---
title: "balíček pro podporu řady StorSimple 8000 aaaCreate | Microsoft Docs"
description: "Zjistěte, jak toocreate, dešifrování a upravit balíčku pro podporu pro vaše zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 857555b6ba31b1527f8f00d19818ebbec6005d0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a>Vytvoření a Správa balíčku pro podporu pro řady StorSimple 8000

## <a name="overview"></a>Přehled

Balíček pro podporu zařízení StorSimple je mechanismus snadné použití, který shromažďuje všechny relevantní protokoly tooassist Microsoft Support vyřešit všechny problémy zařízení StorSimple. Hello shromažďovat protokoly jsou zašifrované a komprimované.

Tento kurz zahrnuje toocreate podrobné pokyny a spravovat hello balíček pro podporu pro vaše zařízení řady StorSimple 8000. Pokud pracujete s polem virtuální zařízení StorSimple, přejděte příliš[generovat balíček protokolu](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="create-a-support-package"></a>Vytvoření balíčku pro podporu

V některých případech budete potřebovat toomanually vytvořit balíček podporu hello prostřednictvím Windows Powershellu pro StorSimple. Například:

* Pokud potřebujete tooremove citlivé informace z protokolu souborů předchozí toosharing s Microsoft Support.
* Pokud máte potíže s odeslání balíčku hello kvůli tooconnectivity problémy.

Váš balíček ručně generovaného podpory můžete sdílet s Microsoft Support prostřednictvím e-mailu. Proveďte následující kroky toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple hello.

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a>toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple

1. toostart relaci prostředí Windows PowerShell s oprávněními správce na hello vzdálený počítač, který byl použit zařízení StorSimple tooyour tooconnect, zadejte následující příkaz hello:
   
    `Start PowerShell`
2. V relaci prostředí Windows PowerShell text hello připojte toohello SSAdmin konzoly vašeho zařízení:
   
   1. Hello příkazového řádku zadejte:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. V poli hello dialogové okno, které se otevře zadejte heslo správce zařízení. výchozí heslo Hello je _Heslo1_.
     
      ![Dialogové okno přihlašovacích údajů prostředí PowerShell](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. Vyberte **OK**.
   4. Hello příkazového řádku zadejte:
     
      `Enter-PSSession $MS`
3. V relaci hello, které se otevře zadejte příslušný příkaz hello.
   
   * Pro sdílené síťové složky, které jsou chráněné heslem zadejte:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       (Protože balíček pro podporu hello je zašifrován) budete vyzváni k zadání hesla, cesta toohello sdílené síťové složky a šifrovací přístupové heslo. Balíček pro podporu je poté jste vytvořili v zadané složce hello.
   * Pro sdílené složky, které nejsou chráněné heslem, není nutné hello `-Credential` parametr. Zadejte hello následující:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       balíček pro podporu Hello se vytvoří pro oba řadiče v hello zadané síťové sdílené složky. Je soubor zašifrované, komprimované, který může odeslat tooMicrosoft podporu pro řešení potíží. Další informace najdete v tématu [obraťte se na podporu společnosti Microsoft](storsimple-8000-contact-microsoft-support.md).

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
> Můžete upravit pouze podporu balíček, který byl vytvořen pomocí Windows Powershellu pro StorSimple. Nelze upravit balíček vytvořen v hello portálu Azure pomocí služby StorSimple Manager zařízení.

tooedit balíčku pro podporu před nahráním na webu Microsoft Support hello, nejprve dešifrovat balíček pro podporu hello, upravit hello soubory a pak ho znovu zašifrovat. Proveďte následující kroky hello.

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a>tooedit balíčku pro podporu ve Windows Powershellu pro StorSimple

1. Generovat balíček podporu jak je popsáno dříve, v [toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Stáhnout skript hello](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na vašeho klienta.
3. Importujte modul prostředí Windows PowerShell hello. Zadejte hello cesta toohello místní složku, ve které jste stáhli hello skriptu. modul hello tooimport, zadejte:
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. Všechny soubory hello *.aes* soubory, které jsou komprimovaná a šifrovaná. toodecompress a dešifrování souborů, zadejte:
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    Všimněte si, že hello skutečný soubor rozšíření se nyní zobrazují pro všechny soubory hello.
   
    ![Upravte balíček podpory](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. Když se zobrazí výzva k hello šifrovací přístupové heslo, zadejte přístupové heslo hello, který jste použili při vytvoření balíčku pro podporu hello.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. Vyhledejte složku toohello, který obsahuje soubory protokolu hello. Protože soubory protokolu hello jsou nyní dekomprimovat a dešifrovat, to bude mít původní přípony souborů. Upravte tyto soubory tooremove žádné informace o zákazníkovi například názvy svazku a IP adresy zařízení a ukládat soubory hello.
7. Zavřít hello soubory toocompress je s gzip šifrovat pomocí standardu AES 256. Toto je pro rychlost a zabezpečení v balíčku pro podporu hello přenosu přes síť. toocompress a šifrování souborů, zadejte následující hello:
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Upravte balíček podpory](./media/storsimple-8000-create-manage-support-package/IC750707.png)
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

* Další informace o hello [informace shromážděny v balíčku pro podporu hello](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)
* Zjistěte, jak příliš[použití podpůrných balíčků a zařízení v protokolech tootroubleshoot nasazením zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

