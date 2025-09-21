
#define _POSIX_C_SOURCE 200809L
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define _GNU_SOURCE
int main() {

  char *line;
  ssize_t read;
  size_t len;
  char *saveptr;
  read = getline(&line, &len, stdin);

  char *token;
  token = strtok_r(line, " ", &saveptr);
  while (token != NULL) {
    printf("  '%s'\n", token);
    token = strtok_r(NULL, " ", &saveptr);
  }

  free(line);
  return 0;
}
