def arithmetic_operations(num1, num2):
    """
    Perform arithmetic operations on two numbers.

    Args:
        num1 (int/float/complex): The first number.
        num2 (int/float/complex): The second number.

    Returns:
        dict: A dictionary containing the results of the arithmetic operations.
    """
    results = {
        "Addition": num1 + num2,
        "Subtraction": num1 - num2,
        "Multiplication": num1 * num2,
        "Division": num1 / num2 if num2 != 0 else "Error: Division by zero",
        "Modulus": num1 % num2 if num2 != 0 else "Error: Division by zero",
        "Exponent": num1 ** num2
    }
    return results

# Test the function with int, float, and complex numbers
numbers = [
    (10, 2),  # int
    (10.5, 2.5),  # float
    (1 + 2j, 3 + 4j)  # complex
]

for num1, num2 in numbers:
    print(f"Arithmetic Operations for {num1} and {num2}:")
    results = arithmetic_operations(num1, num2)
    for operation, result in results.items():
        print(f"{operation}: {result}")
    print()

# Create two variables, num1 and num2, with the same integer value
num1 = 25_000_000  # with underscores
num2 = 25000000  # without underscores

# Print num1 and num2 on separate lines
print(num1)
print(num2)



# Create variables of type int, float, and complex
int_var = 10
float_var = 10.5
complex_var = 1 + 2j

# Check the types of the variables
print("Type of int_var:", type(int_var))
print("Type of float_var:", type(float_var))
print("Type of complex_var:", type(complex_var))