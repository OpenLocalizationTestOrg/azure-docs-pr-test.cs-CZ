---
title: "Vytvoření image Azure Remoteappu | Microsoft Docs"
description: "Další informace o dostupných možnostech pro vytváření bitových kopií pro Azure RemoteApp"
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
ms.openlocfilehash: 4b8ba6f264f982e03930c5ad4ccdb2d80f2c8665
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-remoteapp-image"></a>Vytvoření image Azure RemoteAppu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp používá Image s aplikacemi, které můžete sdílet s uživateli. (Jsme trvat bitové kopie a použít ho k vytvoření virtuálních počítačů -, které je přístup uživatelé při zápisu do Azure RemoteApp.) Pomocí zvoleného aplikací, vytvořit kolekci Azure RemoteApp, jestli je Cloudová nebo hybridní, začněte vytvořením obrázku s těmito aplikacemi nainstalována. Pak vytvořte kolekci, která použije této bitové kopie, přiřazení uživatelů ke kolekci a publikování aplikací pro tyto uživatele.

Máte několik možností pro vytvoření nebo pomocí bitové kopie. Základní [požadavek](remoteapp-imagereqs.md) pro bitovou kopii je, že se systémem Windows Server 2012 R2 a role Hostitel relace vzdálené plochy (RDSH) nainstalována. Jak získat, je, kde získat zajímavé věcí.

Pokud jde o Image máte následující možnosti:

* Můžete importovat a používat [na základě bitové kopie na virtuální počítač Azure](remoteapp-image-on-azurevm.md). Toto je vhodný pro-obchodní aplikace, které vyžadují vlastní nastavení. Můžete přizpůsobit obrázek, který má fungovat pro aplikaci.
* Můžete [vytvořit a nahrát vlastní image](remoteapp-create-custom-image.md). To je vhodný, pokud již máte obrázek, který používáte pro vaše nasazení služby Vzdálená plocha na místě.
* Můžete použít jednu z [Image šablony](remoteapp-images.md) součástí vašeho předplatného vzdálené aplikace RemoteApp. Tyto bitové kopie se vytvoří a udržuje tým vzdálené aplikace RemoteApp a obsahovat některé standardní aplikace (např. v sadě Office), které můžete zpřístupnit uživatelům. Všimněte si, že lze použít pouze image Office 365 Pro Plus v produkčním prostředí.

Bez ohledu na to, kde získat bitové kopie nebo jak vytvořit, budete chtít musíte rozumět [požadavky aplikací na](remoteapp-appreqs.md) zajistit, že aplikace funguje dobře v Remoteappu. Pak je dalším krokem vytvoření [cloudu](remoteapp-create-cloud-deployment.md) nebo [hybridní](remoteapp-create-hybrid-deployment.md) kolekce.

