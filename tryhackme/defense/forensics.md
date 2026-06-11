# Digital Forensics Fundamentals

Application of methods and procedures to investigate and solve cyber crimes.

### Phases of Digital Forensics (CEAR)

As defined by the NIST (National Institute of Standards and Technology):

1. **Collection**: Identifying all the devices from which data can be collected
2. **Examination**: Filtering all the unnecessary/irrelevant data out.
3. **Analysis**: Correlating evidence to draw conclusions. Most important phase.
4. **Reporting**: A detailed report consisting an executive summary, investigation methodology, detailed findings, and recommendations.

### Types of Digital Forensics

1. Disk Forensics
2. Network Forensics: Network traffic logs
3. Database Forensics: Intrusion, data modification, data exfiltration
4. Memory Forensics
5. Mobile Forensics: Investigating mobile devices for call and text records, GPS locations, etc.
6. Malware Forensics
7. Email Forensics: Phishing or fraudulent campaigns
8. Cloud Forensics: Investigating cloud infrastructure, can get tricky
9. Computer Forensics: Investigating computers, most common

### Evidence Acquisition General Practices

1. Proper Authorization: from relevant authorities
2. Chain of Custody: proves integrity and reliability of evidence admitted in court through a Chain of Custody document. Includes *description of the evidence **(name, type)**, **name of individual** who collected the evidence, **date and time** of evidence collection, **storage location** of each piece of evidence, **access times and individual record** of who accessed the evidence*. 
3. Use of Write Blockers: Prevent accidental evidence alteration during routine procedures.

### Windows Forensics

Two types of images are prioritized during the data collection phase:
1. Disk image: non-volatile memory stored in HDD, SSD, etc. like documents, web history, etc.
2. Memory image: volatile memory stored in RAM like open files, running processes, etc. Collected first since a shutdown of the system would lead to losing these files.

Tools used:
1. **FTK Imager**: Takes disk images, used for both collection and analysis, friendly GUI.
2. **Autopsy**: Extensive analysis of disk image.
3. **Dumpit**: Takes memory images using CLI.
4. **Volatility**: Analyzing memory images with various plugins.

### CLI-based Investigation Packages

| Tool Name | Description | Installation | Usage |
| :--- | :--- | :--- | :--- |
| pdfinfo | Displays metadata related to a PDF file such as title, author, subject, creator, creation date. | `sudo apt install poppler-utils` | `pdfinfo DOCUMENT.pdf` |
| exiftool | Displays metadata related to an image file (EXIF = Exchangeable Image File Format), containing camera/smartphone model, date and time of capture, photo settings such as focal length, ISO, shutter speed, aperture, and even GPS coordinates. | `sudo apt install libimage-exiftool-perl` | `exiftool IMAGE.jpg` |