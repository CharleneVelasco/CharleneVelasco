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