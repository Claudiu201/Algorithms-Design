#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Function to find the minimum of three numbers
int min(int a, int b, int c) {
    if (a < b && a < c) return a;
    if (b < a && b < c) return b;
    return c;
}

// Function to calculate the minimum number of operations to correct the syntax
int min_operations_to_correct_syntax(const char *code_fragment, const char *valid_syntax) {
    int m = strlen(code_fragment);
    int n = strlen(valid_syntax);

    // Allocate memory for the DP table
    int **dp = (int **)malloc((m + 1) * sizeof(int *));
    for (int i = 0; i <= m; i++) {
        dp[i] = (int *)malloc((n + 1) * sizeof(int));
    }

    // Initialize the DP table
    for (int i = 0; i <= m; i++) {
        dp[i][0] = i;  // Deletion
    }
    for (int j = 0; j <= n; j++) {
        dp[0][j] = j;  // Insertion
    }

    // Fill the DP table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (code_fragment[i - 1] == valid_syntax[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = min(dp[i - 1][j] + 1,       // Deletion
                               dp[i][j - 1] + 1,       // Insertion
                               dp[i - 1][j - 1] + 1);  // Substitution
            }
        }
    }

    // Get the result from the DP table
    int result = dp[m][n];

    // Free the allocated memory
    for (int i = 0; i <= m; i++) {
        free(dp[i]);
    }
    free(dp);

    return result;
}

int main() {
    const char *code_fragment = "fnuc(myFuncion";
    const char *valid_syntax = "func(myFunction)";
    int result = min_operations_to_correct_syntax(code_fragment, valid_syntax);
    printf("Minimum number of operations: %d\n", result);
    return 0;
}
