#include <stdio.h>  /* Needed for printing the output. */
#include <stdlib.h> /* Needed for rand(), which gives a random starting genome and picks a random site for mutation. */
#include <time.h>  /* Provides a random seed value for the rand() function */
#define GENOMESIZE 16    /* If you make this too long, the output won't fit on your screen. */
#define GUARDPAGE GENOMESIZE 
 
void bigBang();      /*Initializes, randomizes, and documents via printing to standard output. */
int getFitness();    /*Defined here as the number of elements matching their respective neighbors. */
void copyGenome(char *from, char *to); 
                    /*Copies the pattern from one chunk of RAM to another. */
void pointMutate(); /*Switches one element, chosen at random. */
char *describe();   /*Gives directions for writing the genome down. The first digit is the first bit of the genome. */
                    /*The remaining items tell how many of the same bits follow before you should change from */
				    /*ones to zeros, or vice versa. */
void crunch();      /*Prints the final genome with description and blithely ignores potential errors before exit. */
 
 
int main () { 
  int i, oldFitness, newFitness; 
  char genome[GENOMESIZE + 1]; /* Stores a pattern of '0' and '1' values. Initially random, but updated with improved patterns */
                               /* from the "newGenome" function. "c note": The '0' character is the number 48 in decimal.*/
  char newGenome[GENOMESIZE + 1];                   /* Holds a copy of the genome for mutation and testing. */
  char description[2 * GENOMESIZE + 2 + GUARDPAGE]; /* Call me paranoid, or call me if you can prove GUARDPAGE redundant. */
                                                    /* Holds the starting byte, then the lengths of runs of similar bytes, */
                                                    /* separated by commas. */
   
  bigBang(genome, newGenome, description); /* Initializes a random genome, and puts zero delimiters on the arrays.   
                                              The initial genome string is printed, along with a concise description.*/
  /*The random improvement process:
    A trial copy is made, and stored in "newGenome". "newGenome" gets mutated, and if the new fitness is better than the 
    old fitness, the new genome is substituted for the old. */
   
  for (i = 0; i < 1000; i++){ 
    oldFitness = getFitness(genome); 
    copyGenome(genome, newGenome); 
    pointMutate(newGenome); 
    newFitness = getFitness(newGenome); 
    if (newFitness > oldFitness) { 
      copyGenome(newGenome, genome); 
    } 
  }    
  crunch(genome, description);   /* Prints the final genome string, and a concise description. */ 
  return 0; 
} 
void bigBang(char g[], char n[], char d[]){ /* Assumes paarameters as big as GENOMESIZE. */
  int i;          /* The first parameter will be filled with random '0's and '1's.     */
  time_t t;       /* The following three lines use the time as a random number.        */
  t = time(&t);   /* So that the series of rand() calls to follow are different each   */
  srand((unsigned int)t);             /* time you run the program.                     */
  for(i = 0; i < GENOMESIZE; i++){    /* This would not work if you call the program   */
    g[i] = '0' + (char)(rand() & 1);  /* more than once in a second. */
  } 
  g[GENOMESIZE] = 0; 
  n[GENOMESIZE] = 0; 
  d[GENOMESIZE + 1] = 0; 
  printf("bigBang: %s:  Description: %s\n", g, describe(g, d) ); 
} 
void pointMutate(char dna[]){                 /* Flips one bit of the given array... */
  int site = (int)(rand() % GENOMESIZE);      /* Chosen at random.                 */
  dna[site]  = (dna[site] == '0') ? '1': '0'; /* Ain't c sweet?                    */
} 
void   copyGenome(char from[], char to[]){ 
  int i; 
  for (i = 0; i < GENOMESIZE; i++) to[i] = from [i]; 
} 
char * describe(char object[], char description[]){ 
  int descriptionCursor, runLength, i; 
  descriptionCursor = runLength = i = 0; 
  descriptionCursor +=  sprintf(description , "%d",  object[0] - '0'); /* Start with the first digit text of the given string.*/ 
  object[GENOMESIZE] = object[0];                 /* Replace the null terminator with a copy of the first byte (Dangerous*). */
                                                  /* Thus the final bit is compared to the first bit in a wraparound. */
  for (i = 0; i < GENOMESIZE; i++, runLength++){  /* Go over the given string assuming GENOMESIZE doesn't overflow.   */
    if (object[i] != object[i+1]) {               /* See if the bit is the same as its neighbor.                      */
      descriptionCursor += sprintf(description + descriptionCursor, ",%d", runLength); /* If not, record the count so far.*/
      runLength = 0;                              /* and reset the count of similar bits.*/
    } 
  } 
  object[GENOMESIZE] = 0;                         /* (*End danger) Repaired the object so it will be a safe c string again. */
  return description; 
} 
int getFitness(char *digits){   /* "Fitness" is incremented whenever the string        */
  int i, Fitness = 0;           /*  has a character equal to its right-hand neighbor.  */
  for(i = 0; i < GENOMESIZE; i++){  /* Computing fitness and describing the string are */
    if( digits[i] == digits[ (i+GENOMESIZE-1) % GENOMESIZE ] ){  /* similar, but not   */
      Fitness++  ;                                               /* identical.         */
    } 
  } 
  return Fitness; 
} 
void crunch(char g[], char d[]){ 
  printf("crunch:    %s:  Description: %s\n", g, describe(g, d) ); 
} 
