# HW2 - Simple Http Server

The following code shows the implementation of a simple HTTP server written in shell script:
```sh
#!/bin/bash

trap "echo 'Terminating...'; exit" SIGINT SIGTERM

process() {
    content=$(cat)
    echo -e "$content"
}

process_http() {
    payload=$(cat)
    # HTTP Response headers
    echo -e "HTTP/1.1 200 OK"
    echo -e "Server: Simple Http Server"
    echo -e "Date: $(date -u '+%a, %d %b %Y %H:%M:%S GMT')"
    echo -e "Content-Type: text/plain; charset=utf-8"
    echo -e "Content-Length: $(echo -e "$payload" | wc -c)"
    echo -e ""
    echo -e "$payload"
}

echo "Listening on 0.0.0.0:8080"
while :; do
    cat ./log | process | process_http | timeout 1 nc -l 0.0.0.0 8080
done
```

You need to implement the `process` function to meet the following requirements:
1. The format of the `log` file is provided below, logging users login events.
2. Output the top 5 users with the most failed login attempts.
3. Display the exact time of the last successful login by each user in lexicographical order.

Format of the `log` file:
```
...
[2023-10-05 09:15:32] - admin: login, failed
[2023-10-05 09:15:33] - root: login, failed
[2023-10-05 09:15:35] - user1: login, successful
[2023-10-05 09:15:38] - root: login, failed
...
```
Format of the output:
```
Top 5 users with the most failed login attempts:
1. root
2. admin
3. user1
4. user2
5. user3

The time of the last successfully login:
admin: 2023-10-05 09:15:16
root: 2023-10-04 16:35:49
user1: 2023-10-05 09:15:35
user2: 2023-10-05 09:16:39
user3: 2023-10-05 09:16:12
```

Note.
1. You cannot use any other interpreters, compilers or programming languages (such as Python, Perl, C++...)
2. You can only submit one script file.
3. You can use both bash or sh.
4. `awk(mawk)`, `sed` and the other common command tools are allowed.
