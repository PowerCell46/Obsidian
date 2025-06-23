## Multiline String

```c
#include <stdio.h>  
// STanDard Input-Output Header  
  
int main() {  
    printf("Hello there \  
General Kenobi. \  
You are a bold one. \  
Kill him!");  
  
    return 0;  
}
```

# Working with Scanf

```c
#include <stdio.h>

// FOR arrays! -> don't use & because the array name is already a pointer
int main() {

    char firstName[20];
    
    printf("Enter your first name: ");
    
    // Use scanf to read a string (character array) from user input
    // No need for & because firstName is already a pointer to the first element of the array
    scanf("%s", firstName);
    
    printf("Welcome, %s!\nIt's good to see you!\n", firstName);
	    
    printf("What is your brother's name: ");
    
    // INCORRECT: Using & here is not needed for strings
    // &firstName refers to the address of the entire array, not the first element
    // This will likely cause an error or warning depending on the compiler
    // You should simply pass firstName, like in the previous scanf call
    scanf("%s", &firstName);  // <-- INCORRECT

    // Print a welcome message using the brother's name
    printf("Welcome, %s!", firstName);
}

#include <stdio.h>

// For non-array types you MUST use & to pass the memory address to scanf
int main() {

    int age;

    printf("Enter your age: ");

    // &age is required to pass the memory address of the age variable to scanf
    // This allows scanf to store the input value directly into the age variable
    scanf("%d", &age);

    // No need for & here because we are using the value of 'age', not its address
    printf("Welcome, person %d years of age!\nIt's good to see you!\n", age);
}
```

# Gets and Puts

```c
#include <stdio.h>  
  
int main() {  
  
    char name[20];  
  
    // The problem with gets() is that it doesn't check for buffer overflow.  
    // If the user enters more characters than the array can hold, it will overwrite adjacent memory,    
    // potentially leading to crashes or security vulnerabilities    
    gets(name); // read from the console  
  
    // Instead, you can use fgets() which allows you 
    // to specify the maximum number of characters to read,    
    // making it much safer.    
    char secondName[20];  
    fgets(secondName, sizeof(secondName), stdin); // safer alternative to gets()  
  
    
    // It prints the given string followed by a newline (\n).  
    puts(name); // write to the console  
    // puts("Welcome, " + name); // Invalid!  
    return 0;  
}
```

# Random numbers using Time

```c
#include <stdio.h>  
#include <stdlib.h>  
#include <time.h>  
  
int rnd();  
void seedRnd();  
  
int main() {  
    seedRnd();  
  
    puts("Behold! 100 Numbers!\n");  
  
    for (int i = 1; i <= 100; ++i) {  
        printf("%d\t", rnd());  
        if (i % 7 == 0)  
            printf("\n");  
    }  
    return 0;  
}  
  
int rnd() {  
    int r = rand();  
  
    return r;  
}  
  
void seedRnd() {  
    srand((unsigned) time(NULL));  
}
```

## Random numbers using Seeds

```c
#include <stdio.h>  
#include <stdlib.h>  
  
int rnd(void);  
void seedRnd(void);  
  
int main() {  
    seedRnd();  
  
    puts("Behold! 100 Random Numbers!\n");  
  
    for (int i = 0; i < 100; ++i)  
        printf("%d\t", rnd());  
  
    return 0;  
}  
  
int rnd(void) {  
    int r = rand();  
    return r;  
}  
  
void seedRnd() {  
    int seed;  
    char s[6];  
  
    printf("Enter a random number seed (2 - 6500): ");  
    seed = (unsigned) atoi(gets(s));  
  
    srand(seed);  
}
```

# HashMap Impl

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdint.h>
#include <stdbool.h>
#include <time.h>

typedef struct {
    char** keys;
    char** values;
    int count;
} HashTable;

void HashTableInitialize(HashTable* hashTable, int count) {
    *hashTable = (HashTable) {
        .keys = malloc(sizeof(char*)*count),
        .values = malloc(sizeof(char*)*count),
        .count = count
    };

    memset(hashTable->keys, 0, sizeof(char*)*count);
    memset(hashTable->values, 0, sizeof(char*)*count);
}


unsigned long GenHash(char *str) {
    unsigned long hash = 5381;
    int c;

    while ((c = *str++))
        hash = ((hash << 5) + hash) + c;

    return hash;
}

bool HashTableAdd(HashTable* hashTable, char* key, char* value) {
    int index = GenHash(key) % hashTable->count;

    if (hashTable->keys[index] != NULL) {
        printf("[COLLISION!!!] Key exists at index: %d, `%s`.\n", index, hashTable->keys[index]);
        return false;
    }

    hashTable->keys[index] = strdup(key);
    hashTable->values[index] = strdup(value);

    return true;
}

char* HashTableGetValueAtKey(HashTable* hashTable, char* key) {
    int index = GenHash(key) % hashTable->count;

    if (hashTable->keys[index] != NULL) {
        return hashTable->values[index];
    }
    else {
        return NULL;
    }
}

bool HashTableRemove(HashTable* hashTable, char* key) {
    int index = GenHash(key) % hashTable->count;


    if (hashTable->keys[index] == NULL) {
        return false;
    }
    else if (strcmp(hashTable->keys[index], key) != 0) {
        return false;
    }

    free(hashTable->keys[index]);
    free(hashTable->values[index]);

    memset(hashTable->keys+index, 0, sizeof(char*));
    memset(hashTable->values+index, 0, sizeof(char*));

    return true;
}

int main(void) {
    HashTable hashTable = {};
    HashTableInitialize(&hashTable, 10);

    int success = HashTableAdd(&hashTable, "name", "Ali");
    printf("Success adding key: %d\n", success);
    success = HashTableAdd(&hashTable, "surname", "Awan");
    printf("Success adding key: %d\n", success);
    printf("Key: `%s`, Value: `%s`\n", "surname", HashTableGetValueAtKey(&hashTable, "surname"));
    printf("Key: `%s`, Value: `%s`\n", "asdlkfjsd;l", HashTableGetValueAtKey(&hashTable, "asdlkfjsd;l"));
    printf("Removed key: %d\n", HashTableRemove(&hashTable, "surname"));
    printf("Removed key: %d\n", HashTableRemove(&hashTable, "random"));
    success = HashTableAdd(&hashTable, "al", "Awan");
    printf("Success adding key: %d\n", success);
    success = HashTableAdd(&hashTable, "alb", "Awan");
    printf("Success adding key: %d\n", success);
    success = HashTableAdd(&hashTable, "ssdflkjsdafurname", "Awan");
    printf("Success adding key: %d\n", success);
    success = HashTableAdd(&hashTable, "flkj;urname", "Awan");
    printf("Success adding key: %d\n", success);
    success = HashTableAdd(&hashTable, "askdfsurname", "Awan");
    printf("Success adding key: %d\n", success);


    return 0;
} 
}
```

# HashMap Collisions Impl

```c
#include <stdio.h>  
#include <stdlib.h>  
#include <string.h>  
#include <stdint.h>  
  
typedef struct HashNode {  
    char* key;  
    char* value;  
    struct HashNode* parent;  
    struct HashNode* child;  
} HashNode;  
  
typedef struct {  
    HashNode* data;  
    int count;  
} HashTable;  
  
void HashTableInitialize(HashTable* hashTable, int count) {  
    *hashTable = (HashTable) {  
        .data = malloc(sizeof(HashNode)*count),  
        .count = count  
    };  
  
    memset(hashTable->data, 0, sizeof(HashNode)*count);  
}  
  
  
unsigned long GenHash(char *str) {  
    unsigned long hash = 5381;  
    int c;  
  
    while ((c = *str++))  
        hash = ((hash << 5) + hash) + c;  
  
    return hash;  
}  
  
bool HashTableAdd(HashTable* hashTable, char* key, char* value) {  
    int index = GenHash(key) % hashTable->count;  
  
    if (hashTable->data[index].key == NULL) {  
        hashTable->data[index] = (HashNode) {  
            .key = strdup(key),  
            .value = strdup(value),  
            .parent = NULL,  
            .child = NULL  
        };  
  
        return true;  
    }    else {  
        HashNode* node = &hashTable->data[index];  
        for (; node->child != NULL; node = node->child);  
        node->child = malloc(sizeof(HashNode));  
        *node->child = (HashNode) {  
            .key = strdup(key),  
            .value = strdup(value),  
            .parent = node,  
            .child = NULL  
        };  
  
        return true;  
    }  
  
    return false;  
}  
  
char* HashTableGetValueAtKey(HashTable* hashTable, char* key) {  
    int index = GenHash(key) % hashTable->count;  
  
    if (hashTable->data[index].key != NULL) {  
        for (HashNode* node = &hashTable->data[index]; node != NULL; node = node->child) {  
            if (!strcmp(node->key, key)) {  
                return node->value;  
            }        }    }  
    return NULL;  
}  
  
bool HashTableRemove(HashTable* hashTable, char* key) {  
    int index = GenHash(key) % hashTable->count;  
  
    if (hashTable->data[index].key == NULL) {  
        return false;  
    }  
    for (HashNode* node = &hashTable->data[index]; node != NULL; node = node->child) {  
        if (!strcmp(node->key, key)) {  
            if (node->parent == NULL) {  
                free(node->key);  
                free(node->value);  
  
                if (node->child != NULL) {  
                    hashTable->data[index] = *node->child;  
                    node->child->parent = NULL;  
                }  
                return true;  
            }            else if (node->parent != NULL && node->child != NULL) {  
                free(node->key);  
                free(node->value);  
                node->parent->child = node->child;  
                free(node);  
                return true;  
            }            else {  
                node->parent = NULL;  
                free(node->key);  
                free(node->value);  
                free(node);  
                return true;  
            }        }    }  
    return false;  
}  
  
int main(void) {  
    HashTable hashTable = {};  
    HashTableInitialize(&hashTable, 3);  
  
    int success = HashTableAdd(&hashTable, "name", "Ali");  
    printf("Success adding key: %d\n", success);  
    success = HashTableAdd(&hashTable, "surname", "Awan");  
    success = HashTableAdd(&hashTable, "ala", "Ali");  
    printf("Success adding key: %d\n", success);  
    success = HashTableAdd(&hashTable, "afa", "Awan");  
    printf("Success adding key: %d\n", success);  
    success = HashTableAdd(&hashTable, "af", "Awan");  
    printf("Success adding key: %d\n", success);  
    success = HashTableAdd(&hashTable, "fa", "Awan");  
    printf("Key: `%s`, Value: `%s`\n", "surname", HashTableGetValueAtKey(&hashTable, "surname"));  
    printf("Key: `%s`, Value: `%s`\n", "asdlkfjsd;l", HashTableGetValueAtKey(&hashTable, "asdlkfjsd;l"));  
    printf("Removed key: %d\n", HashTableRemove(&hashTable, "surname"));  
    printf("Removed key: %d\n", HashTableRemove(&hashTable, "random"));
  
    return 0;  
}
```