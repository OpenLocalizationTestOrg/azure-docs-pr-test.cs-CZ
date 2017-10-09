Každý objekt blob v úložišti Azure se musí nacházet v kontejneru. Hello kontejner je součástí hello název objektu blob. Například `mycontainer` je název hello hello kontejneru v těchto ukázkových objektů blob identifikátory URI:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Název kontejneru musí být platný název DNS, vyhovující toohello následující pravidla pojmenování:

1. Názvy kontejnerů musí začínat písmenem nebo číslicí a může obsahovat pouze písmena, číslice a pomlčky (-) znak hello.
2. Bezprostředně před a za každým znakem pomlčky (-) se musí nacházet písmeno nebo číslice. Názvy kontejnerů nesmí obsahovat po sobě jdoucí pomlčky.
3. Název kontejneru musí být psaný malými písmeny.
4. Názvy kontejnerů musí mít délku 3 až 63 znaků.

> [!IMPORTANT]
> Všimněte si, že hello název kontejneru musí být vždycky malá písmena. Pokud název kontejneru zahrnout velké písmeno nebo jinak porušíte pravidla pojmenování kontejneru hello, může se zobrazit chyba 400 (Chybný požadavek). 
> 
> 

