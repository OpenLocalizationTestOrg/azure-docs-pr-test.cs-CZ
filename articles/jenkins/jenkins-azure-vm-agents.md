---
title: "virtuální počítač Azure agenty aaaUse pro nepřetržitou integraci s volaných."
description: "Agenti virtuálních počítačů Azure jako podřízené servery Jenkinse"
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 2388e6919d0280372166fbd325d80dafb00d7550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse

Tento rychlý start ukazuje, jak toouse hello agenty virtuálních počítačů Azure volaných modul plug-in toocreate agenta Linux (Ubuntu) na vyžádání v Azure.

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý start:

* Pokud již nemáte volaných master, můžete začít s hello [šablona řešení](install-jenkins-solution-template.md) 
* Odkazovat příliš[vytvořit objekt služby Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) Pokud již nemáte objektu zabezpečení služby Azure.

## <a name="install-azure-vm-agents-plugin"></a>Instalace modulu plug-in Azure VM Agents

Když spustíte z hello [šablona řešení](install-jenkins-solution-template.md), modul plug-in hello agenta virtuálního počítače Azure je nainstalován v předloze volaných hello.

Jinak, instalovat hello **agenti virtuálních počítačů Azure** modulu plug-in v rámci hello volaných řídicího panelu.

## <a name="configure-hello-plugin"></a>Konfigurace modulu plug-in hello

* V rámci hello volaných řídicí panel, klikněte na **volaných spravovat -> Konfigurovat systém ->**. Posuňte se toohello dolní části stránky hello a najít oddíl hello s rozevíracím hello **přidat nové cloudové**. Hello nabídce vyberte **agenti virtuálního počítače Microsoft Azure**
* Z rozevíracího seznamu hello přihlašovací údaje Azure vyberte existující účet.  tooadd nový **Microsoft Azure Service Principal** zadejte hello následující hodnoty: ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0.

![Přihlašovací údaje Azure](./media/jenkins-azure-vm-agents/service-principal.png)

* Klikněte na tlačítko **ověřte konfiguraci** toomake, že tato konfigurace profilu hello je správná.
* Uložte konfiguraci hello a pokračovat dalším krokem toohello.

## <a name="template-configuration"></a>Konfigurace šablony

### <a name="general-configuration"></a>Obecná konfigurace
Potom nakonfigurujte šablonu pro použití toodefine agenta virtuálního počítače Azure. 

* Klikněte na tlačítko **přidat** tooadd šablonu. 
* Zadejte název nové šablony. 
* Pro hello štítek zadejte "ubuntu." Tento popisek se používá během hello úlohy konfigurace.
* Vyberte požadovanou oblast hello z pole se seznamem hello.
* Vyberte hello potřeby velikost virtuálního počítače.
* Zadejte název účtu úložiště Azure hello nebo necháte prázdné toouse hello výchozí název "jenkinsarmst."
* Zadejte dobu uchování hello v minutách. Toto nastavení definuje hello počet minut, po které může volaných čekat před odstraněním automaticky nečinnosti agenta. Pokud nechcete, aby automaticky odstraněn toobe nečinnosti agenty, zadejte 0.

![Obecná konfigurace](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Konfigurace image

toocreate agenta Linux (Ubuntu), vyberte **referenční bitové kopie** a hello použijte následující konfiguraci jako příklad. Odkazovat příliš[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) hello nejnovější Azure podporováno obrázky.

* Image Publisher (Vydavatel image): Canonical
* Image Offer (Nabídka image): UbuntuServer
* Image Sku (Skladová jednotka image): 14.04.5-LTS
* Image version (Verze image): latest (nejnovější)
* OS Type (Typ operačního systému): Linux
* Launch method (Metoda spuštění): SSH
* Zadejte přihlašovací údaje správce
* Jako skript pro inicializaci virtuálního počítače zadejte:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Konfigurace image](./media/jenkins-azure-vm-agents/image-config.png)

* Klikněte na tlačítko **ověřte šablony** tooverify hello konfigurace.
* Klikněte na **Uložit**.

## <a name="create-a-job-in-jenkins"></a>Vytvoření úlohy v Jenkinsu

* V rámci hello volaných řídicí panel, klikněte na **novou položku**. 
* Zadejte název, vyberte **Freestyle project** (Volný projekt) a klikněte na **OK**.
* V hello **Obecné** karty, vyberte možnost "Omezit, kde je možné spustit projekt" a typ "ubuntu" ve výrazu popisku. Zobrazí "ubuntu" v rozevírací nabídce hello.
* Klikněte na **Uložit**.

![Nastavení úlohy](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Sestavení nového projektu

* Vraťte se zpátky toohello volaných řídicího panelu.
* Vytvořit novou úlohu hello klikněte pravým tlačítkem, poté klikněte na tlačítko **sestavení teď**. Spustí se sestavování. 
* Po dokončení sestavení hello přejděte příliš**konzole výstup**. Uvidíte, že sestavení hello byla provedena vzdáleně na platformě Azure.

![Výstup konzoly](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Referenční informace

* Video Azure Friday: [Průběžná integrace pomocí Jenkinse s využitím agentů virtuálních počítačů Azure](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Informace o podpoře a možnosti konfigurace: [Wikiweb modulu plug-in Azure VM Agent pro Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 

