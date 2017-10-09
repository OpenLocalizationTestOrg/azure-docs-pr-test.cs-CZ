---
Title: aaa "Azure Analysis Services kurz lekce 13: nasazení | Microsoft Docs"Popis: Popisuje, jak toodeploy hello kurzu projektu tooAzure Analysis Services.
služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend
---
# <a name="lesson-13-deploy"></a>Lekce 13: Nasazení

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci nakonfigurujete vlastnosti nasazení; zadání toodeploy tooand serveru Azure Analysis Services název modelu hello. Potom nasadíte instanci toothat hello modelu. Po nasazení modelu uživatelé můžou připojovat tooit pomocí sestav klientské aplikace. Další, najdete v části toolearn [nasazení služby Analysis Services tooAzure](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Odhadovaný čas toocomplete této lekci: **5 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 12: analyzovat v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> Musíte mít [oprávnění správce](../analysis-services-server-admins.md) na hello vzdálené služby Analysis Services serveru v daném pořadí toodeploy tooit.  

> [!IMPORTANT]  
> Pokud jste nainstalovali hello AdventureWorksDW2014 ukázkové databáze na serveru SQL na místě a nasazujete serveru Azure Analysis Services tooan modelu, [místní brána dat](../analysis-services-gateway.md) je vyžadován.
  
## <a name="deploy-hello-model"></a>Nasazení modelu hello  
  
#### <a name="tooconfigure-deployment-properties"></a>Vlastnosti nasazení tooconfigure  

  
1.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **AW Internet prodej** projektu a pak klikněte na **vlastnosti**.  
  
2.  V hello **stránky vlastností prodej Internet AW** dialogovém **Server nasazení**, v hello **Server** vlastnost, zadejte celý server hello.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  V hello **databáze** vlastnosti, typ **společnosti Adventure Works Internet prodej**.  
  
4.  V hello **název modelu** vlastnosti, typ **společnosti Adventure Works Internet prodej modelu**.  
  
5.  Zkontrolujte výběr a klikněte na **OK**.  
  
#### <a name="toodeploy-hello-adventure-works-internet-sales"></a>toodeploy hello společnosti Adventure Works Internet prodeje
  
1.  V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **AW Internet prodej** Projekt > **sestavení**.  

2.  Klikněte pravým tlačítkem na hello **AW Internet prodej** Projekt > **nasadit**.

    Pokud nasazujete tooAzure Analysis Services, může být výzvami tooenter váš účet. Zadejte účet organizace a heslo, například nancy@adventureworks.com. Tento účet musí být ve Správci na serveru hello.
  
    Zobrazí se dialogové okno nasadit Hello a zobrazí stav nasazení hello hello metadat a každá tabulka součástí modelu hello.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Po úspěšném dokončení nasazení pokračujte kliknutím na **Zavřít**.  
  
## <a name="conclusion"></a>Závěr  
Blahopřejeme! Vytvořili a nasadili jste první tabelární model služby Analysis Services. Článek pomůže tento kurz vás provede dokončení hello nejčastějších úloh, při vytváření tabulkový model. Teď, když je váš model společnosti Adventure Works Internet prodej nasazenou, můžete použít SQL Server Management Studio toomanage hello modelu; Vytvořte proces skripty a plán zálohování. Uživatelé mohou nyní také připojit toohello model pomocí sestav klientská aplikace, jako je například aplikace Microsoft Excel nebo Power BI.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Co dále?
[Propojení s Power BI Desktop](../analysis-services-connect-pbi.md)   
[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Doplňková lekce – Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Doplňková lekce – Nepravidelné hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
