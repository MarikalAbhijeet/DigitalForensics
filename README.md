<h1>Advanced Malware Analysis and Digital Forensics Expertise	</h1>
<h2>Description</h2>
Advanced malware was examined within a virtual machine disk file, leveraging the Autopsy forensics tools to assess 250 artifacts, culminating in a detailed malware analysis. This was further complemented by the reverse engineering of the malware in a controlled sandbox environment, where its intricate mechanisms were deciphered to shed light on its operations. Covert messages from the malware were also decrypted using a diverse toolkit, highlighting expertise in digital forensics and malware analysis
<br />


<h2>Languages and Utilities Used</h2>

- <b>[AutoPsy Forensic tool](https://www.autopsy.com/)</b> 
- <b>[WireShark](https://www.wireshark.org/a)</b>
- <b>[Veracrypt](https://www.veracrypt.fr/code/VeraCrypt/)</b>

<h2>Environments Used </h2>

- <b>OS: </b> Microsoft Windows 11 Home Single Language
- <b>Version: </b>10.0.22000 Build 22000

<h2>Investigation walk-through:</h2>


REPOSITORY #1: ENPM687 Final XP. vmwarevm <br/>
<img src="https://i.imgur.com/jWmq7bt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
# REPOSITORY #1: ENPM687 Final XP. vmwarevm

## a. SUMMARY OF EVIDENCE VIRTUALDISK.VMDK
VirtualDisk.vmdk is a Disk image of a hard drive retrieved from a rebel malware writer according to the given scenario. I have used the ‘Autopsy tool’ to investigate the image file. After loading the image in the software, I found 2 executables named obiwan.exe and obiwan2.exe which were suspicious. I chose these as the evidence because they were closely related to the scenario provided by the author.

**PATH:** `/vol_vol2/Documents_and_Settings/Administrator/MyDocuments/code/dist/obiwan.exe`
<img src="https://i.imgur.com/e12MPTC.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

## b. ANALYSIS IN DETAILS OR ARTIFACTS

### 1. Packet capture analysis:
After running the 2 executables and capturing their network traffic via Wireshark, we observed that they were sending some messages when run.
- **Obiwan.exe:** The first executable was only sending two messages, but they were not so special.
  <img src="https://i.imgur.com/wBEcZsC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/jVKEx8S.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
- **Obiwan2.exe:** The second executable sent three messages.
  <img src="https://i.imgur.com/UWvaziC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/v8L3PZM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  **Suspected Clues**
  <img src="https://i.imgur.com/cjmDQqz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

### 2. Analysis of the clues:
After analyzing the first message, I realized that the base64 written in the message is used to encode binary data as printable text. The second message looked like encoded text, so I converted it using a BASE64 decoder to get the plaintext message: `r2d2 is the key`.

### 3. Suspected software:
Further analysis of the image disk revealed a program named VeraCrypt.exe, an open-source encryption software. I inspected the files encrypted with VeraCrypt by examining the programs which ran in the data artifacts generated by Autopsy. The logs showed that an MP3 file was encrypted by VeraCrypt.

### 4. Analysis of Suspected MP3:
After exporting and playing the MP3 file, a message was obtained.

## REPOSITORY #2: not-the-droids-youre-looking-for.mp3

## a. Summary of evidence MP3 file
This section will attempt to decrypt and execute the MP3 file to understand its contents.

## b. Analysis in Details or Artifacts

### 1. MP3 Decryption:
From Repository 1, we determined that VeraCrypt was used to encrypt the MP3 file, and a key `r2d2` was decoded from obiwan2.exe. Using VeraCrypt with this key decrypted the MP3.

### 2. Analysis of Decrypted files:
After decryption, VeraCrypt mounted a virtual drive displaying extracted files from the MP3.

- **Death Star Plans (FILE FOLDER):** This folder contained layouts, plans, and blueprints of the Death Star.
- **Final-form.exe:** This executable resembled obiwan and obiwan2 but seemed to send messages to the internet.

### 3. Network Traffic Analysis of Final Form.exe:
Using Wireshark to capture packets while running finalform.exe revealed the following messages:
- Message1: `We-have-the-blue-prints-to-the-Death-Star`
- Message2: `We-will-defeat-Darth-Vader`

### 4. Other Evidence:
Other findings included metadata in artifacts pointing to PDF files explaining disk encryption and key creation for VeraCrypt, recent documents related to the Death Star, files related to executables, and more.

## 4. INVESTIGATION FINAL REMARKS:
Analysis of both repositories suggests that the rebel's plan focuses on destroying the Death Star and defeating Darth Vader. The project presented challenges, especially in understanding base64 encoding and examining program data logs.

