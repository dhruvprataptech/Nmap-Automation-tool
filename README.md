# Nmap-Automation-tool
Nmap Automation  script build with shell scripting to streamline network reconnaissanceand vulnerability assessment this tool automates various Nmap scan organizes the result scanning multiple targets saving time duringsecurity assessment and pentration testing
#!/bin/bash


 {
    echo "====================================="
    echo "       Nmap Automation Tool          "
    echo "====================================="
    echo "1. Host Discovery (Ping Scan)"
    echo "2. Port Scanning"
    echo "3. Service Detection"
    echo "4. Vulnerability Scanning"
    echo "5. Firewall/IDS Evasion"
    echo "6. Comprehensive Scan (All above)"
    echo "7. Custom Nmap Command"
    echo "8. Exit"
    echo "====================================="
    echo -n "Enter your choice [1-8]: "
}

# Function to perform ping scan (host discovery)
ping_scan() {
    echo
    echo "=== Host Discovery (Ping Scan) ==="
    echo "This will discover live hosts in the network without port scanning."
    read -p "Enter target IP or network (e.g., 192.168.1.0/24): " target
    
    echo "Select scan intensity:"
    echo "1. Fast scan (fewer packets)"
    echo "2. Normal scan"
    echo "3. Intensive scan (more reliable)"
    read -p "Enter choice [1-3]: " intensity
    
    case $intensity in
        1) options="-sn -PE --max-rate 100";;
        2) options="-sn -PE";;
        3) options="-sn -PS22,80,443 -PA22,80,443 -PU161 -PY";;
        *) options="-sn";;
    esac
    
    echo "Starting ping scan..."
    nmap $options $target -oN ping_scan_results.txt
    echo "Scan completed. Results saved to ping_scan_results.txt"
}

# Function to perform port scanning
port_scan() {
    echo
    echo "=== Port Scanning ==="
    read -p "Enter target IP or hostname: " target
    
    echo "Select scan type:"
    echo "1. Quick scan (top 100 ports)"
    echo "2. Standard scan (top 1000 ports)"
    echo "3. Full scan (all 65535 ports)"
    echo "4. Custom port range"
    read -p "Enter choice [1-4]: " scantype
    
    case $scantype in
        1) options="-F";;
        2) options="";;
        3) options="-p-";;
        4) 
            read -p "Enter port range (e.g., 20-80 or 22,80,443): " ports
            options="-p $ports"
            ;;
        *) options="";;
    esac
    
    echo "Select speed/timing (higher is faster but more aggressive):"
    echo "1. Paranoid (0) - Very slow"
    echo "2. Sneaky (1)"
    echo "3. Polite (2)"
    echo "4. Normal (3)"
    echo "5. Aggressive (4)"
    echo "6. Insane (5) - Very fast"
    read -p "Enter choice [1-6]: " speed
    
    case $speed in
        1) options="$options -T0";;
        2) options="$options -T1";;
        3) options="$options -T2";;
        4) options="$options -T3";;
        5) options="$options -T4";;
        6) options="$options -T5";;
        *) options="$options -T3";;
    esac
    
    echo "Starting port scan..."
    nmap $options $target -oN port_scan_results.txt
    echo "Scan completed. Results saved to port_scan_results.txt"
}

# Function to perform service detection
service_detection() {
    echo
    echo "=== Service Detection ==="
    read -p "Enter target IP or hostname: " target
    
    echo "Select detection intensity:"
    echo "1. Basic service detection"
    echo "2. Aggressive service detection (more probes)"
    echo "3. Version detection with OS detection"
    read -p "Enter choice [1-3]: " intensity
    
    case $intensity in
        1) options="-sV";;
        2) options="-sV --version-intensity 5";;
        3) options="-sV -O";;
        *) options="-sV";;
    esac
    
    echo "Starting service detection..."
    nmap $options $target -oN service_detection_results.txt
    echo "Scan completed. Results saved to service_detection_results.txt"
}

# Function to perform vulnerability scanning
vulnerability_scan() {
    echo
    echo "=== Vulnerability Scanning ==="
    echo "Note: This requires Nmap with NSE (Nmap Scripting Engine)"
    read -p "Enter target IP or hostname: " target
    
    echo "Select vulnerability scan type:"
    echo "1. Basic vulnerability check"
    echo "2. Common vulnerabilities"
    echo "3. All vulnerability scripts (very aggressive)"
    read -p "Enter choice [1-3]: " scantype
    
    case $scantype in
        1) scripts="vuln";;
        2) scripts="vulners,http-vuln-*,ssl-*";;
        3) scripts="vuln,exploit,malware,dos";;
        *) scripts="vuln";;
    esac
    
    echo "Starting vulnerability scan..."
    nmap -sV --script=$scripts $target -oN vulnerability_scan_results.txt
    echo "Scan completed. Results saved to vulnerability_scan_results.txt"
}

# Function for firewall/IDS evasion techniques
firewall_evasion() {
    echo
    echo "=== Firewall/IDS Evasion ==="
    read -p "Enter target IP or hostname: " target
    
    echo "Select evasion technique:"
    echo "1. Fragmentation (split packets)"
    echo "2. Decoy scan (hide among fake IPs)"
    echo "3. Random data append"
    echo "4. Spoof MAC address"
    echo "5. Custom source port"
    read -p "Enter choice [1-5]: " evasiontype
    
    case $evasiontype in
        1) options="-f";;
        2) 
            read -p "Enter decoy IPs (comma separated): " decoys
            options="-D $decoys"
            ;;
        3) options="--data-length 25";;
        4) 
            echo "Select MAC address to spoof:"
            echo "1. Random"
            echo "2. Cisco"
            echo "3. Apple"
            echo "4. Enter custom MAC"
            read -p "Enter choice [1-4]: " macchoice
            
            case $macchoice in
                1) mac="random";;
                2) mac="Cisco";;
                3) mac="Apple";;
                4) 
                    read -p "Enter MAC address (e.g., 00:11:22:33:44:55): " mac
                    ;;
                *) mac="random";;
            esac
            options="--spoof-mac $mac"
            ;;
        5) 
            read -p "Enter source port number: " port
            options="--source-port $port"
            ;;
        *) options="";;
    esac
    
    echo "Starting scan with evasion techniques..."
    nmap $options $target -oN firewall_evasion_results.txt
    echo "Scan completed. Results saved to firewall_evasion_results.txt"
}

# Function for comprehensive scan
comprehensive_scan() {
    echo
    echo "=== Comprehensive Scan ==="
    echo "This will perform host discovery, port scanning, service detection,"
    echo "OS detection, and vulnerability scanning (may take a long time)."
    read -p "Enter target IP or network: " target
    
    echo "Starting comprehensive scan..."
    nmap -sS -sV -O -A -T4 --script=vuln $target -oN comprehensive_scan_results.txt
    echo "Scan completed. Results saved to comprehensive_scan_results.txt"
}

# Function for custom nmap command
custom_command() {
    echo
    echo "=== Custom Nmap Command ==="
    echo "Enter your custom Nmap command (without 'nmap' at the beginning)"
    echo "Example: -sS -sV -p 1-1000 -T4 192.168.1.1"
    read -p "Enter command: " custom_cmd
    echo "Starting custom scan..."
    nmap $custom_cmd -oN custom_scan_results.txt
    echo "Scan completed. Results saved to custom_scan_results.txt"
}

# Main program loop
while true; do
    show_menu
    read choice
    
    case $choice in
        1) ping_scan;;
        2) port_scan;;
        3) service_detection;;
        4) vulnerability_scan;;
        5) firewall_evasion;;
        6) comprehensive_scan;;
        7) custom_command;;
        8) 
            echo "Exiting Nmap Automation Tool."
            exit 0
            ;;
        *) 
            echo "Invalid option. Please try again."
            ;;
    esac
    
    read -p "Press [Enter] to continue..."
done
