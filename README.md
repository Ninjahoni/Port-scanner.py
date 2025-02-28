# Port-scanner.py
import socket
import threading

def scan_port(ip, port, open_ports):
    try:
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
            s.settimeout(1)  
            result = s.connect_ex((ip, port))  
            if result == 0:
                print(f"[+] Port {port} is open")
                open_ports.append(port)  # Store open port in the list
    except Exception:
        pass  

def port_scanner(ip, start_port, end_port):
    """Scans ports in the given range using multiple threads and displays open ports."""
    print(f"Scanning {ip} for open ports ({start_port}-{end_port})...")
    threads = []
    open_ports = []  
    
    for port in range(start_port, end_port + 1):
        thread = threading.Thread(target=scan_port, args=(ip, port, open_ports))
        thread.start()
        threads.append(thread)
    
    for thread in threads:
        thread.join() 
    
    print("Scan complete.")
    if open_ports:
        print("Open ports:", ', '.join(map(str, open_ports)))
    else:
        print("No open ports found.")

if __name__ == "__main__":
    target_ip = input("Enter target IP: ")
    start_port = int(input("Enter start port: "))
    end_port = int(input("Enter end port: "))
    
    port_scanner(target_ip, start_port, end_port)
