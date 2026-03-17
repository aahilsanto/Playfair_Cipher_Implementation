# EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## AIM:

To write a python program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.

## EXAMPLE:

![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.



## Program:

```python

def build_matrix(key):
    key = "".join(dict.fromkeys((key + "ABCDEFGHIKLMNOPQRSTUVWXYZ").upper().replace("J", "I")))
    return [list(key[i:i+5]) for i in range(0, 25, 5)]

def find_pos(matrix, ch):
    for i, row in enumerate(matrix):
        if ch in row:
            return i, row.index(ch)

def prepare(text):
    text = "".join(filter(str.isalpha, text.upper().replace("J", "I")))
    result = ""
    i = 0
    while i < len(text):
        result += text[i]
        if i + 1 < len(text):
            result += "X" if text[i] == text[i+1] else text[i+1]
            i += 2
        else:
            result += "X"
            i += 1
    return result

def playfair(text, matrix, mode):
    step = 1 if mode == "encrypt" else -1
    result = ""
    for i in range(0, len(text), 2):
        r1, c1 = find_pos(matrix, text[i])
        r2, c2 = find_pos(matrix, text[i+1])
        if r1 == r2:
            result += matrix[r1][(c1 + step) % 5] + matrix[r2][(c2 + step) % 5]
        elif c1 == c2:
            result += matrix[(r1 + step) % 5][c1] + matrix[(r2 + step) % 5][c2]
        else:
            result += matrix[r1][c2] + matrix[r2][c1]
    return result

# --- Main ---
key    = input("Enter key: ")
matrix = build_matrix(key)

print("\nPlayfair Matrix:")
for row in matrix:
    print(" ".join(row))

text      = input("\nEnter plain text: ")
prepared  = prepare(text)
encrypted = playfair(prepared, matrix, "encrypt")
decrypted = playfair(encrypted, matrix, "decrypt")

print(f"\nPrepared : {prepared}")
print(f"Encrypted: {encrypted}")
print(f"Decrypted: {decrypted}")
```

## Output:

<img width="667" height="492" alt="image" src="https://github.com/user-attachments/assets/f58aedc3-8aaa-4ef2-a5b2-78ad46903e8d" />

## Result:

Thus the program to implement Playfair using python is done successfully.
