---
title: "Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse"
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
ms.openlocfilehash: 0b22a559fbc03158a6d4398603d1a7d2874d7b67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse

Tento rychlý start předvádí, jak pomocí modulu plug-in Jenkins Azure VM Agents vytvořit v Azure linuxového (Ubuntu) agenta na vyžádání.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto rychlého startu je potřeba:

* Pokud ještě nemáte hlavní server Jenkinse, můžete začít s využitím [šablony řešení](install-jenkins-solution-template.md). 
* Pokud ještě nemáte instanční objekt Azure, přečtěte si téma [Vytvoření instančního objektu Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json).

## <a name="install-azure-vm-agents-plugin"></a>Instalace modulu plug-in Azure VM Agents

Pokud začínáte z [šablony řešení](install-jenkins-solution-template.md), modul plug-in Azure VM Agents je nainstalovaný na hlavním serveru Jenkinse.

Jinak nainstalujte modul plug-in **Azure VM Agents** z řídicího panelu Jenkinse.

## <a name="configure-the-plugin"></a>Konfigurace modulu plug-in

* Na řídicím panelu Jenkinse klikněte na **Manage Jenkins (Správa Jenkinse) -> Configure System (Konfigurace systému)**. Přejděte do dolní části stránky a vyhledejte část s rozevíracím seznamem **Add new cloud** (Přidat nový cloud). Z nabídky vyberte **Microsoft Azure VM Agents**.
* Vyberte existující účet z rozevíracího seznamu Azure Credentials (Přihlašovací údaje Azure).  Pokud chcete přidat nový **Microsoft Azure Service Principal** (Instanční objekt Microsoft Azure), zadejte následující hodnoty: Subscription ID (ID předplatného), Client ID (ID klienta), Client Secret (Tajný klíč klienta) a OAuth 2.0 Token Endpoint (Koncový bod tokenu OAuth 2.0).

![Přihlašovací údaje Azure](./media/jenkins-azure-vm-agents/service-principal.png)

* Klikněte na **Verify configuration** (Ověřit konfiguraci) a zkontrolujte správnost konfigurace profilu.
* Uložte konfiguraci a pokračujte k dalšímu kroku.

## <a name="template-configuration"></a>Konfigurace šablony

### <a name="general-configuration"></a>Obecná konfigurace
Dále nakonfigurujte šablonu, která se použije k definování agenta virtuálního počítače Azure. 

* Kliknutím na **Add** (Přidat) přidejte šablonu. 
* Zadejte název nové šablony. 
* Jako popisek zadejte ubuntu. Tento popisek se používá během konfigurace úlohy.
* V poli se seznamem vyberte požadovanou oblast.
* Vyberte požadovanou velikost virtuálního počítače.
* Zadejte název účtu Azure Storage. Pokud pole ponecháte prázdné, použije se výchozí název jenkinsarmst.
* Zadejte dobu uchování v minutách. Toto nastavení definuje počet minut, po které může Jenkins čekat před automatickým odstraněním nečinného agenta. Pokud nechcete, aby se nečinní agenti automaticky odstraňovali, zadejte 0.

![Obecná konfigurace](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Konfigurace image

Pokud chcete vytvořit linuxového (Ubuntu) agenta, vyberte **Image reference** (Odkaz na image) a použijte následující konfiguraci jako příklad. Nejnovější podporované image Azure najdete na webu [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1).

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

* Kliknutím na **Verify Template** (Ověřit šablonu) ověřte konfiguraci.
* Klikněte na **Uložit**.

## <a name="create-a-job-in-jenkins"></a>Vytvoření úlohy v Jenkinsu

* Na řídicím panelu Jenkinse klikněte na **New Item** (Nová položka). 
* Zadejte název, vyberte **Freestyle project** (Volný projekt) a klikněte na **OK**.
* Na kartě **General** (Obecné) vyberte možnost „Restrict where project can be run“ (Omezit, kde je možné projekt spustit) a do pole Label Expression (Výraz popisku) zadejte ubuntu. V rozevíracím seznamu se teď zobrazí ubuntu.
* Klikněte na **Uložit**.

![Nastavení úlohy](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Sestavení nového projektu

* Vraťte se na řídicí panel Jenkinse.
* Klikněte pravým tlačítkem na novou úlohu, kterou jste vytvořili, a pak klikněte na **Build now** (Sestavit). Spustí se sestavování. 
* Jakmile bude sestavování dokončeno, přejděte na **Console output** (Výstup konzoly). Uvidíte, že se sestavení provedlo vzdáleně v Azure.

![Výstup konzoly](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Referenční informace

* Video Azure Friday: [Průběžná integrace pomocí Jenkinse s využitím agentů virtuálních počítačů Azure](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Informace o podpoře a možnosti konfigurace: [Wikiweb modulu plug-in Azure VM Agent pro Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 

