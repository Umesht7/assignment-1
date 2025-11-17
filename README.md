# general_programming_logic.py
from typing import List, Tuple

# ----------------------------
# Basic arithmetic / comparisons
# ----------------------------

def add_two_numbers(a: float, b: float) -> float:
    """(1) Return sum of two numbers."""
    return a + b

def max_of_two(a: float, b: float) -> float:
    """(2) Return the bigger of two numbers."""
    return a if a >= b else b

def is_even(number: int) -> bool:
    """(3) Return True if number is even, else False."""
    return number % 2 == 0

# ----------------------------
# Grades
# ----------------------------

def grade_from_three_marks(mark1: float, mark2: float, mark3: float) -> str:
    """
    (4) Determine grade based on average of 3 subject marks.
    Returns one of: 'A+', 'A', 'A-', 'B+', 'B', 'FAIL'
    """
    average = (mark1 + mark2 + mark3) / 3.0
    # grading logic as specified
    if average > 90:
        return "A+"
    if 80 <= average <= 90:
        return "A"
    if 70 <= average < 80:
        return "A-"
    if 60 <= average < 70:
        return "B+"
    if 50 <= average < 60:
        return "B"
    return "FAIL"

# ----------------------------
# Sequences and tables
# ----------------------------

def first_n_natural(n: int = 10) -> List[int]:
    """(5) Return first n natural numbers (1..n)."""
    return list(range(1, n + 1))

def first_n_natural_reverse(n: int = 10) -> List[int]:
    """(6) Return first n natural numbers in reverse order (n..1)."""
    return list(range(n, 0, -1))

def first_n_even_natural(n: int = 10) -> List[int]:
    """(7) Return first n even natural numbers (2,4,...)."""
    return [2 * i for i in range(1, n + 1)]

def numbers_between_range(start: int, end: int) -> List[int]:
    """
    (8) Return numbers between the given range inclusive.
    Works for start <= end or start > end (will still return in logical order).
    """
    if start <= end:
        return list(range(start, end + 1))
    # if start > end, return descending range to match natural expectation.
    return list(range(start, end - 1, -1))

def multiplication_table(number: int, upto: int = 10) -> List[int]:
    """
    (9) Return mathematical table (multiples) of 'number' up to 'upto' entries.
    Example: multiplication_table(5, 10) -> [5,10,15,...,50]
    """
    return [number * i for i in range(1, upto + 1)]

# ----------------------------
# Primes
# ----------------------------

def is_prime(number: int) -> bool:
    """(10) Return True if number is prime (>=2), else False."""
    if number <= 1:
        return False
    if number <= 3:
        return True
    if number % 2 == 0:
        return False
    # test odd divisors up to sqrt(n)
    i = 3
    while i * i <= number:
        if number % i == 0:
            return False
        i += 2
    return True

def primes_in_range(start: int = 2, end: int = 100) -> List[int]:
    """(11) Return list of primes between start and end inclusive."""
    if end < 2:
        return []
    start = max(2, start)
    primes = []
    for num in range(start, end + 1):
        if is_prime(num):
            primes.append(num)
    return primes

# ----------------------------
# Digit operations: sum, reverse, palindrome, lucky number
# ----------------------------

def sum_of_digits(number: int) -> int:
    """(12 & 18) Return sum of individual digits of given number (handles negative)."""
    n = abs(number)
    total = 0
    while n:
        total += n % 10
        n //= 10
    return total

def lucky_number(number: int) -> int:
    """
    (13) Compute the 'lucky number' by repeatedly summing digits until a single digit.
    Example: 12345 -> 1+2+3+4+5 = 15 -> 1+5 = 6 -> returns 6
    """
    n = abs(number)
    while n >= 10:
        n = sum_of_digits(n)
    return n

def reverse_number(number: int) -> int:
    """(14 & 19) Return the integer obtained by reversing digits (preserve sign)."""
    sign = -1 if number < 0 else 1
    n = abs(number)
    rev = 0
    while n:
        rev = rev * 10 + (n % 10)
        n //= 10
    return sign * rev

def is_palindrome_number(number: int) -> bool:
    """(15 & 20) Return True if the given number is a palindrome (ignoring sign)."""
    n = abs(number)
    return n == reverse_number(n)

# ----------------------------
# Factorial and nCr
# ----------------------------

def factorial(n: int) -> int:
    """(16) Return factorial of n (0! = 1). Raises ValueError for negative input."""
    if n < 0:
        raise ValueError("Factorial is not defined for negative numbers.")
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

def nCr(n: int, r: int) -> int:
    """
    (17) Return n choose r using factorial formula.
    Uses symmetry to reduce product size and avoid large factorials when possible.
    Raises ValueError for invalid ranges.
    """
    if r < 0 or n < 0 or r > n:
        raise ValueError("Invalid n and r for nCr.")
    # use direct multiplicative approach for better numerical stability
    r = min(r, n - r)
    if r == 0:
        return 1
    numerator = 1
    denominator = 1
    for i in range(1, r + 1):
        numerator *= (n - r + i)
        denominator *= i
    return numerator // denominator

# ----------------------------
# Digit -> word single-digit and full number -> words
# ----------------------------

_DIGIT_WORDS = {
    0: "Zero", 1: "One", 2: "Two", 3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
}

_TENS_WORDS = {
    10: "Ten", 11: "Eleven", 12: "Twelve", 13: "Thirteen", 14: "Fourteen",
    15: "Fifteen", 16: "Sixteen", 17: "Seventeen", 18: "Eighteen", 19: "Nineteen",
    20: "Twenty", 30: "Thirty", 40: "Forty", 50: "Fifty",
    60: "Sixty", 70: "Seventy", 80: "Eighty", 90: "Ninety"
}

def digit_to_word(digit: int) -> str:
    """(21) Convert a single digit (0-9) to its word. Raises ValueError for invalid input."""
    if digit not in _DIGIT_WORDS:
        raise ValueError("Input must be a single digit 0..9")
    return _DIGIT_WORDS[digit]

def _two_digit_to_words(n: int) -> str:
    """Helper: convert 0 <= n < 100 to words."""
    if n < 10:
        return _DIGIT_WORDS[n]
    if 10 <= n < 20:
        return _TENS_WORDS[n]
    tens = (n // 10) * 10
    ones = n % 10
    if ones == 0:
        return _TENS_WORDS[tens]
    return _TENS_WORDS[tens] + " " + _DIGIT_WORDS[ones]

def number_to_words(number: int) -> str:
    """
    (22) Convert a non-negative integer into words.
    Example: 123 -> "One Hundred Twenty Three"
    Supports 0 <= number < 1_000_000_000 (up to hundreds of millions).
    For negative numbers, returns 'Minus ...'.
    """
    if number == 0:
        return "Zero"
    if number < 0:
        return "Minus " + number_to_words(-number)

    parts = []

    def _hundreds_chunk_to_words(chunk: int) -> str:
        # chunk is 0..999
        words = []
        if chunk >= 100:
            h = chunk // 100
            words.append(_DIGIT_WORDS[h] + " Hundred")
            chunk = chunk % 100
        if chunk > 0:
            words.append(_two_digit_to_words(chunk))
        return " ".join(words)

    billions = number // 1_000_000_000
    number %= 1_000_000_000
    millions = number // 1_000_000
    number %= 1_000_000
    thousands = number // 1_000
    hundreds = number % 1_000

    if billions > 0:
        parts.append(_hundreds_chunk_to_words(billions) + " Billion")
    if millions > 0:
        parts.append(_hundreds_chunk_to_words(millions) + " Million")
    if thousands > 0:
        parts.append(_hundreds_chunk_to_words(thousands) + " Thousand")
    if hundreds > 0:
        parts.append(_hundreds_chunk_to_words(hundreds))

    return " ".join(parts)

# ----------------------------
# Demo / quick tests (allowed to print here)
# ----------------------------

if __name__ == "__main__":
    # Quick manual checks (prints only here; all functions above are print-free)
    print("add_two_numbers(3,4) ->", add_two_numbers(3, 4))
    print("max_of_two(10,7) ->", max_of_two(10, 7))
    print("is_even(11) ->", is_even(11))
    print("grade_from_three_marks(92,95,89) ->", grade_from_three_marks(92, 95, 89))
    print("first_n_natural() ->", first_n_natural())
    print("first_n_natural_reverse() ->", first_n_natural_reverse())
    print("first_n_even_natural() ->", first_n_even_natural())
    print("numbers_between_range(10, 15) ->", numbers_between_range(10, 15))
    print("multiplication_table(7) ->", multiplication_table(7))
    print("is_prime(97) ->", is_prime(97))
    print("primes_in_range(2,100) ->", primes_in_range(2, 100))
    print("sum_of_digits(123) ->", sum_of_digits(123))
    print("lucky_number(12345) ->", lucky_number(12345))
    print("reverse_number(-123) ->"_
