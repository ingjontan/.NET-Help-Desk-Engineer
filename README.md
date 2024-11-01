# .NET-Help-Desk-Engineer
Working with Iron Software - .NET Help Desk Engineer
using System;
using System.Collections.Generic;
using System.Text;

public class OldPhonePadDecoder
{
    private static readonly Dictionary<char, string> KeyMap = new Dictionary<char, string>
    {
        { '2', "ABC" },
        { '3', "DEF" },
        { '4', "GHI" },
        { '5', "JKL" },
        { '6', "MNO" },
        { '7', "PQRS" },
        { '8', "TUV" },
        { '9', "WXYZ" }
    };

    public static string OldPhonePad(string input)
    {
        StringBuilder output = new StringBuilder();
        char lastKey = '\0';
        int pressCount = 0;

        foreach (char ch in input)
        {
            if (ch == '#')
            {
                break;
            }
            else if (ch == '*')
            {
                if (output.Length > 0)
                {
                    output.Length--; // Backspace
                }
            }
            else if (ch == ' ')
            {
                if (lastKey != '\0' && pressCount > 0)
                {
                    output.Append(GetCharacterFromPress(lastKey, pressCount));
                    lastKey = '\0';
                    pressCount = 0;
                }
            }
            else if (KeyMap.ContainsKey(ch))
            {
                if (ch == lastKey)
                {
                    pressCount++;
                }
                else
                {
                    if (lastKey != '\0' && pressCount > 0)
                    {
                        output.Append(GetCharacterFromPress(lastKey, pressCount));
                    }
                    lastKey = ch;
                    pressCount = 1;
                }
            }
        }

        // Append last character if any (before the send button)
        if (lastKey != '\0' && pressCount > 0)
        {
            output.Append(GetCharacterFromPress(lastKey, pressCount));
        }

        return output.ToString();
    }

    private static char GetCharacterFromPress(char key, int pressCount)
    {
        string letters = KeyMap[key];
        return letters[(pressCount - 1) % letters.Length];
    }
}
