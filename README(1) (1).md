# Password Strength Checker

A Python command-line project that evaluates password security using **entropy calculation**, **password policy checks**, and a small **common-password/dictionary leak check**.

## Objective

Build a password security validation program that:
- Checks password policy compliance.
- Calculates estimated entropy in bits.
- Checks whether the password matches a known common-password list.
- Classifies the password as **Weak, Moderate, Strong, or Exceptional**.
- Gives actionable feedback to improve password security.

## Features

1. Minimum length validation (8 characters).
2. Uppercase and lowercase character checks.
3. Digit and special-character checks.
4. Entropy calculation using:

```text
Entropy = password length × log2(character pool size)
```

5. Common/dictionary password detection.
6. Strength classification and user feedback.

> **Note:** The dictionary check in this project is a small demonstration list, not a complete breach database. Entropy is an estimate based on character-set size and does not prove that a password is safe.

## Source Code

Save the following as `password_strength_checker.py`:

```python
import math
import string

COMMON_PASSWORDS = {
    "password", "password123", "123456", "12345678",
    "qwerty", "qwerty123", "admin", "admin123",
    "welcome", "letmein", "iloveyou"
}

def calculate_entropy(password):
    pool = 0
    if any(c.islower() for c in password):
        pool += 26
    if any(c.isupper() for c in password):
        pool += 26
    if any(c.isdigit() for c in password):
        pool += 10
    if any(c in string.punctuation for c in password):
        pool += len(string.punctuation)

    if pool == 0:
        return 0.0
    return len(password) * math.log2(pool)

def check_policy(password):
    return {
        "Minimum length (8)": len(password) >= 8,
        "Uppercase letter": any(c.isupper() for c in password),
        "Lowercase letter": any(c.islower() for c in password),
        "Digit": any(c.isdigit() for c in password),
        "Special character": any(c in string.punctuation for c in password)
    }

def classify_strength(password, entropy, leaked):
    if leaked:
        return "Weak"
    if len(password) < 8:
        return "Weak"
    if entropy < 40:
        return "Moderate"
    if entropy < 60:
        return "Strong"
    return "Exceptional"

def main():
    password = input("Enter password: ")

    policy = check_policy(password)
    entropy = calculate_entropy(password)
    leaked = password.lower() in COMMON_PASSWORDS
    strength = classify_strength(password, entropy, leaked)

    print("\n--- Password Strength Report ---")
    print(f"Length: {len(password)}")
    print(f"Entropy: {entropy:.2f} bits")
    print(f"Known dictionary leak: {'Yes' if leaked else 'No'}")
    print("\nPolicy compliance:")
    for rule, passed in policy.items():
        print(f"  [{'PASS' if passed else 'FAIL'}] {rule}")

    print(f"\nStrength: {strength}")

    if strength in ("Weak", "Moderate"):
        print("Feedback: Use a longer password with uppercase, lowercase, digits,")
        print("          and special characters. Avoid common or leaked passwords.")
    elif strength == "Strong":
        print("Feedback: Good password. Consider making it longer for extra security.")
    else:
        print("Feedback: Excellent password strength. Keep it unique and private.")

if __name__ == "__main__":
    main()
```

## How to Run

Make sure Python 3 is installed, then run:

```bash
python password_strength_checker.py
```

Enter a password when prompted.

## Command-Line Output Screenshots

### 1. Weak Password / Dictionary Match

![Weak password output](weak_output.png)

### 2. Strong Password

![Strong password output](strong_output.png)

### 3. Exceptional Password

![Exceptional password output](exceptional_output.png)

## Example Classification

| Strength | General condition |
|---|---|
| Weak | Short password or common/dictionary match |
| Moderate | Meets basic length but has lower estimated entropy |
| Strong | Higher estimated entropy |
| Exceptional | Very high estimated entropy |

## Security Notes

- Never use real passwords in screenshots, GitHub repositories, or public demonstrations.
- Do not store passwords in source code.
- The program does not send passwords to the internet.
- For real-world security, use established password-strength libraries and breach-password services rather than a small local list.

## Project Deliverables

- `password_strength_checker.py` — source code
- `weak_output.png` — command-line output proof
- `strong_output.png` — command-line output proof
- `exceptional_output.png` — command-line output proof

## Technologies

- Python 3
- `math`
- `string`

## Author

**Sai Nikitha**
