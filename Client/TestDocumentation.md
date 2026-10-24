# Test Documentation

## 1. Unit Tests

**Package:** `utility`

**Tested Class:** `Keyboard`

### Test 1 (`testReadIntConInputNonValido`)

**Purpose:** Verifies the defensive behavior of `readInt()` when given non-numeric input, ensuring that the `NumberFormatException` is handled internally without propagating to the caller.

| Field | Content |
| --- | --- |
| Input | String `"ciao"` provided as input |
| Action | `Keyboard.readInt()` |
| Expected | Returns exactly `Integer.MIN_VALUE` (`-2147483648`); the error counter is incremented by 1 |

### Test 2 (`testReadDoubleConInputNonValido`)

**Purpose:** Verifies the defensive behavior of `readDouble()` when given non-numeric input, ensuring that the exception is handled internally without interrupting the program flow.

| Field | Content |
| --- | --- |
| Input | String `"ciao"` provided as input |
| Action | `Keyboard.readDouble()` |
| Expected | Returns `Double.NaN`; the error counter is incremented |

### Test 3 (`testReadCharConInputNonValido`)

**Purpose:** Verifies the behavior of `readChar()` when a reading error occurs, maintaining consistency with the other methods of the class.

| Field | Content |
| --- | --- |
| Input | Input that causes an exception during reading |
| Action | `Keyboard.readChar()` |
| Expected | Returns `Character.MIN_VALUE`; the error counter is incremented |

### Test 4 (`testIncrementoContatoreErroriMultiplo`)

**Purpose:** Verifies the correct accumulation of the error counter across consecutive errors, demonstrating that the state is maintained between different method calls.

| Field | Content |
| --- | --- |
| Input | Two consecutive invalid reads: `"errore1\n"` followed by `"errore2\n"` |
| Action | `Keyboard.readInt()` on input `"errore1\n"`, followed by `Keyboard.readDouble()` on input `"errore2\n"` |
| Expected | The error counter is correctly incremented to 2 |

### Test 5 (`testReadIntConInputValido`)

**Purpose:** Verifies the correct behavior of `readInt()` with valid input, confirming that normal operation is not affected by the error-handling logic.

| Field | Content |
| --- | --- |
| Input | Value `"42\n"` |
| Action | `Keyboard.readInt()` |
| Expected | Returns the integer value `42`; the error counter remains at 0 |

### Test 6 (`testReadWordSingoloToken`)

**Purpose:** Verifies the correct extraction of a single token using `readWord()`, based on the use of `StringTokenizer`.

| Field | Content |
| --- | --- |
| Input | `"Hello World\n"` |
| Action | `Keyboard.readWord()` |
| Expected | Returns only the first token, `"Hello"`, without incrementing the error counter |

### Test 7 (`testReadStringConcatenaToken`)

**Purpose:** Verifies the concatenation of all tokens up to the end of the line using `readString()`, preserving the original spaces as specified by the implementation.

| Field | Content |
| --- | --- |
| Input | `"Hello World Test\n"` |
| Action | `Keyboard.readString()` (retrieves the first token and iterates through subsequent tokens, concatenating them until `endOfLine()`) |
| Expected | Returns `"Hello World Test"`, preserving the original spaces; the error counter is not incremented |

### Test 8 (`testReadCharConBuffer`)

**Purpose:** Verifies the buffering mechanism in `readChar()`, which stores the remaining characters in `current_token` for subsequent reads.

| Field | Content |
| --- | --- |
| Input | `"ab\nc\n"` |
| Action | Three consecutive calls to `Keyboard.readChar()` |
| Expected | The first read extracts `'a'` and stores `"b"` in the buffer; the second reads `'b'` without consuming new input; the third consumes the next line by reading `'c'`; the error counter remains at 0 for all three reads |