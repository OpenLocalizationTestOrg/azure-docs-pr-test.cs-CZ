---
title: "aaaSet až toocreate prostředí PowerShell virtuálního počítače pro hello Marketplace | Microsoft Docs"
description: "Pokyny pro nastavení prostředí Azure PowerShell a jeho použití jako volitelný proces toku toodeploy bitové kopie toocreate virtuálních počítačů a prodeje na, hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: cd2ebad7472248b8f921706e1a8c82d41f33b9cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-powershell-toocreate-an-offer-for-hello-azure-marketplace"></a>Nastavení prostředí Azure PowerShell toocreate nabídku hello Azure Marketplace
Podrobné informace o tom, tooset až prostředí PowerShell v Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Jednoduchý přístup je toouse hello certifikát metodu, která se stáhne a importuje certifikát potřebný pro ověřování. tooobtain hello potřeby certifikát, použijte hello **Get-AzurePublishSettingsFile** rutiny. Když se zobrazí výzva, uložte soubor hello. certifikát hello tooimport do relace prostředí PowerShell, použijte hello **Import AzurePublishSettingsFile** rutiny.

tooconfigure a úložiště hello běžné Microsoft Azure předplatné nastavení pro relaci prostředí PowerShell hello používat hello **Set-AzureSubscription** a **Select-AzureSubscription** rutiny:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

První příkaz Hello přidruží výchozí účet úložiště s předplatným hello (vyžadována pro některé operace zřizování virtuálních počítačů).  Hello druhý díky hello předplatné hello stávající (rozpoznán jinými rutinami).

## <a name="see-also"></a>Viz také
* [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)
* [Vytváření bitové kopie virtuálního počítače pro hello Marketplace.](marketplace-publishing-vm-image-creation.md)

