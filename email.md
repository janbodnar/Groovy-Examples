# Emails 

## Send test email

Send test email to Mailtrap service  

```groovy
@Grab(group='com.sun.mail', module='jakarta.mail', version='2.0.1')
@Grab(group='com.sun.activation', module='jakarta.activation', version='2.0.1')

import java.util.Properties

import jakarta.mail.Authenticator
import jakarta.mail.Message
import jakarta.mail.MessagingException
import jakarta.mail.PasswordAuthentication
import jakarta.mail.Session
import jakarta.mail.Transport
import jakarta.mail.internet.InternetAddress
import jakarta.mail.internet.MimeMessage

def to = "test@example.com"

def from = "from@example.com"
def username = "username" // get from Mailtrap
def password = "passdw" // get from Mailtrap

def host = "smtp.mailtrap.io"

def props = new Properties()

props.put("mail.smtp.auth", "true")
props.put("mail.smtp.starttls.enable", "true") // optional in Mailtrap
props.put("mail.smtp.host", host)
props.put("mail.smtp.port", "2525")

def auth = new Authenticator() {
    protected PasswordAuthentication getPasswordAuthentication() {
        return new PasswordAuthentication(username, password)
    }
}

def session = Session.getInstance(props, auth)
def message = new MimeMessage(session)

message.setFrom(new InternetAddress(from))
message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(to))

message.setSubject("Groovy email")
message.setText("Test message")

Transport.send(message)

println("Email sent successfully")
```

---

Send test mail with `simple-java-mail` library  

```groovy
@Grab(group='org.simplejavamail', module='simple-java-mail', version='7.1.0')

import org.simplejavamail.api.email.Email
import org.simplejavamail.email.EmailBuilder
import org.simplejavamail.mailer.MailerBuilder

def username = "username" // get from Mailtrap
def password = "password" // get from Mailtrap
def host = "smtp.mailtrap.io"
def port = 2525;

Email email = EmailBuilder.startingBlank()
    .from("John Doe", "john.doe@example.com")
    .to("Jane Doe", "jane.doe@example.com")
    .to("Peter Doe", "peter.doe@example.com")
    .withSubject("Test subject")
    .withPlainText("Test body")
    .buildEmail();

MailerBuilder
    .withSMTPServer(host, port, username, password)
    .buildMailer()
    .sendMail(email);

println("Email sent successfully")
```
