### Substitution par une phrase clé

J'ai choisi le nom au hasard donc bof. Mais c'est l'idée centrale qui compte. Je t'en avais déjà parler ça consiste à remplacer les lettres de l'alphabet par les lettres d'une phrase que l'on aura choisi comme la clé dans l'ordre. Je sais que c'est mal expliqué mais avec un exemple tu comprendra mieux.

Exemple: Nous choisissons comme clé la phrase `Bye bye petit papillon miracullous-ladybug`

- La première étape consiste à supprimer tout les caractères qui ne sont pas des lettres et à tous les mettre dans un même format(majuscule ou minuscule)

  Ainsi la clé devient: `byebyepetitpapillonmiraculousladybug`

- La seconde étape consiste à supprimer les doublons et garder un seul caractère dans l'ordre de la phrase.

  La clé devient: `byeptialonmrcusdg` 

- Enfin on remplace l'alphabet normal par les lettres de notre clé et le reste des lettres de l'alphabet normal par le reste des lettres non présentes dans la clé.

  Donc: `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` (alphabet normal) devient `b, y, e, p, t, i, a, l, o, n, m, r, c, u, s, d, g, f, h, j, k, q, v, w, x, z` (alphabet crypté)

- C'est une clé symétrique donc on peut crypter et décrypter avec la clé.



``````python
# substitution avec phrase clé

import string

# en général les messages cryptés sont en majuscule et ceux décryptés en minuscule
alphabet_min = string.ascii_lowercase
alphabet_maj = string.ascii_uppercase

def crypter(message, cle="On a grandi dans l'illegal", alphabet=alphabet_min):
    # 1e étape
    copie_cle = [e for e in cle.lower() if e in alphabet]
    cle = []
    for e in copie_cle:
        if e not in cle:
            cle.append(e)
    # 2e étape: ajouter les autres letrres
    cle.extend([e for e in alphabet if e not in cle])

    cle = "".join(cle).upper()
    message = message.lower()
    output = ""
    for e in message:
        output += cle[alphabet.find(e)] if e in alphabet else e
    return output


def decrypter(message="FJH OFJUP KJUP TJL RQT AJFFR UH OXLJFR ER HR KRUX KOQ CR GRFJHTPRP", cle="On a grandi dans l'illegal", alphabet=alphabet_min):
    # 1e étape
    copie_cle = [e for e in cle.lower() if e in alphabet]
    cle = []
    for e in copie_cle:
        if e not in cle:
            cle.append(e)
    # 2e étape: ajouter les autres letrres
    cle.extend([e for e in alphabet if e not in cle])

    cle = "".join(cle).upper()
    message = message.upper()
    output = ""
    for e in message:
        output += alphabet[cle.find(e)] if e in cle else e
    return output

print(decrypter())
``````

### Chiffre de Vigenere

Bon celui ci c'est comme le ROT n,  mais seulement que le nombre n n'est pas fixe il varie en fonction de la position de chaque caractère d'une phrase clé.

Par exemple si la clé c'est "salut" et le message c'est "ceci est le message" on va d'abord mettre le message et la clé au mêmenombre de caractère donc la clé devient "salutsalutsalutsalu" (19 caractères comme le message) et ensuite pour chacun des caractères du message qui est une lettre on va faire un ROT n avec n = l'index du caractère correspondant dans la clé par exemple pour le premier caractère qui est "c" le caractère correspondant dans la clé est "s" son index en considérant a=0 est 18 donc pour le "c" on fait un ROT 18 et "c" devient "u". Pour le second caractère qui est "e" on fait un ROT 0 et il reste "e".    

``````python
import string, math, time

# en général les messages cryptés sont en majuscule et ceux décryptés en minuscule
alphabet_min = string.ascii_lowercase
alphabet_maj = string.ascii_uppercase

def clef(cle, message_len):
    # ici on ne va garder que les caractères de la cle qui sont des lettres
    cle = "".join([e for e in cle.lower() if e in alphabet_min])
    # ensuite on vas essayer de mettre la clé à la même taille(même nombre de caractères) que le message
    x, y = message_len, len(cle)
    cle = (cle * math.ceil(x / y))[:x]
    # ici on va simplifier nos futurs calculs en ne gardant que la position dans l'alphabet des caracteres de la clé
    cle_index = [alphabet_min.index(e) for e in cle]
    return cle_index

def crypter(message, cle="On a grandi dans l'illegal"):
    cle_index = clef(cle, len(message))

    message = message.lower()
    output = ""
    for e, i in zip(message, cle_index):
        output += alphabet_maj[(alphabet_min.index(e)+i)%26] if e in alphabet_min else e
    return f"encrypted 🔓: {output}\n"


def decrypter(message="XR Z'AVPM TEGA XL VRTBPEYJE", cle="On a grandi dans l'illegal"):
    cle_index = clef(cle, len(message))

    message = message.upper()
    output = ""
    # etant donné que c'est comme le ROT n
    for e, i in zip(message, cle_index):
        output += alphabet_min[(alphabet_maj.index(e) - i) % 26] if e in alphabet_maj else e
    return f"decrypted 🔓: {output}\n"

def bonus(message):
    n = len(message)
    print("="*n)
    for e in message:
        time.sleep(0.3)
        print(e, end="", flush=True)
    print("="*n)

bonus(decrypter())
``````

