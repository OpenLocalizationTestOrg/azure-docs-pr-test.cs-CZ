---
title: "Změna velikosti informace pro virtuální síť v Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích na IP adresu pro virtuální síť s Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b6e1c4ba-0236-42b2-bced-69353bf211be
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9375981db64ec4a1ae523e958423b5f5787cec33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informace o nastavení velikosti pro virtuální sítě v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Pokud používáte Azure RemoteApp s virtuální sítí (VNET), vzdálené aplikace RemoteApp používá IP adresy v rámci podsítě. Závislosti na rozsahu vaší služby vzdálené aplikace RemoteApp, musíte zajistit, aby podsíť není dost IP adres, které jsou k dispozici pro virtuální počítače vzdálené aplikace RemoteApp. Při této pokyny k dimenzování není ideální daného jak vzdálené aplikace RemoteApp dynamicky otáčí nahoru a dolů virtuálních počítačů v rámci kolekce otáčí, pomůže vám odhad vaší rozsahu podsítě. To je obzvláště důležité, protože po služby vzdálené aplikace RemoteApp je umístěn ve virtuální síti, nelze zvýšit velikost podsítě bez odebrání vzdálené aplikace RemoteApp.

Pro každou kolekci vzdálené aplikace RemoteApp, která se má spustit při maximální kapacitě měli byste mít 100 IP adresy. Například pokud máte v plánu Standard jednu kolekci vzdálené aplikace RemoteApp a budete chtít mít maximální 500 uživatelů, byste měli mít 100 IP adresy pro tuto kolekci. Podobně platí musíte 100 IP adresy pro kolekci vzdálené aplikace RemoteApp v základní plán, který má 800 uživatele. Pokud budete chtít mít méně uživatele (je menší než maximální), můžete snížit IP adresy, potřebuje na kolekci. Požadavek na minimální podsíť velikost je 30 IP adres (/ 27).

Podívejte se na následující informace a ujistěte se virtuální síť je konfigurována a funkční propertly:

* [Migrace z osobní virtuální sítě do Azure VNET](remoteapp-migratevnet.md)
* [Ověření virtuální síť Azure pro použití s Azure RemoteApp](remoteapp-vnet.md)

