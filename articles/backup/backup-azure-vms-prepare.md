---
title: "aaaPreparing tooback vašeho prostředí se virtuální počítače Azure | Microsoft Docs"
description: "Ujistěte se, že vaše prostředí je připravený pro zálohování virtuálních počítačů v Azure"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonn
editor: 
keywords: "zálohování; zálohování;"
ms.assetid: 238ab93b-8acc-4262-81b7-ce930f76a662
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/25/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 3b914c507dd6ad703ea799115ae84ac229e27787
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-environment-tooback-up-azure-virtual-machines"></a>Příprava vašeho prostředí tooback zálohu virtuálních počítačích Azure
> [!div class="op_single_selector"]
> * [Model Resource Manager](backup-azure-arm-vms-prepare.md)
> * [Klasického modelu](backup-azure-vms-prepare.md)
>
>

Předtím, než můžete zálohovat virtuální počítač Azure (VM), existují tři podmínky, které musí existovat.

* Třeba toocreate trezor záloh nebo identifikovat existující trezor záloh *v hello stejné oblasti jako virtuální počítač*.
* Vytvoření připojení k síti mezi hello Azure veřejné internetové adresy a hello koncové body Azure úložiště.
* Nainstalujte agenta virtuálního počítače hello na hello virtuálních počítačů.

Pokud znáte tyto podmínky již neexistuje ve vašem prostředí poté pokračovat toohello [zálohování váš článek virtuálních počítačů](backup-azure-vms.md). Jinak přečtěte si, tento článek vás provede hello kroky tooprepare vaše prostředí tooback si virtuální počítač Azure.

##<a name="supported-operating-system-for-backup"></a>Podporovaný operační systém pro zálohování
 * **Linux**: Azure Backup podporuje [seznam distribucí schválených pro Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), kromě základního OS Linux. _Další přineste-vaše – vlastní-Linuxových distribucích také může fungovat, dokud je k dispozici na virtuálním počítači hello hello agent virtuálního počítače a podpora pro Python existuje. Jsme však není neschvaluje těchto distribuce pro zálohování._
 * **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Omezení při zálohování a obnovení virtuálních počítačů
> [!NOTE]
> Azure má dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Hello následující seznam obsahuje hello omezení při nasazení v hello klasického modelu.
>
>

* Zálohování virtuálních počítačů s více než 16 datových disků není podporována.
* Zálohování virtuálních počítačů s vyhrazenou IP adresu a žádný definovaný koncový bod není podporována.
* Zálohovaná data neobsahuje tooVM jednotky připojené síti připojit.
* Nahrazení existujícího virtuálního počítače během obnovení se nepodporuje. Nejprve odstranit hello existující virtuální počítač a všechny přidružené disky a potom obnovit hello data ze zálohy.
* Mezi oblastmi zálohování a obnovení není podporována.
* Zálohování virtuálních počítačů pomocí služby Azure Backup hello je podporované ve všech oblastech veřejný Azure (viz hello [kontrolní seznam](https://azure.microsoft.com/regions/#services) z podporovaných oblastí). Pokud hello oblast, kterou hledáte, není podporován dnes, nezobrazí se v rozevíracím seznamu hello při vytváření trezoru.
* Zálohování virtuálních počítačů pomocí služby Azure Backup hello je podporována pouze pro verze vyberte operační systém:
* Obnovení řadiče domény (DC) virtuálního počítače, který je součástí konfigurace více – řadič domény je možné pouze pomocí prostředí PowerShell. Další informace o [obnovení řadiče domény, řadiče domény služby více](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Obnovení virtuálních počítačů, které mají hello následující zvláštní síťové konfigurace je podporována pouze pomocí prostředí PowerShell. Virtuální počítače, které vytvoříte pomocí pracovního postupu obnovení hello v hello uživatelského rozhraní nebude mít tyto konfigurace sítě, po dokončení operace obnovení hello. Další, najdete v části toolearn [obnovení virtuálních počítačů s konfigurací speciální síťových](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuální počítače v části Konfigurace služby Vyrovnávání zatížení (interních a externích)
  * Virtuální počítače s více vyhrazené IP adresy
  * Virtuální počítače s více síťových adaptérů

## <a name="create-a-backup-vault-for-a-vm"></a>Vytvořte trezor záloh pro virtuální počítač
Trezor záloh je entita, která ukládá všechny hello zálohy a body obnovení, které byly vytvořeny v čase. Hello trezor záloh obsahuje také zásady zálohování hello, které budou zálohovaných použité toohello virtuálních počítačů.

> [!IMPORTANT]
> Od března 2017 se už můžete trezory Backup portálu classic toocreate hello. Existující trezory Backup jsou stále podporovány, a je možné příliš[pomocí prostředí Azure PowerShell trezory Backup toocreate](./backup-client-automation-classic.md#create-a-backup-vault). Společnost Microsoft doporučuje, protože budoucí vylepšení použít tooRecovery trezory služeb, pouze se vytvořit trezory služeb zotavení pro všechna nasazení.


Tento obrázek ukazuje hello vztahy mezi hello různými entitami Azure Backup: ![Azure Backup entity a vztahy](./media/backup-azure-vms-prepare/vault-policy-vm.png)



## <a name="network-connectivity"></a>Připojení k síti
Snímky virtuálních počítačů hello pořadí toomanage, musí rozšíření zálohování hello toohello připojení k Azure veřejné IP adresy. Bez hello správné připojení k Internetu, hello virtuálního počítače HTTP žádosti o vypršení časového limitu a hello selže celá operace zálohování. Pokud vaše nasazení má omezení přístupu na místě (prostřednictvím skupinu zabezpečení sítě (NSG), např.), zvolte jednu z těchto možností pro poskytování zrušte cestu pro provoz zálohování:

* [Povolit rozšíření rozsahy IP adres Azure datacenter hello](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -najdete v článku hello pokyny na tom, jak toowhitelist hello IP adres.
* Nasazení proxy server HTTP pro směrování provozu.

Při rozhodování, kterou možnost toouse, jsou hello kompromis mezi spravovatelnosti, přesná kontrola a náklady.

| Možnost | Výhody | Nevýhody |
| --- | --- | --- |
| Rozsahy povolených IP adres |Žádné dodatečné poplatky.<br><br>Pro přístup k otevření v skupinu NSG, použijte hello <i>Set-AzureNetworkSecurityRule</i> rutiny. |Komplexní toomanage jako hello dopad IP rozsahy změn v průběhu času.<br><br>Poskytuje přístup k celé toohello Azure a ne jenom úložiště. |
| Server proxy protokolu HTTP |Přesná kontrola proxy serveru hello nad úložištěm hello povolené adresy URL. přesná kontrola toosetup hello proxy serveru, https://\*.blob.core.windows.net/\* vzor adresy URL musí toobe seznam povolených adres. toowhitelist pouze hello účet úložiště používané hello virtuálních počítačů, https://\<storageAccount\>.blob.core.windows.net/\* vzor adresy URL musí toobe seznam povolených adres. <br>Jeden bod Internet tooVMs přístup.<br>Není subjektu tooAzure změny IP adresy. |Další náklady pro spuštění virtuálního počítače se hello proxy softwaru. |

### <a name="whitelist-hello-azure-datacenter-ip-ranges"></a>Povolit hello datové centrum Azure, rozsahy IP adres
rozsazích IP adres toowhitelist hello datové centrum Azure, najdete v tématu hello [webu Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653) podrobnosti o hello rozsahy IP adres a pokyny.

### <a name="using-an-http-proxy-for-vm-backups"></a>Pomocí proxy serveru HTTP pro zálohování virtuálních počítačů
Při zálohování virtuálních počítačů, rozšíření zálohování hello na hello virtuální počítač odešle hello snímku správu příkazy tooAzure Storage pomocí rozhraní API HTTPS. Směrování provozu rozšíření zálohování hello přes proxy server hello HTTP vzhledem k tomu, že je nakonfigurovaná pro přístup k toohello jedinou komponentou hello veřejného Internetu.

> [!NOTE]
> Neexistuje žádná doporučení pro hello proxy software, který se má použít. Ujistěte se, že vyberete proxy server, který má odchozí věrnosti a který je kompatibilní s hello provedením následujících kroků konfigurace. Ujistěte se, že softwary třetích stran neměňte nastavení proxy serveru hello
>
>

Následující obrázek příkladu Hello ukazuje hello tři konfigurační kroky potřebné toouse HTTP proxy:

* Virtuální počítač aplikace tras, které veškerý provoz protokolu HTTP vázaný hello veřejného Internetu prostřednictvím proxy serveru virtuálního počítače.
* Proxy virtuálních počítačů umožňuje příchozí provoz z virtuálních počítačů ve virtuální síti hello.
* Hello skupina zabezpečení sítě (NSG) s názvem NFP uzamčení musí zabezpečení pravidlo povolení odchozí internetové přenosy z virtuálního počítače proxy serveru.

![Skupina NSG s diagram nasazení proxy serveru HTTP](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

toouse HTTP proxy toocommunicating toohello veřejný Internet, postupujte takto:

#### <a name="step-1-configure-outgoing-network-connections"></a>Krok 1. Konfigurace odchozích síťových připojení
###### <a name="for-windows-machines"></a>Pro počítače s Windows
To se nastavit konfiguraci proxy serveru pro místní systémový účet.

1. Stáhněte si [nástroje PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Z řádku se zvýšenými oprávněními spusťte následující příkaz

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Otevře se okno internet Exploreru.
3. Přejděte tooTools -> Možnosti Internetu -> připojení -> Nastavení místní sítě.
4. Ověřte nastavení proxy serveru pro účet System. Nastavení proxy serveru IP adresy a portu.
5. Zavřete Internet Explorer.

To se nastavit konfiguraci proxy celého systému a se použije pro všechny odchozí přenosy HTTP/HTTPS.

Pokud jste nastavili proxy server na aktuální uživatelský účet (ne místní účet systému), použijte hello následující skript tooapply je tooSYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Pokud zjistíte v protokolu serveru proxy "(407) vyžadováno ověření proxy serveru", zkontrolujte, že ověřování je správně nastavená.
>
>

###### <a name="for-linux-machines"></a>Pro počítače se systémem Linux
Přidejte následující řádek toohello hello ```/etc/environment``` souboru:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Přidejte následující řádky toohello hello ```/etc/waagent.conf``` souboru:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-hello-proxy-server"></a>Krok 2. Povolit příchozí připojení na hello proxy serveru:
1. Na serveru proxy hello otevřete bránu Windows Firewall. Hello nejjednodušší způsob, jak tooaccess hello je brána firewall toosearch pro bránu Windows Firewall s pokročilým zabezpečením.

    ![Otevřete hello brány Firewall](./media/backup-azure-vms-prepare/firewall-01.png)
2. V dialogovém okně hello brány Windows Firewall, klikněte pravým tlačítkem na **příchozí pravidla** a klikněte na tlačítko **nové pravidlo...** .

    ![Vytvořit nové pravidlo](./media/backup-azure-vms-prepare/firewall-02.png)
3. V hello **pravidla Průvodce vytvořením nového příchozího**, zvolte hello **vlastní** možnost pro hello **typ pravidla** a klikněte na tlačítko **Další**.
4. Na hello tooselect stránku hello **programu**, zvolte **všechny programy** a klikněte na tlačítko **Další**.
5. Na hello **protokol a porty** , zadejte hello následující informace a klikněte na tlačítko **Další**:

    ![Vytvořit nové pravidlo](./media/backup-azure-vms-prepare/firewall-03.png)

   * pro *protokolu typ* zvolte *TCP*
   * pro *místního portu* zvolte *specifické porty*, hello následujícího pole zadejte hello ```<Proxy Port>``` který byl nakonfigurovaný.
   * pro *vzdálených portů* vyberte *všechny porty*

     Pro hello zbytek hello průvodce klikněte na možnost všechny hello způsob toohello end a pojmenujte toto pravidlo.

#### <a name="step-3-add-an-exception-rule-toohello-nsg"></a>Krok 3. Přidejte toohello výjimky pravidla NSG:
V příkazovém řádku prostředí Azure PowerShell zadejte následující příkaz hello:

Hello následující příkaz přidá k výjimce toohello NSG. Tato výjimka umožňuje přenos TCP z jakéhokoli portu na 10.0.0.5 tooany internetové adresy na portu 80 (HTTP) nebo 443 (HTTPS). Pokud budete potřebovat specifického portu v hello veřejný Internet, že tooadd, který port toohello být ```-DestinationPortRange``` také.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

*Ujistěte se, nahradit názvy hello v příkladu hello hello podrobnosti odpovídající tooyour nasazení.*

## <a name="vm-agent"></a>Agent virtuálního počítače
Předtím, než můžete zálohovat hello virtuální počítač Azure, měli byste zajistit, že tento agent virtuálního počítače Azure hello je správně nainstalovaná hello virtuálního počítače. Vzhledem k tomu, že agent virtuálního počítače hello je volitelná součást hello dobu, po kterou hello virtuální počítač vytvořen, ujistěte se, že hello zaškrtávací políčko pro agenta virtuálního počítače hello je vybrané před zřízení hello virtuálního počítače.

### <a name="manual-installation-and-update"></a>Ruční instalace a aktualizace
Hello agenta virtuálního počítače již existuje ve virtuálních počítačích, které jsou vytvořené pomocí hello Galerie Azure. Virtuální počítače, které jsou migrované z místních datových center by však být nainstalován agent virtuálního počítače hello. Pro tyto virtuální počítače musí agent virtuálního počítače hello toobe explicitně nainstalována. Další informace o [instalaci agenta virtuálního počítače hello na existující virtuální počítač](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace agenta virtuálního počítače hello |<li>Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Budete potřebovat instalaci hello toocomplete oprávnění správce. <li>[Aktualizovat vlastnosti virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná. |<li> Nainstalujte nejnovější hello [agenta systému Linux](https://github.com/Azure/WALinuxAgent) z Githubu. Budete potřebovat instalaci hello toocomplete oprávnění správce. <li> [Aktualizovat vlastnosti virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná. |
| Aktualizace agenta virtuálního počítače hello |Aktualizace agenta virtuálního počítače hello je jednoduché, přeinstalování hello [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Ujistěte se, že během aktualizace agenta virtuálního počítače hello neběží žádná operace zálohování. |Postupujte podle pokynů hello na [aktualizace hello agenta virtuálního počítače s Linuxem ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br><br>Ujistěte se, že během aktualizace agenta virtuálního počítače hello neběží žádná operace zálohování. |
| Ověření instalace agenta virtuálního počítače hello |<li>Přejděte toohello *C:\WindowsAzure\Packages* složky v hello virtuálního počítače Azure. <li>Byste měli najít přítomný soubor WaAppAgent.exe hello.<li> Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello verze produktu pole by mělo být 2.6.1198.718 nebo vyšší. |Není k dispozici |

Další informace o hello [agenta virtuálního počítače](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) a [jak tooinstall ho](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### <a name="backup-extension"></a>Rozšíření zálohování
tooback hello virtuálního počítače, hello služby Azure Backup nainstaluje agenta rozšíření toohello virtuálních počítačů. Hello služba Azure Backup bezproblémově upgraduje a opravy hello rozšíření zálohování bez dalšího zásahu uživatele.

Rozšíření zálohování Hello je nainstalován, pokud běží hello virtuálních počítačů. Spuštění virtuálního počítače poskytuje taky hello největší šanci získání bod obnovení konzistentních s aplikací. Ale hello Azure Backup service bude pokračovat tooback až hello virtuálního počítače –, i když je vypnutý a rozšíření hello nebylo možné nainstalovat (neboli Offline virtuálního počítače). V takovém případě bude bod obnovení hello *zhroutí konzistentní* jak je popsáno výše.

## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
Teď připravíte prostředí pro zálohování virtuálního počítače, je dalším krokem logické toocreate zálohu. Hello plánování článek poskytuje podrobnější informace o zálohování virtuálních počítačů.

* [Zálohování virtuálních počítačů](backup-azure-vms.md)
* [Plánování vaší infrastruktury zálohování virtuálních počítačů](backup-azure-vms-introduction.md)
* [Správa záloh virtuálních počítačů](backup-azure-manage-vms.md)
