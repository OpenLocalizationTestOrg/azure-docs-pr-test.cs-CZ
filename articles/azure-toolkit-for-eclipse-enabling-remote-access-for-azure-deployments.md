---
title: "Povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse"
description: "Postup povolení vzdáleného přístupu pro Azure nasazení pomocí sady nástrojů pro Azure pro Eclipse."
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
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse
Pomoc při řešení potíží s vaše nasazení, můžete povolit a používat vzdáleného přístupu pro připojení k virtuálnímu počítači hostování vašeho nasazení. Funkce vzdáleného přístupu závisí na protokol RDP (Remote Desktop). Vzdálený přístup můžete nakonfigurovat pro vaše nasazení po publikovali do Azure, nebo pokud používáte Eclipse s operačním systémem Windows, můžete nakonfigurovat vzdálený přístup před publikováním do Azure. Všimněte si, že budete potřebovat klient vzdálené plochy, který je kompatibilní s operačním systémem bylo před připojením k vašeho nasazení virtuálního počítače v Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Postup povolení vzdáleného přístupu, před nasazením do Azure
> [!NOTE]
> Postup povolení vzdáleného přístupu, před nasazením aplikace do Azure, budete muset používat prostředí Eclipse v systému Windows.
> 
> 

Na následujícím obrázku **vzdáleného přístupu** dialogové okno Vlastnosti používaným pro povolení vzdáleného přístupu.

![][ic719494]

Existují dva způsoby, jak zobrazit **vzdáleného přístupu** dialogové okno Vlastnosti:

* Klikněte na tlačítko **Upřesnit** na odkaz v **vzdáleného přístupu** části **publikovat do Azure** dialogové okno.

* Otevřete **vlastnosti** dialogové okno Azure projektu.

Když vytvoříte nový projekt nasazení Azure, projekt nebude mít vzdálený přístup ve výchozím nastavení povolené. Však můžete snadno povolit vzdáleného přístupu tak, že zadáte uživatelské jméno a heslo **publikovat do Azure** dialogové okno. Heslo vzdáleného přístupu je zašifrována pomocí certifikátů X.509. Pokud nepoužijete zadejte svůj vlastní certifikát šifrování spoléhá na certifikát podepsaný svým držitelem, které jsou součástí modulu plug-in Azure pro Eclipse. Tento certifikát podepsaný držitelem je v **cert** složce projektu Azure uložená i jako soubor certifikátu veřejného (SampleRemoteAccessPublic.cer) a jako Personal Information Exchange (PFX) soubor certifikátu () SampleRemoteAccessPrivate.pfx). Ta obsahuje soukromý klíč pro certifikát, a má výchozí heslo **Heslo1**. Vzhledem k tomu, že je toto heslo veřejné znalostní báze, výchozí certifikát by měl použít pouze pro účely, není pro produkční nasazení učení. Takže než pro učení účely, pokud chcete povolit vzdálené relace pro vaše nasazení, měli byste kliknout na **Upřesnit** na odkaz v **publikovat do Azure** dialogovém okně můžete zadat vlastní certifikát. Všimněte si, že budete muset nahrát verze PFX certifikátu k vaší hostované služby v rámci portálu pro správu Azure tak, že Azure může dešifrovat heslo uživatele.

Zbývající část tohoto kurzu se dozvíte, jak pro povolení vzdáleného přístupu pro projekt nasazení Azure, která byla původně vytvořena s vzdálený přístup zakázán. Pro účely tohoto kurzu vytvoříme nový certifikát podepsaný svým držitelem a jeho soubor .pfx, bude mít heslo podle svého výběru. Máte také možnost použít certifikát vydaný certifikační autority.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Postup povolení vzdáleného přístupu, když nasadíte do Azure
Postup povolení vzdáleného přístupu, když nasadíte do Azure, použijte následující postup:

1. Přihlaste se k portálu správy Azure pomocí účtu Azure

2. V seznamu **cloudové služby**, vyberte nasazené cloudové služby

3. Cloudové služby webové stránce, klikněte na tlačítko **konfigurace** odkaz

4. V dolní části stránky konfigurace, klikněte na **vzdáleného** odkaz

5. Jakmile se zobrazí dialogové okno automaticky otevírané okno:
   
   * Zadejte Role, pro který chcete povolit vzdálený přístup

   * Kliknutím vyberte **povolení vzdálené plochy** zaškrtávací políčko
   
   * Zadejte uživatelské jméno a heslo, které chcete použít pro vzdálený přístup
   
   * Vyberte certifikát, který chcete použít

6. Klikněte na tlačítko **OK**. 

Zobrazí se zpráva, že změna konfigurace je v průběhu, který může trvat několik minut. Po dokončení změny konfigurace, postupujte podle kroků v **přihlásit vzdáleně** později v tomto článku.

## <a name="how-to-enable-remote-access-in-your-package"></a>Postup povolení vzdáleného přístupu na balíček
1. V podokně Eclipse na prohlížeči projektu klikněte pravým tlačítkem na projekt Azure a klikněte na tlačítko **vlastnosti**.

2. V **vlastnosti** dialogové okno, rozbalte položku **Azure** v levém podokně a klikněte na **vzdáleného přístupu**.

3. V **vzdáleného přístupu** dialogové okno, ujistěte se, **povolit všechny role tak, aby přijímal připojení vzdálené plochy s tyto přihlašovací údaje** je zaškrtnuté.

4. Zadejte uživatelské jméno pro připojení ke vzdálené ploše.

5. Zadejte a potvrďte heslo pro uživatele. Uživatelské jméno a heslo hodnotami nastavenými v tomto dialogu se použije při vytváření připojení vzdálené plochy. (Všimněte si, že se jedná o samostatný heslo z PFX heslo.)

6. Zadejte datum vypršení platnosti pro uživatelský účet.

7. Klikněte na tlačítko **nový** k vytvoření nového certifikátu podepsaného svým držitelem. (Alternativně můžete třeba vybrat certifikát z vašeho pracovního prostoru nebo souboru systému prostřednictvím **prostoru** nebo **FileSystem** tlačítka, v uvedeném pořadí, ale pro účely tohoto kurzu vytvoříme novou certifikát.)

   * V **nový certifikát** dialogové okno, zadejte a potvrďte heslo budete používat pro soubor PFX.

   * Hodnota zadaná pro přijmout **název (CN)**, nebo použít vlastní název.

   * Zadejte název a cesta k souboru, kde bude uložen nový certifikát v podobě .cer. Pro tento krok a dalšímu kroku, můžete použít **cert** složce projektu Azure, ale máte volné vyberte jiné umístění. Pro účely tohoto kurzu použijeme **c:\mycert\mycert.cer**. (Vytvořte **c:\mycert** složku před pokračováním, nebo použijte existující složku v případě potřeby.)

   * Zadejte název a cesta k souboru, kde bude uložen nový certifikát a jeho privátní klíč ve formátu .pfx. Pro účely tohoto kurzu použijeme **c:\mycert\mycert.pfx**. Vaše **nový certifikát** by měl vypadat podobně jako následující dialogové okno (aktualizace cestách ke složkám, pokud jste nepoužili **c:\mycert**):
     
      ![][ic712275]

   * Klikněte na tlačítko **OK** zavřete **nový certifikát** dialogové okno.

8. Vaše **vzdáleného přístupu** dialogové okno by měl vypadat podobně jako následující:</p>
   
   ![][ic719495]

9. Klikněte na tlačítko **OK** zavřete **vzdáleného přístupu** dialogové okno.

Sestavení aplikace, se sestavením, nastavte pro nasazení do cloudu.

## <a name="to-log-in-remotely"></a>Ke vzdálenému
Jakmile vaši instanci role je připraven, můžete vzdáleně přihlášení do virtuálního počítače, který je hostitelem vaší aplikace.

* Pokud používáte Eclipse ve Windows a vybrali jste **spuštění vzdálené plochy na nasazení** možnost během nasazení do Azure, zobrazí se přihlašovací obrazovka připojení ke vzdálené ploše při spuštění vašeho nasazení. Po zobrazení výzvy pro uživatelské jméno a heslo, zadejte hodnoty, které jste zadali pro vzdálené uživatele a bude moct přihlásit.

* Je také možné přihlásit vzdáleně přes <a href="http://go.microsoft.com/fwlink/?LinkID=512959">portálu pro správu Azure</a>:
  
  * V rámci **cloudové služby** zobrazení portálu pro správu Azure, klikněte na tlačítko cloudové služby, klikněte na **instance**, klikněte na konkrétní instanci a klikněte **připojit** tlačítko. **Connect** tlačítko se zobrazí jako následující na panelu příkazů:
    
      ![][ic659273]

  * Po kliknutí na tlačítko **Connect** tlačítko, zobrazí se výzva k otevření souboru RDP. Otevřete soubor a postupujte podle pokynů. (Můžete může také uložte tento soubor do místního počítače a potom spusťte soubor poklepáním na vzdálené přihlášení k virtuálnímu počítači bez nutnosti nejprve přejděte na portálu pro správu.)

  * Po zobrazení výzvy pro uživatelské jméno a heslo, zadejte hodnoty, které jste zadali pro vzdálené uživatele a bude moct přihlásit.

> [!NOTE]
> Pokud jste na jiný systém než Windows operační systém, budete muset použít klienta vzdálené plochy, který je kompatibilní s operačním systémem a postup konfigurace tohoto klienta pomocí nastavení v souboru RDP, který jste stáhli.
> 
> 

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse] 

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
