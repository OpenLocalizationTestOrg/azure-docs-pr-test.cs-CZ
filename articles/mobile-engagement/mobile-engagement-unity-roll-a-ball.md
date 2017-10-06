---
title: "aaaUnity vrátit míč kurzu"
description: "Kroky toocreate hello classic vrátit Unity míč hra, který je nezbytný předpoklad pro všechny kurzy Mobile Engagement Unity"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0afd0eca-f74a-43aa-bba8-436a0265c312
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 10d923682432961207594886b08e5db60cf60d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a id="unity-roll-a-ball"></a>Vytvoření Unity vrátit míč hry
Tento kurz vás provede hello hlavní kroky pro mírně upravenou [Unity vrátit míč kurzu](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Tato ukázka hra se skládá z kulovým 'player, objekt, který řídí uživatele aplikace hello a cílem hello ve hře hello je too'collect' collectible objekty podle kolize objektu player hello se tyto collectible objekty. Toto předpokládá základní znalost prostředí editor Unity. Pokud narazíte na potíže by měl odkazovat toohello úplné kurzu. 

### <a name="setting-up-hello-game"></a>Nastavení hra hello
Hello kroky jsou z hello [kurzu Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Otevřete **Unity Editor** a klikněte na tlačítko **nový**. 
   
    ![][51] 
2. Zadejte **název projektu** & **umístění**, vyberte **3D** a klikněte na tlačítko **vytvořit projekt**.
   
    ![][52]
3. Uložit scény výchozí hello právě vytvořené jako součást nového projektu hello jako s názvem hello **MiniGame** v rámci nového  **\_scény** ve složce **prostředky** složky:
   
    ![][53]
4. Vytvoření **3D objektu -> roviny** jako hello přehrávání pole a přejmenujte tento objekt roviny jako **základů**
   
    ![][1]
5. Resetování hello transformace součásti pro tento **základů** objektu tak, aby se v hello původu. 
   
    ![][3]
6. Zrušte zaškrtnutí políčka **zobrazit mřížky** z **si nabídky** pro hello **základů** objektu.
   
    ![][4]
7. Aktualizace hello **škálování** součásti pro hello **základů** objektu toobe [X = 2, Y = 1, Z = 2]. 
   
    ![][5]
8. Přidejte nový **3D objektu -> oblasti** toohello projektu a přejmenování této oblasti objekt jako **Player**. 
   
    ![][6]
9. Vyberte hello **Player** objekt a klikněte na tlačítko **resetovat transformace** podobné toohello roviny objektu. 
10. Aktualizace **transformace -> Pozice -> souřadnici Y** součásti pro hello Player Y jako 0,5.  
    
    ![][7]
11. Vytvořit novou složku s názvem **materiály** v projektu hello kde vytvoříme hello podstatným toocolor hello přehrávač. 
12. Vytvořte novou **materiálu** názvem **pozadí** v této složce. 
    
    ![][8]
13. Aktualizovat barvu hello hello materiálu aktualizací hello **Albedo** vlastnost ho. Můžete vybrat hodnoty RGB hello [0,32,64]. 
    
    ![][9]
14. Přetáhněte tomto materiálu do hello scény zobrazení tooapply barva toohello **základů** objektu. 
    
    ![][10]
15. Nakonec aktualizujte hello **transformace -> otočení -> Y** too60 hello směrovou Light objektu pro přehlednost. 
    
    ![][12]

### <a name="moving-hello-player"></a>Přesunutí player hello
Hello kroky jsou z hello [kurzu Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Přidat **RigidBody** součást toohello **Player** objektu. 
   
    ![][13]
2. Vytvořit novou složku s názvem **skripty** v hello projektu. 
3. Klikněte na tlačítko **přidat součást -> nový skript -> C# skript**. Pojmenujte ji **PlayerController**a klikněte na tlačítko **vytvořit a přidat**. Tato akce vytvoří a objekt skriptu toohello Player připojit.  
   
    ![][14]
4. Přesunout tento skript v části hello **skripty** složky hello projektu. 
5. Otevřete hello skript pro úpravy v editoru vaše oblíbené skriptu, aktualizujte kód skriptu hello s hello následující kód a uložte jej. 
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
6. Všimněte si skriptu hello výše používá **rychlost** vlastnost. V editoru Unity hello aktualizujte too10 vlastnost rychlost hello.  
   
    ![][15]
7. Stiskněte tlačítko **přehrání** v hello Unity Editor. Nyní byste měli mít toocontrol hello míč pomocí klávesnice hello a měl by otočit a pohyb. 

### <a name="moving-hello-camera"></a>Přesunutí hello fotoaparát
Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) a bude tie hello **hlavní fotoaparát** toohello **Player** objektu. 

1. Aktualizace hello **Transform.Position** toobe X = 0, Y = 10.5, Z =-10.  
2. Aktualizace hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.  
   
    ![][16]
3. Přidat nový skript volá **CameraController** toohello **MainCamera** a přesunout ji pod složka skripty hello. 
   
    ![][17]
4. Otevře hello skript pro úpravy a přidejte následující kód v ní hello:
   
        using UnityEngine;
        using System.Collections;
   
        public class CameraController : MonoBehaviour {
   
            public GameObject player;
   
            private Vector3 offset;
   
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
   
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
5. V prostředí Unity - přetáhněte hello Player proměnné do hello Player slotu pro objekt hlavní fotoaparát hello tak, aby hello dva jsou přidruženy jednu na druhou. 
   
    ![][18]
6. Teď Pokud začít přehrávat v editoru hello Unity a objektu Player míč otáčení hello pak uvidíte hello následující v hello pohybů fotoaparát.  

### <a name="setting-up-hello-play-area"></a>Nastavení oblasti Play hello
Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Vytvoříme hello bariéry, které obaluje hello základů tak, aby hello objektu Player míč není odkládací oblast play hello v jeho pohyb. 

1. Klikněte na tlačítko **vytvořit -> vytvořit prázdnou -> objekt herní** a pojmenujte ji **bariéry**
   
    ![][19]
2. V rámci tohoto objektu bariéry - vytvořit nový **3D objektu -> datové krychle** a pojmenujte ji "Wall – západ". 
   
    ![][20]
3. Aktualizace hello **transformace -> Pozice** a **transformace -> škálování** pro tento objekt Wall – západ. 
   
    ![][21]
4. Duplicitní toocreate wall – západ hello **wall – východ** s hello aktualizovat transformace pozice a škálování. 
   
    ![][22]
5. Duplicitní toocreate wall – východ hello **wall – sever** s hello aktualizovat transformace pozice & škálování. 
   
    ![][23]
6. Duplicitní wall – sever hello a vytvořte **wall – jih** s hello aktualizovat transformace pozice & škálování. 
   
    ![][24]

### <a name="creating-collectible-objects"></a>Vytváření Collectible objektů
Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Vytvoříme některé atraktivní vyhledávání objektů, které tvoří hello sadu collectible objektů, které objekt Player míč hello musí too'collect se podle kolize s nimi. 

1. Vytvořte novou **3D objektu datové krychle** a pojmenujte ji vyzvednutí. 
2. Upravit hello **transformace -> otočení** & **transformace -> škálování** hello výstupního objektu. 
   
    ![][25]
3. Vytvořte a připojte **nový C# skript** názvem **Rotator** toohello výstupního objektu. Ujistěte se, že skript hello tooput hello skripty ve složce. 
   
    ![][26]
4. Otevřete tento skript pro úpravy a aktualizovat ji toobe hello následující: 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. Teď se na ose otáčení přístupů hello Play režimu v hello Unity Editor a zobrazit vaše výstupního objektu.
6. Vytvořit novou složku s názvem **Prefabs** 
   
    ![][27]
7. Přetáhněte hello **vyzvednutí** objektu a umístí jej do složky Prefabs hello.
   
    ![][28]
8. Vytvořte novou **herní prázdný objekt** názvem **Pickups**. Obnovit jeho tooorigin pozice a poté přetáhněte hello výstupního objektu v rámci této herní objektu.  
   
    ![][29]
9. Duplicitní hello **vyzvednutí** objektu a šířit na hello **základů** objekt kolem hello **Player** objekt aktualizací hello **na Transform.Position X & Z**  hodnoty správně. 
   
    ![][30]
10. Vytvoření **nové materiály** názvem **vyzvednutí** a aktualizovat ji toobe Red barevně aktualizací hello **Albedo vlastnost** podobné toowhat jsme pro aktualizaci hello základů objektu. 
    
    ![][31]
11. Použijte hello podstatným tooall hello 4 výstupní objekty.
    
    ![][32]

### <a name="collecting-hello-pickup-objects"></a>Shromažďování hello výstupní objekty
Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Aktualizujeme hello Player, aby bylo možné too'collect' hello výstupní objekty podle kolize s nimi. 

1. Otevřete hello **PlayerController** skript připojené toohello objektu Player pro úpravy a aktualizovat ji toohello následující:  
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour {
   
            public float speed;
   
            private Rigidbody rb;
   
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
   
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
   
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
   
                rb.AddForce (movement * speed);
            }
   
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }
2. Vytvořte novou **značka** názvem **vyberte si** (musí se shodovat co je ve skriptu hello)  
   
    ![][33]
   
    ![][34]
3. Použít **značka** toohello Prefab vyzvednutí objektu. 
   
    ![][35]
4. Povolit **IsTrigger** zaškrtávací políčko pro objekt Prefab hello.
   
    ![][36]
5. Přidá objekt Prefab tooPickup pevné textu. Pro optimalizaci výkonu bude aktualizován, použili jsme dynamické collider tooa statické collider hello. 
   
    ![][37]
6. Nakonec zkontrolujte hello **IsKinematic** vlastnost hello prefab objektu. 
   
    ![][38]
7. Stiskněte tlačítko **přehrání** v editoru hello Unity a bude moct tooplay to **vrátit míč** herní přesunutím hello Player objekt, který používá vaše kláves pro směr vstup. 

### <a name="updating-hello-game-for-mobile-play"></a>Aktualizace hello herní pro mobilní play
Hello části výše svém hello základní kurz z Unity. Nyní jsme upraví hello herní toomake ho popisný mobilních zařízení. Upozorňujeme, že jsme vstup z klávesnice pro hello herní dosavadní pro testování. Nyní jsme ji upravovat tak, aby jsme můžete řídit phone hello player pomocí hello pohybu Dobrý den, tj pomocí zrychlení jako vstup hello. 

Otevřete hello **PlayerController** skript pro úpravy a aktualizace hello **FixedUpdate** metoda toouse hello pohybu z hello zrychlení toomove hello Player objektu. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

V tomto kurzu se ukončí základní herní vytvoření s Unity a můžete nasadit na zařízení, které vaše volba tooplay hello hra. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png    
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png    
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png













