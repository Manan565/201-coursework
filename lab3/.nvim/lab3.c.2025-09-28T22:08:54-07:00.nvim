#define _POSIX_C_SOURCE 200809L
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_HISTORY 5

// Node structure for our linked list
typedef struct Node {
  char *line;        // Pointer to the input string
  struct Node *next; // Pointer to next node
} Node;

// History structure to manage our list
typedef struct {
  Node *head; // Points to oldest entry
  Node *tail; // Points to newest entry
  int count;  // Number of entries (0 to MAX_HISTORY)
} History;

// Initialize an empty history
void init_history(History *hist) {
  hist->head = NULL;
  hist->tail = NULL;
  hist->count = 0;
}

// Add a line to the history
void add_to_history(History *hist, char *line) {
  // Create a new node
  Node *new_node = malloc(sizeof(Node));
  if (new_node == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    return;
  }

  // Duplicate the string so we own our own copy
  new_node->line = strdup(line);
  if (new_node->line == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    free(new_node);
    return;
  }
  new_node->next = NULL;

  // If history is full, remove the oldest entry (head)
  if (hist->count == MAX_HISTORY) {
    Node *old_head = hist->head;
    hist->head = old_head->next;

    free(old_head->line);
    free(old_head);
    hist->count--;

    // If we removed the only node
    if (hist->head == NULL) {
      hist->tail = NULL;
    }
  }

  // Add new node to the end
  if (hist->tail == NULL) {
    // First node
    hist->head = new_node;
    hist->tail = new_node;
  } else {
    hist->tail->next = new_node;
    hist->tail = new_node;
  }

  hist->count++;
}

// Print all entries in the history
void print_history(History *hist) {
  Node *current = hist->head;
  while (current != NULL) {
    printf("%s", current->line); // line already contains \n
    current = current->next;
  }
}

// Free all memory used by history
void free_history(History *hist) {
  Node *current = hist->head;
  while (current != NULL) {
    Node *next = current->next;
    free(current->line);
    free(current);
    current = next;
  }
  hist->head = NULL;
  hist->tail = NULL;
  hist->count = 0;
}

int main() {
  History hist;
  init_history(&hist);

  char *line = NULL; // getline will allocate this
  size_t len = 0;    // getline will set this
  ssize_t nread;

  while (1) {
    printf("Enter input: ");
    fflush(stdout);

    // Read a line from stdin
    nread = getline(&line, &len, stdin);

    // Check for EOF (Ctrl+D) or error
    if (nread == -1) {
      printf("\n");
      break;
    }

    // Add line to history
    add_to_history(&hist, line);

    // Check if the line is "print\n"
    if (strcmp(line, "print\n") == 0) {
      print_history(&hist);
    }
  }

  // Clean up
  free(line); // Free the buffer allocated by getline
  free_history(&hist);

  return 0;
}
