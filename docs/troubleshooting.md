# Troubleshooting log

Each entry follows the same format: problem, hypothesis, test, result, fix, lesson.

## SSH key authentication kept asking for a password

**Problem:** After installing my public key on the Raspberry Pi, SSH still prompted for a password instead of using the key.

**Hypothesis:** The key was not installed correctly on the server, or file permissions were stopping the SSH daemon from trusting it.

**Tests:**
1. Checked the `~/.ssh` directory and the `authorized_keys` file existed.
2. Checked permissions on both.
3. Checked the contents of `authorized_keys`.

**Result:** The file existed but was 0 bytes. I had exited the editor without saving the file, so the key was never written.

**Fix:**
1. Pasted the public key again and saved it properly.
2. Set restrictive permissions: directory `700`, file `600`.
3. Reconnected. Key authentication worked.

**Lessons:**
- "File exists" does not mean "file has content". Check size and contents.
- SSH refuses keys when permissions are too open, so permissions are part of the check.
- Work through the chain in order: client key, server file, contents, permissions.
