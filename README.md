# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
```
import socket

s = socket.socket()

# Fix: allows reuse of port (prevents WinError 10048)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('localhost', 8000))
s.listen(1)

print("Waiting for connection...")

conn, addr = s.accept()
print("Connected to:", addr)

try:
    while True:
        data = conn.recv(1024).decode()

        if not data:
            print("Client disconnected")
            break

        print("Frames received:", data)

        # Send ACK
        ack = "ACK for " + data
        conn.send(ack.encode())

except Exception as e:
    print("Error:", e)

finally:
    conn.close()
    s.close()
```
```
import socket

s = socket.socket()

s.connect(('localhost', 8000))

try:
    n = int(input("Enter number of frames: "))
    w = int(input("Enter window size: "))

    # Validation
    if w > n:
        print("Window size cannot be greater than number of frames")
        s.close()
        exit()

    frames = list(range(1, n + 1))
    i = 0

    while i < n:
        send_frames = frames[i:i + w]

        msg = " ".join(map(str, send_frames))
        print("Sending frames:", msg)

        s.send(msg.encode())

        ack = s.recv(1024).decode()
        print("Received:", ack)

        i += w

except Exception as e:
    print("Error:", e)

finally:
    s.close()
```

## OUPUT
<img width="1860" height="894" alt="image" src="https://github.com/user-attachments/assets/6af6c3c1-93f2-4083-a1e1-887bfa94ff7d" />

## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
