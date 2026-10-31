# MatrixLab

##THEORY
Matrix Lab is a menu-driven console application developed in Python. It performs addition, subtraction, multiplication, scalar multiplication, transpose, determinant, adjoint and inverse of matrices, checks whether a matrix is symmetric or skew-symmetric, and solves a system of linear equations by the matrix method (X = A⁻¹B). All results are computed exactly in fractions.


## PROGRAM
# MATRIX LAB - Matrix Calculator (Class 12 CBSE Computer Science Project)
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
