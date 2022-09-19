Clara SALIVET - 3ICS - 12 Septembre

# TP2 : Bash

## Exercice 1 : Variables d'environnement

1. /usr/local/sbin:/usr/local/bin
:/usr/sbin:/usr/bin:/sbin
:/bin:/usr/games:/usr/local/games
:/snap/bin
2. cd $HOME
3. LANG = langue utilisé par le logiciel
   PWD = chemin absolue vers le répertoire courant
   OLDPWD = chemin absolue vers le répertoire courant précédant
   SHELL = décrit le Shell qui interpréterale commande saisit
4. MY_VAR="abcd"
   echo $MY_VAR
5. la commande bash ouvre une nouvelle session. la variable n'existe donc plus.
6. export MY_VAR="abcd" 
   printenv MY_VAR
7. L'affectation est correcte
8. echo 'Bonjour à vous deux,' $NOMS '!'
9. La commande "unset" supprime la variable d'environnement du système alors que donner une valeur vide à une variable ne la supprime pas mais laisse un contenu vide.
10. echo '$HOME' = "$HOME"

## Exercice 2 : Contôle de mot de passe

```bash
#! /bin/bash

PASSWORD="test"

read -s -p 'mdp :' mdp

if [ $PASSWORD = $mdp ]; then echo "ok :)"
else echo "erreur :("
fi
````

## Exercice 3 : Expressions rationnelles 

```bash
#! /bin/bash


function is_number()
{
   re='^[+-]?[0-9]+([.][0-9]+)?$'

   if !  [[ $1 =~ $re ]]; then echo "pas nombre" return 1
   else echo "nombre" return 0
fi
}
is_number

````

## Exercice 4 : Contrôle d'utilisateur

```bash

#! /bin/bash

read -p "nom de l'utilisateur :" user

for i in $cat /etc/passwd | cut -d ":" -f 1) ;
   if [ "$i" == "$user" ]; then
      echo "l'utilisateur est $user sur le script $0"
   fi
done
```

## Exercice 5 : factorielle 

```bash
#!/bin/bash

function fact()
{
  FACT=$FACT*$1
  $1="$1"-1
  if ! [ $1 = 1 ]; then
    fact $1
  fi
}

export FACT=1
fact $1
echo "$FACT"
````

## Exercice 6 : le juste prix 

```bash
#!/bin/bash

prix=$(((RANDOM%1000)+1))

devine=-1

while [ $devine -ne $prix ]
do
read devine
if [ $devine -gt $prix ]; then
  echo 'C'est moins !'
fi
if [ $devine -lt $prix ]; then
  echo 'C'est plus !'
fi
done

echo 'Gagné !'
````

## Exercice 7 : Statistiques

```bash
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

sum=0
count=0
min=$1
max=$1

while (("$#")); do
    is_number $1
    if [[ $? = 1 ]]; then
        echo Un des paramètres n\'est pas un nombre
        exit
    fi

    sum=$((sum + $1))
    count=$((count + 1))

    if [[ $1 -gt $max ]]; then
        max=$1
    elif [[ $1 -lt $min ]]; then
        min=$1
    fi
    shift
done

echo Min: $min
echo Max: $max
printf 'Moyenne: %.2f\n' $(echo "$sum / $count" | bc -l)
````

```bash
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

values=()
input="0"
index=0

while [ -n "$input" ]; do
    read -p "Entrez un nombre: " input

    if [ -n "$input" ]; then
        is_number $input
        if [[ $? = 1 ]]; then
            echo Ce n\'est pas un nombre !
        else
            values[$index]=$input
            index=$(( $index + 1 ))
        fi
    fi
done

sum=0
min=${values[0]}
max=${values[0]}

for n in ${values[@]}; do
    sum=$((sum + $n))
    count=$((count + 1))

    if [[ $n -gt $max ]]; then
        max=$n
    elif [[ $n -lt $min ]]; then
        min=$n
    fi
done

echo Min: $min
echo Max: $max
printf 'Moyenne: %.2f\n' $(echo "$sum / $index" | bc -l)
```
