---
title: "Požadavky image Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích pro vytvoření bitové kopie, který se má použít s Azure Remoteappem"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7cbb90f4-6dc9-462c-a429-088cdb57414e
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 75b0f8d6b25a80f11002b683152cfb294cbb68bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a>Požadavky pro Azure RemoteApp bitové kopie
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp používá bitovou kopii systému Windows Server 2012 R2 k hostování všechny programy, které chcete sdílet s uživateli. Pokud chcete vytvořit vlastní image, můžete spustit pomocí stávající image nebo [vytvořte novou](remoteapp-create-custom-image.md).

> [!TIP]
> Věděli jste, že vaše předplatné Azure Remoteappu umožňuje přístup k systému Windows Server 2012 R2 obrázek v galerii virtuálních počítačů Azure, který můžete použít k vytvoření vlastního image šablony? [Projděte si](remoteapp-image-on-azurevm.md).  
> 
> 

Požadavky na bitovou kopii, která mohou být nahrány pro použití s Azure Remoteappem jsou:

* Vlastní aplikace neukládejte data místně na bitovou kopii. Tyto Image jsou bezstavové a smí obsahovat pouze aplikace.
* Obrázek neobsahuje data, která mohou být ztraceny.
* Velikost obrázku musí být násobkem MB. Pokud se pokusíte odeslat obrázek, který není přesný více, nahrávání se nezdaří.
* Velikost obrázku musí být 127 GB nebo menší.
* Musí být na soubor virtuálního pevného disku (VHDX soubory nejsou aktuálně podporovány.).
* Virtuální pevný disk nesmí být virtuální počítač generace 2.
* Virtuální pevný disk může být buď pevné velikosti, nebo dynamicky se zvětšující. Dynamicky se zvětšující virtuální pevný disk je doporučená, protože trvá méně času k nahrání do Azure, než soubor virtuálního pevného disku pevné velikosti.
* Disk musí být inicializován pomocí hlavní spouštěcí záznam (MBR) oddílů. Stylem oddílů GUID (GPT) není podporována.
* Virtuální pevný disk musí obsahovat jeden instalace systému Windows Server 2012 R2. Může obsahovat několik svazků, ale jenom jeden s obsahuje instalace systému Windows.
* Musí být nainstalována role Hostitel relace vzdálené plochy (RDSH) a funkci Možnosti práce s počítačem.
* Role Zprostředkovatel připojení ke vzdálené ploše musí *není* nainstalovat.
* Musí se zakázat systému souborů EFS (ENCRYPTING File System).
* Obrázek musí být SYSPREPed pomocí parametrů **/oobe / generalize/shutdown** (se nepoužije **/mode:vm** parametr).
* Odesílání svůj disk VHD z řetěz snímku není podporována.

V tématu [vytvoření image Azure Remoteappu](remoteapp-imageoptions.md) Další informace o vytváření bitových kopií pro Azure RemoteApp.

