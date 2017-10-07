---
title: "aaaExcel add-in pro Machine Learning webové služby | Microsoft Docs"
description: "Jak toouse Azure Machine Learning webové služby přímo v aplikaci Excel bez psaní jakéhokoli kódu."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: c52f40d33c9907f284e4750afe47181dc3365fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Doplněk Excelu pro webové služby Azure Machine Learning
Aplikace Excel je snadno toocall webové služby přímo bez hello potřebovat toowrite žádný kód.

## <a name="steps-toouse-an-existing-web-service-in-hello-workbook"></a>Kroky tooUse webové služby existující v hello sešitu

1. Otevřete hello [ukázkový soubor aplikace Excel](http://aka.ms/amlexcel-sample-2), který obsahuje doplněk hello aplikace Excel a data o cestujících na hello Titanic.
2. Kliknutím vyberte hello webové služby-"Titanic pozůstalým předpověď (ukázka doplňku Excel) [skóre]" v tomto příkladu.
   
    ![Vyberte webovou službu][01]
3. Tím přejdete toohello **Predict** části.  Tento sešit už obsahuje ukázková data, ale pro prázdný sešit můžete vybrat buňku v aplikaci Excel a klikněte na tlačítko **pomocí ukázkových dat**.
4. Vyberte data hello se záhlavími a klikněte na ikonu rozsah hello vstupní data.  Ujistěte se, že je zaškrtnuté políčko "data obsahují záhlaví" hello.
5. V části **výstup**, zadejte počet buněk hello místo, kam chcete hello toobe výstup, například "H1" sem.
6. Klikněte na tlačítko **předpovědi**.
   
    ![Předpověď části][02]

Nasazení webové služby nebo použijte existující webovou službu. Další informace o nasazení webové služby najdete v tématu [návod krok 5: nasazení služby Azure Machine Learning webové hello](machine-learning-walkthrough-5-publish-web-service.md).

Získáte hello klíč rozhraní API pro webovou službu. Tam, kde můžete provést tuto akci, závisí na tom, jestli jste publikovali webovou službu Classic Machine Learning webové služby Machine Learning nové.

**Použít klasický webovou službu** 

1. V nástroji Machine Learning Studio, klikněte na tlačítko hello **webové služby** v levém podokně hello a pak vyberte hello webové služby.
   
    ![Studio vyberte webové služby][04]
2. Zkopírujte hello klíč rozhraní API pro hello webovou službu.
   
    ![Klíč Studio rozhraní API][05]
3. Na hello **řídicí panel** pro hello webovou službu, klikněte na hello **požadavků a odpovědí** odkaz.
4. Vyhledejte hello **URI požadavku** části.  Zkopírujte a uložte hello adresy URL.

> [!NOTE]
> Nyní je možné toosign do hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu tooobtain klíč hello rozhraní API pro webovou službu Classic Machine Learning.
> 
> 

**Použít novou webovou službu**

1. V hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu klikněte na **webové služby**, potom vyberte svoji webovou službu. 
2. Klikněte na tlačítko **využívat**.
3. Vyhledejte hello **informace o základní spotřeby** části. Zkopírujte a uložte hello **primární klíč** a hello **požadavků a odpovědí** adresy URL.

## <a name="steps-tooadd-a-new-web-service"></a>Kroky tooAdd novou webovou službu

1. Nasazení webové služby nebo použijte existující webovou službu. Další informace o nasazení webové služby najdete v tématu [návod krok 5: nasazení služby Azure Machine Learning webové hello](machine-learning-walkthrough-5-publish-web-service.md).
2. Klikněte na tlačítko **využívat**.
3. Vyhledejte hello **informace o základní spotřeby** části. Zkopírujte a uložte hello **primární klíč** a hello **požadavků a odpovědí** adresy URL.
4. V aplikaci Excel přejděte toohello **webové služby** část (Pokud jste v hello **Predict** klikněte na šipku zpět hello toogo toohello seznam webových služeb).
   
    ![Přejděte výběr tooWeb služby][03]
5. Klikněte na tlačítko **přidat webovou službu**.
6. Vložte adresu URL hello do hello Excel doplňku textové pole s popiskem **URL**.
7. Vložení hello rozhraní API nebo primární klíč do hello textového pole s názvem bez přípony **klíč rozhraní API**.
8. Klikněte na tlačítko **Přidat**.
   
    ![Adresa URL a klíč API pro classic webovou službu.][06]
9. toouse hello webové služby, postupujte podle předchozích pokynů hello, "Kroky tooUse existující webové služby."

## <a name="sharing-your-workbook"></a>Sdílení sešitu
Pokud jste uložili vašeho sešitu, hello rozhraní API nebo primární klíč pro hello webové služby, které jste přidali také uloženy. To znamená, že sešit hello by sdílet jenom s osob, kterým důvěřujete.

Požádejte všechny dotazy v hello následující části komentář nebo na našem [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
