
## Overview

This practical guide demonstrates how to use Crunch for generating custom wordlists in legitimate security testing scenarios. Crunch is a valuable tool for penetration testers and security professionals conducting authorized assessments.



## Prerequisites

- Linux system (Kali Linux recommended)
- Crunch installed (`sudo apt-get install crunch`)
- Basic terminal knowledge

## Lab Exercises

### Exercise 1: Basic Wordlist Generation

**Objective:** Create simple wordlists with varying lengths

#### Task 1.1: Generate 1-3 character wordlist

```bash
# Create a basic wordlist with lowercase letters (default charset)
crunch 1 3 -o basic_wordlist.txt

# View the first 10 entries
head -10 basic_wordlist.txt

# Check file size and line count
wc -l basic_wordlist.txt
ls -lh basic_wordlist.txt
```

**Expected Output:** File containing combinations like: a, b, c, ..., aa, ab, ac, ..., aaa, aab, etc.

#### Task 1.2: Numeric-only wordlist

```bash
# Generate 3-4 digit numeric passwords
crunch 3 4 0123456789 -o numeric_pins.txt

# Preview the content
head -20 numeric_pins.txt
tail -10 numeric_pins.txt
```

### Exercise 2: Custom Character Sets

**Objective:** Create targeted wordlists using specific character combinations

#### Task 2.1: Alphanumeric wordlist (5-7 characters)

```bash
# Using specific characters: p, a, s, s, 1, 2, 3
crunch 5 7 pass123 -o alphanumeric_dict.txt

# Analyze the output
echo "Total combinations generated:"
wc -l alphanumeric_dict.txt

# Show first and last few entries
echo "First 5 entries:"
head -5 alphanumeric_dict.txt
echo "Last 5 entries:"
tail -5 alphanumeric_dict.txt
```

#### Task 2.2: Mixed case passwords

```bash
# Generate passwords with uppercase, lowercase, and numbers
crunch 6 8 abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 -o mixed_case.txt

# Alternative shorter syntax
crunch 6 8 -f /usr/share/crunch/charset.lst mixalpha-numeric -o mixed_short.txt
```

### Exercise 3: Special Characters Integration

**Objective:** Include special characters for more complex passwords

#### Task 3.1: Passwords with symbols

```bash
# Create wordlist with letters, numbers, and special characters
crunch 5 7 abc123@$ -o special_chars.txt

# Count total combinations
echo "Special character combinations:"
wc -l special_chars.txt

# Sample some entries
shuf -n 10 special_chars.txt
```

#### Task 3.2: Common special characters

```bash
# More comprehensive special character set
crunch 6 8 'abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*' -o comprehensive.txt

# Note: Use quotes to handle special shell characters
```

### Exercise 4: Pattern-Based Generation

**Objective:** Create wordlists following specific patterns

#### Task 4.1: Using pattern specifications

```bash
# Pattern: 2 lowercase letters + 4 numbers
crunch 6 6 -t @@%%%% -o pattern_basic.txt

# Pattern explanation:
# @ = lowercase letter
# % = number
# ^ = uppercase letter
# , = symbol

# Show examples
head -10 pattern_basic.txt
```

#### Task 4.2: Advanced patterns

```bash
# Pattern: Name-like (Capital + lowercase + 2-4 numbers)
crunch 5 7 -t ^@%%% -o name_pattern.txt
crunch 6 8 -t ^@@%%%% -o name_pattern_long.txt

# Company-style passwords (3 letters + year-like numbers)
crunch 7 7 -t @@@%%%% -o company_style.txt
```

#### Task 4.3: Date-based patterns

```bash
# Birth year patterns (19XX, 20XX)
crunch 4 4 -t 19%% -o birth1900s.txt
crunch 4 4 -t 20%% -o birth2000s.txt

# Combine with names (example: john1995)
crunch 8 8 -t john%%%% -o john_years.txt
```

### Exercise 5: Advanced Techniques

**Objective:** Utilize advanced Crunch features

#### Task 5.1: Using charset files

```bash
# View available character sets
cat /usr/share/crunch/charset.lst

# Use predefined charset
crunch 5 8 -f /usr/share/crunch/charset.lst lalpha-numeric -o predefined.txt

# Create custom charset file
echo "password123!@#" > custom_charset.txt
crunch 8 12 -f custom_charset.txt -o from_file.txt
```

#### Task 5.2: Limiting output size

```bash
# Generate only first 1000 combinations
crunch 4 6 abc123 -c 1000 -o limited.txt

# Split large wordlists
crunch 6 8 abcdefghijklmnopqrstuvwxyz -b 10mb -o split_wordlist
```

#### Task 5.3: Inverting and permutations

```bash
# Start from specific string
crunch 5 7 abc123 -s aa123 -o start_from.txt

# End at specific string
crunch 5 7 abc123 -e cc321 -o end_at.txt
```

### Exercise 6: Real-World Scenarios

**Objective:** Apply Crunch in practical security testing contexts

#### Task 6.1: WiFi password testing wordlist

```bash
# Common WiFi password patterns (8-13 characters)
crunch 8 13 abcdefghijklmnopqrstuvwxyz0123456789 -o wifi_common.txt

# Router default passwords (often 8 chars, mixed case + numbers)
crunch 8 8 -f /usr/share/crunch/charset.lst mixalpha-numeric -o router_defaults.txt
```

#### Task 6.2: Corporate password policy compliance

```bash
# Assuming policy: minimum 8 chars, must have upper, lower, number, symbol
crunch 8 12 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()' -o corporate_policy.txt

# Pattern-based corporate (InitialName + 4 digits)
crunch 5 5 -t ^%%%% -o corporate_pattern.txt
```

## Performance Considerations

### File Size Management

```bash
# Check estimated file size before generation
crunch 6 8 abc123 -c 0

# Use compression for large files
crunch 6 8 abc123 | gzip > compressed_wordlist.txt.gz

# Stream directly to other tools (avoid large files)
crunch 6 8 abc123 | head -1000
```

### Memory and Storage Tips

- Start with smaller ranges to test
- Use `-c` option to limit output
- Consider using pipes instead of files for large wordlists
- Monitor disk space during generation

## Integration with Security Tools

### Using with Hydra

```bash
# Generate targeted wordlist for Hydra
crunch 6 10 abc123!@# -o hydra_wordlist.txt

# Example Hydra command (for authorized testing)
# hydra -l admin -P hydra_wordlist.txt ssh://target-ip
```

### Using with John the Ripper

```bash
# Create wordlist optimized for John
crunch 6 12 -f /usr/share/crunch/charset.lst mixalpha-numeric-all -o john_wordlist.txt

# Example John usage
# john --wordlist=john_wordlist.txt hashfile.txt
```

## Best Practices

1. **Start Small:** Always test with small ranges first
2. **Monitor Resources:** Watch CPU, memory, and disk usage
3. **Authorization:** Only use on systems you own or have permission to test
4. **Documentation:** Keep records of wordlists generated for different scenarios
5. **Cleanup:** Remove large wordlists after testing to save space

## Common Patterns Reference

|Pattern|Description|Example|
|---|---|---|
|`@@%%%%`|2 letters + 4 numbers|ab1234|
|`^@@%%%`|Capital + 2 lowercase + 3 numbers|Abc123|
|`@@@@^^^^`|4 lowercase + 4 uppercase|abcdEFGH|
|`@@,%%%%`|2 letters + symbol + 4 numbers|ab!1234|

## Troubleshooting

### Common Issues:

- **Large file sizes:** Use `-c` to limit or pipe to other tools
- **Special characters:** Use quotes around character sets
- **Permission errors:** Check write permissions in output directory
- **Memory issues:** Reduce range or use streaming approach

### File Size Estimation:

Before generating large wordlists, estimate the size:

```bash
# Check how many combinations will be generated
crunch 6 8 abc123 -c 0
```

## Conclusion

Crunch is a powerful tool for generating targeted wordlists in authorized security testing scenarios. Remember to always:

- Use responsibly and with proper authorization
- Start with small test cases
- Monitor system resources
- Document your testing methodology
- Clean up temporary files
