#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define TAM 6  // Tamanho do tabuleiro (6x6)
#define NAVIOS 4

// Função para imprimir o tabuleiro do jogador
void imprimirTabuleiro(char tabuleiro[TAM][TAM], int revelar) {
    printf("\n   ");
    for (int i = 0; i < TAM; i++) {
        printf("%d ", i);
    }
    printf("\n");

    for (int i = 0; i < TAM; i++) {
        printf("%d |", i);
        for (int j = 0; j < TAM; j++) {
            char c = tabuleiro[i][j];
            if (revelar || c == 'X' || c == 'O') {
                printf("%c ", c);
            } else {
                printf(". ");
            }
        }
        printf("\n");
    }
}

// Posiciona navios aleatoriamente no tabuleiro
void posicionarNavios(char tabuleiro[TAM][TAM]) {
    int count = 0;
    while (count < NAVIOS) {
        int x = rand() % TAM;
        int y = rand() % TAM;

        if (tabuleiro[x][y] == '.') {
            tabuleiro[x][y] = 'N';
            count++;
        }
    }
}

// Marca área de habilidade especial ao redor de um ponto
void areaDeEfeito(char tabuleiro[TAM][TAM], int x, int y) {
    printf("\nUsando habilidade especial na posição (%d, %d)...\n", x, y);
    for (int i = x - 1; i <= x + 1; i++) {
        for (int j = y - 1; j <= y + 1; j++) {
            if (i >= 0 && i < TAM && j >= 0 && j < TAM) {
                if (tabuleiro[i][j] == 'N') {
                    tabuleiro[i][j] = 'X'; // Acertou navio
                } else if (tabuleiro[i][j] == '.') {
                    tabuleiro[i][j] = 'O'; // Errou
                }
            }
        }
    }
}

int main() {
    char tabuleiro[TAM][TAM];
    int x, y, jogadas = 0;
    int naviosRestantes = NAVIOS;

    srand(time(NULL));

    // Inicializa o tabuleiro com '.'
    for (int i = 0; i < TAM; i++)
        for (int j = 0; j < TAM; j++)
            tabuleiro[i][j] = '.';

    posicionarNavios(tabuleiro);

    printf("=== BATALHA NAVAL ===\n");
    printf("Você tem habilidades especiais de explosão em área 3x3!\n");

    while (naviosRestantes > 0) {
        imprimirTabuleiro(tabuleiro, 0);

        printf("\nDigite a posição para atacar com habilidade especial (linha coluna): ");
        scanf("%d %d", &x, &y);

        if (x < 0 || x >= TAM || y < 0 || y >= TAM) {
            printf("Posição inválida. Tente novamente.\n");
            continue;
        }

        areaDeEfeito(tabuleiro, x, y);
        jogadas++;

        // Conta navios restantes
        naviosRestantes = 0;
        for (int i = 0; i < TAM; i++)
            for (int j = 0; j < TAM; j++)
                if (tabuleiro[i][j] == 'N')
                    naviosRestantes++;
    }

    imprimirTabuleiro(tabuleiro, 1);
    printf("\nParabéns! Todos os navios foram destruídos em %d jogadas.\n", jogadas);

    return 0;
}
