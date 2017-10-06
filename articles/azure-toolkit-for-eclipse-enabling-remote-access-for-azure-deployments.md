---
title: "aaaEnabling vzdáleného přístupu pro Azure nasazení v prostředí Eclipse"
description: "Zjistěte, jak tooenable vzdáleného přístupu pro Azure nasazení pomocí hello Azure Toolkit pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 00c2bf22c1f3ec792098f154f771c87506e87881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse
toohelp řešení nasazení, můžete povolit a používat vzdálený přístup tooconnect toohello virtuální počítač hostování vašeho nasazení. Hello funkce vzdáleného přístupu závisí na hello protokolu RDP (Remote Desktop). Po jejím publikování tooAzure, nebo pokud používáte Eclipse s operačním systémem Windows, můžete nakonfigurovat vzdálený přístup před publikováním tooAzure, můžete nakonfigurovat vzdáleného přístupu pro vaše nasazení. Všimněte si, že budete potřebovat klient vzdálené plochy, který je kompatibilní s operačním systémem v pořadí tooconnect tooyour nasazení virtuálního počítače v Azure.

## <a name="how-tooenable-remote-access-before-you-deploy-tooazure"></a>Nasazení vzdáleného přístupu tooenable před tooAzure
> [!NOTE]
> tooenable vzdálený přístup před nasazením tooAzure vaší aplikace, musíte toobe Eclipse systémem Windows.
> 
> 

Hello následující obrázek ukazuje hello **vzdáleného přístupu** dialogové okno Vlastnosti použít tooenable vzdáleného přístupu.

![][ic719494]

Existují dva způsoby toodisplay hello **vzdáleného přístupu** dialogové okno Vlastnosti:

* Klikněte na tlačítko hello **Upřesnit** odkaz v hello **vzdáleného přístupu** části hello **publikování tooAzure** dialogové okno.

* Otevřete hello **vlastnosti** dialogové okno Azure projektu.

Když vytvoříte nový projekt nasazení Azure, hello projektu nebude mít vzdálený přístup ve výchozím nastavení povolené. Však můžete snadno povolit vzdálený přístup zadáním hello uživatelské jméno a heslo v hello **publikování tooAzure** dialogové okno. heslo Hello vzdáleného přístupu je zašifrována pomocí certifikátů X.509. Pokud nepoužijete zadejte svůj vlastní certifikát šifrování hello spoléhá na certifikát podepsaný svým držitelem, které jsou součástí hello modul plug-in Azure pro prostředí Eclipse. Tento certifikát podepsaný držitelem je v hello **cert** složce projektu Azure uložená i jako soubor certifikátu veřejného (SampleRemoteAccessPublic.cer) a jako Personal Information Exchange (PFX) soubor certifikátu () SampleRemoteAccessPrivate.pfx). Hello pozdější obsahuje hello privátní klíč pro certifikát hello a má výchozí heslo **Heslo1**. Vzhledem k tomu, že je toto heslo veřejné znalostní báze, hello výchozí certifikát by měl použít pouze pro účely, není pro produkční nasazení učení. Takže než pro učení účely, pokud chcete vzdálené relace tooenabled nasazení, měli byste kliknout na hello **Upřesnit** odkaz v hello **publikování tooAzure** toospecify dialogové okno Vlastní certifikát. Všimněte si, že potřebujete tooupload hello PFX verze hello certifikát tooyour hostované služby v rámci hello portálu pro správu Azure, tak, že Azure může dešifrovat heslo uživatele hello.

Hello zbytek hello kurz ukazuje, jak tooenable vzdáleného přístupu pro projekt nasazení Azure, která byla původně vytvořena s vzdálený přístup zakázán. Pro účely tohoto kurzu vytvoříme nový certifikát podepsaný svým držitelem a jeho soubor .pfx, bude mít heslo podle svého výběru. Máte také možnost hello používá certifikát vydaný certifikační autoritou.

## <a name="how-tooenable-remote-access-after-you-have-deployed-tooazure"></a>Jak tooenable vzdáleného přístupu po jste nasadili tooAzure
tooenable vzdálený přístup po nasazení tooAzure hello použijte následující kroky:

1. Přihlaste se pomocí účtu Azure portál pro správu Azure hello

2. V seznamu **cloudové služby**, vyberte nasazené cloudové služby

3. Hello cloudové služby webové stránce, klikněte na tlačítko hello **konfigurace** odkaz

4. Na hello dolní části stránky hello konfigurace, klikněte na tlačítko hello **vzdáleného** odkaz

5. Jakmile se zobrazí dialogové okno hello automaticky otevírané okno:
   
   * Zadejte hello Role můžete pro kterou chcete tooenable vzdáleného přístupu

   * Klikněte na tlačítko tooselect hello **povolení vzdálené plochy** zaškrtávací políčko
   
   * Zadejte uživatelské jméno a heslo, které chcete toouse pro vzdálený přístup
   
   * Vyberte certifikát toouse hello

6. Klikněte na tlačítko **OK**. 

Zobrazí se zpráva, že změna konfigurace je v průběhu, který může trvat několik minut toocomplete. Po změně konfigurace hello byl dokončen, postupujte podle kroků hello v hello **toolog v vzdáleně** později v tomto článku.

## <a name="how-tooenable-remote-access-in-your-package"></a>Jak tooenable vzdáleného přístupu na balíček
1. V podokně Eclipse na prohlížeči projektu klikněte pravým tlačítkem na projekt Azure a klikněte na tlačítko **vlastnosti**.

2. V hello **vlastnosti** dialogové okno, rozbalte položku **Azure** v levém podokně hello a klikněte na **vzdáleného přístupu**.

3. V hello **vzdáleného přístupu** dialogové okno, ujistěte se, **povolit všechny role tooaccept připojení vzdálené plochy s tyto přihlašovací údaje** je zaškrtnuté.

4. Zadejte uživatelské jméno pro připojení ke vzdálené ploše hello.

5. Zadejte a potvrďte heslo hello hello uživatele. Hello uživatelské jméno a heslo hodnoty nastavení v tomto dialogu se použije při vytváření připojení vzdálené plochy. (Všimněte si, že se jedná o samostatný heslo z PFX heslo.)

6. Zadejte datum vypršení platnosti hello hello uživatelského účtu.

7. Klikněte na tlačítko **nový** toocreate nový certifikát podepsaný svým držitelem. (Alternativně můžete třeba vybrat certifikát z vašeho pracovního prostoru nebo souboru systému prostřednictvím hello **prostoru** nebo **FileSystem** tlačítka, v uvedeném pořadí, ale pro účely tohoto kurzu vytvoříme novou certifikát.)

   * V hello **nový certifikát** dialogové okno, zadejte a potvrďte heslo hello budete používat pro soubor PFX.

   * Přijměte hello hodnota zadaná pro **název (CN)**, nebo použít vlastní název.

   * Zadejte název a cesta k souboru hello hello nový certifikát v podobě .cer, kam bude uložena. Pro tento krok a další krok text hello, můžete použít hello **cert** složce projektu Azure, ale máte volné toochoose jiné umístění. Pro účely tohoto kurzu použijeme **c:\mycert\mycert.cer**. (Vytvořte hello **c:\mycert** předchozí tooproceeding složky, nebo použijte existující složku v případě potřeby.)

   * Zadejte název a cesta k souboru hello uložení hello nový certifikát a jeho privátní klíč ve formátu .pfx. Pro účely tohoto kurzu použijeme **c:\mycert\mycert.pfx**. Vaše **nový certifikát** dialogové okno by měl vypadat podobně jako následující toohello (aktualizace cesty ke složce zadat hello, pokud jste nepoužili **c:\mycert**):
     
      ![][ic712275]

   * Klikněte na tlačítko **OK** tooclose hello **nový certifikát** dialogové okno.

8. Vaše **vzdáleného přístupu** dialogové okno by měl vypadat podobně jako toohello následující:</p>
   
   ![][ic719495]

9. Klikněte na tlačítko **OK** tooclose hello **vzdáleného přístupu** dialogové okno.

Sestavení aplikace, s hello sestavení sady pro toocloud nasazení.

## <a name="toolog-in-remotely"></a>toolog v vzdáleně
Jakmile vaši instanci role je připraven, můžete vzdáleně přihlásit toohello virtuální počítač, který je hostitelem vaší aplikace.

* Pokud používáte Eclipse v systémech Windows a je vybraný hello **spuštění vzdálené plochy na nasazení** možnost během tooAzure vaše nasazení, zobrazí se přihlašovací obrazovka připojení ke vzdálené ploše při spuštění vašeho nasazení. Po zobrazení výzvy k hello uživatelského jména a hesla, zadejte hello hodnoty, které jste zadali pro hello vzdáleného uživatele a bude moct toolog v.

* Jiný způsob toolog v vzdáleně je prostřednictvím hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">portálu pro správu Azure</a>:
  
  * V rámci hello **cloudové služby** zobrazení hello portálu pro správu Azure, klikněte na tlačítko cloudové služby, klikněte na tlačítko **instance**, klikněte na konkrétní instanci a pak klikněte na tlačítko hello **připojit**tlačítko. Hello **Connect** tlačítko se zobrazí jako následující hello v řádku nabídek hello:
    
      ![][ic659273]

  * Po kliknutí na hello **Connect** tlačítko bude výzvami tooopen soubor RDP. Otevřete soubor hello a budete postupovat podle pokynů hello. (Můžete také uložit tento soubor tooyour místní počítač a spusťte soubor hello poklepáním tooremote protokolu v tooyour virtuálního počítače bez nutnosti toofirst přejděte hello portálu pro správu.)

  * Po zobrazení výzvy k hello uživatelského jména a hesla, zadejte hello hodnoty, které jste zadali pro hello vzdáleného uživatele a bude moct toolog v.

> [!NOTE]
> Pokud jste na jiný systém než Windows operační systém, budete potřebovat toouse klienta vzdálené plochy, který je kompatibilní s operačním systémem a postupujte podle kroků tooconfigure hello tohoto klienta s hello nastavení v souboru RDP hello, který jste stáhli.
> 
> 

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse] 

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
