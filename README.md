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

```
def generate_matrix(key):
    key = key.upper().replace("J", "I")
    matrix = []
    used = set()

    for ch in key:
        if ch.isalpha() and ch not in used:
            used.add(ch)
            matrix.append(ch)

    for ch in "ABCDEFGHIKLMNOPQRSTUVWXYZ":
        if ch not in used:
            used.add(ch)
            matrix.append(ch)

    return [matrix[i:i+5] for i in range(0, 25, 5)]

def find_position(matrix, ch):
    for i in range(5):
        for j in range(5):
            if matrix[i][j] == ch:
                return i, j
                
def preprocess_text(text):
    text = text.upper().replace("J", "I")
    prepared = ""
    i = 0
    while i < len(text):
        if not text[i].isalpha():
            i += 1
            continue
        ch1 = text[i]
        
        if i + 1 < len(text):
            ch2 = text[i + 1]
            if not ch2.isalpha():
                prepared += ch1
                i += 1
            elif ch1 == ch2:
                prepared += ch1 + "X"
                i += 1
            else:
                prepared += ch1 + ch2
                i += 2
        else:
            prepared += ch1 + "X"
            i += 1

    if len(prepared) % 2 != 0:
        prepared += "X"
    return prepared

def encrypt(text, matrix):
    cipher = ""
    for i in range(0, len(text), 2):
        ch1, ch2 = text[i], text[i+1]
        r1, c1 = find_position(matrix, ch1)
        r2, c2 = find_position(matrix, ch2)

        if r1 == r2: 
            cipher += matrix[r1][(c1 + 1) % 5]
            cipher += matrix[r2][(c2 + 1) % 5]

        elif c1 == c2:  
            cipher += matrix[(r1 + 1) % 5][c1]
            cipher += matrix[(r2 + 1) % 5][c2]

        else:  
            cipher += matrix[r1][c2]
            cipher += matrix[r2][c1]
    return cipher

def decrypt(cipher, matrix):
    plain = ""
    for i in range(0, len(cipher), 2):
        ch1, ch2 = cipher[i], cipher[i+1]
        r1, c1 = find_position(matrix, ch1)
        r2, c2 = find_position(matrix, ch2)

        if r1 == r2: 
            plain += matrix[r1][(c1 - 1) % 5]
            plain += matrix[r2][(c2 - 1) % 5]

        elif c1 == c2:  
            plain += matrix[(r1 - 1) % 5][c1]
            plain += matrix[(r2 - 1) % 5][c2]

        else: 
            plain += matrix[r1][c2]
            plain += matrix[r2][c1]

    return plain

key = input("Enter the key: ")
matrix = generate_matrix(key)
print("\nPlayfair Matrix:")
for row in matrix:
    print(" ".join(row))

plain = input("\nEnter the plain text: ")

prepared_text = preprocess_text(plain)
cipher_text = encrypt(prepared_text, matrix)
decrypted_text = decrypt(cipher_text, matrix)

print("\nPrepared Text :", prepared_text)
print("Encrypted Text:", cipher_text)
print("Decrypted Text:", decrypted_text)
```

## Output:

<img width="667" height="492" alt="image" src="https://github.com/user-attachments/assets/f58aedc3-8aaa-4ef2-a5b2-78ad46903e8d" />

## Result:

Thus the program to implement Playfair using python is done successfully.
