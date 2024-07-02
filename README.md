#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>

const int LINHAS = 10;
const int COLUNAS = 10;

struct Posicao {
    int x, y;
};

// Função para exibir o labirinto
void exibirLabirinto(const std::vector<std::vector<int>> &labirinto, const Posicao &jogador) {
    for (int i = 0; i < LINHAS; ++i) {
        for (int j = 0; j < COLUNAS; ++j) {
            if (i == jogador.x && j == jogador.y) {
                std::cout << "P ";
            } else if (labirinto[i][j] == 0) {
                std::cout << ". ";
            } else if (labirinto[i][j] == 1) {
                std::cout << "# ";
            } else if (labirinto[i][j] == 2) {
                std::cout << "B ";
            } else if (labirinto[i][j] == 3) {
                std::cout << "E ";
            }
        }
        std::cout << "\n";
    }
}

void mostrarMenu();
void jogar();
void escolherMapa();
void sobre();
void fim();

std::vector<std::vector<int>> carregarMapa(int escolhaMapa);
void moverJogador(std::vector<std::vector<int>> &labirinto, Posicao &jogador, int dx, int dy, int &movimentos);
void teletransportar(Posicao &jogador);

int main() {
    int escolha;
    do {
        mostrarMenu();
        std::cin >> escolha;
        switch (escolha) {
            case 1:
                jogar();
                break;
            case 2:
                escolherMapa();
                break;
            case 3:
                sobre();
                break;
            case 4:
                fim();
                break;
            default:
                std::cout << "Opção inválida! Tente novamente.\n";
                break;
        }
    } while (escolha != 4);
    return 0;
}

void mostrarMenu() {
    std::cout << "Menu:\n";
    std::cout << "1. Jogar\n";
    std::cout << "2. Mapa\n";
    std::cout << "3. Sobre\n";
    std::cout << "4. Fim\n";
    std::cout << "Escolha uma opção: ";
}

void jogar() {
    int escolhaMapa = 1; // Mapa padrão
    std::vector<std::vector<int>> labirinto = carregarMapa(escolhaMapa);
    Posicao jogador = {0, 0};
    int movimentos = 0;
    char comando;

    std::cout << "Controles: w (cima), a (esquerda), s (baixo), d (direita), t (teletransporte), q (sair)\n";

    while (true) {
        exibirLabirinto(labirinto, jogador);
        std::cout << "Movimentos: " << movimentos << "\n";
        std::cout << "Comando: ";
        std::cin >> comando;

        if (comando == 'q') break;

        switch (comando) {
            case 'w':
                moverJogador(labirinto, jogador, -1, 0, movimentos);
                break;
            case 'a':
                moverJogador(labirinto, jogador, 0, -1, movimentos);
                break;
            case 's':
                moverJogador(labirinto, jogador, 1, 0, movimentos);
                break;
            case 'd':
                moverJogador(labirinto, jogador, 0, 1, movimentos);
                break;
            case 't':
                teletransportar(jogador);
                movimentos++;
                break;
            default:
                std::cout << "Comando inválido! Tente novamente.\n";
                break;
        }

        // Verificar se o jogador alcançou a saída
        if (labirinto[jogador.x][jogador.y] == 3) {
            std::cout << "Parabéns! Você venceu o jogo em " << movimentos << " movimentos.\n";
            break;
        }
    }
}

void escolherMapa() {
    std::cout << "Escolha o mapa:\n";
    std::cout << "1. Mapa 1\n";
    std::cout << "2. Mapa 2\n";
    std::cout << "Escolha uma opção: ";
    int escolha;
    std::cin >> escolha;
    std::vector<std::vector<int>> labirinto = carregarMapa(escolha);
    std::cout << "Mapa escolhido:\n";
    Posicao jogador = {0, 0}; // Posição inicial fictícia apenas para exibir o mapa
    exibirLabirinto(labirinto, jogador);
}

void sobre() {
    std::cout << "Desenvolvedor: Gustavo Rossi\n";
    std::cout << "Data: 1 de julho de 2024\n";
    std::cout << "Professor: Felski\n";
    std::cout << "Disciplina: Algoritmos e Programação I\n";
}

void fim() {
    std::cout << "Encerrando o programa...\n";
}

std::vector<std::vector<int>> carregarMapa(int escolhaMapa) {
    std::vector<std::vector<int>> mapa1 = {
        {0, 0, 1, 0, 0, 0, 1, 0, 0, 0},
        {1, 0, 1, 0, 1, 0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 0, 1, 0},
        {0, 1, 1, 1, 1, 1, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 1, 0, 0, 0},
        {0, 1, 1, 1, 1, 0, 1, 1, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 0, 1, 0},
        {1, 0, 1, 1, 1, 1, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 0, 1, 0},
        {0, 1, 1, 1, 1, 1, 1, 0, 3, 0}
    };

    std::vector<std::vector<int>> mapa2 = {
        {0, 0, 1, 0, 0, 0, 1, 0, 0, 0},
        {1, 0, 1, 0, 1, 0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 0, 1, 0},
        {0, 1, 1, 1, 1, 1, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 1, 0, 0, 0},
        {0, 1, 1, 1, 1, 0, 1, 1, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 0, 1, 0},
        {1, 0, 1, 1, 1, 1, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 0, 1, 0},
        {0, 1, 1, 1, 1, 1, 1, 0, 3, 0}
    };

    if (escolhaMapa == 2) {
        return mapa2;
    } else {
        return mapa1;
    }
}

void moverJogador(std::vector<std::vector<int>> &labirinto, Posicao &jogador, int dx, int dy, int &movimentos) {
    int nx = jogador.x + dx;
    int ny = jogador.y + dy;
    int nx2 = nx + dx;
    int ny2 = ny + dy;

    if (nx >= 0 && nx < LINHAS && ny >= 0 && ny < COLUNAS) {
        if (labirinto[nx][ny] == 0) {
            jogador.x = nx;
            jogador.y = ny;
            movimentos++;
        } else if (labirinto[nx][ny] == 2) {
            if (nx2 >= 0 && nx2 < LINHAS && ny2 >= 0 && ny2 < COLUNAS && labirinto[nx2][ny2] == 0) {
                labirinto[nx2][ny2] = 2;
                labirinto[nx][ny] = 0;
                jogador.x = nx;
                jogador.y = ny;
                movimentos++;
            }
        }
    }
}

void teletransportar(Posicao &jogador) {
    std::srand(std::time(0));
    jogador.x = std::rand() % LINHAS;
    jogador.y = std::rand() % COLUNAS;
}
