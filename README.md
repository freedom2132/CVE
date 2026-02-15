Rainbow Aggregate DNS Management System V2.15 Command Execution High-Risk Vulnerability
Domain: http://127.0.0.2 (Local Test Environment)
Vulnerability Type: Web Vulnerability / Code Execution VulnerabilityVulnerability Title: Rainbow Aggregate DNS Management System V2.15 Has a High-Risk Command Execution Vulnerability
URL: /app/utils/CheckUtils.php
Vulnerability Type: Code Execution Vulnerability (Command Injection)
<img width="414" height="235" alt="image" src="https://github.com/user-attachments/assets/9a558ee0-078d-4f37-81bb-3ac42530fa3f" />

Brief Description
A severe command injection vulnerability exists in the ping() function of the app\utils\CheckUtils.php file in Rainbow Aggregate DNS Management System V2.15. The function does not perform any filtering or escaping on the incoming $target parameter and directly splices the parameter into the exec() system command execution function. Attackers can construct malicious parameters and splice system command separators to achieve arbitrary system command execution, which can further lead to server control, sensitive data theft, and backdoor file writing, posing an extremely high risk level.
Vulnerability Reproduction
1. Build and access the local test environment of Rainbow Aggregate DNS Management System
<img width="327" height="257" alt="image" src="https://github.com/user-attachments/assets/1fb4386b-0bb4-490c-a11c-db78bdf248dd" />

<img width="415" height="221" alt="image" src="https://github.com/user-attachments/assets/e4c7a58a-2d48-46f1-a8b7-2a25546b620d" />

2. Locate the core vulnerability file and code: app\utils\CheckUtils.php
<img width="415" height="316" alt="image" src="https://github.com/user-attachments/assets/df3bcf81-a528-47c0-aead-987c3f96ecf6" />

The core vulnerability code line where the $target parameter is directly spliced into exec() in the ping() function:exec('ping -c 1 -w '.$timeout.' '.$target, $output, $return_var);
3. Write a test file and perform command execution verification
<img width="415" height="316" alt="image" src="https://github.com/user-attachments/assets/cc6b3893-85a6-41b1-ba6c-b3f95d43eec1" />

Access the test file in the browser address bar: http://127.0.0.2/test_cmd.php
Execution Result:Malicious command executed: ping -n 1 -w 1 127.0.0.1&whoamiSystem user name returned in the whoami command execution result: xxx\23325
4. Verify the validity and exploitability of command execution
To further verify the actual exploitability of the vulnerability, tests such as server file traversal, system information query, and file writing were conducted by constructing different malicious system commands, all of which were successfully executed. This proves that the vulnerability can be exploited by attackers to implement a variety of malicious behaviors, with a wide hazard range and high risk level. The specific tests and results are as follows:
Test 1: Execute the dir command to traverse the server website root directory
<img width="415" height="316" alt="image" src="https://github.com/user-attachments/assets/db702480-9885-4668-bc62-4b916a8566eb" />

<img width="415" height="209" alt="image" src="https://github.com/user-attachments/assets/5511ec3f-b022-4037-91b2-2b4350df3f1b" />

Test 2: Execute the systeminfo command
<img width="318" height="324" alt="image" src="https://github.com/user-attachments/assets/90c372a9-c547-4d35-ae0a-f8018dd7695e" />

<img width="415" height="209" alt="image" src="https://github.com/user-attachments/assets/5d0ff839-ee23-4cf5-b9cf-be4536ae8900" />

5. Verification conclusion
<img width="415" height="181" alt="image" src="https://github.com/user-attachments/assets/0800d3f4-af9d-4cf0-9aea-5c52b25dd597" />

This verifies that Rainbow Aggregate DNS Management System V2.15 has a high-risk command execution vulnerability.
Key Notes on Vulnerability Reproduction
This test is based on the local environment of Windows+PHPStudy (Apache/2.4.39 + PHP/8.2.9) without WAF/filter rule interference, and the vulnerability can be triggered stably. The $target parameter in the ping() function is directly spliced into the system command after IP verification. Attackers can bypass the basic IP verification to achieve command injection in the format of 127.0.0.1&any system command. The whoami command is successfully executed and returns the system user name, which proves that the vulnerability has actual exploitation value and is a high-risk vulnerability.
