---
title: aaaCreate image Azure Remoteappu | Microsoft Docs
description: "Další informace o hello možnosti, které jsou dostupné pro vytváření bitových kopií pro Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 54e63b6fa13addfcda96ce581910e1ac48d91e70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-remoteapp-image"></a>Vytvoření image Azure RemoteAppu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp používá aplikace hello toohold bitové kopie, které můžete sdílet s uživateli. (Jsme trvat bitové kopie a použít ho virtuálních počítačů toocreate - která hello uživatelé přístup při zápisu do Azure RemoteApp.) toocreate kolekci Azure Remoteappu s zvoleného aplikací, ať už je Cloudová nebo hybridní, začněte vytvořením bitovou kopii s těmito aplikacemi nainstalována. Pak vytvořte kolekci, která použije této bitové kopie, přiřazení uživatelů toohello kolekce a publikování aplikace toothose uživatele.

Máte několik možností pro vytvoření nebo pomocí bitové kopie. Hello základní [požadavek](remoteapp-imagereqs.md) pro bitovou kopii je, že se systémem Windows Server 2012 R2 a role Hostitel relace vzdálené plochy (RDSH) hello nainstalována. Jak získat, je, kde získat zajímavé věcí.

Máte následující možnosti při přechodu do tooimages hello:

* Můžete importovat a používat [na základě bitové kopie na virtuální počítač Azure](remoteapp-image-on-azurevm.md). Toto je vhodný pro-obchodní aplikace, které vyžadují vlastní nastavení. Můžete přizpůsobit hello toowork bitové kopie pro aplikaci hello.
* Můžete [vytvořit a nahrát vlastní image](remoteapp-create-custom-image.md). To je vhodný, pokud již máte obrázek, který používáte pro vaše nasazení služby Vzdálená plocha na místě.
* Můžete použít jednu z hello [Image šablony](remoteapp-images.md) součástí vašeho předplatného vzdálené aplikace RemoteApp. Tyto bitové kopie se vytvoří a udržuje tým služby RemoteApp hello a obsahovat některé standardní aplikace (např. hello sady Office), abyste vytvořili uživatelé k dispozici tooyour. Všimněte si, že pouze image Office 365 Pro Plus hello může být použit v produkčním prostředí.

Bez ohledu na to, kde získat bitové kopie nebo jak vytvořit, je výhodné rozumět hello toomake [požadavky aplikací na](remoteapp-appreqs.md) tooensure, které vaše aplikace funguje dobře v Remoteappu. Potom hello dalším krokem je toocreate [cloudu](remoteapp-create-cloud-deployment.md) nebo [hybridní](remoteapp-create-hybrid-deployment.md) kolekce.

