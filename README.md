# Les Formes abstraites

## Durée : 30'

## Objectifs
Réactivation des fondamentaux Java : classes et objets.

## Les classes et méthodes abstraites

En Java, une classe abstraite est une classe qui ne peut pas être instanciée directement (Il n'est pas possible de faire un objet à partir de cette classe). Elle sert de modèle pour d'autres classes et peut contenir des méthodes abstraites et des méthodes concrètes. Une méthode abstraite est une méthode déclarée sans implémentation ; elle n'a pas de corps et il sera ensuite obligatoire de l'implémenter par les sous-classes qui héritent de la classe abstraite.

Les classes abstraites sont utilisées lorsqu'on veut définir un comportement commun pour plusieurs classes sans fournir une implémentation complète. Elles permettent de s'assurer que certaines méthodes seront présentes dans toutes les sous-classes, mais elles laissent la liberté à ces sous-classes de définir comment ces méthodes doivent se comporter.

**Prennons un exemple:**

Nous voulons faire une application qui gère des animaux. Nous allons donc faire une classe qui s'appelle `Animal` qui contiendra un nom, une couleur et doit s'exprimer.

Lors de l'implémentation, il sera difficile, voir même impossible, de dire combien de pattes un animal a par défaut. Ce qui est sûr par contre c'est que chaque sous-classe **doit** fournir cette information.

L'implémentation sera donc que la classe animal va gérer le nom et la couleur comme attribut comme cela se passe dans le cas de l'héritage classique et il aura une méthode abstraite `crie()` qui sera ensuite définie dans les sous-classes.

``` Mermaid
classDiagram
    Animal <|-- Chien
    Animal <|-- Oiseau
    Application "1" o--> "0..n" Animal : animaux
    class Animal{
        - nom : String
        - couleur : String
        + crie() String*
    }
    <<abstract>> Animal

    class Chien{
        + crie() String
    }

    class Oiseau{
        + crie() String
    }

    class Application{
        + main(String[] args)
    }
```
> Les méthodes abstaites sont représentées en *italique* dans un digramme UML. 

Et l'implémentation de cela en java serai la suivante:
``` Java
public abstract class Animal
{
    private String nom;
    private String couleur;

    public Animal(String nom, String couleur)
    {
        this.nom = nom;
        this.couleur = couleur;
    }

    public abstract String crie();
}

public class Chien extends Animal
{
    public Chien(String nom, String couleur)
    {
        super(nom, couleur);
    }

    @Override
    public String crie()
    {
        return "Ouaf, ouaf";
    }
}

public class Oiseau extends Animal
{
    public Oiseau(String nom, String couleur)
    {
        super(nom, couleur);
    }

    @Override
    public Override String crie()
    {
        return "Piou, piou";
    }
}

public class Application{

    public static void main (String[] args)
    {
        ArrayList<Animal> animaux = new ArrayList<Animal>();
        animaux.add(new Chien("Médor", "Blanc"));
        animaux.add(new Chien("Felix", "Noir"));
        animaux.add(new Oiseau("Hirondelle", "Noir"));

        for(Animal animal : animaux)
        {
            if(animal instanceOf Chien)
            {
                System.out.println("Ceci est un chien");
            }
        }
    }
}
```

***Rappel:***

**extends**: Défini de quelle classe elle va hériter.
```Java
public class Chien extends Animal
```
La classe Chien hérite de la classe Animal.

**super()**: Appelle le constructeur de la classe parent.
```Java
    public Chien(String nom, String couleur)
    {
        super(nom, couleur);
    }
```
Appelle le constructeur de la classe `Animal` avec les paramètres reçus dans le constructeur `Chien`

**this**: Fait référence à l'attribut de la classe.
```Java
    private String nom;
    private String couleur;

    public Animal(String nom, String couleur)
    {
        this.nom = nom;
        this.couleur = couleur;
    }
```
this.nom fait référence à l'attribut `private String nom;`
et `nom` fait référence au paramètre du constructeur.

**instanceOf**: Permet de savoir si l'objet est une instance d'une classe spécifiée.
```Java
    for(Animal animal : animaux)
    {
        if(animal instanceOf Chien)
        {
            System.out.println("Ceci est un chien");
        }
    }
```
En parcourant les objets présent dans la liste `animaux`, nous vérifions si l'objet courant est une instance de la classe `Chien`. Retourne `true` si c'est bien une instance de la classe `Chien`, sinon retourne `false`.

## Travail à réaliser

Copiez le projet `Les Formes` fait précédement.

En vous basant sur les différents diagrammes en dessous, faites les modifications suivantes:

- La classe Forme devient une classe abstraite. 
- Elle prévoit deux méthodes abstraites calculeSurface() et nombreDeCotes() devant être implémentées par ses descendants.
 - Elle introduit une nouvelle méthode getInfos() servant à produire un texte donnant toutes les infos utiles sur un objet de type forme, qui sera utilisé par le main() pour afficher les formes.

``` Mermaid
classDiagram
    Forme <|-- Triangle
    Forme <|-- Disque
    Forme <|-- Carre
    Forme <|-- Rectangle
    Application "1" o--> "0..n" Forme : lesFormes
    class Application{
        + MAX_FORME : int = 8
        + Application()
        + genererFormes() void
        + claculerSurfaces() void
        + main(String[] args)$ void
    }
    class Forme{
        - nom : String
        + Forme(String nom) 
        + calculSurface() double*
        + nombreDeCotes() String*
        + getInfos() String
        + getNom() String
    }
    <<Abstract>> Forme
    class Triangle{
        - base : int
        - hauteur : int
        + Triangle(String nom, int base, int hauteur)
        + calculSurface() double
        + nombreDeCotes() String
    }

    class Disque{
        - rayon : int
        + Disque(String nom, int rayon)
        + calculSurface() double
        + nombreDeCotes() String
    }

    class Carre{
        - cote : int
        + Carre(String nom, int cote)
        + calculSurface() double
        + nombreDeCotes() String
    }

    class Rectangle{
        - largeur : int
        - longueur : int
        + Rectangle(String nom, int largeur, int longueur)
        + calculSurface() double
        + nombreDeCotes() String
    }
```
Créez ensuite une classe `Application` possédant une méthode `main` respectant les diagrammes de séquences suivants:

### Méthode main(String[] args)
```Mermaid
sequenceDiagram
    create participant Application
    main->>Application: <<Creation>>
    main->>+Application: genererFormes()
    Application-->>-main: 
    main->>+Application: calculerSurfaces()
    Application-->>-main: 
```

### Méthode calculerSurfaces()

```Mermaid
sequenceDiagram
    create participant laSurfaceTotale
    Application->>+laSurfaceTotale: 0.0
    create participant laSurface
    Application->>+laSurface: 0.0
    alt lesFormes != null
        loop i < lesFormes.length
            Application ->>+ Application: uneForme = lesFormes[i]
            alt uneForme != null
                Application ->>+ uneForme: calculeSurface()
                uneForme -->>- Application: 
                Application ->>+ uneForme: getInfos()
                uneForme -->>- Application: 
                Application ->>+ Application: SOUT(le retour de getInfos)
            end
        end
    end

    Application ->>+ Application: SOUT(laSurfaceTotale)
```

### Méthode genererFormes()

```Mermaid
sequenceDiagram
    create participant carre1
    Application->>carre1: cote = 1
    create participant rectangle1
    Application->>rectangle1: largeur=2, longueur=3
    create participant triangle1
    Application->> triangle1 : base=4, hauteur=5
    create participant disque1
    Application->> disque1 : rayon=6
    create participant carre2
    Application->> carre2 : cote=7
    create participant rectangle2
    Application ->> rectangle2 : largeur=8, longueur=9
    create participant triangle2
    Application->> triangle2 : base=10, hauteur=11
    create participant disque2
    Application ->> disque2 : rayon=12

```

## Résultat attendu
> La surface de ce [Carré] qui a [4] côtés est de [1.0]</br>
> La surface de ce [Rectangle] qui a [4] côtés est de [6.0]</br>
> La surface de ce [Triangle] qui a [3] côtés est de [10.0]</br>
> La surface de ce [Disque] qui a [infinité] côtés est de [113.09733552923255]</br>
> La surface de ce [Carré] qui a [4] côtés est de [49.0]</br>
> La surface de ce [Rectangle] qui a [4] côtés est de [72.0]</br>
> La surface de ce [Triangle] qui a [3] côtés est de [55.0]</br>
> La surface de ce [Disque] qui a [infinité] côtés est de [452.3893421169302]</br>
> La surface totale des formes est de 758.4866776461628