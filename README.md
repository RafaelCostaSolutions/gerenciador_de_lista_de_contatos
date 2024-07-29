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

class Contact
{
    public string Name { get; set; }
    public string Phone { get; set; }
    public string Email { get; set; }

    public override string ToString()
    {
        return $"{Name}, {Phone}, {Email}";
    }
}

class Program
{
    static List<Contact> contacts = new List<Contact>();
    static string filePath = "contacts.txt";

    static void Main()
    {
        LoadContacts();
        while (true)
        {
            Console.WriteLine("\nContact Manager");
            Console.WriteLine("1. Add Contact");
            Console.WriteLine("2. Edit Contact");
            Console.WriteLine("3. Remove Contact");
            Console.WriteLine("4. View Contacts");
            Console.WriteLine("5. Exit");
            Console.Write("Choose an option: ");
            var choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    AddContact();
                    break;
                case "2":
                    EditContact();
                    break;
                case "3":
                    RemoveContact();
                    break;
                case "4":
                    ViewContacts();
                    break;
                case "5":
                    SaveContacts();
                    return;
                default:
                    Console.WriteLine("Invalid option, try again.");
                    break;
            }
        }
    }

    static void AddContact()
    {
        var contact = new Contact();

        Console.Write("Enter name: ");
        contact.Name = Console.ReadLine();

        Console.Write("Enter phone: ");
        contact.Phone = Console.ReadLine();

        Console.Write("Enter email: ");
        contact.Email = Console.ReadLine();

        contacts.Add(contact);
        Console.WriteLine("Contact added successfully.");
    }

    static void EditContact()
    {
        Console.Write("Enter the name of the contact to edit: ");
        var name = Console.ReadLine();
        var contact = contacts.FirstOrDefault(c => c.Name.Equals(name, StringComparison.OrdinalIgnoreCase));

        if (contact != null)
        {
            Console.Write("Enter new name (leave blank to keep current): ");
            var newName = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(newName))
            {
                contact.Name = newName;
            }

            Console.Write("Enter new phone (leave blank to keep current): ");
            var newPhone = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(newPhone))
            {
                contact.Phone = newPhone;
            }

            Console.Write("Enter new email (leave blank to keep current): ");
            var newEmail = Console.ReadLine();
            if (!string.IsNullOrWhiteSpace(newEmail))
            {
                contact.Email = newEmail;
            }

            Console.WriteLine("Contact updated successfully.");
        }
        else
        {
            Console.WriteLine("Contact not found.");
        }
    }

    static void RemoveContact()
    {
        Console.Write("Enter the name of the contact to remove: ");
        var name = Console.ReadLine();
        var contact = contacts.FirstOrDefault(c => c.Name.Equals(name, StringComparison.OrdinalIgnoreCase));

        if (contact != null)
        {
            contacts.Remove(contact);
            Console.WriteLine("Contact removed successfully.");
        }
        else
        {
            Console.WriteLine("Contact not found.");
        }
    }

    static void ViewContacts()
    {
        if (contacts.Any())
        {
            Console.WriteLine("\nContacts:");
            foreach (var contact in contacts)
            {
                Console.WriteLine(contact);
            }
        }
        else
        {
            Console.WriteLine("No contacts found.");
        }
    }

    static void LoadContacts()
    {
        if (File.Exists(filePath))
        {
            var lines = File.ReadAllLines(filePath);
            foreach (var line in lines)
            {
                var parts = line.Split(',');
                if (parts.Length == 3)
                {
                    contacts.Add(new Contact { Name = parts[0].Trim(), Phone = parts[1].Trim(), Email = parts[2].Trim() });
                }
            }
        }
    }

    static void SaveContacts()
    {
        var lines = contacts.Select(c => $"{c.Name}, {c.Phone}, {c.Email}");
        File.WriteAllLines(filePath, lines);
    }
}
