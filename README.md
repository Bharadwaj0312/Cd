#include <stdio.h>
#include <ctype.h>
#include <string.h>

char keywords[10][10] = {
    "int", "float", "if", "else", "while",
    "for", "return", "char", "void", "double"
};

int isKeyword(char word[]) {
    for (int i = 0; i < 10; i++)
        if (strcmp(word, keywords[i]) == 0)
            return 1;
    return 0;
}

void analyze(FILE *fp) {
    char ch, buffer[100];
    int i;

    while ((ch = fgetc(fp)) != EOF) {
        if (isspace(ch))
            continue;

        // Identifier or Keyword
        if (isalpha(ch)) {
            i = 0;
            buffer[i++] = ch;
            while (isalnum(ch = fgetc(fp)))
                buffer[i++] = ch;
            buffer[i] = '\0';
            ungetc(ch, fp);
            if (isKeyword(buffer))
                printf("Keyword: %s\n", buffer);
            else
                printf("Identifier: %s\n", buffer);
        }

        // Number
        else if (isdigit(ch)) {
            i = 0;
            buffer[i++] = ch;
            while (isdigit(ch = fgetc(fp)))
                buffer[i++] = ch;
            buffer[i] = '\0';
            ungetc(ch, fp);
            printf("Number: %s\n", buffer);
        }

        // Operators
        else if (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '=')
            printf("Operator: %c\n", ch);

        // Special Symbols
        else if (ch == '(' || ch == ')' || ch == '{' || ch == '}' || ch == ';' || ch == ',')
            printf("Special Symbol: %c\n", ch);

        // Unknown
        else
            printf("Unknown: %c\n", ch);
    }
}

int main() {
    FILE *fp = fopen("source.c", "r");
    if (!fp) {
        printf("Error opening file!\n");
        return 1;
    }
    printf("Lexical Analysis Output:\n\n");
    analyze(fp);
    fclose(fp);
    return 0;
}