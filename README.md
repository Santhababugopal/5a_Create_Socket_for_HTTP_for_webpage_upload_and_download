# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 

```
import socket

def send_request(host, port, request_bytes):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request_bytes)
        response = b""
        while True:
            part = s.recv(4096)
            if not part:
                break
            response += part
    return response

def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()
        content_length = len(file_data)
        headers = (
            f"POST /upload HTTP/1.1\r\n"
            f"Host: {host}\r\n"
            f"Content-Length: {content_length}\r\n"
            f"Content-Type: application/octet-stream\r\n"
            f"\r\n"
        ).encode()
        request = headers + file_data
        response = send_request(host, port, request)
    return response.decode(errors="ignore")

def download_file(host, port, filename):
    request = (
        f"GET /{filename} HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"\r\n"
    ).encode()
    response = send_request(host, port, request)
    headers, _, body = response.partition(b"\r\n\r\n")
    with open(filename, 'wb') as file:
        file.write(body)

if _name_ == "_main_":
    host = 'example.com'
    port = 80

    # Upload file
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:", upload_response)

    # Download file
    download_file(host, port, 'example.txt')
    print("File downloaded successfully.")



```
## OUTPUT

![WhatsApp Image 2025-05-02 at 19 02 25_af596a50](https://github.com/user-attachments/assets/b986d028-ffc2-424d-bc51-6de4138c5ff6)

## Result
Thus the socket for HTTP for web page upload and download created and Executed
