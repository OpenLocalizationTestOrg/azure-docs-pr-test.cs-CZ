---
title: "aaaSet až hybridní HPC Pack clusteru v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Microsoft HPC Pack a Azure tooset až malé, hybridní vysoký výkon výpočetní (prostředí HPC) clusteru"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 5ad30d78dcd0c6a95d2a61f25015232edab3563c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Nastavení hybridní vysokovýkonného výpočetního prostředí (HPC) clusteru pomocí sady Microsoft HPC Pack a na vyžádání Azure výpočetních uzlů
Používejte Microsoft HPC Pack 2012 R2 a Azure tooset až malé, hybridní vysokovýkonného výpočetního prostředí (HPC) clusteru. Hello clusteru uvedené v tomto článku se skládá z hlavního uzlu místními HPC Pack a některé výpočetní uzly, které nasazujete na vyžádání v Azure cloud service. Když pak spustíte výpočetní úlohy v clusteru hybridní hello.

![Hybridní clusteru HPC][Overview] 

Tento kurz ukazuje jeden ze způsobů, se někdy označuje clusteru "shluků toohello cloud," toouse, škálovatelnou a na vyžádání prostředků Azure toorun náročné aplikace.

Tento kurz předpokládá žádné předchozí zkušenosti s výpočetní clustery nebo HPC Pack. Je určený jenom toohelp nasazení hybridního výpočetního clusteru rychle pro demonstrační účely. Požadavky a kroky toodeploy hybridní HPC Pack clusteru větší škálované v provozním prostředí, nebo toouse HPC Pack 2016, najdete v tématu hello [podrobné pokyny](http://go.microsoft.com/fwlink/p/?LinkID=200493). Pro scénáře s HPC Pack, včetně automatizované nasazení clusteru ve virtuálních počítačích Azure, najdete v části [možnosti clusteru HPC pomocí sady Microsoft HPC Pack v Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure** – Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.
* **Místní počítač se systémem Windows Server 2012 R2 nebo Windows Server 2012** -použít tento počítač jako hello hlavního uzlu v clusteru HPC hello. Pokud už používáte Windows Server, můžete stáhnout a nainstalovat [zkušební verze](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * Hello počítač musí být tooan připojené k doméně služby Active Directory. Pro účely testování můžete nakonfigurovat hello hlavního uzlu počítač jako řadič domény. tooadd hello role serveru služby Active Directory Domain Services a hello hlavního uzlu počítač jako řadič domény povýšit, naleznete v dokumentaci k systému Windows Server hello.
  * toosupport HPC Pack hello operačního systému musí být nainstalován v jednom z těchto jazycích: angličtina, japonština nebo čínština (zjednodušená).
  * Ověřte, že se nainstalují důležité a kritické aktualizace.
* **HPC Pack 2012 R2** - [Stáhnout](http://go.microsoft.com/fwlink/p/?linkid=328024) hello instalační balíček pro nejnovější verzi hello volné zdarma a zkopírujte hello soubory toohello hlavního uzlu počítače. Zvolte instalační soubory v hello stejný jazyk jako vaše instalace systému Windows Server.

    >[!NOTE]
    > Pokud chcete toouse HPC Pack 2016 místo HPC Pack 2012 R2, je potřeba další konfiguraci. V tématu hello [podrobné pokyny](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Účet domény** -tento účet musí být nakonfigurované hello hlavního uzlu tooinstall HPC Pack oprávnění místního správce.
* **Připojení TCP na portu 443** z hlavního uzlu tooAzure hello.

## <a name="install-hpc-pack-on-hello-head-node"></a>Instalace sady HPC Pack hello hlavního uzlu
První instalaci sady Microsoft HPC Pack v místní počítače se spuštěným systémem Windows Server. Tento počítač se změní na hello hlavního uzlu clusteru hello.

1. Toohello hlavního uzlu se přihlaste pomocí účtu domény, který má oprávnění místního správce.

2. Spusťte Průvodce instalací HPC Pack hello spuštěním Setup.exe z hello HPC Pack instalačních souborů.

3. Na hello **instalace HPC Pack 2012 R2** obrazovky, klikněte na tlačítko **novou instalaci, nebo přidejte nové funkce tooan existující instalaci**.

    ![Instalace sady HPC Pack 2012][install_hpc1]

4. Na hello **stránku smlouvy uživatele softwaru Microsoft**, klikněte na tlačítko **Další**.

5. Na hello **vybrat typ instalace** klikněte na tlačítko **vytvoření nového clusteru HPC vytvořením hlavního uzlu**a potom klikněte na **Další**.

6. Spustí Průvodce Hello několik testů před instalací. Klikněte na tlačítko **Další** na hello **pravidla instalace** Pokud všechny testy byly úspěšné. Jinak Zkontrolujte zadané informace hello a proveďte potřebné změny ve vašem prostředí. Pak spusťte znovu hello testů nebo pokud nezbytné spuštění hello Průvodce instalací znovu.
7. Na hello **HPC DB konfigurace** se přesvědčte, že **hlavní uzel** pro všechny databáze HPC je vybrána a potom klikněte na **Další**. 

    ![Konfigurace databáze][install_hpc4]

8. Přijměte výchozí nastavení na dalších stránkách průvodce hello hello. Na hello **nainstalovat požadované součásti** klikněte na tlačítko **nainstalovat**.
   
    ![Instalace][install_hpc6]

9. Po dokončení instalace hello, zrušte zaškrtnutí políčka **Start Správce clusteru HPC** a pak klikněte na **Dokončit**. (V pozdější fázi spusťte Správce clusteru HPC.)
   
    ![Dokončit][install_hpc7]

## <a name="prepare-hello-azure-subscription"></a>Příprava hello předplatného Azure
Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com) s předplatným Azure. Po dokončení těchto kroků, můžete nasadit Azure uzly z hlavního uzlu místní hello. 
  
  > [!NOTE]
  > Také si poznamenejte ID vašeho předplatného Azure, které budete potřebovat později. Najít ID hello v **odběry** hello portálu.
  > 

### <a name="upload-hello-default-management-certificate"></a>Nahrát hello výchozího certifikátu pro správu
HPC Pack nainstaluje certifikát podepsaný svým držitelem hello hlavního uzlu, nazývá hello výchozí Microsoft HPC Azure Management certifikát, který nahrajete jako certifikát pro správu Azure. Tento certifikát je k dispozici pro testování a nasazení testování konceptu toosecure hello připojení mezi hello hlavního uzlu a Azure.

1. Z počítače hello hlavního uzlu, přihlášení toohello [portál Azure](https://portal.azure.com).

2. Klikněte na tlačítko **odběry** > *your_subscription_name*.

3. Klikněte na tlačítko **certifikáty pro správu** > **nahrát**.4. Procházejte hello hlavního uzlu pro 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC Pack souboru hello. Potom klikněte na **nahrát**.

   
Hello **výchozí HPC Azure Management** certifikát se zobrazí v seznamu hello certifikáty pro správu.

### <a name="create-an-azure-cloud-service"></a>Vytvoření cloudové služby Azure
> [!NOTE]
> Pro nejlepší výkon, vytvořit hello cloudové služby a účet úložiště hello (v pozdější fázi) v hello stejné zeměpisné oblasti.
> 
> 

1. Hello portálu, klikněte na tlačítko **cloudových služeb (klasické)** > **+ přidat**.

2.  Zadejte název DNS pro službu hello, vyberte skupinu prostředků a umístění a pak klikněte na tlačítko **vytvořit**.


### <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure
1. Hello portálu, klikněte na tlačítko **účty úložiště (klasické)** > **+ přidat**.

2. Zadejte název pro účet hello a vyberte hello **Classic** modelu nasazení.

3. Vyberte skupinu prostředků a umístění a nechte ostatní nastavení na výchozí hodnoty. Poté klikněte na **Vytvořit**.

## <a name="configure-hello-head-node"></a>Konfigurace hlavního uzlu hello
toouse toodeploy Správce clusteru HPC Azure uzly a úlohy toosubmit nejprve provést několik kroků konfigurace požadovaných clusterových.

1. Hello hlavního uzlu spusťte Správce clusteru HPC. Pokud hello **vyberte hlavní uzel** se zobrazí dialogové okno, klikněte na tlačítko **místního počítače**. Hello **nasazení na seznam úkolů** se zobrazí.

2. V části **požadované úlohy nasazení**, klikněte na tlačítko **konfigurace sítě**.
   
    ![Konfigurace sítě][config_hpc2]

3. V hello Průvodce konfigurací sítě, vyberte **všechny uzly pouze v podnikové síti** (topologie 5). Tato konfigurace sítě je nejjednodušší pro demonstrační účely hello.
   
    ![Topologie 5][config_hpc3]
   
4. Klikněte na tlačítko **Další** tooaccept výchozí hodnoty na hello zbývající stránky průvodce hello. Potom na hello **zkontrolujte** , klikněte na **konfigurace** toocomplete hello síťové konfigurace.

5. V hello **nasazení na seznam úkolů**, klikněte na tlačítko **zadejte pověření pro instalaci**.

6. V hello **pověření pro instalaci** dialogové okno, zadejte přihlašovací údaje hello účtu domény hello, kterou jste použili tooinstall HPC Pack. Pak klikněte na **OK**. 
   
    ![Pověření pro instalaci][config_hpc6]
   
7. V hello **nasazení na seznam úkolů**, klikněte na tlačítko **konfigurace hello pojmenování nové uzly**.

8. V hello **zadejte řady pojmenování uzlu** dialogové okno pole, přijměte výchozí hello pojmenování řady a klikněte na tlačítko **OK**. Dokončení tohoto kroku, i když hello uzlů Azure zadaná v tomto kurzu jsou pojmenované automaticky.
   
    ![Pojmenování uzlu][config_hpc8]
   
9. V hello **nasazení na seznam úkolů**, klikněte na tlačítko **vytvoření šablony uzlu**. Později v kurzu hello použijete hello uzel šablony tooadd uzlů Azure toohello clusteru.

10. V hello Průvodce vytvořením šablony uzlu, proveďte následující hello:
    
    a. Na hello **výběr typu šablony uzlu** klikněte na tlačítko **šablony uzlu služby Windows Azure**a potom klikněte na **Další**.
    
    ![Šablony uzlu][config_hpc10]
    
    b. Klikněte na tlačítko **Další** tooaccept hello výchozí název šablony.
    
    c. Na hello **poskytují informace o předplatném** zadejte svoje ID předplatného Azure (k dispozici v informací o vašem účtu Azure). Potom v **certifikát pro správu**, vyhledejte **výchozí Microsoft HPC Azure Management.** Pak klikněte na tlačítko **Další**.
    
    ![Šablony uzlu][config_hpc12]
    
    d. Na hello **poskytují informace o službě** stránky, vyberte hello Cloudová služba a hello účet úložiště, který jste vytvořili v předchozím kroku. Pak klikněte na tlačítko **Další**.
    
    ![Šablony uzlu][config_hpc13]
    
    e. Klikněte na tlačítko **Další** tooaccept výchozí hodnoty na hello zbývající stránky průvodce hello. Potom na hello **zkontrolujte** , klikněte na **vytvořit** toocreate hello uzel šablony.
    
    > [!NOTE]
    > Ve výchozím nastavení, hello Azure uzlu šablona obsahuje nastavení pro toostart (zřídit) a zastavení hello uzly ručně, pomocí Správce clusteru HPC. Volitelně můžete nakonfigurovat plán toostart a ukončení hello uzlů Azure automaticky.
    > 
    > 

## <a name="add-azure-nodes-toohello-cluster"></a>Přidání uzlů Azure toohello clusteru
Teď použijte hello uzel šablony tooadd uzlů Azure toohello clusteru. Přidání clusteru toohello uzly hello ukládá informace o jejich konfiguraci tak, aby bylo možné spustit (zřídit) je kdykoli v hello cloudové služby. Vaše předplatné pouze získá účtovat uzlů Azure po hello instance běží v cloudové službě hello.

Postupujte podle těchto kroků tooadd dva malé uzly.

1. Ve Správci clusteru HPC, klikněte na tlačítko **uzlu správy** (nazývá **Správa prostředků** v aktuálních verzích HPC Pack) > **přidat uzel**.
   
    ![Přidání uzlu][add_node1]

2. V hello Průvodce přidáním uzlu na hello **vybrat způsob nasazení** klikněte na tlačítko **systému Windows Azure přidat uzly**a potom klikněte na **Další**.
   
    ![Přidání uzlu Azure][add_node1_1]

3. Na hello **zadejte nové uzly** stránky, dříve vytvořené šablony Azure uzlu vyberte hello (nazývá ve výchozím nastavení **výchozí šablonu AzureNode**). Pak zadejte **2** uzly velikosti **malé**a potom klikněte na **Další**.
   
    ![Určit uzlů][add_node2]
   
4. Na hello **hello dokončení Průvodce přidáním uzlu** klikněte na tlačítko **Dokončit**.
    
     Dva uzly Azure s názvem **AzureCN 0001** a **AzureCN 0002**, se nyní zobrazí ve Správci clusteru HPC. Obě jsou v hello **není nasazena** stavu.
   
    ![Přidání uzlů][add_node3]

## <a name="start-hello-azure-nodes"></a>Spustit hello uzlů Azure
Pokud chcete prostředky clusteru hello toouse v Azure, použijte Správce clusteru HPC toostart (zřídit) hello uzlů Azure a jejich převedení do online režimu.

1. Ve Správci clusteru HPC, klikněte na tlačítko **uzlu správy** (nazývá **Správa prostředků** v aktuálních verzích HPC Pack), a vyberte hello uzlů Azure.

2. Klikněte na tlačítko **spustit**a potom klikněte na **OK**.
   
   ![Počáteční uzly][add_node4]
   
    uzly Hello přechod toohello **zřizování** stavu. Zobrazení hello zřizování hello tootrack protokolu průběh zřizování.
   
    ![Zřízení uzly][add_node6]

3. Po několika minutách hello uzlů Azure zřídit a jsou v hello **Offline** stavu. V tomto stavu instance rolí hello běží, ale ještě nepovoluje úlohy clusteru.

4. tooconfirm, který hello instance role běží v hello portálu Azure, klikněte na tlačítko **cloudové služby (klasické)** > *your_cloud_service_name*.
   
   Měli byste vidět dvě **HpcWorkerRole** instancí (uzlů) spuštěných ve službě hello. HPC Pack také automaticky nasadí dvě **HpcProxy** instance (velikost střední) toohandle komunikace mezi hello hlavního uzlu a Azure.

   ![Spuštěné instance][view_instances1]

5. toobring hello Azure uzly online toorun clusteru úlohy, vyberte hello uzly, klikněte pravým tlačítkem a pak klikněte na tlačítko **přepnout do režimu Online**.
   
    ![Offline uzly][add_node7]
   
    Správce clusteru HPC označuje, že hello uzly jsou v hello **Online** stavu.

## <a name="run-a-command-across-hello-cluster"></a>Spuštění příkazu v clusteru hello
toocheck hello instalace, použití hello HPC Pack **clusrun** příkaz toorun příkaz nebo aplikace na jeden nebo více uzlech clusteru. Jako jednoduchý příklad, použít **clusrun** konfiguraci IP hello tooget hello uzlů Azure.

1. Hello hlavního uzlu otevřete příkazový řádek jako správce.

2. Zadejte hello následující příkaz:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Pokud se zobrazí výzva, zadejte heslo správce clusteru. Měli byste vidět, že výstup podobný toohello následující příkaz.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Spustit úlohu testu
Teď odešlete testovací úlohu, která běží na clusteru hybridní hello. V tomto příkladu je úloha jednoduché čištění parametrů (typ vnitřně paralelní výpočty). Tento příklad spustí dílčí úkoly, které přidat celé číslo tooitself pomocí hello **nastavit /a** příkaz. Všechny hello uzly v clusteru hello přispívat toofinishing hello dílčí úkoly pro celá čísla od 1 too100.

1. Ve Správci clusteru HPC, klikněte na tlačítko **úlohy správy** > **novou čištění úlohu oblouku**.
   
    ![Nová úloha][test_job1]

2. V hello **novou čištění úlohu oblouku** dialogu **příkazového řádku**, typ `set /a *+*` (přepsal hello výchozího příkazového řádku, který se zobrazí). Ponechte výchozí hodnoty pro hello zbývající nastavení a potom klikněte na **odeslání** toosubmit hello úlohy.
   
    ![Čištění parametrů][param_sweep1]

3. Po dokončení úlohy hello, dvakrát klikněte na hello **Moje oblouku úloh** úlohy.

4. Klikněte na tlačítko **úlohy v zobrazení**a klikněte na výstup hello vypočítat tooview dílčí úlohy na stejné úrovni.
   
    ![Výsledky úlohy][view_job361]

5. Klikněte na tlačítko pro tento dílčí úlohy, který uzel provést hello výpočtu toosee **přidělené uzly**. (Váš cluster může zobrazovat název jiného uzlu.)
   
    ![Výsledky úlohy][view_job362]

## <a name="stop-hello-azure-nodes"></a>Zastavit hello uzlů Azure
Po vyzkoušení hello clusteru, zastavte hello uzlů Azure tooavoid nepotřebné poplatky tooyour účtu. Tento krok zastaví hello cloudové služby a odebere hello Azure role instance.

1. V modulu Správce clusteru HPC v **uzlu správy** (nazývá **Správa prostředků** v předchozích verzích HPC Pack), vyberte oba uzly Azure. Potom klikněte na **Zastavit**.
   
    ![Zastavit uzly][stop_node1]

2. V hello **zastavit systému Windows Azure uzly** dialogové okno, klikněte na tlačítko **Zastavit**. 
   
3. uzly Hello přechod toohello **zastavení** stavu. Po několika minutách, Správce clusteru HPC uvádí, že jsou uzly hello **nasazení není**.
   
    ![Nenasazeno uzly][stop_node4]

4. hello tooconfirm, které instance rolí hello už běží v Azure, v portálu Azure klikněte na **cloudových služeb (klasické)** > *your_cloud_service_name*. Žádné instance jsou nasazeny v provozním prostředí hello. 
   
    Dokončení kurzu hello.

## <a name="next-steps"></a>Další kroky
* Prozkoumejte hello dokumentaci [HPC Pack](https://technet.microsoft.com/library/cc514029).
* najdete v části tooset až hybridní nasazení clusteru HPC Pack větší škálované [Burst tooAzure instance rolí pracovního procesu pomocí sady Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).
* Jiné způsoby toocreate clusteru služby HPC Pack v Azure, včetně pomocí šablon Azure Resource Manageru, najdete v tématu [možnosti clusteru HPC pomocí sady Microsoft HPC Pack v Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
