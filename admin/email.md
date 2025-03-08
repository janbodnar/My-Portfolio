# Emails

## Backup emails

Connects to the email account and copies the emails.

```python
import imaplib
import email
import os
import re
from email.header import decode_header

# Email account credentials
username = 'admin@example.com'
password = 'Secret!'

# IMAP server and port
imap_server = 'imap.example.com'  
imap_port = 993  # Default port for IMAP over SSL

# Directory to save the emails
save_directory = 'downloaded_emails'

# Create directory if it doesn't exist
if not os.path.exists(save_directory):
    os.makedirs(save_directory)

def decode_mime_words(s):
    try:
        decoded_words = decode_header(s)
        print(f"Decoded Words: {decoded_words}")  # Debugging print statement
        decoded_string = ''.join([str(text, charset or 'utf-8') if isinstance(text, bytes) else text for text, charset in decoded_words])
        return decoded_string
    except Exception as e:
        print(f"Error decoding MIME words: {e}")
        return "Unknown Subject"

def sanitize_filename(filename):
    # Remove or replace invalid characters for Windows filenames
    if filename is None:
        return 'unknown_filename'
    filename = re.sub(r'[<>:"/\\|?*\r\n\t]', '_', filename)
    return filename

# Connect to the IMAP server
mail = imaplib.IMAP4_SSL(imap_server, imap_port)
mail.login(username, password)
mail.select('inbox')

# Search for all emails in the inbox
result, data = mail.search(None, 'ALL')

if result == 'OK':
    email_ids = data[0].split()
    for email_id in email_ids:
        try:
            result, message_data = mail.fetch(email_id, '(RFC822)')
            if result == 'OK':
                email_message = email.message_from_bytes(message_data[0][1])
                subject = email_message['subject']
                date = email_message['date']
                
                # Decode the subject if needed
                if subject is not None:
                    subject = decode_mime_words(subject)
                else:
                    subject = 'No Subject'

                if date is None:
                    date = 'No Date'
                
                # Create a sanitized filename based on the subject and date
                filename = f"{subject} - {date}.eml"
                sanitized_filename = sanitize_filename(filename)
                print(f"Sanitized Filename: {sanitized_filename}")  # Debugging print statement
                filepath = os.path.join(save_directory, sanitized_filename)
                
                # Save the email to a file
                with open(filepath, 'wb') as email_file:
                    email_file.write(message_data[0][1])
            else:
                print(f"Failed to fetch email with ID {email_id}")
        except Exception as e:
            print(f"Could not fetch email with ID {email_id}: {e}")
else:
    print("No emails found")

# Logout and close the connection
mail.logout()
```
