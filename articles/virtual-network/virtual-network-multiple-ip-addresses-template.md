---
title: "aaaMultiple IP adresy pro virtuální počítače Azure - šablony | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí šablony Azure Resource Manager."
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
ms.openlocfilehash: e7660257b2d5c7da4b8b86771abe51a2c5012fa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-an-azure-resource-manager-template"></a>Přiřadit více IP adres toovirtual počítače pomocí šablony Azure Resource Manager

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím nasazení Azure Resource Manager hello modelu pomocí šablony Resource Manageru. Toohello nelze přiřadit víc veřejných a privátních IP adres stejné seskupování při nasazování virtuálního počítače pomocí modelu nasazení classic hello. Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Popis šablony

Nasazení šablony vám umožní tooquickly a konzistentně vytváření prostředků Azure s hodnotami jinou konfiguraci. Čtení hello [názorný Průvodce šablonou Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek, pokud si nejste obeznámeni s šablon Azure Resource Manageru. Hello [nasazení virtuálního počítače s více IP adres](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) šablony je použity v tomto článku.

<a name="resources"></a>Nasazení šablony hello vytvoří hello následující prostředky:

|Prostředek|Name (Název)|Popis|
|---|---|---|
|Síťové rozhraní|*myNic1*|Hello tři popsané v části scénář hello tohoto článku Konfigurace protokolu IP jsou vytvořeny a přiřazeny toothis síťový adaptér.|
|Prostředek veřejné IP adresy|jsou vytvořené 2: *myPublicIP* a *myPublicIP2*|Tyto prostředky jsou přiřazeno statické veřejné IP adresy a toohello *IPConfig-1* a *IPConfig 2* konfigurace protokolu IP, které jsou popsané v hello scénáři.|
|Virtuální počítač|*myVM1*|Standardní DS3 virtuálních počítačů.|
|Virtuální síť|*myVNet1*|Virtuální síť s jednou podsítí s názvem *mySubnet*.|
|Účet úložiště|Jedinečné toohello nasazení|Účet úložiště.|

<a name="parameters"></a>Pokud nasazujete hello šablony, je nutné zadat hodnoty pro hello následující parametry:

|Name (Název)|Popis|
|---|---|
|adminUsername|Uživatelské jméno správce. Hello uživatelské jméno musí být v souladu s [požadavky na Azure uživatelské jméno](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|adminPassword|Heslo hello heslo správce musí být v souladu s [požadavky na heslo pro platformu Azure](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
|dnsLabelPrefix|Název DNS pro PublicIPAddressName1. název DNS Hello vyřeší tooone hello veřejné IP adresy přiřazené toohello virtuálních počítačů. Hello název musí být jedinečný v rámci hello Azure hello oblasti (umístění) vytvoříte virtuální počítač v.|
|dnsLabelPrefix1|Název DNS pro PublicIPAddressName2. název DNS Hello vyřeší tooone hello veřejné IP adresy přiřazené toohello virtuálních počítačů. Hello název musí být jedinečný v rámci hello Azure hello oblasti (umístění) vytvoříte virtuální počítač v.|
|OSVersion|verze Windows nebo Linuxem Hello hello virtuálních počítačů. Hello operační systém je plně opravou obrázek hello zadané vybrané verze Windows nebo Linuxem.|
|imagePublisher|Vydavatel image Windows nebo Linuxem Hello hello vybraný virtuální počítač.|
|imageOffer|Obrázek Hello Windows nebo Linuxem pro hello vybrat virtuální počítač.|

Všechny nasazené šablonou hello prostředky hello nakonfigurované s několika výchozí nastavení. Můžete zobrazit tato nastavení prostřednictvím buď hello následující metody:

- **Zobrazit šablonu hello na Githubu:** Pokud jste obeznámeni s šablony, můžete zobrazit nastavení hello v rámci hello [šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json).
- **Zobrazit nastavení hello po nasazení:** Pokud nejste obeznámeni s šablony, můžete nasadit pomocí kroků v jednom z následujících částí hello šablony hello a zobrazte nastavení hello po nasazení.

Můžete použít hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure (CLI) toodeploy hello šablony. Všechny metody vracet hello stejného výsledku. Šablona hello toodeploy, dokončení hello kroky v jednom z hello následující části:

## <a name="deploy-using-hello-azure-portal"></a>Nasazení pomocí portálu Azure hello

Šablona hello toodeploy pomocí hello portál Azure, dokončení hello následující kroky:

1. Změna hello šablony, v případě potřeby. Šablona Hello nasadí hello prostředky a nastavení jsou uvedena v hello [prostředky](#resources) tohoto článku. Další informace o šablonách toolearn a jak tooauthor je, přečtěte si hello [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json)článku.
2. Nasazení šablony hello s jedním z následujících metod hello:
    - **Vyberte hello šablony portálu hello:** dokončení hello kroky hello [nasadit prostředky z vlastní šablony](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template) článku. Vyberte existující šablonu hello s názvem *101-vm několika ipconfig*.
    - **Přímo:** klikněte na následující tlačítko tooopen hello šablony přímo na portálu hello hello:<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Bez ohledu na to metoda hello si zvolíte, bude nutné toosupply hodnoty pro hello [parametry](#parameters) uvedené dříve v tomto článku. Po nasazení hello virtuální počítač připojit toohello virtuálního počítače a přidejte hello privátní IP adresy toohello operačního systému jste nasadili hello provedením kroků v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejné IP adresy toohello operačního systému.

## <a name="deploy-using-powershell"></a>Nasazení pomocí prostředí PowerShell

Šablona hello toodeploy pomocí prostředí PowerShell, dokončení hello následující kroky:

1. Nasadit šablony hello pomocí kroků hello v hello [nasazení šablony v prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy-cli.md) článku. Hello článek popisuje několik možností nasazení šablony. Pokud se rozhodnete toodeploy pomocí hello `-TemplateUri parameter`, hello identifikátor URI pro tuto šablonu je *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Pokud se rozhodnete toodeploy pomocí hello `-TemplateFile` parametr, kopírovat obsah hello hello [soubor šablony](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) z Githubu do nového souboru na vašem počítači. Upravte obsah hello šablony, v případě potřeby. Šablona Hello nasadí hello prostředky a nastavení jsou uvedena v hello [prostředky](#resources) tohoto článku. Další informace o šablonách toolearn a jak tooauthor je, přečtěte si hello [šablon pro tvorbu Azure Resource Manageru ](../azure-resource-manager/resource-group-authoring-templates.md)článku.

    Bez ohledu na to, zvolte šablonu hello toodeploy s hello možnost, je nutné zadat hodnoty uvedené v hello hodnoty parametrů hello [parametry](#parameters) tohoto článku. Pokud si zvolíte toosupply parametry s využitím soubor parametrů, kopírovat obsah hello hello [soubor parametrů](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) z Githubu do nového souboru ve vašem počítači. Upravte hodnoty hello v souboru hello. Použijte soubor hello jste vytvořili jako hodnota hello hello `-TemplateParameterFile` parametr.

    toodetermine platné hodnoty pro hello OSVersion, ImagePublisher a imageOffer parametry, dokončení hello kroky hello [vyhledání a vyberte článek bitové kopie virtuálního počítače s Windows](../virtual-machines/windows/cli-ps-findimage.md) článku.

    >[!TIP]
    >Pokud si nejste jistí, jestli je k dispozici dnslabelprefix, zadejte hello `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` toofind příkaz limitu. Pokud je k dispozici, vrátí příkaz hello `True`.

2. Po nasazení hello virtuální počítač připojit toohello virtuálního počítače a přidejte hello privátní IP adresy toohello operačního systému jste nasadili hello provedením kroků v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejné IP adresy toohello operačního systému.

## <a name="deploy-using-hello-azure-cli"></a>Nasazení pomocí hello rozhraní příkazového řádku Azure

Šablona hello toodeploy pomocí hello Azure CLI 1.0, dokončení hello následující kroky:

1. Nasadit šablony hello pomocí kroků hello v hello [nasazení šablony s hello rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) článku. Hello článek popisuje více možností pro nasazení šablony hello. Pokud se rozhodnete toodeploy pomocí hello `--template-uri` (-f), hello identifikátor URI pro tuto šablonu je *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Pokud se rozhodnete toodeploy pomocí hello `--template-file` (-f) parametr, kopírovat obsah hello hello [soubor šablony](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) z Githubu do nového souboru na vašem počítači. Upravte obsah hello šablony, v případě potřeby. Šablona Hello nasadí hello prostředky a nastavení jsou uvedena v hello [prostředky](#resources) tohoto článku. Další informace o šablonách toolearn a jak tooauthor je, přečtěte si hello [šablon pro tvorbu Azure Resource Manageru ](../azure-resource-manager/resource-group-authoring-templates.md)článku.

    Bez ohledu na to, zvolte šablonu hello toodeploy s hello možnost, je nutné zadat hodnoty uvedené v hello hodnoty parametrů hello [parametry](#parameters) tohoto článku. Pokud si zvolíte toosupply parametry s využitím soubor parametrů, kopírovat obsah hello hello [soubor parametrů](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) z Githubu do nového souboru ve vašem počítači. Upravte hodnoty hello v souboru hello. Použijte soubor hello jste vytvořili jako hodnota hello hello `--parameters-file` (-e) parametr.

    toodetermine platné hodnoty pro hello OSVersion, ImagePublisher a imageOffer parametry, dokončení hello kroky hello [vyhledání a vyberte článek bitové kopie virtuálního počítače s Windows](../virtual-machines/windows/cli-ps-findimage.md) článku.

2. Po nasazení hello virtuální počítač připojit toohello virtuálního počítače a přidejte hello privátní IP adresy toohello operačního systému jste nasadili hello provedením kroků v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejné IP adresy toohello operačního systému.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
