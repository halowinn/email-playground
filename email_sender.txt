import smtplib
import ssl
from email.message import EmailMessage
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders
from email.utils import formatdate
from string import Template
from pathlib import Path  # os.path

# html = Template(Path('index.html').read_text())
sender = 'allowinpy95@gmail.com'
sender_pass = 'allowin1101'
receiver = 'allowinheng95@hotmail.com'
subject = 'Files of password checkers'

email = MIMEMultipart()
email['from'] = sender
email['to'] = receiver
email['Date'] = formatdate(localtime=True)
email['subject'] = subject

body = 'Hello, halowin!'
email.attach(MIMEText(body, 'plain'))

filename = 'C:/Users/Asus/Python/Exercises/email-playground/email_sender.txt'
attachment = open(filename, 'rb')

p = MIMEBase('application', 'octet-stream')
p.set_payload(attachment.read())
encoders.encode_base64(p)
p.add_header('Content-Disposition', f"attachment; filename= {filename}")

email.attach(p)
text = email.as_string()

# email.set_content(html.substitute(name= 'halowin'), 'html')

context = ssl.create_default_context()
with smtplib.SMTP_SSL(host='smtp.gmail.com', port=465, context=context) as smtp:
    smtp.ehlo()
    smtp.login(sender, sender_pass)
    smtp.sendmail(sender, receiver, text)
    smtp.quit()
    print('ALL DONE!')
