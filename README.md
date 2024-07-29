# gerenciador_de_lista_de_contatos

Um programa de console para gerenciar uma lista de contatos, permitindo adicionar, editar, remover e visualizar contatos.

Funcionalidades
Adicionar novos contatos com informações como nome, telefone e e-mail.
Editar informações de contatos existentes.
Remover contatos.
Visualizar a lista de contatos.
Armazenar dados em um arquivo para persistência.

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

class Contato
{
    public string Nome { get; set; }
    public string Telefone { get; set; }
    public string Email { get; set; }

    public override string ToString()
    {
        return $"{Nome}, {Telefone}, {Email}";
    }
}

class Program
{
    static List<Contato> contatos = new List<Contato>();
    static string caminhoArquivo = "contatos.txt";

    static void Main()
    {
        CarregarContatos();
        while (true)
        {
            Console.WriteLine("\nGerenciador de Contatos");
            Console.WriteLine("1. Adicionar Contato");
            Console.WriteLine("2. Editar Contato");
            Console.WriteLine("3. Remover Contato");
            Console.WriteLine("4. Visualizar Contatos");
            Console.WriteLine("5. Sair");
            Console.Write("Escolha uma opção: ");
            var escolha = Console.ReadLine();

            switch (escolha)
            {
                case "1":
                    AdicionarContato();
                    break;
                case "2":
                    EditarContato();
                    break;
                case "3":
                    RemoverContato();
                    break;
                case "4":
                    VisualizarContatos();
                    break;
                case "5":
                    SalvarContatos();
                    return;
                default:
                    Console.WriteLine("Opção inválida, tente novamente.");
                    break;
            }
        }
    }

    static void AdicionarContato()
    {
        var contato = new Contato();

        Console.Write("Digite o nome: ");
        contato.Nome = Console.ReadLine();

        Console.Write("Digite o telefone: ");
        contato.Telefone = Console.ReadLine();

        Console.Write("Digite o email: ");
        contato.Email = Console.ReadLine();

        contatos.Add(contato);
        Console.WriteLine("Contato adicionado com sucesso.");
    }

    static void EditarContato()
    {
        Console.Write("Digite o nome do contato a editar: ");
        var nome = Console.ReadLine();
        var contato = contatos.FirstOrDefault(c => c.Nome.Equals(nome, StringComparison.OrdinalIgnoreCase));

        if (contato != null)
        {
            Console.Write("Digite o novo nome (deixe em branco para manter o atual): ");
            var novoNome = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(novoNome))
            {
                contato.Nome = novoNome;
            }

            Console.Write("Digite o novo telefone (deixe em branco para manter o atual): ");
            var novoTelefone = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(novoTelefone))
            {
                contato.Telefone = novoTelefone;
            }

            Console.Write("Digite o novo email (deixe em branco para manter o atual): ");
            var novoEmail = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(novoEmail))
            {
                contato.Email = novoEmail;
            }

            Console.WriteLine("Contato atualizado com sucesso.");
        }
        else
        {
            Console.WriteLine("Contato não encontrado.");
        }
    }

    static void RemoverContato()
    {
        Console.Write("Digite o nome do contato a remover: ");
        var nome = Console.ReadLine();
        var contato = contatos.FirstOrDefault(c => c.Nome.Equals(nome, StringComparison.OrdinalIgnoreCase));

        if (contato != null)
        {
            contatos.Remove(contato);
            Console.WriteLine("Contato removido com sucesso.");
        }
        else
        {
            Console.WriteLine("Contato não encontrado.");
        }
    }

    static void VisualizarContatos()
    {
        if (contatos.Any())
        {
            Console.WriteLine("\nContatos:");
            foreach (var contato in contatos)
            {
                Console.WriteLine(contato);
            }
        }
        else
        {
            Console.WriteLine("Nenhum contato encontrado.");
        }
    }

    static void CarregarContatos()
    {
        if (File.Exists(caminhoArquivo))
        {
            var linhas = File.ReadAllLines(caminhoArquivo);
            foreach (var linha in linhas)
            {
                var partes = linha.Split(',');
                if (partes.Length == 3)
                {
                    contatos.Add(new Contato { Nome = partes[0].Trim(), Telefone = partes[1].Trim(), Email = partes[2].Trim() });
                }
            }
        }
    }

    static void SalvarContatos()
    {
        var linhas = contatos.Select(c => $"{c.Nome}, {c.Telefone}, {c.Email}");
        File.WriteAllLines(caminhoArquivo, linhas);
    }
}
