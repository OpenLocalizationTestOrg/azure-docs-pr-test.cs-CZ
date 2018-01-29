---
title: "Jak používat Azure umístění na základě služby mapový ovládací prvek | Microsoft Docs"
description: "Další informace o použití knihovny Javascript klienta řízení mapy služeb na základě umístění Azure."
services: location-based-services
keywords: "Nemáte přidat nebo upravit klíčová slova bez konzultace ohledně specialistou na SEO."
author: philmea
ms.author: philmea
ms.date: 11/22/2017
ms.topic: article
ms.service: location-based-services
manager: timlt
ms.openlocfilehash: 494a8308a5ed4ae37ed9561d051155e7433e6193
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-use-the-azure-location-based-services-map-control"></a>Jak používat Azure umístění na základě služby mapový ovládací prvek
Knihovna Javascript na straně klienta mapový ovládací prvek umožňuje vykreslení mapy a vložené funkce služeb na základě umístění Azure do webové nebo mobilních aplikací. 

## <a name="prerequisites"></a>Požadavky
Účet služeb na základě umístění Azure a klíč. Informace o vytvoření účtu a klíč načítání, najdete v části [Správa vašeho účtu služeb na základě umístění Azure a klíče](how-to-manage-account-keys.md). 

## <a name="create-a-new-map-in-a-web-page-using-the-map-control-api"></a>Vytvořit nové mapování na webové stránce pomocí rozhraní API mapy ovládacího prvku
Mapu můžete vložit na webové stránce pomocí knihovny Javascript klienta mapový ovládací prvek.

1. Vytvořte nový soubor s názvem MapSearch.html.

2. Přidejte odkazy na základě Azure polohy šablony stylů a skript zdroje do `<head>` element souboru:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. K vykreslení nové mapování v prohlížeči, přidejte **#map** odkaz v `<style>` elementu.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Chcete-li inicializovat mapový ovládací prvek, zadejte novou část v těle html a vytvořit skript. Použijte vlastní klíč účtu služeb na základě umístění Azure ve skriptu. 

    ```html
    <div id="map">
        <script>
            var LBSAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": LBSAccountKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Otevřete soubor ve webovém prohlížeči a zobrazit vykreslené mapy.