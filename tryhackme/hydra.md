# Hydra: Basics

Hydra is a password-cracking tool. Can be used for brute-forcing web forms as well.

## Commands

### SSH

**`hydra -l username -P wordlist.txt IP-ADDRESS -t number ssh`**

| Flag | Description |
| :--- | :--- |
| -l | specifies the SSH username for login |
| -P | indicates a list of passwords |
| -t | sets the number of threads to spawn |
| ssh | names the protocol, can be anything else too like ftp then it'll check on port 21 |

### POST Web Form

**`hydra -l username -P wordlist.txt IP-ADDRESS http-post-form "path:username=^USER^&password=^PASS^:F=incorrect" -s port -V`** 

| Flag | Description |
| :--- | :--- |
| -l | specifies the web form username for login |
| -P | indicates a list of passwords |
| http-post-form | the type of the form = POST |
| path | the login page URL endpoint, can be "/" or "/login" or "/login.php" or anything else the URL says |
| login_credentials | username=^USER^&password=^PASS^ |
| invalid_response | part of the response when login fails |
| -s port | to specify the port number in cases of uncommon ports |
| -V | for verbose output for every attempt |