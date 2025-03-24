#include <stdio.h>

#define MAX 10

int main() {
    int n, m, i, j, k;
    int alloc[MAX][MAX], max[MAX][MAX], avail[MAX];
    int need[MAX][MAX], finish[MAX] = {0}, safeSeq[MAX], index = 0;

    printf("Enter number of processes and resources: ");
    scanf("%d %d", &n, &m);

    printf("Enter allocation matrix: \n");
    for (i = 0; i < n; i++)
        for (j = 0; j < m; j++)
            scanf("%d", &alloc[i][j]);

    printf("Enter max matrix: \n");
    for (i = 0; i < n; i++)
        for (j = 0; j < m; j++)
            scanf("%d", &max[i][j]);

    printf("Enter available resources: ");
    for (i = 0; i < m; i++)
        scanf("%d", &avail[i]);

    for (i = 0; i < n; i++)
        for (j = 0; j < m; j++)
            need[i][j] = max[i][j] - alloc[i][j];

    int count = 0;
    while (count < n) {
        int found = 0;
        for (i = 0; i < n; i++) {
            if (!finish[i]) {
                int flag = 1;
                for (j = 0; j < m; j++)
                    if (need[i][j] > avail[j]) {
                        flag = 0;
                        break;
                    }
                if (flag) {
                    for (k = 0; k < m; k++)
                        avail[k] += alloc[i][k];
                    safeSeq[index++] = i;
                    finish[i] = 1;
                    found = 1;
                    count++;
                }
            }
        }
        if (!found) {
            printf("System is in unsafe state!\n");
            return 0;
        }
    }

    printf("Safe sequence: ");
    for (i = 0; i < n; i++)
        printf("P%d ", safeSeq[i]);
    printf("\n");

    return 0;
}
