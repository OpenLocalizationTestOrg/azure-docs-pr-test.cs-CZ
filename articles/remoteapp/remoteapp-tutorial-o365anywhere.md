---
title: "Získání stejných možností Office 365 na jakémkoliv zařízení s Azure RemoteAppem | Dokumentace Microsoftu"
description: "Naučte se sdílet libovolnou aplikaci Office 365 s uživateli pomocí Azure RemoteAppu."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
ms.openlocfilehash: 584c781c97097cda3c1455ade05cba8659f11073
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Získejte stejné funkce Office 365 na jakémkoliv zařízení s Azure RemoteAppem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Tento článek se zabývá nasazením služeb Office 365 na jakémkoliv zařízení ve vaší společnosti. Svým uživatelům můžete nabídnout stejné funkce a uživatelské rozhraní v systémech Android, Apple i Windows.

Dosáhnete toho pomocí Azure RemoteAppu hostováním Office 365 na škálovatelných virtuálních počítačích v Azure, ke kterým se uživatelé mohou připojit. Tato sada virtuálních počítačů se nazývá „kolekce v cloudu“.

## <a name="create-a-cloud-collection"></a>Vytvoření cloudové kolekce
Po vytvoření účtu Azure nejprve kliknutím na odkaz na levém okraji stránky přejděte na stránku **remoteapp**.
![Azure RemoteApp na webu Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Pokračujte kliknutím na **NOVÝ** na dolním okraji stránky a rychle vytvořte kolekci. Zadejte název, oblast, předplatné, plán a image Office Proffesional 2013, který nabízíme.
![Dialogové okno Vytvořit](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Po vyplnění formuláře by se měl spustit proces vytvoření kolekce. To může trvat až hodinu.

![Čekání](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Po dokončení procesu bude stránka vypadat nějak takhle. Když kliknete na **PUBLIKOVÁNÍ**, uvidíte, že většina aplikací Office již pro vás byla publikována.
![Vytvořená kolekce](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Publikované aplikace](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

V tomto okamžiku můžete také kliknutím na **PŘÍSTUP UŽIVATELŮ** přidat další uživatele, kteří budou mít k této kolekci přístup.
![Konfigurace přístupu uživatelů](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Nyní už si můžete vyzkoušet připojení k Office 365!

## <a name="connect-to-office-365"></a>Připojení k Office 365
Otevřete stránku [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), přejděte dolů a kliknutím na **Stáhnout klienty** nainstalujte do svého zařízení klienta Azure RemoteAppu. Snímky obrazovky níže jsou pro systém Windows.

Po spuštění aplikace budete vyzváni k přihlášení pomocí účtu Microsoft (dříve „Live ID“) – pro tentokrát použijte stejný jako účet, který jste použili pro Azure. Pokud jste přihlášení, mělo by se vám zobrazit oznámení o nových pozvánkách. Klikněte na něj a zobrazí se seznam jako na obrázku dole. Přijměte pozvánku, která odpovídá e-mailu vlastníka účtu Azure.

![Nová pozvánka](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Takhle to vypadá, když máte nové pozvánky.

![Potvrzení aplikace](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Když pozvánku přijmete, měli byste vidět všechny aplikace Office v klientovi Azure RemoteAppu.

![Seznam aplikací](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Když kliknete na kteroukoliv z těchto položek, aplikace by se měla spustit ve virtuálním počítači Azure a je to. Užijte si ji!

![spouštění](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

