---
title: "aaaCreate klasické virtuální počítač Azure s MySQL | Microsoft Docs"
description: "Vytvořte virtuální počítač Azure se systémem Windows Server 2012 R2 a hello databáze MySQL pomocí modelu nasazení classic hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 98fa06d2-9b92-4d05-ac16-3f8e9fd4feaa
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: cynthn
ms.openlocfilehash: 2c9564955c2bab197a8e494e939ce16c0b9bb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-created-with-hello-classic-deployment-model-running-windows-server-2016"></a>Instalace databáze MySQL na virtuální počítač vytvořený s modelem nasazení classic hello systémem Windows Server 2016
[MySQL](https://www.mysql.com) je populární open source, databáze SQL. Tento kurz ukazuje, jak tooinstall a spusťte hello **komunitní verzi MySQL 5.7.18** jako Server databáze MySQL na virtuálním počítači s **systému Windows Server 2016**. Prostředí může být mírně lišit pro jiné verze MySQL nebo Windows Server.

Pokyny k instalaci MySQL v systému Linux, najdete v části: [jak tooinstall MySQL v Azure](../../linux/mysql-install.md).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

## <a name="create-a-virtual-machine-running-windows-server-2016"></a>Vytvoření virtuálního počítače se systémem Windows Server 2016
Pokud ještě nemáte virtuálního počítače s Windows Server 2016, můžete to [kurzu](./tutorial.md) toocreate hello virtuálního počítače.

## <a name="attach-a-data-disk"></a>Připojení datového disku
Po vytvoření hello virtuálního počítače, můžete volitelně připojit datový disk. Přidání že datový disk se doporučuje pro úlohy v produkčním prostředí a tooavoid systémem hello OS jednotce (C:) nedostatek místa, což zahrnuje hello operačního systému.

V tématu [jak tooattach datový disk virtuálního počítače Windows tooa](../attach-managed-disk-portal.md) a postupujte podle pokynů hello pro prázdný disk se připojuje. Hello hostitele mezipaměti nastavení sady příliš**žádné** nebo **jen pro čtení**.

## <a name="log-on-toohello-virtual-machine"></a>Přihlaste se toohello virtuálního počítače
Dále budete [protokolu na virtuálním počítači toohello](./connect-logon.md) , můžete nainstalovat MySQL.

## <a name="install-and-run-mysql-community-server-on-hello-virtual-machine"></a>Nainstalujte a spusťte MySQL Community Server na virtuálním počítači hello
Postupujte podle těchto kroků tooinstall, konfigurovat a používat verzi komunity hello MySQL serveru:

> [!NOTE]
> Při stahování položek pomocí aplikace Internet Explorer, můžete nastavit hello IE **konfigurace rozšířeného zabezpečení aplikace** tooOff a zjednodušit proces stahování hello. Z nabídky Start hello, klikněte na správce nebo místního serveru pro správu nástroje/Server a pak klikněte na aplikaci Internet Explorer **konfigurace rozšířeného zabezpečení** a nastavte tooOff konfigurace hello).
>
>

1. Po připojení toohello virtuálního počítače pomocí vzdálené plochy, klikněte na tlačítko **Internet Explorer** z obrazovky start hello.
2. Vyberte hello **nástroje** tlačítko v pravém horním rohu hello (ikona kolečka cogged hello) a potom klikněte na **Možnosti Internetu**. Klikněte na tlačítko hello **zabezpečení** , klikněte na hello **Důvěryhodné servery** ikonu a potom klikněte na hello **lokality** tlačítko. Přidáte http://*.mysql.com toohello seznam důvěryhodných serverů. Klikněte na tlačítko **Zavřít**a potom klikněte na **OK**.
3. V hello adresu aplikace Internet Explorer, zadejte https://dev.mysql.com/downloads/mysql/.
4. Použijte hello MySQL lokality toolocate a stáhněte nejnovější verzi hello MySQL Instalační služby pro Windows hello. Při výběru hello MySQL instalační program, stáhněte si hello verzi, která má hello dokončete sadou souborů (například hello mysql – instalační program komunity 5.7.18.0.msi a velikost souboru 352.8 MB) a uložte instalační program hello.
5. Po dokončení instalačního programu hello stahování, klikněte na tlačítko **spustit** toolaunch instalační program.
6. Na hello **licenční smlouvy** , přijmout hello licenční smlouvu a klikněte na tlačítko **Další**.
7. Na hello **výběr typu instalace** klikněte na typ hello instalace, který a pak klikněte na tlačítko **Další**. Hello následující postup předpokládá hello výběr hello **pouze Server** instalace typu.
8. Pokud hello **Zkontrolujte požadavky** zobrazí, klikněte na tlačítko **Execute** toolet hello instalačního programu nainstalujte všechny chybějící součásti. Postupujte podle pokynů zobrazit, například C++ Redistributable runtime hello.
9. Na hello **instalace** klikněte na tlačítko **Execute**. Po dokončení instalace klikněte na tlačítko **Další**.

10. Na hello **konfiguraci produktu** klikněte na tlačítko **Další**.

11. Na hello **typu a sítě** stránky, zadejte požadované konfigurace typu a připojení možnosti, včetně hello TCP port v případě potřeby. Vyberte **zobrazit rozšířené možnosti**a potom klikněte na **Další**.
    ![](./media/mysql-2008r2/MySQL_TypeNetworking.png)

12. Na hello **účtů a rolí** stránky, zadejte silné heslo kořenové MySQL. Podle potřeby přidat další uživatelské účty MySQL a potom klikněte na **Další**.

    ![](./media/mysql-2008r2/MySQL_AccountsRoles_Filled.png)
13. Na hello **služba systému Windows** stránky, zadejte změny toohello výchozí nastavení pro spuštění hello MySQL serveru jako služby systému Windows, podle potřeby a pak klikněte na tlačítko **Další**.

    ![](./media/mysql-2008r2/MySQL_WindowsService.png)
14. Hello volby na hello **modulů plug-in a rozšíření** stránky jsou volitelné. Klikněte na tlačítko **Další** toocontinue.
15. Na hello **pokročilé možnosti** určete možnosti toologging změny podle potřeby a pak klikněte na tlačítko **Další**.

    ![](./media/mysql-2008r2/MySQL_AdvOptions.png)
16. Na hello **použít konfiguraci serveru** klikněte na tlačítko **Execute**. Po dokončení kroků konfigurace hello klikněte na tlačítko **Dokončit**.
17. Na hello **konfiguraci produktu** klikněte na tlačítko **Další**.
18. Na hello **dokončení instalace** klikněte na tlačítko **protokolu kopie tooClipboard** Pokud chcete tooexamine později a pak klikněte na tlačítko **Dokončit**.
19. Na úvodní obrazovce hello zadejte **mysql**a potom klikněte na **MySQL 5.7 příkazového řádku klienta**.
20. Zadejte hello kořenové heslo, které jste zadali v kroku 12 a zobrazovat jsou s výzvou, kde můžete použít příkazy tooconfigure MySQL. Podrobnosti hello příkazů a syntaxe najdete v tématu [MySQL v příručkách](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html).

    ![](./media/mysql-2008r2/MySQL_CommandPrompt.png)
21. Můžete také nakonfigurovat nastavení výchozí konfigurace serveru, například hello základní a data adresáře a jednotky. Další informace najdete v tématu [6.1.2 výchozí nastavení konfigurace serveru](https://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Konfigurace koncových bodů

Pro počítače k dispozici tooclient hello MySQL služby toobe v hello Internetu musí konfigurace koncového bodu hello TCP port a vytvořte pravidlo brány Firewall systému Windows. Hello výchozí port hodnota, na které hello MySQL Server naslouchá služby pro klienty MySQL je 3306. Můžete zadat jiný port, tak dlouho, dokud hello port je konzistentní s hodnotou hello součástí hello **typu a sítě** (krok 11 hello předchozího postupu).

> [!NOTE]
> Pro použití v provozním prostředí zvažte hello provedení hello MySQL serveru služby k dispozici tooall počítače na Internetu hello vliv na zabezpečení. Můžete definovat sadu hello zdrojové IP adresy, které jsou povoleny toouse hello endpoint s seznamu řízení přístupu (ACL). Další informace najdete v tématu [jak tooSet až koncové body tooa virtuální počítač](setup-endpoints.md).
>
>

tooconfigure koncový bod pro hello služby MySQL serveru:

1. V hello portálu Azure, klikněte na **virtuálních počítačů (klasické)**, klikněte na název hello MySQL virtuálního počítače a pak klikněte na tlačítko **koncové body**.
2. Na panelu příkazů hello, klikněte na **přidat**.
3. Na hello **přidání koncového bodu** zadejte jedinečný název pro **název**.
4. Vyberte **TCP** jako protokol pro hello.
5. Zadejte číslo portu hello, například **3306**, v obou **veřejný Port** a **privátní Port**a potom klikněte na **OK**.

## <a name="add-a-windows-firewall-rule-tooallow-mysql-traffic"></a>Přidat pravidlo tooallow brány Windows Firewall provoz MySQL
tooadd pravidlo brány Firewall systému Windows, které umožní MySQL provoz z hello Internetu, spusťte následující příkaz v hello _příkazového řádku se zvýšenými oprávněními prostředí Windows PowerShell_ ve virtuálním počítači hello MySQL serveru.

    New-NetFirewallRule -DisplayName "MySQL57" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public

## <a name="test-your-remote-connection"></a>Testování vzdáleného připojení
tootest vašeho webu spuštěné hello pro připojení ke vzdálené toohello virtuálního počítače Azure MySQL serveru služby, je nutné zadat název DNS hello obsahující hello VN hello cloudové služby.

1. V hello portálu Azure, klikněte na **virtuálních počítačů (klasické)**, klikněte na název hello MySQL serveru virtuálního počítače a pak klikněte na tlačítko **přehled**.
2. Z řídicího panelu hello virtuálního počítače, Všimněte si, hello **název DNS** hodnotu. Zde naleznete příklad:

   ![](media/mysql-2008r2/MySQL_DNSName.png)
3. Z místního počítače se systémem MySQL nebo hello MySQL klienta spusťte následující příkaz toolog v jako uživatel MySQL hello.

     MySQL -u <yourMysqlUsername> - p -h<yourDNSname>

   Například pomocí hello MySQL uživatelské jméno _dbadmin3_ a hello _testmysql.cloudapp.net_ na názvy DNS pro hello virtuálního počítače, můžete začít MySQL pomocí hello následující příkaz:

     MySQL -u dbadmin3 -p -h testmysql.cloudapp.net

## <a name="next-steps"></a>Další kroky
toolearn Další informace o spuštění MySQL, najdete v části hello [MySQL dokumentaci](http://dev.mysql.com/doc/).
