To do so, we can include the PHP configuration file found at (/etc/php/X.Y/apache2/php.ini) for Apache or at (/etc/php/X.Y/fpm/php.ini) for Nginx, where X.Y is your install PHP version. We can start with the latest PHP version, and try earlier versions if we couldn't locate the configuration file. We will also use the base64 filter we used in the previous section, as .ini files are similar to .php files and should be encoded to avoid breaking. Finally, we'll use cURL or Burp instead of a browser, as the output string could be very long and we should be able to properly capture it:

![image](https://github.com/user-attachments/assets/55f76f8b-c380-4273-88d4-343a1f8744ec)

Similar to the data wrapper, the input wrapper can be used to include external input and execute PHP code. The difference between it and the data wrapper is that we pass our input to the input wrapper as a POST request's data. So, the vulnerable parameter must accept POST requests for this attack to work. Finally, the input wrapper also depends on the allow_url_include setting, as mentioned earlier.

Finally, we may utilize the expect wrapper, which allows us to directly run commands through URL streams. Expect works very similarly to the web shells we've used earlier, but don't need to provide a web shell, as it is designed to execute commands.

However, expect is an external wrapper, so it needs to be manually installed and enabled on the back-end server, though some web apps rely on it for their core functionality, so we may find it in specific cases. We can determine whether it is installed on the back-end server just like we did with allow_url_include earlier, but we'd grep for expect instead, and if it is installed and enabled we'd get the following:
