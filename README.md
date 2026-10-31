# MatrixLab

##THEORY
Matrix Lab is a menu-driven console application developed in Python. It performs addition, subtraction, multiplication, scalar multiplication, transpose, determinant, adjoint and inverse of matrices, checks whether a matrix is symmetric or skew-symmetric, and solves a system of linear equations by the matrix method (X = A⁻¹B). All results are computed exactly in fractions.


## PROGRAM
'''# MATRIX LAB - Matrix Calculator (Class 12 CBSE Computer Science Project)
# Concepts used: lists, loops, functions, recursion and the fractions module.
# A matrix is stored as a list of rows, e.g. [[1, 2], [3, 4]].

from fractions import Fraction

# ----- INPUT AND DISPLAY -----

def get_number(message):
    """Ask until a valid number is typed (examples: 5, -2, 0.5, 3/4)."""
    while True:
        try:
            return Fraction(input(message))
        except (ValueError, ZeroDivisionError):
            print("Invalid number. Please try again.")

def get_size(message):
    """Ask until a whole number from 1 to 5 is typed."""
    while True:
        try:
            n = int(input(message))
            if 1 <= n <= 5:
                return n
        except ValueError:
            pass
        print("Please enter a whole number from 1 to 5.")

def read_matrix(name, square=False):
    """Read a matrix from the user, one element at a time."""
    rows = get_size("Number of rows of " + name + ": ")
    if square:
        cols = rows
    else:
        cols = get_size("Number of columns of " + name + ": ")
    matrix = []
    for i in range(rows):
        row = []
        for j in range(cols):
            row.append(get_number(name + "[" + str(i + 1) + "][" + str(j + 1) + "] = "))
        matrix.append(row)
    return matrix

def show_matrix(matrix):
    """Print a matrix row by row."""
    for row in matrix:
        line = ""
        for value in row:
            line = line + str(value).rjust(8)
        print(line)

# ----- BASIC OPERATIONS -----

def add_or_subtract(a, b, sign):
    """sign = 1 gives A + B and sign = -1 gives A - B."""
    result = []
    for i in range(len(a)):
        row = []
        for j in range(len(a[0])):
            row.append(a[i][j] + sign * b[i][j])
        result.append(row)
    return result

def multiply(a, b):
    """Row-by-column multiplication of A and B."""
    result = []
    for i in range(len(a)):
        row = []
        for j in range(len(b[0])):
            total = 0
            for k in range(len(b)):
                total = total + a[i][k] * b[k][j]
            row.append(total)
        result.append(row)
    return result

def scalar_multiply(k, a):
    """Multiply every element of A by the number k."""
    result = []
    for row in a:
        new_row = []
        for value in row:
            new_row.append(k * value)
        result.append(new_row)
    return result

def transpose(a):
    """Interchange the rows and columns of A."""
    result = []
    for j in range(len(a[0])):
        row = []
        for i in range(len(a)):
            row.append(a[i][j])
        result.append(row)
    return result

# ----- DETERMINANT, ADJOINT AND INVERSE -----

def submatrix(a, row, col):
    """Delete one row and one column of A (used to find a minor)."""
    result = []
    for i in range(len(a)):
        if i != row:
            new_row = []
            for j in range(len(a)):
                if j != col:
                    new_row.append(a[i][j])
            result.append(new_row)
    return result

def determinant(a):
    """Expand along the first row (the function calls itself for smaller matrices)."""
    n = len(a)
    if n == 1:
        return a[0][0]
    if n == 2:
        return a[0][0] * a[1][1] - a[0][1] * a[1][0]
    total = 0
    for j in range(n):
        cofactor = (-1) ** j * determinant(submatrix(a, 0, j))
        total = total + a[0][j] * cofactor
    return total

def cofactor_matrix(a):
    """Cofactor Cij = (-1)^(i+j) x Minor Mij."""
    n = len(a)
    if n == 1:
        return [[Fraction(1)]]
    result = []
    for i in range(n):
        row = []
        for j in range(n):
            row.append((-1) ** (i + j) * determinant(submatrix(a, i, j)))
        result.append(row)
    return result

def adjoint(a):
    """Adjoint = transpose of the cofactor matrix."""
    return transpose(cofactor_matrix(a))

def inverse(a):
    """Inverse = (1 / |A|) x adj A. Returns None if |A| = 0."""
    d = determinant(a)
    if d == 0:
        return None
    return scalar_multiply(1 / d, adjoint(a))

# ----- EXTRA FEATURES -----

def check_type(a):
    """Check whether a square matrix is symmetric or skew-symmetric."""
    if a == transpose(a):
        print("A' = A, so the matrix is SYMMETRIC.")
    elif a == scalar_multiply(-1, transpose(a)):
        print("A' = -A, so the matrix is SKEW-SYMMETRIC.")
    else:
        print("The matrix is neither symmetric nor skew-symmetric.")

def solve_equations():
    """Solve AX = B using X = (A inverse) B."""
    print("Coefficient matrix A (rows = equations, columns = x, y, z ...):")
    a = read_matrix("A", True)
    n = len(a)
    b = []
    for i in range(n):
        b.append([get_number("Constant on the right of equation " + str(i + 1) + ": ")])
    d = determinant(a)
    print("|A| =", d)
    if d == 0:
        print("|A| = 0, so a unique solution does not exist.")
        return
    x = multiply(inverse(a), b)
    print("Solution X = (A inverse) B:")
    for i in range(n):
        print("  variable", i + 1, "=", x[i][0])

# ----- MENU AND MAIN PROGRAM -----

def show_menu():
    print()
    print("1.  Add two matrices            7.  Adjoint of a matrix")
    print("2.  Subtract two matrices       8.  Inverse of a matrix")
    print("3.  Multiply two matrices       9.  Symmetric / skew-symmetric check")
    print("4.  Multiply by a number (k)    10. Solve linear equations (AX = B)")
    print("5.  Transpose of a matrix       0.  Exit")
    print("6.  Determinant of a matrix")

def main():
    print("=" * 50)
    print("                    MATRIX LAB")
    print("=" * 50)
    while True:
        show_menu()
        choice = input("Enter your choice: ")
        if choice == "0":
            print("Thank you for using Matrix Lab.")
            break
        elif choice in ("1", "2", "3"):
            a = read_matrix("A")
            b = read_matrix("B")
            if choice == "3":
                if len(a[0]) != len(b):
                    print("Not possible: columns of A must equal rows of B.")
                else:
                    print("A x B =")
                    show_matrix(multiply(a, b))
            elif len(a) != len(b) or len(a[0]) != len(b[0]):
                print("Not possible: both matrices must have the same order.")
            elif choice == "1":
                print("A + B =")
                show_matrix(add_or_subtract(a, b, 1))
            else:
                print("A - B =")
                show_matrix(add_or_subtract(a, b, -1))
        elif choice == "4":
            k = get_number("Value of k: ")
            a = read_matrix("A")
            print("kA =")
            show_matrix(scalar_multiply(k, a))
        elif choice == "5":
            a = read_matrix("A")
            print("Transpose of A =")
            show_matrix(transpose(a))
        elif choice in ("6", "7", "8", "9"):
            a = read_matrix("A", True)
            if choice == "6":
                print("|A| =", determinant(a))
            elif choice == "7":
                print("adj A =")
                show_matrix(adjoint(a))
            elif choice == "8":
                print("|A| =", determinant(a))
                if determinant(a) == 0:
                    print("A is a singular matrix, so its inverse does not exist.")
                else:
                    print("adj A =")
                    show_matrix(adjoint(a))
                    print("A inverse = (1/|A|) x adj A =")
                    show_matrix(inverse(a))
            else:
                check_type(a)
        elif choice == "10":
            solve_equations()
        else:
            print("Invalid choice. Please select from the menu.")

main()'''
