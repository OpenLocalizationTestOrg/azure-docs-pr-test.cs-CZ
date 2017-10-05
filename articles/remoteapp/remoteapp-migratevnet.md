---
title: "Jak migrovat z virtuální sítě vzdálené aplikace RemoteApp o virtuální síť Azure | Microsoft Docs"
description: "Zjistěte, jak migrovat z virtuální sítě vzdálené aplikace RemoteApp o virtuální síť Azure"
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
ms.openlocfilehash: 10b5f4844a38fe97852dee8634e8cf54f1a23a1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Jak migrovat hybridní kolekce z virtuální sítě vzdálené aplikace RemoteApp o virtuální síť Azure
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Dobrá zpráva! Jsme povolili můžete nasadit hybridní kolekce vzdálené aplikace RemoteApp přímo do vaší stávající virtuální sítě Azure (virtuální sítě) místo vytvoření virtuální sítě vzdálené aplikace RemoteApp specifické. To vám umožní využívat výhody nejnovějších funkcí virtuální síť (jako je ExpressRoute) a poskytnout přímý přístup k síti v případě hybridních kolekcí k dalším službám Azure a virtuální počítače nasazené do ní.  (To získá je lepší výkon a jednodušší nastavení než konfigurace propojení VNET-to-VNET).

Řekněme, že jste už vytvořili hybridní kolekce vzdálené aplikace RemoteApp názvem *OriginalCollection* v virtuální sítě vzdálené aplikace RemoteApp označuje jako *RemoteAppVNET*. Tady jsou kroky k migraci na nové virtuální sítě Azure volá *AzureVNET*.

1. Na **sítě** kartě v [portálu pro správu](http://manage.windowsazure.com/), vytvořit virtuální síť, která volá *AzureVNET*, pomocí stejného umístění, konfigurace serveru DNS a adresní prostor (pro minimálně jeden z *AzureVNET* podsítě) jako jste použili pro *RemoteAppVNET*.
2. Konfigurace *AzureVNET* na hostitele nebo mít síťové připojení k nasazení služby Active Directory, *OriginalCollection* je připojený k doméně do.
3. Na **vzdálené aplikace RemoteApp** kartě, vytvořit novou kolekci vzdálené aplikace RemoteApp názvem *nové kolekce*. (Použití **vytvořit s virtuální síť** možnost, není **rychle vytvořit**.)
4. Konfigurace *NewCollection* k nasazení pro podsíť v *AzureVNET*.
5. Konfigurace *NewCollection* použít stejnou bitovou kopii a informace o připojení k doméně, jako jste použili pro *OriginalCollection*.
6. Po několik hodin *NewCollection* se zobrazí v seznamu kolekce s aktivním stavu.

Nyní pokud nepotřebujete migrovat nějaké informace o uživateli z původní kolekci do nové kolekce, proveďte tyto kroky dále:

1. Odstranit *OriginalCollection*.
2. Odstranit *RemoteAppVNET*.

A jste hotovi!

Případně pokud potřebujete migrovat informace o uživateli z původní kolekci do nové kolekce, proveďte tyto kroky dále:

1. E-mailovou zprávu na [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) svoje ID předplatného Azure, názvem vašeho původního kolekce a s názvem vaší nové kolekce a požádejte je o migrovat informace o uživateli.
2. Do 2 pracovních dnů se tým služby RemoteApp přesune přístup k seznamu uživatelů a všechny dokumenty uživatele a nastavení uživatele z původní kolekce do nové kolekce.
3. Odstranit *OriginalCollection*.
4. Odstranit *RemoteAppVNET*.

A teď jste hotovi!

Pokud máte nějaké otázky nebo potřebujete speciální pomoc, pošlete e-mail [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).

