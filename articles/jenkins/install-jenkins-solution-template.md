---
title: "aaaCreate volaných serveru v Azure"
description: "Nainstalujte volaných na virtuálním počítači Azure Linux z hello volaných řešení šablony a vytvoření ukázkové aplikace Java."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 82ab2ac52594acba131414b449b608978591d4b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-hello-azure-portal"></a>Vytvoření serveru volaných ve virtuálním počítači Azure Linux z hello portálu Azure

Tento rychlý start ukazuje, jak tooinstall [volaných](https://jenkins.io) na Linux virtuálního počítače s Ubuntu s hello nástrojů a modulů plug-in nakonfigurovat toowork s Azure. Až budete hotovi, budete mít v Azure spuštěný server Jenkins sestavující ukázkovou aplikaci v Javě z [GitHubu](https://github.com).

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure
* TooSSH přístup na příkazovém řádku počítače (například hello Bash prostředí nebo [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-jenkins-vm-from-hello-solution-template"></a>Vytvoření hello volaných virtuálního počítače ze šablony řešení hello

Otevřete hello [marketplace bitovou kopii pro volaných](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) ve webovém prohlížeči a vyberte **získat IT teď** z levé straně stránky hello hello. Zkontrolujte hello ceny podrobnosti a vyberte **pokračovat**, pak vyberte **vytvořit** tooconfigure hello volaných server hello portálu Azure. 
   
![Dialogové okno na webu Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

V hello **nakonfigurovali základní nastavení** kartě, vyplňte následující pole hello:

![Konfigurace základního nastavení](./media/install-jenkins-solution-template/ap-basic.png)

* Jako **Název** použijte **Jenkins**.
* Zadejte **Uživatelské jméno**. Hello uživatelské jméno musí splňovat [specifické požadavky](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
* Vyberte **heslo** jako hello **typ ověřování** a zadejte heslo. Hello heslo musí mít velké písmeno, číslo a jeden speciální znak.
* Použití **myJenkinsResourceGroup** pro hello **skupiny prostředků**.
* Zvolte hello **východní USA** [oblast Azure](https://azure.microsoft.com/regions/) z hello **umístění** rozevíracího seznamu.

Vyberte **OK** tooproceed toohello **konfigurace dalších možností** kartě. Zadejte jedinečný doménový název tooidentify hello volaných server a vyberte **OK**.

![Nastavení dalších možností](./media/install-jenkins-solution-template/ap-addtional.png)  

 Po ověření neselže, vybrat **OK** znovu z hello **Souhrn** kartě. Nakonec vyberte **nákupu** toocreate hello volaných virtuálních počítačů. Když se váš server je připraven, zobrazí oznámení v hello portálu Azure:   

![Oznámení o připravenosti Jenkinse](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-toojenkins"></a>Připojit tooJenkins

Tooyour virtuálního počítače (například http://jenkins2517454.eastus.cloudapp.azure.com/) přejděte v prohlížeči. Hello volaných konzoly je nedostupné přes nezabezpečený HTTP, takže pokyny jsou k dispozici na konzole volaných hello stránky tooaccess hello bezpečně z počítače pomocí tunelového propojení SSH.

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Nastavit hello tunelové propojení prostřednictvím hello `ssh` příkaz na stránce hello z příkazového řádku hello, nahraďte `username` s názvem hello předtím vybrali při nastavování hello virtuálního počítače ze šablony řešení hello uživatel s oprávněními správce hello virtuálního počítače.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Po spuštění hello tunel přejděte toohttp://localhost:8080 / na místním počítači. 

Získáte hello počátečního hesla tak, že spustíte následující příkaz na příkazovém řádku hello během připojení prostřednictvím SSH toohello volaných VM hello.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Odemkněte hello volaných řídicím panelu hello první přihlášení pomocí toto počáteční heslo.

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-unlock.png)

Vyberte **nainstalovat navrhované modulů plug-in** na hello další stránky a pak vytvořit řídicí panel volaných správce uživatele použít tooaccess hello volaných.

![Jenkins je připraven!](./media/install-jenkins-solution-template/jenkins-welcome.png)

Hello volaných server je nyní připraven toobuild kódu.

## <a name="create-your-first-job"></a>Vytvoření první úlohy

Vyberte **vytvoření nové úlohy** hello volaných konzoly, pojmenujte ji **mySampleApp** a vyberte **volný styl projektu**, pak vyberte **OK**.

![Vytvoření nové úlohy](./media/install-jenkins-solution-template/jenkins-new-job.png) 

Vyberte hello **správu zdrojového kódu** kartě, povolte **Git**a zadejte následující adresu URL v hello **adresu URL úložiště** pole:`https://github.com/spring-guides/gs-spring-boot.git`

![Definování hello úložiště Git](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Vyberte hello **sestavení** a potom vyberte **přidat krok sestavení**, **Gradle vyvolání skriptu**. Vyberte **Use Gradle Wrapper** (Použít obálku Gradle) a pak zadejte `complete` do pole **Wrapper location** (Umístění obálky) a `build` do pole **Tasks** (Úlohy).

![Použít toobuild obálku Gradle hello](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Vyberte **Advanced...** (Upřesnit...) a pak zadejte `complete` v hello **kořenové sestavit skript** pole. Vyberte **Uložit**.

![Pokročilé nastavení v kroku sestavení obálku Gradle hello](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-hello-code"></a>Sestavení hello kódu

Vyberte **sestavení teď** toocompile hello kód a balíček hello ukázkovou aplikaci. Pokud vaše sestavení se dokončí, vyberte hello **prostoru** propojení projektu hello.

![Vyhledejte soubor JAR toohello prostoru tooget hello ze sestavení hello](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Přejděte příliš`complete/build/libs` a ujistěte se, hello `gs-spring-boot-0.1.0.jar` je k dispozici tooverify úspěšného buildu. Vaše volaných, které server je nyní připraveni toobuild vašich vlastních projektů v Azure.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Přidání virtuálních počítačů Azure jako agentů Jenkinse](jenkins-azure-vm-agents.md)
