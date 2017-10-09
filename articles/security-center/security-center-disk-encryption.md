---
title: "Virtuální počítač Azure aaaEncrypt | Microsoft Docs"
description: "Tento dokument vám pomůže tooencrypt virtuální počítač Azure po přijetí oznámení z Azure Security Center."
services: security, security-center
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: 
ms.assetid: f6c28bc4-1f79-4352-89d0-03659b2fa2f5
ms.service: security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2017
ms.author: tomsh
ms.openlocfilehash: 7c7c6eed39d16bde8a0dfaffe3a3331c58101634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-an-azure-virtual-machine"></a>Šifrování virtuálního počítače Azure
Azure Security Center vás upozorní, pokud máte virtuální počítače, které nejsou šifrované. Tyto výstrahy se zobrazují jako vysokou závažností a hello doporučení je tooencrypt těchto virtuálních počítačů.

![Doporučení pro šifrování disku](./media/security-center-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Hello informace v tomto dokumentu se vztahuje tooencrypting virtuální počítače bez použití šifrování klíče, (které jsou nezbytné pro zálohování virtuálních počítačů pomocí služby Azure Backup). Prosím najdete v článku hello [Azure Disk Encryption pro systém Windows a Linux Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) informace o tom toouse klíč šifrovacího klíče toosupport Azure Backup pro šifrované virtuální počítače Azure.
>
>

tooencrypt virtuální počítače Azure, které byly identifikovány pomocí služby Azure Security Center potřebují šifrování, doporučujeme hello následující kroky:

* Instalace a konfigurace Azure Powershellu. To vám umožní toorun hello prostředí PowerShell příkazy požadované tooset až hello požadavky požadované tooencrypt virtuální počítače Azure.
* Získat a spustit skript prostředí Azure PowerShell Azure Disk Encryption požadavky hello
* Šifrování virtuálních počítačů

Hello cílem tohoto dokumentu je tooenable tooencrypt můžete virtuální počítače, i v případě, že máte v prostředí Azure PowerShell nebo téměř žádné zkušenosti.
Tento dokument předpokládá, že používáte Windows 10 jako hello klientský počítač, ze kterého budete konfigurovat Azure Disk Encryption.

Existuje mnoho přístupů, které se dají použít toosetup hello požadavky a tooconfigure šifrování pro virtuální počítače Azure. Pokud jste už dobře versed v prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure, dáte možná přednost alternativním přístupům toouse.

> [!NOTE]
> toolearn Další informace o alternativních přístupech tooconfiguring šifrování pro virtuální počítače Azure, najdete v tématu [Azure Disk Encryption pro systém Windows a Linux Azure Virtual Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).
>
>

## <a name="install-and-configure-azure-powershell"></a>Instalace a konfigurace Azure Powershellu
Budete potřebovat, aby na vašem počítači byl nainstalovaný Azure PowerShell verze 1.2.1 nebo novější. článek Hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) obsahuje všechny kroky hello potřebujete tooprovision toowork váš počítač s prostředím Azure PowerShell. Hello nejjednodušší je toouse hello instalaci Web PI uvedenou v tomto článku. I když už máte Azure PowerShell nainstalovaný, nainstalujte znovu pomocí instalace webové platformy přístup hello tak, že máte nejnovější verzi prostředí Azure PowerShell hello.

## <a name="obtain-and-run-hello-azure-disk-encryption-prerequisites-configuration-script"></a>Získání a spuštění hello Azure disk encryption požadavky konfigurační skript
Hello konfigurační skript požadovaných součástí pro Azure Disk Encryption nastaví všechny hello nezbytné součásti potřebné pro šifrování Azure Virtual Machines.

1. Přejděte toohello GitHub stránce, která má hello [skript Azure Disk Encryption požadovaných součástí instalace](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
2. Na stránce Gibhubu hello klikněte hello **Raw** tlačítko.
3. Použít **CTRL-A** tooselect všechny hello text na stránce hello a pak použijte **CTRL-C** toocopy všechny hello textu hello stránky toohello schránky.
4. Otevřete **Poznámkový blok** a vložte hello zkopírovat text do poznámkového bloku.
5. Na jednotce C: vytvořte novou složku s názvem **AzureADEScript**.
6. Uložte soubor poznámkového bloku hello – klikněte na tlačítko **soubor**, pak klikněte na tlačítko **uložit jako**. Do textového pole název souboru hello, zadejte **"ADEPrereqScript.ps1"** a klikněte na tlačítko **Uložit**. (Nezapomeňte uvést název hello hello uvozovky, jinak bude ukládat hello soubor s příponou .txt souboru).

Teď, když je hello obsah skriptu uložený, otevřete v hello PowerShell ISE hello skriptu:

1. V nabídce Start hello, klikněte na **Cortana**. Požádejte **Cortana** "PowerShell" zadáním **prostředí PowerShell** hello Cortana vyhledávání textového pole.
2. Klikněte pravým tlačítkem na **Integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell** a klikněte na **Spustit jako správce**.
3. V hello **správce: Windows PowerShell ISE** okně klikněte na tlačítko **zobrazení** a pak klikněte na **zobrazit podokno skriptu**.
4. Pokud se zobrazí hello **příkazy** podokně na pravé straně hello hello okna, klikněte na tlačítko hello **"x"** v hello pravém horním rohu hello podokně tooclose ho. Pokud je příliš malá pro toosee jste hello text, použijte **CTRL + Add** ("Přidat" je hello "+" přihlášení). Pokud hello text je příliš velký, použijte **CTRL + Subtract** (Subtract je hello "-" přihlášení).
5. Klikněte na **Soubor** a potom klikněte na **Otevřete**. Přejděte toohello **C:\AzureADEScript** složku a hello dvakrát klikněte na hello **ADEPrereqScript**.
6. Hello **ADEPrereqScript** obsahu by se měla zobrazit v hello PowerShell ISE a je barevně toohelp snadněji najdete různé komponenty, například příkazy, parametry a proměnné.

Měli byste vidět něco podobného jako na následujícím obrázku hello.

![Okno integrovaného skriptovacího prostředí (ISE) v prostředí PowerShell](./media/security-center-disk-encryption/security-center-disk-encryption-fig2.png)

horní podokno Hello je odkazované tooas hello "podokno skriptu" a hello dolní podokno je odkazované tooas hello "console". Tyto termíny využijeme dále v tomto článku.

## <a name="run-hello-azure-disk-encryption-prerequisites-powershell-command"></a>Spusťte příkaz prostředí PowerShell hello Azure disk encryption požadavky
Hello Azure Disk Encryption požadavky skript zobrazí výzvu k hello po spuštění skriptu hello následující informace:

* **Název skupiny prostředků** – název z hello skupinu prostředků, které chcete tooput hello Key Vault do.  Novou skupinu prostředků s názvem hello, kterou zadáte bude vytvořen, pokud není k dispozici již s tímto názvem vytvořen. Pokud už máte skupinu prostředků, které chcete toouse v tomto předplatném, zadejte název hello této skupiny prostředků.
* **Název pro Key Vault** -název hello Key Vault, ve které šifrovací klíče jsou toobe umístit. Pokud služba Key Vault se zadaným názvem neexistuje, vytvoří se nová. Pokud jste již máte službu Key Vault, které chcete toouse, zadejte název hello hello existující Key Vault.
* **Umístění** -umístění hello Key Vault. Zkontrolujte, zda jsou hello Key Vault a virtuální počítače toobe šifrované v hello stejné umístění. Pokud si nejste jisti hello umístění, jsou dále v tomto článku, který vám ukáže, jak kroky toofind limitu.
* **Azure Active Directory název_aplikace** -název hello aplikaci Azure Active Directory, které se bude toohello tajné klíče používané toowrite Key Vault. Pokud aplikace se zadaným názvem neexistuje, vytvoří se nová. Pokud již máte aplikaci Azure Active Directory, které chcete toouse, zadejte název hello této aplikace Azure Active Directory.

> [!NOTE]
> Pokud jste zvědaví jako toowhy potřebujete toocreate aplikaci Azure Active Directory najdete v tématu *zaregistrovat aplikaci s Azure Active Directory* v článku hello [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).
>
>

Proveďte následující kroky tooencrypt virtuální počítač Azure hello:

1. Pokud jste zavřeli hello PowerShell ISE, otevřete instanci hello PowerShell ISE se zvýšenými oprávněními. Pokud otevřete hello PowerShell ISE není již, postupujte podle pokynů hello dříve v tomto článku. Pokud jste zavřeli hello skriptu, otevřete hello **ADEPrereqScript.ps1** kliknutím na tlačítko **soubor**, pak **otevřete** a vyberete hello skriptu z hello **c:\ AzureADEScript** složky. Pokud jste tento článek procházeli od začátku hello, přejděte na další krok toohello.
2. V konzole hello hello PowerShell ISE (hello dolní podokno hello PowerShell ISE), změňte hello fokus toohello místní hello skriptu zadáním **cd c:\AzureADEScript** a stiskněte klávesu **ENTER**.
3. Nastavte zásady spouštění hello na počítači, aby můžete spustit skript hello. Typ **Set-ExecutionPolicy Unrestricted** v hello konzoly a stiskněte klávesu ENTER. Pokud se zobrazí dialogové okno s informacemi o hello účinky zásad tooexecution hello změn, klikněte na možnost **Ano tooall** nebo **Ano** (Pokud se zobrazí **Ano tooall**, vyberte tuto možnost – Pokud nevidíte **Ano tooall**, pak klikněte na tlačítko **Ano**).
4. Přihlaste se ke svému účtu Azure. V konzole hello zadejte **Login-AzureRmAccount** a stiskněte klávesu **ENTER**. Dialogové okno se zobrazí při zadávání přihlašovacích údajů (ujistěte se, máte práva toochange hello virtuální počítače – Pokud nemáte práva, nebudete moct tooencrypt je. Pokud si nejste jisti, zeptejte se správce nebo vlastníka předplatného.) Měly by se zobrazit tyto informace: **prostředí**, **účet**, **ID tenanta**, **ID předplatného** a **aktuální účet úložiště**. Kopírování hello **SubscriptionId** tooNotepad. Budete potřebovat toouse v kroku #6.
5. Jaké předplatné najít virtuálního počítače patří tooand jeho umístění. Přejděte příliš[https://portal.azure.com](ttps://portal.azure.com) a přihlaste se.  Na levé straně stránky hello hello, klikněte na tlačítko **virtuální počítače**. Zobrazí se seznam virtuálních počítačů a předplatná hello, ke které patří.

   ![Virtuální počítače](./media/security-center-disk-encryption/security-center-disk-encryption-fig3.png)
6. Vrátí toohello prostředí PowerShell ISE. Nastavte kontext předplatného hello, ve kterém se spustí skript hello. V konzole hello zadejte **Select-AzureRmSubscription – SubscriptionId < id_předplatného >** (Nahraďte **< id_předplatného >** s aktuální ID předplatného) a stiskněte klávesu  **Zadejte**. Zobrazí se informace o hello prostředí **účet**, **TenantId**, **SubscriptionId** a **aktuální účet úložiště**.
7. Nyní je připraven toorun hello skriptu. Klikněte na tlačítko hello **spustit skript** tlačítko nebo klikněte na tlačítko **F5** na hello klávesnice.

   ![Spuštění powershellového skriptu](./media/security-center-disk-encryption/security-center-disk-encryption-fig4.png)
8. Hello skript vyzve k zadání **resourceGroupName:** – zadejte název hello *skupiny prostředků* chcete toouse, stiskněte **ENTER**. Pokud nemáte, zadejte název pro novou chcete toouse. Pokud již máte *skupiny prostředků* má toouse (například hello, virtuální počítač je v, jeden), zadejte název hello hello existující skupinu prostředků.
9. Hello skript vyzve k zadání **keyVaultName:** – zadejte název hello hello *Key Vault* toouse a pak stiskněte klávesu ENTER. Pokud nemáte, zadejte název pro novou chcete toouse. Pokud jste již máte službu Key Vault, které chcete toouse, zadejte název hello hello existující *Key Vault*.
10. Hello skript vyzve k zadání **umístění:** – zadejte název hello hello umístění, ve které hello tooencrypt chcete virtuální počítač se nachází, a stiskněte **ENTER**. Pokud jste hello umístění nepamatujete, přejděte zpátky toostep #. 5.
11. Hello skript vyzve k zadání **aadAppName:** – zadejte název hello hello *Azure Active Directory* aplikace, které chcete toouse, stiskněte **ENTER**. Pokud nemáte, zadejte název pro novou chcete toouse. Pokud už máte *aplikaci Azure Active Directory* má toouse, zadejte název hello hello existující *aplikaci Azure Active Directory*.
12. Zobrazí se přihlašovací dialogové okno. Zadejte svoje přihlašovací údaje (Ano, jste byli přihlášeni jednou, ale nyní je třeba toodo ho znovu).
13. Hello skript se spustí a po dokončení se zobrazí dotaz, toocopy hello hodnoty hello **aadClientID**, **aadClientSecret**, **diskEncryptionKeyVaultUrl**a **keyVaultResourceId**. Každý z těchto hodnot toohello schránky zkopírujte a vložte je do poznámkového bloku.
14. Vrátí toohello prostředí PowerShell ISE a umístěte kurzor hello na konci hello hello posledního řádku a stiskněte klávesu **ENTER**.

výstup Hello hello skriptu by měl vypadat podobně jako úvodní obrazovka níže:

![Výstup prostředí PowerShell](./media/security-center-disk-encryption/security-center-disk-encryption-fig5.png)

## <a name="encrypt-hello-azure-virtual-machine"></a>Šifrování hello virtuální počítač Azure
Můžete je nyní připraven tooencrypt virtuálního počítače. Pokud virtuální počítač nachází ve stejné skupině prostředků jako služba Key Vault, hello, můžete přesunout na toohello šifrování krokům. Ale pokud virtuální počítač není v hello stejné skupina prostředků jako služba Key Vault, budete potřebovat následující hello tooenter v konzole hello v hello PowerShell ISE:

**$resourceGroupName = &lt;’skupina_prostředků_virtuálního_počítače’&gt;**

Nahraďte **< ' skupina_prostředků_virtuálního_počítače ' >** s hello název skupiny prostředků hello, ve kterém jsou obsaženy virtuálních počítačů, obklopená v jednoduchých uvozovkách. Potom stiskněte **ENTER**.
tooconfirm, který Dobrý den správné, že byl zadán název skupiny prostředků, zadejte následující hello hello konzole PowerShell ISE:

**$resourceGroupName**

Stiskněte **ENTER**. Měli byste vidět hello název skupiny prostředků, které jsou umístěné vaše virtuální počítače v. Například:

![Výstup prostředí PowerShell](./media/security-center-disk-encryption/security-center-disk-encryption-fig6.png)

### <a name="encryption-steps"></a>Kroky pro šifrování
Nejprve je třeba název hello prostředí PowerShell tootell hello virtuálního počítače chcete tooencrypt. V konzole hello zadejte:

**$vmName = &lt;’název_virtuálního_počítače’&gt;**

Nahraďte **<' název_virtuálního_počítače' >** s hello název vašeho virtuálního počítače (ujistěte se, název hello mezi v jednoduchých uvozovkách) a potom stiskněte klávesu **ENTER**.

tooconfirm, který Dobrý den správné, že byl zadán název virtuálního počítače, zadejte:

**$vmName**

Stiskněte **ENTER**. Měli byste vidět název hello hello virtuálního počítače chcete tooencrypt. Například:

![Výstup prostředí PowerShell](./media/security-center-disk-encryption/security-center-disk-encryption-fig7.png)

Existují dvě metody toorun hello šifrování příkaz tooencrypt všechny disky na virtuálním počítači hello. První metoda Hello je tootype hello následující příkaz v konzole PowerShell ISE hello:

~~~
Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
~~~

Po zadání tohoto příkazu stiskněte **ENTER**.

Druhá metoda Hello je tooclick v podokně skriptu hello (hello horní podokno hello PowerShell ISE) a přejděte dolů toohello dolní hello skriptu. Zvýrazněte příkaz hello uvedený výše a pak klikněte pravým tlačítkem ji a pak klikněte na tlačítko **spustit výběr** nebo stiskněte klávesu **F8** na hello klávesnice.

![Integrované skriptovací prostředí (ISE) v prostředí PowerShell](./media/security-center-disk-encryption/security-center-disk-encryption-fig8.png)

Bez ohledu na to hello metodu použijete, dialogové okno se zobrazí upozorněním, že bude trvat 10 až 15 minut pro operaci toocomplete hello. Klikněte na **Ano**.

Během procesu šifrování hello probíhající, můžete vrátit toohello portálu Azure a zobrazit stav hello hello virtuálního počítače. Na levé straně stránky hello hello, klikněte na tlačítko **virtuální počítače**, potom v hello **virtuální počítače** okně klikněte na název hello hello virtuálního počítače, který šifrujete. V okně hello, který se zobrazí, můžete si všimnout, že hello **stav** informacemi o tom, že je **aktualizace**. To znamená, že probíhá šifrování.

![Další podrobnosti o hello virtuálních počítačů](./media/security-center-disk-encryption/security-center-disk-encryption-fig9.png)

Vrátí toohello prostředí PowerShell ISE. Po dokončení skriptu hello uvidíte, co se zobrazí v hello obrázek.

![Výstup prostředí PowerShell](./media/security-center-disk-encryption/security-center-disk-encryption-fig10.png)

toodemonstrate, který hello virtuální počítač je zašifrovaný, vraťte toohello portálu Azure a klikněte na tlačítko **virtuální počítače** na levé straně stránky hello hello. Klikněte na název hello hello virtuálního počítače, který jste zašifrovali. V hello **nastavení** okně klikněte na tlačítko **disky**.

![Možnosti nastavení](./media/security-center-disk-encryption/security-center-disk-encryption-fig11.png)

Na hello **disky** okno, zobrazí se **šifrování** je **povoleno**.

![Vlastnosti disku](./media/security-center-disk-encryption/security-center-disk-encryption-fig12.png)

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste se naučili jak tooencrypt virtuální počítač Azure. toolearn Další informace o službě Azure Security Center, najdete v části hello následující:

* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
