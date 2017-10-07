---
title: "řešení potíží Site Recovery aaaAzure z VMware tooAzure | Microsoft Docs"
description: "Řešení potíží s chybami při replikaci virtuálních počítačů Azure"
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/24/2017
ms.author: asgang
ms.openlocfilehash: 912097c8892540dd798ba025e0b10374ca51d664
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-mobility-service-push-install-issues"></a>Řešení potíží s služba Mobility nabízené instalace

Tento článek podrobně hello běžné problémy, kterým čelí při pokusu o tooinstall hello služba Mobility na toosource serveru pro povolení ochrany.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>(Kód chyby 95107) Nebylo možné povolit ochranu.
**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95107 </br>***Zpráva –*** nabízenou instalaci hello mobility service toohello zdrojového počítače selhalo s kódem chyby ***EP0858***. <br> Aby hello přihlašovací údaje služby mobility tooinstall je nesprávný nebo hello uživatelský účet nemá dostatečná oprávnění | Přihlašovací údaje uživatele předpokladu, že tooinstall služba mobility na zdrojový počítač je nesprávný | Zkontrolujte správnost hello pověření uživatele zadané pro zdrojový počítač hello na konfiguračním serveru. <br> přihlašovací údaje uživatele tooadd či upravit: přejděte tooconfiguration serveru > Cspsconfigtool ikonu > Správa účtu. </br> Kromě toho ověřte následující předpoklady jsou zaškrtnuté toosuccessfully dokončení instalace push.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>(Kód chyby 95015) Nebylo možné povolit ochranu.
**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95105 </br>***Zpráva –*** nabízenou instalaci hello mobility service toohello zdrojového počítače selhalo s kódem chyby ***EP0856***. <br> Buď "Sdílení souborů a tiskáren" není povoleno na zdrojovém počítači hello nebo jsou problémy se síťovým připojením mezi hello procesový server a hello zdrojový počítač| Není povoleno sdílení souborů a tiskáren | Povolit "Sdílení souborů a tiskáren" na zdrojovém počítači hello v hello brány Windows Firewall, přejděte toohello zdrojový počítač > Nastavení v části brány Windows Firewall > "Povolit aplikace nebo funkci průchod bránou Firewall" > vyberte "Sdílení souborů a tiskáren pro všechny profily". </br> Kromě toho ověřte následující předpoklady jsou zaškrtnuté toosuccessfully dokončení instalace push.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>(Kód chyby 95117) Nebylo možné povolit ochranu.
**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95117 </br>***Zpráva –*** nabízenou instalaci hello mobility service toohello zdrojového počítače selhalo s kódem chyby ***EP0865***. <br> Buď hello zdrojový počítač není spuštěný nebo jsou problémy se síťovým připojením mezi hello procesový server a hello zdrojový počítač | Připojení k síti mezi procesovým serverem a zdrojového serveru | Zkontrolujte připojení mezi procesovým serverem a zdrojového serveru. </br> Kromě toho ověřte následující předpoklady jsou zaškrtnuté toosuccessfully dokončení instalace push.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>(Kód chyby 95103) Nebylo možné povolit ochranu.
**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95103 </br>***Zpráva –*** nabízenou instalaci hello mobility service toohello zdrojového počítače selhalo s kódem chyby ***EP0854***. <br> Buď "Windows Management Instrumentation (WMI)" není povolena na zdrojovém počítači hello nebo jsou problémy se síťovým připojením mezi hello procesový server a hello zdrojový počítač| Windows Management Instrumentation (WMI) v hello brány Windows Firewall | Povolit Windows Management Instrumentation (WMI) na hello brány Windows Firewall. V části nastavení brány Windows Firewall > "Povolit aplikace nebo funkci průchod bránou Firewall" > "Vyberte rozhraní WMI pro všechny profily". </br> Kromě toho ověřte následující předpoklady jsou zaškrtnuté toosuccessfully dokončení instalace push.

## <a name="check-push-install-logs-for-errors"></a>Zkontrolujte protokoly instalace Push chyby

Na serveru nebo proces konfigurace hello přejděte toofile 'PushinstallService' nacházející se v <Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\ toounderstand hello zdroj problému hello a použijte níže kroky tooresolve hello problém vyřešit.</br>
![pushiinstalllogs](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-install-pre-requisites-for-windows"></a>Nabízená instalace součásti pro Windows
### <a name="ensure-file-and-printer-sharing-is-enabled"></a>Ujistěte se, že "Sdílení souborů a tiskáren" je povolena
Povolit "Sdílení souborů a tiskáren" a "Windows Management Instrumentation" na zdrojovém počítači hello v hello brány Windows Firewall </br>
#### <a name="if-source-machine-is-domain-joined-br"></a>Pokud zdrojový počítač je připojený k doméně: </br>
Konfigurace nastavení brány firewall pomocí konzoly pro správu zásad skupiny (GPMC).
1. Přihlášení tooActive directory domény počítači jako správce a otevřete konzolu pro správu zásad skupiny (GPMC. MSC, spusťte z spuštění > Spustit).</br>
3. Pokud není nainstalován pro správu zásad skupiny, použijte odkaz hello příliš[hello instalace pro správu zásad skupiny](https://technet.microsoft.com/library/cc725932.aspx) </br>
4. Ve stromu konzoly pro správu zásad skupiny hello dvakrát klikněte na objekty zásad skupiny v doménové struktuře hello a doméně a přejděte příliš "Výchozí zásady domény". </br>
![gpmc1](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
5. Klikněte pravým tlačítkem na "Výchozí zásady domény" > Upravit > Otevře se nové okno "Editor správy zásad skupiny". </br>
![gpmc2](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
6. V hello Editor správy zásad skupiny přejděte tooComputer konfigurace > zásady > šablony pro správu > sítě > připojení k síti > Brána Windows Firewall. </br>
![gpmc3](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
7. Povolit hello následující nastavení pro profil domény a standardní profil </br>
a) poklikejte na soubor na výjimky "brány Windows Firewall: Povolit příchozí souborů a tiskáren výjimku pro sdílení". Vyberte povoleno a klikněte na tlačítko OK. </br>
b) dvakrát klikněte na výjimky "brány Windows Firewall: Povolit výjimku příchozí vzdálené správy". Vyberte povoleno a klikněte na tlačítko OK. </br>
![gpmc4](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

###### <a name="if-source-machine-is-not-domain-joined-and-part-of-workgroup-br"></a>Pokud zdrojový počítač není připojený k doméně a pracovní skupiny </br>
Konfigurace nastavení brány firewall na vzdáleném počítači (pro pracovní skupiny):
1. Přejděte toohello zdrojového počítače,</br>
2. V části nastavení brány Windows Firewall > "Povolit aplikace nebo funkci průchod bránou Firewall" > vyberte "Sdílení souborů a tiskáren pro všechny profily". </br>
3. V části nastavení brány Windows Firewall > "Povolit aplikace nebo funkci průchod bránou Firewall" > "Vyberte rozhraní WMI pro všechny profily". </br>

#### <a name="disable-remote-user-account-control-uac"></a>Zakázání vzdálené řízení uživatelských účtů (UAC)
Zakážete nástroj Řízení uživatelských účtů pomocí služby mobility hello toopush klíče registru.
1. Klikněte na tlačítko Start > Spustit > zadejte regedit > zadejte
2. Vyhledejte a pak klikněte na tlačítko hello následující podklíč registru: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
3. Pokud hello LocalAccountTokenFilterPolicy položky registru neexistuje, postupujte takto:
4. V nabídce Upravit hello > Nový > klikněte na hodnotu DWORD.
5. Zadejte LocalAccountTokenFilterPolicy a stiskněte klávesu ENTER.
6. Klikněte pravým tlačítkem na LocalAccountTokenFilterPolicy a poté klikněte na Upravit.
7. Do hello údaj hodnoty zadejte hodnotu 1 a pak klikněte na tlačítko OK.
8. Ukončete Editor registru.


## <a name="push-install-pre-requisites-for-linux"></a>Nabízená instalace předpoklady pro Linux:

1. Vytvořte uživatele root na zdrojovém serveru se Linux hello. (Jenom pro hello nabízené instalace a aktualizace pomocí tohoto účtu)</br>
2. Zkontrolujte tento soubor/etc/hosts hello na zdroji hello Linux server má položky, které mapují hello názvem místního hostitele tooIP adresy přidružené k všechny síťové adaptéry. </br>
3. Zkontrolujte, zda text hello nejnovější openssh, openssh server a openssl balíčky jsou nainstalované na zdrojovém serveru Linux. </br>
Zkontrolujte, jestli ssh port 22 je povolený a spuštěný. </br>
4. Zkontrolujte starší agenty hello odinstalovat, pokud na zdrojovém serveru hello už figurují zastaralé položky agentů a restartujte hello server, přeinstalujte agenta. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-hello-sshdconfig-file"></a>Povolit ověřování pomocí protokolu SFTP subsystém a heslo v souboru sshd_config hello
1. Na zdrojovém serveru Přihlaste se jako kořenový adresář. </br>
2. V souboru /etc/ssh/sshd_config souboru hello</br> Najít hello řádek, který začíná PasswordAuthentication. </br>
3. d.   Zrušením komentáře u hello řádku a změňte hodnotu "Ne" hello příliš "Ano". </br>
![Linux1](./media/site-recovery-protection-common-errors/linux1.png)
4. Najít hello řádek, který začíná "Subsystému" a zrušte komentář u hello řádku. </br>
![Linux2](./media/site-recovery-protection-common-errors/linux2.png)
5. Uložte změny hello a restartujte službu sshd hello. </br>

## <a name="push-installation-checks-on-configurationprocess-server"></a>Nabízená instalace kontroly na proces/konfigurace serveru.
#### <a name="validate-credentials-for-discovery-and-installation"></a>Ověření pověření pro zjišťování a instalaci

1. Z konfigurace serveru spusťte Cspsconfigtool</br>
![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Ujistěte se, že má účet hello sloužící k ochraně práva správce na zdrojovém počítači hello. </br>

#### <a name="check-connectivity-between-process-server-and-source-server"></a>Zkontrolujte připojení mezi procesovým serverem a zdrojového serveru
1. Zajistěte, aby byl procesový Server připojení k Internetu.
2. Připojení rozhraní WMI pomocí wbemtest.exe ověřte. </br>
Na procesní server hello, klepněte na tlačítko Start > Spustit > wbemtest.exe > okno Testování služby WMI se otevře, jak je vidět.</br>
   ![WMItestConnect1](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
Klikněte na připojit > zadejte IP adresu serveru hello zdroje v hello Namespace vstup uživatelské jméno a heslo (Pokud je zdrojový počítač připojený k doméně, zadejte název domény hello společně s uživatelské jméno jako "Název_domény\uživatelské_jméno". Pokud zdrojový počítač je v pracovní skupině, zadejte jenom uživatelské jméno hello.) Vyberte úroveň ověřování hello jako paket o ochraně osobních údajů. </br>
![WMItestConnect2](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
   Kliknutím na tlačítko Připojit. Teď připojení WMI hello by měla být úspěšné s hello poskytl data a okno Testování služby WMI hello mají být zobrazeny, jak je uvedeno níže: </br>
   ![WMItestConnect3](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
</br>
   Pokud není úspěšné připojení WMI bude chybovou zprávu automaticky otevírané okno. Hello následující snímek obrazovky ukazuje neúspěšný pokus, pokud rozhraní WMI nebo vzdálené správy není povoleno v bráně Windows firewall povolené aplikace. </br>
   ![WMItestConnect4](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
</br>

3. Zkontrolujte stav služby WMI hello a připojení.</br>
Na serveru nebo proces konfigurace hello </br>
Klikněte na tlačítko start > Spustit > wmimgmt.msc > Akce > Další akce > připojte počítač tooanother (zdrojový počítač). </br>
Zadejte přihlašovací údaje hello hello účet použitý pro ochranu a kontrolu, pokud připojení je v pořádku. </br>

#### <a name="verify-network-shared-folders-of-source-machine-is-accessible-from-process-server-ps-remotely-using-specified-credentials"></a>Ověřte síťových sdílených složek zdrojový počítač je přístupný ze serveru procesu (PS) vzdáleně pomocí zadaných přihlašovacích údajů.
  1. Přihlášení tooProcess serveru (PS) počítače, otevřete Průzkumníka souborů > v typu panelu Adresa hello > "\\\source-machine-ip\C$" > klikněte na tlačítko Enter. </br>
  ![Fileshare1](./media/site-recovery-protection-common-errors/fileshare1.png) </br>
  2. Průzkumník souborů zobrazí výzvu k zadání pověření. Zadejte hello uživatelské jméno a heslo > klikněte na tlačítko OK.</br>
   Pokud zdrojový počítač připojený k doméně, zadejte název domény hello společně s uživatelské jméno jako "Název_domény\uživatelské_jméno".</br>
   Pokud zdrojový počítač je v pracovní skupině, zadejte pouze hello "username". </br>
  ![Fileshare2](./media/site-recovery-protection-common-errors/fileshare2.png) </br>
  3. Pokud je připojení úspěšné, můžete si prohlédnout hello složky zdrojového počítače vzdáleně z procesu serveru (PS) </br>
  ![Fileshare3](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Pokud je připojení úspěšné, zkontrolujte, zda jsou splněny všechny požadavky.
>

Pokud nechcete, aby tooopen "Windows Management Instrumentation", můžete taky nainstalovat službu mobility ručně na zdrojovém počítači hello.</br> [Nainstalujte službu Mobility ručně prostřednictvím grafického uživatelského rozhraní](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) </br>
[Instalace prostřednictvím pokyny ke konfiguraci správce](site-recovery-install-mobility-service-using-sccm.md) </br>

## <a name="next-steps"></a>Další kroky
- [Povolení replikace pro virtuální počítače VMware](vmware-walkthrough-enable-replication.md)
