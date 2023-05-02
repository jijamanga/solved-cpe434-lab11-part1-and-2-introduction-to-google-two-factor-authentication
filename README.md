Download Link: https://assignmentchef.com/product/solved-cpe434-lab11-part1-and-2-introduction-to-google-two-factor-authentication
<br>



In the last lab assignment, we saw how we can hack into a vulnerable account using multiple retries of passwords. But as an engineer with security in mind, we must be able to protect such vulnerable systems.

In this assignment, we will study one such method of protection, namely <strong>Google Two Factor Authentication</strong>. <strong>You are required to take at least one screenshot per each step. Please paste them with description in your report.</strong>

<h1>PreTask I</h1>

<ol>

 <li>How many possible protection strategies can you think of in order to protect the system that you hacked into during last lab assignment? Please think about at least two such strategies. More than two are always welcome. You do not have to be specific about any product or tool, but just think of the possible strategies that can be adapted to protect from intruder (which you were in last assignment). <strong>[10]</strong></li>

</ol>

<em>Pluggable Authentication Module</em>, PAM is an authentication infrastructure for user authentication. <em>Google Two Factor</em> is an example of PAM. We will install Two Factor Authentication in second user account, that you previously created in Guest machine. This time again we will try to hack into this second guest user account using same strategy we used before and see if we can hack or not.

<em>Warning: All the task that you perform in this assignment should be performed in a single sitting. Following the exact order of instructions is compulsory. Please do not close any open terminals unless specifically told to. This lab module is designed to follow <strong>Learning By Doing</strong> paradigm, so there might not be so many statements ending in <strong>?</strong> sign. You should prepare the report by keeping in mind that you are preparing a tutorial highlighting the necessity of Two Factor authentication and steps involved in doing so.</em>

<h1>Subtask I (Installation)</h1>

<ol>

 <li>Open a terminal. Log into <em>echo</em> and log into your odroid machine using the credentials given to you. This terminal will be called <strong>First Terminal</strong></li>

 <li>Log into the <em>root</em> user of the guest using ssh.</li>

 <li>Open second terminal. Log into <em>echo</em>, log into your odroid machine and log into <em>guest</em> as root. This terminal will be called <strong>Backup Terminal.</strong> All your following operations will be performed in the previous <strong>First Terminal</strong> unless specifically mentioned, but you will leave <strong>Backup Terminal</strong> open through the entire duration of this lab session.</li>

 <li>In your <strong>First Terminal</strong> as <em>root</em> in guest</li>

 <li>Make sure that you are in your root user account in guest. Perform the following command in terminal as a sudoer</li>

</ol>

apt-get install libpam-google-authenticator

This will install google authenticator in your machine

<ol start="5">

 <li>Although you have installed two factor authenticator in your guest machine, you can still run attacks since we have not configured it to work. Log out of the root user and Log back in to see it for yourself.</li>

</ol>

<h1>Hacking I</h1>

<ol start="6">

 <li>Open a new terminal. This will be called <strong>Hacking Terminal</strong>. Log into your odroid Host. Use Hydra attack to attack the second user in guest as you did in previous lab. Explain the procedure and result. Are you able to hack into the account? Post the screenshot.</li>

</ol>

<h1>Subtask II (Configuring Google Auth)</h1>

<ol start="7">

 <li>Now we will set up two factor authentication for guest. And again try to hack to the guest using Hydra and the same password file that you created earlier. Make sure you have not changed the password of the second guest. If you have, then make sure to change the password in the password file also.</li>

 <li>Go to play store on your phone and look for application called <strong>Google Authenticator</strong> and install it on your phone. read carefully all the instructions that you see while installing the app.</li>

 <li>Come back to your computer again in <strong>First terminal</strong>. Log into the second user account in guest. One you are logged into the second user in guest, run following command. You do not have to be sudo for this operation.</li>

</ol>

google-authenticator

Once you press enter, you will be asked a question something like the following. Answer ‘<em>yes</em>’ to that. Thereafter you will be presented with a QR code, scan this with your app that you installed previously.

<ol start="10">

 <li>Take a screenshot and save it in a safe place. <strong>Do not loose it</strong>. Your screenshot should contain the six emergency codes also. Save them in a safe place.</li>

 <li>You will be presented will a series of Yes/No questions.

  <ol>

   <li>How many questions were there?</li>

   <li>Describe each one of them in at least two sentences for each. (What each does and why is it important?)</li>

  </ol></li>

 <li>After you scan the QR code in your phone, you should get a number in your app. Put a screenshot of your code.</li>

 <li>You can change the <em><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f0858395829e919d95b09d919398999e95">[email protected]</a>_name</em> according to your wish so that what you feel easy to remember can be named in the app. The six digit number is the verification code that you will enter when prompted to enter while logging into the second user. The circle shows the time limit remaining for the validity of code.</li>

</ol>

At this moment, you have enabled two factor authentication for your second user accout in your guest machine. Your root account is still without two factor authentication. This is the configuration that we will be using for this lab.

<ol start="15">

 <li>Log out of the second user account in your First Terminal. Try loging back in.

  <ol>

   <li>What is the difference that you noticed?</li>

   <li>Were you able to log into the second user account? Why/Why not?</li>

  </ol></li>

</ol>

<h1>Hacking II &amp; III</h1>

<ol start="16">

 <li>Go to your hacking terminal. You should be at host machine. Use hydra attack to guess the password of the second user in guest machine. Make sure that the text file that you provide has the correct password.</li>

 <li>Were you able to hack? Why/ Why not?</li>

 <li>Log into the root user of guest machine and perform the hydra attack again. Write your results here.</li>

</ol>

<h1>Subtask III</h1>

<ol start="18">

 <li>Although you have enabled the two factor mechanism in the second user, you need to configure the ssh engine to use two factor authentication. So now we will be using the <strong>Backup Terminal</strong> that you had open and unused for a long time.</li>

 <li>We will basically edit two files from root user.

  <ol>

   <li>Open the file <em>/etc/ssh/sshd_config</em>. In the file find the following statement and change the option <em>no</em> to <em>yes</em> towards the middle of the file. Save and close the file</li>

  </ol></li>

</ol>

ChallengeResponseAuthentication yes

<ol start="2">

 <li>Open the file <em>/etc/pam.d/sshd</em>. In the file add the following statement in the bottom of the file.</li>

</ol>

auth required pam_google_authenticator.so nullok Save and close the file.

The statement that you just added makes the <em>pam_google_authenticator</em> as a required module for authentication for users. The term <em>nullok</em> means that it is not required for those users that do not have authenticator enabled. Which users in our case would require the google authentication and which do not in our case?

Restart the ssh daemon using the following command. sudo systemctl restart sshd.service

<h1>Subtask IV (Testing)</h1>

<ol start="20">

 <li>Now go back to <strong>First Terminal</strong>. Log out of it if you are logged in as second user in guest. Try logging back in as second user account. Explain your experience in a paragraph (Verification code is the code in the app)</li>

</ol>

<h1>Hacking IV</h1>

<ol start="21">

 <li>Go to <strong>Hacking Terminal</strong>. Log into your odroid Host.</li>

 <li>Use Hydra attack to attack the second user in guest. Explain the procedure and result. Are you able to hack into the account? Why/WhyNot?</li>

</ol>

<h1>Tampering</h1>

<ol start="23">

 <li><strong>Write a short note on NTP Server?</strong></li>

 <li>Go to <strong>Backup Terminal</strong>. You should be logged in as root in guest. Set the time in your guest so that it is not equal to the UTC time in your Phone (choose any random time).

  <ol>

   <li>In your <strong>Hacking Terminal</strong> type in <em>date</em> to see the UTC time in host (make sure you are logged in to host).</li>

   <li>Go to your <strong>Backup Terminal</strong> (you should be logged in as root in guest), and perform the same operation. <strong>What do you see?</strong></li>

   <li>To set the date in guest in your <strong>Backup Terminal</strong>, do the following: date -s “19 APR</li>

  </ol></li>

</ol>

2012 11:14:00″

<ol start="25">

 <li>Go to the <strong>Hacking Terminal</strong> and check to see if you can log into the second guest account from the Host (Normal ssh, not hydra). Explain your experience.</li>

 <li>In the above step, you should be able to login after using Verification code. Google authentication module is time dependent (this means that the UTC time in your phone should be same as your machine). But, NTP server in your guest synchronizes the time before you log back in again after you change the time.</li>

 <li>Change the time in your machine so that NTP server does not synchronize it. For this we have to disable the synchronization. Run the following command as root in guest from</li>

</ol>

<strong>Backup Terminal</strong>. timedatectl set-ntp 0

Now change the time to any random time you want. Check if the time changed or not. Try to log into the second user account in guest. Explain your experience.

<strong>[Make sure you change the set-ntp to 1 once you are done with this]</strong>

<h2>Finishing</h2>

Here we will get rid of google authenticator essentially undoing what we just did.

<ol start="28">

 <li>Go to your <strong>Backup Terminal</strong> where you should be logged in as root in guest. Edit the file</li>

</ol>

<em>/etc/ssh/sshd_config</em> file and undo the change that you previously made. ie change the option <em>ChallengeResponseAuthentication yes</em> to <em>ChallengeResponseAuthentication no</em>

<ol>

 <li>Save and close the file.</li>

 <li>Edit file <em>/etc/pam.d/sshd</em> and comment the statement that we added. save and close the file.</li>

 <li>Restart the ssh service.</li>

 <li>Go to Hacking Terminal and try to run hydra for second user in guest. Are you able to run hydra and guess the correct password?</li>

 <li>Go to your <strong>Backup Terminal</strong> and perform</li>

</ol>

sudo apt-get remove libpam-google-authenticator Now you can gracefully close all the terminals.