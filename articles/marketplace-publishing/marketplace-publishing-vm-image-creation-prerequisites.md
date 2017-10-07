---
title: "aaaTechnical požadavky pro vytvoření bitové kopie virtuálního počítače pro hello Azure Marketplace | Microsoft Docs"
description: "Pochopení hello požadavky pro vytváření a nasazování toohello bitové kopie virtuální počítač Azure Marketplace pro ostatní toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: fcae4d9e052581e3c1dfe7962e6d50ec31040419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-hello-azure-marketplace"></a>Technické požadavky pro vytvoření bitové kopie virtuálního počítače pro hello Azure Marketplace
Přečtěte si hello proces důkladně před zahájením a pochopit, kde a proč se provádí každý krok. Co nejvíce, měli byste Příprava informací o společnosti a další data, stáhněte potřebné nástroje, a vytvořit technické součásti před zahájením procesu vytváření nabídka hello. Tyto položky musí být vymazat z kontrola v tomto článku.  

## <a name="download-needed-tools--applications"></a>Stáhněte potřebné nástroje & aplikace
Měli byste hello následující položky, které jsou připravené před zahájením procesu hello:

* V závislosti na tom, jaký operační systém cílení, instalace hello [rutin prostředí Azure PowerShell](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) nebo [Linux rozhraní příkazového řádku nástroje](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) z hello [Azure stáhne](https://azure.microsoft.com/downloads/) stránka.
* Nainstalujte Azure Storage Explorer na webu CodePlex.
* Stáhněte a nainstalujte hello nástroj pro testování certifikační pro certifikaci Azure:
  * [http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Je nutné nástroj Certifikační hello toorun počítače se systémem Windows. Pokud jste počítači se systémem Windows k dispozici, můžete spustit nástroj hello pomocí systému Windows virtuálního počítače v Azure.

## <a name="platforms-supported"></a>Podporované platformy
Můžete vyvíjet založené na Azure virtuální počítače v systému Windows nebo Linux. Některé prvky hello publikování proces – například vytváření služby Azure kompatibilní virtuální pevný disk (VHD) – pomocí různých nástrojů a kroky v závislosti na tom, jaký operační systém, kterou používáte:  

* Pokud používáte Linux, najdete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (systémem Linux)" toohello Dobrý den [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).
* Pokud používáte systém Windows, naleznete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (založené na Windows)" toohello Dobrý den [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> Potřebujete přístup tooa založené na Windows počítače:
> 
> * Spusťte nástroj ověření certifikační hello.
> * Vytvořte adresu URL pro hello sdíleného virtuálního pevného disku přístup podpis pro hello odesílání virtuálního pevného disku certifikační.
> 
> 

## <a name="develop-your-vhd"></a>Vytvořte svůj disk VHD
Můžete vyvíjet virtuální pevné disky Azure v cloudu hello nebo místní:

* Vývoj v cloudu znamená, že všechny kroky vývoj se vzdáleně na virtuální pevný disk uložené v Azure.
* Místní vývoj vyžaduje stahování virtuální pevný disk a vývoj pomocí místní infrastruktury. I když je to možné, nedoporučujeme ji. Nezapomeňte, že vývoj pro Windows nebo SQL místní vyžaduje můžete kódy VLK relevantní místní toohave hello. Nelze zahrnout nebo po vytvoření virtuálního počítače nainstalovat systém SQL Server. Vaši nabídku musí také založit na bitovou kopii schválené SQL hello portálu Azure. Pokud se rozhodnete toodevelop místně, je nutné provést některé kroky jinak než pokud byly vývoj v cloudu hello. Může hledání příslušných informací v [vytvořit bitovou kopii virtuálního počítače místní](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
