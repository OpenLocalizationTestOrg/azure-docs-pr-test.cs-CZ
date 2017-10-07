---
title: "aaaSetting si pověření ověřování s názvem | Microsoft Docs"
description: "Zjistěte, jak tootooprovide přihlašovací údaje, sady Visual Studio můžete použít tooauthenticate požadavky tooAzure toopublish tooAzure aplikace z Visual Studio nebo toomonitor existující cloudové služby... "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 3cc147a2f7a3e766293ca282f56078cf24281346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-named-authentication-credentials"></a>Nastavení pověření s názvem ověřování
toopublish tooAzure aplikace z Visual Studio nebo toomonitor stávající cloudovou službu, je třeba zadat pověření, Visual Studio můžete použít tooauthenticate požadavky tooAzure. Existuje několik míst v sadě Visual Studio, kde se můžete přihlásit tooprovide tyto přihlašovací údaje. Například z hello Průzkumníka serveru, můžete otevřít hello místní nabídku pro hello **Azure** uzel a zvolte **připojení tooMicrosoft předplatné Azure...** . Při přihlášení, informace o předplatném hello přidružené k účtu Azure je k dispozici v sadě Visual Studio a není nic jiného, že máte toodo.

Nástroje Azure také podporuje starší způsob, jak poskytnout přihlašovací údaje, pomocí hello předplatné soubor (soubor .publishsettings). Toto téma popisuje tuto metodu, který ještě není podporovaný v Azure SDK 2.2.

jsou požadovány pro ověřování tooAzure Hello následující položky:

* Vaše ID odběru
* Platný certifikát X.509 v3

> [!NOTE]
> Délka Hello certifikát X.509 v3 hello klíče musí být alespoň 2 048 bitů. Azure odmítnou libovolný certifikát, který nesplňuje tento požadavek nebo který není platný.
>
>

Visual Studio použije svoje ID předplatného společně s hello data certifikátu jako přihlašovací údaje. Hello příslušná pověření se odkazuje v hello předplatné soubor (.publishsettings), který obsahuje veřejný klíč pro certifikát hello. soubor odběru Hello může obsahovat přihlašovací údaje pro více než jedno předplatné.

Můžete upravit informace o předplatném hello z hello **nové či upravit předplatné** dialogové okno, jak je popsáno dále v tomto tématu.

Pokud chcete toocreate certifikát sami, najdete pokyny toohello v [vytvoření a nahrání certifikátu pro správu pro Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) a ručně odeslat hello certifikát toohello [portál Azure classic ](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Tyto přihlašovací údaje, které sada Visual Studio vyžaduje toomanage, které nejsou cloudové služby hello stejné přihlašovací údaje, které jsou požadované tooauthenticate požadavek hello služby Azure storage.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Import, nastavit nebo upravit pověření pro ověření v sadě Visual Studio
Můžete importovat, nastavit nebo změnit pověření pro ověření v hello **nové předplatné** dialogové okno, které se zobrazí, pokud provádíte hello následující akce:

* V **Průzkumníka serveru**, otevřete hello místní nabídku pro hello **Azure** uzlu, zvolte **spravovat a odběry filtru...** , zvolte hello **certifikáty** kartě a proveďte jednu z následujících hello:

    * Zvolte **importovat** tooopen hello předplatná Microsoft Azure Import dialogové okno kde soubor můžete stáhnout hello odběry pro hello aktuálně načíst předplatného, procházení tooits umístění souboru ke stažení a naimportujte ho pro použití v ověřování.
    * Zvolte **nový** tooopen hello nové předplatné dialogu, kde můžete nastavit nový odběr pro použití v ověřování.
    * Zvolte **upravit** (po výběru aktivní předplatné) hello dialogové okno Upravit předplatné tooopen kde můžete upravit stávající předplatné pro použití při ověřování. 

Hello následujícího postupu se předpokládá, že hello **nové předplatné** dialogového okna.

### <a name="tooset-up-authentication-credentials-in-visual-studio"></a>tooset si pověření pro ověření v sadě Visual Studio
1. V hello **vybrat existující certifikát** pro ověření seznamu zvolte certifikát.
2. Zvolte hello **kopírovat hello úplnou cestu** odkaz. Cesta Hello hello certifikát (soubor .cer) je zkopírovaný toohello schránky.

   > [!IMPORTANT]
   > toopublish vaše aplikace Azure ze sady Visual Studio, musíte nahrát tento certifikát toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. tooupload hello certifikát toohello portálu Azure:

   1. Otevřete hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   2. Pokud budete vyzváni, přihlaste se toohello portál a pak přejděte příliš**nastavení**, **certifikáty pro správu**.
   3. V podokně Správa certifikátů hello, zvolte **nahrát**.
   4. Vyberte předplatné Azure, vložte hello úplnou cestu hello soubor .cer, který jste právě vytvořili a zvolte **nahrát**.
