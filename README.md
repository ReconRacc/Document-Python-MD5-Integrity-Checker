# Document-Python-MD5-Integrity-Checker
Developed a Python automation tool that generates and verifies MD5 hashes to ensure file integrity. The script automatically scans specified files or directories, calculates their MD5 checksums, and compares them against known values to detect changes or tampering. Includes logging and error handling to support repeatable integrity checks, making it useful for security audits and system monitoring.

# Overview
- This Python 3 script generates and verifies MD5 hashes to check file integrity. It scans files in a given directory, calculates their MD5 checksum, and compares them against known hash values to detect any changes or tampering.
- The tool is lightweight, easy to use, and requires only Python’s built-in libraries.

# Features
- Generate MD5 hashes for files
- Verify files against known hash values
- Simple, readable code for learning or adapting to other use cases
- No external dependencies

# How It Works
- The script reads each target file in binary mode and calculates its MD5 checksum.
- It compares the calculated checksum against a known value from a provided dictionary.
- It prints whether each file matches or has been altered.

# Example Usage
- Edit the script to set:
- target_directory — the path to the directory containing your files
- known_hashes_dict — a dictionary of filenames and their expected MD5 hashes

# Run the script:
- python3 md5_hash_verifier.py

# Sample Output
[OK] example.txt matches expected hash.
[WARNING] data.csv does not match expected hash!
Expected: 5d41402abc4b2a76b9719d911017c592 | Found: 098f6bcd4621d373cade4e832627b4f6

# Code Example
import hashlib
import os

def md5_hash_file(file_path):
    md5 = hashlib.md5()
    try:
        with open(file_path, "rb") as f:
            for chunk in iter(lambda: f.read(4096), b""):
                md5.update(chunk)
        return md5.hexdigest()
    except FileNotFoundError:
        print(f"File not found: {file_path}")
        return None

def verify_files(directory, known_hashes):
    for filename, known_hash in known_hashes.items():
        file_path = os.path.join(directory, filename)
        file_hash = md5_hash_file(file_path)
        if file_hash is None:
            continue
        if file_hash == known_hash:
            print(f"[OK] {filename} matches expected hash.")
        else:
            print(f"[WARNING] {filename} does not match expected hash!")
            print(f"Expected: {known_hash} | Found: {file_hash}")

if __name__ == "__main__":
    target_directory = "path/to/files"
    known_hashes_dict = {
        "example.txt": "098f6bcd4621d373cade4e832627b4f6",
        "data.csv": "5d41402abc4b2a76b9719d911017c592"
    }
    verify_files(target_directory, known_hashes_dict)
