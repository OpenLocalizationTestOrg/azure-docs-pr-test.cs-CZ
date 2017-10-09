---
title: "aaaAzure seznam účtů úložiště"
description: "Spravovat nastavení účtu úložiště pomocí hello Azure Toolkit pro Eclipse"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35e25881ca95ae4050a26283e4726d9549b37f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-account-list"></a>Seznam účtů úložiště Azure
Úložiště Azure, která účty povolit stáhnout toobe umístění použít JDK, aplikační server a libovolné součásti a také pro ukládání stavu, při použití ukládání do mezipaměti. Eclipse udržuje seznam známých úložiště účty, které jsou k dispozici tooyour projekty v pracovním prostoru Eclipse. tooopen hello **účty úložiště** dialog, který je použité toomanage, který zobrazí seznam v prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure** a potom klikněte na **účty úložiště**.

Následující Hello ukazuje hello **účty úložiště** dialogové okno.

![][ic719496]

Tento dialog můžete také otevřít z **účty** odkaz v dialogových oknech, které používají účty úložiště, jako je například hello následující:

* Hello **JDK** kartě hello **konfigurace serveru** dialogové okno.
* Hello **Server** kartě hello **konfigurace serveru** dialogové okno.
* Hello **přidat součást** dialogové okno.
* Hello **ukládání do mezipaměti** dialogovém okně Vlastnosti.

## <a name="tooimport-your-storage-accounts-using-a-publish-settings-file"></a>tooimport účtů úložiště pomocí soubor nastavení publikování
1. V rámci hello **účty úložiště** dialogové okno, klikněte na tlačítko **Import ze souboru nastavení publikování**.

2. (Tento krok přeskočte, pokud již máte uložené publikování nastavení souboru tooyour místního počítače.) V hello **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **stáhnout soubor nastavení publikování**. Pokud ještě nejste přihlášeni k účtu Azure, bude výzvami toolog v. Potom budete vyzváni k souboru s nastavením publikování toosave Azure. (Můžete ignorovat hello výsledné pokynů na stránkách přihlášení hello - jejich jsou poskytovány hello portál Azure a jsou určeny pro Visual Studio uživatele.) Uložte tooyour místního počítače.

3. Stále v hello **importovat informace o předplatném** dialogové okno, klikněte na tlačítko hello **Procházet** tlačítko, vyberte hello publikovat soubor nastavení, který jste si místně předtím uložili a potom klikněte na **otevřete**.

4. Klikněte na tlačítko **OK** tooclose hello **importovat informace o předplatném** dialogové okno.

## <a name="toocreate-a-new-storage-account"></a>toocreate nový účet úložiště
1. V rámci hello **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.

2. V rámci hello **přidat účet úložiště** dialogové okno, klikněte na tlačítko **nový**.

3. V rámci hello **nový účet úložiště** dialogové okno, zadejte hodnoty pro hello následující:

   * Název účtu úložiště.

   * Umístění účtu úložiště hello.

   * Popis účtu úložiště hello.

   * účet úložiště Hello předplatné toowhich hello patří.

4. Klikněte na tlačítko **OK** tooclose hello **nový účet úložiště** dialogové okno.

Ho může trvat několik minut, než vaše toobe účet úložiště vytvořit. Po vytvoření, klikněte na tlačítko **OK** tooclose hello **přidat účet úložiště** dialogové okno a váš nový účet úložiště se přidá toohello seznam účtů úložiště k dispozici.

## <a name="tooadd-an-existing-storage-account-toohello-list"></a>tooadd existující toohello seznam účtů úložiště
1. Pokud již nemáte úložiště Azure, účet, vytvořte ho pomocí následujících kroků hello uvedené hello **toocreate novou část účtu úložiště** výše. (Alternativně můžete vytvořit nový účet úložiště v hello [portálu pro správu Azure][Azure Management Portal].)

2. V rámci hello **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.

3. V rámci hello **přidat účet úložiště** dialogové okno, zadejte hodnoty pro **název** a **přístupový klíč**. název a přístupový klíč účtu Hello musí být pro existující účet úložiště Azure. Použití hello **úložiště** části hello [portálu pro správu Azure] [ Azure Management Portal] tooview účtu úložiště názvy a klíče. Vaše **přidat účet úložiště** dialogové okno bude vypadat podobně jako následující toohello.
   
   ![][ic719497]

4. Klikněte na tlačítko **OK** tooclose hello **přidat účet úložiště** dialogové okno.

## <a name="toomodify-a-storage-account-toouse-a-new-access-key"></a>toomodify toouse účtu úložiště nového přístupového klíče
1. V rámci hello **účty úložiště** dialogové okno, klikněte na účet úložiště hello má tooedit a pak klikněte na **upravit**.

2. V rámci hello **upravit přístupový klíč účtu úložiště** dialogu Upravit hello **přístupový klíč** hodnotu.

3. Klikněte na tlačítko **OK** tooclose hello **upravit přístupový klíč účtu úložiště** dialogové okno.

## <a name="tooremove-a-storage-account-from-hello-list-maintained-in-eclipse"></a>tooremove účet úložiště ze seznamu hello udržovat v prostředí Eclipse
1. V rámci hello **účty úložiště** dialogové okno, klikněte na účet úložiště hello má tooedit a pak klikněte na **odebrat**.

2. Klikněte na tlačítko **OK** při výzvami tooremove hello účet úložiště.

> [!NOTE]
> Odebrání účtu úložiště hello prostřednictvím hello **účty úložiště** dialogové okno pouze odebere z hello seznam účtů úložiště lze zobrazit v Eclipse. Neodebere účet úložiště hello ze svého předplatného Azure. Kromě toho hello účet úložiště může znovu po zobrazí v seznamu Eclipse znovu načte hello podrobnosti svého předplatného.
> 
> 

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse] 

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
