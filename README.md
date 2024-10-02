#include <iostream>
#include <fstream>

using namespace std;

const int MAX_NOME = 50; // Tamanho máximo para o nome do produto

struct Produto {
    int id;
    char nome[MAX_NOME]; // Usando C-string
    float preco;
    int quantidade;
    char status; // 'A' para Ativo, 'D' para Eliminado
};

// Função para adicionar um novo produto
void adicionarProduto(Produto produtos[], int& quantidadeAtual) {
    cout << "Nome do produto: ";
    cin >> produtos[quantidadeAtual].nome;
    cout << "Preço do produto: ";
    cin >> produtos[quantidadeAtual].preco;
    cout << "Quantidade em estoque: ";
    cin >> produtos[quantidadeAtual].quantidade;
    produtos[quantidadeAtual].id = quantidadeAtual + 1; // ID baseado na posição
    produtos[quantidadeAtual].status = 'A'; // Produto ativo
    quantidadeAtual++;
}

// Função para exibir todos os produtos ativos
void exibirProdutos(const Produto produtos[], int quantidadeAtual) {
    for (int i = 0; i < quantidadeAtual; i++) {
        if (produtos[i].status == 'A') {
            cout << "ID: " << produtos[i].id 
                 << ", Nome: " << produtos[i].nome 
                 << ", Preço: " << produtos[i].preco 
                 << " €, Quantidade: " << produtos[i].quantidade << endl;
        }
    }
}

// Função para gravar os produtos em um arquivo
void salvarProdutosNoArquivo(const Produto produtos[], int quantidadeAtual) {
    ofstream arquivo("produtos.txt");
    for (int i = 0; i < quantidadeAtual; i++) {
        arquivo << produtos[i].id << ","
                << produtos[i].nome << ","
                << produtos[i].preco << ","
                << produtos[i].quantidade << ","
                << produtos[i].status << endl;
    }
    arquivo.close();
    cout << "Produtos salvos no arquivo com sucesso." << endl;
}

// Função para calcular o valor total do estoque
float calcularValorTotal(const Produto produtos[], int quantidadeAtual) {
    float valorTotal = 0.0;
    for (int i = 0; i < quantidadeAtual; i++) {
        if (produtos[i].status == 'A') {
            valorTotal += produtos[i].preco * produtos[i].quantidade;
        }
    }
    return valorTotal;
}

// Função para alterar um produto
void alterarProduto(Produto produtos[], int quantidadeAtual) {
    int id;
    cout << "Informe o ID do produto que deseja alterar: ";
    cin >> id;

    bool encontrado = false;
    for (int i = 0; i < quantidadeAtual; i++) {
        if (produtos[i].id == id && produtos[i].status == 'A') {
            encontrado = true;
            cout << "Novo nome: ";
            cin >> produtos[i].nome;
            cout << "Novo preço: ";
            cin >> produtos[i].preco;
            cout << "Nova quantidade: ";
            cin >> produtos[i].quantidade;
            cout << "Produto atualizado com sucesso!" << endl;
            break;
        }
    }

    if (!encontrado) {
        cout << "Produto com ID " << id << " não encontrado ou está eliminado." << endl;
    }
}

// Função para eliminar um produto (muda o status para 'D')
void eliminarProduto(Produto produtos[], int quantidadeAtual) {
    int id;
    cout << "Informe o ID do produto que deseja eliminar: ";
    cin >> id;

    bool encontrado = false;
    for (int i = 0; i < quantidadeAtual; i++) {
        if (produtos[i].id == id && produtos[i].status == 'A') {
            encontrado = true;
            produtos[i].status = 'D'; // Marca como eliminado
            cout << "Produto com ID " << id << " foi eliminado." << endl;
            break;
        }
    }

    if (!encontrado) {
        cout << "Produto com ID " << id << " não encontrado ou já foi eliminado." << endl;
    }
}

// Função principal
int main() {
    Produto produtos[100]; // Array para armazenar até 100 produtos
    int quantidadeAtual = 0; // Contador de produtos
    int escolha;

    do {
        cout << "Escolha uma opção:" << endl;
        cout << "1. Adicionar Produto" << endl;
        cout << "2. Exibir Produtos" << endl;
        cout << "3. Calcular Valor Total do estoque" << endl;
        cout << "4. Salvar Produtos em Arquivo" << endl;
        cout << "5. Alterar Produto" << endl;
        cout << "6. Eliminar Produto" << endl;
        cout << "0. Sair" << endl;
        cin >> escolha;

        switch (escolha) {
            case 1:
                adicionarProduto(produtos, quantidadeAtual);
                break;
            case 2:
                exibirProdutos(produtos, quantidadeAtual);
                break;
            case 3:
                cout << "Valor total do estoque: " << calcularValorTotal(produtos, quantidadeAtual) << " €" << endl;
                break;
            case 4:
                salvarProdutosNoArquivo(produtos, quantidadeAtual);
                break;
            case 5:
                alterarProduto(produtos, quantidadeAtual);
                break;
            case 6:
                eliminarProduto(produtos, quantidadeAtual);
                break;
            case 0:
                cout << "Saindo do programa..." << endl;
                break;
            default:
                cout << "Escolha uma opção inválida, tente novamente." << endl;
        }
    } while (escolha != 0);

    return 0;
}
