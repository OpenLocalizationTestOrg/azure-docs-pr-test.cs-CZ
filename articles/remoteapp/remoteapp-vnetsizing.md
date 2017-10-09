---
title: "informace o aaaSizing pro virtuální sítě v Azure Remoteappu | Microsoft Docs"
description: "Další informace o hello požadavky na IP adresu pro virtuální síť s Azure RemoteApp"
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
ms.openlocfilehash: f98b831af32c41740b258d122b3e18765be08d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informace o nastavení velikosti pro virtuální sítě v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Pokud používáte Azure RemoteApp s virtuální sítí (VNET), vzdálené aplikace RemoteApp používá IP adresy v rámci podsítě hello. Podle hello škálování služby vzdálené aplikace RemoteApp, je nutné tooensure že podsíť má dost IP adres, které jsou k dispozici pro virtuální počítače vzdálené aplikace RemoteApp. Při této pokyny k dimenzování není ideální daného jak vzdálené aplikace RemoteApp dynamicky otáčí nahoru a dolů virtuálních počítačů v rámci kolekce otáčí, pomůže vám odhad vaší rozsahu podsítě. To je obzvláště důležité, protože po služby vzdálené aplikace RemoteApp je umístěn ve virtuální síti, nelze zvýšit velikost podsítě hello bez odebrání vzdálené aplikace RemoteApp.

Pro každou kolekci vzdálené aplikace RemoteApp, že chcete toorun při maximální kapacitě měli byste mít 100 IP adresy. Například pokud máte v plánu Standard hello jednu kolekci vzdálené aplikace RemoteApp a chcete toohave hello maximální 500 uživatelů, měli byste 100 IP adresy pro tuto kolekci. Podobně platí musíte 100 IP adresy pro kolekci vzdálené aplikace RemoteApp v hello základní plán, který má 800 uživatele. Pokud máte v plánu toohave méně uživatelů (menší než maximální hello), můžete snížit hello IP adresy, které potřebuje na kolekci. požadavek na velikost minimální podsíť Hello je 30 IP adres (/ 27).

Projděte si následující informace toomake se, že jsou vaše virtuální síť nakonfigurované a práci propertly hello:

* [Migrace z osobní tooan virtuální sítě Azure VNET](remoteapp-migratevnet.md)
* [Ověření hello toouse virtuální síť Azure s Azure Remoteappem](remoteapp-vnet.md)

