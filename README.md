
# CuckooHashing

DAA Assignment 
#### Name - Swyam Laira.
#### Sec - A
#### Roll No. - 67
#### Year/Sem - 3rd Yr/Vth Sem
#### Subject - DAA
#### Branch - CSE - 1

## Appendix

Cuckoo hashing is a form of open addressing in which each non-
empty cell of a hash table contains a key or key–value pair. A
 hash function is used to determine the location for each key, and its presence in the table (or the value associated with it) can be found by examining that cell of the table. However, open addressing suffers from collisions, which happens when more than one key is mapped to the same cell. The basic idea of cuckoo hashing is to resolve collisions by using two hash functions instead of only one. This provides two possible locations in the hash table for each key. In one of the commonly used variants of the algorithm, the hash table is split into two smaller tables of equal size, and each hash function provides an index into one of these two tables. It is also possible for both hash functions to provide indexes into a single table


## Functions

The following hash functions are given:

h1(key) = key%11

h2(key) = (key/11)%11


## Raw Example
The following two tables show the insertion of some example elements. Each column corresponds to the state of the two hash tables over time. The possible insertion locations for each new value are highlighted.

![Chash](https://user-images.githubusercontent.com/95228478/203807613-099173ee-0b3f-4a70-aa8f-f4397cf377b7.png)


## Code for Cuckoo Hash in distributed database application


// Java program to demonstrate working ofCuckoo hashing.

``` java 
import java.util.Arrays;

class CuckooHash {
   
    // upper bound on number of elements in our set

    static int MAXN = 11;

    // choices for position
    static int ver = 2;

    // Auxiliary space bounded by a small multiple of MAXN, minimizing wastage
    static int[][] hashtable = new int[ver][MAXN];

    // Array to store possible positions for a key
    static int[] pos = new int[ver];

    /* function to fill hash table with dummy value*/
    static void initTable() {
        for (int j = 0; j < MAXN; j++)
            for (int i = 0; i < ver; i++)
                hashtable[i][j] = Integer.MIN_VALUE;
    }

    /* return hashed value for a key */
    static int hash(int function, int key) {
        switch (function) {
            case 1:
                return key % MAXN;
            case 2:
                return (key / MAXN) % MAXN;
        }
        return Integer.MIN_VALUE;
    }

    /* function to place a key in one of its possible positions*/
    static void place(int key, int tableID, int cnt, int n) {
	/* if function has been recursively called max number
	of times, stop and declare cycle. Rehash. */
        if (cnt == n) {
            System.out.printf("%d unpositioned\n", key);
            System.out.printf("Cycle present. REHASH.\n");
            return;
        }

	/* calculate and store possible positions for the key.*/
        for (int i = 0; i < ver; i++) {
            pos[i] = hash(i + 1, key);
            if (hashtable[i][pos[i]] == key)
                return;
        }

	/* check if another key is already present at the
	position for the new key in the table */
        if (hashtable[tableID][pos[tableID]] != Integer.MIN_VALUE) {
            int dis = hashtable[tableID][pos[tableID]];
            hashtable[tableID][pos[tableID]] = key;
            place(dis, (tableID + 1) % ver, cnt + 1, n);
        } else // else: place the new key in its position
            hashtable[tableID][pos[tableID]] = key;
    }

    /* function to print hash table contents */
    static void printTable() {
        System.out.printf("Final hash tables:\n");

        for (int i = 0; i < ver; i++, System.out.printf("\n"))
            for (int j = 0; j < MAXN; j++)
                if (hashtable[i][j] == Integer.MIN_VALUE)
                    System.out.printf("- ");
                else
                    System.out.printf("%d ", hashtable[i][j]);

        System.out.printf("\n");
    }

    /* function for Cuckoo-hashing key*/
    static void cuckoo(int keys[], int n) {

        initTable();

        for (int i = 0, cnt = 0; i < n; i++, cnt = 0)
            place(keys[i], 0, cnt, n);

        // print the final hash tables
        printTable();
    }


    // Driver Code
    public static void main(String[] args) {


        int keys_1[] = new int[10];

        Arrays.fill(keys_1, 0);

        System.out.println("Entered Data into Database is = abcdefgbgr");
        char[] charArray = "abcdefgbgr".toCharArray();
        int[] intArray = new int[charArray.length];
        for (int i = 0; i < charArray.length; ++i) {
            intArray[i] = (int) charArray[i];
        }
        for (int i = 0; i < charArray.length; ++i)
            System.out.println(intArray[i]);

        System.out.println("Database Hashing and storage in form of Key Value Pair is -");
        keys_1 = new int[intArray.length];
        for (int i = 0; i < intArray.length; i++) {
            keys_1[i] = intArray[i];
        }


        int n = keys_1.length;

        cuckoo(keys_1, n);


        int keys_2[] = new int[10];
        Arrays.fill(keys_1, 0);

        System.out.println("Entered Data into Database is = assdafadfw");
        char[] charArray1 = "assdafadfw".toCharArray();
        int[] intArray1 = new int[charArray1.length];
        for (int i = 0; i < charArray1.length; ++i) {
            intArray1[i] = (int) charArray1[i];
        }
        for (int i = 0; i < charArray1.length; ++i)
            System.out.println(intArray1[i]);
        System.out.println("Database Hashing and storage in form of Key Value Pair is -");
        keys_2 = new int[intArray1.length];
        for (int i = 0; i < intArray1.length; i++) {
            keys_2[i] = intArray1[i];
        }

        int m = keys_2.length;
        cuckoo(keys_2, m);

    }
    
    }```



