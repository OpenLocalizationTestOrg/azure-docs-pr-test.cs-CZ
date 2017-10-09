---
title: "aaaNon technické požadavky pro vytváření nabídku hello Azure Marketplace | Microsoft Docs"
description: "Pochopení hello požadavky pro vytváření a nasazení toohello nabídka Azure Marketplace pro ostatní toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ms.openlocfilehash: 472096863084cc58dc921313419ab60b1a08a3bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="general-prerequisites-for-creating-an-offer-for-hello-azure-marketplace"></a>Obecné požadavky pro vytváření nabídku hello Azure Marketplace
Pochopit, že hello Obecné, proces zaměřené na obchodní požadavky, které jsou potřebné toomove dál s hello nabídnout procesu vytváření.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Ujistěte se, že jste zaregistrováni jako prodejce se společností Microsoft
Podrobné informace o registraci účtu prodejce se společností Microsoft, přejděte příliš[vytváření účtů a registrace](marketplace-publishing-accounts-creation-registration.md).

* **Pokud vaše společnost je již zaregistrován jako prodejce v hello Dev Center a chcete toocreate na novou nabídku** pak přihlášení toohello publikování portál hello stejné id, pomocí které Dev Center se provádí registraci e-mailu. Tento krok je povinný, tak, aby hello portál Dev Center a publikování jsou propojení mezi sebou.
* **Pokud vaše společnost je již zaregistrován jako prodejce v hello Dev Center a chcete tooedit existující nabídku,** pak buď přihlášení toohello publikování portálu pomocí účtu správce hello nebo pomocí účtu, který se přidal jako spolusprávce v hello publikování portálu . Dále je uveden postup tooadd účet spolusprávce.

## <a name="steps-tooadd-a-co-admin-in-hello-publishing-portal"></a>Kroky tooadd spolusprávce v hello publikování portálu
Správci hello publikování portálu můžete přidat hello ostatním členům společnosti hello, kteří pracují na hello aplikace, jako spolusprávce v hello publikování portálu. **Za předpokladu, že jste správcem hello** vypsáni níže jsou hello kroky tooadd ko-správce.

> [!NOTE]
> Pro nové uživatele, než přidáte spolusprávce v hello publikování portál, ujistěte se, že jste vytvořili aspoň jednu aplikaci v hello publikování portálu. To je potřeba jako hello **VYDAVATELŮ** kartě se zobrazují pouze po vytvoření alespoň jedna aplikace v hello publikování portálu.
> 
> 

1. Zkontrolujte, zda toto id e-mailu spolusprávce hello account(MSA) společnosti Microsoft. Pokud ne, zaregistrujte ji jako účet, pomocí této [odkaz](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Zkontrolujte, zda je alespoň jedna aplikace pod účtem správce hello než to zkusíte tooadd ko-správce.
3. Po hello výše kroky se provádějí, přihlášení toohello publikování portálu s hello spolusprávce e-mailu id a potom se přihlaste.
4. Teď přihlášení toohello publikování portálu s id e-mailu správce hello.
5. Přejděte tooPublishers -> vyberte -> váš účet správce -> Přidat hello spolusprávce (snímek vypsáni níže)
   
    ![Kreslení](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Zkontrolujte, zda zadaný v hello různé fáze hello publikování procesu (například Dev Center, publikování portálu) jsou monitorovány pro všechny komunikaci od společnosti Microsoft, ID e-mail.
7. Pro registraci Dev Center nepoužívejte účet přidružený jedna osoba. To je navržený pro odebrání závislostí ze jedna osoba.
8. Pokud jste čelí všechny problémy s registrací Dev Center, pak vyvolejte lístek pomocí této [odkaz](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-toodelete-a-co-admin-in-hello-publishing-portal"></a>Kroky toodelete spolusprávce v hello publikování portálu
**Za předpokladu, že jste správcem hello** vypsáni níže jsou hello kroky toodelete ko-správce.

1. Přihlášení toohello publikování portálu s id e-mailu správce hello.
2. Přejděte příliš**vydavatelů** -> vyberte -> váš účet **správci** -> **Spolusprávci**.
3. Klikněte na hello **X** tlačítko Další toohello spolusprávce chcete odstranit tot (snímek vypsáni níže).
   
    ![Kreslení](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Pokud jsou plánování toopublish pouze volné nabídky (nebo přineste vlastní licence) nemají toocomplete daň a bank informace o společnosti.
> 
> registraci Hello společnosti musí být dokončený tooget spuštěna. Ale když vaše společnost pracuje na hello daň a bank informace v účtu Microsoft Developer hello, vývojáři hello začít pracovat na vytvoření bitové kopie virtuálního počítače hello v hello [publikování portál](https://publish.windowsazure.com), načítání certifikaci, a testování v hello pracovní prostředí Azure. Budete potřebovat hello dokončení prodejce účet schválení pouze u poslední krok hello publikování toohello vaši nabídku Azure Marketplace.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Získat předplatné Azure "průběžnými platbami"
Toto je hello předplatné, že budete používat toocreate vaší Image virtuálních počítačů a ruční přes hello bitové kopie toohello [Azure Marketplace](https://azure.microsoft.com/marketplace/). Pokud nemáte předplatné existující, pak zaregistrujte v https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Zákazník – od" zemích
> [!WARNING]
> V pořadí toosell vašim službám na hello Azure Marketplace, je nezbytné provést se, že je vaše registrovaná entita z jednoho z hello schválení "zákazník – od" zemích. Toto omezení je výběr a zdanění z důvodů. Aktivně Těšíme se tooexpand tento seznam zemí v blízké budoucnosti hello, tak si Nenechte ujít. Úplný seznam hello, najdete v části 1b Dobrý den [zásady zapojení Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Další kroky
Jakmile jsou splněny požadavky netechnické hello, jsou vedle hello nabídka konkrétních technických požadavků. Klikněte na tlačítko hello odkaz toohello článku hello typ odpovídající nabídky, které chcete toocreate pro hello Azure Marketplace.

* [Požadavky technické virtuálních počítačů](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Řešení šablony technické požadavky](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Viz také
* [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md)

