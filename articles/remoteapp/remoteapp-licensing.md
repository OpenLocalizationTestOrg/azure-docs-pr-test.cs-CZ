---
title: "licencování vzdálené aplikace RemoteApp aaaAzure | Microsoft Docs"
description: "Přečtěte si, jak funguje licencování v Azure RemoteAppu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: ff8ebd20-61a1-4f10-87a6-234a170534c9
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: dfa808a65ea6b1a78bf74f3daddb9a84e186eebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Jak v Azure RemoteAppu funguje licencování?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Abyste nastavili službu Azure RemoteApp, vytvořili šablony a jsou připravené toopublish aplikace tooyour uživatelé. Ale je stále jednu poslední věcí toofigure out: licencování. Právě jak funguje licencování pro RemoteApp a pro hello aplikace, které sdílíte prostřednictvím Remoteappu?

RemoteApp nevyžaduje žádné licence Windows ani licence CAL pro Vzdálenou plochu. Vaše předplatné má na starosti hello RemoteApp pokryje sám sebe. (Podívejte se na podrobnosti o hello hello [cenových plánů](https://azure.microsoft.com/pricing/details/remoteapp).)

Pokud použijete jeden hello bitových kopií, které jsou součástí vašeho předplatného, můžete sdílet jakékoli aplikace hello nainstalované v tomto imagi bez nutnosti samostatné licence. Například pokud používáte toobuild image šablony Windows Server 2012 R2 hello kolekce, můžete sdílet System Center Endpoint Protection s uživateli. Hello pouze toothis pravidlo výjimky jsou Office 365 ProPlus, který vyžaduje samostatné předplatné, a Office 2013, který nelze sdílet v produkční kolekci.

Pokud chcete, aby image šablony Office 365 hello toouse, která se dodává s Remoteappem, musíte mít *existující* plán Office 365 ProPlus. Hello totéž platí pro všechny aplikace Office 365 publikujete pomocí vlastní šablony. Je třeba aplikace hello tooactivate pomocí svého vlastního předplatného. To platí pro zkušební i placené předplatné. Pokud chcete image šablony toouse hello Office 365 během zkušební doby hello, *a ještě nemáte předplatné*, přejděte toohello Office 365 stránku příliš[zaregistrovat](https://go.microsoft.com/fwlink/p/?LinkID=403802) zkušební předplatné. Další informace najdete v článku [Jak spolu fungují RemoteApp a Office?](remoteapp-o365.md)

Pokud během zkušební doby hello, nechcete, aby zkušební předplatné tooget Office 365, použijte image šablony Office 2013 Professional Plus hello, která se dodává s Remoteappem. Tento image šablony lze používat pouze 30 dnů a nelze jej převést na placenou kolekci.

Pro jiné aplikace budete potřebovat toomake, že máte hello licence tooshare hello aplikace.

To dává smysl, ne? Můžete publikovat v jakékoli aplikaci, že máte nárok právními předpisy tooshare. A je třeba toomake, že jste skutečně právo tooshare programy.

Upozorňujeme, že v cloudové kolekci nemůžete používat licence CAL ani multilicenční smlouvu. Můžete *můžete* používání multilicenční smlouvy tooactivate aplikací v hybridní kolekci (s výjimkou Office). Stačí tooinstall je v šabloně obrázek z hello multilicenčního média. Podle pokynů hello informace hello aplikace dodavatele tooinstall licence v prostředí vzdálené plochy.

