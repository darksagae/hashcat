# hashcat
**Hashcat** is a powerful password recovery tool used for recovering or cracking passwords by utilizing various hashing algorithms. Here’s a guide on how to use Hashcat on Kali Linux, along with examples and expected outputs.

### Installation

Hashcat is usually pre-installed on Kali Linux. You can verify its presence by running:

```bash
hashcat --version
```

If it’s not installed, you can install it with:

```bash
sudo apt update
sudo apt install hashcat
```

### Basic Usage

1. **Prepare Your Hashes**:
   - Create a text file containing the hashes you want to crack. For example, create a file named `hashes.txt`:

   ```plaintext
   $6$rounds=656000$RANDOMSALT$hashedpassword
   ```

2. **Select a Hashing Algorithm**:
   - Determine the type of hash you are dealing with. Hashcat supports many algorithms, which can be found in the official documentation. For example, if you are cracking SHA-512 hashes, you would use the identifier `1800`.

3. **Run Hashcat**:
   - Use the command structure:

   ```bash
   hashcat -m <hash_type> -a <attack_mode> <hash_file> <wordlist>
   ```

   For example, to crack SHA-512 hashes using a dictionary attack:

   ```bash
   hashcat -m 1800 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
   ```

### Attack Modes

- **-a 0**: Straight attack (using a wordlist).
- **-a 1**: Combination attack (combining words from two wordlists).
- **-a 3**: Brute-force attack.

### Examples

1. **Cracking MD5 Hashes**:
   - Create a file `md5_hashes.txt`:

   ```plaintext
   5f4dcc3b5aa765d61d8327deb882cf99
   ```

   - Run Hashcat:

   ```bash
   hashcat -m 0 -a 0 md5_hashes.txt /usr/share/wordlists/rockyou.txt
   ```

2. **Using a Custom Wordlist**:
   - Create a custom wordlist file named `custom_wordlist.txt`:

   ```plaintext
   password123
   letmein
   qwerty
   ```

   - Run Hashcat:

   ```bash
   hashcat -m 0 -a 0 md5_hashes.txt custom_wordlist.txt
   ```

### Expected Outputs

- **Successful Cracks**: If Hashcat successfully cracks a password, it will display the hash and the corresponding plaintext password. For example:

```plaintext
5f4dcc3b5aa765d61d8327deb882cf99:password
```

- **Progress Indicators**: Hashcat will show progress indicators, including the number of hashes cracked, the total time taken, and the speed of the cracking process.

### Conclusion

Hashcat is a versatile and powerful tool for password recovery and cracking. Always ensure that you use it responsibly and legally, only on hashes you have permission to test.


                            ALTERNATIVE
### Using `hashcat` on Kali Linux

**Hashcat** is one of the most powerful and versatile password recovery tools available. It is used to crack password hashes using brute force or dictionary attacks and supports a wide range of hashing algorithms.

---

### Installation

Hashcat is pre-installed in Kali Linux, but you can verify its presence or install it if necessary:

```bash
sudo apt update
sudo apt install hashcat
```

---

### Basic Usage

The general syntax for Hashcat is as follows:

```bash
hashcat [options] hashfile [wordlist|mask|rules]
```

---

### Steps to Use `hashcat`

#### 1. **Identify the Hash Type**
Before using `hashcat`, you need to identify the type of hash you want to crack. You can use tools like `hashid` or `hash-identifier`.

Example:

```bash
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt
hashid hash.txt
```

This will identify the hash type (e.g., MD5 in this case).

---

#### 2. **Prepare the Hash File**
Put the hash you want to crack into a text file. For example:

```plaintext
5f4dcc3b5aa765d61d8327deb882cf99
```

Save it as `hash.txt`.

---

#### 3. **Choose an Attack Mode**
Hashcat supports several attack modes, such as:

- **Straight Attack (Default)**: Uses a wordlist.
- **Combination Attack**: Combines two wordlists.
- **Brute-Force Attack**: Tries all possible combinations.
- **Mask Attack**: Targets specific patterns (e.g., "password1", "admin123").
- **Hybrid Attack**: Combines wordlist and mask attacks.

---

#### 4. **Run Hashcat**

**Example 1: Cracking MD5 Hash with a Wordlist**

If you have a wordlist (e.g., `rockyou.txt`), use it to try cracking the hash:

```bash
hashcat -a 0 -m 0 hash.txt /usr/share/wordlists/rockyou.txt
```

- `-a 0`: Specifies a straight attack (wordlist-based).
- `-m 0`: Specifies the hash type (MD5 in this case).
- `hash.txt`: Contains the hash to crack.
- `/usr/share/wordlists/rockyou.txt`: The wordlist to use.

**Expected Output**:

```plaintext
5f4dcc3b5aa765d61d8327deb882cf99:password
```

This output shows the cracked password (`password`) for the hash.

---

**Example 2: Brute-Force Attack**

If the password is not in your wordlist, you can use a brute-force attack:

```bash
hashcat -a 3 -m 0 hash.txt ?a?a?a?a?a?a
```

- `-a 3`: Specifies a brute-force attack.
- `-m 0`: Specifies the hash type (MD5).
- `?a?a?a?a?a?a`: Defines the mask (all printable characters, 6-character length).

**Expected Output**:

```plaintext
5f4dcc3b5aa765d61d8327deb882cf99:abc123
```

This output shows the cracked password using brute force.

---

**Example 3: Mask Attack**

If you know part of the password or its pattern (e.g., "admin123"), you can use a mask to narrow down the possibilities:

```bash
hashcat -a 3 -m 0 hash.txt admin?d?d?d
```

- `admin`: Fixed prefix.
- `?d`: Represents a digit (0-9).

**Expected Output**:

```plaintext
5f4dcc3b5aa765d61d8327deb882cf99:admin123
```

---

**Example 4: Cracking SHA256 Hash**

If you have a SHA256 hash, use the appropriate hash mode:

```bash
hashcat -a 0 -m 1400 hash.txt /usr/share/wordlists/rockyou.txt
```

- `-m 1400`: Specifies SHA256 as the hash type.

**Expected Output**:

```plaintext
b109f3bbbc244eb82441917ed06d618b9008dd09:welcome123
```

---

### Commonly Used Hash Modes

| Hash Type         | Mode   |
|--------------------|--------|
| MD5               | `0`    |
| SHA1              | `100`  |
| SHA256            | `1400` |
| NTLM              | `1000` |
| bcrypt            | `3200` |
| WPA/WPA2          | `2500` |
| Kerberos 5 TGS    | `13100`|

You can find the full list of hash types with:

```bash
hashcat --help
```

---

### Tips

1. Use GPU acceleration if available (Hashcat supports CUDA and OpenCL GPUs).
2. Optimize performance using rules and masks.
3. Always ensure you have permission to crack or test passwords.

**Disclaimer**: Hashcat should only be used for ethical purposes, such as penetration testing or password recovery for systems you own or have explicit permission to test.





                        ALTERNATIVE
Understood, my apologies for the confusion earlier. Let me provide information on using the `hashcat` tool in Kali Linux.

`hashcat` is a powerful password cracking tool available in Kali Linux. It is designed to recover various types of hashed passwords using brute-force or dictionary attacks. Here's how you can use it, along with some examples and expected outputs.

**Installation**
`hashcat` is typically pre-installed in Kali Linux. You can verify its installation by running the following command:

```
hashcat --version
```

If it's not installed, you can install it using the following command:

```
sudo apt-get update
sudo apt-get install hashcat
```

**Usage**
The basic syntax for using `hashcat` is:

```
hashcat [options] [hash] [dictionary]
```

Here's a breakdown of the essential options:

- `-m`: Specifies the hash type (e.g., `-m 1000` for NTLM hashes)
- `-a`: Specifies the attack mode (e.g., `-a 0` for dictionary attack, `-a 3` for brute-force attack)
- `[hash]`: The hash you want to crack
- `[dictionary]`: The dictionary file containing candidate passwords

**Examples**

1. **Cracking an NTLM hash using a dictionary attack**:
   ```
   hashcat -m 1000 -a 0 hash.txt rockyou.txt
   ```
   This command will attempt to crack an NTLM hash (hash type 1000) using a dictionary attack with the `rockyou.txt` wordlist.

2. **Cracking an MD5 hash using a brute-force attack**:
   ```
   hashcat -m 0 -a 3 hash.txt ?a?a?a?a?a?a
   ```
   This command will attempt to crack an MD5 hash (hash type 0) using a brute-force attack with a password length of 6 characters, consisting of any alphanumeric character.

**Expected Outputs**
The output of `hashcat` will depend on the success of the password cracking process. Some possible outputs include:

- **Cracked Passwords**: If `hashcat` is able to find the original password, it will display the cracked password.
- **Candidate Passwords**: If `hashcat` is unable to find the original password, it will display the candidate passwords that it tried during the attack.
- **Performance Statistics**: `hashcat` will provide information about the performance of the attack, such as the number of hashes processed per second (hash rate).
- **Errors**: If there are any issues with the input hash or the attack parameters, `hashcat` will display relevant error messages.

It's important to note that using `hashcat` for password cracking should be done with caution and only on systems you have permission to access, as it can be used for both legitimate and malicious purposes.




                                    ALTERNATIVE
Hashcat is a popular password cracking tool available on Kali Linux. Here's a comprehensive guide on how to use it, along with examples and expected outputs:

**Usage**

Hashcat is a command-line tool, and its basic syntax is as follows:

```
hashcat [options] hashfile [mask|wordlist|stdin]
```

**Options**

* `-m` specifies the hash type (e.g., `-m 0` for MD5, `-m 1000` for NTLM)
* `-a` specifies the attack mode (e.g., `-a 0` for straight mode, `-a 3` for brute-force mode)
* `-o` specifies the output file
* `-w` specifies the workload profile (e.g., `-w 1` for low priority, `-w 4` for high priority)

**Examples**

1. **Cracking a Hash**

Suppose you have a hash file `hashes.txt` containing the following MD5 hash:

```
31d65653844346565
```

You can crack it using the following command:

```
hashcat -m 0 -a 0 -o cracked.txt hashes.txt rockyou.txt
```

Here, we're using the `rockyou.txt` wordlist to crack the hash.

2. **Brute-Force Attack**

You can perform a brute-force attack on a hash using the following command:

```
hashcat -m 0 -a 3 -o cracked.txt hashes.txt ?d?d?d?d?d
```

Here, we're using a mask of 5 digits (`?d?d?d?d?d`) to perform the brute-force attack.

**Output**

The output of Hashcat depends on the success of the cracking attempt. If the hash is cracked, Hashcat will display the cracked password and save it to the specified output file.

For example, if the hash is cracked successfully, the output might look like this:

```
$hashcat -m 0 -a 0 -o cracked.txt hashes.txt rockyou.txt
[...]
31d65653844346565:password123
```

In this case, the cracked password is `password123`.

**Tips and Precautions**

* Always use Hashcat responsibly and ensure you have permission to crack the hashes.
* Use strong wordlists and masks to increase the chances of cracking the hash.
* Be cautious when using brute-force attacks, as they can be computationally intensive and may take a long time to complete.

I hope this helps you get started with using Hashcat on Kali Linux!



                                 ALTERNATIVE
Hashcat is a powerful password recovery tool in Kali Linux, known for its speed and versatility in supporting various hashing algorithms. It is used for cracking hashed passwords using various attack methods like brute-force, dictionary, hybrid, and rule-based attacks.

### Installation

Hashcat comes pre-installed in Kali Linux. To ensure you have the latest version, you can update it using the following commands:

```bash
sudo apt update
sudo apt install hashcat
```

To verify the installation, you can check the version:

```bash
hashcat -V
```

### Basic Usage

1.  **Prepare Your Hashes**: You need a list of hashed passwords you want to crack. For example, you might have a file named `hashes.txt` containing MD5 hashed passwords.
2.  **Choose a Wordlist**: Hashcat uses wordlists for dictionary attacks. A common wordlist is `rockyou.txt`, which is often located at `/usr/share/wordlists/rockyou.txt`.
3.  **Run Hashcat**: Execute Hashcat with the appropriate options. The basic command structure is:

```bash
hashcat -m <hash_type> -a <attack_mode> -o <output_file> <hash_file> <wordlist>
```

*   `-m <hash_type>`: Specifies the hash type. For MD5, use `0`.
*   `-a <attack_mode>`: Specifies the attack mode. For a dictionary attack, use `0`.
*   `-o <output_file>`: Specifies the file to save recovered passwords.
*   `<hash_file>`: The file containing the hashes to crack.
*   `<wordlist>`: The wordlist to use for the attack.

### Examples

1.  **Dictionary Attack with MD5 Hashes**:

```bash
hashcat -m 0 -a 0 -o recovered_passwords.txt hashes.txt /usr/share/wordlists/rockyou.txt
```

This command performs a dictionary attack on MD5 hashes in `hashes.txt`, using the `rockyou.txt` wordlist, and saves the cracked passwords to `recovered_passwords.txt`.

2.  **Using a specific Hashmode**

```bash
hashcat -m 500 example500.hash /usr/share/wordlists/sqlmap.txt
```

This command uses md5crypt mode (-m 500) to crack the sample hash (example500.hash) with the provided wordlist (/usr/share/wordlists/sqlmap.txt).

### Expected Outcomes

*   **Cracked Passwords**: Hashcat will save any recovered passwords in the specified output file (e.g., `recovered_passwords.txt`). The format is typically `hash:password`.
*   **Session Management**: Hashcat saves its progress in a `.pot` file.

### Additional Features

*   **Session Management**:
    *   Save a session:

    ```bash
    hashcat -m 0 -a 0 -o recovered_passwords.txt --session=mySession hashes.txt /usr/share/wordlists/rockyou.txt
    ```
    *   Restore a session:

    ```bash
    hashcat --session=mySession --restore
    ```

*   **Show Cracked Passwords**: To display the cracked hashes saved in the `.pot` file:

```bash
hashcat --show
```



                                  ALTERNATIVE
### Using Hashcat on Kali Linux

Hashcat is a powerful password recovery tool available on Kali Linux, designed to crack various types of hashes using different attack modes. Here’s how to use it, along with examples and expected outputs.

#### Installation

Hashcat is typically pre-installed in Kali Linux. If you need to install it, you can do so with the following command:

```bash
sudo apt install hashcat
```

#### Basic Usage

1. **Running a Benchmark**:
   To check the performance of your hardware with different hash types, you can run a benchmark:

   ```bash
   hashcat -b
   ```

   This command will display the speed of various hash types supported by your hardware [[1]](https://www.kali.org/tools/hashcat/).

2. **Cracking a Hash**:
   To crack a specific hash, you need to know the hash type and have a wordlist. For example, to crack an MD5 hash using a wordlist, you would use:

   ```bash
   hashcat -m 0 -a 0 example.hash /usr/share/wordlists/rockyou.txt
   ```

   Here, `-m 0` specifies the hash type (MD5), `-a 0` specifies a straight dictionary attack, `example.hash` is the file containing the hash, and `/usr/share/wordlists/rockyou.txt` is the wordlist file [[1]](https://www.kali.org/tools/hashcat/).

3. **Using a Custom Wordlist**:
   If you have a custom wordlist, you can specify it in the command. For example:

   ```bash
   hashcat -m 1000 -a 0 example.hash /path/to/your/wordlist.txt
   ```

   In this case, `-m 1000` is used for NTLM hashes [[1]](https://www.kali.org/tools/hashcat/).

4. **Monitoring Progress**:
   While Hashcat is running, you can monitor its progress. The output will show the current status, including the number of hashes cracked, speed, and estimated time remaining. For example:

   ```
   Session..........: hashcat
   Status...........: Running
   Hash.Type........: NTLM
   Hash.Target......: 5baa61e5b0c090b4b11e8bda1d3b3e0d
   Time.Started.....: Mon Feb 26 12:00:00 2025
   Time.Estimated...: Mon Feb 26 12:05:00 2025
   Guess.Base.......: File (/path/to/your/wordlist.txt)
   Speed.#1.........: 1000 H/s
   Recovered.........: 1/1 (100.00%)
   ```

#### Example Output

When you successfully crack a hash, Hashcat will display the cracked password alongside the hash. For example:

```
5baa61e5b0c090b4b11e8bda1d3b3e0d:password
```

This indicates that the hash `5baa61e5b0c090b4b11e8bda1d3b3e0d` corresponds to the password `password` [[1]](https://www.kali.org/tools/hashcat/).

### Conclusion

Hashcat is a versatile tool for password recovery and hash cracking on Kali Linux. By using the appropriate commands and options, you can effectively crack various types of hashes.

### Using Hashcat on Kali Linux

Hashcat is a powerful password recovery tool that supports various hashing algorithms and is widely used for cracking passwords. Here’s how to use it effectively on Kali Linux, along with examples and expected outputs.

#### Installation

Hashcat is typically pre-installed on Kali Linux. If you need to install it, you can do so with the following command:

```bash
sudo apt install hashcat
```

#### Basic Usage

1. **Running a Benchmark**:
   To check the performance of your hardware with different hash types, you can run a benchmark:

   ```bash
   hashcat -b
   ```

   This command will display the speed of various hash algorithms supported by your system [[1]](https://www.kali.org/tools/hashcat/).

2. **Cracking Passwords**:
   To crack a password hash, you need the hash itself and a wordlist. For example, if you have an MD5 hash and a wordlist located at `/usr/share/wordlists/rockyou.txt`, you can use:

   ```bash
   hashcat -m 0 -a 0 <hash_file> /usr/share/wordlists/rockyou.txt
   ```

   Here, `-m 0` specifies the hash type (MD5), and `-a 0` indicates a straight dictionary attack [[1]](https://www.kali.org/tools/hashcat/).

3. **Example of Cracking an MD5 Hash**:
   Suppose you have a file named `hashes.txt` containing the following MD5 hash:

   ```
   5f4dcc3b5aa765d61d8327deb882cf99
   ```

   You can run:

   ```bash
   hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
   ```

   The output will show the progress of the cracking attempt, including the number of guesses made and the time elapsed. If successful, it will display the cracked password [[1]](https://www.kali.org/tools/hashcat/).

4. **Using Rules**:
   Hashcat allows the use of rules to modify the wordlist entries. For example, to apply a set of rules to the cracking process, you can use:

   ```bash
   hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
   ```

   This command applies the `best64` rules to the wordlist, potentially increasing the chances of cracking complex passwords [[1]](https://www.kali.org/tools/hashcat/).

#### Expected Outputs

- **Benchmark Results**: When running a benchmark, you will see the speed of different hash types, which helps you understand the capabilities of your hardware.
- **Cracking Progress**: While cracking, Hashcat will display the current status, including the number of hashes processed, the speed of guesses per second, and the estimated time remaining.
- **Cracked Passwords**: If successful, Hashcat will output the cracked passwords alongside their corresponding hashes.

### Conclusion

Hashcat is a versatile tool for password recovery and cracking, supporting numerous hash types and attack modes. By using it effectively, you can recover lost passwords or test the strength of your own passwords.

---
Learn more:
1. [hashcat | Kali Linux Tools](https://www.kali.org/tools/hashcat/)
2. [hashcat - advanced password recovery](https://hashcat.net/hashcat/)
3. [GitHub - hashcat/hashcat: World's fastest and most advanced password recovery utility](https://github.com/hashcat/hashcat)
