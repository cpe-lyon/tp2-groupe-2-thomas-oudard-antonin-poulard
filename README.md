# Tp 2 - BASH

### Exercice 1 - Variables d’environnement

1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?

    ```txt
    echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
    ```

2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans
votre répertoire personnel ?

    ```txt
    echo $HOME
    /root
    ```

3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.

    ```txt
    echo $LANG
    Retourne le type de formatage du text

    echo $PWD
    Retourne le repertoire courant

    echo $OLDPWD
    Retourne l'ancien repertoire courant

    echo $SHELL
    Indique l'interpréteur shell par défaut

    echo $_
    Retourne la derniére variable utilisé
    ```

4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.

    ```txt
    MY_VAR=toto
    echo $MY_VAR
    toto
    ```

5. Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin
de cette question, tapez la commande exit pour revenir dans votre session initiale.

    ```txt
    La commande 'bash' ouvre un autre bash. La variable MY_VAR n'existe pas car elle à était
    créé localement pour le bash courant
    ```

6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.

    ```txt
    export MY_VAR=toto
    echo $MY_VAR
    toto

    On vien de créer une variable d'environnement, elle est donc accessible a tout le monde tout le temps
    ```

7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace.
Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.

    ```txt
    NOMS="OUDARD POULARD"
    echo $NOMS
    OUDARD POULARD
    ```

8. Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2
sont vos deux noms) en utilisant la variable NOMS.

    ```txt
    echo "Bonjour à vous deux, $NOMS"
    ```

9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande
unset ?

    ```txt
    Ne rien affecter à une variable, créais la varibale avec rien dedans,
    contrairement a unset qui supprime la varibale (unset --help)
    ```

10. Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre
dossier personnel d’après bash)

    ```txt
    echo '$HOME =' $HOME
    $HOME = /root
    ```

## Programmation Bash

```txt
export PATH = "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/mnt/c/Users/toudard/script/"
```

### Exercice 2 - Contrôle de mot de passe

Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au
contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par
l’utilisateur ne doit pas s’afficher.

```sh
#!/bin/bash
# testpwd.sh
# version 1.0
# date 19/02/2020
# state OK

USER_PASSWORD="IRC"

read -s -p "Your password : "  password_to_test

if [[ "$password_to_test" = "$USER_PASSWORD" ]]
then
    echo -e "\nWelcome home !"
else
    echo -e "\nWrong password !"
fi
```

### Exercice 3 - Expressions rationnelles

Ecrivez un script qui prend un paramètre et utilise la
fonction ```is_number()``` pour vérifie
r que ce paramètre est un nombre réel.
Il affichera un message d’erreur dans le cas contraire.

```sh
#!/bin/bash
# regex.sh
# version 1.0
# date 19/02/2020
# state OK

function is_number()
{
    re='^[+-]?[0-9]+([.][0-9]+)?$'
    if ! [[ $1 =~ $re ]]
    then
        return 1
    else
        return 0
    fi
}

is_number $1

if [[ $? = "1" ]]
then
    echo "Argument passed is not a number"
else
    echo "Argument passed is a number"
fi
```

### Exercice 4 - Contrôle d’utilisateur

Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné
en paramètre du script. Si le script est appelé sans nom d’utilisateur, il
affiche le message : ”Utilisation : nom_du_script nom_utilisateur”,
où nom_du_script est le nom de votre script récupéré automatiquement (si vous
changez le nom de votre script, le message doit changer automatiquement)

```sh
#!/bin/bash
# user.sh
# version 1.0
# date 19/02/2020
# state OK

user=`cut -d: -f1 /etc/passwd | grep ^$1$`

if [[ $1 = '' ]]
then
    echo "Usage : $0 nom_utilisateur"
elif ! [[ $user = "" ]]
then
    echo "This user exist"
else
    echo "This user doesn't exist"
fi
```

### Exercice 5 - Factorielle

Écrivez un programme qui calcule la factorielle d’un entier naturel passé en
paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).

```sh
#!/bin/bash
# factorielle.sh
# version 1.0
# date 19/02/2020
# state OK

factorial() {
    if [[ $1 -le 1 ]]
    then
        echo 1
    else
        last=$(factorial $(( $1 - 1)))
        echo $(( $1 * last ))
    fi
}

factorial $1
```

### Exercice 6 - Le juste prix

Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et
demande à l’utilisateur de le deviner.
Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !”
selon les cas (vous utiliserez $RANDOM).

```sh
#!/bin/bash
# justeprix.sh
# version 1.0
# date 19/02/2020
# state OK

rand=$((($RANDOM % 100) + 1))

echo "Bienvenue au juste prix !"
read -p "Entrez votre nombre : " nb

while [[ $nb != $rand ]]
do
    if [[ $nb -lt $rand ]]
    then
        echo "Le nombre à trouver est superieur à celui rentré"
        read -p "Entrez votre nombre : " nb
    elif [[ $nb -gt $rand ]]
    then
        echo "Le nombre à trouver est inferieur à celui rentré"
        read -p "Entrez votre nombre : " nb
    fi
done

echo "Bravo vous avez trouvé le bon nombre"
```

### Exercice 7 - Statistiques

1. Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max
et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres
sont bien des entiers.
2. Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT)

    ```sh
    #!/bin/bash
    # factorielle.sh
    # version 1.0
    # date 19/02/2020
    # state OK

    function usage() {
        echo "Usage: $0 [ARG]"
        echo "[ARG] : Many integer parameters between -100 and 100"
        exit 1
    }

    function is_number()
    {
        re='^[+-]?[0-9]+([.][0-9]+)?$'
        if ! [[ $1 =~ $re ]]
        then
            return 1
        else
            return 0
        fi
    }

    function statistique() {
        min=$1
        max=$1
        total=0
        for int in $*
        do
            if (( $min > $int ))
            then
                min=$int
            elif (( $max < $int ))
            then
                max=$int
            fi

            total=$(( $total + $int ))

            (( i++ ))
        done

        moy=$(( $total / $i ))
        echo -e "min : $min\nmoy : $moy\nmax : $max"
    }

    function test_params() {

        for param in $*
        do
            is_number $param
            if [[ $? == 0 ]]
            then
                if [ $param -lt -100 ] || [ $param -gt 100 ]
                then
                    usage
                fi
            else
                usage
            fi
        done

        statistique $*
    }

    test_params $*
    ```

3. Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et
stockées au fur et à mesure dans un tableau.
