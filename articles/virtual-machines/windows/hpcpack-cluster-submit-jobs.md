---
title: "tooan aaaSubmit úlohy HPC Pack clusteru v Azure | Microsoft Docs"
description: "Zjistěte, jak tooset až místní počítač toosubmit úlohy clusteru HPC Pack tooan v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 2918cf633917d8730487152e6a5ddb863eb8bb5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-tooan-hpc-pack-cluster-deployed-in-azure"></a>Odeslání úlohy HPC z clusteru HPC Pack tooan počítači místní nasazené v Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Konfigurace místní klientský počítač toosubmit úlohy tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) clusteru v Azure. Tento článek ukazuje, jak tooset místního počítače s klientem nástroje toosubmit úlohy přes HTTPS toohello clusteru v Azure. Tímto způsobem můžete několika clusteru uživatelům odeslat úlohy tooa cloudové HPC Pack clusteru, ale bez připojení přímo toohello hlavního uzlu virtuálního počítače nebo přístup k předplatnému Azure.

![Odeslání úlohy clusteru tooa v Azure][jobsubmit]

## <a name="prerequisites"></a>Požadavky
* **Nasadit virtuální počítač Azure hlavního uzlu HPC Pack** -doporučujeme použít automatizované nástroje, jako [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) nebo [skript prostředí Azure PowerShell](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello hlavního uzlu a cluster. Potřebujete název DNS hello hello hlavního uzlu a hello přihlašovací údaje Správce clusteru dokončit hello kroky v tomto článku.
* **Klientský počítač** -potřebujete klientského počítače Windows nebo Windows Server, který můžete spustit HPC Pack klienta nástroje (viz [požadavky na systém](https://technet.microsoft.com/library/dn535781.aspx)). Pokud chcete pouze toouse hello HPC Pack webový portál nebo REST API toosubmit úloh, můžete použít libovolného klientského počítače podle svého výběru.
* **HPC Pack instalačním médiu** -tooinstall hello HPC Pack klienta nástroje, hello volné instalační balíček pro nejnovější verzi sady HPC Pack (HPC Pack 2012 R2) je k dispozici z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Ujistěte se, že si stáhnout hello stejnou verzi HPC Pack, který je nainstalován na hello hlavního uzlu virtuálního počítače.

## <a name="step-1-install-and-configure-hello-web-components-on-hello-head-node"></a>Krok 1: Instalace a konfigurace webové komponenty hello hello hlavního uzlu
tooenable cluster REST rozhraní toosubmit úlohy toohello přes protokol HTTPS, nakonfigurujte hello HPC Pack webové komponenty hlavního uzlu HPC Pack hello. Pokud již nejsou nainstalovány, nejprve nainstalujte hello webové komponenty spuštěním hello HpcWebComponents.msi instalační soubor. Potom nakonfigurujte hello součásti spuštěním skriptu prostředí HPC PowerShell hello **Set-HPCWebComponents.ps1**.

Podrobné postupy najdete v tématu [nainstalovat hello Microsoft HPC Pack webové komponenty](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> Některé šablony Azure rychlý start pro HPC Pack nainstalujte a nakonfigurujte hello webové komponenty automaticky. Pokud používáte hello [skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello clusteru, můžete volitelně nainstalovat a nakonfigurovat hello webové komponenty jako součást nasazení hello.
> 
> 

**tooinstall hello webové komponenty**

1. Připojte toohello hlavního uzlu virtuálního počítače pomocí hello přihlašovací údaje Správce clusteru.
2. Ze složky, instalace sady HPC Pack hello spusťte HpcWebComponents.msi hello hlavního uzlu.
3. Postupujte podle kroků hello v hello Průvodce tooinstall hello webové komponenty

**tooconfigure hello webové komponenty**

1. Hello hlavního uzlu spusťte prostředí HPC PowerShell jako správce.
2. toochange directory toohello umístění hello konfigurační skript hello zadejte následující příkaz:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. tooconfigure hello rozhraní REST a spusťte hello HPC webové služby, typ hello následující příkaz:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. Když výzvami tooselect certifikát, zvolte hello certifikát, který odpovídá toohello veřejný název DNS hello hlavního uzlu. Například, pokud nasazujete hello hlavního uzlu virtuálního počítače pomocí modelu nasazení classic hello, název certifikátu hello vypadá CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Pokud používáte model nasazení Resource Manager hello, název certifikátu hello vypadá CN =&lt;*HeadNodeDnsName*&gt;.&lt; *oblast*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > Můžete vybrat tento certifikát později při odesílání úlohy toohello hlavního uzlu z místního počítače. Nevyberete ani nakonfigurovat certifikát, který odpovídá názvu počítače toohello hello hlavního uzlu v doméně služby Active Directory hello (například CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
5. tooconfigure hello webový portál pro úlohu odeslání, hello zadejte následující příkaz:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Po dokončení skriptu hello, zastavte a restartujte hello služba Plánovač úloh HPC zadáním hello následující příkazy:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-hello-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Krok 2: Instalace na místním počítači hello HPC Pack klienta nástroje
Pokud chcete tooinstall hello HPC Pack klienta nástroje ve vašem počítači, stáhnout instalační soubory HPC Pack (úplná instalace) z hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Když zahájíte instalaci hello, zvolte možnost instalace hello pro hello **HPC Pack klienta nástroje**.

nástroje toouse hello HPC Pack klienta toosubmit úlohy toohello hlavního uzlu virtuálního počítače, můžete také potřebovat tooexport certifikát z hlavního uzlu hello a nainstalovat na klientský počítač hello. Hello certifikát musí být v. Formátu CER.

**certifikát hello tooexport z hlavního uzlu hello**

1. Hello hlavního uzlu přidejte hello certifikátů modulu snap-in tooa konzoly Microsoft Management Console pro hello účet místního počítače. Postup modul snap-in hello tooadd najdete v tématu [přidat hello modul Snap-in Certifikáty tooan konzoly MMC](https://technet.microsoft.com/library/cc754431.aspx).
2. Rozbalte ve stromu konzoly hello **certifikáty – místní** > **osobní**a potom klikněte na **certifikáty**.
3. Vyhledejte hello certifikát, který jste nakonfigurovali pro hello HPC Pack webové komponenty v [krok 1: instalace a konfigurace webové komponenty hello hlavního uzlu hello](#step-1:-install-and-configure-the-web-components-on-the-head-node) (například CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
4. Klikněte pravým tlačítkem na certifikát hello a klikněte na tlačítko **všechny úlohy** > **exportovat**.
5. V Průvodci exportem certifikátu hello, klikněte na **Další**a ujistěte se, že **Ne, neexportovat privátní klíč hello** je vybrána.
6. Binární X.509, kódování hello postupujte podle zbývajících kroků hello Průvodce tooexport hello certifikátu v kódování DER (. Formátu CER).

**tooimport hello certifikát na klientském počítači hello**

1. Zkopírujte hello certifikát, který jste exportovali ze složky tooa hello hlavního uzlu na klientském počítači hello.
2. Na klientském počítači hello spusťte certmgr.msc.
3. Ve Správci certifikátů rozbalte **certifikáty – aktuální uživatel** > **důvěryhodné kořenové certifikační autority**, klikněte pravým tlačítkem na **certifikáty**a potom klikněte na **všechny úlohy** > **Import**.
4. V hello Průvodce importem certifikátu, klikněte na **Další** a postupujte podle hello kroky tooimport hello certifikát, který jste exportovali z toohello hello hlavního uzlu úložiště důvěryhodných kořenových certifikačních autorit.

> [!TIP]
> Může se zobrazit upozornění zabezpečení, protože hello certifikační autority hlavního uzlu hello nerozpoznají hello klientský počítač. Pro účely testování můžete ignorovat, toto upozornění a dokončení importu certifikátu hello.
> 
> 

## <a name="step-3-run-test-jobs-on-hello-cluster"></a>Krok 3: Spuštění testu úloh na clusteru hello
tooverify konfiguraci, zkuste spuštěné úlohy na hello cluster v Azure z hello místní počítač. Můžete například použít HPC Pack grafického uživatelského rozhraní nástroje nebo příkazy příkazového řádku toosubmit úlohy toohello clusteru. Můžete taky založené na webu portálu toosubmit úlohy.

**toorun úlohy odeslání příkazy na klientském počítači hello**

1. Na klientském počítači, kde jsou nainstalovány nástroje klienta HPC Pack hello spusťte příkazový řádek.
2. Zadejte příkaz Ukázka. Například toolist všechny úlohy v hello clusteru zadejte podobné tooone příkaz Dobrý den, v závislosti na úplný název DNS hello hello hlavního uzlu následující:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    nebo
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Úplný název DNS hello hello hlavního uzlu, ne hello IP adresy, použijte v adrese URL hello plánovače. Pokud zadáte hello IP adresu, objeví se chyba podobné příliš "hello certifikát serveru musí tooeither mít platný řetěz důvěryhodnosti nebo umístit do úložiště důvěryhodných kořenových hello toobe."
   > 
   > 
3. Po zobrazení výzvy zadejte uživatelské jméno hello (v podobě hello &lt;DomainName&gt;\\&lt;uživatelské jméno&gt;) a heslo správce clusteru HPC hello nebo jiný uživatel clusteru, který jste nakonfigurovali. Můžete toostore hello pověření místně pro další operace úlohy.
   
    Zobrazí se seznam úloh.

**na klientském počítači hello toouse Správce úloh HPC**

1. Pokud při odesílání úlohy nebyla dříve ukládat přihlašovací údaje domény pro uživatele clusteru, můžete přidat přihlašovací údaje hello do správce přihlašovacích údajů.
   
    a. V Ovládacích panelech na hello klientského počítače spusťte Správce přihlašovacích údajů.
   
    b. Klikněte na tlačítko **pověření systému Windows** > **přidat obecné přihlašovací údaje**.
   
    c. Zadejte hello internetovou adresu (například https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler nebo https://&lt;HeadNodeDnsName&gt;.&lt; oblast&gt;.cloudapp.azure.com/HpcScheduler) a hello uživatelské jméno (&lt;DomainName&gt;\\&lt;uživatelské jméno&gt;) a heslo správce clusteru hello nebo jiné uživatel clusteru, který jste nakonfigurovali.
2. Na klientském počítači hello spusťte Správce úloh HPC.
3. V hello **vyberte hlavní uzel** dialogové okno, typ hello URL toohello hlavního uzlu v Azure (například https://&lt;HeadNodeDnsName&gt;. cloudapp.net nebo https://&lt;HeadNodeDnsName&gt;. &lt;oblast&gt;. cloudapp.azure.com).
   
    Správce úloh HPC otevře a zobrazí se seznam úloh hello hlavního uzlu.

**toouse hello webový portál s hello hlavního uzlu**

1. Spustit webový prohlížeč na klientském počítači hello a zadáním jednoho z následujících adresy, v závislosti na úplný název DNS hello hlavního uzlu hello hello:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    nebo
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Hello zabezpečení zobrazeném dialogu zadejte přihlašovací údaje domény hello Správce clusteru HPC hello. (Můžete také přidat další clusteru uživatele v různých rolích. V tématu [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335.aspx).)
   
    Hello webový portál otevře toohello zobrazení seznamu úloh.
3. Klikněte na tlačítko toosubmit ukázka úlohu, která vrací řetězec hello "Hello, World" z clusteru hello **nová úloha** v levém navigačním panelu hello.
4. Na hello **nová úloha** v části **ze stránek odeslání**, klikněte na tlačítko **HelloWorld**. Zobrazí se stránka odeslání úlohy Hello.
5. Klikněte na tlačítko **odeslání**. Pokud se zobrazí výzva, zadejte přihlašovací údaje domény hello Správce clusteru HPC hello. Hello úloha odeslána a hello ID úlohy se zobrazí na hello **Mé úlohy** stránky.
6. výsledky hello tooview hello úlohy, které jste odeslali, klikněte na tlačítko hello ID úlohy a pak klikněte na **úlohy v zobrazení** výstupu příkazu hello tooview (v části **výstup**).

## <a name="next-steps"></a>Další kroky
* Můžete také odeslat úlohy toohello clusteru Azure s hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Pokud chcete toosubmit clusteru úlohy z klienta systému Linux, najdete v části hello Python ukázku v hello [HPC Pack 2012 R2 SDK a ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
