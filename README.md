using System;
using System.Linq;
using System.Text;

class Program
{
    private const string Symbols = "!@#$%^&*()_+-=[]{}|;':\",.<>?";
    private const string UppercaseLetters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private const string LowercaseLetters = "abcdefghijklmnopqrstuvwxyz";
    private const string Numbers = "0123456789";

    private static readonly string[][] Permutations = {
        new[] { Numbers, LowercaseLetters, UppercaseLetters, Symbols },
        new[] { Numbers, LowercaseLetters, UppercaseLetters, Symbols },
        new[] { Numbers, LowercaseLetters, Symbols, UppercaseLetters },
        new[] { Numbers, LowercaseLetters, Symbols, UppercaseLetters },
        new[] { Numbers, Symbols, LowercaseLetters, UppercaseLetters },
        new[] { Numbers, Symbols, LowercaseLetters, UppercaseLetters },
        new[] { UppercaseLetters, Symbols, LowercaseLetters, Numbers },
        new[] { UppercaseLetters, Symbols, LowercaseLetters, Numbers },
        new[] { UppercaseLetters, Numbers, LowercaseLetters, Symbols },
        new[] { UppercaseLetters, Numbers, LowercaseLetters, Symbols },
        new[] { UppercaseLetters, Numbers, Symbols, LowercaseLetters },
        new[] { UppercaseLetters, Numbers, Symbols, LowercaseLetters },
        new[] { Symbols, Numbers, LowercaseLetters, UppercaseLetters },
        new[] { Symbols, Numbers, LowercaseLetters, UppercaseLetters },
        new[] { Symbols, LowercaseLetters, Numbers, UppercaseLetters },
        new[] { Symbols, LowercaseLetters, Numbers, UppercaseLetters },
        new[] { Symbols, LowercaseLetters, UppercaseLetters, Numbers },
        new[] { Symbols, LowercaseLetters, UppercaseLetters, Numbers },
        new[] { LowercaseLetters, Numbers, UppercaseLetters, Symbols },
        new[] { LowercaseLetters, Numbers, UppercaseLetters, Symbols },
        new[] { LowercaseLetters, Symbols, Numbers, UppercaseLetters },
        new[] { LowercaseLetters, Symbols, Numbers, UppercaseLetters },
        new[] { LowercaseLetters, Symbols, UppercaseLetters, Numbers },
        new[] { LowercaseLetters, Symbols, UppercaseLetters, Numbers },
    };

    private static readonly Random Random = new Random();

    private static string ShuffleCharacters(string input)
    {
        char[] characters = input.ToCharArray();
        int n = characters.Length;
        while (n > 1)
        {
            int k = Random.Next(n--);
            char temp = characters[n];
            characters[n] = characters[k];
            characters[k] = temp;
        }
        return new string(characters);
    }

    private static string ShuffleCharactersWithoutDuplicates(string input, string exclude = "")
    {
        char[] characters = input.ToCharArray();
        int n = characters.Length;
        while (n > 1)
        {
            int k = Random.Next(n--);
            char temp = characters[n];
            characters[n] = characters[k];
            characters[k] = temp;
        }

        string result = new string(characters.Except(exclude).ToArray());
        return result;
    }

    private static char GetRandomCharacter(string type, string exclude = "")
    {
        string possibleCharacters = new string(type.Except(exclude).ToArray());
        int index = Random.Next(possibleCharacters.Length);
        return possibleCharacters[index];
    }

    private static string GeneratePassword(int length)
    {
        string[] permutation = Permutations[Random.Next(Permutations.Length)];

        StringBuilder password = new StringBuilder();

        for (int i = 0; i < length; i++)
        {
            string type = permutation[i % permutation.Length]; // Correction ici pour s'assurer de ne pas dépasser la longueur de la permutation
            char randomCharacter = GetRandomCharacter(type, password.ToString());
            password.Append(randomCharacter);
        }

        password = new StringBuilder(RemoveConsecutiveDuplicates(password.ToString()));

        // Si la longueur de la permutation est inférieure à la longueur demandée, ajouter des caractères aléatoires supplémentaires
        while (password.Length < length)
        {
            string type = permutation[Random.Next(permutation.Length)];
            char randomCharacter = GetRandomCharacter(type, password.ToString());
            password.Append(randomCharacter);
        }

        // Tronquer le mot de passe si sa longueur dépasse celle demandée
        if (password.Length > length)
        {
            password.Length = length;
        }

        return password.ToString();
    }

    private static string RemoveConsecutiveDuplicates(string password)
    {
        if (password.Length <= 1)
        {
            return password;
        }

        StringBuilder cleanedPassword = new StringBuilder(password[0].ToString());

        for (int i = 1; i < password.Length; i++)
        {
            if (password[i] != password[i - 1])
            {
                cleanedPassword.Append(password[i]);
            }
        }

        return cleanedPassword.ToString();
    }

    static void Main()
    {
        int passwordLength = 16;
        Console.WriteLine("Generated Passwords:");

        for (int i = 0; i < 10; i++)
        {
            string password = GeneratePassword(passwordLength);
            Console.WriteLine($"Password {i + 1}: {password}");
        }
    }
}
