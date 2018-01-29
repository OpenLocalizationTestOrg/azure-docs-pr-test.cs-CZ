---
title: " Spravovat konfigurační server pro obnovení po havárii VMware s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak spravovat existující konfigurační server pro zotavení po havárii VMware do Azure, se službou Azure Site Recovery."
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2017
ms.author: anoopkv
ms.openlocfilehash: e9e4bfc86df2cae1facac62472c915d91fb8c84c
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="manage-the-configuration-server"></a>Správa konfiguračního serveru

Můžete nastavení konfigurace serveru místní, při použití [Azure Site Recovery](site-recovery-overview.md) služby pro zotavení po havárii virtuálních počítačů VMware a fyzické servery do Azure. Konfigurační server koordinuje komunikaci mezi místní VMware a Azure a spravuje replikaci dat. Tento článek shrnuje běžné úlohy správy konfigurační server po je nasazený.

## <a name="modify-vmware-settings"></a>Upravit nastavení VMware

Upravte nastavení pro server VMware, ke kterému se připojí konfigurační server.

1. Přihlaste se počítač se systémem konfigurační server.
2. Spuštění nástroje Azure Site Recovery Configuration Manager ze zástupce na ploše. Můžete také otevřít **https://configuration-server-name/IP:44315**.
3. Klikněte na tlačítko **spravovat server ESXi Server vSPhere vCenter**:
    - Chcete-li přidružit jiný server VMware konfigurační server, klikněte na tlačítko **server přidat vCenter Server vSphere ESXi**a zadejte podrobnosti serveru.
    - Chcete-li aktualizovat pověření použitá k připojení k serveru VMware pro automatické zjišťování virtuálních počítačů VMware, klikněte na tlačítko **upravit**. Zadejte nové přihlašovací údaje a pak klikněte na tlačítko **OK**.

        ![Upravit VMware](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Upravit přihlašovací údaje pro instalaci služby Mobility

Změňte pověření používaná k automatické instalaci služby Mobility na virtuální počítače VMware, povolíte pro replikaci.

1. Přihlaste se počítač se systémem konfigurační server.
2. Spuštění nástroje Azure Site Recovery Configuration Manager ze zástupce na ploše. Můžete také otevřít **https://configuration-server-name/IP:44315**.
3. Klikněte na tlačítko **spravovat virtuální počítač pověření**a zadejte nové přihlašovací údaje. Pak klikněte na tlačítko **OK** se aktualizovat nastavení.

    ![Upravit přihlašovací údaje služby Mobility](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Upravte nastavení proxy serveru

Upravte nastavení proxy serveru používá počítačem konfigurace serveru pro přístup k Internetu do Azure. Pokud máte server další proces počítač kromě výchozí proces serveru spuštěna v počítači serveru konfigurace, změňte nastavení na obou počítačích.

1. Přihlaste se počítač se systémem konfigurační server.
2. Spuštění nástroje Azure Site Recovery Configuration Manager ze zástupce na ploše. Můžete také otevřít **https://configuration-server-name/IP:44315**.
3. Klikněte na tlačítko **spravovat připojení**a aktualizujte hodnoty proxy. Pak klikněte na tlačítko **Uložit** se aktualizovat nastavení.

## <a name="add-a-network-adapter"></a>Přidání síťového adaptéru

Šablona OVF nasadí konfigurační server virtuálních počítačů s jedním síťovým adaptérem. Můžete [přidat další adaptér k virtuálnímu počítači)](how-to-deploy-configuration-server.md#add-an-additional-adapter), ale je potřeba to udělat předtím, než zaregistrujete konfigurační server v trezoru.

Pokud potřebujete přidat adaptér po jste registrováni konfigurační server v trezoru, budete muset přidat adaptér ve vlastnostech virtuálního počítače a pak zaregistrujte server v trezoru.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Znovu zaregistrujte konfigurační server v ke stejnému trezoru

Pokud potřebujete, můžete zaregistrujte server konfigurace v ke stejnému trezoru. f máte server další proces počítač kromě výchozí proces serveru spuštěna v počítači konfigurace serveru, znovu zaregistrujte obou počítačů.

  1. V úložišti, otevřete **spravovat** > **infrastruktura Site Recovery** > **konfigurační servery**.
  2. V **servery**, klikněte na tlačítko **stáhnout registrační klíč**. Tím se stáhne soubor s přihlašovacími údaji.
  3. Přihlaste se k počítači konfigurace serveru.
  4. V **%ProgramData%\ASR\home\svagent\bin**, otevřete **cspsconfigtool.exe**.
  5. Na **registrace trezoru** kartě, klikněte na tlačítko Procházet a umístěný soubor s přihlašovacími údaji jste si stáhli.
  6. V případě potřeby zadejte podrobnosti o serveru proxy. Pak klikněte na tlačítko **zaregistrovat**.
  7. Otevřete okno příkazového prostředí PowerShell pro správce a spusťte následující příkaz:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="delete-or-unregister-a-configuration-server"></a>Odstranit nebo zrušit registraci konfigurační server

1. Zakázat [zakažte](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) pro všechny virtuální počítače v rámci konfigurace serveru.
2. [Zrušit přidružení](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) a [odstranit](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) všechny zásady replikace z konfiguračního serveru.
3. [Odstranit](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) všichni hostitelé Vcenter vSphere servery, které jsou spojeny s konfiguračním serverem.
4. V trezoru, otevřete **infrastruktura Site Recovery** > **konfigurační servery**
5. Klikněte na možnost konfigurace serveru, který chcete odebrat. Potom na **podrobnosti** klikněte na tlačítko **odstranit**.

    ![Odstraňte konfigurační server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Odstranit pomocí prostředí PowerShell

Volitelně můžete odstranit konfigurační server pomocí prostředí PowerShell:

1. [Nainstalujte](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) modul Azure PowerShell
2. Přihlaste se k účtu Azure pomocí příkazu:
    
    `Login-AzureRmAccount`
3. Vyberte předplatné trezoru:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Nastavte kontext úložiště:
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Načtěte konfigurační server:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Odstraňte konfigurační server:

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Můžete použít **-Force** v AzureRmSiteRecoveryFabric odebrat možnost pro Vynucené odstranění konfigurační server.
 


## <a name="renew-ssl-certificates"></a>Obnovení certifikátů SSL

Konfigurační server má integrované webový server, která orchestruje činnosti služby Mobility, proces servery a hlavních cílových serverů, které jsou k němu připojená. Webový server používá certifikát SSL k ověřování klientů. Platnost certifikátu vyprší po tři roky a můžete obnovit kdykoli.

### <a name="check-expiry"></a>Kontrola vypršení platnosti

Pro nasazení serveru konfigurace před může 2016 vypršení platnosti certifikátu byla nastavena na jeden rok. Pokud máte certifikát se blíží uplynutí platnosti, dojde k následujícímu:

- Když datum vypršení platnosti je dvou měsíců nebo méně, spuštění služby odesílání oznámení na portálu a e-mailem (Pokud se přihlášení k odběru oznámení Azure Site Recovery).
- Na stránce prostředků úložiště se zobrazí nápis informující o oznámení. Klikněte na tlačítko Banner informující o další podrobnosti.
- Pokud se zobrazí **upgradovat nyní** tlačítko, to znamená, že se některé součásti ve vašem prostředí, které nebyly aktualizovány na verzi 9.4.xxxx.x nebo vyšší verze. Upgrade součástí než obnovení certifikátu. Nelze obnovit na starší verze.

### <a name="renew-the-certificate"></a>Obnovení certifikátu

1. V úložišti, otevřete **infrastruktura Site Recovery** > **konfigurační Server**a klikněte na požadované konfigurační server.
2. Datum vypršení platnosti se zobrazí pod **stav konfigurace serveru**
3. Klikněte na tlačítko **obnovení certifikátů**. 


## <a name="next-steps"></a>Další postup

Přečtěte si podrobné pokyny pro nastavení zotavení po havárii [virtuální počítače VMware](tutorial-vmware-to-azure.md) a fyzické servers(tutorial-physical-to-azure.md) do Azure.
