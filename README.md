# Loic-tenda
devoir de game programming
Exercice 2

#include <iostream>

/**
 
 * Copie tous les caractères de source vers destination
 * Ajoute automatiquement le '\0' à la fin
 * Suppose que destination a assez d'espace
 * Gère le chevauchement des zones mémoire
 */
void CopierChaine(char* destination, const char* source) {
    // Vérification basique : si pointeurs invalides, on ne fait rien
    if (destination == nullptr || source == nullptr)
        return;

    // Si les zones mémoire se chevauchent, on copie à l’envers
    if (destination > source && destination < source + std::strlen(source)) {
        // Pointeurs à la fin des chaînes
        const char* src = source + std::strlen(source)
        
        // Copie à rebours (y compris le '\0')
        while (src >= source) {
            *dest-- = *src--;
        }
    } 
    else {
        // Copie normale (avant le '\0')
        while (*source != '\0') {
            *destination++ = *source++;
        }
        // Ajout du caractère nul final
        *destination = '\0';
    }
}

int main() {
    char source[] = "Bonjour";
    char destination[50];

    CopierChaine(destination, source);

    std::cout << "Source : " << source << std::endl;
    std::cout << "Destination : " << destination << std::endl;

    // Exemple de chevauchement (copie interne dans la même chaîne)
    char texte[] = "ABCDE";
    std::cout << "Après chevauchement : " << texte << std::endl;

    return 0;
}

Exrcice 3

#include <iostream>
#include <cstddef>   // pour size_t

/**
 * Concatène deux chaînes : ajoute source à la fin de destination
 * @param destination: Chaîne à laquelle on ajoute (sera modifiée)
 * @param source: Chaîne à ajouter (constante)
 * 
 * Suppose que destination a assez d'espace
 */
void ConcatenerChaines(char* destination, const char* source) {
    if (destination == nullptr || source == nullptr)
        return; // sécurité basique

    //Trouver la fin de la chaîne destination
    while (*destination != '\0') {
        ++destination;
    }

    //Copier les caractères de source à la suite
    while (*source != '\0') {
        *destination++ = *source++;
    }

    //Ajouter le caractère nul final
    *destination = '\0';
}

int main() {
    char destination[100] = "Bonjour";
    const char* source = " le monde !";

    std::cout << "Résultat : " << destination << std::endl;

    return 0;
}

Exrcice 4

#include <iostream>

/**
 * Recherche la première occurrence d'un caractère dans une chaîne
 * 
 * Utilise l'arithmétique des pointeurs pour le parcours
 * Retourne nullptr si le caractère n'est pas trouvé
 */
char* TrouverCaractere(const char* chaine, char caractere) {
    if (chaine == nullptr)
        return nullptr; // sécurité

    // Parcours de la chaîne caractère par caractère
    while (*chaine != '\0') {
        if (*chaine == caractere) {
            // On retourne un pointeur vers une position de la chaîne
            // mais la fonction retourne un char* non-const,
            return const_cast<char*>(chaine);
        }
        ++chaine; // on avance au caractère suivant
    }

    // Si on sort de la boucle, le caractère n'a pas été trouvé
    return nullptr;
}

int main() {
    const char* texte = "Bonjour le monde !";
    char caractere = 'o';


    if (resultat != nullptr)
        std::cout << "Caractère '" << caractere 
        std::cout << "Caractère '" << caractere << "' non trouvé." << std::endl;

    return 0;
}

Exrcice 5

#include <iostream>
#include <cstddef> // pour size_t

/**
 * Compte le nombre d'occurrences d'un caractère dans une chaîne
 * Compte toutes les occurrences, pas seulement la première
 */
size_t CompterOccurrences(const char* chaine, char caractere) {
    if (chaine == nullptr)
        return 0; // sécurité : chaîne invalide

    size_t compteur = 0; // initialisation du compteur

    // parcours de la chaîne caractère par caractère
    while (*chaine != '\0') {
        if (*chaine == caractere) {
            ++compteur; // incrémentation si correspondance
        }
        ++chaine;
    }

    return compteur;
}

int main() {
    const char* texte = "Bonjour le monde !";
    char caractere = 'o';

    size_t nb = CompterOccurrences(texte, caractere);

    std::cout << "Le caractère '" << caractere 
              << "' apparaît " << nb << " fois dans \"" 

    return 0;
}

Exercice 6

#include <iostream>
#include <cstddef> // pour size_t

/**
 * Copie un bloc mémoire d'une zone à une autre
 * 
 * Utilise des pointeurs de type unsigned char* pour copier octet par octet
 * Gère la copie même si les zones se chevauchent
 * Plus sûr que CopierChaine car utilise une taille explicite
 */
void CopierMemoire(void* destination, const void* source, size_t taille) {
    if (destination == nullptr || source == nullptr || taille == 0)
        return; // sécurité basique

    // Conversion des pointeurs en unsigned char*
    unsigned char* dest = static_cast<unsigned char*>(destination);
    const unsigned char* src = static_cast<const unsigned char*>(source);

    // Si chevauchement et destination après source → copier à rebours
    if (dest > src && dest < src + taille) {
        dest += taille;
        while (taille--) {
            *(--dest) = *(--src);
        }
    } 
    // Sinon, copier vers l’avant (cas normal)
    else {
        for (size_t i = 0; i < taille; ++i) {
            dest[i] = src[i];
        }
    }
}

int main() {
    char texte[] = "ABCDE12345";

    // Copie sans chevauchement
    char destination[20];
    std::cout << "Copie normale : " << destination << std::endl;

    // Copie avec chevauchement (comme memmove)
    std::cout << "Après chevauchement : " << texte << std::endl;

    return 0;
}

Exercice 7

#include <iostream>
#include <cstddef> // pour size_t

/**
 * Remplit une zone mémoire avec une valeur spécifique
 * 
 * Parcourt la zone octet par octet et écrit la valeur
 * Utile pour initialiser des tableaux ou structures
 * Peut être utilisé pour mettre des zéros ou d'autres valeurs
 */
void InitialiserMemoire(void* zone, unsigned char valeur, size_t taille) {
    if (zone == nullptr || taille == 0)
        return; // sécurité basique

    // Conversion en pointeur vers unsigned char
    unsigned char* ptr = static_cast<unsigned char*>(zone);

    // Parcours de la zone octet par octet
    for (size_t i = 0; i < taille; ++i) {
        ptr[i] = valeur;
    }
}

int main() {
    char tableau[10];


    std::cout << "Après initialisation à 0 : ";
    for (unsigned char c : tableau)
        std::cout << static_cast<int>(c) << ' ';
    std::cout << std::endl;

    std::cout << "Après initialisation à 'A' : ";
    for (char c : tableau)
        std::cout << c << ' ';
    std::cout << std::endl;

    return 0;
}

Exercice 8

#include <iostream>
#include <cstddef>  // pour size_t

/**
 * Extrait une portion d'une chaîne
 * 
 * Vérifie que debut et longueur sont valides
 * Copie les caractères de [debut] à [debut+longueur-1]
 * Ajoute toujours le '\0' final
 * Tronque si on dépasse la fin de la source
 */
void ExtraireSousChaine(char* destination, const char* source, 
                        size_t debut, size_t longueur) {
    if (destination == nullptr || source == nullptr)
        return; // sécurité

    // Calcul de la longueur réelle de la source
    size_t tailleSource = 0;
    while (source[tailleSource] != '\0') {
        ++tailleSource;
    }

    // Vérifie que 'debut' est dans les limites de la source
    if (debut >= tailleSource) {
        destination[0] = '\0'; // chaîne vide
        return;
    }

    // Copie des caractères
    size_t i = 0;
    while (i < longueur && (debut + i) < tailleSource) {
        destination[i] = source[debut + i];

}

int main() {
    const char* texte = "Bonjour le monde !";
    char sousChaine[50];

    ExtraireSousChaine(sousChaine, texte, 8, 6);
    std::cout << "Sous-chaîne extraite : \"" << sousChaine << "\"" << std:endl;

    ExtraireSousChaine(sousChaine, texte, 50, 5); // cas hors limites
    std::cout << "Sous-chaîne extraite (hors limites) : \"" << sousChaine << "\"" << std::endl;

    return 0;
}
