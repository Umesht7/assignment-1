"""
general_programming_logic.py

Solutions to the assignment "General programming logic".
- Each numbered problem from the assignment is implemented as a separate function.
- Functions do not call input() or print() internally (they return values).
- Small demo block under __main__ shows example usage (safe to remove before submission).
"""

from typing import List

# 1. Add two numbers
def add_two_numbers(a: float, b: float) -> float:
    """Return the sum of a and b."""
    return a + b

# 2. Biggest between 2 numbers
def max_of_two(a: float, b: float) -> float:
    """Return the larger of a and b."""
    return a if a >= b else b

# 3. Is even
def is_even(number: int) -> bool:
    """Return True if number is even, else False."""
    return number % 2 == 0

# 4. Grade of 3 subject marks (based on average)
def grade_from_three_marks(mark1: float, mark2: float, mark3: float) -> str:
    """
    Return grade string based on average:
    >90 -> 'A+'
    80-90 -> 'A'
    70-79.999.. -> 'A-'
    60-69.999.. -> 'B+'
    50-59.999.. -> 'B'
    <50 -> 'FAIL'
    """
    average = (mark1 + mark2 + mark3) / 3.0
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

# 5. First n natural numbers (default 10)
def first_n_natural(n: int = 10) -> List[int]:
    """Return the list [1, 2, ..., n]."""
    return list(range(1, n + 1))

# 6. First n natural numbers in reverse order (default 10)
def first_n_natural_reverse(n: int = 10) -> List[int]:
    """Return the list [n, n-1, ..., 1]."""
    return list(range(n, 0, -1))

# 7. First n even natural numbers
def first_n_even_natural(n: int = 10) -> List[int]:
    """Return first n even numbers [2,4,6,...]."""
    return [2 * i for i in range(1, n + 1)]

# 8. Numbers between a range (inclusive)
def numbers_between_range(start: int, end: int) -> List[int]:
    """
    Return list of numbers between start and end inclusive.
    If start <= end: ascending order.
    If start > end: descending order.
    """
    if start <= end:
        return list(range(start, end + 1))
    return list(range(start, end - 1, -1))

# 9. Mathematical table (multiplication table) of given number
def multiplication_table(number: int, upto: int = 10) -> List[int]:
    """Return the multiplication table of `number` up to `upto` (1..upto)."""
    return [number * i for i in range(1, upto + 1)]

# 10. Check prime
def is_prime(number: int) -> bool:
    """Return True if number is prime (number >= 2)."""
    if number <= 1:
        return False
    if number <= 3:
        return True
    if number % 2 == 0:
        return False
    i = 3
    while i * i <= number:
        if number % i == 0:
            return False
        i += 2
    return True

# 11. Prime numbers between 2 and 100 (or general start/end)
def primes_in_range(start: int = 2, end: int = 100) -> List[int]:
    """Return list of primes between start and end inclusive."""
    start = max(start, 2)
    primes = []
    for n in range(start, end + 1):
        if is_prime(n):
            primes.append(n)
    return primes

# 12 & 18. Sum of individual digits of given number
def sum_of_digits(number: int) -> int:
    """Return the sum of digits of absolute(number)."""
    n = abs(number)
    total = 0
    while n > 0:
        total += n % 10
        n //= 10
    return total

# 13. Lucky number (reduce digits until single digit)
def lucky_number(number: int) -> int:
    """
    Compute repeated digit sum until a single digit (digital root).
    Example: 12345 -> 1+2+3+4+5=15 -> 1+5=6 -> returns 6
    """
    n = abs(number)
    while n >= 10:
        n = sum_of_digits(n)
    return n

# 14 & 19. Reverse of given number
def reverse_number(number: int) -> int:
    """
    Return the integer that is digits reversed.
    Keeps the sign of the original number.
    """
    sign = -1 if number < 0 else 1
    n = abs(number)
    rev = 0
    while n > 0:
        rev = rev * 10 + n % 10
        n //= 10
    return sign * rev

# 15 & 20. Check palindrome number
def is_palindrome_number(number: int) -> bool:
    """Return True if the absolute value of number reads the same reversed."""
    n = abs(number)
    return n == reverse_number(n)

# 16. Factorial
def factorial(n: int) -> int:
    """Return n! for n >= 0. Raise ValueError for negative inputs."""
    if n < 0:
        raise ValueError("Factorial undefined for negative numbers")
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

# 17. nCr using multiplicative method
def nCr(n: int, r: int) -> int:
    """
    Return n choose r (combination) = n! / (r! * (n-r)!)
    Uses multiplicative method to avoid huge intermediate factorials.
    """
    if r < 0 or n < 0 or r > n:
        raise ValueError("Invalid n and r for nCr")
    r = min(r, n - r)
    if r == 0:
        return 1
    numerator = 1
    denominator = 1
    for i in range(1, r + 1):
        numerator *= (n - r + i)
        denominator *= i
    return numerator // denominator

# 21. Convert single digit to word
_DIGIT_WORDS = {
    0: "Zero", 1: "One", 2: "Two", 3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
}

def digit_to_word(digit: int) -> str:
    """Return the English word for a single digit 0..9. Raises ValueError otherwise."""
    if digit not in _DIGIT_WORDS:
        raise ValueError("Input must be a single digit (0-9).")
    return _DIGIT_WORDS[digit]

# 22. Convert number to words (supports up to billions)
_TENS_WORDS = {
    10: "Ten", 11: "Eleven", 12: "Twelve", 13: "Thirteen", 14: "Fourteen",
    15: "Fifteen", 16: "Sixteen", 17: "Seventeen", 18: "Eighteen", 19: "Nineteen",
    20: "Twenty", 30: "Thirty", 40: "Forty", 50: "Fifty",
    60: "Sixty", 70: "Seventy", 80: "Eighty", 90: "Ninety"
}

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
    Convert integer to English words. Handles 0 and negatives.
    Example: 123 -> "One Hundred Twenty Three"
    Supports up to billions (tested for numbers < 1_000_000_000_000).
    """
    if number == 0:
        return "Zero"
    if number < 0:
        return "Minus " + number_to_words(-number)

    def _hundreds_chunk_to_words(chunk: int) -> str:
        # chunk: 0..999
        words = []
        if chunk >= 100:
            hundreds = chunk // 100
            words.append(_DIGIT_WORDS[hundreds] + " Hundred")
            chunk %= 100
        if chunk > 0:
            words.append(_two_digit_to_words(chunk))
        return " ".join(words)

    parts = []
    trillion = number // 1_000_000_000_000
    number %= 1_000_000_000_000
    billion = number // 1_000_000_000
    number %= 1_000_000_000
    million = number // 1_000_000
    number %= 1_000_000
    thousand = number // 1_000
    remainder = number % 1_000

    if trillion:
        parts.append(_hundreds_chunk_to_words(trillion) + " Trillion")
    if billion:
        parts.append(_hundreds_chunk_to_words(billion) + " Billion")
    if million:
        parts.append(_hundreds_chunk_to_words(million) + " Million")
    if thousand:
        parts.append(_hundreds_chunk_to_words(thousand) + " Thousand")
    if remainder:
        parts.append(_hundreds_chunk_to_words(remainder))

    return " ".join(parts)


# If run directly, this block shows a small demonstration. Remove or edit before final submission if required.
if __name__ == "__main__":
    # Quick checks / example outputs
    print("1) add_two_numbers(3,4):", add_two_numbers(3, 4))
    print("2) max_of_two(7,10):", max_of_two(7, 10))
    print("3) is_even(8):", is_even(8))
    print("4) grade_from_three_marks(95, 88, 92):", grade_from_three_marks(95, 88, 92))
    print("5) first_n_natural():", first_n_natural())
    print("6) first_n_natural_reverse():", first_n_natural_reverse())
    print("7) first_n_even_natural():", first_n_even_natural())
    print("8) numbers_between_range(10, 15):", numbers_between_range(10, 15))
    print("9) multiplication_table(5):", multiplication_table(5))
    print("10) is_prime(97):", is_prime(97))
    print("11) primes_in_range(2,100):", primes_in_range(2, 100))
    print("12) sum_of_digits(123):", sum_of_digits(123))
    print("13) lucky_number(12345):", lucky_number(12345))
    print("14) reverse_number(-123):", reverse_number(-123))
    print("15) is_palindrome_number(121):", is_palindrome_number(121))
    print("16) factorial(6):", factorial(6))
    print("17) nCr(5,2):", nCr(5, 2))
    print("18) sum_of_digits(1001):", sum_of_digits(1001))
    print("19) reverse_number(1200):", reverse_number(1200))
    print("20) is_palindrome_number(1221):", is_palindrome_number(1221))
    print("21) digit_to_word(4):", digit_to_word(4))
    print("22) number_to_words(123):", number_to_words(123))

