
### **✨ Java Code Snippets Collection  ✨**

A clean, well-formatted collection of common Java problems and solutions for quick reference and practice.

---

### **📋 Features**
- 🔢 Check if a number is prime.
- 🔄 Reverse a number's digits.
- 💪 Check if a number is an Armstrong number.
- ✖️ Calculate the factorial of a number.
- ↔️ Check if a string is a palindrome.
- 🔤 Count vowels and consonants in a string.
- 🗑️ Remove duplicate characters from a string.
- 📈 Find max and min elements in an array.
- 👯 Find duplicates in an array.
- 🛁 Sort an array using bubble sort.
- 🤝 Merge and sort two arrays.
- 🥈 Find the second largest element in an array.

---

### **💻 Code Snippets**

#### 🔢 **Check Prime Number**
```java
// Checks if a number is prime (optimized for efficiency)
public static boolean isPrime(int num) {
    if (num <= 1) return false; // Numbers <= 1 are not prime
    if (num == 2) return true;  // 2 is prime
    if (num % 2 == 0) return false; // Even numbers > 2 are not prime
    // Check odd divisors up to square root of num
    for (int i = 3; i <= Math.sqrt(num); i += 2) {
        if (num % i == 0) return false;
    }
    return true;
}

public static void main(String[] args) {
    int num = 113;
    System.out.println(num + (isPrime(num) ? " is Prime" : " is Not Prime"));
}
```

#### 🔄 **Reverse a Number**
```java
// Reverses the digits of a number
public static int reverseNumber(int num) {
    int reversed = 0;
    while (num > 0) {
        reversed = reversed * 10 + num % 10; // Extract last digit and build reversed number
        num /= 10; // Remove last digit
    }
    return reversed;
}

public static void main(String[] args) {
    int num = 12345;
    System.out.println("Reversed: " + reverseNumber(num));
}
```

#### 💪 **Check Armstrong Number**
```java
// Checks if a number is an Armstrong number (sum of digits raised to power of digit count equals the number)
public static boolean isArmstrong(int num) {
    int digitCount = (int) Math.log10(num) + 1; // Count digits
    int sum = 0, temp = num;
    while (temp > 0) {
        int digit = temp % 10;
        sum += Math.pow(digit, digitCount); // Raise digit to power of digit count
        temp /= 10;
    }
    return sum == num;
}

public static void main(String[] args) {
    int num = 153;
    System.out.println(num + (isArmstrong(num) ? " is an Armstrong number" : " is not an Armstrong number"));
}
```

#### ✖️ **Factorial of a Number**
```java
// Calculates factorial of a number
public static long factorial(int num) {
    if (num < 0) return -1; // Invalid input
    long fact = 1;
    for (int i = 1; i <= num; i++) {
        fact *= i;
    }
    return fact;
}

public static void main(String[] args) {
    int num = 5;
    System.out.println("Factorial of " + num + ": " + factorial(num));
}
```

#### ↔️ **Palindrome String Check**
```java
// Checks if a string is a palindrome
public static boolean isPalindrome(String str) {
    str = str.toLowerCase(); // Case-insensitive comparison
    int left = 0, right = str.length() - 1;
    while (left < right) {
        if (str.charAt(left++) != str.charAt(right--)) {
            return false;
        }
    }
    return true;
}

public static void main(String[] args) {
    String str = "madam";
    System.out.println(str + (isPalindrome(str) ? " is a Palindrome" : " is not a Palindrome"));
}
```

#### 🔤 **Vowel and Consonant Counter**
```java
// Counts vowels and consonants in a string
public static void countVowelsAndConsonants(String str) {
    str = str.toLowerCase();
    int vowels = 0, consonants = 0;
    for (char ch : str.toCharArray()) {
        if (ch >= 'a' && ch <= 'z') {
            if ("aeiou".indexOf(ch) != -1) {
                vowels++;
            } else {
                consonants++;
            }
        }
    }
    System.out.println("Vowels: " + vowels + ", Consonants: " + consonants);
}

public static void main(String[] args) {
    String str = "Hello Java";
    countVowelsAndConsonants(str);
}
```

#### 🗑️ **Remove Duplicate Characters from String**
```java
// Removes duplicate characters from a string
public static String removeDuplicates(String str) {
    StringBuilder result = new StringBuilder();
    for (char ch : str.toCharArray()) {
        if (result.indexOf(String.valueOf(ch)) == -1) {
            result.append(ch);
        }
    }
    return result.toString();
}

public static void main(String[] args) {
    String str = "programming";
    System.out.println("Unique characters: " + removeDuplicates(str));
}
```

#### 📈 **Max and Min in Array**
```java
// Finds maximum and minimum elements in an array
public static void findMaxMin(int[] arr) {
    if (arr.length == 0) {
        System.out.println("Array is empty");
        return;
    }
    int max = arr[0], min = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) max = arr[i];
        if (arr[i] < min) min = arr[i];
    }
    System.out.println("Max: " + max + ", Min: " + min);
}

public static void main(String[] args) {
    int[] arr = {12, 5, 8, 20, 1};
    findMaxMin(arr);
}
```

#### 👯 **Find Duplicates in Array**
```java
// Finds duplicate elements in an array using a HashSet for efficiency
public static void findDuplicates(int[] arr) {
    Set<Integer> seen = new HashSet<>();
    Set<Integer> duplicates = new HashSet<>();
    for (int num : arr) {
        if (!seen.add(num)) {
            duplicates.add(num);
        }
    }
    System.out.print("Duplicates: ");
    for (int num : duplicates) {
        System.out.print(num + " ");
    }
    if (duplicates.isEmpty()) System.out.print("None");
    System.out.println();
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 2, 4, 3};
    findDuplicates(arr);
}
```

#### 🛁 **Bubble Sort**
```java
// Sorts an array using bubble sort
public static void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap elements
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

public static void main(String[] args) {
    int[] arr = {5, 2, 8, 1, 3};
    bubbleSort(arr);
    System.out.print("Sorted: ");
    for (int num : arr) {
        System.out.print(num + " ");
    }
    System.out.println();
}
```

#### 🤝 **Merge and Sort Two Arrays**
```java
// Merges two arrays and sorts the result
public static int[] mergeAndSort(int[] a, int[] b) {
    int[] merged = new int[a.length + b.length];
    System.arraycopy(a, 0, merged, 0, a.length);
    System.arraycopy(b, 0, merged, a.length, b.length);
    Arrays.sort(merged); // Use built-in sort for efficiency
    return merged;
}

public static void main(String[] args) {
    int[] a = {1, 2, 3}, b = {4, 5, 3};
    int[] result = mergeAndSort(a, b);
    System.out.print("Merged and Sorted: ");
    for (int num : result) {
        System.out.print(num + " ");
    }
    System.out.println();
}
```

#### 🥈 **Second Largest Element in Array**
```java
// Finds the second largest element in an array
public static int findSecondLargest(int[] arr) {
    if (arr.length < 2) return Integer.MIN_VALUE; // Not enough elements
    int max = Integer.MIN_VALUE, second = Integer.MIN_VALUE;
    for (int num : arr) {
        if (num > max) {
            second = max;
            max = num;
        } else if (num > second && num != max) {
            second = num;
        }
    }
    return second;
}

public static void main(String[] args) {
    int[] arr = {10, 20, 5, 15, 25};
    int secondLargest = findSecondLargest(arr);
    System.out.println("Second Largest: " + (secondLargest == Integer.MIN_VALUE ? "Not found" : secondLargest));
}
```

---
These snippets are now easier to read, maintain, and reuse, making them ideal for learning and quick reference. Let me know if you need further clarification or additional snippets! 🚀
