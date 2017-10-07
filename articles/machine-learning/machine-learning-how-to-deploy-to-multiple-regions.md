---
title: "aaaHow toodeploy webové služby toomultiple oblasti | Microsoft Docs"
description: "Kroky toodeploy (kopie) oblasti tooother novou webovou službu."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 21fcdb96f118c60ed98b60b1b2df833766c7c8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-web-service-toomultiple-regions"></a>Jak toodeploy webové služby toomultiple oblastí
Hello nové webové služby Azure umožňují tooeasily nasazení webové služby toomultiple oblastí bez nutnosti více předplatných nebo pracovních prostorů. 

Ceny je oblast konkrétní, že proto je nutné definovat plán fakturace pro každou oblast, ve které budete nasazovat hello webové služby.

## <a name="toocreate-a-plan-in-another-region"></a>toocreate plán v jiné oblasti
1. Přihlaste se k [Microsoft Azure Machine Learning webové služby](https://services.azureml.net/).
2. Klikněte na tlačítko hello **plány** možnost nabídky.
3. V plánech hello přes stránka zobrazení, klikněte na tlačítko **nový**.
4. Z hello **předplatné** rozevíracího seznamu, vyberte hello předplatné, ve které hello se bude nacházet nový plán.
5. Z hello **oblast** rozevíracího seznamu, vyberte oblast pro hello nový plán. Hello možnosti plánování pro vybrané oblasti hello se zobrazí v hello **možnosti plánování** oddílu hello stránky.
6. Z hello **skupiny prostředků** rozevíracího seznamu, vyberte prostředek skupiny pro plán hello. Další informace o skupinách prostředků najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
7. V **název plánu** název typu hello hello plánu.
8. V části **plán možnosti**, klikněte na tlačítko hello fakturace úroveň hello nový plán.
9. Klikněte na možnost **Vytvořit**.

## <a name="deploying-hello-web-service-tooanother-region"></a>Nasazení hello webové služby tooanother oblast
1. Klikněte na tlačítko hello **webové služby** možnost nabídky.
2. Vyberte hello webové služby, které nasazujete tooa novou oblast.
3. Klikněte na tlačítko **kopie**.
4. V **název webové služby**, zadejte nový název pro hello webovou službu.
5. V **webové služby popis**, zadejte popis pro hello webovou službu.
6. Z hello **předplatné** rozevíracího seznamu, vyberte hello předplatné, ve které hello bude umístěn novou webovou službu.
7. Z hello **skupiny prostředků** rozevíracího seznamu, vyberte prostředek skupiny pro hello webovou službu. Další informace o skupinách prostředků najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
8. Z hello **oblast** rozevíracího seznamu, vyberte hello oblast, ve které toodeploy hello webové služby.
9. Z hello **účet úložiště** rozevíracího seznamu, vyberte účet úložiště, ve které toostore hello webové služby.
10. Z hello **cena plán** rozevíracího seznamu, vyberte plán v oblasti hello jste vybrali v kroku 8.
11. Klikněte na tlačítko **kopie**.

