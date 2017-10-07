---
title: "požadavky na image vzdálené aplikace RemoteApp aaaAzure | Microsoft Docs"
description: "Další informace o hello požadavky pro vytvoření bitové kopie toobe použít s Azure Remoteappem"
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
ms.openlocfilehash: 4e35203eb93a866d4e0bd591d42b34746c7ffa4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a>Požadavky pro Azure RemoteApp bitové kopie
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp používá všechny hello programy, které chcete tooshare s uživateli toohost bitové kopie systému Windows Server 2012 R2. toocreate vlastní image, můžete spustit pomocí stávající image nebo [vytvořte novou](remoteapp-create-custom-image.md).

> [!TIP]
> Věděli jste, že vaše Azure RemoteApp předplatné vám umožňuje přístup tooa bitové kopie systému Windows Server 2012 R2 v hello Galerie virtuálního počítače Azure, které můžete použít toocreate vlastního image šablony? [Projděte si](remoteapp-image-on-azurevm.md).  
> 
> 

požadavky na Hello hello image, která mohou být nahrány pro použití s Azure Remoteappem jsou:

* Vlastní aplikace neukládejte data místně na bitovou kopii hello. Tyto Image jsou bezstavové a smí obsahovat pouze aplikace.
* bitová kopie Hello neobsahuje data, která mohou být ztraceny.
* velikost bitové kopie Hello musí být násobkem MB. Pokud se pokusíte tooupload obrázek, který není přesný více, nahrávání hello se nezdaří.
* velikost obrazu Hello musí být 127 GB nebo menší.
* Musí být na soubor virtuálního pevného disku (VHDX soubory nejsou aktuálně podporovány.).
* Hello virtuálního pevného disku nesmí být virtuální počítač generace 2.
* Hello virtuální pevný disk může být buď pevné velikosti, nebo dynamicky se zvětšující. Dynamicky se zvětšující virtuální pevný disk je doporučená, protože trvá méně času tooupload tooAzure než soubor virtuálního pevného disku pevné velikosti.
* Hello disku musí být inicializován pomocí rozdělení styl hello hlavní spouštěcí záznam (MBR). Hello stylem oddílů GUID (GPT) oddílu není podporováno.
* Hello virtuální pevný disk musí obsahovat jeden instalace systému Windows Server 2012 R2. Může obsahovat několik svazků, ale jenom jeden s obsahuje instalace systému Windows.
* musí být nainstalován Hello hostitele relace vzdálené plochy (RDSH) role a funkce Možnosti práce s počítačem hello.
* role Zprostředkovatel připojení ke vzdálené ploše Hello musí *není* nainstalovat.
* musí se zakázat Hello systém souborů (Encrypting File System).
* Hello bitová kopie musí být SYSPREPed pomocí parametrů hello **/oobe / generalize/shutdown** (nesmí použít hello **/mode:vm** parametr).
* Odesílání svůj disk VHD z řetěz snímku není podporována.

V tématu [vytvoření image Azure Remoteappu](remoteapp-imageoptions.md) Další informace o vytváření bitových kopií pro Azure RemoteApp.

