---
title: "aaaHow toomigrate z virtuální sítě vzdálené aplikace RemoteApp tooan virtuální síť Azure | Microsoft Docs"
description: "Zjistěte, jak toomigrate z virtuální sítě vzdálené aplikace RemoteApp tooan virtuální síť Azure"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: baea5d29-353b-48f8-b47f-806f2163e067
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: c0f8617556c6f1e33eca8322febf67ff33937ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-a-hybrid-collection-from-a-remoteapp-vnet-tooan-azure-vnet"></a>Jak toomigrate hybridní kolekci z virtuální sítě vzdálené aplikace RemoteApp tooan virtuální síť Azure
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Dobrá zpráva! Jsme jste povolili toodeploy hybridní kolekce vzdálené aplikace RemoteApp přímo do vaší stávající virtuální sítě Azure (virtuální sítě) místo vytvoření virtuální sítě vzdálené aplikace RemoteApp specifické. To vám umožní využít výhod hello nejnovější funkce virtuální síť (jako je ExpressRoute) a poskytnout vaší hybridní kolekce přímého síťového přístupu tooother služby Azure a virtuální počítače nasazené toothat virtuální sítě.  (To získá je lepší výkon a jednodušší nastavení než konfigurace propojení VNET-to-VNET).

Řekněme, že jste už vytvořili hybridní kolekce vzdálené aplikace RemoteApp názvem *OriginalCollection* v virtuální sítě vzdálené aplikace RemoteApp označuje jako *RemoteAppVNET*. Tady jsou kroky toomigrate hello ho tooa novou virtuální síť Azure volá *AzureVNET*.

1. Na hello **sítě** ve hello [portálu pro správu](http://manage.windowsazure.com/), vytvořte virtuální síť, která volá *AzureVNET*pomocí hello stejné umístění, konfigurace serveru DNS a adresní prostor (pro minimálně jeden Dobrý den *AzureVNET* podsítě) jako jste použili pro *RemoteAppVNET*.
2. Konfigurace *AzureVNET* tooeither hostitele nebo mít nasazení služby Active Directory toohello připojení k síti, *OriginalCollection* je připojený k doméně do.
3. Na hello **vzdálené aplikace RemoteApp** kartě, vytvořit novou kolekci vzdálené aplikace RemoteApp názvem *nové kolekce*. (Použití hello **vytvořit s virtuální síť** možnost, není **rychle vytvořit**.)
4. Konfigurace *NewCollection* toobe nasazené tooa podsítě v *AzureVNET*.
5. Konfigurace *NewCollection* toouse hello stejnou bitovou kopii a informace o připojení k doméně jako jste použili pro *OriginalCollection*.
6. Po několik hodin *NewCollection* se zobrazí v seznamu kolekce s aktivním stavu.

Nyní pokud nepotřebujete toomigrate nějaké informace o uživateli z hello původní kolekce toohello nové kolekce, proveďte tyto kroky dále:

1. Odstranit *OriginalCollection*.
2. Odstranit *RemoteAppVNET*.

A jste hotovi!

Případně pokud potřebujete informace o uživateli toomigrate z hello původní kolekce toohello nové kolekce, proveďte tyto kroky dále:

1. Odeslat e-mail příliš[ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) s svoje ID předplatného Azure hello název původní kolekci a hello název nové kolekce a požádejte je toomigrate informace o uživateli.
2. V rámci 2 pracovních dnů přesune tým služby RemoteApp hello hello uživatel přístup k seznamu a všechny dokumenty uživatele a nastavení uživatele z hello původní kolekce toohello nové kolekce.
3. Odstranit *OriginalCollection*.
4. Odstranit *RemoteAppVNET*.

A teď jste hotovi!

Pokud máte nějaké otázky nebo potřebujete speciální pomoc, pošlete e-mail [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).

