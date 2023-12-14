import hashlib
import os
# Line 9 has to have .upper() in order to convert hash to all upper case; otherwise it will return all lower case and it will not work properly.
def calculate_file_hash(file_path):
    sha256 = hashlib.sha256()
    with open(file_path, 'rb') as f:
        for byte_block in iter(lambda: f.read(4096), b""):
            sha256.update(byte_block)
    return sha256.hexdigest().upper()

# Lines 14 and 15 can be removed after checkin to see if the code works properly.
# Lines 14 and 15 can give information to attackers if not removed.
def check_file_integrity(file_path, stored_hash):
    current_hash = calculate_file_hash(file_path)
    print(f"Calculated Hash: {current_hash}")
    print(f"Stored Hash: {stored_hash}")
    return current_hash == stored_hash

if __name__ == "__main__":
    # Make sure to include (r'file_path') to fix escape backslashes for windows.
    file_path = r'file_path'

    # Get Hash value from PowerShell
    # Get-Filehash C:\File_path
    # Everytime the file is altered, get new hash value from PowerShell to adjust stored_hash_value
    stored_hash_value = 'hash_value'

    if os.path.exists(file_path):
        if check_file_integrity(file_path, stored_hash_value):
            print("File integrity is intact.")
        else:
            print("File integrity has been compromised.")
    else:
        print("File not found.")
