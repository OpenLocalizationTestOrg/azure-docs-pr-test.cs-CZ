---
title: "aaaAzure SDK poznámky k verzi rozhraní .NET 3.0 | Microsoft Docs"
description: "Poznámky k verzi sady Azure SDK pro rozhraní .NET 3.0"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: 8970b4c9b64de40dc29a33d69006a00ae5e38a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a>Poznámky k verzi sady Azure SDK pro rozhraní .NET 3.0

Toto téma obsahuje poznámky k verzi pro verzi 3.0 hello Azure SDK pro .NET.

##<a name="azure-sdk-for-net-30-release-summary"></a>Azure SDK pro verzi rozhraní .NET 3.0 souhrn

Datum vydání: 03/07/2017
 
V této verzi byly zavedeny žádné narušující změny toohello Azure SDK 3.0. Je také žádné tooleverage procesu upgradu potřeba tuto sadu SDK s existující projekty cloudové služby. použití tooallow hello Azure SDK 3.0 bez nutnosti upgradu procesu, Azure SDK 3.0 nainstaluje toohello stejných adresářích jako sadu Azure SDK 2.9. Většina komponent hello nezměnila hello hlavní verzi z 2.9, ale místo toho právě aktualizovat číslo sestavení hello.

## <a name="visual-studio-2017-rtw"></a>Visual Studio 2017 RTW

- V aplikaci Visual Studio 2017 tato verze hello Azure SDK pro .NET je součástí toohello pracovního vytížení Azure. Všechny hello nástroje, které potřebujete toodo Azure development budou součástí Visual Studio 2017 do budoucna. Pro Visual Studio 2015 hello SDK bude stále k dispozici prostřednictvím WebPI. Azure SDK pro .NET verze jsme mít nepoužívá pro Visual Studio 2013 teď, když byla vydána Visual Studio 2017.

### <a name="azure-diagnostics"></a>Diagnostika Azure

- Změněné hello chování tooonly ukládat částečné připojovací řetězec s klíčem hello nahrazuje token pro připojovací řetězec úložiště diagnostiky cloudové služby. klíč Hello skutečné úložiště jsou teď uložená v složky profilu uživatele hello tak přístup se dá řídit. Visual Studio, bude číst klíč úložiště hello ze složky profilu uživatele pro místní ladění a proces publikování. 
- V odpovědi toohello změn popsané výše Visual Studio Online team rozšířené hello šablonu úloh nasazení Azure Cloud Services, uživatelé mohou zadat klíč hello úložiště pro nastavení rozšíření diagnostiky při publikování tooAzure v průběžnou integraci a nasazení.
- Provedli jsme ji možné toostore zabezpečené připojovací řetězec a tokenizaci pro Azure Diagnostics (WAD), toohelp řešení problémů s konfigurací napříč environements.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuální počítače

- Visual Studio teď podporuje nasazení cloudové služby tooOS rodiny 5 (Windows Server 2016) virtuálních počítačů. Pro existující cloudové služby, můžete změnit vaše nastavení tootarget hello nové řada operačního systému. Při vytváření nových cloudových služeb, pokud se rozhodnete toocreate hello služby pomocí rozhraní .net 4.6 nebo vyšší, bude použita výchozí služba toouse hello operačního systému rodiny 5.  Další informace, můžete zkontrolovat hello [skupina hostovaných operačních systémů podporují tabulky](../cloud-services/cloud-services-guestos-update-matrix.md).

### <a name="known-issues"></a>Známé problémy

- Azure .NET SDK 3.0 zavedla problém při odebírání Visual Studio 2017 v konfiguraci vedle sebe s Visual Studiem 2015.  Pokud máte hello Azure SDK pro Visual Studio 2015 nainstalována, hello emulátor úložiště Microsoft Azure a Microsoft Azure výpočetní emulátor budou odebrány, pokud odinstalujete Visual Studio 2017.  To způsobí chybu při vytváření a ladění nové projekty cloudových služeb v sadě Visual Studio 2015. V pořadí toofix tento problém, přeinstalujte hello Azure SDK z hello instalačního programu webové platformy.  Hello problém bude vyřešen v budoucí aktualizaci Visual Studio 2017.  .

 
### <a name="azure-in-role-cache"></a>Mezipaměť hostovaná v instanci Role na Azure 

- Podpora pro mezipaměť hostovaná v instanci Role Azure skončila 30. listopadu 2016. Další podrobnosti získáte kliknutím na [zde](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).




